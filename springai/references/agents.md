# Spring AI - Agent 代理

**页数:** 3

---

## 构建高效代理 :: Spring AI 参考文档

**URL:** https://docs.spring.io/spring-ai/reference/1.0/api/effective-agents.html

**目录:**
- 构建高效代理
- 代理系统
  - 1. 链式工作流
  - 2. 并行化工作流
  - 3. 路由工作流
  - 4. 编排器-工作器
  - 5. 评估器-优化器
- Spring AI 的实现优势
  - 模型可移植性
  - 结构化输出

最新的快照版本,请使用 Spring AI 1.1.2!

在最近的研究出版物《构建高效代理》中,Anthropic 分享了关于构建高效大语言模型(LLM)代理的宝贵见解。这项研究特别有趣的地方在于它强调简单性和可组合性,而非复杂的框架。让我们探讨如何将这些原则转化为使用 Spring AI 的实际实现。

虽然模式描述和图表来源于 Anthropic 的原始出版物,但我们将专注于如何使用 Spring AI 的模型可移植性和结构化输出功能来实现这些模式。我们建议先阅读原始论文。

spring-ai-examples 仓库中的 agentic-patterns 目录包含以下所有示例的代码。

该研究出版物对两种代理系统类型做出了重要的架构区分:

**工作流(Workflows):** LLM 和工具通过预定义的代码路径进行编排的系统(例如,规范性系统)

**代理(Agents):** LLM 动态指导自身进程和工具使用的系统

关键洞察是,虽然完全自主的代理可能看起来很有吸引力,但对于明确定义的任务,工作流通常提供更好的可预测性和一致性。这与企业要求完美契合,在企业环境中,可靠性和可维护性至关重要。

让我们通过五种基本模式来审视 Spring AI 如何实现这些概念,每种模式都服务于特定的用例:

**链式工作流(Chain Workflow)** 模式体现了将复杂任务分解为更简单、更易管理的步骤的原则。

**使用场景:**
- 具有清晰顺序步骤的任务
- 当您愿意以延迟换取更高准确性时
- 当每个步骤都基于前一步骤的输出时

以下是 Spring AI 实现的实用示例:

该实现展示了几个关键原则:

- 每个步骤都有明确的职责
- 一个步骤的输出成为下一步的输入
- 链易于扩展和维护

**并行化工作流(Parallelization Workflow)** - LLM 可以同时处理任务,并以编程方式聚合其输出。

**使用场景:**
- 处理大量相似但独立的项目
- 需要多个独立视角的任务
- 当处理时间至关重要且任务可并行化时

**路由模式(Routing Pattern)** 实现智能任务分配,为不同类型的输入启用专业化处理。

**使用场景:**
- 具有明显输入类别的复杂任务
- 当不同输入需要专业化处理时
- 当可以准确处理分类时

**编排器-工作器模式(Orchestrator-Workers)** - 适用于无法预先预测子任务的复杂任务。

**使用场景:**
- 无法预先预测子任务的复杂任务
- 需要不同方法或视角的任务
- 需要自适应问题求解的情况

**评估器-优化器模式(Evaluator-Optimizer)** - 当存在明确的评估标准时。

**使用场景:**
- 存在明确的评估标准
- 迭代优化提供可衡量的价值
- 任务受益于多轮批评

Spring AI 对这些模式的实现提供了多个与 Anthropic 建议相一致的好处:

- 跨不同 LLM 提供商的统一接口
- 内置的错误处理和重试机制
- 灵活的提示管理

**设计原则:**

**从简单开始**
- 在添加复杂性之前先从基本工作流开始
- 使用满足您需求的最简单模式
- 仅在需要时增加复杂性

**为可靠性而设计**
- 实现清晰的错误处理
- 尽可能使用类型安全的响应
- 在每一步构建验证

**平衡延迟与准确性**
- 评估何时使用并行处理
- 在固定工作流和动态代理之间做出选择

这些指南将更新,以探索如何构建更高级的代理,将这些基础模式与复杂功能相结合:

**模式组合** - 结合多个模式以创建更强大的工作流
- 构建利用每种模式优势的混合系统
- 创建能够适应不断变化的需求的灵活架构

**高级代理内存管理** - 实现跨对话的持久内存
- 高效管理上下文窗口
- 开发长期知识保留策略

**工具和模型上下文协议(MCP)集成** - 通过标准化接口利用外部工具
- 实现 MCP 以增强模型交互
- 构建可扩展的代理架构

Anthropic 的研究见解与 Spring AI 的实际实现的结合,为构建高效的基于 LLM 的系统提供了强大的框架。

通过遵循这些模式和原则,开发人员可以创建健壮、可维护且高效的 AI 应用程序,这些应用程序提供真正的价值,同时避免不必要的复杂性。

关键是要记住,有时最简单的解决方案是最有效的。从基本模式开始,彻底了解您的用例,只有当复杂性能够显著改善系统性能或能力时才添加复杂性。

**示例:**

示例 1 (java):
```java
// 链式工作流示例
public class ChainWorkflow {
    private final ChatClient chatClient;
    private final String[] systemPrompts;

    /**
     * 执行链式工作流
     * @param userInput 用户输入
     * @return 处理后的响应
     */
    public String chain(String userInput) {
        String response = userInput;
        // 依次执行每个提示步骤
        for (String prompt : systemPrompts) {
            String input = String.format("{%s}\n {%s}", prompt, response);
            response = chatClient.prompt(input).call().content();
        }
        return response;
    }
}
```

示例 2 (java):
```java
// 并行化工作流示例
List<String> parallelResponse = new ParallelizationWorkflow(chatClient)
    .parallel(
        "分析市场变化将如何影响这些利益相关者群体。",
        List.of(
            "客户: ...",
            "员工: ...",
            "投资者: ...",
            "供应商: ..."
        ),
        4  // 并行度
    );
```

示例 3 (java):
```java
// 路由工作流示例
@Autowired
private ChatClient chatClient;

RoutingWorkflow workflow = new RoutingWorkflow(chatClient);

// 定义路由映射
Map<String, String> routes = Map.of(
    "billing", "您是账单专家。帮助解决账单问题...",
    "technical", "您是技术支持工程师。帮助解决技术问题...",
    "general", "您是客户服务代表。帮助处理一般咨询..."
);

String input = "上周我的账户被收取了两次费用";
String response = workflow.route(input, routes);
```

示例 4 (java):
```java
// 编排器-工作器工作流示例
public class OrchestratorWorkersWorkflow {
    /**
     * 处理任务描述
     * @param taskDescription 任务描述
     * @return 工作器响应
     */
    public WorkerResponse process(String taskDescription) {
        // 1. 编排器分析任务并确定子任务
        OrchestratorResponse orchestratorResponse = // ...

        // 2. 工作器并行处理子任务
        List<String> workerResponses = // ...

        // 3. 将结果组合成最终响应
        return new WorkerResponse(/*...*/);
    }
}
```

---

## 构建高效代理 :: Spring AI 参考文档

**URL:** https://docs.spring.io/spring-ai/reference/api/effective-agents.html

**目录:**
- 构建高效代理
- 代理系统
  - 1. 链式工作流
  - 2. 并行化工作流
  - 3. 路由工作流
  - 4. 编排器-工作器
  - 5. 评估器-优化器
- Spring AI 的实现优势
  - 模型可移植性
  - 结构化输出

在最近的研究出版物《构建高效代理》中,Anthropic 分享了关于构建高效大语言模型(LLM)代理的宝贵见解。这项研究特别有趣的地方在于它强调简单性和可组合性,而非复杂的框架。让我们探讨如何将这些原则转化为使用 Spring AI 的实际实现。

虽然模式描述和图表来源于 Anthropic 的原始出版物,但我们将专注于如何使用 Spring AI 的模型可移植性和结构化输出功能来实现这些模式。我们建议先阅读原始论文。

spring-ai-examples 仓库中的 agentic-patterns 目录包含以下所有示例的代码。

该研究出版物对两种代理系统类型做出了重要的架构区分:

**工作流(Workflows):** LLM 和工具通过预定义的代码路径进行编排的系统(例如,规范性系统)

**代理(Agents):** LLM 动态指导自身进程和工具使用的系统

关键洞察是,虽然完全自主的代理可能看起来很有吸引力,但对于明确定义的任务,工作流通常提供更好的可预测性和一致性。这与企业要求完美契合,在企业环境中,可靠性和可维护性至关重要。

让我们通过五种基本模式来审视 Spring AI 如何实现这些概念,每种模式都服务于特定的用例:

**链式工作流(Chain Workflow)** 模式体现了将复杂任务分解为更简单、更易管理的步骤的原则。

**使用场景:**
- 具有清晰顺序步骤的任务
- 当您愿意以延迟换取更高准确性时
- 当每个步骤都基于前一步骤的输出时

以下是 Spring AI 实现的实用示例:

该实现展示了几个关键原则:

- 每个步骤都有明确的职责
- 一个步骤的输出成为下一步的输入
- 链易于扩展和维护

**并行化工作流(Parallelization Workflow)** - LLM 可以同时处理任务,并以编程方式聚合其输出。

**使用场景:**
- 处理大量相似但独立的项目
- 需要多个独立视角的任务
- 当处理时间至关重要且任务可并行化时

**路由模式(Routing Pattern)** 实现智能任务分配,为不同类型的输入启用专业化处理。

**使用场景:**
- 具有明显输入类别的复杂任务
- 当不同输入需要专业化处理时
- 当可以准确处理分类时

**编排器-工作器模式(Orchestrator-Workers)** - 适用于无法预先预测子任务的复杂任务。

**使用场景:**
- 无法预先预测子任务的复杂任务
- 需要不同方法或视角的任务
- 需要自适应问题求解的情况

**评估器-优化器模式(Evaluator-Optimizer)** - 当存在明确的评估标准时。

**使用场景:**
- 存在明确的评估标准
- 迭代优化提供可衡量的价值
- 任务受益于多轮批评

Spring AI 对这些模式的实现提供了多个与 Anthropic 建议相一致的好处:

- 跨不同 LLM 提供商的统一接口
- 内置的错误处理和重试机制
- 灵活的提示管理

**设计原则:**

**从简单开始**
- 在添加复杂性之前先从基本工作流开始
- 使用满足您需求的最简单模式
- 仅在需要时增加复杂性

**为可靠性而设计**
- 实现清晰的错误处理
- 尽可能使用类型安全的响应
- 在每一步构建验证

**平衡延迟与准确性**
- 评估何时使用并行处理
- 在固定工作流和动态代理之间做出选择

这些指南将更新,以探索如何构建更高级的代理,将这些基础模式与复杂功能相结合:

**模式组合** - 结合多个模式以创建更强大的工作流
- 构建利用每种模式优势的混合系统
- 创建能够适应不断变化的需求的灵活架构

**高级代理内存管理** - 实现跨对话的持久内存
- 高效管理上下文窗口
- 开发长期知识保留策略

**工具和模型上下文协议(MCP)集成** - 通过标准化接口利用外部工具
- 实现 MCP 以增强模型交互
- 构建可扩展的代理架构

Anthropic 的研究见解与 Spring AI 的实际实现的结合,为构建高效的基于 LLM 的系统提供了强大的框架。

通过遵循这些模式和原则,开发人员可以创建健壮、可维护且高效的 AI 应用程序,这些应用程序提供真正的价值,同时避免不必要的复杂性。

关键是要记住,有时最简单的解决方案是最有效的。从基本模式开始,彻底了解您的用例,只有当复杂性能够显著改善系统性能或能力时才添加复杂性。

**示例:**

示例 1 (java):
```java
// 链式工作流示例
public class ChainWorkflow {
    private final ChatClient chatClient;
    private final String[] systemPrompts;

    /**
     * 执行链式工作流
     * @param userInput 用户输入
     * @return 处理后的响应
     */
    public String chain(String userInput) {
        String response = userInput;
        // 依次执行每个提示步骤
        for (String prompt : systemPrompts) {
            String input = String.format("{%s}\n {%s}", prompt, response);
            response = chatClient.prompt(input).call().content();
        }
        return response;
    }
}
```

示例 2 (java):
```java
// 并行化工作流示例
List<String> parallelResponse = new ParallelizationWorkflow(chatClient)
    .parallel(
        "分析市场变化将如何影响这些利益相关者群体。",
        List.of(
            "客户: ...",
            "员工: ...",
            "投资者: ...",
            "供应商: ..."
        ),
        4  // 并行度
    );
```

示例 3 (java):
```java
// 路由工作流示例
@Autowired
private ChatClient chatClient;

RoutingWorkflow workflow = new RoutingWorkflow(chatClient);

// 定义路由映射
Map<String, String> routes = Map.of(
    "billing", "您是账单专家。帮助解决账单问题...",
    "technical", "您是技术支持工程师。帮助解决技术问题...",
    "general", "您是客户服务代表。帮助处理一般咨询..."
);

String input = "上周我的账户被收取了两次费用";
String response = workflow.route(input, routes);
```

示例 4 (java):
```java
// 编排器-工作器工作流示例
public class OrchestratorWorkersWorkflow {
    /**
     * 处理任务描述
     * @param taskDescription 任务描述
     * @return 工作器响应
     */
    public WorkerResponse process(String taskDescription) {
        // 1. 编排器分析任务并确定子任务
        OrchestratorResponse orchestratorResponse = // ...

        // 2. 工作器并行处理子任务
        List<String> workerResponses = // ...

        // 3. 将结果组合成最终响应
        return new WorkerResponse(/*...*/);
    }
}
```

---

## 构建高效代理 :: Spring AI 参考文档

**URL:** https://docs.spring.io/spring-ai/reference/2.0/api/effective-agents.html

**目录:**
- 构建高效代理
- 代理系统
  - 1. 链式工作流
  - 2. 并行化工作流
  - 3. 路由工作流
  - 4. 编排器-工作器
  - 5. 评估器-优化器
- Spring AI 的实现优势
  - 模型可移植性
  - 结构化输出

此版本仍在开发中,尚未被视为稳定版本。最新的快照版本,请使用 Spring AI 1.1.2!

在最近的研究出版物《构建高效代理》中,Anthropic 分享了关于构建高效大语言模型(LLM)代理的宝贵见解。这项研究特别有趣的地方在于它强调简单性和可组合性,而非复杂的框架。让我们探讨如何将这些原则转化为使用 Spring AI 的实际实现。

虽然模式描述和图表来源于 Anthropic 的原始出版物,但我们将专注于如何使用 Spring AI 的模型可移植性和结构化输出功能来实现这些模式。我们建议先阅读原始论文。

spring-ai-examples 仓库中的 agentic-patterns 目录包含以下所有示例的代码。

该研究出版物对两种代理系统类型做出了重要的架构区分:

**工作流(Workflows):** LLM 和工具通过预定义的代码路径进行编排的系统(例如,规范性系统)

**代理(Agents):** LLM 动态指导自身进程和工具使用的系统

关键洞察是,虽然完全自主的代理可能看起来很有吸引力,但对于明确定义的任务,工作流通常提供更好的可预测性和一致性。这与企业要求完美契合,在企业环境中,可靠性和可维护性至关重要。

让我们通过五种基本模式来审视 Spring AI 如何实现这些概念,每种模式都服务于特定的用例:

**链式工作流(Chain Workflow)** 模式体现了将复杂任务分解为更简单、更易管理的步骤的原则。

**使用场景:**
- 具有清晰顺序步骤的任务
- 当您愿意以延迟换取更高准确性时
- 当每个步骤都基于前一步骤的输出时

以下是 Spring AI 实现的实用示例:

该实现展示了几个关键原则:

- 每个步骤都有明确的职责
- 一个步骤的输出成为下一步的输入
- 链易于扩展和维护

**并行化工作流(Parallelization Workflow)** - LLM 可以同时处理任务,并以编程方式聚合其输出。

**使用场景:**
- 处理大量相似但独立的项目
- 需要多个独立视角的任务
- 当处理时间至关重要且任务可并行化时

**路由模式(Routing Pattern)** 实现智能任务分配,为不同类型的输入启用专业化处理。

**使用场景:**
- 具有明显输入类别的复杂任务
- 当不同输入需要专业化处理时
- 当可以准确处理分类时

**编排器-工作器模式(Orchestrator-Workers)** - 适用于无法预先预测子任务的复杂任务。

**使用场景:**
- 无法预先预测子任务的复杂任务
- 需要不同方法或视角的任务
- 需要自适应问题求解的情况

**评估器-优化器模式(Evaluator-Optimizer)** - 当存在明确的评估标准时。

**使用场景:**
- 存在明确的评估标准
- 迭代优化提供可衡量的价值
- 任务受益于多轮批评

Spring AI 对这些模式的实现提供了多个与 Anthropic 建议相一致的好处:

- 跨不同 LLM 提供商的统一接口
- 内置的错误处理和重试机制
- 灵活的提示管理

**设计原则:**

**从简单开始**
- 在添加复杂性之前先从基本工作流开始
- 使用满足您需求的最简单模式
- 仅在需要时增加复杂性

**为可靠性而设计**
- 实现清晰的错误处理
- 尽可能使用类型安全的响应
- 在每一步构建验证

**平衡延迟与准确性**
- 评估何时使用并行处理
- 在固定工作流和动态代理之间做出选择

这些指南将更新,以探索如何构建更高级的代理,将这些基础模式与复杂功能相结合:

**模式组合** - 结合多个模式以创建更强大的工作流
- 构建利用每种模式优势的混合系统
- 创建能够适应不断变化的需求的灵活架构

**高级代理内存管理** - 实现跨对话的持久内存
- 高效管理上下文窗口
- 开发长期知识保留策略

**工具和模型上下文协议(MCP)集成** - 通过标准化接口利用外部工具
- 实现 MCP 以增强模型交互
- 构建可扩展的代理架构

Anthropic 的研究见解与 Spring AI 的实际实现的结合,为构建高效的基于 LLM 的系统提供了强大的框架。

通过遵循这些模式和原则,开发人员可以创建健壮、可维护且高效的 AI 应用程序,这些应用程序提供真正的价值,同时避免不必要的复杂性。

关键是要记住,有时最简单的解决方案是最有效的。从基本模式开始,彻底了解您的用例,只有当复杂性能够显著改善系统性能或能力时才添加复杂性。

**示例:**

示例 1 (java):
```java
// 链式工作流示例
public class ChainWorkflow {
    private final ChatClient chatClient;
    private final String[] systemPrompts;

    /**
     * 执行链式工作流
     * @param userInput 用户输入
     * @return 处理后的响应
     */
    public String chain(String userInput) {
        String response = userInput;
        // 依次执行每个提示步骤
        for (String prompt : systemPrompts) {
            String input = String.format("{%s}\n {%s}", prompt, response);
            response = chatClient.prompt(input).call().content();
        }
        return response;
    }
}
```

示例 2 (java):
```java
// 并行化工作流示例
List<String> parallelResponse = new ParallelizationWorkflow(chatClient)
    .parallel(
        "分析市场变化将如何影响这些利益相关者群体。",
        List.of(
            "客户: ...",
            "员工: ...",
            "投资者: ...",
            "供应商: ..."
        ),
        4  // 并行度
    );
```

示例 3 (java):
```java
// 路由工作流示例
@Autowired
private ChatClient chatClient;

RoutingWorkflow workflow = new RoutingWorkflow(chatClient);

// 定义路由映射
Map<String, String> routes = Map.of(
    "billing", "您是账单专家。帮助解决账单问题...",
    "technical", "您是技术支持工程师。帮助解决技术问题...",
    "general", "您是客户服务代表。帮助处理一般咨询..."
);

String input = "上周我的账户被收取了两次费用";
String response = workflow.route(input, routes);
```

示例 4 (java):
```java
// 编排器-工作器工作流示例
public class OrchestratorWorkersWorkflow {
    /**
     * 处理任务描述
     * @param taskDescription 任务描述
     * @return 工作器响应
     */
    public WorkerResponse process(String taskDescription) {
        // 1. 编排器分析任务并确定子任务
        OrchestratorResponse orchestratorResponse = // ...

        // 2. 工作器并行处理子任务
        List<String> workerResponses = // ...

        // 3. 将结果组合成最终响应
        return new WorkerResponse(/*...*/);
    }
}
```

---
