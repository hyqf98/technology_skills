# Spring AI Agent 开发指南

**页数:** 5

---

## 1. Agent 概述

Spring AI Agent 是基于 LLM 的智能代理系统，可以自主执行任务、调用工具、管理状态。

**核心能力：**
- 任务规划和分解
- 工具调用（Function Calling）
- 多轮对话和推理
- 状态管理和记忆
- 工作流编排

---

## 2. 基础 Agent

### 2.1 简单 Agent

```java
@Service
public class SimpleAgent {

    private final ChatClient chatClient;

    public String execute(String task) {
        return chatClient.prompt()
            .system("你是一个任务执行助手，负责完成用户指定的任务")
            .user(task)
            .call()
            .content();
    }
}
```

### 2.2 带工具的 Agent

**定义工具**

```java
@Service
public class CalculatorTool implements Function<CalculatorTool.Request, CalculatorTool.Response> {

    public record Request(String operation, double a, double b) {}
    public record Response(double result) {}

    @Override
    public Response apply(Request request) {
        return switch (request.operation()) {
            case "add" -> new Response(request.a() + request.b());
            case "subtract" -> new Response(request.a() - request.b());
            case "multiply" -> new Response(request.a() * request.b());
            case "divide" -> new Response(request.a() / request.b());
            default -> throw new IllegalArgumentException("Unknown operation: " + request.operation());
        };
    }
}
```

**使用工具**

```java
@Service
public class ToolUsingAgent {

    private final ChatClient chatClient;
    private final CalculatorTool calculatorTool;

    public String execute(String task) {
        return chatClient.prompt()
            .user(task)
            .functions(calculatorTool)
            .call()
            .content();
    }
}
```

---

## 3. 工作流模式

### 3.1 链式工作流

**顺序执行任务**

```java
@Service
public class ChainWorkflowAgent {

    private final ChatClient chatClient;

    public String execute(String input) {
        String current = input;

        // 步骤1: 理解任务
        current = chatClient.prompt()
            .system("你负责理解用户任务的核心需求")
            .user("任务：" + current + "\n请提取核心需求")
            .call()
            .content();

        // 步骤2: 制定计划
        current = chatClient.prompt()
            .system("你负责制定执行计划")
            .user("需求：" + current + "\n请制定详细执行计划")
            .call()
            .content();

        // 步骤3: 执行任务
        return chatClient.prompt()
            .system("你负责执行任务")
            .user("计划：" + current + "\n请执行并报告结果")
            .call()
            .content();
    }
}
```

### 3.2 并行工作流

**并行处理多个子任务**

```java
@Service
public class ParallelWorkflowAgent {

    private final ChatClient chatClient;

    public String execute(String task) {
        // 1. 分解任务
        String subtasksPrompt = String.format("""
            将以下任务分解为3个独立的子任务：

            任务：%s

            请列出3个子任务，每行一个。
            """, task);

        String subtasks = chatClient.prompt()
            .user(subtasksPrompt)
            .call()
            .content();

        // 2. 并行处理
        List<String> subtaskList = Arrays.stream(subtasks.split("\n"))
            .map(String::trim)
            .filter(s -> !s.isEmpty())
            .toList();

        List<String> results = subtaskList.parallelStream()
            .map(subtask -> chatClient.prompt()
                .user("执行子任务：" + subtask)
                .call()
                .content())
            .toList();

        // 3. 汇总结果
        return chatClient.prompt()
            .user(String.format("""
                原始任务：%s

                子任务执行结果：
                %s

                请汇总以上结果，给出完整的答案。
                """, task, String.join("\n\n", results)))
            .call()
            .content();
    }
}
```

### 3.3 路由工作流

**智能任务分配**

```java
@Service
public class RoutingWorkflowAgent {

    private final ChatClient chatClient;

    private static final Map<String, String> ROUTES = Map.of(
        "technical", "你是技术专家，负责回答技术问题",
        "billing", "你是账单专家，负责处理账单问题",
        "general", "你是通用助手，负责处理一般咨询"
    );

    public String execute(String query) {
        // 1. 分类查询
        String category = chatClient.prompt()
            .user(String.format("""
                将以下查询分类（technical/billing/general）：

                查询：%s

                只返回类别名称。
                """, query))
            .call()
            .content()
            .trim()
            .toLowerCase();

        // 2. 选择对应的处理器
        String systemPrompt = ROUTES.getOrDefault(category, ROUTES.get("general"));

        // 3. 使用选定的处理器回答
        return chatClient.prompt()
            .system(systemPrompt)
            .user(query)
            .call()
            .content();
    }
}
```

### 3.4 编排器-工作器模式

```java
@Service
public class OrchestratorWorkerAgent {

    private final ChatClient chatClient;

    public String execute(String complexTask) {
        // 1. 编排器：分析任务并分解
        String plan = chatClient.prompt()
            .system("你是任务编排器，负责将复杂任务分解为简单的子任务")
            .user("请将以下任务分解为3-5个可执行的子任务：\n" + complexTask)
            .call()
            .content();

        // 2. 解析子任务
        List<String> subtasks = parseSubtasks(plan);

        // 3. 工作器：并行执行子任务
        List<String> results = subtasks.parallelStream()
            .map(subtask -> chatClient.prompt()
                .system("你是任务执行者，负责完成指定的子任务")
                .user(subtask)
                .call()
                .content())
            .toList();

        // 4. 编排器：汇总结果
        return chatClient.prompt()
            .system("你是结果汇总者，负责整合多个子任务的执行结果")
            .user(String.format("""
                原始任务：%s

                子任务执行结果：
                %s

                请整合以上结果，给出完整的最终答案。
                """, complexTask, String.join("\n\n", results)))
            .call()
            .content();
    }

    private List<String> parseSubtasks(String plan) {
        return Arrays.stream(plan.split("\n"))
            .filter(line -> line.matches("\\d+\\..*"))
            .map(line -> line.replaceFirst("^\\d+\\.\\s*", ""))
            .toList();
    }
}
```

---

## 4. 高级 Agent

### 4.1 带记忆的 Agent

```java
@Service
public class MemoryAgent {

    private final ChatClient chatClient;
    private final List<Message> conversationHistory = new ArrayList<>();
    private final Map<String, Object> memory = new HashMap<>();

    public String execute(String task) {
        // 添加用户任务到历史
        conversationHistory.add(new UserMessage(task));

        // 构建包含记忆的提示
        String memoryContext = buildMemoryContext();
        String prompt = task + "\n\n上下文信息：" + memoryContext;

        // 执行任务
        String response = chatClient.prompt()
            .messages(conversationHistory)
            .user(prompt)
            .call()
            .content();

        // 更新记忆和历史
        updateMemory(task, response);
        conversationHistory.add(new AssistantMessage(response));

        return response;
    }

    private String buildMemoryContext() {
        return memory.entrySet().stream()
            .map(e -> e.getKey() + ": " + e.getValue())
            .collect(Collectors.joining("\n"));
    }

    private void updateMemory(String task, String response) {
        memory.put("lastTask", task);
        memory.put("lastResponse", response);
        memory.put("timestamp", Instant.now());
    }
}
```

### 4.2 自主 Agent

```java
@Service
public class AutonomousAgent {

    private final ChatClient chatClient;
    private final List<Function> tools;

    public String execute(String goal) {
        String currentThought = "目标：" + goal;
        String observation = "";

        for (int step = 0; step < 10; step++) {
            // 1. 思考下一步行动
            String thought = chatClient.prompt()
                .system("你是自主Agent，通过思考和行动完成目标")
                .user(String.format("""
                    当前状态：%s

                    上一步的观察：%s

                    请思考下一步应该做什么。

                    格式：
                    Thought: [你的思考]
                    Action: [要执行的动作]
                    """, currentThought, observation))
                .call()
                .content();

            // 2. 解析思考和动作
            String[] parts = parseThoughtAndAction(thought);
            currentThought = parts[0];
            String action = parts[1];

            // 3. 执行动作
            if (action.startsWith("ANSWER:")) {
                return action.substring("ANSWER:".length());
            }

            observation = executeAction(action);

            // 4. 检查是否完成
            if (isGoalComplete(goal, observation)) {
                return chatClient.prompt()
                    .user("基于以下观察给出最终答案：" + observation)
                    .call()
                    .content();
            }
        }

        return "未能完成目标";
    }

    private String executeAction(String action) {
        // 实现具体的动作执行逻辑
        return "已执行：" + action;
    }
}
```

### 4.3 多 Agent 协作

```java
@Service
public class MultiAgentSystem {

    private final ChatClient chatClient;

    public String execute(String task) {
        // 1. 管理者 Agent：分析和分配任务
        String assignment = chatClient.prompt()
            .system("你是任务管理者，负责将任务分配给合适的专家")
            .user("任务：" + task + "\n请分析任务类型并分配给专家（researcher/writer/reviewer）")
            .call()
            .content();

        // 2. 研究者 Agent：收集信息
        String research = chatClient.prompt()
            .system("你是研究专家，负责收集和整理信息")
            .user("任务：" + task)
            .call()
            .content();

        // 3. 写作 Agent：生成内容
        String draft = chatClient.prompt()
            .system("你是写作专家，负责生成高质量内容")
            .user(String.format("""
                任务：%s

                研究资料：%s

                请基于研究资料生成内容。
                """, task, research))
            .call()
            .content();

        // 4. 审核者 Agent：质量检查
        return chatClient.prompt()
            .system("你是审核专家，负责检查内容质量并提出改进建议")
            .user(String.format("""
                任务：%s

                初稿：%s

                请审核初稿并给出最终版本。
                """, task, draft))
            .call()
            .content();
    }
}
```

---

## 5. Agent 配置

### 5.1 ChatClient 配置

```java
@Configuration
public class AgentConfig {

    @Bean
    public ChatClient agentChatClient(ChatModel chatModel) {
        return ChatClient.builder(chatModel)
            .defaultOptions(OpenAiChatOptions.builder()
                .model("gpt-4o")
                .temperature(0.7)
                .build())
            .build();
    }
}
```

### 5.2 工具注册

```java
@Configuration
public class ToolConfig {

    @Bean
    @Description("计算器：执行基本数学运算")
    public Function<CalculatorTool.Request, CalculatorTool.Response> calculator() {
        return new CalculatorTool();
    }

    @Bean
    @Description("天气查询：获取指定城市的天气信息")
    public Function<WeatherTool.Request, WeatherTool.Response> weather() {
        return new WeatherTool();
    }

    @Bean
    @Description("时间查询：获取当前时间")
    public Function<TimeTool.Request, TimeTool.Response> time() {
        return new TimeTool();
    }
}
```

---

## 6. 最佳实践

### 6.1 任务分解

- 将复杂任务分解为简单步骤
- 每个步骤都有明确的输入输出
- 使用中间验证确保质量

### 6.2 工具设计

- 工具功能单一、职责明确
- 提供清晰的描述和参数说明
- 实现错误处理和重试机制

### 6.3 状态管理

- 维护对话历史和上下文
- 使用持久化存储保存状态
- 定期清理过期数据

### 6.4 错误处理

```java
@Service
public class RobustAgent {

    private final ChatClient chatClient;

    public String executeWithErrorHandling(String task) {
        try {
            return execute(task);
        } catch (RetryExhaustedException e) {
            log.error("执行失败，尝试简化任务");
            return executeSimplified(task);
        } catch (Exception e) {
            log.error("未知错误：{}", e.getMessage());
            return "抱歉，执行任务时遇到问题：" + e.getMessage();
        }
    }

    private String executeSimplified(String task) {
        return chatClient.prompt()
            .user("请用最简单的方式回答：" + task)
            .call()
            .content();
    }
}
```

---

## 7. 完整示例

### 7.1 智能客服 Agent

```java
@Service
public class CustomerServiceAgent {

    private final ChatClient chatClient;
    private final VectorStore knowledgeBase;
    private final TicketSystem ticketSystem;

    public String handleCustomerQuery(String query) {
        // 1. 理解客户意图
        String intent = chatClient.prompt()
            .system("分析客户查询的意图（general/technical/billing/complaint）")
            .user("客户查询：" + query)
            .call()
            .content()
            .toLowerCase();

        // 2. 根据意图路由
        return switch (intent) {
            case "technical" -> handleTechnicalQuery(query);
            case "billing" -> handleBillingQuery(query);
            case "complaint" -> handleComplaint(query);
            default -> handleGeneralQuery(query);
        };
    }

    private String handleTechnicalQuery(String query) {
        // 检索技术文档
        List<Document> docs = knowledgeBase.similaritySearch(query, 3);
        String context = buildContext(docs);

        // 生成技术答案
        return chatClient.prompt()
            .system("你是技术支持专家")
            .user("""
                技术文档：
                {context}

                问题：{query}

                请提供详细的技术解决方案。
                """.formatted(context, query))
            .call()
            .content();
    }

    private String handleBillingQuery(String query) {
        // 调用账单系统工具
        return chatClient.prompt()
            .user(query)
            .functions(billingQueryTool, paymentTool)
            .call()
            .content();
    }

    private String handleComplaint(String query) {
        // 创建工单
        String ticketId = ticketSystem.createTicket(query);

        return chatClient.prompt()
            .system("你是投诉处理专员，语气要诚恳、专业")
            .user("""
                客户投诉：%s

                工单号：%s

                请给出专业、诚恳的回复。
                """.formatted(query, ticketId))
            .call()
            .content();
    }

    private String handleGeneralQuery(String query) {
        // 简单问答
        return chatClient.prompt()
            .system("你是客服代表，友好、专业")
            .user(query)
            .call()
            .content();
    }
}
```

### 7.2 研究助手 Agent

```java
@Service
public class ResearchAssistantAgent {

    private final ChatClient chatClient;
    private final VectorStore paperDatabase;

    public String conductResearch(String topic) {
        // 1. 制定研究计划
        String plan = chatClient.prompt()
            .system("你是研究顾问，负责制定详细的研究计划")
            .user("为以下主题制定研究计划：" + topic)
            .call()
            .content();

        // 2. 检索相关论文
        List<Document> papers = paperDatabase.similaritySearch(topic, 10);

        // 3. 分析论文
        String analysis = papers.parallelStream()
            .limit(5)
            .map(paper -> chatClient.prompt()
                .system("你是论文分析师，负责提取论文的核心观点")
                .user("请分析以下论文：" + paper.getContent())
                .call()
                .content())
            .collect(Collectors.joining("\n\n"));

        // 4. 综合研究结果
        return chatClient.prompt()
            .system("你是研究报告撰写者")
            .user(String.format("""
                研究主题：%s

                研究计划：%s

                论文分析：%s

                请基于以上信息撰写完整的研究报告。
                """, topic, plan, analysis))
            .call()
            .content();
    }
}
```

---

## 8. 监控和调试

### 8.1 Agent 执行追踪

```java
@Service
public class TracedAgent {

    private final ChatClient chatClient;

    public String execute(String task) {
        AgentTrace trace = new AgentTrace();
        trace.start("execute");

        try {
            // 执行步骤1
            trace.start("step1");
            String result1 = executeStep1(task);
            trace.end("step1", result1);

            // 执行步骤2
            trace.start("step2");
            String result2 = executeStep2(result1);
            trace.end("step2", result2);

            trace.end("execute", "completed");
            return result2;

        } catch (Exception e) {
            trace.error("execute", e);
            throw e;
        } finally {
            log.info("Agent trace: {}", trace.toJson());
        }
    }
}
```

### 8.2 性能监控

```java
@Aspect
@Component
public class AgentMonitorAspect {

    @Around("@annotation(MonitorAgent)")
    public Object monitorAgentPerformance(ProceedingJoinPoint pjp) throws Throwable {
        long startTime = System.currentTimeMillis();
        String methodName = pjp.getSignature().getName();

        try {
            Object result = pjp.proceed();
            long duration = System.currentTimeMillis() - startTime;

            // 记录指标
            meterRegistry.timer("agent.execution.time",
                "method", methodName)
                .record(duration, TimeUnit.MILLISECONDS);

            return result;
        } catch (Exception e) {
            meterRegistry.counter("agent.execution.errors",
                "method", methodName,
                "error", e.getClass().getSimpleName())
                .increment();
            throw e;
        }
    }
}
```

---

© 2025 Spring AI 技术文档
