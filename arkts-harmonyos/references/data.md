# 数据管理 (Data Management)

HarmonyOS 提供了多种数据管理方式，包括应用级状态管理、本地数据存储、分布式数据等。本文档详细介绍 HarmonyOS NEXT 的数据管理相关技术。

## 目录

- [应用状态管理](#应用状态管理)
- [本地数据存储](#本地数据存储)
- [关系型数据库](#关系型数据库)
- [分布式数据](#分布式数据)
- [数据模型](#数据模型)
- [最佳实践](#最佳实践)

## 应用状态管理

应用状态管理是 HarmonyOS 开发中的核心概念，用于管理应用中的数据状态。ArkTS 提供了多种状态管理装饰器，支持不同范围和层级的状态共享。

### 1. LocalStorage - 页面级状态管理

LocalStorage 是页面级的 UI 状态存储，通常用于 UIAbility 内、页面间的状态共享。

#### 使用场景

- 页面内部组件间共享状态
- UIAbility 内多个页面共享状态
- 需要在页面间传递的临时数据

#### 基本用法

```typescript
// 1. 创建 LocalStorage 实例
let storage = new LocalStorage();

// 2. 使用 @Entry 装饰器传入 LocalStorage
@Entry(storage)
@Component
struct MyPage {
  // 3. 使用 @LocalStorageProp 装饰器创建单向同步变量
  @LocalStorageProp('userName') userName: string = '';
  @LocalStorageProp('userAge') userAge: number = 0;

  // 4. 使用 @LocalStorageLink 装饰器创建双向同步变量
  @LocalStorageLink('isLoggedIn') isLoggedIn: boolean = false;

  build() {
    Column() {
      Text(`用户名: ${this.userName}`)
      Text(`年龄: ${this.userAge}`)
      Text(`登录状态: ${this.isLoggedIn ? '已登录' : '未登录'}`)

      Button('更新状态')
        .onClick(() => {
          this.userName = '张三';
          this.userAge = 25;
          this.isLoggedIn = true;
        })
    }
  }
}

// 5. 设置和获取 LocalStorage 中的数据
storage.setOrCreate('userName', '李四');
storage.setOrCreate('userAge', 30);
```

#### 完整示例

```typescript
// 创建 LocalStorage 并设置初始值
let paraStore = new LocalStorage();
paraStore.setOrCreate('userName', '默认用户');
paraStore.setOrCreate('isLoggedIn', false);

@Entry(paraStore)
@Component
struct Index {
  @LocalStorageProp('userName') userName: string = '';
  @LocalStorageLink('isLoggedIn') isLoggedIn: boolean = false;

  build() {
    Column({ space: 20 }) {
      Text(`欢迎, ${this.userName}`)
        .fontSize(24)

      if (this.isLoggedIn) {
        Text('您已登录')
          .fontColor(Color.Green)
      } else {
        Text('您未登录')
          .fontColor(Color.Red)
      }

      Button(this.isLoggedIn ? '退出登录' : '登录')
        .onClick(() => {
          this.isLoggedIn = !this.isLoggedIn;
          if (this.isLoggedIn) {
            this.userName = '张三';
          } else {
            this.userName = '游客';
          }
        })
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}
```

### 2. AppStorage - 应用级状态管理

AppStorage 是应用全局的 UI 状态存储，是应用级的全局状态共享中心。

#### 使用场景

- 应用全局配置
- 用户信息
- 主题设置
- 多页面需要访问的数据

#### 基本用法

```typescript
// 1. 使用静态方法设置和获取 AppStorage 数据
AppStorage.setOrCreate('appTheme', 'light');
AppStorage.setOrCreate('currentUser', null);

// 2. 使用 @StorageLink 装饰器创建双向同步变量
@Component
struct MyComponent {
  @StorageLink('appTheme') appTheme: string = 'light';

  build() {
    Column() {
      Text(`当前主题: ${this.appTheme}`)
      Button('切换主题')
        .onClick(() => {
          this.appTheme = this.appTheme === 'light' ? 'dark' : 'light';
        })
    }
  }
}

// 3. 使用 @StorageProp 装饰器创建单向同步变量
@Component
struct ReadOnlyComponent {
  @StorageProp('appTheme') appTheme: string = 'light';

  build() {
    Text(`主题: ${this.appTheme}`)
  }
}
```

#### 完整示例

```typescript
// 应用初始化时设置全局状态
AppStorage.setOrCreate('globalCounter', 0);
AppStorage.setOrCreate('globalMessage', 'Hello HarmonyOS');

@Entry
@Component
struct AppStorageDemo {
  @StorageLink('globalCounter') counter: number = 0;
  @StorageLink('globalMessage') message: string = '';

  build() {
    Column({ space: 20 }) {
      Text(`计数器: ${this.counter}`)
        .fontSize(24)

      Text(`消息: ${this.message}`)
        .fontSize(18)

      Row({ space: 10 }) {
        Button('增加')
          .onClick(() => {
            this.counter++;
          })

        Button('减少')
          .onClick(() => {
            this.counter--;
          })
      }

      Button('更新消息')
        .onClick(() => {
          this.message = `更新时间: ${new Date().toLocaleTimeString()}`;
        })

      // 使用子组件，也能访问到 AppStorage 中的数据
      ChildComponent()
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}

@Component
struct ChildComponent {
  @StorageLink('globalCounter') counter: number = 0;

  build() {
    Text(`子组件计数器: ${this.counter}`)
      .fontSize(16)
      .fontColor(Color.Blue)
  }
}
```

### 3. PersistentStorage - 持久化存储

PersistentStorage 用于持久化存储应用状态，确保应用重启后数据不丢失。

#### 使用场景

- 用户设置（如主题、语言）
- 应用配置
- 需要持久化的状态数据

#### 基本用法

```typescript
// 1. 持久化 AppStorage 中的数据
PersistentStorage.persistProp('userSettings', '{}');
PersistentStorage.persistProp('isFirstLaunch', true);

// 2. 结合 AppStorage 使用
@Component
struct SettingsComponent {
  @StorageLink('userSettings') userSettings: string = '{}';
  @StorageLink('isFirstLaunch') isFirstLaunch: boolean = true;

  aboutToAppear() {
    // 从持久化存储中读取数据
    console.log('用户设置:', this.userSettings);
    console.log('首次启动:', this.isFirstLaunch);

    // 更新首次启动标记
    if (this.isFirstLaunch) {
      this.isFirstLaunch = false;
      this.showWelcomeGuide();
    }
  }

  private showWelcomeGuide() {
    // 显示欢迎引导
  }

  build() {
    Column() {
      // UI 实现
    }
  }
}
```

#### 支持的数据类型

PersistentStorage 支持以下数据类型：

- 基本类型：number, string, boolean
- 枚举类型
- 可被 JSON 序列化的对象

#### 限制条件

```typescript
// 不支持的类型示例
// ❌ 不支持嵌套对象
let invalidData1 = {
  user: {
    name: '张三',
    age: 25
  }
};

// ❌ 不支持 undefined 和 null
let invalidData2 = undefined;
let invalidData3 = null;

// ❌ 不支持复杂对象方法
class MyClass {
  name: string = '';
  method() {
    // 方法不会被持久化
  }
}

// ✅ 支持的简单对象
let validData = {
  name: '张三',
  age: 25,
  city: '北京'
};
```

### 4. Environment - 环境参数

Environment 提供设备环境参数，如语言、颜色模式、屏幕方向等。

#### 常用环境参数

```typescript
@Component
struct EnvironmentDemo {
  @StorageLink('language') language: string = 'zh-Hans';
  @StorageLink('colorMode') colorMode: string = 'light';

  build() {
    Column({ space: 20 }) {
      Text(`当前语言: ${this.language}`)
      Text(`颜色模式: ${this.colorMode}`)

      // 获取更多环境信息
      Button('显示环境信息')
        .onClick(() => {
          let config = AppStorage.get('configuration') as Configuration;
          console.log('设备配置:', JSON.stringify(config));
        })
    }
  }
}
```

### 5. 状态管理装饰器对比

| 装饰器 | 作用范围 | 数据流向 | 使用场景 |
|--------|----------|----------|----------|
| @State | 组件内 | 组件内部 | 组件私有状态 |
| @Prop | 父子组件 | 单向同步 | 父组件向子组件传递数据 |
| @Link | 父子组件 | 双向同步 | 父子组件数据双向绑定 |
| @Provide/@Consume | 跨组件层级 | 双向同步 | 跨层级组件状态共享 |
| @LocalStorageProp | 页面内 | 单向同步 | 页面内单向数据同步 |
| @LocalStorageLink | 页面内 | 双向同步 | 页面内双向数据同步 |
| @StorageProp | 应用内 | 单向同步 | 应用内单向数据同步 |
| @StorageLink | 应用内 | 双向同步 | 应用内双向数据同步 |
| @Observed/@ObjectLink | 嵌套对象 | 双向同步 | 复杂嵌套对象状态管理 |

## 本地数据存储

### 1. 首选项 (Preferences)

首选项提供轻量级的 Key-Value 数据存储能力。

#### 基本用法

```typescript
import { preferences } from '@kit.ArkData';

// 获取首选项实例
async getPreferences() {
  try {
    let dataPreferences = await preferences.getPreferences(getContext(), 'mystore');
    return dataPreferences;
  } catch (err) {
    console.error('获取首选项失败', JSON.stringify(err));
    return null;
  }
}

// 保存数据
async saveData(key: string, value: preferences.ValueType): Promise<void> {
  let dataPreferences = await this.getPreferences();
  if (dataPreferences) {
    try {
      await dataPreferences.put(key, value);
      await dataPreferences.flush();
      console.info('保存数据成功');
    } catch (err) {
      console.error('保存数据失败', JSON.stringify(err));
    }
  }
}

// 读取数据
async getData(key: string): Promise<preferences.ValueType | undefined> {
  let dataPreferences = await this.getPreferences();
  if (dataPreferences) {
    try {
      let value = await dataPreferences.get(key, '');
      console.info('读取数据成功:', value);
      return value;
    } catch (err) {
      console.error('读取数据失败', JSON.stringify(err));
      return undefined;
    }
  }
  return undefined;
}

// 删除数据
async deleteData(key: string): Promise<void> {
  let dataPreferences = await this.getPreferences();
  if (dataPreferences) {
    try {
      await dataPreferences.delete(key);
      await dataPreferences.flush();
      console.info('删除数据成功');
    } catch (err) {
      console.error('删除数据失败', JSON.stringify(err));
    }
  }
}
```

#### 完整示例

```typescript
import { preferences } from '@kit.ArkData';

@Entry
@Component
struct PreferencesDemo {
  @State userName: string = '';
  @State userAge: string = '';

  aboutToAppear() {
    this.loadUserData();
  }

  async loadUserData() {
    let dataPreferences = await preferences.getPreferences(getContext(), 'user_store');

    try {
      this.userName = await dataPreferences.get('userName', '') as string;
      this.userAge = await dataPreferences.get('userAge', '') as string;
    } catch (err) {
      console.error('加载用户数据失败', JSON.stringify(err));
    }
  }

  async saveUserData() {
    let dataPreferences = await preferences.getPreferences(getContext(), 'user_store');

    try {
      await dataPreferences.put('userName', this.userName);
      await dataPreferences.put('userAge', this.userAge);
      await dataPreferences.flush();
      console.info('保存用户数据成功');
    } catch (err) {
      console.error('保存用户数据失败', JSON.stringify(err));
    }
  }

  build() {
    Column({ space: 20 }) {
      Text('用户信息')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)

      TextInput({ placeholder: '请输入用户名', text: this.userName })
        .onChange((value: string) => {
          this.userName = value;
        })

      TextInput({ placeholder: '请输入年龄', text: this.userAge })
        .type(InputType.Number)
        .onChange((value: string) => {
          this.userAge = value;
        })

      Row({ space: 10 }) {
        Button('保存')
          .onClick(() => {
            this.saveUserData();
          })

        Button('加载')
          .onClick(() => {
            this.loadUserData();
          })

        Button('清除')
          .onClick(() => {
            this.userName = '';
            this.userAge = '';
          })
      }
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}
```

### 2. 关系型数据库 (RDB)

HarmonyOS 提供了基于 SQLite 的关系型数据库功能。

#### 创建数据库

```typescript
import { relationalStore } from '@kit.ArkData';

// 数据库配置
const STORE_CONFIG: relationalStore.StoreConfig = {
  name: 'MyDatabase.db',  // 数据库文件名
  securityLevel: relationalStore.SecurityLevel.S1  // 安全级别
};

// 创建数据库表
const SQL_CREATE_TABLE = 'CREATE TABLE IF NOT EXISTS USER (' +
  'ID INTEGER PRIMARY KEY AUTOINCREMENT, ' +
  'NAME TEXT NOT NULL, ' +
  'AGE INTEGER, ' +
  'SALARY REAL, ' +
  'CODES BLOB)';

async createDatabase(context: Context) {
  try {
    // 获取 RdbStore 实例
    let rdbStore = await relationalStore.getRdbStore(context, STORE_CONFIG);

    // 创建表
    await rdbStore.executeSql(SQL_CREATE_TABLE);

    console.info('创建数据库成功');
    return rdbStore;
  } catch (err) {
    console.error('创建数据库失败', JSON.stringify(err));
    return null;
  }
}
```

#### 数据库操作 (CRUD)

```typescript
// 插入数据
async insertData(rdbStore: relationalStore.RdbStore, name: string, age: number) {
  let valueBucket: relationalStore.ValuesBucket = {
    'NAME': name,
    'AGE': age,
    'SALARY': 10000.50,
    'CODES': new Uint8Array([1, 2, 3, 4, 5])
  };

  try {
    let rowId = await rdbStore.insert('USER', valueBucket);
    console.info('插入数据成功, rowId:', rowId);
    return rowId;
  } catch (err) {
    console.error('插入数据失败', JSON.stringify(err));
    return -1;
  }
}

// 查询数据
async queryData(rdbStore: relationalStore.RdbStore) {
  let predicates = new relationalStore.RdbPredicates('USER');
  predicates.equalTo('AGE', 25);

  try {
    let resultSet = await rdbStore.query(predicates, ['ID', 'NAME', 'AGE', 'SALARY']);

    console.info('查询结果行数:', resultSet.rowCount);

    while (resultSet.goToNextRow()) {
      let id = resultSet.getLong(resultSet.getColumnIndex('ID'));
      let name = resultSet.getString(resultSet.getColumnIndex('NAME'));
      let age = resultSet.getLong(resultSet.getColumnIndex('AGE'));
      let salary = resultSet.getDouble(resultSet.getColumnIndex('SALARY'));

      console.info(`ID: ${id}, NAME: ${name}, AGE: ${age}, SALARY: ${salary}`);
    }

    resultSet.close();
  } catch (err) {
    console.error('查询数据失败', JSON.stringify(err));
  }
}

// 更新数据
async updateData(rdbStore: relationalStore.RdbStore, id: number, newName: string) {
  let valueBucket: relationalStore.ValuesBucket = {
    'NAME': newName
  };

  let predicates = new relationalStore.RdbPredicates('USER');
  predicates.equalTo('ID', id);

  try {
    let rowsAffected = await rdbStore.update(valueBucket, predicates);
    console.info('更新数据成功, 影响行数:', rowsAffected);
    return rowsAffected;
  } catch (err) {
    console.error('更新数据失败', JSON.stringify(err));
    return 0;
  }
}

// 删除数据
async deleteData(rdbStore: relationalStore.RdbStore, id: number) {
  let predicates = new relationalStore.RdbPredicates('USER');
  predicates.equalTo('ID', id);

  try {
    let rowsAffected = await rdbStore.delete(predicates);
    console.info('删除数据成功, 影响行数:', rowsAffected);
    return rowsAffected;
  } catch (err) {
    console.error('删除数据失败', JSON.stringify(err));
    return 0;
  }
}
```

#### 完整示例

```typescript
import { relationalStore } from '@kit.ArkData';
import { common } from '@kit.AbilityKit';

@Entry
@Component
struct RdbDemo {
  @State userName: string = '';
  @State userAge: number = 0;
  @State userList: string[] = [];

  private rdbStore: relationalStore.RdbStore | null = null;

  aboutToAppear() {
    this.initDatabase();
  }

  async initDatabase() {
    const STORE_CONFIG: relationalStore.StoreConfig = {
      name: 'UserDatabase.db',
      securityLevel: relationalStore.SecurityLevel.S1
    };

    const SQL_CREATE_TABLE = 'CREATE TABLE IF NOT EXISTS USER (' +
      'ID INTEGER PRIMARY KEY AUTOINCREMENT, ' +
      'NAME TEXT NOT NULL, ' +
      'AGE INTEGER)';

    try {
      let context = getContext() as common.UIAbilityContext;
      this.rdbStore = await relationalStore.getRdbStore(context, STORE_CONFIG);
      await this.rdbStore.executeSql(SQL_CREATE_TABLE);
      await this.queryAllUsers();
    } catch (err) {
      console.error('初始化数据库失败', JSON.stringify(err));
    }
  }

  async insertUser() {
    if (!this.rdbStore || !this.userName) {
      return;
    }

    let valueBucket: relationalStore.ValuesBucket = {
      'NAME': this.userName,
      'AGE': this.userAge
    };

    try {
      await this.rdbStore.insert('USER', valueBucket);
      console.info('插入用户成功');
      await this.queryAllUsers();

      // 清空输入
      this.userName = '';
      this.userAge = 0;
    } catch (err) {
      console.error('插入用户失败', JSON.stringify(err));
    }
  }

  async queryAllUsers() {
    if (!this.rdbStore) {
      return;
    }

    let predicates = new relationalStore.RdbPredicates('USER');

    try {
      let resultSet = await this.rdbStore.query(predicates, ['ID', 'NAME', 'AGE']);
      this.userList = [];

      while (resultSet.goToNextRow()) {
        let id = resultSet.getLong(resultSet.getColumnIndex('ID'));
        let name = resultSet.getString(resultSet.getColumnIndex('NAME'));
        let age = resultSet.getLong(resultSet.getColumnIndex('AGE'));
        this.userList.push(`ID: ${id}, 姓名: ${name}, 年龄: ${age}`);
      }

      resultSet.close();
    } catch (err) {
      console.error('查询用户失败', JSON.stringify(err));
    }
  }

  build() {
    Column({ space: 20 }) {
      Text('用户管理')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)

      TextInput({ placeholder: '请输入用户名', text: this.userName })
        .onChange((value: string) => {
          this.userName = value;
        })

      TextInput({ placeholder: '请输入年龄', text: this.userAge.toString() })
        .type(InputType.Number)
        .onChange((value: string) => {
          this.userAge = parseInt(value) || 0;
        })

      Button('添加用户')
        .onClick(() => {
          this.insertUser();
        })

      Divider()

      Text('用户列表')
        .fontSize(18)
        .fontWeight(FontWeight.Bold)

      List() {
        ForEach(this.userList, (user: string) => {
          ListItem() {
            Text(user)
              .fontSize(16)
          }
        }, (user: string) => user)
      }
      .width('100%')
      .layoutWeight(1)

      Button('刷新')
        .onClick(() => {
          this.queryAllUsers();
        })
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}
```

## 分布式数据

HarmonyOS 支持分布式数据服务，实现跨设备的数据同步。

### 分布式数据服务 (Distributed Data)

```typescript
import { distributedData } from '@kit.ArkData';

// 创建分布式数据库
async createDistributedDatabase() {
  let config: distributedData.KVStoreConfig = {
    name: 'distributed_store',
    type: distributedData.KVStoreType.SINGLE_VERSION
  };

  try {
    let kvStore = await distributedData.createKVStore(getContext(), config);
    console.info('创建分布式数据库成功');
    return kvStore;
  } catch (err) {
    console.error('创建分布式数据库失败', JSON.stringify(err));
    return null;
  }
}

// 写入数据
async writeToDistributedStore(kvStore: distributedData.KVStore, key: string, value: string) {
  try {
    await kvStore.put(key, value);
    console.info('写入分布式数据成功');
  } catch (err) {
    console.error('写入分布式数据失败', JSON.stringify(err));
  }
}

// 读取数据
async readFromDistributedStore(kvStore: distributedData.KVStore, key: string) {
  try {
    let value = await kvStore.get(key);
    console.info('读取分布式数据成功:', value);
    return value;
  } catch (err) {
    console.error('读取分布式数据失败', JSON.stringify(err));
    return undefined;
  }
}

// 订阅数据变化
subscribeToDataChange(kvStore: distributedData.KVStore) {
  kvStore.on('dataChange', distributedData.SubscribeType.SUBSCRIBE_TYPE_ALL, (data) => {
    console.info('分布式数据变化:', JSON.stringify(data));
  });
}
```

## 数据模型

### 定义数据模型

```typescript
// 用户数据模型
export class UserModel {
  id: string = '';
  name: string = '';
  age: number = 0;
  email: string = '';
  avatar: string = '';

  constructor(id: string, name: string, age: number, email: string, avatar: string) {
    this.id = id;
    this.name = name;
    this.age = age;
    this.email = email;
    this.avatar = avatar;
  }

  // 转换为 JSON
  toJSON(): Object {
    return {
      id: this.id,
      name: this.name,
      age: this.age,
      email: this.email,
      avatar: this.avatar
    };
  }

  // 从 JSON 创建实例
  static fromJSON(json: Object): UserModel {
    return new UserModel(
      json['id'] || '',
      json['name'] || '',
      json['age'] || 0,
      json['email'] || '',
      json['avatar'] || ''
    );
  }
}

// 图片数据模型
export class ImageData {
  id: string;
  img: Resource;
  name: string;

  constructor(id: string, img: Resource, name: string) {
    this.id = id;        // 图片唯一标识
    this.img = img;      // 图片资源
    this.name = name;    // 图片名称
  }
}

// 购物车数据模型
export class CartItem {
  id: string = '';
  name: string = '';
  price: number = 0;
  quantity: number = 1;
  image: string = '';

  constructor(id: string, name: string, price: number, quantity: number, image: string) {
    this.id = id;
    this.name = name;
    this.price = price;
    this.quantity = quantity;
    this.image = image;
  }

  // 计算总价
  getTotalPrice(): number {
    return this.price * this.quantity;
  }
}
```

## 最佳实践

### 1. 选择合适的数据存储方式

| 数据类型 | 推荐存储方式 | 说明 |
|---------|-------------|------|
| 用户设置 | PersistentStorage + Preferences | 持久化存储，应用重启后保留 |
| 应用状态 | AppStorage | 应用全局状态 |
| 页面状态 | LocalStorage | 页面内状态共享 |
| 大量结构化数据 | 关系型数据库 | 复杂查询和事务支持 |
| 跨设备数据 | 分布式数据服务 | 多设备同步 |

### 2. 性能优化

```typescript
// ❌ 不好的做法 - 频繁写入
async badExample() {
  for (let i = 0; i < 1000; i++) {
    await this.saveData('key' + i, 'value' + i);
  }
}

// ✅ 好的做法 - 批量写入
async goodExample() {
  let batchData: Object = {};
  for (let i = 0; i < 1000; i++) {
    batchData['key' + i] = 'value' + i;
  }
  await this.saveBatchData(batchData);
}
```

### 3. 数据验证

```typescript
// 保存数据前进行验证
async validateAndSaveData(key: string, value: string) {
  // 验证 key
  if (!key || key.length > 80) {
    console.error('无效的 key');
    return;
  }

  // 验证 value
  if (!value || value.length > 8192) {
    console.error('无效的 value');
    return;
  }

  // 执行保存
  await this.saveData(key, value);
}
```

### 4. 错误处理

```typescript
async safeDatabaseOperation() {
  try {
    // 数据库操作
    await this.databaseOperation();
  } catch (err) {
    console.error('数据库操作失败', JSON.stringify(err));

    // 根据错误类型进行不同处理
    if (err.code === 14800010) {
      // 数据库不存在
      await this.initDatabase();
    } else if (err.code === 14800011) {
      // 数据库已损坏
      await this.recreateDatabase();
    }
  }
}
```

### 5. 内存管理

```typescript
@Entry
@Component
struct DataManagementDemo {
  @State userList: User[] = [];

  aboutToAppear() {
    this.loadData();
  }

  aboutToDisappear() {
    // 清理资源
    this.releaseResources();
  }

  async loadData() {
    // 使用懒加载
    const PAGE_SIZE = 20;
    let currentPage = 0;

    let data = await this.fetchData(currentPage, PAGE_SIZE);
    this.userList = data;
  }

  releaseResources() {
    // 清理大对象
    this.userList = [];

    // 关闭数据库连接
    if (this.rdbStore) {
      relationalStore.deleteRdbStore(getContext(), 'UserDatabase.db');
    }
  }

  build() {
    // UI 实现
  }
}
```

## 常见问题

### Q1: PersistentStorage 和 Preferences 如何选择？

**答**：
- **PersistentStorage**：用于持久化应用状态，与 AppStorage 配合使用，适合配置类数据
- **Preferences**：用于轻量级数据存储，适合 Key-Value 类型的简单数据

### Q2: 如何处理大对象存储？

**答**：
- 对于大型数据，建议使用关系型数据库
- 将大对象拆分为多个小对象
- 使用懒加载和分页加载策略

### Q3: 如何实现数据的自动备份？

**答**：
```typescript
// 使用 BackupExtensionAbility 实现自动备份
export default class BackupAbility extends BackupExtensionAbility {
  async onBackup() {
    // 备份首选项数据
    let prefs = await preferences.getPreferences(getContext(), 'mystore');
    let allData = await prefs.getAll();

    // 备份到指定位置
    await this.backupToFile(allData);
  }

  async onRestore(bundleVersion: BundleVersion) {
    // 从备份恢复数据
    let backupData = await this.restoreFromFile();

    // 恢复到首选项
    let prefs = await preferences.getPreferences(getContext(), 'mystore');
    for (let key in backupData) {
      await prefs.put(key, backupData[key]);
    }
    await prefs.flush();
  }
}
```

## 总结

HarmonyOS 提供了完善的数据管理能力，包括：

1. **应用状态管理**：LocalStorage、AppStorage、PersistentStorage、Environment
2. **本地数据存储**：首选项、关系型数据库
3. **分布式数据**：跨设备数据同步
4. **数据模型**：结构化的数据定义

合理选择数据存储方式，遵循最佳实践，可以构建高效、可靠的数据管理方案。

## 参考资源

- [应用状态管理官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/arkts-state-management-V5)
- [数据持久化官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/data-persistence-by-preferences-V5)
- [关系型数据库官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/data-persistence-by-rdb-V5)
- [分布式数据管理官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/distributed-data-sync-V5)
