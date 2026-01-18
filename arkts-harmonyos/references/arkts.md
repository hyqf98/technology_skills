# ArkTS 语言参考文档

> **HarmonyOS Next ArkTS 完整参考手册**
> **更新时间**: 2026-01-18
> **适用版本**: HarmonyOS Next API 12+
> **文档页数**: 170+ 页

## 📋 目录导航

### 快速导航
- [🚀 HarmonyOS Next 新特性](#harmonyos-next-新特性) - 最新特性和改进
- [📖 核心语言特性](#核心语言特性) - ArkTS 基础语法
- [🎯 实战示例集锦](#实战示例集锦) - 实用代码示例
- [⚡ 性能优化指南](#性能优化指南) - 优化建议和最佳实践
- [🔧 常见问题解答](#常见问题解答) - 问题排查和解决方案
- [📚 完整示例代码](#完整示例代码) - 项目源码

### 主要内容分类
1. **基础语法** - 类型系统、类、接口、泛型
2. **组件开发** - UI 组件、状态管理、生命周期
3. **数据管理** - 本地存储、分布式数据、网络请求
4. **多媒体** - 音视频、图像、相机
5. **系统能力** - 权限、位置、通知、后台任务
6. **性能优化** - 内存管理、渲染优化、异步处理

---

## 🚀 HarmonyOS Next 新特性

### API 12+ 核心增强

#### 1. 性能提升
- **编译优化**: ArkTS 编译器性能提升 30%
- **运行时优化**: 框架启动速度提升 40%
- **内存优化**: 应用内存占用降低 25%

#### 2. 新增语言特性
```typescript
// 1. 增强的模式匹配
describeMatch(value: unknown): string {
  switch (value) {
    case { type: 'user', name: string, age: number }:
      return `用户: ${name}, 年龄: ${age}`;
    case { type: 'product', price: number }:
      return `产品价格: ${price}`;
    default:
      return '未知类型';
  }
}

// 2. 更强大的泛型约束
interface Container<T> {
  value: T;
  clone(): Container<T>;
}

// 3. 异步流处理 (Async Iterators)
async function processStream(items: AsyncIterable<T>) {
  for await (const item of items) {
    await processItem(item);
  }
}
```

#### 3. 状态管理增强
```typescript
// 新增的 @ObservedV2 和 @Trace 装饰器
@ObservedV2
class UserModel {
  @Trace name: string = '';  // 自动深度观察
  @Trace age: number = 0;

  @Computed get displayName(): string {
    return `${this.name} (${this.age})`;
  }
}

// 使用 @Local 优化本地状态
@Component
struct UserCard {
  @Local user: UserModel = new UserModel();

  build() {
    Text(this.user.displayName)
  }
}
```

#### 4. 并发编程支持
```typescript
// TaskPool 并发任务
import { taskpool } from '@kit.ArkTS';

@Concurrent
function computeHeavyTask(data: number[]): number {
  // 并发计算密集型任务
  return data.reduce((sum, val) => sum + val, 0);
}

// 使用 Worker 线程
async function runConcurrentTask() {
  const task = new taskpool.Task(computeHeavyTask, [1, 2, 3, 4, 5]);
  const result = await taskpool.execute(task);
  console.log(`计算结果: ${result}`);
}
```

#### 5. 网络请求增强
```typescript
// 新增的 HTTP 客户端 API
import { http } from '@kit.NetworkKit';

async function fetchWithRetry(url: string): Promise<void> {
  const httpClient = http.createHttp();

  try {
    // 支持重试、超时、缓存等高级特性
    const response = await httpClient.request(url, {
      method: http.RequestMethod.GET,
      connectTimeout: 10000,
      readTimeout: 10000,
      retryCount: 3,
      retryInterval: 1000,
      header: {
        'Content-Type': 'application/json'
      }
    });

    console.log(`响应码: ${response.responseCode}`);
    console.log(`响应数据: ${response.result as string}`);
  } catch (error) {
    console.error(`请求失败: ${error.message}`);
  } finally {
    httpClient.destroy();  // 记得释放资源
  }
}
```

---

## 📖 核心语言特性

### 类型系统增强

#### 联合类型与交叉类型
```typescript
// 联合类型 - 可以是多种类型之一
type StringOrNumber = string | number;

function processValue(value: StringOrNumber): void {
  if (typeof value === 'string') {
    console.log(`字符串长度: ${value.length}`);
  } else {
    console.log(`数值翻倍: ${value * 2}`);
  }
}

// 交叉类型 - 合并多个类型
type Colorful = { color: string };
type Circle = { radius: number };
type ColorfulCircle = Colorful & Circle;

const myCircle: ColorfulCircle = {
  color: 'red',
  radius: 10
};
```

#### 泛型编程
```typescript
// 泛型函数
function firstElement<T>(arr: T[]): T | undefined {
  return arr[0];
}

// 泛型约束
interface Lengthwise {
  length: number;
}

function logLength<T extends Lengthwise>(arg: T): void {
  console.log(`长度: ${arg.length}`);
}

// 泛型类
class Storage<T> {
  private items: T[] = [];

  add(item: T): void {
    this.items.push(item);
  }

  get(index: number): T | undefined {
    return this.items[index];
  }

  // 泛型方法
  map<U>(callback: (item: T) => U): U[] {
    return this.items.map(callback);
  }
}
```

#### 空安全与类型守卫
```typescript
// 空安全处理
function processUser(user: User | null): string {
  // 使用可选链操作符
  const name = user?.profile?.name ?? '匿名用户';

  // 类型断言
  if (user !== null) {
    // TypeScript 知道这里 user 非空
    return user.name;
  }

  return name;
}

// 自定义类型守卫
function isString(value: unknown): value is string {
  return typeof value === 'string';
}

function processValue(value: unknown): void {
  if (isString(value)) {
    // 这里 value 被推断为 string 类型
    console.log(value.toUpperCase());
  }
}
```

### 面向对象编程

#### 类与继承
```typescript
// 基类
abstract class Animal {
  protected name: string;

  constructor(name: string) {
    this.name = name;
  }

  // 抽象方法
  abstract makeSound(): void;

  // 具体方法
  move(): void {
    console.log(`${this.name} 正在移动`);
  }
}

// 派生类
class Dog extends Animal {
  private breed: string;

  constructor(name: string, breed: string) {
    super(name);  // 调用父类构造函数
    this.breed = breed;
  }

  makeSound(): void {
    console.log('汪汪！');
  }

  // 重写父类方法
  move(): void {
    super.move();  // 调用父类方法
    console.log(`${this.name} 跑得很快`);
  }
}
```

#### 接口与实现
```typescript
// 接口定义
interface Drawable {
  draw(context: CanvasRenderingContext2D): void;
}

interface Transformable {
  scale(factor: number): void;
  rotate(angle: number): void;
}

// 类实现多个接口
class Rectangle implements Drawable, Transformable {
  constructor(
    private width: number,
    private height: number
  ) {}

  draw(context: CanvasRenderingContext2D): void {
    context.fillRect(0, 0, this.width, this.height);
  }

  scale(factor: number): void {
    this.width *= factor;
    this.height *= factor;
  }

  rotate(angle: number): void {
    // 旋转实现
  }
}
```

#### 访问修饰符与封装
```typescript
class BankAccount {
  // 公有属性 - 外部可访问
  public accountNumber: string;

  // 私有属性 - 仅类内部可访问
  private balance: number;

  // 保护属性 - 类及其子类可访问
  protected owner: string;

  // 只读属性 - 只能在构造函数中设置
  public readonly createdAt: Date;

  constructor(accountNumber: string, owner: string) {
    this.accountNumber = accountNumber;
    this.balance = 0;
    this.owner = owner;
    this.createdAt = new Date();
  }

  // Getter 方法
  get currentBalance(): number {
    return this.balance;
  }

  // Setter 方法 - 可以添加验证逻辑
  set deposit(amount: number) {
    if (amount <= 0) {
      throw new Error('存款金额必须大于零');
    }
    this.balance += amount;
  }

  // 私有方法
  private validateTransaction(amount: number): boolean {
    return amount > 0 && amount <= this.balance;
  }

  // 公有方法
  public withdraw(amount: number): boolean {
    if (this.validateTransaction(amount)) {
      this.balance -= amount;
      return true;
    }
    return false;
  }
}
```

### 高级特性

#### 装饰器
```typescript
// 组件装饰器
@Component
struct MyComponent {
  // 状态装饰器 - 状态变化会触发 UI 更新
  @State count: number = 0;

  // props 装饰器 - 从父组件接收数据
  @Prop title: string = '';

  // Link 装饰器 - 双向数据绑定
  @Link inputValue: string;

  // Provide 装饰器 - 向子组件提供数据
  @Provide theme: string = 'light';

  build() {
    Column() {
      Text(`${this.title}: ${this.count}`)
      Button('点击')
        .onClick(() => {
          this.count++;
        })
    }
  }
}

// 自定义装饰器
function Log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function(...args: any[]) {
    console.log(`调用方法: ${propertyKey}`);
    console.log(`参数:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`返回值:`, result);
    return result;
  };

  return descriptor;
}

class MyClass {
  @Log
  myMethod(param: string): string {
    return `处理结果: ${param}`;
  }
}
```

#### 异步编程
```typescript
// Promise 链式调用
async function fetchUserData(): Promise<void> {
  try {
    const user = await getUserById(1);
    const profile = await getUserProfile(user.id);
    const posts = await getUserPosts(user.id);

    console.log('用户数据:', user);
    console.log('个人资料:', profile);
    console.log('用户文章:', posts);
  } catch (error) {
    console.error('获取数据失败:', error);
  }
}

// 并发请求
async function fetchMultipleData(): Promise<void> {
  try {
    const [users, posts, comments] = await Promise.all([
      fetchUsers(),
      fetchPosts(),
      fetchComments()
    ]);

    console.log('所有数据获取完成');
  } catch (error) {
    console.error('至少一个请求失败:', error);
  }
}

// 异步迭代器
async function processAsyncStream(): Promise<void> {
  const stream = createAsyncStream();

  for await (const item of stream) {
    console.log('处理项目:', item);
  }
}
```

---

## ⚡ 性能优化指南

### 1. 状态管理优化

#### 避免不必要的重新渲染
```typescript
// ❌ 不好的做法 - 每次都会重新创建对象
@Component
struct BadExample {
  @State items: Item[] = [];

  build() {
    List() {
      ForEach(this.items, (item: Item) => {
        ListItem() {
          Text(item.name)
        }
      })  // 缺少 key，会导致性能问题
    }
  }
}

// ✅ 好的做法 - 使用唯一标识符
@Component
struct GoodExample {
  @State items: Item[] = [];

  build() {
    List() {
      ForEach(this.items, (item: Item, index: number) => {
        ListItem() {
          Text(item.name)
        }
      }, (item: Item, index: number) => item.id.toString())  // 使用唯一 key
    }
  }
}
```

#### 使用 @Local 优化本地状态
```typescript
@Component
struct OptimizedComponent {
  @State globalState: string = '';  // 需要全局共享的状态
  @Local localState: string = '';   // 仅组件内部使用的状态

  build() {
    Column() {
      Text(this.globalState)  // 全局状态变化时重新渲染
      Text(this.localState)   // 本地状态变化时才重新渲染
    }
  }
}
```

### 2. 列表性能优化

#### 使用懒加载
```typescript
@Component
struct LazyListComponent {
  @State items: Item[] = [];

  build() {
    List() {
      ForEach(this.items, (item: Item) => {
        ListItem() {
          ExpensiveItem({ item: item })
        }
      }, (item: Item) => item.id)
    }
    .cachedCount(5)  // 缓存 5 个屏幕外的项
    .space(10)       // 设置间距
  }
}

@Component
struct ExpensiveItem {
  @Prop item: Item;

  build() {
    // 使用 aboutToAppear 延迟加载资源
    Text(this.item.name)
      .aboutToAppear(() => {
        // 预加载资源
      })
  }
}
```

### 3. 内存优化

#### 及时释放资源
```typescript
@Component
struct MediaPlayer {
  private player: media.AVPlayer | null = null;

  aboutToAppear(): void {
    this.player = await media.createAVPlayer();
  }

  aboutToDisappear(): void {
    // 组件销毁时释放资源
    if (this.player) {
      this.player.release();
      this.player = null;
    }
  }
}
```

#### 使用对象池
```typescript
// 对象池实现
class ObjectPool<T> {
  private pool: T[] = [];
  private factory: () => T;
  private reset: (obj: T) => void;

  constructor(factory: () => T, reset: (obj: T) => void, initialSize: number = 10) {
    this.factory = factory;
    this.reset = reset;

    for (let i = 0; i < initialSize; i++) {
      this.pool.push(factory());
    }
  }

  acquire(): T {
    if (this.pool.length > 0) {
      return this.pool.pop()!;
    }
    return this.factory();
  }

  release(obj: T): void {
    this.reset(obj);
    this.pool.push(obj);
  }
}

// 使用对象池
const itemPool = new ObjectPool(
  () => new ListItem(),
  (item) => item.reset(),
  20
);
```

### 4. 网络请求优化

#### 请求合并与缓存
```typescript
class NetworkManager {
  private cache: Map<string, CachedData> = new Map();
  private pendingRequests: Map<string, Promise<any>> = new Map();

  async fetchWithCache(url: string, ttl: number = 60000): Promise<any> {
    // 检查缓存
    const cached = this.cache.get(url);
    if (cached && Date.now() - cached.timestamp < ttl) {
      return cached.data;
    }

    // 检查是否有正在进行的请求
    const pending = this.pendingRequests.get(url);
    if (pending) {
      return pending;
    }

    // 发起新请求
    const promise = this.fetchData(url);
    this.pendingRequests.set(url, promise);

    try {
      const data = await promise;
      this.cache.set(url, { data, timestamp: Date.now() });
      return data;
    } finally {
      this.pendingRequests.delete(url);
    }
  }

  private async fetchData(url: string): Promise<any> {
    // 实际的网络请求逻辑
  }
}

interface CachedData {
  data: any;
  timestamp: number;
}
```

### 5. 渲染性能优化

#### 使用 Canvas 优化复杂绘制
```typescript
@Component
struct CanvasRenderer {
  private settings: RenderingContextSettings = new RenderingContextSettings(true);
  private context: CanvasRenderingContext2D = new CanvasRenderingContext2D(this.settings);

  build() {
    Canvas(this.context)
      .onReady(() => {
        this.drawComplexScene();
      })
  }

  private drawComplexScene(): void {
    // 批量绘制操作
    this.context.beginPath();
    this.context.moveTo(0, 0);
    // ... 更多绘制操作
    this.context.stroke();
  }
}
```

---

## 🔧 常见问题解答

### Q1: 如何处理组件间的数据共享？
```typescript
// 使用 @Provide 和 @Consume
@Entry
@Component
struct ParentComponent {
  @Provide sharedData: string = '共享数据';

  build() {
    Column() {
      ChildComponent()
      AnotherChildComponent()
    }
  }
}

@Component
struct ChildComponent {
  @Consume sharedData: string;

  build() {
    Text(this.sharedData)  // 自动获取父组件提供的数据
  }
}
```

### Q2: 如何优化大型列表的性能？
```typescript
@Component
struct OptimizedLargeList {
  @State dataSource: ItemDataSource = new ItemDataSource();

  build() {
    List() {
      LazyForEach(this.dataSource, (item: Item) => {
        ListItem() {
          ItemComponent({ item: item })
        }
      }, (item: Item) => item.id.toString())
    }
    .cachedCount(10)  // 缓存更多项
  }
}

// 实现数据源
class ItemDataSource implements IDataSource {
  private items: Item[] = [];
  private listeners: DataChangeListener[] = [];

  totalCount(): number {
    return this.items.length;
  }

  getData(index: number): Item {
    return this.items[index];
  }

  registerDataChangeListener(listener: DataChangeListener): void {
    this.listeners.push(listener);
  }

  unregisterDataChangeListener(listener: DataChangeListener): void {
    const index = this.listeners.indexOf(listener);
    if (index >= 0) {
      this.listeners.splice(index, 1);
    }
  }
}
```

### Q3: 如何处理异步操作的错误？
```typescript
async function handleAsyncOperation(): Promise<void> {
  try {
    // 方式1: 使用 try-catch
    const result = await riskyOperation();
    console.log('操作成功:', result);
  } catch (error) {
    console.error('操作失败:', error);
    // 错误处理逻辑
    if (error instanceof BusinessError) {
      // 处理业务错误
    } else {
      // 处理系统错误
    }
  }
}

// 方式2: 使用 Promise 方法链
function handleWithPromiseChain(): void {
  riskyOperation()
    .then(result => {
      console.log('操作成功:', result);
      return nextOperation(result);
    })
    .then(finalResult => {
      console.log('最终结果:', finalResult);
    })
    .catch(error => {
      console.error('操作失败:', error);
      // 统一错误处理
    });
}
```

### Q4: 如何实现组件的生命周期管理？
```typescript
@Component
struct LifecycleExample {
  @State data: string = '';

  // 组件即将出现
  aboutToAppear(): void {
    console.log('组件即将出现');
    this.loadData();
  }

  // 组件即将消失
  aboutToDisappear(): void {
    console.log('组件即将消失');
    this.cleanup();
  }

  // 页面即将显示
  onPageShow(): void {
    console.log('页面即将显示');
  }

  // 页面即将隐藏
  onPageHide(): void {
    console.log('页面即将隐藏');
  }

  private async loadData(): Promise<void> {
    // 加载数据
  }

  private cleanup(): void {
    // 清理资源
  }
}
```

---

## 📚 完整示例代码

### 实用工具函数集合

#### 1. 日期时间工具
```typescript
/**
 * 日期时间工具类
 * 提供常用的日期格式化、计算、比较等功能
 */
class DateTimeUtil {
  /**
   * 格式化日期为指定格式
   * @param date 日期对象
   * @param format 格式字符串，如 'YYYY-MM-DD HH:mm:ss'
   * @returns 格式化后的日期字符串
   */
  static format(date: Date, format: string = 'YYYY-MM-DD HH:mm:ss'): string {
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');
    const hours = String(date.getHours()).padStart(2, '0');
    const minutes = String(date.getMinutes()).padStart(2, '0');
    const seconds = String(date.getSeconds()).padStart(2, '0');

    return format
      .replace('YYYY', year.toString())
      .replace('MM', month)
      .replace('DD', day)
      .replace('HH', hours)
      .replace('mm', minutes)
      .replace('ss', seconds);
  }

  /**
   * 获取相对时间描述
   * @param date 目标日期
   * @returns 相对时间描述，如 '刚刚'、'5分钟前'、'2小时前'
   */
  static getRelativeTime(date: Date): string {
    const now = new Date();
    const diff = now.getTime() - date.getTime();
    const seconds = Math.floor(diff / 1000);
    const minutes = Math.floor(seconds / 60);
    const hours = Math.floor(minutes / 60);
    const days = Math.floor(hours / 24);

    if (seconds < 60) {
      return '刚刚';
    } else if (minutes < 60) {
      return `${minutes}分钟前`;
    } else if (hours < 24) {
      return `${hours}小时前`;
    } else if (days < 7) {
      return `${days}天前`;
    } else {
      return this.format(date, 'YYYY-MM-DD');
    }
  }

  /**
   * 计算两个日期之间的天数差
   * @param date1 第一个日期
   * @param date2 第二个日期
   * @returns 天数差（绝对值）
   */
  static daysBetween(date1: Date, date2: Date): number {
    const oneDay = 24 * 60 * 60 * 1000;
    return Math.abs(Math.floor((date1.getTime() - date2.getTime()) / oneDay));
  }

  /**
   * 判断是否为今天
   * @param date 待判断的日期
   * @returns 是否为今天
   */
  static isToday(date: Date): boolean {
    const today = new Date();
    return date.getFullYear() === today.getFullYear() &&
           date.getMonth() === today.getMonth() &&
           date.getDate() === today.getDate();
  }
}
```

#### 2. 字符串工具
```typescript
/**
 * 字符串工具类
 * 提供字符串处理、验证、转换等功能
 */
class StringUtil {
  /**
   * 判断字符串是否为空或仅包含空白字符
   * @param str 待判断的字符串
   * @returns 是否为空
   */
  static isEmpty(str: string | null | undefined): boolean {
    return str === null || str === undefined || str.trim().length === 0;
  }

  /**
   * 截断字符串并添加省略号
   * @param str 原字符串
   * @param maxLength 最大长度
   * @param suffix 后缀，默认为 '...'
   * @returns 截断后的字符串
   */
  static truncate(str: string, maxLength: number, suffix: string = '...'): string {
    if (str.length <= maxLength) {
      return str;
    }
    return str.substring(0, maxLength - suffix.length) + suffix;
  }

  /**
   * 生成随机字符串
   * @param length 字符串长度
   * @param charset 字符集，默认为字母和数字
   * @returns 随机字符串
   */
  static random(length: number = 8, charset: string = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'): string {
    let result = '';
    for (let i = 0; i < length; i++) {
      result += charset.charAt(Math.floor(Math.random() * charset.length));
    }
    return result;
  }

  /**
   * 驼峰命名转换为短横线命名
   * @param str 驼峰命名字符串
   * @returns 短横线命名字符串
   */
  static camelToKebab(str: string): string {
    return str.replace(/([a-z])([A-Z])/g, '$1-$2').toLowerCase();
  }

  /**
   * 短横线命名转换为驼峰命名
   * @param str 短横线命名字符串
   * @returns 驼峰命名字符串
   */
  static kebabToCamel(str: string): string {
    return str.replace(/-([a-z])/g, (_, letter) => letter.toUpperCase());
  }

  /**
   * 手机号脱敏处理
   * @param phoneNumber 手机号
   * @returns 脱敏后的手机号，如 138****1234
   */
  static maskPhoneNumber(phoneNumber: string): string {
    if (phoneNumber.length !== 11) {
      return phoneNumber;
    }
    return phoneNumber.substring(0, 3) + '****' + phoneNumber.substring(7);
  }

  /**
   * 验证手机号格式
   * @param phoneNumber 手机号
   * @returns 是否有效
   */
  static isValidPhoneNumber(phoneNumber: string): boolean {
    const regex = /^1[3-9]\d{9}$/;
    return regex.test(phoneNumber);
  }

  /**
   * 验证邮箱格式
   * @param email 邮箱地址
   * @returns 是否有效
   */
  static isValidEmail(email: string): boolean {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(email);
  }
}
```

#### 3. 存储工具
```typescript
/**
 * 本地存储工具类
 * 封装 Preferences API，提供类型安全的存储操作
 */
import { preferences } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

class StorageUtil {
  private static instance: StorageUtil;
  private preferencesStore: preferences.Preferences | null = null;
  private readonly STORE_NAME = 'app_preferences';

  private constructor() {
    this.initPreferences();
  }

  /**
   * 获取单例实例
   */
  static getInstance(): StorageUtil {
    if (!StorageUtil.instance) {
      StorageUtil.instance = new StorageUtil();
    }
    return StorageUtil.instance;
  }

  /**
   * 初始化 Preferences
   */
  private async initPreferences(): Promise<void> {
    try {
      const context = getContext(this);
      this.preferencesStore = await preferences.getPreferences(context, this.STORE_NAME);
      console.info('Preferences 初始化成功');
    } catch (err) {
      const error = err as BusinessError;
      console.error(`Preferences 初始化失败: ${error.code}, ${error.message}`);
    }
  }

  /**
   * 保存数据
   * @param key 键
   * @param value 值（支持 string、number、boolean）
   */
  async put(key: string, value: string | number | boolean): Promise<void> {
    try {
      if (this.preferencesStore) {
        await this.preferencesStore.put(key, value);
        await this.preferencesStore.flush();
        console.info(`保存数据成功: ${key} = ${value}`);
      }
    } catch (err) {
      const error = err as BusinessError;
      console.error(`保存数据失败: ${error.code}, ${error.message}`);
    }
  }

  /**
   * 获取数据
   * @param key 键
   * @param defaultValue 默认值
   * @returns 存储的值或默认值
   */
  async get<T>(key: string, defaultValue: T): Promise<T> {
    try {
      if (this.preferencesStore) {
        const value = await this.preferencesStore.get(key, defaultValue);
        return value as T;
      }
      return defaultValue;
    } catch (err) {
      const error = err as BusinessError;
      console.error(`获取数据失败: ${error.code}, ${error.message}`);
      return defaultValue;
    }
  }

  /**
   * 删除数据
   * @param key 键
   */
  async delete(key: string): Promise<void> {
    try {
      if (this.preferencesStore) {
        await this.preferencesStore.delete(key);
        await this.preferencesStore.flush();
        console.info(`删除数据成功: ${key}`);
      }
    } catch (err) {
      const error = err as BusinessError;
      console.error(`删除数据失败: ${error.code}, ${error.message}`);
    }
  }

  /**
   * 清空所有数据
   */
  async clear(): Promise<void> {
    try {
      if (this.preferencesStore) {
        await this.preferencesStore.clear();
        await this.preferencesStore.flush();
        console.info('清空数据成功');
      }
    } catch (err) {
      const error = err as BusinessError;
      console.error(`清空数据失败: ${error.code}, ${error.message}`);
    }
  }

  /**
   * 保存对象（自动序列化为 JSON）
   * @param key 键
   * @param obj 对象
   */
  async putObject(key: string, obj: object): Promise<void> {
    try {
      const json = JSON.stringify(obj);
      await this.put(key, json);
    } catch (err) {
      console.error(`保存对象失败: ${err}`);
    }
  }

  /**
   * 获取对象（自动从 JSON 反序列化）
   * @param key 键
   * @param defaultValue 默认值
   * @returns 对象或默认值
   */
  async getObject<T>(key: string, defaultValue: T): Promise<T> {
    try {
      const json = await this.get(key, '');
      if (json) {
        return JSON.parse(json) as T;
      }
      return defaultValue;
    } catch (err) {
      console.error(`获取对象失败: ${err}`);
      return defaultValue;
    }
  }
}
```

#### 4. 网络请求工具
```typescript
/**
 * 网络请求工具类
 * 封装 HTTP 请求，支持拦截器、错误处理、请求取消等功能
 */
import { http } from '@kit.NetworkKit';
import { BusinessError } from '@kit.BasicServicesKit';

interface RequestConfig {
  method?: http.RequestMethod;
  header?: Record<string, string>;
  connectTimeout?: number;
  readTimeout?: number;
  extraData?: ESObject;
}

interface Response<T> {
  data: T;
  code: number;
  message: string;
}

class NetworkUtil {
  private static instance: NetworkUtil;
  private baseUrl: string = '';
  private defaultHeaders: Record<string, string> = {
    'Content-Type': 'application/json'
  };

  private constructor() {}

  static getInstance(): NetworkUtil {
    if (!NetworkUtil.instance) {
      NetworkUtil.instance = new NetworkUtil();
    }
    return NetworkUtil.instance;
  }

  /**
   * 设置基础 URL
   * @param url 基础 URL
   */
  setBaseUrl(url: string): void {
    this.baseUrl = url;
  }

  /**
   * 设置默认请求头
   * @param headers 请求头
   */
  setDefaultHeaders(headers: Record<string, string>): void {
    this.defaultHeaders = { ...this.defaultHeaders, ...headers };
  }

  /**
   * 发送 GET 请求
   * @param url 请求地址
   * @param params 查询参数
   * @param config 请求配置
   * @returns 响应数据
   */
  async get<T>(url: string, params?: Record<string, string>, config?: RequestConfig): Promise<Response<T>> {
    const queryString = params ? this.buildQueryString(params) : '';
    const fullUrl = queryString ? `${this.baseUrl}${url}?${queryString}` : `${this.baseUrl}${url}`;
    return this.request<T>(fullUrl, { method: http.RequestMethod.GET, ...config });
  }

  /**
   * 发送 POST 请求
   * @param url 请求地址
   * @param data 请求数据
   * @param config 请求配置
   * @returns 响应数据
   */
  async post<T>(url: string, data?: ESObject, config?: RequestConfig): Promise<Response<T>> {
    return this.request<T>(`${this.baseUrl}${url}`, {
      method: http.RequestMethod.POST,
      extraData: data,
      ...config
    });
  }

  /**
   * 发送 PUT 请求
   * @param url 请求地址
   * @param data 请求数据
   * @param config 请求配置
   * @returns 响应数据
   */
  async put<T>(url: string, data?: ESObject, config?: RequestConfig): Promise<Response<T>> {
    return this.request<T>(`${this.baseUrl}${url}`, {
      method: http.RequestMethod.PUT,
      extraData: data,
      ...config
    });
  }

  /**
   * 发送 DELETE 请求
   * @param url 请求地址
   * @param config 请求配置
   * @returns 响应数据
   */
  async delete<T>(url: string, config?: RequestConfig): Promise<Response<T>> {
    return this.request<T>(`${this.baseUrl}${url}`, {
      method: http.RequestMethod.DELETE,
      ...config
    });
  }

  /**
   * 发送 HTTP 请求
   * @param url 请求地址
   * @param config 请求配置
   * @returns 响应数据
   */
  private async request<T>(url: string, config: RequestConfig = {}): Promise<Response<T>> {
    const httpRequest = http.createHttp();

    try {
      const response = await httpRequest.request(url, {
        method: config.method || http.RequestMethod.GET,
        header: { ...this.defaultHeaders, ...config.header },
        connectTimeout: config.connectTimeout || 10000,
        readTimeout: config.readTimeout || 10000,
        extraData: config.extraData
      });

      if (response.responseCode === 200) {
        return {
          data: response.result as T,
          code: response.responseCode,
          message: '请求成功'
        };
      } else {
        return {
          data: {} as T,
          code: response.responseCode,
          message: '请求失败'
        };
      }
    } catch (err) {
      const error = err as BusinessError;
      console.error(`请求失败: ${error.code}, ${error.message}`);
      return {
        data: {} as T,
        code: error.code,
        message: error.message
      };
    } finally {
      httpRequest.destroy();
    }
  }

  /**
   * 构建查询字符串
   * @param params 查询参数
   * @returns 查询字符串
   */
  private buildQueryString(params: Record<string, string>): string {
    return Object.entries(params)
      .map(([key, value]) => `${encodeURIComponent(key)}=${encodeURIComponent(value)}`)
      .join('&');
  }
}
```

#### 5. 权限管理工具
```typescript
/**
 * 权限管理工具类
 * 封装权限申请、检查等功能
 */
import { abilityAccessCtrl, Permissions } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';

class PermissionUtil {
  private static instance: PermissionUtil;
  private atManager: abilityAccessCtrl.AtManager;

  private constructor() {
    this.atManager = abilityAccessCtrl.createAtManager();
  }

  static getInstance(): PermissionUtil {
    if (!PermissionUtil.instance) {
      PermissionUtil.instance = new PermissionUtil();
    }
    return PermissionUtil.instance;
  }

  /**
   * 请求权限
   * @param context 上下文
   * @param permissions 权限列表
   * @returns 是否全部授予
   */
  async requestPermissions(context: common.Context, permissions: Permissions[]): Promise<boolean> {
    try {
      const data = await this.atManager.requestPermissionsFromUser(context, permissions);
      const authResults = data.authResults;

      // 检查是否所有权限都被授予
      return authResults.every(result => result === 0);
    } catch (err) {
      const error = err as BusinessError;
      console.error(`请求权限失败: ${error.code}, ${error.message}`);
      return false;
    }
  }

  /**
   * 检查权限
   * @param context 上下文
   * @param permission 权限
   * @returns 是否已授予
   */
  async checkPermission(context: common.Context, permission: Permissions): Promise<boolean> {
    try {
      const grantStatus = await this.atManager.checkAccessToken(
        context.applicationInfo.accessTokenId,
        permission
      );
      return grantStatus === abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED;
    } catch (err) {
      const error = err as BusinessError;
      console.error(`检查权限失败: ${error.code}, ${error.message}`);
      return false;
    }
  }

  /**
   * 请求单个权限（带用户提示）
   * @param context 上下文
   * @param permission 权限
   * @param rationale 权限说明
   * @returns 是否授予
   */
  async requestSinglePermission(
    context: common.Context,
    permission: Permissions,
    rationale: string = '需要此权限以正常使用功能'
  ): Promise<boolean> {
    try {
      // 先检查是否已有权限
      const hasPermission = await this.checkPermission(context, permission);
      if (hasPermission) {
        return true;
      }

      // 申请权限
      const granted = await this.requestPermissions(context, [permission]);

      if (!granted) {
        console.warn(`用户拒绝了权限: ${permission}`);
      }

      return granted;
    } catch (err) {
      console.error(`请求权限失败: ${err}`);
      return false;
    }
  }
}
```

---

### 最佳实践示例

#### 1. MVVM 架构实现
```typescript
// Model 层 - 数据模型
interface User {
  id: number;
  name: string;
  email: string;
  avatar: string;
}

// ViewModel 层 - 视图模型
class UserViewModel {
  private users: User[] = [];

  async loadUsers(): Promise<void> {
    try {
      const response = await NetworkUtil.getInstance().get<User[]>('/users');
      this.users = response.data;
    } catch (error) {
      console.error('加载用户失败:', error);
    }
  }

  getUserById(id: number): User | undefined {
    return this.users.find(user => user.id === id);
  }
}

// View 层 - 视图组件
@Entry
@Component
struct UserListView {
  @State users: User[] = [];
  private viewModel: UserViewModel = new UserViewModel();

  async aboutToAppear(): Promise<void> {
    await this.viewModel.loadUsers();
    this.users = this.viewModel['users'];  // 简化示例，实际应使用响应式数据
  }

  build() {
    List() {
      ForEach(this.users, (user: User) => {
        ListItem() {
          UserItem({ user: user })
        }
      }, (user: User) => user.id.toString())
    }
  }
}

@Component
struct UserItem {
  @Prop user: User;

  build() {
    Row() {
      Image(this.user.avatar)
        .width(50)
        .height(50)
        .borderRadius(25)

      Text(this.user.name)
        .margin({ left: 10 })
        .fontSize(16)
    }
    .width('100%')
    .padding(10)
  }
}
```

#### 2. 依赖注入实现
```typescript
/**
 * 简单的依赖注入容器
 */
class ServiceContainer {
  private static instance: ServiceContainer;
  private services: Map<string, any> = new Map();

  private constructor() {}

  static getInstance(): ServiceContainer {
    if (!ServiceContainer.instance) {
      ServiceContainer.instance = new ServiceContainer();
    }
    return ServiceContainer.instance;
  }

  /**
   * 注册服务
   * @param name 服务名称
   * @param service 服务实例
   */
  register<T>(name: string, service: T): void {
    this.services.set(name, service);
  }

  /**
   * 获取服务
   * @param name 服务名称
   * @returns 服务实例
   */
  get<T>(name: string): T | undefined {
    return this.services.get(name);
  }

  /**
   * 注册单例服务（工厂函数）
   * @param name 服务名称
   * @param factory 工厂函数
   */
  registerSingleton<T>(name: string, factory: () => T): void {
    let instance: T | undefined;
    this.services.set(name, {
      get: () => {
        if (!instance) {
          instance = factory();
        }
        return instance;
      }
    });
  }
}

// 使用示例
const container = ServiceContainer.getInstance();

// 注册服务
container.registerSingleton('apiService', () => new ApiService());
container.registerSingleton('storageService', () => new StorageService());

// 获取服务
const apiService = container.get<ApiService>('apiService');
const storageService = container.get<StorageService>('storageService');
```

#### 3. 事件总线实现
```typescript
/**
 * 事件总线 - 用于组件间通信
 */
type EventHandler = (data?: any) => void;

class EventBus {
  private static instance: EventBus;
  private events: Map<string, EventHandler[]> = new Map();

  private constructor() {}

  static getInstance(): EventBus {
    if (!EventBus.instance) {
      EventBus.instance = new EventBus();
    }
    return EventBus.instance;
  }

  /**
   * 订阅事件
   * @param eventName 事件名称
   * @param handler 事件处理器
   */
  on(eventName: string, handler: EventHandler): void {
    if (!this.events.has(eventName)) {
      this.events.set(eventName, []);
    }
    this.events.get(eventName)!.push(handler);
  }

  /**
   * 取消订阅
   * @param eventName 事件名称
   * @param handler 事件处理器
   */
  off(eventName: string, handler: EventHandler): void {
    const handlers = this.events.get(eventName);
    if (handlers) {
      const index = handlers.indexOf(handler);
      if (index > -1) {
        handlers.splice(index, 1);
      }
    }
  }

  /**
   * 触发事件
   * @param eventName 事件名称
   * @param data 事件数据
   */
  emit(eventName: string, data?: any): void {
    const handlers = this.events.get(eventName);
    if (handlers) {
      handlers.forEach(handler => handler(data));
    }
  }

  /**
   * 订阅一次性事件
   * @param eventName 事件名称
   * @param handler 事件处理器
   */
  once(eventName: string, handler: EventHandler): void {
    const onceHandler: EventHandler = (data?: any) => {
      handler(data);
      this.off(eventName, onceHandler);
    };
    this.on(eventName, onceHandler);
  }

  /**
   * 清空所有事件监听器
   */
  clear(): void {
    this.events.clear();
  }
}

// 使用示例
const eventBus = EventBus.getInstance();

// 订阅事件
eventBus.on('user:login', (user) => {
  console.log('用户登录:', user);
});

// 触发事件
eventBus.emit('user:login', { id: 1, name: '张三' });
```

---

## 🎯 实战示例集锦

### 示例 1: 表单验证组件
```typescript
/**
 * 表单验证组件
 * 支持多种验证规则和自定义错误提示
 */
interface ValidationRule {
  validate: (value: string) => boolean;
  message: string;
}

@Entry
@Component
struct FormValidationExample {
  @State username: string = '';
  @State password: string = '';
  @State usernameError: string = '';
  @State passwordError: string = '';

  // 验证规则
  private usernameRules: ValidationRule[] = [
    { validate: (value) => value.length >= 3, message: '用户名至少3个字符' },
    { validate: (value) => value.length <= 20, message: '用户名最多20个字符' },
    { validate: (value) => /^[a-zA-Z0-9_]+$/.test(value), message: '只能包含字母、数字和下划线' }
  ];

  private passwordRules: ValidationRule[] = [
    { validate: (value) => value.length >= 6, message: '密码至少6个字符' },
    { validate: (value) => /[A-Z]/.test(value), message: '密码必须包含大写字母' },
    { validate: (value) => /[0-9]/.test(value), message: '密码必须包含数字' }
  ];

  /**
   * 验证字段
   * @param value 字段值
   * @param rules 验证规则
   * @returns 错误信息（无错误返回空字符串）
   */
  validateField(value: string, rules: ValidationRule[]): string {
    for (const rule of rules) {
      if (!rule.validate(value)) {
        return rule.message;
      }
    }
    return '';
  }

  /**
   * 验证表单
   * @returns 是否验证通过
   */
  validateForm(): boolean {
    this.usernameError = this.validateField(this.username, this.usernameRules);
    this.passwordError = this.validateField(this.password, this.passwordRules);
    return this.usernameError === '' && this.passwordError === '';
  }

  build() {
    Column() {
      Text('用户注册')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)
        .margin({ bottom: 30 })

      // 用户名输入框
      Column() {
        TextInput({ placeholder: '请输入用户名' })
          .width('100%')
          .onChange((value) => {
            this.username = value;
            this.usernameError = this.validateField(value, this.usernameRules);
          })

        if (this.usernameError) {
          Text(this.usernameError)
            .fontSize(12)
            .fontColor(Color.Red)
            .margin({ top: 5 })
        }
      }
      .width('100%')
      .margin({ bottom: 20 })

      // 密码输入框
      Column() {
        TextInput({ placeholder: '请输入密码' })
          .width('100%')
          .type(InputType.Password)
          .onChange((value) => {
            this.password = value;
            this.passwordError = this.validateField(value, this.passwordRules);
          })

        if (this.passwordError) {
          Text(this.passwordError)
            .fontSize(12)
            .fontColor(Color.Red)
            .margin({ top: 5 })
        }
      }
      .width('100%')
      .margin({ bottom: 20 })

      // 提交按钮
      Button('注册')
        .width('100%')
        .onClick(() => {
          if (this.validateForm()) {
            console.log('注册成功');
            // 执行注册逻辑
          } else {
            console.log('表单验证失败');
          }
        })
    }
    .width('100%')
    .height('100%')
    .padding(20)
    .backgroundColor('#f5f5f5')
  }
}
```

### 示例 2: 下拉刷新和加载更多
```typescript
/**
 * 下拉刷新和加载更多示例
 * 实现常见的列表交互功能
 */
@Entry
@Component
struct PullToRefreshExample {
  @State items: string[] = [];
  @State isRefreshing: boolean = false;
  @State isLoadingMore: boolean = false;
  @State page: number = 1;

  async aboutToAppear(): Promise<void> {
    await this.loadData(false);
  }

  /**
   * 加载数据
   * @param refresh 是否刷新
   */
  async loadData(refresh: boolean): Promise<void> {
    if (refresh) {
      this.isRefreshing = true;
      this.page = 1;
    } else {
      this.isLoadingMore = true;
    }

    try {
      // 模拟网络请求
      await new Promise(resolve => setTimeout(resolve, 1500));

      const newItems = Array.from({ length: 10 }, (_, i) => `项目 ${this.page * 10 + i + 1}`);

      if (refresh) {
        this.items = newItems;
      } else {
        this.items = [...this.items, ...newItems];
      }

      this.page++;
    } catch (error) {
      console.error('加载数据失败:', error);
    } finally {
      this.isRefreshing = false;
      this.isLoadingMore = false;
    }
  }

  build() {
    List() {
      ForEach(this.items, (item: string, index: number) => {
        ListItem() {
          Text(item)
            .width('100%')
            .height(80)
            .backgroundColor(Color.White)
            .textAlign(TextAlign.Center)
            .borderRadius(8)
        }
        .margin({ bottom: 10 })
      })
    }
    .width('100%')
    .height('100%')
    .padding(10)
    .backgroundColor('#f5f5f5')
    .onReachEnd(() => {
      // 加载更多
      if (!this.isLoadingMore) {
        this.loadData(false);
      }
    })
    .refresh({  // 下拉刷新
      refreshing: this.isRefreshing,
      offset: 60,
      friction: 80
    })
    .onStateChange((refreshStatus: number) => {
      if (refreshStatus === RefreshStatus.Refresh) {
        this.loadData(true);
      }
    })
  }
}
```

---

## 【画龙迎春】纯血鸿蒙来画龙！基于HarmonyOS ArkTS来操作SVG图片

Source: harmonyos-tutorial/samples/ArkTSSVGChineseLoong/README.md

# 【画龙迎春】纯血鸿蒙来画龙！基于HarmonyOS ArkTS来操作SVG图片



大家好，龙年报喜，大地回春，作为程序员，以代码之名，表达对于龙年的祝福。本节将演示如何在基于HarmonyOS ArkTS的Image组件来实现画一条中国龙，祝大家“码”上“鸿”福到！


## 效果演示

手机效果图如下：

![](screenshots/arktssvgchineseloong.gif)


B站视频：https://www.bilibili.com/video/BV1Tz421R7Rq/


## 图文介绍

见：https://developer.huawei.com/consumer/cn/forum/topic/0203143920386713714






---

## samples/ArkTSAudioCapturer/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSAudioCapturer/entry/src/main/ets/entryability/EntryAbility.ets

import { abilityAccessCtrl, AbilityConstant, Permissions, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 权限校验
    let context = this.context;
    let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
    let permissions: Array<Permissions> = ["ohos.permission.MICROPHONE"];

    // requestPermissionsFromUser会判断权限的授权状态
    atManager.requestPermissionsFromUser(context, permissions).then((data) => {
      let grantStatus: Array<number> = data.authResults;
      let length: number = grantStatus.length;
      for (let i = 0; i < length; i++) {
        if (grantStatus[i] === 0) {
          // 用户同意授权
          windowStage.loadContent('pages/Index', (err, data) => {
            if (err.code) {
              hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
              return;
            }
            hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
          });
        } else {
          // 用户拒绝授权
          return;
        }
      }
      // 授权成功
    }).catch((err: BusinessError) => {
      console.error(`requestPermissionsFromUser failed, code is ${err.code}, message is ${err.message}`);
    })
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSAudioCapturer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSAudioCapturer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSAudioCapturer/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSAudioCapturer/entry/src/main/ets/pages/Index.ets

import { audio } from '@kit.AudioKit';
import { fileIo } from '@kit.CoreFileKit';

// 录音文件缓存位置
const filePath: string = getContext().cacheDir + '/result_48000_1.pcm';

@Entry
@Component
struct Index {
  @State message: string = '待开始';
  private audioRendererInfo: audio.AudioRendererInfo = {
    usage: audio.StreamUsage.STREAM_USAGE_MOVIE,
    rendererFlags: 0
  };

  // 音频渲染器实例
  private audioRenderer: audio.AudioRenderer | null = null

  private audioStreamInfo: audio.AudioStreamInfo = {
    samplingRate: audio.AudioSamplingRate.SAMPLE_RATE_48000, // 采样率
    channels: audio.AudioChannel.CHANNEL_2, // 通道
    sampleFormat: audio.AudioSampleFormat.SAMPLE_FORMAT_S16LE, // 采样格式
    encodingType: audio.AudioEncodingType.ENCODING_TYPE_RAW // 编码格式
  };

  private audioCapturerInfo: audio.AudioCapturerInfo = {
    source: audio.SourceType.SOURCE_TYPE_MIC,
    capturerFlags: 0
  };

  // 音频采集器实例
  private audioCapturer: audio.AudioCapturer | null = null

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        Button(('开始录音'), { type: ButtonType.Capsule })
          .width(240)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            this.startCapturer(filePath)
          })

        Button(('结束录音'), { type: ButtonType.Capsule })
          .width(240)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            // 获取结果
            this.stopCapturer();
          })

        Button(('播放录音'), { type: ButtonType.Capsule })
          .width(240)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {

            this.startRenderer(filePath)
          })

        Button(('停止播放'), { type: ButtonType.Capsule })
          .width(240)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            this.stopRenderer();
          })
      }
      .width('100%')
    }
    .height('100%')
  }

  // 获取音频采集器实例
  async getAudioCapturer() {
    // 如果已经存在，直接返回
    if (this.audioCapturer) {
      return this.audioCapturer
    }
    // 创建音频采集器
    const audioCapturer = await audio.createAudioCapturer({
      streamInfo: this.audioStreamInfo,
      capturerInfo: this.audioCapturerInfo
    })
    // 保存方便下次直接获取
    this.audioCapturer = audioCapturer
    // 返回音频采集器
    return audioCapturer
  }

  // 开始录音
  async startCapturer(filePath: string) {
    // 根据 filePath 打开文件，可读可写模式，如果文件不存在自动创建
    const file = fileIo.openSync(filePath, fileIo.OpenMode.READ_WRITE | fileIo.OpenMode.CREATE)
    // 1. 获取音频采集器
    const audioCapturer = await this.getAudioCapturer()
    // 偏移值
    let bufferSize: number = 0
    // 2. 调用on('readData')方法，订阅监听音频数据读入回调
    audioCapturer.on('readData', (buffer) => {
      // 把采集的音频信息写入到打开的文件中
      fileIo.writeSync(file.fd, buffer, { offset: bufferSize, length: buffer.byteLength })
      // 累加偏移值
      bufferSize += buffer.byteLength
      // 测试用的
      this.message = bufferSize.toString();
    })
    // 3. 开始录音采集
    audioCapturer.start()
  }

  async stopCapturer(){
    // 获取音频采集器
    const audioCapturer = await this.getAudioCapturer()
    await audioCapturer.stop() // 停止采集
    audioCapturer.release() // 释放资源
    this.audioCapturer = null // 重置采集器变量

    // 测试用的
    this.message = '停止录音';
  }

  // 获取音频渲染器（播放器）
  async getAudioRenderer() {
    if (this.audioRenderer) {
      return this.audioRenderer
    }
    this.audioRenderer = await audio.createAudioRenderer({
      streamInfo: this.audioStreamInfo,
      rendererInfo: this.audioRendererInfo
    })
    return this.audioRenderer
  }

  // 播放录音
  async startRenderer(filePath: string) {
    // 根据路径打开文件
    const file = fileIo.openSync(filePath)
    // 获取文件信息，如果读取时已经超出文件大小，自动停止
    const stat = fileIo.statSync(file.fd)
    // 1. 获取音频渲染器（播放器）
    const audioRenderer = await this.getAudioRenderer()
    // 偏移值
    let bufferSize: number = 0
    // 2. 调用on('writeData')方法，订阅监听音频数据写入回调
    let writeDataCallback = (buffer: ArrayBuffer) => {
      fileIo.readSync(file.fd, buffer, { offset: bufferSize, length: buffer.byteLength })
      bufferSize += buffer.byteLength

      this.message = bufferSize.toString()
      if (bufferSize >= stat.size) {
        // 停止渲染器（播放器）
        audioRenderer.stop() // 停止
        audioRenderer.release() // 释放资源
        this.audioRenderer = null // 清理变量
      }
    }

    audioRenderer.on('writeData', writeDataCallback)

    // 3. 启动音频渲染器（播放器）
    audioRenderer.start()
  }

  // 停止播放录音
  async stopRenderer() {
    // 获取音频渲染器（播放器）
    const audioRenderer = await this.getAudioRenderer()
    if (
      audioRenderer.state === audio.AudioState.STATE_RUNNING
        ||
        audioRenderer.state === audio.AudioState.STATE_PAUSED
    ) {
      await audioRenderer.stop() // 停止
      audioRenderer.release() // 释放资源
      this.audioRenderer = null // 清理变量
    }

    this.message = '停止播放'
  }
}

---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { display, window } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';
import { BreakpointConstants } from '../constants/BreakpointConstants';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 获取窗口对象
    windowStage.getMainWindow((err: BusinessError<void>, data) => {
      let windowObj: window.Window = data;

      // 计算设备的尺寸
      this.updateBreakpoint(windowObj.getWindowProperties().windowRect.width);
      windowObj.on('windowSizeChange', (windowSize: window.Size) => {
        this.updateBreakpoint(windowSize.width);
      })

      if (err.code) {
        hilog.info(0x0000, 'testTag', '%{public}s', 'getMainWindow failed');
        return;
      }
    })

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  // 变更设备类型
  private updateBreakpoint(windowWidth: number) :void{
    let windowWidthVp = 0;
    try {
      windowWidthVp = windowWidth / display.getDefaultDisplaySync().densityPixels;
    } catch (error) {
      hilog.error(0x0000, 'testTag', 'Cause: %{public}s', JSON.stringify(error) ?? '');
    }
    let curBp: string = '';
    if (windowWidthVp < BreakpointConstants.BREAKPOINT_SCOPE[2]) {
      curBp = BreakpointConstants.BREAKPOINT_SM;
    } else if (windowWidthVp < BreakpointConstants.BREAKPOINT_SCOPE[3]) {
      curBp = BreakpointConstants.BREAKPOINT_MD;
    } else {
      curBp = BreakpointConstants.BREAKPOINT_LG;
    }
    AppStorage.setOrCreate('currentBreakpoint', curBp);
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/constants/BreakpointConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/constants/BreakpointConstants.ets

/**
 * 设备尺寸常量
 */
/**
 * 设备类型常量
 */
export class BreakpointConstants {
  /**
   * 小设备
   */
  static readonly BREAKPOINT_SM: string = 'sm';

  /**
   * 中设备
   */
  static readonly BREAKPOINT_MD: string = 'md';

  /**
   * 大设备
   */
  static readonly BREAKPOINT_LG: string = 'lg';

  /**
   * 屏幕宽度范围
   */
  static readonly BREAKPOINT_SCOPE: number[] = [0, 320, 600, 840];
}

---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/view/ActionList.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/view/ActionList.ets

/**
 * 操作栏
 */
@Component
export struct ActionList {

  build() {
    Column(){
        Text('操作栏')
          .fontSize('24fp')
    }
    .height('52vp')
    .borderColor(Color.Black)
    .borderWidth('2vp')
  }
}

---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/view/CenterPart.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/view/CenterPart.ets

/**
 * 中部图片显示区
 */
@Component
export struct CenterPart {

  build() {
    Row() {
      Column() {
        Text('中部图片显示区')
          .fontSize('24fp')
      }
    }
    .height('100%')
    .width('100%')
    .borderColor(Color.Black)
    .borderWidth('2vp')
  }
}

---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/view/TopBar.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/view/TopBar.ets

import { BreakpointConstants } from '../constants/BreakpointConstants';
import { ActionList } from './ActionList';


/**
 * 顶部区域
 */
@Preview
@Component
export struct TopBar {
  @StorageLink('currentBreakpoint') currentBp: string = BreakpointConstants.BREAKPOINT_MD;

  build() {
    Flex({
      direction: FlexDirection.Row,
      alignItems: ItemAlign.Center,
    }) {
      Column() {
        Flex({
          justifyContent: FlexAlign.SpaceBetween,
          direction: FlexDirection.Row,
          alignItems: ItemAlign.Stretch
        }) {
          Row() {
            Column() {
              Text('应用标题')
                .fontSize('24fp')
            }
            .alignItems(HorizontalAlign.Start)
          }

          Row() {
            // 仅在大设备上显示操作按钮
            if (this.currentBp === BreakpointConstants.BREAKPOINT_LG) {
              ActionList();
            }
          }
        }
      }
      .borderColor(Color.Black)
      .borderWidth('2vp')
    }
    .height('52vp')
  }
}

---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/view/PreviewList.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/view/PreviewList.ets

/**
 * 图片预览列表
 */
@Component
export struct PreviewList {

  build() {
    Column() {
      Text('图片预览列表')
        .fontSize('24fp')
    }
    .height('47vp')
    .borderColor(Color.Black)
    .borderWidth('2vp')
  }
}

---

## samples/ArkTSMultiPictureUI/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPictureUI/entry/src/main/ets/pages/Index.ets

import { BreakpointConstants } from '../constants/BreakpointConstants';
import { ActionList } from '../view/ActionList';
import { CenterPart } from '../view/CenterPart';
import { PreviewList } from '../view/PreviewList';
import { TopBar } from '../view/TopBar';

/**
 * 图片查看器主页
 */
@Entry
@Component
struct Index {
  @StorageLink('currentBreakpoint') currentBreakpoint: string = BreakpointConstants.BREAKPOINT_MD

  build() {
    Column() {
      Flex({
        direction: FlexDirection.Column,
        alignItems: ItemAlign.Center
      }) {
        // 顶部区域
        TopBar()

        // 中部图片显示区
        CenterPart( )

        // 图片预览列表
        PreviewList( )

        // 非大设备，则显示底部操作栏
        if (this.currentBreakpoint !== BreakpointConstants.BREAKPOINT_LG) {
          ActionList()
        }
      }
    }
    .height('100%')
    .width('100%')
  }
}

---

## samples/ArkTSUserGrantMicrophone/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSUserGrantMicrophone/entry/src/main/ets/entryability/EntryAbility.ets

import { abilityAccessCtrl, AbilityConstant, Permissions, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 权限校验
    let context = this.context;
    let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
    let permissions: Array<Permissions> = ["ohos.permission.MICROPHONE"];

    // requestPermissionsFromUser会判断权限的授权状态
    atManager.requestPermissionsFromUser(context, permissions).then((data) => {
      let grantStatus: Array<number> = data.authResults;
      let length: number = grantStatus.length;
      for (let i = 0; i < length; i++) {
        if (grantStatus[i] === 0) {
          // 用户同意授权
          windowStage.loadContent('pages/Index', (err, data) => {
            if (err.code) {
              hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
              return;
            }
            hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
          });
        } else {
          // 用户拒绝授权
          return;
        }
      }
      // 授权成功
    }).catch((err: BusinessError) => {
      console.error(`requestPermissionsFromUser failed, code is ${err.code}, message is ${err.message}`);
    })
  }


  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSUserGrantMicrophone/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSUserGrantMicrophone/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSUserGrantMicrophone/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSUserGrantMicrophone/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('HelloWorld')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
    }
    .height('100%')
    .width('100%')
  }
}

---

## samples/ArkTSAVPlayer/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSAVPlayer/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSAVPlayer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSAVPlayer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSAVPlayer/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSAVPlayer/entry/src/main/ets/pages/Index.ets

import { common } from '@kit.AbilityKit';
import { media } from '@kit.MediaKit';

export default interface Song {
  id: number; // 索引
  title: string; // 标题
  author: string // 作者
  path: string; // 文件路径
}

// 资源放在resources目录下的rawfile文件夹中
const SONGS: Song[] = [
  {
    id: 1,
    title: '东方红',
    author: '李有源',
    path: '东方红.mp3'
  },
  {
    id: 2,
    title: '花海',
    author: '周杰伦',
    path: '花海.mp3'
  },
  {
    id: 3,
    title: '铁血丹心',
    author: '罗文',
    path: '铁血丹心.mp3'
  }]

@Entry
@Component
struct Index {
  private avPlayer: media.AVPlayer | null = null;
  private isPlaying: boolean = false;
  @State playerState: string = '暂停'
  @State selectedSong: Song = {
    id: -1,
    title: '',
    author: '',
    path: ''
  };

  async onPageShow() {
    // 创建avPlayer实例对象
    this.avPlayer = await media.createAVPlayer();
    // 创建状态机变化回调函数
    this.setAVPlayerCallback();
    console.info('播放器准备完成')
  }

  build() {
    Column() {
      Row() {
        Row() {
          Text('音乐播放器')
            .fontColor(Color.White).fontSize(32)
        }.margin({ left: 20 })
      }.backgroundColor(Color.Green).height('8%').width('100%')

      Column() {
        List() {
          ForEach(SONGS, (song: Song) => {
            ListItem() {
              Row() {
                Button({ type: ButtonType.Normal }) {
                  Row() {
                    Text(song.id + '')
                      .fontSize(32)
                    Column() {
                      Text(song.title).fontSize(20).fontWeight(700)
                      Text(song.author).fontSize(14)
                    }.alignItems(HorizontalAlign.Start)
                    .margin({ left: 20 })
                  }.justifyContent(FlexAlign.Start)
                  .width('90%')
                }
                .backgroundColor(Color.White)
                .width("100%")
                .height(50)
                .margin({ top: 10 })
                .onClick(() => {
                  this.playerState = '暂停';
                  this.isPlaying = true;
                  this.onPageShow();
                  this.changeSong(song);
                  this.selectedSong = song;
                })
              }
            }
          })
        }.width('100%')
      }.height('84%')

      Row() {
        Row() {
          if (this.selectedSong.id == -1) {
            Text('点击歌曲开始播放')
              .fontSize(20).fontColor(Color.White)
          } else {
            Column() {
              Text(this.selectedSong.title)
                .fontSize(20).fontColor(Color.White)
            }.width('70%').alignItems(HorizontalAlign.Start)

            Column() {
              Button({ type: ButtonType.Normal, stateEffect: true }) {
                Text(this.playerState)
                  .fontSize(20).fontColor(Color.White)
              }
              .borderRadius(8)
              .height(26)
              .width(70)
              .backgroundColor(Color.Orange)
              .onClick(() => {
                if (this.avPlayer !== null && this.isPlaying == true) {
                  this.avPlayer.pause()
                  this.playerState = '继续'
                  this.isPlaying = false
                } else {
                  this.avPlayer?.play()
                  this.playerState = '暂停'
                  this.isPlaying = true
                }
              })
            }.width('20%')
          }
        }.width('99%').margin({ left: 15 })
      }.backgroundColor(Color.Green).height('8%').width('100%')
    }.height('100%').width('100%')
  }

  // 以下为使用资源管理接口获取打包在HAP内的媒体资源文件并通过fdSrc属性进行播放
  async changeSong(song: Song) {
    if (this.avPlayer !== null) {
      this.avPlayer?.reset()
      // 创建状态机变化回调函数
      this.setAVPlayerCallback();
      // 通过UIAbilityContext的resourceManager成员的getRawFd接口获取媒体资源播放地址
      // 返回类型为{fd,offset,length},fd为HAP包fd地址，offset为媒体资源偏移量，length为播放长度
      let context = getContext(this) as common.UIAbilityContext;
      let fileDescriptor = await context.resourceManager.getRawFd(song.path);
      // 为fdSrc赋值触发initialized状态机上报
      this.avPlayer.fdSrc = fileDescriptor;
    }
  }

  setAVPlayerCallback() {
    if (this.avPlayer !== null) {
      this.avPlayer.on('error', (err) => {
        console.error(`播放器发生错误，错误码：${err.code}, 错误信息：${err.message}`);
        this.avPlayer?.reset();
      });

      this.avPlayer.on('stateChange', async (state, reason) => {
        switch (state) {
          case 'initialized':
            console.info('资源初始化完成');
            this.avPlayer?.prepare();
            break;
          case 'prepared':
            console.info('资源准备完成');
            this.avPlayer?.play();
            break;
          case 'completed':
            console.info('播放完成');
            this.avPlayer?.stop();
            break;
        }
      });
    }
  }
}


---

## samples/ArkTSMusicPlayer/entry/src/main/ets/viewmodel/SongListData.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/viewmodel/SongListData.ets


import router from '@ohos.router'
import { RouterUrlConstants } from '../common/constants/RouterUrlConstants'
const optionList : OptionItem[] = [
  { image: $r('app.media.ic_collect'), text: $r('app.string.collect') },
  { image: $r('app.media.ic_download'), text: $r('app.string.download') },
  { image: $r('app.media.ic_comments'), text: $r('app.string.comment'), action: () => {
    router.pushUrl({
      url: RouterUrlConstants.MUSIC_COMMENT
    }, router.RouterMode.Single)
  }},
  { image: $r('app.media.ic_share'), text: $r('app.string.share') }
]

type OptionItem = {
  image: Resource;
  text: Resource;
  action?: () => void;
}

export { optionList, OptionItem }

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/viewmodel/SongDataSource.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/viewmodel/SongDataSource.ets


import { SongItem } from '../common/bean/SongItem';

export class SongDataSource implements IDataSource {
  private dataArray: SongItem[] = [];
  private listeners: DataChangeListener[] = [];

  constructor(element: SongItem[]) {
    for (let index = 0; index < element.length; index++) {
      this.dataArray.push(element[index]);
    }
  }

  public totalCount(): number {
    return this.dataArray.length;
  }

  public getData(index: number): SongItem {
    return this.dataArray[index];
  }

  public addData(index: number, data: SongItem): void {
    this.dataArray.splice(index, 0, data);
    this.notifyDataAdd(index);
  }

  public pushData(data: SongItem): void {
    this.dataArray.push(data);
    this.notifyDataAdd(this.dataArray.length - 1);
  }

  registerDataChangeListener(listener: DataChangeListener): void {
    if (this.listeners.indexOf(listener) < 0) {
      this.listeners.push(listener);
    }
  }

  unregisterDataChangeListener(listener: DataChangeListener): void {
    const pos = this.listeners.indexOf(listener);
    if (pos >= 0) {
      this.listeners.splice(pos, 1);
    }
  }

  notifyDataReload(): void {
    this.listeners.forEach(listener => {
      listener.onDataReloaded();
    });
  }

  notifyDataAdd(index: number): void {
    this.listeners.forEach(listener => {
      listener.onDataAdd(index);
    })
  }

  notifyDataChange(index: number): void {
    this.listeners.forEach(listener => {
      listener.onDataChange(index);
    })
  }

  notifyDataDelete(index: number): void {
    this.listeners.forEach(listener => {
      listener.onDataDelete(index);
    })
  }

  notifyDataMove(from: number, to: number): void {
    this.listeners.forEach(listener => {
      listener.onDataMove(from, to);
    })
  }
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/components/Header.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/components/Header.ets

import router from '@ohos.router';
import { StyleConstants } from '../common/constants/StyleConstants';
import { HeaderConstants } from '../common/constants/HeaderConstants';
import { BreakpointType } from '../common/media/BreakpointSystem';

@Preview
@Component
export struct Header {
  @Link currentBreakpoint: string;

  build() {
    Row() {
      // 返回按钮
      Image($r('app.media.ic_back'))
        .width($r('app.float.icon_width'))
        .height($r('app.float.icon_height'))
        .margin({ left: $r('app.float.icon_margin') })
        .onClick(() => {
          router.back()
        })

      // 播放器名称
      Text($r('app.string.play_list'))
        .fontSize(new BreakpointType({
          sm: $r('app.float.header_font_sm'),
          md: $r('app.float.header_font_md'),
          lg: $r('app.float.header_font_lg')
        }).getValue(this.currentBreakpoint))
        .fontWeight(HeaderConstants.TITLE_FONT_WEIGHT)
        .fontColor($r('app.color.title_color'))
        .opacity($r('app.float.title_opacity'))
        .letterSpacing(HeaderConstants.LETTER_SPACING)
        .padding({ left: $r('app.float.title_padding_left') })

      Blank()

      // 菜单
      Image($r('app.media.ic_more'))
        .width($r('app.float.icon_width'))
        .height($r('app.float.icon_height'))
        .margin({ right: $r('app.float.icon_margin') })
        //.bindMenu(this.getMenu())
    }
    .width(StyleConstants.FULL_WIDTH)
    .height($r('app.float.title_bar_height'))
    .zIndex(HeaderConstants.Z_INDEX)
  }

}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/components/AlbumComponent.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/components/AlbumComponent.ets


import { BreakpointConstants } from '../common/constants/BreakpointConstants';
import { ContentConstants } from '../common/constants/ContentConstants';
import { GridConstants } from '../common/constants/GridConstants';
import { StyleConstants } from '../common/constants/StyleConstants';
import { BreakpointType } from '../common/media/BreakpointSystem';
import { OptionItem, optionList } from '../viewmodel/SongListData';

@Component
export struct AlbumComponent {
  @State imgHeight: number = 0;
  @Link currentBreakpoint: string;

  @Builder
  CoverImage() {
    Stack({ alignContent: Alignment.BottomStart }) {
      Image($r('app.media.ic_album'))
        .width(StyleConstants.FULL_WIDTH)
        .aspectRatio(ContentConstants.ASPECT_RATIO_ALBUM_COVER)
        .borderRadius($r('app.float.album_cover_border_radius'))
        .onAreaChange((oldArea: Area, newArea: Area) => {
          this.imgHeight = newArea.height as number;
        })
      Text($r('app.string.collection_num'))
        .letterSpacing(ContentConstants.LETTER_SPACING)
        .fontColor(Color.White)
        .fontSize(new BreakpointType({
          sm: $r('app.float.collection_font_sm'),
          md: $r('app.float.collection_font_md'),
          lg: $r('app.float.collection_font_lg')
        }).getValue(this.currentBreakpoint))
        .translate({
          x: StyleConstants.TRANSLATE_X,
          y: StyleConstants.TRANSLATE_Y
        })
    }
    .width(StyleConstants.FULL_WIDTH)
    .height(StyleConstants.FULL_HEIGHT)
    .aspectRatio(ContentConstants.ASPECT_RATIO_ALBUM_COVER)
  }

  @Builder
  CoverIntroduction() {
    Column() {
      Text($r('app.string.list_name'))
        .opacity($r('app.float.album_name_opacity'))
        .fontWeight(ContentConstants.ALBUM_FONT_WEIGHT)
        .fontColor($r('app.color.album_name_introduction'))
        .fontSize(new BreakpointType({
          sm: $r('app.float.list_font_sm'),
          md: $r('app.float.list_font_md'),
          lg: $r('app.float.list_font_lg')
        }).getValue(this.currentBreakpoint))
        .margin({ bottom: $r('app.float.album_name_margin') })

      Text($r('app.string.playlist_Introduction'))
        .opacity($r('app.float.introduction_opacity'))
        .width(StyleConstants.FULL_WIDTH)
        .fontWeight(ContentConstants.INTRODUCTION_FONT_WEIGHT)
        .fontColor($r('app.color.album_name_introduction'))
        .fontSize(new BreakpointType({
          sm: $r('app.float.introduction_font_sm'),
          md: $r('app.float.introduction_font_md'),
          lg: $r('app.float.introduction_font_lg')
        }).getValue(this.currentBreakpoint))
    }
    .width(StyleConstants.FULL_WIDTH)
    .height(this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ?
    this.imgHeight : $r('app.float.introduction_height'))
    .alignItems(HorizontalAlign.Start)
    .justifyContent(FlexAlign.Center)
    .padding({
      left: this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ? $r('app.float.introduction_padding') : 0
    })
    .margin({
      top: this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ? 0 : $r('app.float.introduction_margin_top'),
      bottom: this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ?
        0 : $r('app.float.introduction_margin_bottom')
    })
  }

  @Builder
  CoverOptions() {
    Row() {
      ForEach(optionList, (item: OptionItem) => {
        Column({ space: ContentConstants.COVER_OPTION_SPACE }) {
          Image(item.image)
            .height($r('app.float.option_image_size'))
            .width($r('app.float.option_image_size'))
          Text(item.text)
            .fontColor($r('app.color.album_name_introduction'))
            .fontSize(new BreakpointType({
              sm: $r('app.float.option_font_sm'),
              md: $r('app.float.option_font_md'),
              lg: $r('app.float.option_font_lg')
            }).getValue(this.currentBreakpoint))
        }
        .onClick(item.action)
      }, (item, index) => index + JSON.stringify(item))
    }
    .height($r('app.float.option_area_height'))
    .width(StyleConstants.FULL_WIDTH)
    .padding({
      left: $r('app.float.options_padding'),
      right: $r('app.float.options_padding')
    })
    .justifyContent(FlexAlign.SpaceBetween)
  }

  build() {
    Column() {
      GridRow() {
        GridCol({
          span: { sm: GridConstants.SPAN_FOUR, md: GridConstants.SPAN_TWELVE, lg: GridConstants.SPAN_TWELVE }
        }) {
          this.CoverImage()
        }

        GridCol({
          span: { sm: GridConstants.SPAN_EIGHT, md: GridConstants.SPAN_TWELVE, lg: GridConstants.SPAN_TWELVE }
        }) {
          this.CoverIntroduction()
        }

        GridCol({
          span: { sm: GridConstants.SPAN_TWELVE, md: GridConstants.SPAN_TWELVE, lg: GridConstants.SPAN_TWELVE }
        }) {
          this.CoverOptions()
        }
        .padding({
          top: this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ? $r('app.float.option_margin') : 0,
          bottom: this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ? $r('app.float.option_margin') : 0
        })
      }
      .padding({
        top: this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ?
        $r('app.float.cover_padding_top_sm') : $r('app.float.cover_padding_top_other'),
        left: new BreakpointType({
          sm: $r('app.float.album_padding_sm'),
          md: $r('app.float.album_padding_md'),
          lg: $r('app.float.album_padding_lg')
        }).getValue(this.currentBreakpoint),
        right: new BreakpointType({
          sm: $r('app.float.album_padding_sm'),
          md: $r('app.float.album_padding_md'),
          lg: $r('app.float.album_padding_lg')
        }).getValue(this.currentBreakpoint)
      })
    }
    .margin({
      left: new BreakpointType({
        sm: $r('app.float.cover_margin_sm'),
        md: $r('app.float.cover_margin_md'),
        lg: $r('app.float.cover_margin_lg')
      }).getValue(this.currentBreakpoint),
      right: new BreakpointType({
        sm: $r('app.float.cover_margin_sm'),
        md: $r('app.float.cover_margin_md'),
        lg: $r('app.float.cover_margin_lg')
      }).getValue(this.currentBreakpoint)
    })
  }
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/components/Player.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/components/Player.ets

import { SongItem } from '../common/bean/SongItem';
import { PlayerConstants } from '../common/constants/PlayerConstants';
import { StyleConstants } from '../common/constants/StyleConstants';
import { BreakpointType } from '../common/media/BreakpointSystem';
import { MusicList } from '../common/media/MusicList';

@Preview
@Component
export struct Player {
  @StorageProp('selectIndex') selectIndex: number = 0;
  @StorageLink('isPlay') isPlay: boolean = false;
  songList: SongItem[] = MusicList;
  @Link currentBreakpoint: string;

  build() {
    Row() {
      Row() {
        Image(this.songList[this.selectIndex]?.label)
          .height($r('app.float.cover_height'))
          .width($r('app.float.cover_width'))
          .borderRadius($r('app.float.label_border_radius'))
          .margin({ right: $r('app.float.cover_margin') })
          .rotate({ angle: this.isPlay ? PlayerConstants.ROTATE : 0 })
          .animation({
            duration: PlayerConstants.ANIMATION_DURATION,
            iterations: PlayerConstants.ITERATIONS,
            curve: Curve.Linear
          })
        Column() {
          Text(this.songList[this.selectIndex].title)
            .fontColor($r('app.color.song_name'))
            .fontSize(new BreakpointType({
              sm: $r('app.float.song_title_sm'),
              md: $r('app.float.song_title_md'),
              lg: $r('app.float.song_title_lg')
            }).getValue(this.currentBreakpoint))
          Row() {
            Image($r('app.media.ic_vip'))
              .height($r('app.float.vip_icon_height'))
              .width($r('app.float.vip_icon_width'))
              .margin({ right: $r('app.float.vip_icon_margin') })
            Text(this.songList[this.selectIndex].singer)
              .fontColor($r('app.color.singer'))
              .fontSize(new BreakpointType({
                sm: $r('app.float.singer_title_sm'),
                md: $r('app.float.singer_title_md'),
                lg: $r('app.float.singer_title_lg')
              }).getValue(this.currentBreakpoint))
              .opacity($r('app.float.singer_opacity'))
          }
        }
        .alignItems(HorizontalAlign.Start)
      }
      .layoutWeight(PlayerConstants.LAYOUT_WEIGHT_PLAYER_CONTROL)

      Blank()

      Row() {
        Image($r('app.media.ic_previous'))
          .height($r('app.float.control_icon_height'))
          .width($r('app.float.control_icon_width'))
          .margin({ right: $r('app.float.control_icon_margin') })
          .displayPriority(PlayerConstants.DISPLAY_PRIORITY_TWO)

        Image(this.isPlay ? $r('app.media.ic_play') : $r('app.media.ic_pause'))
          .height($r('app.float.control_icon_height'))
          .width($r('app.float.control_icon_width'))
          .displayPriority(PlayerConstants.DISPLAY_PRIORITY_THREE)

        Image($r('app.media.ic_next'))
          .height($r('app.float.control_icon_height'))
          .width($r('app.float.control_icon_width'))
          .margin({
            right: $r('app.float.control_icon_margin'),
            left: $r('app.float.control_icon_margin')
          })
          .displayPriority(PlayerConstants.DISPLAY_PRIORITY_TWO)
        Image($r('app.media.ic_music_list'))
          .height($r('app.float.control_icon_height'))
          .width($r('app.float.control_icon_width'))
          .displayPriority(PlayerConstants.DISPLAY_PRIORITY_ONE)
      }
      .width(new BreakpointType({
        sm: $r('app.float.play_width_sm'),
        md: $r('app.float.play_width_sm'),
        lg: $r('app.float.play_width_lg')
      }).getValue(this.currentBreakpoint))
      .justifyContent(FlexAlign.End)
    }
    .width(StyleConstants.FULL_WIDTH)
    .height($r('app.float.player_area_height'))
    .backgroundColor($r('app.color.player_background'))
    .padding({
      left: $r('app.float.player_padding'),
      right: $r('app.float.player_padding')
    })
    .position({
      x: 0,
      y: StyleConstants.FULL_HEIGHT
    })
    .translate({
      x: 0,
      y: StyleConstants.TRANSLATE_PLAYER_Y
    })
  }
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/components/PlayList.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/components/PlayList.ets


import { SongItem } from '../common/bean/SongItem';
import { BreakpointConstants } from '../common/constants/BreakpointConstants';
import { ContentConstants } from '../common/constants/ContentConstants';
import { StyleConstants } from '../common/constants/StyleConstants';
import { BreakpointType } from '../common/media/BreakpointSystem';
import { MusicList } from '../common/media/MusicList';
import { SongDataSource } from '../viewmodel/SongDataSource';
@Component
export struct PlayList {
  @Link currentBreakpoint: string;
  songList: SongItem[] = MusicList;

  @Builder
  PlayAll() {
    Row() {
      Image($r('app.media.ic_play_all'))
        .height($r('app.float.play_all_icon_size'))
        .width($r('app.float.play_all_icon_size'))
      Text($r('app.string.play_all', this.songList.length))
        .maxLines(ContentConstants.PLAY_ALL_MAX_LINES)
        .padding({ left: $r('app.float.play_all_text_padding') })
        .fontColor(Color.Black)
        .fontSize(new BreakpointType({
          sm: $r('app.float.play_font_sm'),
          md: $r('app.float.play_font_md'),
          lg: $r('app.float.play_font_lg')
        }).getValue(this.currentBreakpoint))
      Blank()
      Image($r('app.media.ic_order_play'))
        .width($r('app.float.order_icon_size'))
        .height($r('app.float.order_icon_size'))
        .margin({ right: $r('app.float.order_icon_margin') })
      Image($r('app.media.ic_sort_list'))
        .height($r('app.float.order_icon_size'))
        .width($r('app.float.order_icon_size'))
    }
    .height($r('app.float.play_all_area_height'))
    .width(StyleConstants.FULL_WIDTH)
    .backgroundColor(Color.White)
    .padding({
      left: $r('app.float.play_all_area_padding'),
      right: $r('app.float.play_all_area_padding')
    })
    .borderRadius({
      topRight: $r('app.float.play_all_border_radius'),
      topLeft: $r('app.float.play_all_border_radius')
    })
    .position({ x: 0, y: 0 })
  }

  @Builder
  SongItem(item: SongItem, index: number) {
    Row() {
      Column() {
        Text(item.title)
          .fontColor(Color.Black)
          .fontSize(new BreakpointType({
            sm: $r('app.float.item_font_sm'),
            md: $r('app.float.item_font_md'),
            lg: $r('app.float.item_font_lg')
          }).getValue(this.currentBreakpoint))
          .margin({ bottom: $r('app.float.list_item_title_margin') })
        Row() {
          Image(item.mark)
            .width($r('app.float.list_item_image_size'))
            .height($r('app.float.list_item_image_size'))
            .margin({ right: $r('app.float.list_item_image_margin') })
          Text(item.singer)
            .opacity($r('app.float.singer_opacity'))
            .fontColor(Color.Black)
            .fontSize(new BreakpointType({
              sm: $r('app.float.singer_title_sm'),
              md: $r('app.float.singer_title_md'),
              lg: $r('app.float.singer_title_lg')
            }).getValue(this.currentBreakpoint))
        }
      }
      .alignItems(HorizontalAlign.Start)

      Blank()
      Image($r('app.media.ic_list_more'))
        .height($r('app.float.order_icon_size'))
        .width($r('app.float.order_icon_size'))
    }
    .height($r('app.float.list_item_height'))
    .width(StyleConstants.FULL_WIDTH)
  }

  build() {
    Column() {
      // 播放全部
      this.PlayAll()

      // 歌单列表
      List() {
        LazyForEach(new SongDataSource(this.songList), (item: SongItem, index: number) => {
          ListItem() {
            Column() {
              this.SongItem(item, index)
            }
            .padding({
              left: $r('app.float.list_item_padding'),
              right: $r('app.float.list_item_padding')
            })
          }
        }, (item, index) => JSON.stringify(item) + index)
      }
      .width(StyleConstants.FULL_WIDTH)
      .backgroundColor(Color.White)
      .margin({ top: $r('app.float.list_area_margin_top') })
      .lanes(this.currentBreakpoint === BreakpointConstants.BREAKPOINT_LG ?
      ContentConstants.COL_TWO : ContentConstants.COL_ONE)
      .layoutWeight(1)
      .divider({
        color: $r('app.color.list_divider'),
        strokeWidth: $r('app.float.stroke_width'),
        startMargin: $r('app.float.list_item_padding'),
        endMargin: $r('app.float.list_item_padding')
      })
    }
    .padding({
      top: this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ? 0 : $r('app.float.list_area_padding_top'),
      bottom: $r('app.float.list_area_padding_bottom')
    })
  }
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/components/Content.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/components/Content.ets

import { GridConstants } from '../common/constants/GridConstants';
import { StyleConstants } from '../common/constants/StyleConstants';
import { AlbumCover } from './AlbumCover';
import { PlayList } from './PlayList';

@Preview
@Component
export struct Content {
  @Link currentBreakpoint: string;

  build() {
    GridRow() {

      // 封面
      GridCol({ span: { sm: GridConstants.SPAN_TWELVE, md: GridConstants.SPAN_SIX, lg: GridConstants.SPAN_FOUR } }) {
        AlbumCover({ currentBreakpoint: $currentBreakpoint })
      }
      .backgroundColor($r('app.color.album_background'))

      // 歌曲列表
      GridCol({ span: { sm: GridConstants.SPAN_TWELVE, md: GridConstants.SPAN_SIX, lg: GridConstants.SPAN_EIGHT } }) {
        PlayList({ currentBreakpoint: $currentBreakpoint })
      }
      .borderRadius($r('app.float.playlist_border_radius'))
    }
    .height(StyleConstants.FULL_HEIGHT)
    .onBreakpointChange((breakpoints: string) => {
      this.currentBreakpoint = breakpoints;
    })
  }
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/components/AlbumCover.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/components/AlbumCover.ets

import { BreakpointConstants } from '../common/constants/BreakpointConstants';
import { StyleConstants } from '../common/constants/StyleConstants';
import { AlbumComponent } from './AlbumComponent';

@Component
export struct AlbumCover {
  @Link currentBreakpoint: string;

  build() {
    if (this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM) {
      AlbumComponent({ currentBreakpoint: $currentBreakpoint })
    } else {
      AlbumComponent({ currentBreakpoint: $currentBreakpoint })
        .height(StyleConstants.FULL_HEIGHT)
    }
  }
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/bean/MenuData.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/bean/MenuData.ets

/**
 * Menu item info.
 */
export class MenuData {

  /**
   * Indicates menu title.
   */
  value: string;

  /**
   * Indicates menu action.
   */
  action: () => void;
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/bean/SongItem.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/bean/SongItem.ets


/**
 * Music information entity class.
 */
export class SongItem {
  /**
   * Primary key ID.
   */
  id: number;

  /**
   * Music name.
   */
  title: string;

  /**
   * Music author name.
   */
  singer: string;

  /**
   * Music logo information.
   */
  mark: Resource;

  /**
   * Music avatar information.
   */
  label: Resource;

  /**
   * Music file path information.
   */
  src: string;

  /**
   * Index of the current music list.
   */
  index: number;
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/ContentConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/ContentConstants.ets


/**
 * Constants for main content area.
 */
export class ContentConstants {
  /**
   * The max lines of play all area is 1.
   */
  static readonly PLAY_ALL_MAX_LINES: number = 1;

  /**
   * The font size of the singer is smaller.
   */
  static readonly SINGER_FONT_REDUCE: number = 4;

  /**
   * The font size of the album name is larger.
   */
  static readonly ALBUM_FONT_PLUS: number = 2;

  /**
   * The font size of the introduction is smaller.
   */
  static readonly INTRODUCTION_FONT_REDUCE: number = 2;

  /**
   * Width of the list item divider.
   */
  static readonly DIVIDER_STROKE_WIDTH: number = 0.5;

  /**
   * Aspect ratio of the album cover image.
   */
  static readonly ASPECT_RATIO_ALBUM_COVER: number = 1;

  /**
   * Letter spacing.
   */
  static readonly LETTER_SPACING: number = 1;

  /**
   * Font weight of the album title.
   */
  static readonly ALBUM_FONT_WEIGHT: number = 500;

  /**
   * Font weight of the album introduction.
   */
  static readonly INTRODUCTION_FONT_WEIGHT: number = 400;

  /**
   * Space between cover options.
   */
  static readonly COVER_OPTION_SPACE: number = 4;

  /**
   * Value of lanes is 2.
   */
  static readonly COL_TWO: number = 2;

  /**
   * Value of lanes is 1.
   */
  static readonly COL_ONE: number = 1;
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/GridConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/GridConstants.ets


/**
 * Constants for Grid components.
 */
export class GridConstants {
  /**
   * Current component width: 4 grids.
   */
  static readonly SPAN_FOUR: number = 4;

  /**
   * Current component width: 6 grids.
   */
  static readonly SPAN_SIX: number = 6;

  /**
   * Current component width: 8 grids.
   */
  static readonly SPAN_EIGHT: number = 8;

  /**
   * Current component width: 12 grids.
   */
  static readonly SPAN_TWELVE: number = 12;
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/PlayerConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/PlayerConstants.ets


/**
 * Constants for player area.
 */
export class PlayerConstants {
  /**
   * The font size of the singer is smaller.
   */
  static readonly FONT_REDUCE: number = 4;

  /**
   * The layout weight of player control.
   */
  static readonly LAYOUT_WEIGHT_PLAYER_CONTROL: number = 1;

  /**
   * The display priority is 1.
   */
  static readonly DISPLAY_PRIORITY_ONE: number = 1;

  /**
   * The display priority is 2.
   */
  static readonly DISPLAY_PRIORITY_TWO: number = 2;

  /**
   * The display priority is 3.
   */
  static readonly DISPLAY_PRIORITY_THREE: number = 3;

  /**
   * The rotate is 360.
   */
  static readonly ROTATE: number = 360;

  /**
   * The animation duration is 3000.
   */
  static readonly ANIMATION_DURATION: number = 3000;

  /**
   * The value of iterations.
   */
  static readonly ITERATIONS: number = -1;
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/HeaderConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/HeaderConstants.ets


/**
 * Constants for header area.
 */
export class HeaderConstants {
  /**
   * The font size of the title is larger.
   */
  static readonly TITLE_FONT_SIZE_PLUS: number = 4;

  /**
   * The font weight of the title.
   */
  static readonly TITLE_FONT_WEIGHT: number = 500;

  /**
   * Letter spacing of the title.
   */
  static readonly LETTER_SPACING: number = 2;

  /**
   * Title bar z-index.
   */
  static readonly Z_INDEX: number = 2;

  /**
   * SystemCapability that indicates audio device management capability.
   */
  static readonly SYSCAP_ETHERNET: string = 'SystemCapability.Multimedia.Audio.Device';

  /**
   * SystemCapability name: audio device service.
   */
  static readonly AUDIO_DEVICE_SERVICE: string = '音频设备管理';

  /**
   * Toast duration is 2000ms.
   */
  static readonly TOAST_DURATION: number = 2000;
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/RouterUrlConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/RouterUrlConstants.ets


/**
 * Routing information on the music page.
 */
export class RouterUrlConstants {
  /**
   * Playback page.
   */
  static readonly MUSIC_PLAY: string = '@bundle:com.huawei.music.musichome/musicPlay/ets/pages/PlayPage';

  /**
   * Music list page.
   */
  static readonly MUSIC_LIST: string = '@bundle:com.huawei.music.musichome/musicList/ets/pages/Index';

  /**
   * Music review page.
   */
  static readonly MUSIC_COMMENT: string = '@bundle:com.huawei.music.musichome/musicComment/ets/pages/Index';
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/SongConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/SongConstants.ets


/**
 * Constants for common components.
 */
export class SongConstants {
  /**
   * Song list index add 1.
   */
  static readonly ADD_INDEX_ONE: number = 1;

  /**
   * Song list index add 2.
   */
  static readonly ADD_INDEX_TWO: number = 2;

  /**
   * Song list index add 3.
   */
  static readonly ADD_INDEX_THREE: number = 3;

  /**
   * The slice start is 0.
   */
  static readonly SLICE_START_ZERO: number = 0;

  /**
   * The slice end is 3.
   */
  static readonly SLICE_END_THREE: number = 3;

  /**
   * The slice index is 1.
   */
  static readonly SLICE_INDEX_ONE: number = 1;

  /**
   * The slice index is 2.
   */
  static readonly SLICE_INDEX_TWO: number = 2;

  /**
   * The slice index is 4.
   */
  static readonly SLICE_INDEX_FOUR: number = 4;

  /**
   * The form id is no exit.
   */
  static readonly ID_NO_EXIT: number = 16501001;

  /**
   * The duration of page transition.
   */
  static readonly TRANSITION_DURATION: number = 500;
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/BreakpointConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/BreakpointConstants.ets

/**
 * Constants for breakpoint.
 */
export class BreakpointConstants {
  /**
   * Breakpoints that represent small device types.
   */
  static readonly BREAKPOINT_SM: string = 'sm';

  /**
   * Breakpoints that represent middle device types.
   */
  static readonly BREAKPOINT_MD: string = 'md';

  /**
   * Breakpoints that represent large device types.
   */
  static readonly BREAKPOINT_LG: string = 'lg';

  /**
   * The break point value.
   */
  static readonly BREAKPOINT_VALUE: Array<string> = ['320vp', '600vp', '840vp'];

  /**
   * The number of columns for SM device.
   */
  static readonly COLUMN_SM: number = 4;

  /**
   * The number of columns for MD device.
   */
  static readonly COLUMN_MD: number = 8;

  /**
   * The number of columns for LG device.
   */
  static readonly COLUMN_LG: number = 12;

  /**
   * The number of gutter X for device.
   */
  static readonly GUTTER_X: number = 12;

  /**
   * The number of span for SM device.
   */
  static readonly SPAN_SM: number = 4;

  /**
   * The number of span for MD device.
   */
  static readonly SPAN_MD: number = 6;

  /**
   * The number of span for LG device.
   */
  static readonly SPAN_LG: number = 8;

  /**
   * The number of offset for MD device.
   */
  static readonly OFFSET_MD: number = 1;

  /**
   * The number of offset for LG device.
   */
  static readonly OFFSET_LG: number = 2;

  /**
   * Current breakpoints that to query the device types.
   */
  static readonly CURRENT_BREAKPOINT: string = 'currentBreakpoint';

  /**
   * Font size of the small device type.
   */
  static readonly FONT_SIZE_SM: number = 14;

  /**
   * Font size of the middle device type.
   */
  static readonly FONT_SIZE_MD: number = 16;

  /**
   * Font size of the large device type.
   */
  static readonly FONT_SIZE_LG: number = 18;

  /**
   * Cover margin of the small device type.
   */
  static readonly COVER_MARGIN_SM: number = 10;

  /**
   * Cover margin of the middle device type.
   */
  static readonly COVER_MARGIN_MD: number = 30;

  /**
   * Cover margin of the large device type.
   */
  static readonly COVER_MARGIN_LG: number = 40;

  /**
   * Range of the small device width.
   */
  static readonly RANGE_SM: string = '(320vp<=width<600vp)';

  /**
   * Range of the middle device width.
   */
  static readonly RANGE_MD: string = '(600vp<=width<840vp)';

  /**
   * Range of the large device width.
   */
  static readonly RANGE_LG: string = '(840vp<=width)';
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/StyleConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/constants/StyleConstants.ets


/**
 * Constants for common style.
 */
export class StyleConstants {
  /**
   * Component width percentage: 100%.
   */
  static readonly FULL_WIDTH: string = '100%';

  /**
   * Component height percentage: 100%.
   */
  static readonly FULL_HEIGHT: string = '100%';

  /**
   * Translate value of the collection text.
   */
  static readonly TRANSLATE_X: number = 10;

  /**
   * Translate value of the number of playbacks.
   */
  static readonly TRANSLATE_Y: string = '-100%';

  /**
   * Translate value of the player area on the bottom.
   */
  static readonly TRANSLATE_PLAYER_Y: string = '-48vp';
}

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/media/BreakpointSystem.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/media/BreakpointSystem.ets

import mediaQuery from '@ohos.mediaquery';
import { BreakpointConstants } from '../constants/BreakpointConstants';

export class BreakpointSystem {
  private currentBreakpoint: string = BreakpointConstants.BREAKPOINT_SM;
  private smListener: mediaQuery.MediaQueryListener;
  private mdListener: mediaQuery.MediaQueryListener;
  private lgListener: mediaQuery.MediaQueryListener;

  private updateCurrentBreakpoint(breakpoint: string): void {
    if (this.currentBreakpoint !== breakpoint) {
      this.currentBreakpoint = breakpoint;
      AppStorage.SetOrCreate<string>(BreakpointConstants.CURRENT_BREAKPOINT, this.currentBreakpoint);
    }
  }

  private isBreakpointSM = (mediaQueryResult: mediaQuery.MediaQueryResult): void => {
    if (mediaQueryResult.matches) {
      this.updateCurrentBreakpoint(BreakpointConstants.BREAKPOINT_SM);
    }
  }
  private isBreakpointMD = (mediaQueryResult: mediaQuery.MediaQueryResult): void => {
    if (mediaQueryResult.matches) {
      this.updateCurrentBreakpoint(BreakpointConstants.BREAKPOINT_MD);
    }
  }
  private isBreakpointLG = (mediaQueryResult: mediaQuery.MediaQueryResult): void => {
    if (mediaQueryResult.matches) {
      this.updateCurrentBreakpoint(BreakpointConstants.BREAKPOINT_LG);
    }
  }

  public register(): void {
    this.smListener = mediaQuery.matchMediaSync(BreakpointConstants.RANGE_SM);
    this.smListener.on('change', this.isBreakpointSM);
    this.mdListener = mediaQuery.matchMediaSync(BreakpointConstants.RANGE_MD);
    this.mdListener.on('change', this.isBreakpointMD);
    this.lgListener = mediaQuery.matchMediaSync(BreakpointConstants.RANGE_LG);
    this.lgListener.on('change', this.isBreakpointLG);
  }

  public unregister(): void {
    this.smListener.off('change', this.isBreakpointSM);
    this.mdListener.off('change', this.isBreakpointMD);
    this.lgListener.off('change', this.isBreakpointLG);
  }
}


declare interface BreakPointTypeOption<T> {
  sm?: T,
  md?: T,
  lg?: T,
  xl?: T,
  xxl?: T
}

export class BreakpointType<T> {
  options: BreakPointTypeOption<T>;

  constructor(option: BreakPointTypeOption<T>) {
    this.options = option;
  }

  getValue(currentPoint: string): T {
    return this.options[currentPoint] as T;
  }
}


---

## samples/ArkTSMusicPlayer/entry/src/main/ets/common/media/MusicList.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/common/media/MusicList.ets


const MusicList = [
  { id: 1, title: '不知道', singer: '小碗你好', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar1'), src: 'boisterous.wav', index:0},
  { id: 2, title: '歌名你好', singer: '张三-你好我好都好', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar2'), src: 'dynamic.wav', index:1 },
  { id: 3, title: '还是歌名', singer: '不知道你是谁', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar16'), src: 'dynamic.wav', index:2 },
  { id: 4, title: 'AIUHGVNHK', singer: 'Gwyu-Hjjiyabn', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar4'), src: 'boisterous.wav' , index:3},
  { id: 5, title: '可可不喜欢', singer: '名佚', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar5'), src: 'dynamic.wav', index:4 },
  { id: 6, title: '我是UOUYGBJ', singer: '我是小树', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar6'), src: 'boisterous.wav' , index:5},
  { id: 7, title: '好好学习', singer: '全村最帅', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar17'), src: 'dynamic.wav', index:6 },
  { id: 8, title: '安心安心', singer: '小安安', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar8'), src: 'boisterous.wav', index:7 },
  { id: 9, title: 'HBNJGHJHB', singer: '我是小树', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar9'), src: 'dynamic.wav', index:8 },
  { id: 10, title: '天天向上', singer: '靓仔', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar10'), src: 'boisterous.wav', index:9 },
  { id: 11, title: 'Notebook', singer: '小安安', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar11'), src: 'dynamic.wav', index:10},
  { id: 12, title: '我是谁', singer: '小碗你好', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar12'), src: 'boisterous.wav', index:11 },
  { id: 13, title: '你好吗', singer: '张三-你好我好都好', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar13'), src: 'dynamic.wav', index:12 },
  { id: 14, title: '你在哪', singer: '不知道你是谁', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar14'), src: 'boisterous.wav', index:13 },
  { id: 15, title: 'lovely', singer: 'Gwyu-Hjjiyabn', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar15'), src: 'dynamic.wav', index:14 },
  { id: 16, title: '谢谢你', singer: '名佚', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar16'), src: 'boisterous.wav', index:15 },
  { id: 17, title: '我是靓仔', singer: '我是小树', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar17'), src: 'dynamic.wav', index:16 },
  { id: 18, title: '听我说', singer: '全村最帅', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar1'), src: 'boisterous.wav', index:17 },
  { id: 19, title: '没什么大不了', singer: '小安安', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar2'), src: 'dynamic.wav', index:18 },
  { id: 20, title: '其实也一样', singer: '我是小树', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar13'), src: 'boisterous.wav', index:19 },
  { id: 21, title: '想明白', singer: '小安安', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar4'), src: 'dynamic.wav', index:20 },
  { id: 22, title: '你懂的', singer: '小安安', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar5'), src: 'boisterous.wav', index:21 },
  { id: 23, title: '谁了解', singer: '小安安', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar6'), src: 'dynamic.wav', index:22 },
  { id: 24, title: '白天', singer: '小安安', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar17'), src: 'dynamic.wav', index:23 },
  { id: 25, title: '黑夜', singer: 'Gwyu-Hjjiyabn', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar15'), src: 'boisterous.wav', index:24 },
  { id: 26, title: '春夏秋冬', singer: '名佚', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar16'), src: 'boisterous.wav', index:25 },
  { id: 27, title: '一年四季', singer: '我是小树', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar17'), src: 'dynamic.wav', index:26 },
  { id: 28, title: '朝雪', singer: '全村最帅', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar12'), src: 'dynamic.wav', index:27 },
  { id: 29, title: '暮色', singer: '小安安', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar13'), src: 'boisterous.wav', index:28 },
  { id: 30, title: '天下', singer: '我是小树', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar14'), src: 'dynamic.wav', index:29 },
  { id: 31, title: '勇敢', singer: '小安安', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar15'), src: 'boisterous.wav', index:30 },
  { id: 32, title: '安明', singer: '小安安', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar16'), src: 'boisterous.wav', index:31 },
  { id: 33, title: '心安', singer: '小安安', mark: $r('app.media.ic_vip'),
    label: $r('app.media.ic_avatar17'), src: 'dynamic.wav', index:32 },
  { id: 34, title: '无归', singer: '小安安', mark: $r('app.media.ic_sq'),
    label: $r('app.media.ic_avatar11'), src: 'boisterous.wav', index:33 }
]

export { MusicList }

---

## samples/ArkTSMusicPlayer/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSMusicPlayer/entry/src/main/ets/pages/Index.ets

import { BreakpointConstants } from '../common/constants/BreakpointConstants';

import { StyleConstants } from '../common/constants/StyleConstants';

import { Content } from '../components/Content';
import { Header } from '../components/Header';
import { Player } from '../components/Player';

@Entry
@Component
struct Index {

  @State currentBreakpoint: string = BreakpointConstants.BREAKPOINT_SM;
  build() {
    Stack({ alignContent: Alignment.Top }) {
      // 头部
      Header({ currentBreakpoint: $currentBreakpoint })

      // 中部
      Content({ currentBreakpoint: $currentBreakpoint })

      // 底部
      Player({ currentBreakpoint: $currentBreakpoint })
    }
    .width(StyleConstants.FULL_WIDTH)
  }

}

---

## samples/ArkTSUserGrant/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSUserGrant/entry/src/main/ets/entryability/EntryAbility.ets

import { abilityAccessCtrl, AbilityConstant, Permissions, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 权限校验
    let context = this.context;
    let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
    let permissions: Array<Permissions> = ["ohos.permission.CAMERA"];

    // requestPermissionsFromUser会判断权限的授权状态
    atManager.requestPermissionsFromUser(context, permissions).then((data) => {
      let grantStatus: Array<number> = data.authResults;
      let length: number = grantStatus.length;
      for (let i = 0; i < length; i++) {
        if (grantStatus[i] === 0) {
          // 用户同意授权
          windowStage.loadContent('pages/Index', (err, data) => {
            if (err.code) {
              hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
              return;
            }
            hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
          });
        } else {
          // 用户拒绝授权
          return;
        }
      }
      // 授权成功
    }).catch((err: BusinessError) => {
      console.error(`requestPermissionsFromUser failed, code is ${err.code}, message is ${err.message}`);
    })
  }


  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSUserGrant/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSUserGrant/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSUserGrant/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSUserGrant/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkTSSpeechAICaption/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSSpeechAICaption/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    try {
      this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    } catch (err) {
      hilog.error(DOMAIN, 'testTag', 'Failed to set colorMode. Cause: %{public}s', JSON.stringify(err));
    }
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSSpeechAICaption/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSSpeechAICaption/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSSpeechAICaption/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSSpeechAICaption/entry/src/main/ets/pages/Index.ets

// 引入相关的类
import { AICaptionComponent, AICaptionController, AICaptionOptions, AudioData } from '@kit.SpeechKit'
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct Index {
  private videoController: VideoController = new VideoController()
  @State isCaptionShown: boolean = false;
  private captionController: AICaptionController = new AICaptionController()
  private captionOptions: AICaptionOptions = {
    initialOpacity: 0.8,
    onPrepared: () => {
      console.info('onPrepared')
    },
    onError: (error) => {
      console.error(`onError code: ${error.code}, message: ${error.message}`)
    }
  }

  build() {
    Column({ space: 16 }) {
      // 视频播放器组件
      Video({
        src: $r('app.media.huawei_mate_40'),
        controller: this.videoController
      })
        .width('100%')
        .height(300)

      // AI 字幕组件
      AICaptionComponent({
        isShown: this.isCaptionShown,
        controller: this.captionController,
        options: this.captionOptions
      })
        .width('100%')
        .height(80)

      // 字幕控制按钮
      Button(this.isCaptionShown ? '关闭字幕' : '开启字幕')
        .onClick(() => {
          this.isCaptionShown = !this.isCaptionShown

          // 开启字幕的时候，就开启识别
          if (this.isCaptionShown) {
            this.startCaptionRecognition()
          }
        })
    }
    .height('100%')
    .width('100%')
    .padding(20)
  }

  // 开启字幕识别
  private startCaptionRecognition() {
    // 读取本地视频的音频数据
    this.getUIContext().getHostContext()?.resourceManager.getMediaContent($r('app.media.huawei_mate_40').id, 0)
      .then((value: Uint8Array) => {
        const audioBuffer = value

        // 分块传入音频数据，避免一次性加载卡顿
        // 每块大小为1280字节
        const bufferSize = 1280
        let offset = 0
        const totalLength = audioBuffer.byteLength

        while (offset < totalLength) {
          const endOffset = Math.min(offset + bufferSize, totalLength)
          const audioChunk = audioBuffer.slice(offset, endOffset)

          // 封装为AudioData，传入控制器
          const audioData: AudioData = {
            data: audioChunk
          }

          this.captionController.writeAudio(audioData)

          offset = endOffset
        }
      })
      .catch((error: BusinessError) => {
        console.error(`getMediaContent failed, code: ${error.code}, message: ${error.message}`)
      })
  }
}

---

## samples/ArkTSDistributedData/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSDistributedData/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSDistributedData/entry/src/main/ets/database/AccountData.ets

Source: harmonyos-tutorial/samples/ArkTSDistributedData/entry/src/main/ets/database/AccountData.ets

export default interface AccountData {
  id: number;
  accountType: number;
  typeText: string;
  amount: number;
}

---

## samples/ArkTSDistributedData/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSDistributedData/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSDistributedData/entry/src/main/ets/common/DistributedDataUtil.ets

Source: harmonyos-tutorial/samples/ArkTSDistributedData/entry/src/main/ets/common/DistributedDataUtil.ets

// 导入模块
import { distributedKVStore } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';
import AccountData from '../database/AccountData';

class DistributedDataUtil {

  private KEY: string = 'fruit';
  private kvManager: distributedKVStore.KVManager | undefined = undefined;
  private kvStore: distributedKVStore.SingleKVStore | undefined = undefined;

  public initKVManager(){
    try {
      let context: Context = getContext(this) as Context;

      const kvManagerConfig: distributedKVStore.KVManagerConfig = {
        context: context,
        bundleName: 'com.waylau.hmos.arktsdistributeddata'
      };

      this.kvManager = distributedKVStore.createKVManager(kvManagerConfig);
    } catch (e) {
      console.log("An unexpected error occurred. Error:" + JSON.stringify(e));
    }
  }

  public initKVStore() {
    try {
      const options: distributedKVStore.Options = {
        createIfMissing: true,
        encrypt: false,
        backup: false,
        autoSync: false,
        // kvStoreType不填时，默认创建多设备协同数据库
        kvStoreType: distributedKVStore.KVStoreType.SINGLE_VERSION,
        // 多设备协同数据库：kvStoreType: distributedKVStore.KVStoreType.DEVICE_COLLABORATION,
        securityLevel: distributedKVStore.SecurityLevel.S1
      };


      if (this.kvManager !== undefined) {
        this.kvManager.getKVStore<distributedKVStore.SingleKVStore>('storeId', options,
          (err, store: distributedKVStore.SingleKVStore) => {
            if (err) {
              console.error(`Failed to get KVStore: Code:${err.code},message:${err.message}`);
              return;
            }
            console.info('Succeeded in getting KVStore.');
            this.kvStore = store;
            // 请确保获取到键值数据库实例后，再进行相关数据操作
          });
      }

    } catch (e) {
      let error = e as BusinessError;
      console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
    }
  }

  public addData() {
    try {
      if (this.kvStore !== undefined) {

        let newAccount: AccountData = { id: 1, accountType: 0, typeText: '苹果', amount: 0 };
        this.kvStore.put(this.KEY, JSON.stringify(newAccount), (err) => {
          if (err !== undefined) {
            console.error(`Failed to put data. Code:${err.code},message:${err.message}`);
            return;
          }
          console.info('Succeeded in putting data.');
        });
      }
    } catch (e) {
      let error = e as BusinessError;
      console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
    }
  }

  async queryData(){
    let result: string= '';

    if (this.kvStore !== undefined) {
      await this.kvStore.get(this.KEY).then((data) => {
        result = data.toString();
        console.info(`Succeeded in getting value`);
      }).catch((err: BusinessError) => {
        console.error(`Failed to get preferences, Cause:` + err);
      });
    }

    return result;
  }

  public deleteData() {
    try {
      if (this.kvStore !== undefined) {
        this.kvStore.delete(this.KEY, (err:BusinessError) => {
          if (err !== undefined) {
            console.error(`Failed to delete data. Code:${err.code},message:${err.message}`);
            return;
          }
          console.info('Succeeded in deleting data.');
        });
      }

    } catch (e) {
      console.log("An unexpected error occurred. Error:" + JSON.stringify(e));
    }
  }

  public updateData() {
    try {
      if (this.kvStore !== undefined) {
        let newAccount: AccountData = { id: 1, accountType: 1, typeText: '栗子', amount: 1 };
        this.kvStore.put(this.KEY, JSON.stringify(newAccount), (err:BusinessError) => {
          if (err !== undefined) {
            console.error(`Failed to put data. Code:${err.code},message:${err.message}`);
            return;
          }
          console.info('Succeeded in putting data.');
        });
      }
    } catch (e) {
      let error = e as BusinessError;
      console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
    }
  }


}

export default new DistributedDataUtil();

---

## samples/ArkTSDistributedData/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSDistributedData/entry/src/main/ets/pages/Index.ets

// 导入DistributedDataUtil
import DistributedDataUtil from '../common/DistributedDataUtil'

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'

  aboutToAppear() {
    DistributedDataUtil.initKVManager();
    DistributedDataUtil.initKVStore();
  }

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        // 增加
        Button(('增加'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            DistributedDataUtil.addData()
          })

        // 查询
        Button(('查询'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            // 获取结果
            DistributedDataUtil.queryData().then((resultData: string) => {
              this.message = resultData;
            });
          })

        // 修改
        Button(('修改'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            DistributedDataUtil.updateData()
          })

        // 删除
        Button(('删除'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            DistributedDataUtil.deleteData()
          })
      }
      .width('100%')
    }
    .height('100%')
  }

}

---

## samples/ArkTSSubWindow/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSSubWindow/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });

    // 给Index页面传递windowStage
    AppStorage.setOrCreate('windowStage', windowStage);
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSSubWindow/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSSubWindow/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSSubWindow/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSSubWindow/entry/src/main/ets/pages/Index.ets

import { window } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

let windowStage_: window.WindowStage | undefined = undefined;
let sub_windowClass: window.Window | undefined = undefined;
@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
        Button(){
          Text('创建子窗口')
            .fontSize(24)
            .fontWeight(FontWeight.Normal)
        }.width(220).height(68)
        .margin({left:10, top:60})
        .onClick(() => {
          this.createSubWindow()
        })
        Button(){
          Text('销毁子窗口')
            .fontSize(24)
            .fontWeight(FontWeight.Normal)
        }.width(220).height(68)
        .margin({left:10, top:60})
        .onClick(() => {
          this.destroySubWindow()
        })
      }
      .width('100%')
    }
    .height('100%')
  }

  private createSubWindow(){
    // 获取windowStage
    windowStage_ = AppStorage.get('windowStage');
    // 1.创建应用子窗口。
    if (windowStage_ == null) {
      console.error('Failed to create the subwindow. Cause: windowStage_ is null');
    }
    else {
      windowStage_.createSubWindow("mySubWindow", (err: BusinessError, data) => {
        let errCode: number = err.code;
        if (errCode) {
          console.error('Failed to create the subwindow. Cause: ' + JSON.stringify(err));
          return;
        }
        sub_windowClass = data;
        console.info('Succeeded in creating the subwindow. Data: ' + JSON.stringify(data));
        // 2.子窗口创建成功后，设置子窗口的位置、大小及相关属性等。
        sub_windowClass.moveWindowTo(100, 140, (err: BusinessError) => {
          let errCode: number = err.code;
          if (errCode) {
            console.error('Failed to move the window. Cause:' + JSON.stringify(err));
            return;
          }
          console.info('Succeeded in moving the window.');
        });
        sub_windowClass.resize(1000, 500, (err: BusinessError) => {
          let errCode: number = err.code;
          if (errCode) {
            console.error('Failed to change the window size. Cause:' + JSON.stringify(err));
            return;
          }
          console.info('Succeeded in changing the window size.');
        });
        // 3.为子窗口加载对应的目标页面。
        sub_windowClass.setUIContent("pages/SubWindowPage", (err: BusinessError) => {
          let errCode: number = err.code;
          if (errCode) {
            console.error('Failed to load the content. Cause:' + JSON.stringify(err));
            return;
          }
          console.info('Succeeded in loading the content.');
          // 3.显示子窗口。
          (sub_windowClass as window.Window).showWindow((err: BusinessError) => {
            let errCode: number = err.code;
            if (errCode) {
              console.error('Failed to show the window. Cause: ' + JSON.stringify(err));
              return;
            }
            console.info('Succeeded in showing the window.');
          });
        });
      })
    }
  }

  private destroySubWindow(){
    // 4.销毁子窗口。当不再需要子窗口时，可根据具体实现逻辑，使用destroy对其进行销毁。
    (sub_windowClass as window.Window).destroyWindow((err: BusinessError) => {
      let errCode: number = err.code;
      if (errCode) {
        console.error('Failed to destroy the window. Cause: ' + JSON.stringify(err));
        return;
      }
      console.info('Succeeded in destroying the window.');
    });
  }
}

---

## samples/ArkTSSubWindow/entry/src/main/ets/pages/SubWindowPage.ets

Source: harmonyos-tutorial/samples/ArkTSSubWindow/entry/src/main/ets/pages/SubWindowPage.ets

@Entry
@Component
struct SubWindowPage {
  @State message: string = 'Sub Window Page';

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('SubWindowPageHelloWorld')
        .fontSize(30)
        .fontWeight(FontWeight.Bold)
        .width('100%')
        .height('100%')
        .backgroundColor(Color.Orange)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
    }
    .height('100%')
    .width('100%')
  }
}

---

## samples/ArkTSWindow/entry/src/main/ets/pages/Second.ets

Source: harmonyos-tutorial/samples/ArkTSWindow/entry/src/main/ets/pages/Second.ets

@Entry
@Component
struct Second {
  @State message: string = 'Hello World'

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkTSWindow/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSWindow/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
      }
      .width('100%')
    }
    .height('100%')
  }

}

---

## samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want, abilityAccessCtrl, Permissions } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    try {
      this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    } catch (err) {
      hilog.error(DOMAIN, 'testTag', 'Failed to set colorMode. Cause: %{public}s', JSON.stringify(err));
    }
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 权限校验
    let context = this.context;
    let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
    let permissions: Array<Permissions> = ["ohos.permission.MICROPHONE"];

    // 判定权限的授权状态
    atManager.requestPermissionsFromUser(context, permissions)
      .then((data) => {
        let grantStatus: Array<number> = data.authResults;
        let length: number = grantStatus.length;
        for (let i=0; i<length; i++) {
          if (grantStatus[i] === 0) {
            windowStage.loadContent('pages/Index', (err) => {
              if (err.code) {
                hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
                return;
              }
              hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
            });
          } else {
            return;
          }
        }
      })
      .catch((error: BusinessError) => {
        console.error(`requestPermissionsFromUser failed, code: ${error.code}, message: ${error.message}`)
      })
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/service/AudioCapturer.ets

Source: harmonyos-tutorial/samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/service/AudioCapturer.ets

import { audio } from "@kit.AudioKit";

const TAG = 'AudioCapturer'

interface AudioStreamInfo {
  samplingRate: audio.AudioSamplingRate;
  channels: audio.AudioChannel;
  sampleFormat: audio.AudioSampleFormat;
  encodingType: audio.AudioEncodingType;
}

interface AudioCapturerInfo {
  source: audio.SourceType;
  capturerFlags: number;
}

interface AudioCapturerOptions {
  streamInfo: AudioStreamInfo;
  capturerInfo: AudioCapturerInfo
}

export default class AudioCapturer {
  // 采集工具对象
  private mAudioCapturer: audio.AudioCapturer | null = null;
  // 音频流信息
  private audioStreamInfo: AudioStreamInfo = {
    samplingRate: audio.AudioSamplingRate.SAMPLE_RATE_16000,
    channels: audio.AudioChannel.CHANNEL_1,
    sampleFormat: audio.AudioSampleFormat.SAMPLE_FORMAT_S16LE,
    encodingType: audio.AudioEncodingType.ENCODING_TYPE_RAW
  }
  // 音频采集器信息
  private audioCapturerInfo: AudioCapturerInfo = {
    source: audio.SourceType.SOURCE_TYPE_MIC,
    capturerFlags: 0
  }
  // 音频采集器选型信息
  private audioCapturerOptions: AudioCapturerOptions = {
    streamInfo: this.audioStreamInfo,
    capturerInfo: this.audioCapturerInfo
  }

  // 初始化采集器
  public async init(dataCallback: (data: ArrayBuffer) => void) {
    if (null != this.mAudioCapturer) {
      console.error(TAG, 'AudioCapturer already init');
      return
    }

    try {
      this.mAudioCapturer = await audio.createAudioCapturer(this.audioCapturerOptions)
    } catch (error) {
      console.error(TAG, `AudioCapturer init failed, code: ${error.code}, message: ${error.message}`);
    }

  }
}

---

## samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSCoreSpeechSpeechRecognizer/entry/src/main/ets/pages/Index.ets

// 引用相关的类
import { speechRecognizer } from '@kit.CoreSpeechKit'
import { BusinessError } from '@kit.BasicServicesKit'
import { PromptAction } from '@kit.ArkUI'
import AudioCapturer from '../service/AudioCapturer'

let asrEngine: speechRecognizer.SpeechRecognitionEngine

const TAG = 'AsrDemo'

@Entry
@Component
struct Index {
  @State outputText: string = ''
  @State uiContext: UIContext = this.getUIContext()
  @State promptAction: PromptAction = this.uiContext.getPromptAction()
  @State sessionId: string = '123456'
  private mAudioCapturer: AudioCapturer = new AudioCapturer()
  @State sessionId2: string = '1234567'

  build() {
    Column() {
      // 展示生成的文本
      Text(this.outputText)
        .width('90%')
        .backgroundColor('#d2d2d2')
        .height(100)
        .padding(20)
        .align(Alignment.TopStart)
        .borderRadius(5)
        .margin({ top: 20 })

      // 操作按钮
      Button('创建引擎')
        .fontSize(20)
        .type(ButtonType.Capsule)
        .width('80%')
        .height(50)
        .margin(10)
        .onClick(() => {
          try {
            // 创建引擎
            this.createEngine()

            // 设置延迟
            this.sleep(500).then(() => {
              // 设置监听器
              this.setListener()

              // 提示信息
              this.promptAction.showToast({
                message: 'CreateEngine succeeded!',
                duration: 2000
              })
            })
          } catch (error) {
            this.outputText = `Failed to CreateEngine, message: ${error.message}`

            // 提示信息
            this.promptAction.showToast({
              message: 'CreateEngine failed!',
              duration: 2000
            })
          }

        })

      Button('开始录音')
        .fontSize(20)
        .type(ButtonType.Capsule)
        .width('80%')
        .height(50)
        .margin(10)
        .onClick(() => {
          // 执行录音
          this.startRecording()
            .then(() => {
              this.promptAction.showToast({
                message: 'start Recording',
                duration: 2000
              })
            })
            .catch((error: BusinessError) => {
              this.promptAction.showToast({
                message: `start Recording failed, code: ${error.code}, message: ${error.message}}`,
                duration: 2000
              })
            })
        })

      Button('关闭录音')
        .fontSize(20)
        .type(ButtonType.Capsule)
        .width('80%')
        .height(50)
        .margin(10)
        .onClick(() => {
          try {
            asrEngine.shutdown()

            this.promptAction.showToast({
              message: `shutdown succeeded!`,
              duration: 2000
            })
          } catch (error) {
            this.promptAction.showToast({
              message: `shutdown failed, code: ${error.code}, message: ${error.message}}`,
              duration: 2000
            })
          }
        })
    }
    .height('100%')
    .width('100%')
  }

  // 调用speechRecognizer.createEngine接口创建引擎
  private createEngine() {
    let extraParams: Record<string, Object> = {
      'locate': 'CN',
      'recognizerMode': 'short'
    }

    let createEngineParams: speechRecognizer.CreateEngineParams = {
      language: 'zh-CN',
      online: 1,
      extraParams: extraParams
    }

    speechRecognizer.createEngine(createEngineParams)
      .then((value: speechRecognizer.SpeechRecognitionEngine) => {
        asrEngine = value
        console.info(TAG, 'succeeded in creating engine.')
      })
      .catch((error: BusinessError) => {
        console.error(TAG, `failed in creating engine, code: ${error.code}, message: ${error.message}`)
      })
  }

  // 延迟
  private sleep(ms: number): Promise<void> {
    return new Promise(resolve => setTimeout(resolve, ms))
  }

  // 设置监听器
  private setListener() {
    let listener: speechRecognizer.RecognitionListener = {
      // 开始
      onStart:(sessionId: string, eventMessage: string) => {
        console.info(TAG, `onStart sessionId: ${sessionId}, eventMessage: ${eventMessage}`)
        this.outputText = ''
      },
      // 事件
      onEvent(sessionId: string, eventCode: number, eventMessage: string) {
        console.info(TAG, `onEvent sessionId: ${sessionId}, eventCode: ${eventCode}, eventMessage: ${eventMessage}`)
      },
      // 识别结果
      onResult:(sessionId: string, result: speechRecognizer.SpeechRecognitionResult) => {
        console.info(TAG, `onResult sessionId: ${sessionId}, result: ${JSON.stringify(result)}`)
        this.outputText = result.result
      },
      // 识别完成
      onComplete(sessionId: string, eventMessage: string) {
        console.info(TAG, `onComplete sessionId: ${sessionId}, eventMessage: ${eventMessage}`)
      },
      // 错误
      onError(sessionId: string, eventCode: number, eventMessage: string) {
        console.info(TAG, `onError sessionId: ${sessionId}, eventCode: ${eventCode}, eventMessage: ${eventMessage}`)
      }
    }

    // 在引擎上设置监听器
    asrEngine.setListener(listener)
  }

  // 执行录音
  private async startRecording() {
    // 设置监听器
    this.setListener()

    let audioInfo: speechRecognizer.AudioInfo = {
      audioType: 'pcm',
      sampleRate: 16000,
      soundChannel: 1,
      sampleBit: 16
    }

    let extraParams: Record<string, Object> = {
      'recognitionMode': 0,
      'vadBegin': 2000,
      'vadEnd': 3000,
      'maxAudioDuration': 20000
    }

    let startParams: speechRecognizer.StartParams = {
      sessionId: this.sessionId,
      audioInfo: audioInfo,
      extraParams: extraParams
    }

    // 调用asrEngine.startListening接口执行录音
    try {
      asrEngine.startListening(startParams)
    } catch (error) {
      this.outputText = `Message: ${error.message}`
    }

    // 获取音频数据
    let data: ArrayBuffer;
    this.mAudioCapturer.init((dataBuffer: ArrayBuffer) => {
      data = dataBuffer
      let uint8Array: Uint8Array = new Uint8Array(data)

      // 将音频流写入引擎
      try {
        asrEngine.writeAudio(this.sessionId2, uint8Array)
      } catch (error) {
        this.outputText = `Message: ${error.message}`
      }
    })

  }
}

---

## samples/ArkTSPagesRouter/entry/src/main/ets/pages/Second.ets

Source: harmonyos-tutorial/samples/ArkTSPagesRouter/entry/src/main/ets/pages/Second.ets

// 导入router模块
import router from '@ohos.router';

@Entry
@Component
struct Second {
  @State message: string = 'Second页面'

  // 获取Index页面传递过来的自定义路由参数
  @State src: string = router.getParams()?.['src'];

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        // 显示传参的内容
        Text(this.src)
          .fontSize(30)

        // 添加按钮，触发返回
        Button('返回')
          .fontSize(40)
          .onClick(() => {
            router.back();
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkTSPagesRouter/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSPagesRouter/entry/src/main/ets/pages/Index.ets

// 导入router模块
import router from '@ohos.router';

@Entry
@Component
struct Index {
  // 修改变量值以示区分
  @State message: string = 'Index页面'

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        // 添加按钮，触发跳转
        Button('跳转')
          .fontSize(40)
          .onClick(() => {
            router.pushUrl({
              url: 'pages/Second',
              params: {
                src: 'Index页面传来的数据',
              }
            });
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkTSOpenLink/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSOpenLink/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    try {
      this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    } catch (err) {
      hilog.error(DOMAIN, 'testTag', 'Failed to set colorMode. Cause: %{public}s', JSON.stringify(err));
    }
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSOpenLink/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSOpenLink/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSOpenLink/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSOpenLink/entry/src/main/ets/pages/Index.ets

// 引入相关的类
import { BusinessError } from '@kit.BasicServicesKit'
import { common, OpenLinkOptions } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';

const TAG: string = 'ArkTSOpenLink'
const DOMAIN: number = 0x000000

@Entry
@Component
struct Index {
  @State url: string = 'https://github.com/waylau/harmonyos-tutorial';

  build() {
    Column() {
      Text(this.url)
        .id('HelloWorld')
        .fontSize(23)
        .fontWeight(FontWeight.Bold)

      Button('访问链接')
        .type(ButtonType.Capsule)
        .fontColor(Color.White)
        .margin(10)
        .onClick(() => {
          // 获取上下文
          let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
          let link: string = this.url;
          let openLinkOptions: OpenLinkOptions = {
            // 将appLinkingOnly参数设为false或者不传，
            // 若有App Linking匹配的应用，则直接打开目标应用。
            // 若无App Linking匹配的应用，则尝试以浏览器打开链接的方式打开应用
            appLinkingOnly: false,
            parameters: {}
          };

          try {
            context.openLink(
              link,
              openLinkOptions,
              (err, result) => {
                hilog.error(DOMAIN, TAG, `openLink callback error.code: ${JSON.stringify(err)}`);
                hilog.info(DOMAIN, TAG, `openLink callback result: ${JSON.stringify(result.resultCode)}`);
                hilog.info(DOMAIN, TAG, `openLink callback result data: ${JSON.stringify(result.want)}`);
              }
            ).then(() => {
              hilog.info(DOMAIN, TAG, `open link success.`);
            }).catch((err: BusinessError) => {
              hilog.error(DOMAIN, TAG, `open link failed, errCode ${JSON.stringify(err.code)}`);
            });
          } catch (e) {
            hilog.error(DOMAIN, TAG, `exception occured, errCode ${JSON.stringify(e.code)}`);
          }
        })
    }
    .height('100%')
    .width('100%')
    .justifyContent(FlexAlign.Center)
  }
}

---

## samples/ArkTSMultiPicture/commons/base/BuildProfile.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/commons/base/BuildProfile.ets

/**
 * Use these variables when you tailor your ArkTS code. They must be of the const type.
 */
export const HAR_VERSION = '1.0.0';
export const BUILD_MODE_NAME = 'debug';
export const DEBUG = true;
export const TARGET_NAME = 'default';

/**
 * BuildProfile Class is used only for compatibility purposes.
 */
export default class BuildProfile { 
	static readonly HAR_VERSION = HAR_VERSION;
	static readonly BUILD_MODE_NAME = BUILD_MODE_NAME;
	static readonly DEBUG = DEBUG;
	static readonly TARGET_NAME = TARGET_NAME;
}

---

## samples/ArkTSMultiPicture/commons/base/Index.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/commons/base/Index.ets

export { BaseConstants } from './src/main/ets/constants/BaseConstants'
export { BreakpointConstants } from './src/main/ets/constants/BreakpointConstants'
export { BreakpointType } from './src/main/ets/utils/BreakpointType'

---

## samples/ArkTSMultiPicture/commons/base/src/main/ets/constants/BaseConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/commons/base/src/main/ets/constants/BaseConstants.ets

/**
 * 基础常量
 */
export class BaseConstants {
  /**
   * Component size.
   */
  static readonly FULL_HEIGHT: string = '100%';
  static readonly FULL_WIDTH: string = '100%';
  /**
   * Text property.
   */
  static readonly FONT_FAMILY_NORMAL: Resource = $r('app.float.font_family_normal');
  static readonly FONT_FAMILY_MEDIUM: Resource = $r('app.float.font_family_medium');
  static readonly FONT_WEIGHT_FIVE: number = 500;
  static readonly FONT_WEIGHT_FOUR: number = 400;
  static readonly FONT_SIZE_TEN: Resource = $r('app.float.font_size_ten');
  static readonly FONT_SIZE_TWENTY: Resource = $r('app.float.font_size_twenty');
  /**
   * Default icon size.
   */
  static readonly DEFAULT_ICON_SIZE: number = 24;
  /**
   * Device 2in1.
   */
  static readonly DEVICE_2IN1: string = '2in1';
}



---

## samples/ArkTSMultiPicture/commons/base/src/main/ets/constants/BreakpointConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/commons/base/src/main/ets/constants/BreakpointConstants.ets

/**
 * 设备类型常量
 */
export class BreakpointConstants {
  /**
   * 小设备
   */
  static readonly BREAKPOINT_SM: string = 'sm';
  /**
   * 中设备
   */
  static readonly BREAKPOINT_MD: string = 'md';
  /**
   * 大设备
   */
  static readonly BREAKPOINT_LG: string = 'lg';
  /**
   * 屏幕宽度范围
   */
  static readonly BREAKPOINT_SCOPE: number[] = [0, 320, 600, 840];
}

---

## samples/ArkTSMultiPicture/commons/base/src/main/ets/utils/BreakpointType.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/commons/base/src/main/ets/utils/BreakpointType.ets

import { BreakpointConstants } from '../constants/BreakpointConstants';

/**
 * 设备尺寸类型
 */
export class BreakpointType<T> {
  sm: T;
  md: T;
  lg: T;

  constructor(sm: T, md: T, lg: T) {
    this.sm = sm;
    this.md = md;
    this.lg = lg;
  }

  GetValue(currentBreakpoint: string): T {
    if (currentBreakpoint === BreakpointConstants.BREAKPOINT_MD) {
      return this.md;
    } else if (currentBreakpoint === BreakpointConstants.BREAKPOINT_LG) {
      return this.lg;
    } else {
      return this.sm;
    }
  }
}

---

## samples/ArkTSMultiPicture/features/pictureView/BuildProfile.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/BuildProfile.ets

/**
 * Use these variables when you tailor your ArkTS code. They must be of the const type.
 */
export const HAR_VERSION = '1.0.0';
export const BUILD_MODE_NAME = 'debug';
export const DEBUG = true;
export const TARGET_NAME = 'default';

/**
 * BuildProfile Class is used only for compatibility purposes.
 */
export default class BuildProfile { 
	static readonly HAR_VERSION = HAR_VERSION;
	static readonly BUILD_MODE_NAME = BUILD_MODE_NAME;
	static readonly DEBUG = DEBUG;
	static readonly TARGET_NAME = TARGET_NAME;
}

---

## samples/ArkTSMultiPicture/features/pictureView/Index.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/Index.ets

/*
 * Copyright (c) 2023 Huawei Device Co., Ltd.
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

export { PictureViewIndex } from './src/main/ets/pages/PictureViewIndex'

---

## samples/ArkTSMultiPicture/features/pictureView/src/main/ets/viewmodel/Adaptive.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/src/main/ets/viewmodel/Adaptive.ets

import { BaseConstants as Constants, BreakpointType } from '@ohos/commons';
import PictureViewConstants from '../constants/PictureViewConstants';

/**
 * 尺寸适配
 */
export class Adaptive {
  static PICTURE_HEIGHT = (currentBreakpoint: string): string => {
    return new BreakpointType(
      PictureViewConstants.PICTURE_HEIGHT_SM,
      PictureViewConstants.PICTURE_HEIGHT_MD,
      PictureViewConstants.PICTURE_HEIGHT_LG,
    ).GetValue(currentBreakpoint);
  };
  static PICTURE_WIDTH = (currentBreakpoint: string): string => {
    return new BreakpointType(
      PictureViewConstants.PICTURE_WIDTH_SM,
      PictureViewConstants.PICTURE_WIDTH_MD,
      PictureViewConstants.PICTURE_WIDTH_LG,
    ).GetValue(currentBreakpoint);
  };
}

---

## samples/ArkTSMultiPicture/features/pictureView/src/main/ets/constants/PictureViewConstants.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/src/main/ets/constants/PictureViewConstants.ets

/**
 * 常量
 */
export interface ActionInterface {
  icon: Resource
  icon_name: string
}

export default class PictureViewConstants {
  /**
   * Picture size.
   */
  static PICTURE_HEIGHT_SM = '88%';
  static PICTURE_HEIGHT_MD = '100%';
  static PICTURE_HEIGHT_LG = '100%';
  static PICTURE_WIDTH_SM = '100%';
  static PICTURE_WIDTH_MD = '84.5%';
  static PICTURE_WIDTH_LG = '46.9%';
  static readonly ICON_LIST_WIDTH = "18%";
  /**
   * Actions.
   */
  static ACTIONS: ActionInterface[] = [
    {
      icon: $r('app.media.ic_public_share'), icon_name: "分享"
    },
    {
      icon: $r('app.media.ic_public_favor'), icon_name: "收藏"
    },
    {
      icon: $r("app.media.ic_gallery_public_details_4"), icon_name: "编辑"
    },
    {
      icon: $r("app.media.ic_gallery_public_details_5"), icon_name: "删除"
    },
    {
      icon: $r('app.media.ic_public_more'), icon_name: "更多"
    }
  ];
  /**
   * Pictures.
   */
  static readonly PICTURES: Resource[] = [
    $r('app.media.photo1'),
    $r('app.media.photo2'),
    $r('app.media.photo3'),
    $r('app.media.photo4'),
    $r('app.media.photo5'),
    $r('app.media.photo6'),
    $r('app.media.photo7'),
    $r('app.media.photo8'),
    $r('app.media.photo9'),
    $r('app.media.photo1'),
    $r('app.media.photo2'),
    $r('app.media.photo3'),
    $r('app.media.photo4'),
    $r('app.media.photo5'),
    $r('app.media.photo6'),
    $r('app.media.photo7'),
    $r('app.media.photo8'),
    $r('app.media.photo9'),
    $r('app.media.photo1'),
    $r('app.media.photo2'),
    $r('app.media.photo3'),
    $r('app.media.photo4'),
    $r('app.media.photo5'),
    $r('app.media.photo6'),
    $r('app.media.photo7'),
    $r('app.media.photo8'),
    $r('app.media.photo9'),
    $r('app.media.photo1'),
    $r('app.media.photo2'),
    $r('app.media.photo3'),
    $r('app.media.photo4'),
    $r('app.media.photo5'),
    $r('app.media.photo6'),
    $r('app.media.photo7'),
    $r('app.media.photo8'),
    $r('app.media.photo9'),
    $r('app.media.photo1'),
    $r('app.media.photo2'),
    $r('app.media.photo3'),
    $r('app.media.photo4'),
    $r('app.media.photo5'),
    $r('app.media.photo6'),
    $r('app.media.photo7'),
    $r('app.media.photo8'),
    $r('app.media.photo9'),
    $r('app.media.photo1'),
    $r('app.media.photo2'),
    $r('app.media.photo3'),
    $r('app.media.photo4'),
    $r('app.media.photo5'),
    $r('app.media.photo6'),
    $r('app.media.photo7'),
    $r('app.media.photo8'),
    $r('app.media.photo9')
  ];
}

---

## samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/BottomBar.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/BottomBar.ets

import PictureViewConstants, { ActionInterface } from '../constants/PictureViewConstants';
import { BaseConstants } from '@ohos/commons';

/**
 * 底部操作栏
 */
@Component
export struct BottomBar {
  build() {
    Flex({
      justifyContent: FlexAlign.Center,
      direction: FlexDirection.Row
    }) {
      ForEach(PictureViewConstants.ACTIONS, (item: ActionInterface) => {
        Column() {
          Image(item.icon)
            .height(BaseConstants.DEFAULT_ICON_SIZE)
            .width(BaseConstants.DEFAULT_ICON_SIZE)
          Text(item.icon_name)
            .fontFamily(BaseConstants.FONT_FAMILY_NORMAL)
            .fontSize(BaseConstants.FONT_SIZE_TEN)
            .fontWeight(BaseConstants.FONT_WEIGHT_FOUR)
            .padding({ top: $r('app.float.icon_padding_top') })
        }
        .width(PictureViewConstants.ICON_LIST_WIDTH)
      }, (item: ActionInterface, index: number) => index + JSON.stringify(item))
    }
    .height($r('app.float.icon_list_height'))
  }
}

---

## samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/CenterPart.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/CenterPart.ets

import { BreakpointConstants } from '@ohos/commons';
import { Adaptive } from '../viewmodel/Adaptive'

const FINGER_NUM: number = 2

/**
 * 中部图片显示区
 */
@Component
export struct CenterPart {
  @StorageLink('currentBreakpoint') currentBreakpoint: string = BreakpointConstants.BREAKPOINT_MD;
  @State scaleValue: number = 1;
  @State pinchValue: number = 1;
  @Link selectedPhoto: Resource;

  build() {
    Flex({ direction: FlexDirection.Column }) {
      Blank()
      Row() {
        Column() {
          // 绑定选中的图片
          Image(this.selectedPhoto)
            .autoResize(true)
        }
      }
      .height(Adaptive.PICTURE_HEIGHT(this.currentBreakpoint))
      .width(Adaptive.PICTURE_WIDTH(this.currentBreakpoint))
      .scale({ x: this.scaleValue, y: this.scaleValue, z: 1 })
      // 设置2指缩放
      .gesture(PinchGesture({ fingers: FINGER_NUM })
        .onActionUpdate((event: GestureEvent | undefined) => {
          if (event) {
            this.scaleValue = this.pinchValue * event.scale;
          }
        })
        .onActionEnd(() => {
          this.pinchValue = this.scaleValue;
        }))

      Blank()// 针对小设备设置
        .height(this.currentBreakpoint === BreakpointConstants.BREAKPOINT_SM ? $r('app.float.center_blank_height_lg') :
          0)
    }
    .margin({
      top: $r('app.float.center_margin_top'),
      bottom: $r('app.float.center_margin_bottom')
    })
  }
}

---

## samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/TopBar.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/TopBar.ets

import { BaseConstants, BreakpointConstants } from '@ohos/commons';
import PictureViewConstants, { ActionInterface } from '../constants/PictureViewConstants';

const TITLE: string = '图片预览器';

/**
 * 顶部区域
 */
@Preview
@Component
export struct TopBar {
  @StorageLink('currentBreakpoint') currentBp: string = BreakpointConstants.BREAKPOINT_MD;

  build() {
    Flex({
      direction: FlexDirection.Row,
      alignItems: ItemAlign.Center,
    }) {
      Column() {
        Flex({
          justifyContent: FlexAlign.SpaceBetween,
          direction: FlexDirection.Row,
          alignItems: ItemAlign.Stretch
        }) {
          Row() {
            Column() {
              Text(TITLE)
                .fontFamily(BaseConstants.FONT_FAMILY_MEDIUM)
                .fontSize(BaseConstants.FONT_SIZE_TWENTY)
                .fontWeight(BaseConstants.FONT_WEIGHT_FIVE)
            }
            .alignItems(HorizontalAlign.Start)
          }

          Row() {
            // 仅在大设备上显示操作按钮
            if (this.currentBp === BreakpointConstants.BREAKPOINT_LG) {
              ForEach(PictureViewConstants.ACTIONS, (item: ActionInterface) => {
                Image(item.icon)
                  .height(BaseConstants.DEFAULT_ICON_SIZE)
                  .width(BaseConstants.DEFAULT_ICON_SIZE)
                  .margin({ left: $r('app.float.detail_image_left') })
              }, (item: ActionInterface, index: number) => index + JSON.stringify(item))
            }
          }
        }
      }
    }
    .height($r('app.float.top_bar_height'))
    .margin({
      top: $r('app.float.top_bar_top'),
      bottom: $r('app.float.top_bar_bottom'),
      left: $r('app.float.top_bar_left'),
      right: $r('app.float.top_bar_right')
    })
  }
}

---

## samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/PreviewList.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/src/main/ets/view/PreviewList.ets

import { BreakpointConstants } from '@ohos/commons';
import PictureViewConstants from '../constants/PictureViewConstants';

const IMAGE_ASPECT_RATIO: number = 0.5;

/**
 * 图片预览列表
 */
@Component
export struct PreviewList {
  @StorageLink('currentBreakpoint') currentBreakpoint: string = BreakpointConstants.BREAKPOINT_MD;
  @Link selectedPhoto: Resource;

  build() {
    List({ initialIndex: 1 }) {
      ForEach(PictureViewConstants.PICTURES, (item: Resource) => {
        ListItem() {
          Image(item)
            .height($r('app.float.list_image_height'))
            .aspectRatio(IMAGE_ASPECT_RATIO)
            .autoResize(true)
            .margin({ left: $r('app.float.list_image_margin_left') })
            .onClick(()=>{
              this.selectedPhoto = item;
            })
        }
      }, (item: Resource, index: number) => index + JSON.stringify(item))
    }
    .height($r('app.float.list_image_height'))
    .padding({
      top: $r('app.float.list_margin_top'),
      bottom: $r('app.float.list_margin_bottom')
    })
    .listDirection(Axis.Horizontal)
    .scrollSnapAlign(ScrollSnapAlign.CENTER)
    .scrollBar(BarState.Off)
  }
}

---

## samples/ArkTSMultiPicture/features/pictureView/src/main/ets/pages/PictureViewIndex.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/features/pictureView/src/main/ets/pages/PictureViewIndex.ets

import { TopBar } from '../view/TopBar';
import { CenterPart } from '../view/CenterPart';
import { BottomBar } from '../view/BottomBar';
import { PreviewList } from '../view/PreviewList';
import { BaseConstants, BreakpointConstants } from '@ohos/commons';
import { deviceInfo } from '@kit.BasicServicesKit';

/**
 * 预览器主页
 */
@Entry
@Preview
@Component
export struct PictureViewIndex {
  @StorageLink('currentBreakpoint') currentBreakpoint: string = BreakpointConstants.BREAKPOINT_MD;
  @State selectedPhoto: Resource = $r('app.media.photo');

  build() {
    Column() {
      Flex({
        direction: FlexDirection.Column,
        alignItems: ItemAlign.Center
      }) {
        // 顶部区域
        TopBar()

        // 中部图片显示区
        CenterPart({ selectedPhoto: this.selectedPhoto })
          .flexGrow(1)

        // 图片预览列表
        PreviewList({ selectedPhoto: this.selectedPhoto })

        // 非大设备，则显示底部操作栏
        if (this.currentBreakpoint !== BreakpointConstants.BREAKPOINT_LG) {
          BottomBar()
        }
      }.padding({
        // 针对2in1设置
        top: deviceInfo.deviceType === BaseConstants.DEVICE_2IN1 ? $r('app.float.zero') :
        $r('app.float.device_padding_top'),

        // 针对非2in1设置
        bottom: deviceInfo.deviceType !== BaseConstants.DEVICE_2IN1 ? $r('app.float.tab_content_pb') :
        $r('app.float.zero')
      })
    }
    .height(BaseConstants.FULL_HEIGHT)
    .width(BaseConstants.FULL_WIDTH)
  }
}

---

## samples/ArkTSMultiPicture/product/default/src/main/ets/defaultability/DefaultAbility.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/product/default/src/main/ets/defaultability/DefaultAbility.ets

import { UIAbility, AbilityConstant, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { display, window } from '@kit.ArkUI';
import { BusinessError} from '@kit.BasicServicesKit';
import { BreakpointConstants } from '@ohos/commons/Index';

export default class PhoneAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 获取窗口对象
    windowStage.getMainWindow((err: BusinessError<void>, data) => {
      let windowObj: window.Window = data;

      // 计算设备的尺寸
      this.updateBreakpoint(windowObj.getWindowProperties().windowRect.width);
      windowObj.on('windowSizeChange', (windowSize: window.Size) => {
        this.updateBreakpoint(windowSize.width);
      })

      if (err.code) {
        hilog.info(0x0000, 'testTag', '%{public}s', 'getMainWindow failed');
        return;
      }
    })

    windowStage.loadContent('pages/Index', (err, data) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
    });
  }

  // 变更设备类型
  private updateBreakpoint(windowWidth: number) :void{
    let windowWidthVp = windowWidth / display.getDefaultDisplaySync().densityPixels;
    let curBp: string = '';
    if (windowWidthVp < BreakpointConstants.BREAKPOINT_SCOPE[2]) {
      curBp = BreakpointConstants.BREAKPOINT_SM;
    } else if (windowWidthVp < BreakpointConstants.BREAKPOINT_SCOPE[3]) {
      curBp = BreakpointConstants.BREAKPOINT_MD;
    } else {
      curBp = BreakpointConstants.BREAKPOINT_LG;
    }
    AppStorage.setOrCreate('currentBreakpoint', curBp);
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSMultiPicture/product/default/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSMultiPicture/product/default/src/main/ets/pages/Index.ets

import { PictureViewIndex } from '@ohos/view';

/**
 * 图片查看器主页
 */
@Entry
@Component
struct Index {
  build() {
    Column() {
      PictureViewIndex()
    }
  }
}

---

## samples/ArkTSShoppingCart/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSShoppingCart/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSShoppingCart/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSShoppingCart/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSShoppingCart/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSShoppingCart/entry/src/main/ets/pages/Index.ets

// 导入Emitter
import { emitter } from '@kit.BasicServicesKit';
import { HashMap, JSON } from '@kit.ArkTS';


class EventData {
  name: string = ""
}


@Entry
@Component
struct Index {
  //用于接收事件数据
  @State eventData: string = ''
  // 定义一个eventId为1的事件
  private addEvent: emitter.InnerEvent = {
    eventId: 1
  };
  // 定义一个eventId为0的事件
  private deleteEvent: emitter.InnerEvent = {
    eventId: 0
  };
  private goodsMap: HashMap<string, string> = new HashMap<string, string>();

  build() {
    Row() {
      Column() {
        Row() {
          Checkbox({ name: 'checkbox1', group: 'checkboxGroup' })
            .onChange((value: boolean) => { // 设置选中事件
              this.handleCheckbox('可乐', value);
            })
          Text('可乐').fontSize(20)
        }

        Row() {
          Checkbox({ name: 'checkbox2', group: 'checkboxGroup' })
            .onChange((value: boolean) => { // 设置选中事件
              this.handleCheckbox('鸡翅', value);
            })
          Text('鸡翅').fontSize(20)
        }

        Row() {
          Checkbox({ name: 'checkbox3', group: 'checkboxGroup' })
            .onChange((value: boolean) => { // 设置选中事件
              this.handleCheckbox('薯条', value);
            })
          Text('薯条').fontSize(20)
        }

        Row() {
          Checkbox({ name: 'checkbox4', group: 'checkboxGroup' })
            .onChange((value: boolean) => { // 设置选中事件
              this.handleCheckbox('汉堡', value);
            })
          Text('汉堡').fontSize(20)
        }

        // 接收到的事件数据
        Text(this.eventData)
          .fontSize(25)
          .fontWeight(FontWeight.Bold)
      }
      .width('100%')
    }
    .height('100%')
  }

  aboutToAppear() {
    console.log("aboutToAppear");
    this.subscriberEvent();
  }

  private handleCheckbox(name: string, value: boolean) {
    if (value) {
      this.addGoods(name);
    } else {
      this.deleteGoods(name);
    }
  }

  private subscriberEvent() {
    // 收到eventId为1的事件后执行该回调
    let addEventCallback = (eventData: emitter.EventData): void => {
      // 接收到事件
      let data: EventData =  eventData.data as EventData;
      let name  = data.name;
      this.goodsMap.set(name, name);

      this.eventData = "add: " + name + "; total: " + this.mapToSting(this.goodsMap);
    };

    // 订阅eventId为1的事件
    emitter.on(this.addEvent, addEventCallback);


    // 收到eventId为0的事件后执行该回调
    let deleteEventCallback = (eventData: emitter.EventData): void => {
      // 接收到事件
      let data: EventData =  eventData.data as EventData;
      let name  = data.name;
      this.goodsMap.remove(name);

      this.eventData = "delete: " + name + "; total: " + this.mapToSting(this.goodsMap);
    };

    // 订阅eventId为0的事件
    emitter.on(this.deleteEvent, deleteEventCallback);

    console.log("subscriber already created");
  }

  private addGoods(name: string) {
    let eventData: emitter.EventData = {
      data: {
        name: name
      }
    };

    // 发送eventId为1的事件，事件内容为eventData
    emitter.emit(this.addEvent, eventData);
  }

  private deleteGoods(name: string) {
    let eventData: emitter.EventData = {
      data: {
        name: name
      }
    };

    // 发送eventId为0的事件，事件内容为eventData
    emitter.emit(this.deleteEvent, eventData);
  }

  private mapToSting(hashMap : HashMap<string, string>): string {
    let str = '';
    hashMap.forEach((value?: string, key?: string) => {
      str = str + value + ' ';
    });

    return str;
  }

}

---

## samples/ArkTSHttp/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSHttp/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSHttp/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSHttp/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSHttp/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSHttp/entry/src/main/ets/pages/Index.ets

// 导入http模块
import { http } from '@kit.NetworkKit';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(38)
          .fontWeight(FontWeight.Bold)

        // 请求
        Button(('请求'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            this.httpReq()
          })
      }
      .width('100%')
    }
    .height('100%')
  }

  private httpReq() {
    // 创建httpRequest对象。
    let httpRequest = http.createHttp();

    let url = "https://waylau.com/data/people.json";
    let promise = httpRequest.request(
      // 请求url地址
      url,
      {
        // 请求方式
        method: http.RequestMethod.GET,
        // 可选，默认为60s
        connectTimeout: 60000,
        // 可选，默认为60s
        readTimeout: 60000,
        // 开发者根据自身业务需要添加header字段
        header: {
          'Content-Type': 'application/json'
        }
      });

    // 处理响应结果。
    promise.then((data) => {
      if (data.responseCode === http.ResponseCode.OK) {
        console.info('Result:' + data.result);
        console.info('code:' + data.responseCode);
        this.message = JSON.stringify(data.result);
      }
    }).catch((err:BusinessError) => {
      console.info('error:' + JSON.stringify(err));
    });
  }
}

---

## samples/ArkTSWantOpenManageApplications/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSWantOpenManageApplications/entry/src/main/ets/pages/Index.ets

// 导入common
import common from '@ohos.app.ability.common';

@Entry
@Component
struct Index {
  build() {
    Row() {
      Column() {
        // 添加按钮，启动Ability
        Button('启动')
          .fontSize(40)
          .onClick(this.implicitStartAbility) // 隐示启动Ability
      }
      .width('100%')
    }
    .height('100%')
  }

  // 隐示启动Ability
  async implicitStartAbility() {
    try {
      let want = {
        // 调用应用管理
        "action": "ohos.settings.manage.applications"
      }
      let context = getContext(this) as common.UIAbilityContext;
      await context.startAbility(want)
      console.info(`implicit start ability succeed`)
    } catch (error) {
      console.info(`implicit start ability failed with ${error.code}`)
    }
  }
}

---

## samples/ArkTSEmitter/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSEmitter/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSEmitter/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSEmitter/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSEmitter/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSEmitter/entry/src/main/ets/pages/Index.ets

// 导入Emitter
import { emitter } from '@kit.BasicServicesKit';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'
  //用于接收事件数据
  @State eventData: string = ''
  // 定义一个eventId为1的事件
  private event: emitter.InnerEvent = {
    eventId: 1
  };

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(25)
          .fontWeight(FontWeight.Bold)

        // 订阅事件
        Button(('订阅事件'), { type: ButtonType.Capsule })
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 10, bottom: 10 })
          .onClick(() => {
            this.subscriberEvent()
          })

        // 发送事件
        Button(('发送事件'), { type: ButtonType.Capsule })
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 10, bottom: 10 })
          .onClick(() => {
            this.emitEvent()
          })

        // 接收到的事件数据
        Text(this.eventData)
          .fontSize(25)
          .fontWeight(FontWeight.Bold)
      }
      .width('100%')
    }
    .height('100%')
  }

  private subscriberEvent() {
    // 收到eventId为1的事件后执行该回调
    let callback = (eventData: emitter.EventData): void => {
      // 接收到事件
      this.eventData = "subscribe event success: " + JSON.stringify(eventData);
    };

    // 订阅eventId为1的事件
    emitter.on(this.event, callback);

    this.message = "subscriber already created";
  }

  private emitEvent() {
    let eventData: emitter.EventData = {
      data: {
        content: 'waylau.com',
        id: 1,
        isEmpty: false
      }
    };

    // 发送eventId为1的事件，事件内容为eventData
    emitter.emit(this.event, eventData);
  }

}

---

## samples/ArkTSGeoLocationManager/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSGeoLocationManager/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

import { abilityAccessCtrl } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    let atManager = abilityAccessCtrl.createAtManager();
    try {
      atManager.requestPermissionsFromUser(this.context,
        ['ohos.permission.LOCATION', 'ohos.permission.APPROXIMATELY_LOCATION'])
        .then((data) => {
          hilog.info(0x0000, 'testTag', `data: ${JSON.stringify(data)}`);
        })
        .catch((err: BusinessError) => {
          hilog.error(0x0000, 'testTag', `err: ${JSON.stringify(err)}`);
        })
    } catch (err) {
      hilog.error(0x0000, 'testTag', `catch err->${JSON.stringify(err)}`);
    }
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSGeoLocationManager/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSGeoLocationManager/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSGeoLocationManager/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSGeoLocationManager/entry/src/main/ets/pages/Index.ets

import { geoLocationManager } from '@kit.LocationKit';

import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('HelloWorld')
        .fontSize(30)
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
        .onClick(() => {
          this.getLocation();
        })
    }
    .height('100%')
    .width('100%')
  }

  getLocation() {
    let startTime = new Date().getTime();
    let requestInfo: geoLocationManager.CurrentLocationRequest = {
      'priority': geoLocationManager.LocationRequestPriority.FIRST_FIX,
      'scenario': geoLocationManager.LocationRequestScenario.UNSET, 'maxAccuracy': 100
    };

    let locationChange = (err: BusinessError, location: geoLocationManager.Location): void => {
      if (err) {
        console.error('locationChanger: err=' + JSON.stringify(err));
      }
      if (location) {
        console.log('locationChanger: location=' + JSON.stringify(location));
        this.message =
          '定位信息：' + JSON.stringify(location) + '\n 花费时间：' + (new Date().getTime() - startTime) / 1000
      }
    };
    try {
      geoLocationManager.getCurrentLocation(requestInfo, locationChange);
    } catch (err) {
      console.error("errCode:" + err.code + ",errMessage:" + err.message);
    }
    ;
  }
}

---

## samples/ArkUIBasicComponents/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkUIBasicComponents/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkUIBasicComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkUIBasicComponents/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkUIBasicComponents/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkUIBasicComponents/entry/src/main/ets/pages/Index.ets

// HTTP请求网络图片需要导入的包
import http from '@ohos.net.http';

import image from '@ohos.multimedia.image';

// Web组件控制器需要导入的包
import web_webview from '@ohos.web.webview'

@Entry
@Component
struct Index {
  private dataPanelValues: number[] = [11, 3, 10, 2, 36, 4, 7, 22, 5]

  // 先创建一个PixelMap状态变量用于接收网络图片
  @State imagePixelMap: image.PixelMap = null!;

  // 初始动画状态
  @State animationStatus: AnimationStatus = AnimationStatus.Initial

  // 可滚动组件的控制器
  private scroller: Scroller = new Scroller()

  private dataScroller: number[] = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

  private webviewController: web_webview.WebviewController = new web_webview.WebviewController()

  // TextTimer组件的控制器
  private textTimerController: TextTimerController = new TextTimerController()

  private tooTmpOne: ToolbarItem = {'value': "首页",
    'icon': $r('app.media.house'),
    'action': ()=> {}
  }
  private tooTmpTwo: ToolbarItem = {'value': "好友",
    'icon': $r('app.media.person_2'),
    'action': ()=> {}
  }
  private tooTmpThree: ToolbarItem = {'value': "我的",
    'icon': $r('app.media.gearshape'),
    'action': ()=> {}
  }
  private toolbarConfig: ToolbarItem[] = [this.tooTmpOne, this.tooTmpTwo, this.tooTmpThree];

  build() {
    Column() {
      /*** 以下为 Blank示例 ***/
      // Blank父组件Row未设置宽度时，子组件间无空白填充
      /*
      Row() {
        Text('Left Space').fontSize(24)
        Blank()
        Text('Right Space').fontSize(24)
      }

      // Blank父组件Row设置了宽度时，子组件间以空白填充
      Row() {
        Text('Left Space').fontSize(24)
        Blank()
        Text('Right Space').fontSize(24)
      }.width('100%')

      // Blank组件设置了颜色
      Row() {
        Text('Left Space').fontSize(24)

        // 设置空白填充的填充颜色
        Blank().color(Color.Yellow)

        Text('Right Space').fontSize(24)
      }.width('100%')
      */

      /*** 以下为 Button示例 ***/
      /*
      Row() {
        // 一个基本的按钮，设置要显示的文字
        Button('01')

        // 设置边框半径、背景色、宽度
        Button('02').borderRadius(8).backgroundColor(0x317aff).width(90)
      }

      Row() {
        // 胶囊型按钮（圆角默认为高度的一半）
        Button('03', { type: ButtonType.Capsule }).width(90)

        // 圆形按钮。
        Button('04', { type: ButtonType.Circle}).width(90)

        // 普通按钮（默认不带圆角）。
        Button('05', { type: ButtonType.Normal}).width(90)
      }

      Row() {
        // 可以包含子组件。文字就用Text组件来显示
        Button({ type: ButtonType.Capsule, stateEffect: true }) {
          Row() {
            LoadingProgress().width(20).height(20).margin({ left: 12 }).color(0xFFFFFF)
            Text('06').fontSize(12).fontColor(0xffffff).margin({ left: 5, right: 12 })
          }.alignItems(VerticalAlign.Center).width(90).height(40)
        }.backgroundColor(0x317aff)
      }
      */


      /*** 以下为 Checkbox 示例 ***/
      /*
      Row() {
        Checkbox({ name: 'checkbox1', group: 'checkboxGroup' })
          .select(true) // 设置默认选中
          .selectedColor(0xed6f21) // 设置选中颜色
          .onChange((value: boolean) => { // 设置选中事件
            console.info('Checkbox1 change is ' + value)
          })

        Checkbox({ name: 'checkbox2', group: 'checkboxGroup' })
          .select(false)
          .selectedColor(0x39a2db)
          .onChange((value: boolean) => {
            console.info('Checkbox2 change is ' + value)
          })
      }
      */

      /*** 以下为 CheckboxGroup 示例 ***/
      /*
      Row() {
        CheckboxGroup({ group: 'checkboxGroup' })
        Text('全要').fontSize(20)
      }

      Row() {
        Checkbox({ name: 'checkbox1', group: 'checkboxGroup' })
        Text('可乐').fontSize(20)
      }

      Row() {
        Checkbox({ name: 'checkbox2', group: 'checkboxGroup' })
        Text('鸡翅').fontSize(20)
      }
      */

      /*** 以下为 DataPanel 示例 ***/
      // 环形数据面板
      /*
      DataPanel({ values: this.dataPanelValues, max: 100, type: DataPanelType.Circle })

        .width(350)
        .height(350)

      // 线型数据面板
      DataPanel({ values: this.dataPanelValues, max: 100, type: DataPanelType.Line })
        .width(350)
        .height(50)
       */

      /*** 以下为 DatePicker 示例 ***/
      /*// 不设置参数就取默认值
      DatePicker()

      // 三个构造参数
      DatePicker({
        start: new Date('1970-1-1'), //指定选择器的起始日期。 默认值：Date('1970-1-1')
        end: new Date('2100-1-1'), // 指定选择器的结束日期。 默认值：Date('2100-12-31')
        selected: new Date('2023-02-15'), // 设置选中项的日期。默认值：当前系统日期
      })

      // 设置农历和事件监听
      DatePicker({
        start: new Date('1970-1-1'), //指定选择器的起始日期。 默认值：Date('1970-1-1')
        end: new Date('2100-1-1'), // 指定选择器的结束日期。 默认值：Date('2100-12-31')
        selected: new Date('2023-02-15'), // 设置选中项的日期。默认值：当前系统日期
      }).lunar(true) // 设置农历
        .onDateChange((value: Date) => { //选择日期时触发该事件
          console.info('select current date is: ' + value)
        })*/

      /*** 以下为 Divider 示例 ***/

      /*Text('我是天').fontSize(29)
      Divider()
      Text('我是地').fontSize(29)*/


      /*Text('我是天').fontSize(29)
      // 设置垂直
      Divider().vertical(true).height(100)
      Text('我是地').fontSize(29)*/

      /*Text('我是天').fontSize(29)
      // 设置样式
      Divider()
        .strokeWidth(15)  // 宽度
        .color(0x2788D9) // 颜色
        .lineCap(LineCapStyle.Round) // 端点样式
      Text('我是地').fontSize(29)*/

      /*** 以下为 Gauge 示例 ***/

      // value值的设置，是使用默认的min和max为0-100，角度范围默认0-360
      // 参数中设置当前值为75
      /*Gauge({ value: 75 })
        .width(200).height(200)
        // 设置量规图的颜色，支持分段颜色设置。
        .colors([[0x317AF7, 1], [0x5BA854, 1], [0xE08C3A, 1], [0x9C554B, 1]])*/

      // 参数设置当前值为75，属性设置值为25，属性设置优先级高
      /*Gauge({ value: 75 })
        .value(25) //属性和参数都设置时以属性为准
        .width(200).height(200)
        .colors([[0x317AF7, 1], [0x5BA854, 1], [0xE08C3A, 1], [0x9C554B, 1]])*/

      // 210--150度环形图表
      /*Gauge({ value: 70 })
        .startAngle(210) // 起始角度
        .endAngle(150) // 终止角度
        .colors([[0x317AF7, 0.1], [0x5BA854, 0.2], [0xE08C3A, 0.3], [0x9C554B, 0.4]])
        .strokeWidth(20) // 环形厚度
        .width(200)
        .height(200)*/

      /*** 以下为 Image 示例 ***/
      // 使用本地图片的示例
      // 图片资源在base/media目录下
      /*Image($r('app.media.waylau_181_181'))
        .width(180).height(180)*/

      // 使用网络图片的示例
      /*Button("获取网络图片")
        .onClick(() => {
          // 请求网络资源
          this.httpRequest();
        })
      Image(this.imagePixelMap).width(207).height(281)*/

      /*** 以下为 ImageAnimator 示例 ***/
      /*// 按钮控制动画的播放和暂停
      Button('播放').width(100).padding(5).onClick(() => {
        this.animationStatus = AnimationStatus.Running
      }).margin(5)
      Button('暂停').width(100).padding(5).onClick(() => {
        this.animationStatus = AnimationStatus.Paused
      }).margin(5)

      // images设置设置图片帧信息集合。
      // 每一帧的帧信息(ImageFrameInfo)包含图片路径、图片大小、图片位置和图片播放时长信息，
      ImageAnimator()
        .images([
          {
            src: $r('app.media.book01'), //图片路径
            duration: 500, //播放时长
            width: 240, //图片大小
            height: 350,
            top: 0, //图片位置
            left: 0
          },
          {
            src: $r('app.media.book02'),
            duration: 500,
            width: 240,
            height: 350,
            top: 0,
            left: 170
          },
          {
            src: $r('app.media.book03'),
            duration: 500,
            width: 240,
            height: 350,
            top: 120,
            left: 170
          },
          {
            src: $r('app.media.book04'),
            duration: 500,
            width: 240,
            height: 350,
            top: 120,
            left: 0
          }
        ])
        .state(this.animationStatus)
        .reverse(false) //是否逆序播放
        .fixedSize(false) //是否固定大小
        .iterations(-1) //循环播放次数
        .width(240)
        .height(350)
        .margin({ top: 100 })*/

      /*** 以下为 LoadingProgress 示例 ***/
      /*// 显示加载动效
      LoadingProgress()
        .color(Color.Red) //设置为红色*/

      /*** 以下为 Marquee 示例 ***/

      /*// 文本内容宽度未超过跑马灯组件宽度，不滚动。
      Marquee({
        start: true, // 控制跑马灯是否进入播放状态。
        step: 12, // 滚动动画文本滚动步长。默认值：6，单位vp
        loop: -1, // 循环次数，-1为无限循环
        fromStart: true, // 设置文本从头开始滚动或反向滚动
        src: "HarmonyOS也称为鸿蒙系统"
      }).fontSize(20)

      // 文本内容宽度超过了跑马灯组件宽度，滚动。
      Marquee({
        start: true, // 控制跑马灯是否进入播放状态。
        step: 12, // 滚动动画文本滚动步长。默认值：6，单位vp
        loop: -1, // 循环次数，-1为无限循环
        fromStart: true, // 设置文本从头开始滚动或反向滚动
        src: "在传统的单设备系统能力基础上，HarmonyOS提出了基于同一套系统能力、适配多种终端形态的分布式理念。"
      }).fontSize(20)*/

      /*** 以下为 Navigation 示例 ***/
      /*Navigation() {
        Flex() {
        }
      }
      .toolbarConfiguration(this.toolbarConfig); // 使用自定义属性*/

      /*** 以下为 PatternLock 示例 ***/
      /*PatternLock()
        .sideLength(200)//设置组件的宽度和高度（宽高相同）
        .circleRadius(9)//设置宫格中圆点的半径。
        .pathStrokeWidth(18)//设置连线的宽度。设置为0或负数等非法值时连线不显示。
        .activeColor('#B0C4DE')//设置宫格圆点在“激活”状态的填充颜色
        .selectedColor('#228B22')//设置宫格圆点在“选中”状态的填充颜色。
        .pathColor('#90EE90')//设置连线的颜色。
        .backgroundColor('#F5F5F5')//背景颜色
        .autoReset(true)//设置在完成密码输入后再次在组件区域按下时是否重置组件状态。*/

      /*** 以下为 Progress 示例 ***/
      /*Progress({ value: 20, total: 100, type: ProgressType.Linear })
        .width(150).margin({ top: 10 })
      Progress({ value: 20, total: 100, type: ProgressType.Ring })
        .width(150).margin({ top: 10 })
      Progress({ value: 20, total: 100, type: ProgressType.Eclipse })
        .width(150).margin({ top: 10 })
      Progress({ value: 20, total: 100, type: ProgressType.ScaleRing })
        .width(150).margin({ top: 10 })
      Progress({ value: 20, total: 100, type: ProgressType.Capsule })
        .width(40).margin({ top: 10 })*/

      /*** 以下为 QRCode 示例 ***/
      /*QRCode("https://waylau.comn")
        .width(360).height(360) // 大小
        .backgroundColor(Color.Orange)// 颜色*/

      /*** 以下为 Radio 示例 ***/

      /*Radio({ value: 'Radio1', group: 'radioGroup' })
        .checked(false) //默认不选中
        .height(50)
        .width(50)
      Radio({ value: 'Radio2', group: 'radioGroup' })
        .checked(true) //默认选中
        .height(50)
        .width(50)
      Radio({ value: 'Radio2', group: 'radioGroup' })
        .checked(false) //默认不选中
        .height(50)
        .width(50)*/

      /*** 以下为 Rating 示例 ***/

      // 设置初始星数为1，可以操作
      /*Rating({ rating: 1, indicator: false })
        .stars(5) // 设置评星总数。默认值：5
        .stepSize(0.5) // 操作评级的步长。默认值：0.5
        .onChange((value: number) => {
          //...
        })*/

      /*** 以下为 RichText 示例 ***/

      /*RichText('<h1 style="text-align: center;">h1标题</h1>' +
      '<h1 style="text-align: center;"><i>h1斜体</i></h1>' +
      '<h1 style="text-align: center;"><u>h1下划线</u></h1>' +
      '<h2 style="text-align: center;">h2标题</h2>' +
      '<h3 style="text-align: center;">h3标题</h3>' +
      '<p style="text-align: center;">p常规</p><hr/>' +
      '<div style="width: 500px;height: 500px;border: 1px solid;margin: 0auto;">' +
      '<p style="font-size: 35px;text-align: center;font-weight: bold; color: rgb(24,78,228)">' +
        '字体大小35px,行高45px' +
        '</p>' +
      '<p style="background-color: #e5e5e5;line-height: 45px;font-size: 35px;text-indent: 2em;">' +
      '<p>这是一段文字这是一段文字这是一段文字这是一段文字这是一段文字这是一段文字这是一段文字这是一段文字这是一段文字</p>')
      */
      /*** 以下为 Stack 示例 ***/
      /*Stack({ alignContent: Alignment.End }) {
        // 定义了可滚动组件Scroll
        Scroll(this.scroller) {
          Flex({ direction: FlexDirection.Column }) {
            ForEach(this.dataScroller, (item: number) => {
              Row() {
                Text(item.toString())
                  .width('90%')
                  .height(100)
                  .backgroundColor('#3366CC')
                  .borderRadius(15)
                  .fontSize(16)
                  .textAlign(TextAlign.Center)
                  .margin({ top: 5 })
              }
            })
          }.margin({ left: 52 })
        }
        .scrollBar(BarState.Off)
        .scrollable(ScrollDirection.Vertical)

        // 定义了滚动条组件ScrollBar
        ScrollBar({ scroller: this.scroller,
          direction: ScrollBarDirection.Vertical,
          state: BarState.Auto }) {
          // 定义Text作为滚动条的样式
          Text()
            .width(30)
            .height(100)
            .borderRadius(10)
            .backgroundColor('#C0C0C0')
        }.width(30).backgroundColor('#ededed')
      }*/

      /*** 以下为 Search 示例 ***/
      /*Search({ placeholder: '输入内容...'})
        .searchButton('搜索') // 搜索按钮的文字
        .width(300)
        .height(80)
        .placeholderColor(Color.Grey) // 提示文本样式
        .placeholderFont({ size: 24, weight: 400 }) // 提示文本字体大小
        .textFont({ size: 24, weight: 400 }) // 搜索框文字字体大小*/


      /*** 以下为 Select 示例 ***/
      // 设置下来列表值和图标
      /*Select([{ value: 'Java核心编程', icon: $r('app.media.book01') },
        { value: '轻量级Java EE企业应用开发实战', icon: $r('app.media.book02') },
        { value: '鸿蒙HarmonyOS手机应用开发实战', icon: $r('app.media.book03') },
        { value: 'Node.js+Express+MongoDB+Vue.js全栈开发实战', icon: $r('app.media.book04') }])
        .selected(2) // 选择的下来列表索引
        .value('老卫作品集') // 下拉按钮本身的文本内容。
        .font({ size: 16, weight: 500 }) // 下拉按钮本身的文本样式。
        .fontColor('#182431') // 下拉按钮本身的文本颜色。
        .selectedOptionFont({ size: 16, weight: 400 }) // 下拉菜单选中项的文本样式。
        .optionFont({ size: 16, weight: 400 }) // 下拉菜单项的文本样式。*/

      /*** 以下为 Slider 示例 ***/
      /*// 设置垂直的Slider
      Slider({
        value: 40,
        step: 10,
        style: SliderStyle.InSet, // 滑块在滑轨上
        direction: Axis.Vertical // 方向
      })
        .showSteps(true) // 设置显示步长刻度值
        .height('50%')

      // 设置水平的Slider
      Slider({
        value: 40,
        min: 0,
        max: 100,
        style: SliderStyle.OutSet //滑块在滑轨内
      })
        .blockColor('#191970') // 设置滑块的颜色
        .trackColor('#ADD8E6') // 设置滑轨的背景颜色
        .selectedColor('#4169E1') // 设置滑轨的已滑动部分颜色
        .showTips(true) // 设置气泡提示
        .width('50%')*/

      /*** 以下为 Span 示例 ***/
      /*// 文本添加横线
      Text() {
        Span('文本添加横线').decoration({ type: TextDecorationType.Underline, color: Color.Red }).fontSize(24)
      }

      // 文本添加划掉线
      Text() {
        Span('文本添加划掉线')
          .decoration({ type: TextDecorationType.LineThrough, color: Color.Red })
          .fontSize(24)
      }

      // 文本添加上划线
      Text() {
        Span('文本添加上划线').decoration({ type: TextDecorationType.Overline, color: Color.Red }).fontSize(24)
      }

      // 文本字符间距
      Text() {
        Span('文本字符间距')
          .letterSpacing(10)
          .fontSize(24)
      }

      // 文本转化为小写LowerCase
      Text() {
        Span('文本转化为小写LowerCase').fontSize(24)
          .textCase(TextCase.LowerCase)
          .decoration({ type: TextDecorationType.None })
      }

      // 文本转化为大写UpperCase
      Text() {
        Span('文本转化为小写UpperCase').fontSize(24)
          .textCase(TextCase.UpperCase)
          .decoration({ type: TextDecorationType.None })
      }*/

      /*** 以下为 Stepper 示例 ***/
      /*Stepper({
        // 设置步骤导航器当前显示StepperItem的索引值。
        index: 0
      }) {
        // 第1页
        StepperItem() {
          Text('第1页').fontSize(34)
        }
        .nextLabel('下一页')

        // 第2页
        StepperItem() {
          Text('第2页').fontSize(34)
        }
        .nextLabel('下一页')
        .prevLabel('上一页')

        // 第3页
        StepperItem() {
          Text('第3页').fontSize(34)
        }
        .prevLabel('上一页')

      }*/

      /*** 以下为 Text 示例 ***/
      /*// 单行文本
      // 红色单行文本居中
      Text('红色单行文本居中').fontSize(24)
        .fontColor(Color.Red) // 红色
        .textAlign(TextAlign.Center) // 居中
        .width('100%')

      // 单行文本对齐左侧
      Text('单行文本对齐左侧').fontSize(24)
        .textAlign(TextAlign.Start) // 对齐左侧
        .width('100%')

      // 单行文本带边框对齐右侧
      Text('单行文本带边框对齐右侧')
        .fontSize(24)
        .textAlign(TextAlign.End) // 对齐右侧
        .border({ width: 1 }) // 边宽
        .padding(10)
        .width('100%')

      // 多行文本
      // 超出maxLines截断内容展示
      Text('寒雨连江夜入吴，平明送客楚山孤。洛阳亲友如相问，一片冰心在玉壶。')
        .textOverflow({ overflow: TextOverflow.None }) // 超出截断内容
        .maxLines(2) //　最多显示２行
        .fontSize(24)
        .border({ width: 1 })
        .padding(10)

      // 超出maxLines展示省略号
      Text('寒雨连江夜入吴，平明送客楚山孤。洛阳亲友如相问，一片冰心在玉壶。')
        .textOverflow({ overflow: TextOverflow.Ellipsis }) // 超出展示省略号
        .maxLines(2)
        .fontSize(24)
        .border({ width: 1 })
        .padding(10)

      // 设置行高
      Text('寒雨连江夜入吴，平明送客楚山孤。洛阳亲友如相问，一片冰心在玉壶。')
        .textOverflow({ overflow: TextOverflow.Ellipsis }) // 超出展示省略号
        .maxLines(2)
        .fontSize(24)
        .border({ width: 1 })
        .padding(10)
        .lineHeight(50) // 设置文本的文本行高*/

      /*** 以下为 TextArea 示例 ***/
      /*TextArea({
        // 设置无输入时的提示文本
        placeholder: '寒雨连江夜入吴，平明送客楚山孤。洛阳亲友如相问，一片冰心在玉壶。'
      })
        .placeholderFont({ size: 24, weight: 400 })  // 设置placeholder文本样式
        .width(336)
        .height(100)
        .margin(20)
        .fontSize(16)
        .fontColor('#182431')
        .backgroundColor('#FFFFFF')*/

      /*** 以下为 TextClock 示例 ***/
      // 普通的TextClock示例
      /*TextClock().margin(20).fontSize(30)

      // 带日期格式化的TextClock示例
      TextClock().margin(20).fontSize(30)
        .format('yyyyMMdd hh:mm:ss') // 日期格式化*/

      /*** 以下为 TextInput 示例 ***/
      /*// 文本输入框
      TextInput({ placeholder: '请输入...'}) // 设置无输入时的提示文本
        .placeholderColor(Color.Grey) // 设置placeholder文本颜色
        .placeholderFont({ size: 14, weight: 400 }) // 设置placeholder文本样式
        .caretColor(Color.Blue) // 设置输入框光标颜色
        .width(300)
        .height(40)
        .margin(20)
        .fontSize(24)
        .fontColor(Color.Black)

      // 密码输入框
      TextInput({ placeholder: '请输入密码...' })
        .width(300)
        .height(40)
        .margin(20)
        .fontSize(24)
        .type(InputType.Password) // 密码类型
        .maxLength(9) // 	设置文本的最大输入字符数
        .showPasswordIcon(true) // 输入框末尾的图标显示*/

      /*** 以下为 TextPicker 示例 ***/
      /*TextPicker({
        // 选择器的数据选择列表
        range: ['Java核心编程', '轻量级Java EE企业应用开发实战', '鸿蒙HarmonyOS手机应用开发实战', 'Node.js+Express+MongoDB+Vue.js全栈开发实战'],
        // 设置默认选中项在数组中的索引值。默认值是0
        selected: 1
      }).defaultPickerItemHeight(30)*/

      /*** 以下为 TextTimer 示例 ***/
      // 定义TextTimer组件
      /*TextTimer({ controller: this.textTimerController,
        isCountDown: true, // 是否倒计时。默认值false
        count: 30000 }) // 倒计时时间，单位为毫秒
        .format('mm:ss.SS') // 格式化
        .fontColor(Color.Black) // 字体颜色
        .fontSize(50) // 字体大小

      // 控制按钮
      Row() {
        Button("开始").onClick(() => {
          this.textTimerController.start()
        })
        Button("暂停").onClick(() => {
          this.textTimerController.pause()
        })
        Button("重置").onClick(() => {
          this.textTimerController.reset()
        })
      }*/

      /*** 以下为 TimePicker 示例 ***/
      /*TimePicker()
        .useMilitaryTime(true) // 设置为24小时制*/

      /*** 以下为 Toggle 示例 ***/
      /*// 关闭的Switch类型
      Toggle({ type: ToggleType.Switch, isOn: false })
        .size({ width: 40, height: 40 }) // 设置大小
        .selectedColor('#007DFF') // 设置组件打开状态的背景颜色
        .switchPointColor('#FFFFFF') // 设置Switch类型的圆形滑块颜色

      // 打开的Switch类型
      Toggle({ type: ToggleType.Switch, isOn: true })
        .size({ width: 40, height: 40 }) // 设置大小
        .selectedColor('#007DFF') // 设置组件打开状态的背景颜色
        .switchPointColor('#FFFFFF') // 设置Switch类型的圆形滑块颜色

      // 关闭的Checkbox类型
      Toggle({ type: ToggleType.Checkbox, isOn: false })
        .size({ width: 40, height: 40 }) // 设置大小
        .selectedColor('#007DFF') // 设置组件打开状态的背景颜色

      // 打开的Checkbox类型
      Toggle({ type: ToggleType.Checkbox, isOn: true })
        .size({ width: 40, height: 40 }) // 设置大小
        .selectedColor('#007DFF') // 设置组件打开状态的背景颜色

      // 关闭的Button类型
      Toggle({ type: ToggleType.Button, isOn: false })
        .size({ width: 40, height: 40 }) // 设置大小
        .selectedColor('#007DFF') // 设置组件打开状态的背景颜色

      // 打开的Button类型
      Toggle({ type: ToggleType.Button, isOn: true })
        .size({ width: 40, height: 40 }) // 设置大小
        .selectedColor('#007DFF') // 设置组件打开状态的背景颜色*/

      /*** 以下为 Web 示例 ***/
      //Web({ src: 'https://waylau.com', controller: this.webviewController })

      /*** 以下为 SymbolGlyph 示例 ***/
      /*Row() {
        Column() {
          Text("Light")
          SymbolGlyph($r('sys.symbol.ohos_trash'))
            .fontWeight(FontWeight.Lighter)
            .fontSize(96)
        }

        Column() {
          Text("Normal")
          SymbolGlyph($r('sys.symbol.ohos_trash'))
            .fontWeight(FontWeight.Normal)
            .fontSize(96)
        }

        Column() {
          Text("Bold")
          SymbolGlyph($r('sys.symbol.ohos_trash'))
            .fontWeight(FontWeight.Bold)
            .fontSize(96)
        }
      }

      Row() {
        Column() {
          Text("单色")
          SymbolGlyph($r('sys.symbol.ohos_folder_badge_plus'))
            .fontSize(96)
            .renderingStrategy(SymbolRenderingStrategy.SINGLE)
            .fontColor([Color.Black, Color.Green, Color.White])
        }

        Column() {
          Text("多色")
          SymbolGlyph($r('sys.symbol.ohos_folder_badge_plus'))
            .fontSize(96)
            .renderingStrategy(SymbolRenderingStrategy.MULTIPLE_COLOR)
            .fontColor([Color.Black, Color.Green, Color.White])
        }

        Column() {
          Text("分层")
          SymbolGlyph($r('sys.symbol.ohos_folder_badge_plus'))
            .fontSize(96)
            .renderingStrategy(SymbolRenderingStrategy.MULTIPLE_OPACITY)
            .fontColor([Color.Black, Color.Green, Color.White])
        }
      }

      Row() {
        Column() {
          Text("无动效")
          SymbolGlyph($r('sys.symbol.ohos_wifi'))
            .fontSize(96)
            .effectStrategy(SymbolEffectStrategy.NONE)
        }

        Column() {
          Text("整体缩放动效")
          SymbolGlyph($r('sys.symbol.ohos_wifi'))
            .fontSize(96)
            .effectStrategy(1)
        }

        Column() {
          Text("层级动效")
          SymbolGlyph($r('sys.symbol.ohos_wifi'))
            .fontSize(96)
            .effectStrategy(2)
        }
      }*/
    }
    .height('100%')
  }

  // 自定义一个Toolbar组件
  @Builder NavigationToolbar() {
    Row() {
      Text("首页").fontSize(25).margin({ left: 70 })
      Text("+").fontSize(25).margin({ left: 70 })
      Text("我").fontSize(25).margin({ left: 70 })
    }
  }

  // 网络图片请求方法
  private httpRequest() {
    let httpRequest = http.createHttp();

    httpRequest.request(
      "https://waylau.com/images/showmethemoney-sm.jpg", // 网络图片地址
      (error, data) => {
        if (error) {
          console.log("error code: " + error.code + ", msg: " + error.message)
        } else {
          let code = data.responseCode
          if (http.ResponseCode.OK == code) {
            let resultArrayBuffer = data.result as ArrayBuffer

            let imageSource = image.createImageSource(resultArrayBuffer);
            let options: image.InitializationOptions =
              { editable: true,
                pixelFormat: 3,
                size:
                { height: 281,
                  width: 207
                }
              }
            imageSource.createPixelMap(options).then((pixelMap) => {
              this.imagePixelMap = pixelMap
            })
          } else {
            console.log("response code: " + code);
          }
        }
      }
    )
  }
}

---

## samples/ArkTSImageCodec/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSImageCodec/entry/src/main/ets/pages/Index.ets

// 导入image模块
import image from '@ohos.multimedia.image'
import http from '@ohos.net.http';

@Entry
@Component
struct Index {
  // 先创建一个PixelMap状态变量用于接图片
  @State image: PixelMap = undefined

  build() {
    Row() {
      Column() {
        // 图片
        Image($r('app.media.waylau_181_181'))
          .width(181)
          .height(181)
          .margin({ top: 20, bottom: 20 })


        // 图片
        Image(this.image)
          .width(181)
          .height(181)
          .margin({ top: 20, bottom: 20 })

        Row() {
          // 编码
          Button(('编码'), { type: ButtonType.Capsule })
            .width(140)
            .fontSize(40)
            .fontWeight(FontWeight.Medium)
            .margin({ top: 20, bottom: 20 })
            .onClick(() => {
              this.decode()
            })

          // 解码
          Button(('解码'), { type: ButtonType.Capsule })
            .width(140)
            .fontSize(40)
            .fontWeight(FontWeight.Medium)
            .margin({ top: 20, bottom: 20 })
        }


      }
      .width('100%')
    }
    .height('100%')
  }


  // 解码
  private decode(){
    let uri = 'https://waylau.com/images/waylau_181_181.jpg'; // 设置创建imagesource的路径
    let imageSource = image.createImageSource(uri)
    let options = {alphaType: 0,                     // 透明度
      editable: false,                  // 是否可编辑
      pixelFormat: 3,                   // 像素格式
      scaleMode: 1,                     // 缩略值
      size: {height: 100, width: 100}}  // 创建图片大小
    imageSource.createPixelMap(options).then((pixelMap) => {
      this.image = pixelMap
    })
  }

  private encode(){

  }
}

---

## samples/ArkTSWebComponentHTML/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWebComponentHTML/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSWebComponentHTML/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWebComponentHTML/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSWebComponentHTML/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSWebComponentHTML/entry/src/main/ets/pages/Index.ets

import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}

---

## samples/ArkTSIndicator/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSIndicator/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSIndicator/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSIndicator/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSIndicator/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSIndicator/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State indicatorIndex: number = 0;

  // Indicator组件的控制器，可以将此对象绑定至Indicator组件来控制翻页。
  private indicatorController: IndicatorComponentController = new IndicatorComponentController();

  build() {
    Column() {
      Text(this.indicatorIndex + '')
        .fontSize($r('app.float.page_text_font_size'))
        .fontWeight(FontWeight.Bold)

      IndicatorComponent(this.indicatorController)
        .initialIndex(0) // 设置首次显示时当前导航点的索引值。
        .style( // 可选导航点指示器样式。
          new DotIndicator()
            .itemWidth(25)
            .itemHeight(25)
            .selectedItemWidth(25)
            .selectedItemHeight(25)
            .color(Color.Gray)
            .selectedColor(Color.Blue))
        .loop(true) // 设置是否开启循环
        .count(6) // 设置导航点总数量
        .vertical(false) // 设置是否为纵向滑动
        // 当前显示的选中导航点索引变化时触发该事件，可通过回调函数获取当前选中导航点的索引值。
        .onChange((index: number) => {
          console.log("current index: " + index );

          // 索引赋值给变量indicatorIndex
          this.indicatorIndex = index;
        })
    }
    .height('100%')
    .width('100%')
  }
}

---

## samples/ArkTSWebComponent/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWebComponent/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSWebComponent/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWebComponent/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSWebComponent/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSWebComponent/entry/src/main/ets/pages/Index.ets

// 导入模块
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct Index {

  // 创建WebviewController
  controller: webview.WebviewController = new webview.WebviewController();
  build() {
    Column() {
      // 添加Web组件
      Web({ src: 'https://waylau.com/', controller: this.controller })
    }
  }
}

---

## samples/ArkTSRdb/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSRdb/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSRdb/entry/src/main/ets/viewmodel/AccountData.ets

Source: harmonyos-tutorial/samples/ArkTSRdb/entry/src/main/ets/viewmodel/AccountData.ets

export default class AccountData {
  id: number = -1;
  accountType: number = 0;
  typeText: string = '';
  amount: number = 0;
}

---

## samples/ArkTSRdb/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSRdb/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSRdb/entry/src/main/ets/common/database/Rdb.ets

Source: harmonyos-tutorial/samples/ArkTSRdb/entry/src/main/ets/common/database/Rdb.ets

import { relationalStore } from '@kit.ArkData';
import CommonConstants from '../constants/CommonConstants';

export default class Rdb {
  private rdbStore: relationalStore.RdbStore | null = null;
  private tableName: string;
  private sqlCreateTable: string;
  private columns: Array<string>;

  constructor(tableName: string, sqlCreateTable: string, columns: Array<string>) {
    this.tableName = tableName;
    this.sqlCreateTable = sqlCreateTable;
    this.columns = columns;
  }

  getRdbStore(callback: Function = () => {
  }) {
    if (!callback || typeof callback === 'undefined' || callback === undefined) {
      console.info('getRdbStore() has no callback!');
      return;
    }
    if (this.rdbStore !== null) {
      console.info('The rdbStore exists.');
      callback();
      return
    }
    let context: Context = getContext(this) as Context;
    relationalStore.getRdbStore(context, CommonConstants.STORE_CONFIG, (err, rdb) => {
      if (err) {
        console.error(`gerRdbStore() failed, err: ${err}`);
        return;
      }
      this.rdbStore = rdb;
      this.rdbStore.executeSql(this.sqlCreateTable);
      console.info('getRdbStore() finished.');
      callback();
    });
  }

  insertData(data: relationalStore.ValuesBucket, callback: Function = () => {
  }) {
    if (!callback || typeof callback === 'undefined' || callback === undefined) {
      console.info('insertData() has no callback!');
      return;
    }
    let resFlag: boolean = false;
    const valueBucket: relationalStore.ValuesBucket = data;
    if (this.rdbStore) {
      this.rdbStore.insert(this.tableName, valueBucket, (err, ret) => {
        if (err) {
          console.error(`insertData() failed, err: ${err}`);
          callback(resFlag);
          return;
        }
        console.info(`insertData() finished: ${ret}`);
        callback(ret);
      });
    }
  }

  deleteData(predicates: relationalStore.RdbPredicates, callback: Function = () => {
  }) {
    if (!callback || typeof callback === 'undefined' || callback === undefined) {
      console.info('deleteData() has no callback!');
      return;
    }
    let resFlag: boolean = false;
    if (this.rdbStore) {
      this.rdbStore.delete(predicates, (err, ret) => {
        if (err) {
          console.error(`deleteData() failed, err: ${err}`);
          callback(resFlag);
          return;
        }
        console.info(`deleteData() finished: ${ret}`);
        callback(!resFlag);
      });
    }
  }

  updateData(predicates: relationalStore.RdbPredicates, data: relationalStore.ValuesBucket, callback: Function = () => {
  }) {
    if (!callback || typeof callback === 'undefined' || callback === undefined) {
      console.info('updateDate() has no callback!');
      return;
    }
    let resFlag: boolean = false;
    const valueBucket: relationalStore.ValuesBucket = data;
    if (this.rdbStore) {
      this.rdbStore.update(valueBucket, predicates, (err, ret) => {
        if (err) {
          console.error(`updateData() failed, err: ${err}`);
          callback(resFlag);
          return;
        }
        console.info(`updateData() finished: ${ret}`);
        callback(!resFlag);
      });
    }
  }

  query(predicates: relationalStore.RdbPredicates, callback: Function = () => {
  }) {
    if (!callback || typeof callback === 'undefined' || callback === undefined) {
      console.info('query() has no callback!');
      return;
    }
    if (this.rdbStore) {
      this.rdbStore.query(predicates, this.columns, (err, resultSet) => {
        if (err) {
          console.error(`query() failed, err:  ${err}`);
          return;
        }
        console.info('query() finished.');
        callback(resultSet);
        resultSet.close();
      });
    }
  }
}

---

## samples/ArkTSRdb/entry/src/main/ets/common/database/tables/AccountTable.ets

Source: harmonyos-tutorial/samples/ArkTSRdb/entry/src/main/ets/common/database/tables/AccountTable.ets

import { relationalStore } from '@kit.ArkData';
import AccountData from '../../../viewmodel/AccountData';
import CommonConstants from '../../constants/CommonConstants';
import Rdb from '../Rdb';

export default class AccountTable {
  private accountTable = new Rdb(CommonConstants.ACCOUNT_TABLE.tableName, CommonConstants.ACCOUNT_TABLE.sqlCreate,
    CommonConstants.ACCOUNT_TABLE.columns);

  constructor(callback: Function = () => {
  }) {
    this.accountTable.getRdbStore(callback);
  }

  getRdbStore(callback: Function = () => {
  }) {
    this.accountTable.getRdbStore(callback);
  }

  insertData(account: AccountData, callback: Function) {
    const valueBucket: relationalStore.ValuesBucket = generateBucket(account);
    this.accountTable.insertData(valueBucket, callback);
  }

  deleteData(account: AccountData, callback: Function) {
    let predicates = new relationalStore.RdbPredicates(CommonConstants.ACCOUNT_TABLE.tableName);
    predicates.equalTo('id', account.id);
    this.accountTable.deleteData(predicates, callback);
  }

  updateData(account: AccountData, callback: Function) {
    const valueBucket: relationalStore.ValuesBucket = generateBucket(account);
    let predicates = new relationalStore.RdbPredicates(CommonConstants.ACCOUNT_TABLE.tableName);
    predicates.equalTo('id', account.id);
    this.accountTable.updateData(predicates, valueBucket, callback);
  }

  query(amount: number, callback: Function, isAll: boolean = true) {
    let predicates = new relationalStore.RdbPredicates(CommonConstants.ACCOUNT_TABLE.tableName);
    if (!isAll) {
      predicates.equalTo('amount', amount);
    }
    this.accountTable.query(predicates, (resultSet: relationalStore.ResultSet) => {
      let count: number = resultSet.rowCount;
      if (count === 0 || typeof count === 'string') {
        console.log('Query no results!');
        callback([]);
      } else {
        resultSet.goToFirstRow();
        const result: AccountData[] = [];
        for (let i = 0; i < count; i++) {
          let tmp: AccountData = {
            id: 0, accountType: 0, typeText: '', amount: 0
          };
          tmp.id = resultSet.getDouble(resultSet.getColumnIndex('id'));
          tmp.accountType = resultSet.getDouble(resultSet.getColumnIndex('accountType'));
          tmp.typeText = resultSet.getString(resultSet.getColumnIndex('typeText'));
          tmp.amount = resultSet.getDouble(resultSet.getColumnIndex('amount'));
          result[i] = tmp;
          resultSet.goToNextRow();
        }
        callback(result);
      }
    });
  }
}

function generateBucket(account: AccountData): relationalStore.ValuesBucket {
  let obj: relationalStore.ValuesBucket = {};
  obj.accountType = account.accountType;
  obj.typeText = account.typeText;
  obj.amount = account.amount;
  return obj;
}


---

## samples/ArkTSRdb/entry/src/main/ets/common/constants/CommonConstants.ets

Source: harmonyos-tutorial/samples/ArkTSRdb/entry/src/main/ets/common/constants/CommonConstants.ets

import { relationalStore } from '@kit.ArkData';

export default class CommonConstants {
  /**
   * Rdb database config.
   */
  static readonly STORE_CONFIG: relationalStore.StoreConfig = {
    name: 'database.db',
    securityLevel: relationalStore.SecurityLevel.S1
  };
  /**
   * Account table config.
   */
  static readonly ACCOUNT_TABLE: TableConfig = {
    tableName: 'accountTable',
    sqlCreate: 'CREATE TABLE IF NOT EXISTS accountTable(id INTEGER PRIMARY KEY AUTOINCREMENT, accountType INTEGER, ' +
      'typeText TEXT, amount INTEGER)',
    columns: ['id', 'accountType', 'typeText', 'amount']
  };
}

interface TableConfig {
  tableName: string;
  sqlCreate: string;
  columns: Array<string>;
}

---

## samples/ArkTSRdb/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSRdb/entry/src/main/ets/pages/Index.ets

import AccountTable from '../common/database/tables/AccountTable';
import AccountData from '../viewmodel/AccountData';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'
  private accountTable = new AccountTable();

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        // 增加
        Button(('增加'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            let newAccount: AccountData = { id: 1, accountType: 0, typeText: '苹果', amount: 0 };
            this.accountTable.insertData(newAccount, () => {
            })
          })

        // 查询
        Button(('查询'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            this.accountTable.query(0, (result: AccountData[]) => {
              this.message = JSON.stringify(result);
            }, true);
          })

        // 修改
        Button(('修改'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            let newAccount: AccountData = { id: 1, accountType: 1, typeText: '栗子', amount: 1 };
            this.accountTable.updateData(newAccount, () => {
            })
          })

        // 删除
        Button(('删除'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            let newAccount: AccountData = { id: 1, accountType: 1, typeText: '栗子', amount: 1 };
            this.accountTable.deleteData(newAccount, () => {
            })
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkTSCommonEventService/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSCommonEventService/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSCommonEventService/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSCommonEventService/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSCommonEventService/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSCommonEventService/entry/src/main/ets/pages/Index.ets

// 导入公共事件管理器
import commonEventManager from '@ohos.commonEventManager';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'
  //用于接收事件数据
  @State eventData: string = ''
  // 用于保存创建成功的订阅者对象，后续使用其完成订阅及退订的动作
  private subscriber: commonEventManager.CommonEventSubscriber | null = null;

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(25)
          .fontWeight(FontWeight.Bold)

        // 创建订阅者
        Button(('创建订阅者'), { type: ButtonType.Capsule })
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 10, bottom: 10 })
          .onClick(() => {
            this.createSubscriber()
          })

        // 订阅事件
        Button(('订阅事件'), { type: ButtonType.Capsule })
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 10, bottom: 10 })
          .onClick(() => {
            this.subscriberCommonEvent()
          })

        // 发送事件
        Button(('发送事件'), { type: ButtonType.Capsule })
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 10, bottom: 10 })
          .onClick(() => {
            this.publishCommonEvent()
          })

        // 发送事件
        Button(('取消订阅'), { type: ButtonType.Capsule })
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 10, bottom: 10 })
          .onClick(() => {
            this.unsubscribeCommonEvent()
          })

        // 接收到的事件数据
        Text(this.eventData)
          .fontSize(25)
          .fontWeight(FontWeight.Bold)
      }
      .width('100%')
    }
    .height('100%')
  }

  private createSubscriber() {
    if (this.subscriber) {
      this.message = "subscriber already created";
    } else {
      commonEventManager.createSubscriber({ // 创建订阅者
        events: ["testEvent"] // 指定订阅的事件名称
      }, (err, subscriber) => { // 创建结果的回调
        if (err) {
          this.message = "create subscriber failure"
        } else {
          this.subscriber = subscriber; // 创建订阅成功
          this.message = "create subscriber success";
        }
      })
    }
  }

  private subscriberCommonEvent() {
    if (this.subscriber) {
      // 根据创建的subscriber开始订阅事件
      commonEventManager.subscribe(this.subscriber, (err, data) => {
        if (err) {
          // 异常处理
          this.eventData = "subscribe event failure: " + err;
        } else {
          // 接收到事件
          this.eventData = "subscribe event success: " + JSON.stringify(data);
        }
      })
    } else {
      this.message = "please create subscriber";
    }
  }

  private publishCommonEvent() {
    //发布公共事件
    commonEventManager.publish("testEvent", (err) => { // 结果回调
      if (err) {
        this.message = "publish event error: " + err;
      } else {
        this.message = "publish event with data success";
      }
    })
  }

  private unsubscribeCommonEvent() {
    if (this.subscriber) {
      commonEventManager.unsubscribe(this.subscriber, (err) => { // 取消订阅事件
        if (err) {
          this.message = "unsubscribe event failure: " + err;
        } else {
          this.subscriber = null;
          this.message = "unsubscribe event success";
        }
      })
    } else {
      this.message = "already subscribed";
    }
  }
}

---

## samples/ArkTSNavigation/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSNavigation/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    try {
      this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    } catch (err) {
      hilog.error(DOMAIN, 'testTag', 'Failed to set colorMode. Cause: %{public}s', JSON.stringify(err));
    }
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSNavigation/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSNavigation/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSNavigation/entry/src/main/ets/pages/PageOne.ets

Source: harmonyos-tutorial/samples/ArkTSNavigation/entry/src/main/ets/pages/PageOne.ets

@Builder
export function PageOneBuilder(name: string, param: Object) {
  PageOne()
}

@Component
export struct PageOne {
  @State message: string = '第一页';
  pageInfos: NavPathStack = new NavPathStack();

  build() {
    NavDestination() {
      RelativeContainer() {
        Text(this.message)
          .fontSize($r('app.float.page_text_font_size'))
          .fontWeight(FontWeight.Bold)
          .alignRules({
            center: { anchor: '__container__', align: VerticalAlign.Center },
            middle: { anchor: '__container__', align: HorizontalAlign.Center }
          })
      }
      .height('100%')
      .width('100%')
    }
    .title('PageOne')
    .onReady((context: NavDestinationContext) => {
      this.pageInfos = context.pathStack;
    })
  }
}

---

## samples/ArkTSNavigation/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSNavigation/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = '主页';

  // 创建一个导航控制器对象并传入Navigation
  pageInfos: NavPathStack = new NavPathStack();

  build() {
    Navigation(this.pageInfos) {
      Column() {
        Text(this.message)
          .fontSize($r('app.float.page_text_font_size'))
          .fontWeight(FontWeight.Bold)
          .alignRules({
            center: { anchor: '__container__', align: VerticalAlign.Center },
            middle: { anchor: '__container__', align: HorizontalAlign.Center }
          })

        Button('导航到第一页')
          .width(128).height(28)
          .onClick(() => {
            // 跳转页面时不携带参数
            this.pageInfos.pushPathByName('pageOne', null);
          })

        Button('导航到第二页')
          .width(128).height(28)
          .onClick(() => {
            // 跳转页面时携带参数
            this.pageInfos.pushPathByName('pageTwo', "Page Two");
          })
      }
      .height('100%')
      .width('100%')
    }
    .mode(NavigationMode.Auto)
  }
}

---

## samples/ArkTSNavigation/entry/src/main/ets/pages/PageTwo.ets

Source: harmonyos-tutorial/samples/ArkTSNavigation/entry/src/main/ets/pages/PageTwo.ets

@Builder
export function PageTwoBuilder(name: string, param: Object) {
  PageTwo()
}

@Component
export struct PageTwo {
  @State message: string = '第二页';
  pageInfos: NavPathStack = new NavPathStack();

  build() {
    NavDestination() {
      RelativeContainer() {
        Text(this.message)
          .fontSize($r('app.float.page_text_font_size'))
          .fontWeight(FontWeight.Bold)
          .alignRules({
            center: { anchor: '__container__', align: VerticalAlign.Center },
            middle: { anchor: '__container__', align: HorizontalAlign.Center }
          })
      }
      .height('100%')
      .width('100%')
    }
    .title('PageTwo')
    .onReady((context: NavDestinationContext) => {
      this.pageInfos = context.pathStack
      this.message = context.pathInfo.param as string;
    })
  }
}

---

## samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

// 导入
import { BusinessError } from '@kit.BasicServicesKit';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    // 1.获取应用主窗口。
    let windowClass: window.Window | null = null;
    windowStage.getMainWindow((err: BusinessError, data) => {
      let errCode: number = err.code;
      if (errCode) {
        console.error('Failed to obtain the main window. Cause: ' + JSON.stringify(err));
        return;
      }
      windowClass = data;
      console.info('Succeeded in obtaining the main window. Data: ' + JSON.stringify(data));

      // 2.实现沉浸式效果。方式一：设置导航栏、状态栏不显示。
      let names: Array<'status' | 'navigation'> = [];
      windowClass.setWindowSystemBarEnable(names)
        .then(() => {
          console.info('Succeeded in setting the system bar to be visible.');
        })
        .catch((err: BusinessError) => {
          console.error('Failed to set the system bar to be visible. Cause:' + JSON.stringify(err));
        });
      // 2.实现沉浸式效果。方式二：设置窗口为全屏布局，配合设置导航栏、状态栏的透明度、背景/文字颜色及高亮图标等属性，与主窗口显示保持协调一致。
      let isLayoutFullScreen = true;
      windowClass.setWindowLayoutFullScreen(isLayoutFullScreen)
        .then(() => {
          console.info('Succeeded in setting the window layout to full-screen mode.');
        })
        .catch((err: BusinessError) => {
          console.error('Failed to set the window layout to full-screen mode. Cause:' + JSON.stringify(err));
        });
      let sysBarProps: window.SystemBarProperties = {
        statusBarColor: '#ff00ff',
        navigationBarColor: '#00ff00',
        // 以下两个属性从API 8开始支持
        statusBarContentColor: '#ffffff',
        navigationBarContentColor: '#ffffff'
      };
      windowClass.setWindowSystemBarProperties(sysBarProps)
        .then(() => {
          console.info('Succeeded in setting the system bar properties.');
        })
        .catch((err: BusinessError) => {
          console.error('Failed to set the system bar properties. Cause: ' + JSON.stringify(err));
        });
    })

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSWindowLayoutFullScreen/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('HelloWorld')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
    }
    .height('100%')
    .width('100%')
  }
}

---

## samples/ArkTSPreferences/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSPreferences/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSPreferences/entry/src/main/ets/database/AccountData.ets

Source: harmonyos-tutorial/samples/ArkTSPreferences/entry/src/main/ets/database/AccountData.ets

export default interface AccountData {
  id: number;
  accountType: number;
  typeText: string;
  amount: number;
}

---

## samples/ArkTSPreferences/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSPreferences/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSPreferences/entry/src/main/ets/common/PreferencesUtil.ets

Source: harmonyos-tutorial/samples/ArkTSPreferences/entry/src/main/ets/common/PreferencesUtil.ets

// 导入preferences模块
import { preferences } from '@kit.ArkData';

import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';


let context = getContext(this) as common.UIAbilityContext;
let options: preferences.Options = { name: 'myStore' };

export default class PreferencesUtil {
  private dataPreferences: preferences.Preferences | null = null;

  // 调用getPreferences方法读取指定首选项持久化文件，
  // 将数据加载到Preferences实例，用于数据操作
  async getPreferencesFromStorage() {
    await preferences.getPreferences(context, options).then((data) => {
      this.dataPreferences = data;
      console.info(`Succeeded in getting preferences`);
    }).catch((err: BusinessError) => {
      console.error(`Failed to get preferences, Cause:` + err);
    });
  }

  // 将用户输入的数据，保存到缓存的Preference实例中
  async putPreference(key: string, data: string) {
    if (this.dataPreferences === null) {
      await this.getPreferencesFromStorage();
    }
	
	await this.dataPreferences?.put(key, data).then(() => {
	  console.info(`Succeeded in putting value`);
	}).catch((err: BusinessError) => {
	  console.error(`Failed to get preferences, Cause:` + err);
	});

	// 将Preference实例存储到首选项持久化文件中
	await this.dataPreferences?.flush();
  }

  // 使用Preferences的get方法读取数据
  async getPreference(key: string) {
    let result: string= '';
    if (this.dataPreferences === null) {
      await this.getPreferencesFromStorage();
    } else {
      await this.dataPreferences.get(key, '').then((data) => {
        result = data.toString();
        console.info(`Succeeded in getting value`);
      }).catch((err: BusinessError) => {
        console.error(`Failed to get preferences, Cause:` + err);
      });
    }

    return result;
  }

  // 从内存中移除指定文件对应的Preferences单实例。
  // 移除Preferences单实例时，应用不允许再使用该实例进行数据操作，否则会出现数据一致性问题。
  async deletePreferences() {

    preferences.deletePreferences(context, options, (err: BusinessError) => {
      if (err) {
        console.error(`Failed to delete preferences. Code:${err.code}, message:${err.message}`);
        return;
      }

      this.dataPreferences = null;
      console.info('Succeeded in deleting preferences.');
    })
  }
}


---

## samples/ArkTSPreferences/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSPreferences/entry/src/main/ets/pages/Index.ets

// 导入PreferencesUtil
import PreferencesUtil from '../common/PreferencesUtil';
// 导入AccountData
import AccountData from '../database/AccountData';

const PREFERENCES_KEY = 'fruit';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'
  private preferencesUtil = new PreferencesUtil();

  async aboutToAppear() {
    // 初始化首选项
    await this.preferencesUtil.getPreferencesFromStorage();

    // 获取结果
    this.preferencesUtil.getPreference(PREFERENCES_KEY).then(resultData => {
      this.message = resultData;
    });
  }

  build() {
    Row() {
      Column() {
        Text(this.message)
          .id('text_result')
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        // 增加
        Button(('增加'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            // 保存数据
            let newAccount: AccountData = { id: 1, accountType: 0, typeText: '苹果', amount: 0 };
            this.preferencesUtil.putPreference(PREFERENCES_KEY, JSON.stringify(newAccount));
          })

        // 查询
        Button(('查询'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            // 获取结果
            this.preferencesUtil.getPreference(PREFERENCES_KEY).then(resultData => {
              this.message = resultData;
            });
          })

        // 修改
        Button(('修改'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            // 修改数据
            let newAccount: AccountData = { id: 1, accountType: 1, typeText: '栗子', amount: 1 };
            this.preferencesUtil.putPreference(PREFERENCES_KEY, JSON.stringify(newAccount));
          })

        // 删除
        Button(('删除'), { type: ButtonType.Capsule })
          .width(140)
          .fontSize(40)
          .fontWeight(FontWeight.Medium)
          .margin({ top: 20, bottom: 20 })
          .onClick(() => {
            this.preferencesUtil.deletePreferences();
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkTSSVGChineseLoong/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSSVGChineseLoong/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = '画龙迎春，“码”上“鸿”福到';
  @State fillColor: Color = Color.Black;

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(26)
          .fontWeight(FontWeight.Bold)
        Image($r('app.media.chineseloong'))
          .height(390)
          .width(330)
          .fillColor(this.fillColor)
        Button('画龙')
          .onClick(()=>{
            // 点击变化颜色
            if (this.fillColor == Color.Black) {
              this.fillColor = Color.Red;
            } else if (this.fillColor == Color.Red) {
            this.fillColor = Color.Blue;
            }else if (this.fillColor == Color.Blue) {
              this.fillColor = Color.Orange;
            }else if (this.fillColor == Color.Orange) {
              this.fillColor = Color.Pink;
            }else if (this.fillColor == Color.Pink) {
              this.fillColor = Color.Black;
            }
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}




---

## samples/ArkTSWantOpenSetting/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWantOpenSetting/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}


---

## samples/ArkTSWantOpenSetting/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWantOpenSetting/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(0x0000, 'testTag', 'onBackup ok');
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(0x0000, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
  }
}

---

## samples/ArkTSWantOpenSetting/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSWantOpenSetting/entry/src/main/ets/pages/Index.ets

// 导入common、Want
import { common, Want } from '@kit.AbilityKit';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('HelloWorld')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
        .onClick(this.explicitStartAbility) // 设置点击事件，显示启动Ability
    }
    .height('100%')
    .width('100%')
  }

  // 显示启动Ability
  explicitStartAbility() {
    try {
      // 在启动Ability时指定了abilityName和bundleName
      let want: Want  = {
        deviceId: '',
        bundleName: 'com.huawei.hmos.settings',
        abilityName: 'com.huawei.hmos.settings.MainAbility'
      };

      // 获取UIAbility的上下文信息
      let context = getContext(this) as common.UIAbilityContext;

      // 启动UIAbility实例
      context.startAbility(want);
      console.info(`explicit start ability succeed`);
    } catch (error) {
      console.info(`explicit start ability failed with ${error.code}`);
    }
  }
}

---

## samples/ArkTSHelloWorld/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSHelloWorld/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    try {
      this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    } catch (err) {
      hilog.error(DOMAIN, 'testTag', 'Failed to set colorMode. Cause: %{public}s', JSON.stringify(err));
    }
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSHelloWorld/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSHelloWorld/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSHelloWorld/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSHelloWorld/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    RelativeContainer() {
      Text(this.message)
        .id('HelloWorld')
        .fontSize($r('app.float.page_text_font_size'))
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
        .onClick(() => {
          this.message = 'Welcome';
        })
    }
    .height('100%')
    .width('100%')
  }
}

---

## samples/ArkTSCPIChart/entry/src/main/ets/model/CPIData.ets

Source: harmonyos-tutorial/samples/ArkTSCPIChart/entry/src/main/ets/model/CPIData.ets

export class CPIData {
  id: number
  month: string
  data: number

  constructor(id: number, month: string, data: number) {
    this.id = id // 唯一表示
    this.month = month // 月份
    this.data = data // CPI数值
  }
}

---

## samples/ArkTSCPIChart/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSCPIChart/entry/src/main/ets/pages/Index.ets

import { CPIData } from '../model/CPIData';
 
@Entry
@Component
struct Index {
  @State cpiData: Array<CPIData> = [
    { id: 1,    month: '2023年08月份',    data: 100.1 },
    { id: 2,    month: '2023年07月份',    data: 99.7 },
    { id: 3,    month: '2023年06月份',    data: 100 },
    { id: 4,    month: '2023年05月份',    data: 100.2 },
    { id: 5,    month: '2023年04月份',    data: 100.1 },
    { id: 6,    month: '2023年03月份',    data: 100.7 },
    { id: 7,    month: '2023年02月份',    data: 101 },
    { id: 8,    month: '2023年01月份',    data: 102.1 }
  ];
 
  build() {
 
  }
}

---

## samples/ArkTSAtomicService/entry/src/main/ets/widget/pages/WidgetCard.ets

Source: harmonyos-tutorial/samples/ArkTSAtomicService/entry/src/main/ets/widget/pages/WidgetCard.ets

@Entry
@Component
struct WidgetCard {
  /*
   * The max lines.
   */
  readonly MAX_LINES: number = 1;

  /*
   * The action type.
   */
  readonly ACTION_TYPE: string = 'router';

  /*
   * The message.
   */
  readonly MESSAGE: string = 'add detail';

  /*
   * The ability name.
   */
  readonly ABILITY_NAME: string = 'EntryAbility';

  /*
   * The with percentage setting.
   */
  readonly FULL_WIDTH_PERCENT: string = '100%';

  /*
   * The height percentage setting.
   */
  readonly FULL_HEIGHT_PERCENT: string = '100%';

  build() {
    Stack() {
      Image($r("app.media.ic_widget"))
        .width(this.FULL_WIDTH_PERCENT)
        .height(this.FULL_HEIGHT_PERCENT)
        .objectFit(ImageFit.Cover)
      Column() {
        Text($r('app.string.title_immersive'))
          .fontSize($r('app.float.title_immersive_font_size'))
          .textOverflow({ overflow: TextOverflow.Ellipsis })
          .fontColor($r('app.color.text_font_color'))
          .maxLines(this.MAX_LINES)
        Text($r('app.string.detail_immersive'))
          .fontSize($r('app.float.detail_immersive_font_size'))
          .opacity($r('app.float.detail_immersive_opacity'))
          .margin({ top: $r('app.float.detail_immersive_margin_top') })
          .textOverflow({ overflow: TextOverflow.Ellipsis })
          .fontColor($r('app.color.text_font_color'))
          .maxLines(this.MAX_LINES)
      }
      .width(this.FULL_WIDTH_PERCENT)
      .height(this.FULL_HEIGHT_PERCENT)
      .alignItems(HorizontalAlign.Start)
      .justifyContent(FlexAlign.End)
      .padding($r('app.float.column_padding'))
    }
    .width(this.FULL_WIDTH_PERCENT)
    .height(this.FULL_HEIGHT_PERCENT)
    .onClick(() => {
      postCardAction(this, {
        "action": this.ACTION_TYPE,
        "abilityName": this.ABILITY_NAME,
        "params": {
          "message": this.MESSAGE
        }
      });
    })
  }
}

---

## samples/ArkTSAtomicService/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSAtomicService/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  @State message: string = 'Hello'
  @State messageService: string = 'Atomic Service'

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)

        Text(this.messageService)
          .fontSize(35)
          .fontWeight(Color.Orange)
      }
      .width('100%')
    }
    .height('100%')
  }
}

---

## samples/ArkTSSwiperAnimationMode/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSSwiperAnimationMode/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSSwiperAnimationMode/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSSwiperAnimationMode/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSSwiperAnimationMode/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSSwiperAnimationMode/entry/src/main/ets/pages/Index.ets

@Entry
@Component
struct Index {
  private swiperController: SwiperController = new SwiperController();
  private data: MyDataSource = new MyDataSource([]);

  aboutToAppear(): void {
    let list: number[] = [];
    for (let i = 0; i <= 20; i++) {
      list.push(i);
    }
    this.data = new MyDataSource(list);
  }

  build() {
    Column({ space: 5 }) {
      Swiper(this.swiperController) {
        LazyForEach(this.data, (item: string) => {
          Text(item.toString())
            .width('90%')
            .height(360)
            .backgroundColor(0xAFEEEE)
            .textAlign(TextAlign.Center)
            .fontSize(30)
        }, (item: string) => item)
      }
      .cachedCount(2)
      .index(1)
      .autoPlay(false)
      .interval(4000)
      .loop(false)
      .duration(1000)
      .itemSpace(5)
      .prevMargin(35)
      .nextMargin(35)
      .onChange((index: number) => {
        console.info(index.toString());
      })
      .onAnimationStart((index: number, targetIndex: number, extraInfo: SwiperAnimationEvent) => {
        console.info("index: " + index);
        console.info("targetIndex: " + targetIndex);
        console.info("current offset: " + extraInfo.currentOffset);
        console.info("target offset: " + extraInfo.targetOffset);
        console.info("velocity: " + extraInfo.velocity);
      })
      .onAnimationEnd((index: number, extraInfo: SwiperAnimationEvent) => {
        console.info("index: " + index);
        console.info("current offset: " + extraInfo.currentOffset);
      })

      Column({ space: 5 }) {
        Button('NO_ANIMATION 0')
          .onClick(() => {
            this.swiperController.changeIndex(0, SwiperAnimationMode.NO_ANIMATION);
          })
        Button('DEFAULT_ANIMATION 10')
          .onClick(() => {
            this.swiperController.changeIndex(10, SwiperAnimationMode.DEFAULT_ANIMATION);
          })
        Button('FAST_ANIMATION 20')
          .onClick(() => {
            this.swiperController.changeIndex(20, SwiperAnimationMode.FAST_ANIMATION);
          })
      }.margin(5)
    }.width('100%')
    .margin({ top: 5 })
  }
}

// 该类主要是用于提供Swiper组件的数据源。
class MyDataSource implements IDataSource {
  private list: number[] = [];

  constructor(list: number[]) {
    this.list = list;
  }

  totalCount(): number {
    return this.list.length;
  }

  getData(index: number): number {
    return this.list[index];
  }

  registerDataChangeListener(listener: DataChangeListener): void {
  }

  unregisterDataChangeListener() {
  }
}

---

## samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/entryability/EntryAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/entryability/EntryAbility.ets

import { AbilityConstant, ConfigurationConstant, UIAbility, Want } from '@kit.AbilityKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { window } from '@kit.ArkUI';

const DOMAIN = 0x0000;

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    try {
      this.context.getApplicationContext().setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_NOT_SET);
    } catch (err) {
      hilog.error(DOMAIN, 'testTag', 'Failed to set colorMode. Cause: %{public}s', JSON.stringify(err));
    }
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onCreate');
  }

  onDestroy(): void {
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onDestroy');
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onBackground');
  }
}

---

## samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

Source: harmonyos-tutorial/samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/entrybackupability/EntryBackupAbility.ets

import { hilog } from '@kit.PerformanceAnalysisKit';
import { BackupExtensionAbility, BundleVersion } from '@kit.CoreFileKit';

const DOMAIN = 0x0000;

export default class EntryBackupAbility extends BackupExtensionAbility {
  async onBackup() {
    hilog.info(DOMAIN, 'testTag', 'onBackup ok');
    await Promise.resolve();
  }

  async onRestore(bundleVersion: BundleVersion) {
    hilog.info(DOMAIN, 'testTag', 'onRestore ok %{public}s', JSON.stringify(bundleVersion));
    await Promise.resolve();
  }
}

---

## samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/pages/Index.ets

Source: harmonyos-tutorial/samples/ArkTSWifiManagerConnectToWifi/entry/src/main/ets/pages/Index.ets

import { wifiManager }  from '@kit.ConnectivityKit';

@Entry
@Component
struct Index {
  @State ssid: string = ''
  @State securityType: string = ''
  @State password: string = ''


  build() {
    Column() {

      Row() {
        Text('WiFi名称：')
        TextInput({ text: this.ssid })
          .width('80%')
          .onChange((value) => {
            this.ssid = value
          })
      }.margin(10)

      Row() {
        Text('密码：')
        TextInput({ text: this.password })
          .type(InputType.Password)
          .width('80%')
          .onChange((value) => {
            this.password = value
          })
      }.margin(10)

      // 操作按钮
      Row() {
        Button('连接该网络')
          .type(ButtonType.Capsule)
          .fontColor(Color.White)
          .margin(10)
          .onClick(() => {
            // 连接到WiFi
            this.connectToWifi()
          })

      }
      .width('100%')
      .height('10%')
      .justifyContent(FlexAlign.Center)
    }
    .height('100%')
    .width('100%')
    .justifyContent(FlexAlign.Center)
  }

  // 连接到WiFi
  private connectToWifi() {

    let config:wifiManager.WifiDeviceConfig = {
      ssid : this.ssid,
      preSharedKey : this.password,
      securityType : 3
    }

    console.info('config:' + JSON.stringify(config));

    // 添加候选网络配置
    wifiManager.addCandidateConfig(config).then(result => {
      // 连接至网络
      console.info('addCandidateConfig result:' + JSON.stringify(result));

      wifiManager.connectToCandidateConfig(result);
    }).catch((err: number) => {
      console.error('failed:' + JSON.stringify(err));
    });

  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/viewmodel/HomeVideoListModel.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/viewmodel/HomeVideoListModel.ets


import image from '@ohos.multimedia.image';
import { VideoBean } from '../common/bean/VideoBean';
import { VIDEO_DATA } from '../common/constants/CommonConstants';

export class HomeVideoListModel {
  private videoLocalList: Array<VideoBean> = [];
  private videoInternetList: Array<VideoBean> = [];

  /**
   * Scan the local video.
   *
   * @return Local video list data
   */
  async getLocalVideo() {
    this.videoLocalList = [];
    await this.assemblingVideoBean();
    globalThis.videoLocalList = this.videoLocalList;
    return this.videoLocalList;
  }

  /**
   * Assembling the video object
   */
  async assemblingVideoBean () {
    VIDEO_DATA.forEach(async (item: VideoBean) => {
      let videoBean =  await globalThis.resourceManager.getRawFd(item.src);
      let uri = `fd://${videoBean.fd}`;
      this.videoLocalList.push(new VideoBean(item.name, uri));
    });
  }

  /**
   * Scan the internet video.
   *
   * @param name Video Name.
   * @param pixelMap pixelMap object.
   * @param src Playback Path.
   * @param duration Video duration.
   * @return Network video list data.
   */
  async setInternetVideo(name: string, src: string, pixelMap?: image.PixelMap) {
    this.videoInternetList.push(new VideoBean(name, src, pixelMap));
    globalThis.videoInternetList = this.videoInternetList;
    return globalThis.videoInternetList;
  }
}

let homeVideoListModel = new HomeVideoListModel();
export default homeVideoListModel as HomeVideoListModel;

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/viewmodel/HomeDialogModel.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/viewmodel/HomeDialogModel.ets


import prompt from '@ohos.promptAction';
import media from '@ohos.multimedia.media';
import Logger from '../common/util/Logger';
import { HomeConstants } from '../common/constants/HomeConstants';
import { AvplayerStatus, Events } from '../common/constants/CommonConstants';

export class HomeDialogModel {
  private context;
  private avPlayer;
  private url;
  private duration;
  private checkFlag;
  private isLoading;

  constructor(context) {
    this.context = context;
    this.isLoading = false;
  }

  /**
   * Creates a videoPlayer object.
   */
  createAvPlayer() {
    media.createAVPlayer().then((video) => {
      if (video != null) {
        this.avPlayer = video;
        this.bindState();
        this.url = this.context.src;
        this.avPlayer.url = this.url;
      } else {
        Logger.info(`[HomeDialogModel] createAVPlayer fail`);
      }
    }).catch((err) => {
      this.failureCallback(err);
    });
  }

  bindState() {
    this.avPlayer.on(Events.STATE_CHANGE, async (state) => {
      switch (state) {
        case AvplayerStatus.IDLE:
          break;
        case AvplayerStatus.INITIALIZED:
          this.avPlayer.prepare();
          break;
        case AvplayerStatus.PREPARED:
          this.duration = this.avPlayer.duration;
          this.checkUrlValidity();
          break;
        case AvplayerStatus.RELEASED:
          break;
        case Events.ERROR:
          this.avPlayer.reset();
          break;
        default:
          break;
      }
    });
    this.avPlayer.on(Events.ERROR, (error) => {
      this.isLoading = false;
      this.context.linkCheck = $r('app.string.link_check');
      this.context.loadColor = $r('app.color.index_tab_selected_font_color');
      this.avPlayer.release();
      this.failureCallback(error);
    })
  }

  /**
   * Verifying Network Connections.
   *
   * @param checkFlag Determine whether to verify or add data.
   */
  async checkSrcValidity(checkFlag: number) {
    if (this.isLoading) {
      return;
    }
    this.isLoading = true;
    this.context.linkCheck = $r('app.string.link_checking');
    this.context.loadColor = $r('app.color.index_tab_unselected_font_color');
    this.checkFlag = checkFlag;
    this.createAvPlayer();
  }

  /**
   * Verifying Network Connections.
   */
  checkUrlValidity() {
    this.isLoading = false;
    this.context.linkCheck = $r('app.string.link_check');
    this.context.loadColor = $r('app.color.index_tab_selected_font_color');
    this.avPlayer.release();
    if (this.duration === HomeConstants.INTERNET_ADD_DIALOG.DURATION_TWO) {
      // Failed to verify the link
      this.showPrompt($r('app.string.link_check_fail'));
      this.context.result = false;
    } else if (this.duration === HomeConstants.INTERNET_ADD_DIALOG.DURATION_ONE) {
      // The address is incorrect or no network is available
      this.showPrompt($r('app.string.link_check_address_internet'));
      this.context.result = false;
    } else {
      this.duration = 0;
      if (this.checkFlag === 0) {
        this.showPrompt($r('app.string.link_check_success'));
      } else {
        this.context.confirm();
        this.context.controller.close();
      }
    }
  }

  /**
   * This parameter is used to report error information when an error occurs in function invoking.
   *
   * @param error Error information.
   */
  failureCallback(error) {
    Logger.error(`[HomeDialogModel] error happened: ` + JSON.stringify(error));
    this.context.result = false;
    this.showPrompt($r('app.string.link_check_fail'));
  }

  /**
   * Prompt dialog box.
   *
   * @param msg Verification Information.
   */
  showPrompt(msg: Resource) {
    prompt.showToast({
      duration: HomeConstants.INTERNET_ADD_DIALOG.DURATION,
      message: msg
    });
  }

  /**
   * Check whether the playback path is empty.
   */
  checkSrcNull() {
    if (this.isLoading) {
      return;
    }
    if (this.context.src.trim() === '') {
      prompt.showToast({
        duration: HomeConstants.INTERNET_ADD_DIALOG.DURATION,
        message: $r('app.string.place_holder_src')
      });
      return false;
    }
    return true;
  }

  /**
   * Check whether the name is empty.
   */
  checkNameNull() {
    if (this.isLoading) {
      return;
    }
    if (this.context.name.trim() === '') {
      prompt.showToast({
        duration: HomeConstants.INTERNET_ADD_DIALOG.DURATION,
        message: $r('app.string.place_holder_name')
      });
      return false;
    }
    return true;
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/controller/VideoController.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/controller/VideoController.ets


import media from '@ohos.multimedia.media';
import prompt from '@ohos.promptAction';
import Logger from '../common/util/Logger';
import DateFormatUtil from '../common/util/DateFormatUtil';
import { CommonConstants, AvplayerStatus, Events } from '../common/constants/CommonConstants';
import { PlayConstants } from '../common/constants/PlayConstants';

@Observed
export class VideoController {
  private avPlayer;
  private duration: number = 0;
  private status: number;
  private loop: boolean = false;
  private index: number;
  private url: string;
  private surfaceId: number;
  private playSpeed: number = PlayConstants.PLAY_PAGE.PLAY_SPEED;
  private seekTime: number = PlayConstants.PLAY_PROGRESS.SEEK_TIME;
  private progressThis;
  private playerThis;
  private playPageThis;
  private titleThis;
  private positionX: number = PlayConstants.PLAY_PAGE.POSITION_X;
  private positionY: number = PlayConstants.PLAY_PAGE.POSITION_Y;

  constructor() {
    this.createAVPlayer();
  }

  /**
   * 创建 videoPlayer对象
   */
  createAVPlayer() {
    media.createAVPlayer().then((avPlayer) => {
      if (avPlayer) {
        this.avPlayer = avPlayer;
        this.bindState();
      } else {
        Logger.error('[PlayVideoModel] createAvPlayer fail!');
      }
    });
  }

  /**
   * AVPlayer 绑定事件.
   */
  bindState() {
    this.avPlayer.on(Events.STATE_CHANGE, async (state) => {
      switch (state) {
        case AvplayerStatus.IDLE:
          this.resetProgress();
          this.avPlayer.url = this.url;
          break;
        case AvplayerStatus.INITIALIZED:
          this.avPlayer.surfaceId = this.surfaceId;
          this.avPlayer.prepare();
          break;
        case AvplayerStatus.PREPARED:
          this.avPlayer.videoScaleType = 0;
          this.setVideoSize();
          this.avPlayer.play();
          this.duration = this.avPlayer.duration;
          break;
        case AvplayerStatus.PLAYING:
          this.avPlayer.setVolume(this.playerThis.volume);
          this.setBright();
          this.status = CommonConstants.STATUS_START;
          this.watchStatus();
          break;
        case AvplayerStatus.PAUSED:
          this.status = CommonConstants.STATUS_PAUSE;
          this.watchStatus();
          break;
        case AvplayerStatus.COMPLETED:
          this.titleThis.playSpeed = 1;
          this.duration = PlayConstants.PLAY_PLAYER.DURATION;
          if (!this.loop) {
            let curIndex = this.index + PlayConstants.PLAY_PLAYER.NEXT;
            this.index = (curIndex === globalThis.videoList.length) ?
            PlayConstants.PLAY_PLAYER.FIRST : curIndex;
            this.url = globalThis.videoList[this.index].src;
          } else {
            this.url = this.avPlayer.url;
          }
          this.avPlayer.reset();
          break;
        case AvplayerStatus.RELEASED:
          this.avPlayer.release();
          this.status = CommonConstants.STATUS_STOP;
          this.watchStatus();
          Logger.info('[PlayVideoModel] state released called')
          break;
        default:
          Logger.info('[PlayVideoModel] unKnown state: ' + state);
          break;
      }
    });
    this.avPlayer.on(Events.TIME_UPDATE, (time: number) => {
      this.initProgress(time);
    });
    this.avPlayer.on(Events.ERROR, (error) => {
      this.playError();
    })
  }

  /**
   * This method is triggered when the video playback page is displayed on the video list page.
   *
   * @param index Video object subscript in the video list.
   * @param url Playback Path.
   * @param surfaceId Indicates the surface ID of the surfaceId.
   */
  firstPlay(index: number, url: string, surfaceId: number) {
    this.index = index;
    this.url = url;
    this.surfaceId = surfaceId;
    this.avPlayer.url = this.url;
  }

  /**
   * Release the video player.
   */
  release() {
    this.avPlayer.release();
  }

  /**
   * Pause Playing.
   */
  pause() {
    this.avPlayer.pause();
  }

  /**
   * Playback mode. The options are as follows: true: playing a single video; false: playing a cyclic video.
   */
  setLoop() {
    this.loop = !this.loop;
  }

  /**
   * Set the playback speed.
   *
   * @param playSpeed Current playback speed.
   */
  setSpeed(playSpeed: number) {
    if (CommonConstants.OPERATE_STATE.indexOf(this.avPlayer.state) === -1) {
      return;
    }
    this.playSpeed = playSpeed;
    this.avPlayer.setSpeed(this.playSpeed);
  }

  /**
   * Previous video.
   */
  previousVideo() {
    if (CommonConstants.OPERATE_STATE.indexOf(this.avPlayer.state) === -1) {
      return;
    }
    this.titleThis.playSpeed = 1;
    let curIndex = this.index - PlayConstants.PLAY_CONTROL.NEXT;
    this.index = (curIndex === -PlayConstants.PLAY_CONTROL.NEXT) ?
      (globalThis.videoList.length - PlayConstants.PLAY_CONTROL.NEXT) : curIndex;
    this.url = globalThis.videoList[this.index].src;
    this.avPlayer.reset();
  }

  /**
   * Next video.
   */
  nextVideo() {
    if (CommonConstants.OPERATE_STATE.indexOf(this.avPlayer.state) === -1) {
      return;
    }
    this.titleThis.playSpeed = 1;
    let curIndex = this.index + PlayConstants.PLAY_CONTROL.NEXT;
    this.index = (curIndex === globalThis.videoList.length) ?
    PlayConstants.PLAY_CONTROL.FIRST : curIndex;
    this.url = globalThis.videoList[this.index].src;
    this.avPlayer.reset();
  }

  /**
   * Switching Between Video Play and Pause.
   */
  switchPlayOrPause() {
    if (this.status === CommonConstants.STATUS_START) {
      this.avPlayer.pause();
    } else {
      this.avPlayer.play();
    }
  }

  /**
   * Slide the progress bar to set the playback progress.
   *
   * @param value Value of the slider component.
   * @param mode Slider component change event.
   */
  setSeekTime(value: number, mode: SliderChangeMode) {
    if (mode === SliderChangeMode.Moving) {
      // The current time is changed during dragging, and other parameters remain unchanged.
      this.progressThis.progressVal = value;
      this.progressThis.currentTime = DateFormatUtil.secondToTime(Math.floor(value * this.duration /
      CommonConstants.ONE_HUNDRED / CommonConstants.A_THOUSAND));
    }
    if (mode === SliderChangeMode.End) {
      this.seekTime = value * this.duration / CommonConstants.ONE_HUNDRED;
      this.avPlayer.seek(this.seekTime, media.SeekMode.SEEK_PREV_SYNC);
    }
  }

  /**
   * Setting the brightness.
   */
  setBright() {
    globalThis.windowClass.setWindowBrightness(this.playerThis.bright)
  }

  /**
   * Obtains the current video playing status.
   */
  getStatus() {
    return this.status;
  }

  /**
   * Transfer this on the playProgress page.
   *
   * @param progressThis PlayProgress this object.
   */
  initProgressThis(progressThis) {
    this.progressThis = progressThis;
  }

  /**
   * Transfer this on the playTitle page.
   *
   * @param progressThis PlayTitle this object.
   */
  initTitleThis(titleThis) {
    this.titleThis = titleThis;
  }

  /**
   * Pass this on the playback page.
   *
   * @param playerThis PlayPlayer this object.
   */
  initPlayerThis(playerThis) {
    this.playerThis = playerThis;
  }

  /**
   * Initialization progress bar.
   *
   * @param time Current video playback time.
   */
  initProgress(time: number) {
    let nowSeconds = Math.floor(time / CommonConstants.A_THOUSAND);
    let totalSeconds = Math.floor(this.duration / CommonConstants.A_THOUSAND);
    this.progressThis.currentTime = DateFormatUtil.secondToTime(nowSeconds);
    this.progressThis.totalTime = DateFormatUtil.secondToTime(totalSeconds);
    this.progressThis.progressVal = Math.floor(nowSeconds * CommonConstants.ONE_HUNDRED / totalSeconds);
  }

  /**
   * Reset progress bar data.
   */
  resetProgress() {
    this.seekTime = PlayConstants.PLAY_PROGRESS.SEEK_TIME;
    this.progressThis.currentTime = PlayConstants.PLAY_PROGRESS.CURRENT_TIME;
    this.progressThis.progressVal = PlayConstants.PLAY_PROGRESS.PROGRESS_VAL;
  }

  /**
   * Volume gesture method onActionStart.
   *
   * @param event Gesture event.
   */
  onVolumeActionStart(event: GestureEvent) {
    this.positionX = event.offsetX;
  }

  /**
   * Bright gesture method onActionStart.
   *
   * @param event Gesture event.
   */
  onBrightActionStart(event: GestureEvent) {
    this.positionY = event.offsetY;
  }

  /**
   * Gesture method onActionUpdate.
   *
   * @param event Gesture event.
   */
  onVolumeActionUpdate(event: GestureEvent) {
    if (CommonConstants.OPERATE_STATE.indexOf(this.avPlayer.state) === -1) {
      return;
    }
    if (!this.playerThis.brightShow) {
      this.playerThis.volumeShow = true;
      let changeVolume = (event.offsetX - this.positionX) / globalThis.screenWidth;
      let currentVolume = this.playerThis.volume + changeVolume;
      let volumeMinFlag = currentVolume <= PlayConstants.PLAY_PAGE.MIN_VALUE;
      let volumeMaxFlag = currentVolume > PlayConstants.PLAY_PAGE.MAX_VALUE;
      this.playerThis.volume = volumeMinFlag ? PlayConstants.PLAY_PAGE.MIN_VALUE :
        (volumeMaxFlag ? PlayConstants.PLAY_PAGE.MAX_VALUE : currentVolume);
      this.avPlayer.setVolume(this.playerThis.volume);
      this.positionX = event.offsetX;
    }
  }
  /**
   * Gesture method onActionUpdate.
   *
   * @param event Gesture event.
   */
  onBrightActionUpdate(event: GestureEvent) {
    if (!this.playerThis.volumeShow) {
      this.playerThis.brightShow = true;
      let changeBright = (this.positionY - event.offsetY) / globalThis.screenHeight;
      let currentBright = this.playerThis.bright + changeBright;
      let brightMinFlag = currentBright <= PlayConstants.PLAY_PAGE.MIN_VALUE;
      let brightMaxFlag = currentBright > PlayConstants.PLAY_PAGE.MAX_VALUE;
      this.playerThis.bright = brightMinFlag ? PlayConstants.PLAY_PAGE.MIN_VALUE :
        (brightMaxFlag ? PlayConstants.PLAY_PAGE.MAX_VALUE : currentBright);
      this.setBright();
      this.positionY = event.offsetY;
    }
  }

  /**
   * Gesture method onActionEnd.
   */
  onActionEnd() {
    setTimeout(() => {
      this.playerThis.volumeShow = false;
      this.playerThis.brightShow = false;
      this.positionX = PlayConstants.PLAY_PAGE.POSITION_X;
      this.positionY = PlayConstants.PLAY_PAGE.POSITION_Y;
    }, PlayConstants.PLAY_PAGE.DISAPPEAR_TIME);
  }

  /**
   * Sets whether the screen is a constant based on the playback status.
   */
  watchStatus() {
    if (this.status === CommonConstants.STATUS_START) {
      globalThis.windowClass.setWindowKeepScreenOn(true);
    } else {
      globalThis.windowClass.setWindowKeepScreenOn(false);
    }
  }

  /**
   * Sets the playback page size based on the video size.
   */
  setVideoSize() {
    if (this.avPlayer.height > this.avPlayer.width) {
      this.playPageThis.videoWidth = PlayConstants.PLAY_PAGE.PLAY_PLAYER_HEIGHT_FULL;
      this.playPageThis.videoHeight = PlayConstants.PLAY_PAGE.PLAY_PLAYER_HEIGHT_FULL;
      this.playPageThis.videoPosition = FlexAlign.Start;
      this.playPageThis.videoMargin = PlayConstants.PLAY_PAGE.HEIGHT;
    } else {
      this.playPageThis.videoWidth = CommonConstants.FULL_PERCENT;
      this.playPageThis.videoHeight = PlayConstants.PLAY_PAGE.PLAY_PLAYER_HEIGHT;
      this.playPageThis.videoPosition = FlexAlign.Center;
      this.playPageThis.videoMargin =  PlayConstants.PLAY_PAGE.MARGIN_ZERO;
    }
  }

  /**
   * Obtains the this object of the PlayPage.
   *
   * @param playPageThis This object of PlayPage.
   */
  initPlayPageThis(playPageThis) {
    this.playPageThis = playPageThis;
  }

  /**
   * An error is reported during network video playback.
   */
  playError() {
    prompt.showToast({
      duration: PlayConstants.PLAY_ERROR_TIME,
      message: $r('app.string.link_check_address_internet')
    });
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/ScreenUtil.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/ScreenUtil.ets

import Window from '@ohos.window';
import Logger from '../util/Logger';

class ScreenUtil {
  setScreenSize(): void {
    Window.getLastWindow(getContext(this))
      .then((windowClass) => {
        globalThis.screenWidth = px2fp(windowClass.getWindowProperties().windowRect.width);
        globalThis.screenHeight = px2fp(windowClass.getWindowProperties().windowRect.height);
        globalThis.windowClass = windowClass;
      })
      .catch((error) => {
        Logger.error('[ScreenUtil] Failed to obtain the window size. Cause: ' + JSON.stringify(error));
      })
  }
}

export default new ScreenUtil();


---

## samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/DateFormatUtil.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/DateFormatUtil.ets

import { CommonConstants } from '../constants/CommonConstants'

class DateFormatUtil {
  /**
   * Seconds converted to HH:mm:ss.
   *
   * @param seconds Maximum video duration (seconds).
   * @return Time after conversion.
   */
  secondToTime(seconds: number) {
    let time = `${CommonConstants.INITIAL_TIME_UNIT}${':'}${CommonConstants.INITIAL_TIME_UNIT}`;
    let hourUnit = CommonConstants.TIME_UNIT * CommonConstants.TIME_UNIT;
    let hour = Math.floor(seconds / hourUnit);
    let minute = Math.floor((seconds - hour * hourUnit) / CommonConstants.TIME_UNIT);
    let second = seconds - hour * hourUnit - minute * CommonConstants.TIME_UNIT;
    if (hour > 0) {
      return `${this.padding(hour.toString())}${':'}
        ${this.padding(minute.toString())}${':'}${this.padding(second.toString())}`;
    }
    if (minute > 0) {
      return `${this.padding(minute.toString())}${':'}${this.padding(second.toString())}`;
    } else {
      return `${CommonConstants.INITIAL_TIME_UNIT}${':'}${this.padding(second.toString())}`;
    }
    return time;
  }

  /**
   * Zero padding, 2 bits.
   *
   * @param num Number to be converted.
   * @return Result after zero padding.
   */
  padding(num: string) {
    let length = CommonConstants.PADDING_LENGTH;
    for (var len = (num.toString()).length; len < length; len = num.length) {
      num = `${CommonConstants.PADDING_STR}${num}`;
    }
    return num;
  }
}

export default new DateFormatUtil();



---

## samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/Logger.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/common/util/Logger.ets


import hilog from '@ohos.hilog';

class Logger {
  private domain: number;
  private prefix: string;
  private format: string = '%{public}s, %{public}s';

  /**
   * constructor.
   *
   * @param Prefix Identifies the log tag.
   * @param domain Domain Indicates the service domain, which is a hexadecimal integer ranging from 0x0 to 0xFFFFF.
   */
  constructor(prefix: string = 'MyApp', domain: number = 0xFF00) {
    this.prefix = prefix;
    this.domain = domain;
  }

  debug(...args: any[]): void {
    hilog.debug(this.domain, this.prefix, this.format, args);
  }

  info(...args: any[]): void {
    hilog.info(this.domain, this.prefix, this.format, args);
  }

  warn(...args: any[]): void {
    hilog.warn(this.domain, this.prefix, this.format, args);
  }

  error(...args: any[]): void {
    hilog.error(this.domain, this.prefix, this.format, args);
  }
}

export default new Logger('VideoPlayer', 0xFF00)

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/common/bean/VideoBean.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/common/bean/VideoBean.ets

import image from '@ohos.multimedia.image';

@Observed export class VideoBean {
  name: string;
  src: string;
  pixelMap?: image.PixelMap;

  constructor(name: string, src: string, pixelMap?: image.PixelMap) {
    this.name = name;
    this.src = src;
    this.pixelMap = pixelMap;
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/PlayConstants.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/PlayConstants.ets


/**
 * Play constants for all features.
 */
export class PlayConstants {
  /**
   * Playback page constant.
   */
  static readonly PLAY_PAGE = {
    PLAY_SPEED: 1,
    VOLUME: 0.5,
    VOLUME_SHOW: false,
    BRIGHT: 0.5,
    BRIGHT_SHOW: false,
    POSITION_X: 0,
    POSITION_Y: 0,
    HEIGHT: '7.2%',
    PLAY_PLAYER_HEIGHT: '25.6%',
    PLAY_PLAYER_HEIGHT_FULL: '75.4%',
    PLAY_PROGRESS_HEIGHT: '7.1%',
    COLUMN_HEIGHT_ONE: '26.9%',
    COLUMN_HEIGHT_TWO: '22.9%',
    MIN_ANGLE: 0,
    MAX_ANGLE: 30,
    MIN_VALUE: 0,
    MAX_VALUE: 1,
    DISAPPEAR_TIME: 200,
    MARGIN_ZERO: '0'
  }

  /**
   * Playback Page Header Constant.
   */
  static readonly PLAY_TITLE = {
    DX: 0,
    DY: -20,
    GRID_COUNT: 4,
    TEXT_MARGIN_LEFT: '4.4%',
    ROW_WIDTH: '86.6%',
    POPUP: {
      ROW_HEIGHT: '45.3%',
      ROW_MARGIN_TOP: '3.8%',
      DIVIDER_STROKE_WIDTH: 1,
      DIVIDER_MARGIN_RIGHT: '10.2%',
      COLUMN_WIDTH: '43.3%',
      COLUMN_HEIGHT: '13.5%',
      CLOSE_TIME: 500
    },
  }

  /**
   * Constants for setting the playback speed.
   */
  static readonly PLAY_TITLE_DIALOG = {
    ROW_HEIGHT: '25%',
    ROW_WIDTH: '86.6%',
    COLUMNS_TEMPLATE: '1fr 1fr 1fr',
    ROWS_TEMPLATE: '1fr 1fr',
    COLUMNS_GAP: 10,
    ROWS_GAP: 10,
    COLUMN_WIDTH: '39.2%'
  }

  /**
   * Video playback constant.
   */
  static  readonly PLAY_PLAYER = {
    ID: '',
    TYPE: 'surface',
    LIBRARY_NAME: '',
    SURFACE_WIDTH: 1920,
    SURFACE_HEIGHT: 1080,
    STACK_WIDTH: '16.7%',
    IMAGE_WIDTH: '95%',
    FIRST: 0,
    NEXT: 1,
    DURATION: 0
  }

  /**
   * Video playback control constant.
   */
  static readonly PLAY_CONTROL = {
    ROW_WIDTH: '68.8%',
    PLAY_START: 1,
    PLAY_PAUSE: 2,
    NEXT: 1,
    FIRST: 0
  }

  /**
   * Progress bar page constant.
   */
  static readonly PLAY_PROGRESS = {
    CURRENT_TIME: '00:00',
    TOTAL_TIME: '00:00',
    PROGRESS_VAL: 0,
    INTERVAL: -1,
    STEP: 1,
    TRACK_THICKNESS: 1,
    SLIDER_WIDTH: '68.9%',
    MARGIN_LEFT: '2.2%',
    SEEK_TIME: 0,
    ROW_WIDTH: '93.4%'
  }

  /**
   * Network video playback error notification duration
   */
  static readonly PLAY_ERROR_TIME = 3000;
}


---

## samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/CommonConstants.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/CommonConstants.ets

import { VideoBean } from '../bean/VideoBean';

/**
 * Common constants for all features.
 */
export class CommonConstants {
  /**
   * Full percent.
   */
  static readonly FULL_PERCENT: string = '100%';

  /**
   * Ninety percent.
   */
  static readonly NINETY_PERCENT: string = '90%';

  /**
   * Fifty percent.
   */
  static readonly FIFTY_PERCENT: string = '50%';

  /**
   * Playback page path.
   */
  static readonly PAGE: string = 'pages/PlayPage';

  /**
   * Local video ID.
   */
  static readonly TYPE_LOCAL: number = 0;

  /**
   * Network video ID.
   */
  static readonly TYPE_INTERNET: number = 1;

  /**
   * Start playing.
   */
  static readonly STATUS_START: number = 1;

  /**
   * Playing Pause.
   */
  static readonly STATUS_PAUSE: number = 2;

  /**
   * Stop Playing.
   */
  static readonly STATUS_STOP: number = 3;

  /**
   * Width-height ratio.
   */
  static readonly ASPECT_RATIO: number = 1;

  /**
   * One hundred.
   */
  static readonly ONE_HUNDRED: number = 100;

  /**
   * A thousand.
   */
  static readonly A_THOUSAND: number = 1000;

  /**
   * Speed set.
   */
  static readonly SPEED_ARRAY = [
    { text: '0.75X', value: 0 },
    { text: '1.0X', value: 1 },
    { text: '1.25X', value: 2 },
    { text: '1.75X', value: 3 },
    { text: '2.0X', value: 4 }
  ];

  /**
   * time system, Hour-minute-second conversion.
   */
  static readonly TIME_UNIT: number = 60;

  /**
   * Initial Time UNIT.
   */
  static readonly INITIAL_TIME_UNIT: string = '00';

  /**
   * Zero padding, 2 bits.
   */
  static readonly PADDING_LENGTH: number = 2;

  /**
   * String zero padding.
   */
  static readonly PADDING_STR: string = '0';

  /**
   * Breath screen status.
   */
  static readonly SCREEN_OFF: string = 'usual.event.SCREEN_OFF';

  /**
   * Operation status of video player 4.
   */
  static readonly OPERATE_STATE: Array<string> = ['prepared','playing', 'paused', 'completed'];
}

/**
 * Player component status.
 */
export enum AvplayerStatus {
  IDLE = 'idle',
  INITIALIZED = 'initialized',
  PREPARED = 'prepared',
  PLAYING = 'playing',
  PAUSED = 'paused',
  COMPLETED = 'completed',
  STOPPED = 'stopped',
  RELEASED = 'released',
  ERROR = 'error'
}

/**
 * AVPlayer binding event.
 */
export enum Events {
  STATE_CHANGE = 'stateChange',
  TIME_UPDATE = 'timeUpdate',
  ERROR = 'error'
}

/**
 * Video object collection.
 */
export const VIDEO_DATA: VideoBean[] = [
  {
    'name': 'video1',
    'src': 'video1.mp4'
  },
  {
    'name': 'video2',
    'src': 'video2.mp4'
  }
]


---

## samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/HomeConstants.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/common/constants/HomeConstants.ets


/**
 * Home constants for all features.
 */
export class HomeConstants {
  /**
   * Constants on the tab page of the main interface.
   */
  static readonly HOME_TAB = {
    CURRENT_INDEX: 0,
    TAB_BAR_FIRST: 0,
    TAB_BAR_SECOND: 1,
    BAR_WIDTH: 360,
    BAR_HEIGHT: 60,
    FONT_WEIGHT_SELECT: 500,
    FONT_WEIGHT_UNSELECT: 400,
    LINE_HEIGHT: 22,
    MARGIN_TOP_TWO: 17,
    MARGIN_BOTTOM: 7,
    STROKE_WIDTH: 2
  }

  /**
   * Constant of the video list.
   */
  static readonly HOME_TAB_LIST = {
    COLUMN_WIDTH: '86.7%',
    LIST_SPACE: 20,
    LIST_INITIAL_INDEX: 0,
    IMAGE_HEIGHT: '84.8%',
    IMAGE_WIDTH: '26.7%',
    DIVIDER_STROKE_WIDTH: 1,
    LIST_ITEM_ROW_COLUMN_WIDTH: '73.3%',
    LIST_ITEM_ROW_HEIGHT: '12.3%',
    WIDTH: 720,
    HEIGHT: 720
  }

  /**
   * Scan local video and add network video buttons.
   */
  static readonly HOME_TAB_BUTTON = {
    HEIGHT: '51%',
    COLUMN_HEIGHT: '10%'
  }

  /**
   * Add Network Video dialog box.
   */
  static readonly INTERNET_ADD_DIALOG= {
    OFFSET_DX: 0,
    OFFSET_DY: -20,
    GRID_COUNT: 4,
    TEXT_HEIGHT: '6.7%',
    TEXT_MARGIN_TOP: '3.1%',
    DURATION: 1000,
    DURATION_ONE: -1,
    DURATION_TWO: 0
  }

  /**
   * The video player is used to verify the video link.
   */
  static readonly X_COMPONENT = {
    ID: '',
    TYPE: 'surface',
    LIBRARY_NAME: '',
  }
}


---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayProgress.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayProgress.ets


import { VideoController } from '../controller/VideoController';
import { CommonConstants } from '../common/constants/CommonConstants';
import { PlayConstants } from '../common/constants/PlayConstants';

@Component
export struct PlayProgress {
  private playVideoModel: VideoController;
  @State currentTime: string = PlayConstants.PLAY_PROGRESS.CURRENT_TIME;
  @State totalTime: string = PlayConstants.PLAY_PROGRESS.TOTAL_TIME;
  @State progressVal: number = PlayConstants.PLAY_PROGRESS.PROGRESS_VAL;

  aboutToAppear() {
    if (this.playVideoModel !== null) {
      this.playVideoModel.initProgressThis(this);
    }
  }

  build() {
    Column() {
      Row() {
        Text(this.currentTime)
          .fontSize($r('app.float.slider_font_size'))
          .fontColor(Color.White)
        Slider({
          value: this.progressVal,
          step: PlayConstants.PLAY_PROGRESS.STEP,
          style: SliderStyle.OutSet
        })
          .blockColor(Color.White)
          .trackColor($r('app.color.track_color'))
          .selectedColor(Color.White)
          .trackThickness(PlayConstants.PLAY_PROGRESS.TRACK_THICKNESS)
          .layoutWeight(1)
          .margin({ left: PlayConstants.PLAY_PROGRESS.MARGIN_LEFT })
          .onChange((value: number, mode: SliderChangeMode) => {
            this.playVideoModel.setSeekTime(value, mode);
          })
        Text(this.totalTime)
          .fontSize($r('app.float.slider_font_size'))
          .fontColor(Color.White)
          .margin({ left: PlayConstants.PLAY_PROGRESS.MARGIN_LEFT })
      }
      .width(PlayConstants.PLAY_PROGRESS.ROW_WIDTH)
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(CommonConstants.FULL_PERCENT)
    .justifyContent(FlexAlign.Center)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayPlayer.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayPlayer.ets


import { VideoController } from '../controller/VideoController';
import { CommonConstants } from '../common/constants/CommonConstants';
import { PlayConstants } from '../common/constants/PlayConstants';

@Component
export struct PlayPlayer {
  private playVideoModel: VideoController;
  @Consume src: string;
  @Consume index: number;
  @State volume: number = PlayConstants.PLAY_PAGE.VOLUME;
  @State volumeShow: boolean = PlayConstants.PLAY_PAGE.VOLUME_SHOW;
  @State bright: number = PlayConstants.PLAY_PAGE.BRIGHT;
  @State brightShow: boolean = PlayConstants.PLAY_PAGE.BRIGHT_SHOW;
  private xComponentController;
  private surfaceID: number;

  aboutToAppear() {
    if (this.playVideoModel !== null) {
      this.playVideoModel.initPlayerThis(this);
    }
    this.xComponentController = new XComponentController();
  }

  build() {
    Stack() {
      XComponent({
        id: PlayConstants.PLAY_PLAYER.ID,
        type: PlayConstants.PLAY_PLAYER.TYPE,
        libraryname: PlayConstants.PLAY_PLAYER.LIBRARY_NAME,
        controller: this.xComponentController
      })
        .onLoad(async () => {
          this.xComponentController.setXComponentSurfaceSize({
            surfaceWidth: PlayConstants.PLAY_PLAYER.SURFACE_WIDTH,
            surfaceHeight: PlayConstants.PLAY_PLAYER.SURFACE_HEIGHT
          });
          this.surfaceID = this.xComponentController.getXComponentSurfaceId();
          this.playVideoModel.firstPlay(this.index, this.src, this.surfaceID);
        })
        .width(CommonConstants.FULL_PERCENT)
        .height(CommonConstants.FULL_PERCENT)

      Stack() {
        Progress({
          value: Math.floor(this.volume * CommonConstants.ONE_HUNDRED),
          type: ProgressType.Ring
        })
          .width(CommonConstants.FULL_PERCENT)
          .aspectRatio(CommonConstants.ASPECT_RATIO)
        Image($r('app.media.ic_volume'))
          .width(PlayConstants.PLAY_PLAYER.IMAGE_WIDTH)
          .aspectRatio(CommonConstants.ASPECT_RATIO)
      }
      .width(PlayConstants.PLAY_PLAYER.STACK_WIDTH)
      .aspectRatio(CommonConstants.ASPECT_RATIO)
      .visibility(this.volumeShow ? Visibility.Visible : Visibility.Hidden)

      Stack() {
        Progress({
          value: Math.floor(this.bright * CommonConstants.ONE_HUNDRED),
          type: ProgressType.Ring
        })
          .width(CommonConstants.FULL_PERCENT)
          .aspectRatio(CommonConstants.ASPECT_RATIO)
        Image($r('app.media.ic_brightness'))
          .width(PlayConstants.PLAY_PLAYER.IMAGE_WIDTH)
          .aspectRatio(CommonConstants.ASPECT_RATIO)
      }
      .width(PlayConstants.PLAY_PLAYER.STACK_WIDTH)
      .aspectRatio(CommonConstants.ASPECT_RATIO)
      .visibility(this.brightShow ? Visibility.Visible : Visibility.Hidden)
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(CommonConstants.FULL_PERCENT)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentListItem.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentListItem.ets


import { VideoBean } from '../common/bean/VideoBean';
import { HomeConstants } from '../common/constants/HomeConstants';
import { CommonConstants } from '../common/constants/CommonConstants';

@Component
export struct HomeTabContentListItem {
  private item: VideoBean;

  build() {
    Row() {
      Image(this.item.pixelMap === undefined ? $r('app.media.ic_internet') : this.item.pixelMap)
        .height(HomeConstants.HOME_TAB_LIST.IMAGE_HEIGHT)
        .width(HomeConstants.HOME_TAB_LIST.IMAGE_WIDTH)
        .margin({ left: $r('app.float.item_image_margin_left') })
        .borderRadius($r('app.float.image_border_radius'))
      Column() {
        Column() {
          Text(this.item.name)
            .fontSize($r('app.float.item_font_size'))
            .margin({
              left: $r('app.float.item_text_margin_left'),
              right: $r('app.float.item_text_margin_right')
            })
        }
        .height(CommonConstants.FULL_PERCENT)
        .width(CommonConstants.FULL_PERCENT)
        .justifyContent(FlexAlign.Center)
        .alignItems(HorizontalAlign.Start)

        Divider()
          .strokeWidth(HomeConstants.HOME_TAB_LIST.DIVIDER_STROKE_WIDTH)
          .color($r('app.color.divider_color'))
          .margin({
            left: $r('app.float.item_divider_margin_left'),
            right: $r('app.float.item_divider_margin_right')
          })
      }
      .height(CommonConstants.FULL_PERCENT)
      .width(HomeConstants.HOME_TAB_LIST.LIST_ITEM_ROW_COLUMN_WIDTH)
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(HomeConstants.HOME_TAB_LIST.LIST_ITEM_ROW_HEIGHT)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayTitle.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayTitle.ets


import router from '@ohos.router';
import { PlayTitleDialog } from '../view/PlayTitleDialog';
import { VideoController } from '../controller/VideoController';
import { CommonConstants } from '../common/constants/CommonConstants';
import { PlayConstants } from '../common/constants/PlayConstants';

@Component
export struct PlayTitle {
  private  playVideoModel: VideoController;
  @State @Watch('watchSpeed')playSpeed: number = 1;
  @State loop: boolean = false;
  @State customPopup: boolean = false;
  dialogController: CustomDialogController = new CustomDialogController({
    builder: PlayTitleDialog({
      playSpeed: $playSpeed
    }),
    autoCancel: true,
    alignment: DialogAlignment.Bottom,
    offset: { dx: PlayConstants.PLAY_TITLE.DX, dy: PlayConstants.PLAY_TITLE.DY },
    gridCount: PlayConstants.PLAY_TITLE.GRID_COUNT,
    customStyle: false
  })

  @Builder popupBuilder() {
    Column() {
      Row() {
        Image($r('app.media.ic_speed'))
          .width($r('app.float.title_popup_image_size'))
          .aspectRatio(CommonConstants.ASPECT_RATIO)
          .margin({ left: $r('app.float.title_popup_image_left') })
        Text($r('app.string.speed_play'))
          .fontSize($r('app.float.title_popup_font_size'))
          .margin({ left: $r('app.float.title_popup_text_left') })
      }
      .width(CommonConstants.FULL_PERCENT)
      .height(PlayConstants.PLAY_TITLE.POPUP.ROW_HEIGHT)
      .margin({ top: PlayConstants.PLAY_TITLE.POPUP.ROW_MARGIN_TOP })
      .onClick(() => {
        this.customPopup = !this.customPopup;
        this.dialogController.open();
      })

      Row() {
        Divider()
          .strokeWidth(PlayConstants.PLAY_TITLE.POPUP.DIVIDER_STROKE_WIDTH)
          .color($r('app.color.divider_color'))
          .margin({
            left: $r('app.float.title_popup_divider_left'),
            right: PlayConstants.PLAY_TITLE.POPUP.DIVIDER_MARGIN_RIGHT
          })
      }
      .width(CommonConstants.FULL_PERCENT)

      Row() {
        Image(this.loop ? $r('app.media.ic_single_loop') : $r('app.media.ic_sequence_play'))
          .width($r('app.float.title_popup_image_size'))
          .aspectRatio(CommonConstants.ASPECT_RATIO)
          .margin({ left: $r('app.float.title_popup_image_left') })
        Text(this.loop ? $r('app.string.monolithic_cycle') : $r('app.string.continuous_playback'))
          .fontSize($r('app.float.title_popup_font_size'))
          .margin({ left: $r('app.float.title_popup_text_left') })
      }
      .width(CommonConstants.FULL_PERCENT)
      .height(PlayConstants.PLAY_TITLE.POPUP.ROW_HEIGHT)
      .onClick(() => {
        this.loop = !this.loop;
        this.playVideoModel.setLoop();
        setTimeout(() => {
          this.customPopup = !this.customPopup;
        },  PlayConstants.PLAY_TITLE.POPUP.CLOSE_TIME);
      })
    }
    .justifyContent(FlexAlign.Center)
    .alignItems(HorizontalAlign.Center)
    .width(PlayConstants.PLAY_TITLE.POPUP.COLUMN_WIDTH)
    .height(PlayConstants.PLAY_TITLE.POPUP.COLUMN_HEIGHT)
  }

  aboutToAppear() {
    if (this.playVideoModel !== null) {
      this.playVideoModel.initTitleThis(this);
    }
  }

  watchSpeed() {
    this.playVideoModel.setSpeed(this.playSpeed);
  }

  build() {
    Column() {
      Row() {
        Image($r('app.media.ic_back'))
          .width($r('app.float.title_image_size'))
          .aspectRatio(CommonConstants.ASPECT_RATIO)
          .onClick(() => {
            router.back();
          })
        Text($r('app.string.video_playback'))
          .fontColor(Color.White)
          .fontSize($r('app.float.title_font_size'))
          .margin({ left: PlayConstants.PLAY_TITLE.TEXT_MARGIN_LEFT })
          .layoutWeight(1)
        Image($r('app.media.ic_more'))
          .width($r('app.float.title_image_size'))
          .aspectRatio(CommonConstants.ASPECT_RATIO)
          .bindPopup(this.customPopup, {
            builder: this.popupBuilder,
            placement: Placement.BottomRight,
            popupColor: Color.White,
            enableArrow: false
          })
          .onClick(() => {
            this.customPopup = !this.customPopup;
          })
      }
      .width(PlayConstants.PLAY_TITLE.ROW_WIDTH)
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(CommonConstants.FULL_PERCENT)
    .justifyContent(FlexAlign.Center)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContent.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContent.ets


import { VideoBean } from '../common/bean/VideoBean';
import { HomeTabContentList } from './HomeTabContentList';
import { HomeTabContentButton } from './HomeTabContentButton';
import { CommonConstants } from '../common/constants/CommonConstants';

@Component
export struct HomeTabContent {
  private currIndex: number;
  @Provide videoList: Array<VideoBean> = [];

  build() {
    Column() {
      HomeTabContentList({currIndex: this.currIndex});
      HomeTabContentButton({currIndex: this.currIndex});
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(CommonConstants.FULL_PERCENT)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayTitleDialog.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayTitleDialog.ets


import { CommonConstants } from '../common/constants/CommonConstants';
import { PlayConstants } from '../common/constants/PlayConstants';

@CustomDialog
export struct PlayTitleDialog {
  @Link playSpeed: number;
  controller: CustomDialogController;

  build() {
    Column() {
      Row() {
        Text($r('app.string.speed_play'))
          .fontSize($r('app.float.title_font_size'))
      }
      .height(PlayConstants.PLAY_TITLE_DIALOG.ROW_HEIGHT)
      .width(PlayConstants.PLAY_TITLE_DIALOG.ROW_WIDTH)

      Column() {
        Grid() {
          ForEach(CommonConstants.SPEED_ARRAY, (item) => {
            GridItem() {
              Text(item.text)
                .fontSize($r('app.float.title_dialog_font_size'))
                .backgroundColor($r('app.color.speed_text_color'))
                .width(CommonConstants.FULL_PERCENT)
                .height(CommonConstants.FULL_PERCENT)
                .textAlign(TextAlign.Center)
                .borderRadius($r('app.float.text_border_radius'))
            }
            .onClick(() => {
              this.playSpeed = item.value;
              this.controller.close();
            })
          }, item => JSON.stringify(item))
        }
        .columnsTemplate(PlayConstants.PLAY_TITLE_DIALOG.COLUMNS_TEMPLATE)
        .rowsTemplate(PlayConstants.PLAY_TITLE_DIALOG.ROWS_TEMPLATE)
        .columnsGap(PlayConstants.PLAY_TITLE_DIALOG.COLUMNS_GAP)
        .rowsGap(PlayConstants.PLAY_TITLE_DIALOG.ROWS_GAP)
        .width(PlayConstants.PLAY_TITLE_DIALOG.ROW_WIDTH)
        .height(CommonConstants.FULL_PERCENT)
      }
      .height(CommonConstants.FIFTY_PERCENT)

      Row() {
        Text($r('app.string.cancel'))
          .fontColor($r('app.color.index_tab_selected_font_color'))
          .fontSize($r('app.float.title_dialog_font_size'))
      }
      .height(PlayConstants.PLAY_TITLE_DIALOG.ROW_HEIGHT)
      .width(PlayConstants.PLAY_TITLE_DIALOG.ROW_WIDTH)
      .justifyContent(FlexAlign.Center)
      .onClick(() => {
        this.controller.close();
      })
    }
    .height(PlayConstants.PLAY_TITLE_DIALOG.COLUMN_WIDTH)
    .width(CommonConstants.FULL_PERCENT)
    .backgroundColor(Color.White)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentDialog.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentDialog.ets


import { VideoBean } from '../common/bean/VideoBean';
import { HomeDialogModel } from '../viewmodel/HomeDialogModel';
import { CommonConstants } from '../common/constants/CommonConstants';
import { HomeConstants } from '../common/constants/HomeConstants';

@CustomDialog
export struct HomeTabContentDialog {
  private dialogModel: HomeDialogModel = new HomeDialogModel(this);
  @State linkCheck: Resource = $r('app.string.link_check');
  @State confirmAdd: Resource = $r('app.string.confirm_add');
  @State loadColor: Resource = $r('app.color.index_tab_selected_font_color');
  @Link name: string;
  @Link src: string;
  @Link videoList: Array<VideoBean>;
  controller: CustomDialogController;
  confirm: () => void;

  build() {
    Column() {
      TextInput({ placeholder: $r('app.string.link_placeholder'), text: this.src })
        .height(HomeConstants.INTERNET_ADD_DIALOG.TEXT_HEIGHT)
        .width(CommonConstants.NINETY_PERCENT)
        .margin({ top: HomeConstants.INTERNET_ADD_DIALOG.TEXT_MARGIN_TOP })
        .onChange((value: string) => {
          this.src = value;
        })
      TextInput({ placeholder: $r('app.string.name_placeholder'), text: this.name })
        .height(HomeConstants.INTERNET_ADD_DIALOG.TEXT_HEIGHT)
        .width(CommonConstants.NINETY_PERCENT)
        .margin({ top: HomeConstants.INTERNET_ADD_DIALOG.TEXT_MARGIN_TOP })
        .onChange((value: string) => {
          this.name = value;
        })
      Flex({ justifyContent: FlexAlign.SpaceAround }) {
        Text(this.linkCheck)
          .fontSize($r('app.float.dialog_font_size'))
          .fontColor(this.loadColor)
          .onClick(() => {
            if (this.dialogModel.checkSrcNull()) {
              this.dialogModel.checkSrcValidity(0);
            }
          })
        Divider()
          .vertical(true)
          .height($r('app.float.tab_dialog_divider_height'))
          .color($r('app.color.divider_color'))
          .opacity($r('app.float.tab_dialog_divider_opacity'))
          .margin({
            left: $r('app.float.dialog_divider_margin_left'),
            right: $r('app.float.dialog_divider_margin_left')
          })
        Text(this.confirmAdd)
          .fontSize($r('app.float.dialog_font_size'))
          .fontColor(this.loadColor)
          .onClick(() => {
            if (this.dialogModel.checkSrcNull() && this.dialogModel.checkNameNull()) {
              this.dialogModel.checkSrcValidity(1);
            }
          })
      }
      .margin({
        top: $r('app.float.dialog_column_margin_top'),
        bottom: $r('app.float.dialog_column_margin_bottom')
      })
    }
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentList.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentList.ets


import router from '@ohos.router';
import Logger from '../common/util/Logger';
import { VideoBean } from '../common/bean/VideoBean';
import HomeVideoListModel from '../viewmodel/HomeVideoListModel';
import { HomeConstants } from '../common/constants/HomeConstants';
import { CommonConstants } from '../common/constants/CommonConstants';
import { HomeTabContentListItem } from './HomeTabContentListItem';

@Component
export struct HomeTabContentList {
  private currIndex: number;
  @Consume videoList: Array<VideoBean>;
  @State item: VideoBean = undefined;

  async aboutToAppear() {
    if (this.currIndex === CommonConstants.TYPE_LOCAL) {
      this.videoList = await HomeVideoListModel.getLocalVideo();
    } else {
      this.videoList = globalThis.videoInternetList;
    }
  }

  build() {
    Column() {
      List({
        space: HomeConstants.HOME_TAB_LIST.LIST_SPACE,
        initialIndex: HomeConstants.HOME_TAB_LIST.LIST_INITIAL_INDEX
      }) {
        ForEach(this.videoList, (item: VideoBean, index: number) => {
          ListItem() {
            HomeTabContentListItem({item: item});
          }.onClick(() => {
            globalThis.videoList = this.videoList;
            router.pushUrl({
              url: CommonConstants.PAGE,
              params: {
                src: item.src,
                index: index,
                type: this.currIndex
              }
            }).catch(err => {
              Logger.error('[IndexTabLocalList] router error: ' + JSON.stringify(err))
            });
          })
        }, item => JSON.stringify(item))
      }
      .backgroundColor(Color.White)
      .borderRadius($r('app.float.list_border_radius'))
    }
    .width(HomeConstants.HOME_TAB_LIST.COLUMN_WIDTH)
    .height(CommonConstants.NINETY_PERCENT)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayControl.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/PlayControl.ets

import { VideoController } from '../controller/VideoController';
import { CommonConstants } from '../common/constants/CommonConstants';
import { PlayConstants } from '../common/constants/PlayConstants';

@Component
export struct PlayControl {
  private playVideoModel: VideoController;
  @Consume status: number;

  build() {
    Column() {
      Row() {
        Image($r('app.media.ic_previous'))
          .width($r('app.float.control_image_width'))
          .aspectRatio(CommonConstants.ASPECT_RATIO)
          .onClick(async () => {
            this.playVideoModel.previousVideo();
            this.status = CommonConstants.STATUS_START;
          })
        Column() {
          Image(this.status === CommonConstants.STATUS_START ?
            $r('app.media.ic_pause') : $r('app.media.ic_play'))
            .width($r('app.float.control_image_width'))
            .aspectRatio(CommonConstants.ASPECT_RATIO)
            .onClick(async () => {
              let curStatus = (this.playVideoModel.getStatus() === CommonConstants.STATUS_START);
              this.status = curStatus ? CommonConstants.STATUS_PAUSE : CommonConstants.STATUS_START;
              this.playVideoModel.switchPlayOrPause();
            })
        }
        .layoutWeight(1)
        Image($r('app.media.ic_next'))
          .width($r('app.float.control_image_width'))
          .aspectRatio(CommonConstants.ASPECT_RATIO)
          .onClick(() => {
            this.playVideoModel.nextVideo();
            this.status = CommonConstants.STATUS_START;
          })
      }
      .width(PlayConstants.PLAY_CONTROL.ROW_WIDTH)
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(CommonConstants.FULL_PERCENT)
    .justifyContent(FlexAlign.Center)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentButton.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/view/HomeTabContentButton.ets


import { HomeTabContentDialog } from './HomeTabContentDialog';
import { VideoBean } from '../common/bean/VideoBean';
import HomeVideoListModel from '../viewmodel/HomeVideoListModel';
import { CommonConstants } from '../common/constants/CommonConstants';
import { HomeConstants } from '../common/constants/HomeConstants';

@Component
export struct HomeTabContentButton {
  private currIndex: number;
  @Consume videoList: Array<VideoBean>;
  @State name: string = '';
  @State src: string = '';
  dialogController: CustomDialogController = new CustomDialogController({
    builder: HomeTabContentDialog({
      confirm: this.confirm,
      name: $name,
      src: $src,
      videoList: $videoList
    }),
    autoCancel: true,
    alignment: DialogAlignment.Default,
    offset: {
      dx: HomeConstants.INTERNET_ADD_DIALOG.OFFSET_DX,
      dy: HomeConstants.INTERNET_ADD_DIALOG.OFFSET_DY
    },
    gridCount: HomeConstants.INTERNET_ADD_DIALOG.GRID_COUNT,
    customStyle: false
  });

  confirm() {
    HomeVideoListModel.setInternetVideo(this.name, this.src);
    this.videoList = globalThis.videoInternetList;
    this.src = '';
    this.name = '';
  }

  build() {
    Column() {
      Button(this.currIndex === 0 ? $r('app.string.scan_local_video') : $r('app.string.add_internet_video'), {
        type: ButtonType.Normal,
        stateEffect: true
      })
        .borderRadius($r('app.float.tab_border_radius'))
        .fontSize($r('app.float.button_font_size'))
        .height(HomeConstants.HOME_TAB_BUTTON.HEIGHT)
        .backgroundColor($r('app.color.button_back_ground_color'))
        .onClick(async () => {
          if (this.currIndex === 0) {
            this.videoList = await HomeVideoListModel.getLocalVideo();
          } else {
            this.dialogController.open();
          }
        })
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(HomeConstants.HOME_TAB_BUTTON.COLUMN_HEIGHT)
    .justifyContent(FlexAlign.Center)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/pages/PlayPage.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/pages/PlayPage.ets

import router from '@ohos.router';
import { PlayTitle } from '../view/PlayTitle';
import { PlayPlayer } from '../view/PlayPlayer';
import { PlayControl } from '../view/PlayControl';
import { PlayProgress } from '../view/PlayProgress';
import { VideoController } from '../controller/VideoController';
import { CommonConstants } from '../common/constants/CommonConstants';
import { PlayConstants } from '../common/constants/PlayConstants';

@Entry
@Component
struct PlayPage {
  @State videoHeight: string = PlayConstants.PLAY_PAGE.PLAY_PLAYER_HEIGHT;
  @State videoWidth: string = CommonConstants.FULL_PERCENT;
  @State videoMargin: string = PlayConstants.PLAY_PAGE.MARGIN_ZERO;
  @State videoPosition: FlexAlign  = FlexAlign.Center;
  private playVideoModel: VideoController = new VideoController();
  @Provide src: string = router.getParams()['src'];
  @Provide index: number = router.getParams()['index'];
  @Provide type: number = router.getParams()['type'];
  @Provide status: number = CommonConstants.STATUS_START;
  private panOptionBright: PanGestureOptions = new PanGestureOptions({ direction: PanDirection.Vertical });
  private panOptionVolume: PanGestureOptions = new PanGestureOptions({ direction: PanDirection.Horizontal });

  aboutToAppear() {
    this.playVideoModel.initPlayPageThis(this);
  }

  aboutToDisappear() {
    this.playVideoModel.release();
  }

  onPageHide() {
    this.status = CommonConstants.STATUS_PAUSE;
    this.playVideoModel.pause();
  }

  build() {
    Stack() {
      Column () {
        Column(){
        }
        .height(this.videoMargin)
        PlayPlayer({ playVideoModel: this.playVideoModel })
          .width(this.videoWidth)
          .height(this.videoHeight)
      }
      .height(CommonConstants.FULL_PERCENT)
      .width(CommonConstants.FULL_PERCENT)
      .justifyContent(this.videoPosition)
      .zIndex(0)
      Column() {
        PlayTitle({ playVideoModel: this.playVideoModel })
          .width(CommonConstants.FULL_PERCENT)
          .height(PlayConstants.PLAY_PAGE.HEIGHT)
        Column()
          .width(CommonConstants.FULL_PERCENT)
          .height(PlayConstants.PLAY_PAGE.COLUMN_HEIGHT_ONE)
          .gesture(
            PanGesture(this.panOptionBright)
              .onActionStart((event: GestureEvent) => {
                this.playVideoModel.onBrightActionStart(event);
              })
              .onActionUpdate((event: GestureEvent) => {
                this.playVideoModel.onBrightActionUpdate(event);
              })
              .onActionEnd(() => {
                this.playVideoModel.onActionEnd();
              })
          )
        Column() {
        }
        .width(CommonConstants.FULL_PERCENT)
        .height(PlayConstants.PLAY_PAGE.PLAY_PLAYER_HEIGHT)
        Column()
          .width(CommonConstants.FULL_PERCENT)
          .height(PlayConstants.PLAY_PAGE.COLUMN_HEIGHT_TWO)
          .gesture(
            PanGesture(this.panOptionVolume)
              .onActionStart((event: GestureEvent) => {
                this.playVideoModel.onVolumeActionStart(event);
              })
              .onActionUpdate((event: GestureEvent) => {
                this.playVideoModel.onVolumeActionUpdate(event);
              })
              .onActionEnd(() => {
                this.playVideoModel.onActionEnd();
              })
          )
        PlayControl({ playVideoModel: this.playVideoModel })
          .width(CommonConstants.FULL_PERCENT)
          .height(PlayConstants.PLAY_PAGE.HEIGHT)
        PlayProgress({ playVideoModel: this.playVideoModel })
          .width(CommonConstants.FULL_PERCENT)
          .height(PlayConstants.PLAY_PAGE.PLAY_PROGRESS_HEIGHT)
      }
      .height(CommonConstants.FULL_PERCENT)
      .width(CommonConstants.FULL_PERCENT)
      .zIndex(1)
    }
    .height(CommonConstants.FULL_PERCENT)
    .width(CommonConstants.FULL_PERCENT)
    .backgroundColor(Color.Black)
  }
}

---

## samples/ArkTSVideoPlayer/entry/src/main/ets/pages/HomePage.ets

Source: harmonyos-tutorial/samples/ArkTSVideoPlayer/entry/src/main/ets/pages/HomePage.ets

import ScreenUtil from '../common/util/ScreenUtil';
import { HomeConstants } from '../common/constants/HomeConstants';
import { CommonConstants } from '../common/constants/CommonConstants';
import { HomeTabContent } from '../view/HomeTabContent';

@Entry
@Component
struct HomePage {
  @State currentIndex: number = HomeConstants.HOME_TAB.CURRENT_INDEX;
  private controller: TabsController = new TabsController();

  aboutToAppear() {
    ScreenUtil.setScreenSize();
  }

  @Builder TabBuilder(index: number, name: Resource) {
    Column() {
      Text(name)
        .fontColor(this.currentIndex === index ?
          $r('app.color.index_tab_selected_font_color') : $r('app.color.index_tab_font_color'))
        .fontSize($r('app.float.home_page_font_size'))
        .fontWeight(this.currentIndex === index ?
        HomeConstants.HOME_TAB.FONT_WEIGHT_SELECT : HomeConstants.HOME_TAB.FONT_WEIGHT_UNSELECT)
        .lineHeight(HomeConstants.HOME_TAB.LINE_HEIGHT)
        .margin({
          top: HomeConstants.HOME_TAB.MARGIN_TOP_TWO,
          bottom: HomeConstants.HOME_TAB.MARGIN_BOTTOM
        })
      Divider()
        .strokeWidth(HomeConstants.HOME_TAB.STROKE_WIDTH)
        .color($r('app.color.index_tab_selected_font_color'))
        .opacity(this.currentIndex === index ?
        HomeConstants.HOME_TAB.TAB_BAR_SECOND : HomeConstants.HOME_TAB.TAB_BAR_FIRST)
    }
    .width(CommonConstants.FULL_PERCENT)
  }

  build() {
    Column() {
      Tabs({ barPosition: BarPosition.Start, controller: this.controller }) {
        TabContent() {
          HomeTabContent({ currIndex: HomeConstants.HOME_TAB.TAB_BAR_FIRST });
        }.tabBar(this.TabBuilder(HomeConstants.HOME_TAB.TAB_BAR_FIRST, $r('app.string.index_tab_local_video')))

        TabContent() {
          HomeTabContent({ currIndex: HomeConstants.HOME_TAB.TAB_BAR_SECOND });
        }.tabBar(this.TabBuilder(HomeConstants.HOME_TAB.TAB_BAR_SECOND, $r('app.string.index_tab_internet_video')))
      }
      .vertical(false)
      .barMode(BarMode.Fixed)
      .barWidth(HomeConstants.HOME_TAB.BAR_WIDTH)
      .barHeight(HomeConstants.HOME_TAB.BAR_HEIGHT)
      .onChange((index: number) => {
        this.currentIndex = index;
      })
      .width(CommonConstants.FULL_PERCENT)
      .height(CommonConstants.FULL_PERCENT)
      .margin({ top: $r('app.float.home_tab_margin_top') })
    }
    .width(CommonConstants.FULL_PERCENT)
    .height(CommonConstants.FULL_PERCENT)
    .backgroundColor($r('app.color.index_background'))
  }
}

---

## 背景

Source: HarmonyOSNextStudyNote/2.ArkTS基础语法.md

2.ArkTS基础语法
===



### 背景

传统的Web开发使用的语言:  

- HTML: 控制页面元素
- CSS: 控制布局和样式
- JavaScript: 控制页面逻辑和数据状态


需要三种语言，而且语法不一样。 

但是对华为来说Java又用不了，Web开发又太复杂，所以有了ArkTS。   

为了确保应用开发的最佳体验，ArkTS提供对方舟开发框架ArkUI的声明式语法和其他特性的支持。由于此部分特性不在既有TypeScript的范围内。   

<img src="https://github.com/CharonChui/Pictures/blob/master/arkts_1.png?raw=true" width="70%" height="70%" />

<img src="https://github.com/CharonChui/Pictures/blob/master/arkts_2.png?raw=true" width="70%" height="70%" />


但是对于Android和iOS的开发者，都会想到，如果有前端开发这一套，效率能行吗？ 

别忘了，鸿蒙还提供了方舟编译器(ArkCompiler)，ArkCompiler AOT优化 : 隐藏继承优化、循环高级优化、过程空间优化、性能提升20%。      
随着移动设备在人们的日常生活中变得越来越普遍，许多编程语言在设计之初没有考虑到移动设备，导致应用的运行缓慢、低效、功耗大，针对移动环境的编程语言优化需求也越来越大。ArkTS是专为解决这些问题而设计的，聚焦于提高运行效率。     
ArkTS的一大特性是它专注于低运行时开销。ArkTS对TypeScript的动态类型特性施加了更严格的限制，以减少运行时开销，提高执行效率。通过取消动态类型特性，ArkTS代码能更有效地被运行前编译和优化，从而实现更快的应用启动和更低的功耗。

### ArkTS

ArkTS是OpenHarmony优选的主力应用开发语言。     
ArkTS围绕应用开发在TypeScript（简称TS）生态基础上做了进一步扩展，保持了TS的基本风格，同时通过规范定义强化开发期静态检查和分析，提升程序执行稳定性和性能。

ArkTS是一种为构建高性能应用而设计的编程语言。ArkTS在继承TypeScript语法的基础上进行了优化，以提供更高的性能和开发效率。


目前流行的编程语言TypeScript是在JavaScript基础上通过添加类型定义扩展而来的，而ArkTS则是TypeScript的进一步扩展。             TypeScript深受开发者的喜爱，因为它提供了一种更结构化的JavaScript编码方法。ArkTS旨在保持TypeScript的大部分语法，为现有的TypeScript开发者实现无缝过渡，让移动开发者快速上手ArkTS。


与JavaScript的互通性是ArkTS语言设计中的关键考虑因素。鉴于许多移动应用开发者希望重用其TypeScript和JavaScript代码和库，ArkTS提供了与JavaScript的无缝互通，使开发者可以很容易地将JavaScript代码集成到他们的应用中。这意味着开发者可以利用现有的代码和库进行ArkTS开发。



从API version 10开始，ArkTS进一步通过规范强化静态检查和分析，对比标准TS的差异可以参考从TypeScript到ArkTS的适配规则：

- 强制使用静态类型：静态类型是ArkTS最重要的特性之一。如果使用静态类型，那么程序中变量的类型就是确定的。同时，由于所有类型在程序实际运行前都是已知的，编译器可以验证代码的正确性，从而减少运行时的类型检查，有助于性能提升。

- 禁止在运行时改变对象布局：为实现最大性能，ArkTS要求在程序执行期间不能更改对象布局。

- 限制运算符语义：为获得更好的性能并鼓励开发者编写更清晰的代码，ArkTS限制了一些运算符的语义。比如，一元加法运算符只能作用于数字，不能用于其他类型的变量。

- 不支持Structural typing：对Structural typing的支持需要在语言、编译器和运行时进行大量的考虑和仔细的实现，当前ArkTS不支持该特性。根据实际场景的需求和反馈，我们后续会重新考虑。

当前，ArkTS在TypeScript的基础上，匹配ArkUI框架，并扩展了声明式UI、状态管理等能力，的主要扩展了如下能力：

- 基本语法：ArkTS定义了声明式UI描述、自定义组件和动态扩展UI元素的能力，再配合ArkUI开发框架中的系统组件及其相关的事件方法、属性方法等共同构成了UI开发的主体。

- 状态管理：ArkTS提供了多维度的状态管理机制。在UI开发框架中，与UI相关联的数据可以在组件内使用，也可以在不同组件层级间传递，比如父子组件之间、爷孙组件之间，还可以在应用全局范围内传递或跨设备传递。另外，从数据的传递形式来看，可分为只读的单向传递和可变更的双向传递。开发者可以灵活的利用这些能力来实现数据和UI的联动。

- 渲染控制：ArkTS提供了渲染控制的能力。条件渲染可根据应用的不同状态，渲染对应状态下的UI内容。循环渲染可从数据源中迭代获取数据，并在每次迭代过程中创建相应的组件。数据懒加载从数据源中按需迭代数据，并在每次迭代过程中创建相应的组件。




### 声明

ArkTS通过声明引入变量、常量、函数和类型。



#### 变量

```TypeScript
let hi: string = 'hello'
hi = 'hello, world'
```

由于ArkTS是一种静态类型语言，所有数据的类型都必须在编译时确定。

但是，如果一个变量或常量的声明包含了初始值，那么开发者就不需要显式指定其类型。ArkTS规范中列举了所有允许自动推断类型的场景。
```TypeScript
let hi1: string = 'hello'
let hi2 = 'hello, world'
```

#### 常量 

```TypeScript
const hello: string = 'hello'
```




#### number类型

ArkTS提供number和Number类型，任何整数和浮点数都可以被赋给此类型的变量。

数字字面量包括整数字面量和十进制浮点数字面量。

整数字面量包括以下类别：

- 由数字序列组成的十进制整数。例如：0、117、-345
- 以0x（或0X）开头的十六进制整数，可以包含数字（0-9）和字母a-f或A-F。例如：0x1123、0x00111、-0xF1A7
- 以0o（或0O）开头的八进制整数，只能包含数字（0-7）。例如：0o777
- 以0b（或0B）开头的二进制整数，只能包含数字0和1。例如：0b11、0b0011、-0b11

浮点字面量包括以下：

- 十进制整数，可为有符号数（即，前缀为“+”或“-”）；
- 小数点（“.”）
- 小数部分（由十进制数字字符串表示）
- 以“e”或“E”开头的指数部分，后跟有符号（即，前缀为“+”或“-”）或无符号整数。


```TypeScript
let n1 = 3.14
let n2 = 3.141592
let n3 = .5
let n4 = 1e10

function factorial(n: number): number {
  if (n <= 1) {
    return 1
  }
  return n * factorial(n - 1)
}
```


#### boolean类型

```TypeScript
let isDone: boolean = false
```


#### string

字符串字面量由单引号（'）或双引号（"）之间括起来的零个或多个字符组成。     

字符串字面量还有一特殊形式，是用反向单引号（`）括起来的模板字面量。


```TypeScript
let s1 = 'Hello, world!\n'
let s2 = 'this is a string'
let a = 'Success'
let s3 = `The result is ${a}`
```

#### void

void类型用于指定函数没有返回值。 此类型只有一个值，同样是void。由于void是引用类型，因此它可以用于泛型类型参数。


#### object

Object类型是所有引用类型的基类型。任何值，包括基本类型的值（它们会被自动装箱），都可以直接被赋给Object类型的变量。

#### 数组

```TypeScript
// 方式1
let names: string[] = ['Alice', 'Bob', 'Carol']
let firstName = names[0]

// 方式2
let names: Array<string> = ['Jack', 'Rose']
console.log(firstName)
```

两种方式都可以


##### for循环

当一个对象实现了Symbol.iterator属性时，我们认为它是可迭代的。     
一些内置的类型如Array、Map、Set、string、等都具有可迭代性。  

- fori
- for in
- for of

```TypeScript
let someArray = [1, 'string', false]
for （let item of someArray) {
    console.log(item)
}

for (let item in soneArray) {
    console.log(item)
}
```


#### 元组

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。  

```TypeScript
let x: [string, number]
x = ['hello', 10]  // 正确
x = [10, 'hello']  // 错误
```


#### 枚举


enum类型，又称枚举类型，是预先定义的一组命名值的值类型，其中命名值又称为枚举常量。 使用枚举常量时必须以枚举类型名称为前缀。

```TypeScript

enum Msg {
  HI = 'hi',
  HELLO = 'hello'
}

// 如果不赋值，默认的值就是0、1、2累加
enum Color { Red, Green, Blue }
let c: Color = Color.Red
```

#### unknown

有时候，我们会想要为那些在编译阶段还不清楚类型的变量指定一个类型。那么可以用unknown类型来标记这些变量。 

```TypeScript
let notSure: unknown = 4
notSure = 'hello'
notSure = false
```

#### void

和Java一样，当一个函数没有返回值，通常会见到其返回类型是void。  

#### null和undefined

TypeScript里，undefined和null两者各自有自己的类型，分别叫做undefined和null。

```TypeScript
let u: undefined = undefined
let n: null = null
```

#### 联合类型

union类型，即联合类型，是由多个类型组合成的引用类型。联合类型包含了变量可能的所有类型。

```TypeScript
class Cat {
  // ...
}
class Dog {
  // ...
}
class Frog {
  // ...
}
type Animal = Cat | Dog | Frog | number
// Cat、Dog、Frog是一些类型（类或接口）

let animal: Animal = new Cat()
animal = new Frog()
animal = 42
// 可以将类型为联合类型的变量赋值为任何组成类型的有效值
```

#### 别名

为匿名类型（数组、函数、对象字面量或联合类型）提供名称，或为已有类型提供替代名称。

```TypeScript
type Matrix = number[][]
type Handler = (s: string, no: number) => string
type Predicate <T> = (x: T) => Boolean
type NullableObject = Object | null
```


其他运算符、表达式、三元运算符都和Java基本一样。

```TypeScript
function process (a: number, b: number) {
  try {
    let res = divide(a, b)
    console.log(res)
  } catch (x) {
    console.log('some error')
  } finally {

  }
}
```


##### 函数  

可选参数的格式可为name?: Type。

```TypeScript
function hello(name?: string) {
  if (name == undefined) {
    console.log('Hello!')
  } else {
    console.log('Hello, ${name}!')
  }
}



```

可选参数的另一种形式为设置的参数默认值。如果在函数调用中这个参数被省略了，则会使用此参数的默认值作为实参。

```TypeScript
function multiply(n: number, coeff: number = 2): number {
  return n * coeff
}
multiply(2)  // 返回2*2
multiply(2, 3) // 返回2*3

// 匿名函数
let sun = function(n: number): number {
    return x + 1;
}
```
支持函数重载，和Java一样。


##### 箭头函数或Lambda函数
函数可以定义为箭头函数，箭头函数是定义匿名函数的简写语法，类似lambda表达式，它省略的function关键字。

```TypeScript
([param1, param2...]) => {
    ....
}
```

例如：

```TypeScript

let sum = (x: number, y: number): number => {
  return x + y
}
```


箭头函数的返回类型可以省略；省略时，返回类型通过函数体推断。

表达式可以指定为箭头函数，使表达更简短，因此以下两种表达方式是等价的：

```TypeScript
let sum1 = (x: number, y: number) => { return x + y }
let sum2 = (x: number, y: number) => x + y
```


##### 函数的可选参数

可以在参数名旁使用`?`实现可选参数的功能。 例如下面的函数lastName是可选的。  

```TypeScript
function buildName(firstName: string, lastName?: string) {
    .....
}

let result1 = buildName('bob')
let result2 = buildName('bob', 'adams')
```

##### 可变参数

可变参数(剩余参数会)被当做个数不限的可选参数。 可以一个都没有，同样也可以有任意个。 可以使用省略号（ ...）进行定义：

```TypeScript
function getEmployeeName(firstName: string, ...restOfName: string[]) {
  return firstName + ' ' + restOfName.join(' ');
}

let employeeName = getEmployeeName('Joseph', 'Samuel', 'Lucas', 'MacKinzie');
```


##### 闭包

箭头函数通常在另一个函数中定义。作为内部函数，它可以访问外部函数中定义的所有变量和函数。

为了捕获上下文，内部函数将其环境组合成闭包，以允许内部函数在自身环境之外的访问。

```TypeScript
function f(): () => number {
  let count = 0
  return (): number => { count++; return count }
}

let z = f()
console.log(z()) // 输出：1
console.log(z()) // 输出：2
```

在以上示例中，箭头函数闭包捕获count变量。



##### 类

TypeScript具备面向对象编程的基本语法，例如interface、class、enum等。     
也具备封装、继承、多态等面向对象基本特征。     

```TypeScript
class Person {
  name: string = ''
  surname: string = ''
  constructor (n: string, sn: string) {
    this.name = n
    this.surname = sn
  }
  fullName(): string {
    return this.name + ' ' + this.surname
  }
}
```

定义类后，可以使用关键字new创建实例：

```TypeScript
let p = new Person('John', 'Smith')
console.log(p.fullName())
```

或者，可以使用对象字面量创建实例，对象字面量是一个表达式，可用于创建类实例并提供一些初始值。     

它在某些情况下更方便，可以用来代替new表达式。     


```TypeScript
class Point {
  x: number = 0
  y: number = 0
}
let p: Point = {x: 42, y: 42}
```

ArkTS是静态类型语言，如上述示例所示，对象字面量只能在可以推导出该字面量类型的上下文中使用。     

其他正确的例子:    

```ArkTS
class C {
    n: number = 0
    s: string = ''
}

function foo(c: C) {}

let c: C

c = {n: 42, s: 'foo'}   // 使用变量的类型
foo({n: 42, s: 'foo'})  // 使用参数的类型

function bar(): C {
    return {n: 42, s: 'foo'} // 使用返回类型
}

```

也可以在数组元素类型或类字段类型中使用:   

```ArkTS
class C {
    n: number = 0
    s: string = ''
}

let cc: C[] = [{n: 1, s: 'a'}, {n: 2, s: 'b'}]
```



##### 静态字段
使用关键字static将字段声明为静态。静态字段属于类本身，类的所有实例共享一个静态字段。


##### 字段初始化
为了减少运行时的错误和获得更好的执行性能， ArkTS要求所有字段在声明时或者构造函数中显式初始化。



接下来的代码展示了如果name的值可以是undefined，那么应该如何写代码。

```TypeScript
class Person {
  name ?: string // 可能为`undefined`

  setName(n:string): void {
    this.name = n
  }

  // 编译时错误：name可以是"undefined"，所以将这个API的返回值类型标记为string
  getNameWrong(): string {
    return this.name
  }

  getName(): string | undefined { // 返回类型匹配name的类型
    return this.name
  }
}

let jack = new Person()
// 假设代码中没有对name赋值，例如调用"jack.setName('Jack')"

// 编译时错误：编译器认为下一行代码有可能会访问undefined的属性，报错
console.log(`${jack.getName().length}`);  // 编译失败

console.log(`${jack.getName()?.length}`); // 编译成功，没有运行时错误
```


#### 继承和实现 

```TypeScript
class Person {
  name: string = ''
  private _age = 0
  get age(): number {
    return this._age
  }
}
class Employee extends Person {
  salary: number = 0
  calculateTaxes(): number {
    return this.salary * 0.42
  }
}


interface DateInterface {
  now(): string;
}
class MyDate implements DateInterface {
  now(): string {
    // 在此实现
    return 'now is now'
  }
}
```

##### 可见性修饰符
类的方法和属性都可以使用可见性修饰符。

可见性修饰符包括：private、protected和public。默认可见性为public。



##### 泛型类型和函数
泛型类型和函数允许创建的代码在各种类型上运行，而不仅支持单一类型。

类和接口可以定义为泛型，将参数添加到类型定义中，如以下示例中的类型参数Element：

```TypeScript
class Stack<Element> {
  public pop(): Element {
    // ...
  }
  public push(e: Element):void {
    // ...
  }
}
```
要使用类型Stack，必须为每个类型参数指定类型实参：

```TypeScript
let s = new Stack<string>
s.push('hello')
```

编译器在使用泛型类型和函数时会确保类型安全。参见以下示例：

```TypeScript
let s = new Stack<string>
s.push(55) // 将会产生编译时错误
```

##### 泛型约束
泛型类型的类型参数可以绑定。例如，HashMap<Key, Value>容器中的Key类型参数必须具有哈希方法，即它应该是可哈希的。

```TypeScript
interface Hashable {
  hash(): number
}
class HasMap<Key extends Hashable, Value> {
  public set(k: Key, v: Value) {
    let h = k.hash()
    // ...其他代码...
  }
}
```

在上面的例子中，Key类型扩展了Hashable，Hashable接口的所有方法都可以为key调用。

#### 泛型函数
使用泛型函数可编写更通用的代码。比如返回数组最后一个元素的函数：

```TypeScript
function last(x: number[]): number {
  return x[x.length - 1]
}
console.log(last([1, 2, 3])) // 输出：3
```

如果需要为任何数组定义相同的函数，使用类型参数将该函数定义为泛型：

```TypeScript
function last<T>(x: T[]): T {
  return x[x.length - 1]
}
```
现在，该函数可以与任何数组一起使用。

在函数调用中，类型实参可以显式或隐式设置：

```TypeScript
// 显式设置的类型实参
console.log(last<string>(['aa', 'bb']))
console.log(last<number>([1, 2, 3]))

// 隐式设置的类型实参
// 编译器根据调用参数的类型来确定类型实参
console.log(last([1, 2, 3]))
```




#### 空安全
默认情况下，ArkTS中的所有类型都是不可为空的，因此类型的值不能为空。这类似于TypeScript的严格空值检查模式（strictNullChecks），但规则更严格。

在下面的示例中，所有行都会导致编译时错误：

```TypeScript
let x: number = null    // 编译时错误
let y: string = null    // 编译时错误
let z: number[] = null  // 编译时错误
```
可以为空值的变量定义为联合类型T | null。

```TypeScript
let x: number | null = null
x = 1    // ok
x = null // ok
if (x != null) { /* do something */ }
```



#### 空值合并运算符
空值合并二元运算符?? 用于检查左侧表达式的求值是否等于null。如果是，则表达式的结果为右侧表达式；否则，结果为左侧表达式。

换句话说，a ?? b等价于三元运算符(a != null && a != undefined) ? a : b。

在以下示例中，getNick方法如果设置了昵称，则返回昵称；否则，返回空字符串：

```TypeScript
class Person {
  // ...
  nick: string | null = null
  getNick(): string {
    return this.nick ?? ''
  }
}
```




### 模块

程序可划分为多组编译单元或模块。

当应用复杂时，我们可以把通用功能都抽取到单独的ts文件中，每个文件都是一个模块(module)，模块可以相互加载，提高代码复用性。  

模块可以相互加载，并可以使用特殊的指令export和import来交换功能，从另一个模块调用一个模块的函数。
每个模块都有其自己的作用域，即在模块中创建的任何声明（变量、函数、类等）在该模块之外都不可见，除非它们被显式导出。

与此相对，从另一个模块导出的变量、函数、类、接口等必须首先导入到模块中。




##### 导出
可以使用关键字export导出顶层的声明。

未导出的声明名称被视为私有名称，只能在声明该名称的模块中使用。

注意：通过export方式导出，在导入时要加{}。

```TypeScript
export class Point {
  x: number = 0
  y: number = 0
  constructor(x: number, y: number) {
    this.x = x
    this.y = y
  }
}
export let Origin = new Point(0, 0)
export function Distance(p1: Point, p2: Point): number {
  return Math.sqrt((p2.x - p1.x) * (p2.x - p1.x) + (p2.y - p1.y) * (p2.y - p1.y))
}
```


##### 导入
导入声明用于导入从其他模块导出的实体，并在当前模块中提供其绑定。导入声明由两部分组成：

- 导入路径，用于指定导入的模块      
- 导入绑定，用于定义导入的模块中的可用实体集和使用形式（限定或不限定使用）      

导入绑定可以有几种形式:     

- 假设模块具有路径“./utils”和导出实体“X”和“Y”。

导入绑定* as A表示绑定名称“A”，通过A.name可访问从导入路径指定的模块导出的所有实体：

```TypeScript
import * as Utils from './utils'
Utils.X // 表示来自Utils的X
Utils.Y // 表示来自Utils的Y
```

- 导入绑定{ ident1, ..., identN }表示将导出的实体与指定名称绑定，该名称可以用作简单名称：

```TypeScript
import { X, Y } from './utils'
X // 表示来自utils的X
Y // 表示来自utils的Y
```

- 如果标识符列表定义了ident as alias，则实体ident将绑定在名称alias下：

```TypeScript
import { X as Z, Y } from './utils'
Z // 表示来自Utils的X
Y // 表示来自Utils的Y
X // 编译时错误：'X'不可见
```


### 语句

和Java一样。  

```TypeScript
if ... else if ... else 
switch ... case ... default ...
```

### 字段初始化

为了减少运行时的错误和获得更好的执行性能，

ArkTS要求所有字段在声明时或者构造函数中显式初始化。这和标准TS中的strictPropertyInitialization模式一样。

### getter和setter

setter和getter可用于提供对对象属性的受控访问。

```TypeScript
class Person {
  name: string = ''
  private _age: number = 0
  get age(): number { return this._age }
  set age(x: number) {
    if (x < 0) {
      throw Error('Invalid age argument')
    }
    this._age = x
  }
}

let p = new Person()
console.log (p.age) // 将打印输出0
p.age = -42 // 设置无效age值会抛出错误
```


### 静态方法

使用关键字static将方法声明为静态。静态方法属于类本身，只能访问静态字段。

静态方法定义了类作为一个整体的公共行为。

所有实例都可以访问静态方法。



### 空安全
默认情况下，ArkTS中的所有类型都是不可为空的，因此类型的值不能为空。这类似于TypeScript的严格空值检查模式（strictNullChecks），但规则更严格。

在下面的示例中，所有行都会导致编译时错误：
```TypeScript
let x: number = null    // 编译时错误
let y: string = null    // 编译时错误
let z: number[] = null  // 编译时错误
```
可以为空值的变量定义为联合类型T | null。

```TypeScript
let x: number | null = null
x = 1    // ok
x = null // ok
if (x != null) { /* do something */ }
```

#### 非空断言运算符

后缀运算符! 可用于断言其操作数为非空。

应用于空值时，运算符将抛出错误。否则，值的类型将从T | null更改为T：
```TypeScript
let x: number | null = 1
let y: number
y = x + 1  // 编译时错误：无法对可空值作加法
y = x! + 1 // ok
```




-------------

- [上一篇:1.HarmonyOS简介](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/1.HarmonyOS%E7%AE%80%E4%BB%8B.md)
- [下一篇:3.HarmonyOS开发术语](https://github.com/CharonChui/HarmonyOSNextStudyNote/blob/main/3.HarmonyOS%E5%BC%80%E5%8F%91%E6%9C%AF%E8%AF%AD.md)

    
---

- 邮箱 ：charon.chui@gmail.com  
- Good Luck! 


---

