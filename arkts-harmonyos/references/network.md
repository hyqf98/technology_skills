# 网络请求 (Network Request)

HarmonyOS 提供了完善的网络请求能力，支持 HTTP/HTTPS 协议、WebSocket、上传下载等功能。本文档详细介绍 HarmonyOS NEXT 的网络请求相关技术。

## 目录

- [HTTP 请求](#http-请求)
- [WebSocket](#websocket)
- [文件上传下载](#文件上传下载)
- [网络状态监听](#网络状态监听)
- [最佳实践](#最佳实践)

## HTTP 请求

### 基本用法

HarmonyOS 使用 `@kit.NetworkKit` 中的 HTTP 模块进行网络请求。

#### 1. 导入模块

```typescript
import http from '@kit.NetworkKit';
```

#### 2. 创建 HTTP 请求对象

```typescript
// 创建 httpRequest 对象
let httpRequest = http.createHttp();

// 注意：每个 httpRequest 对象对应一个 HTTP 请求任务，不可复用
```

#### 3. 发起 GET 请求

```typescript
async function getRequest(url: string) {
  let httpRequest = http.createHttp();

  try {
    // 订阅响应头（可选）
    httpRequest.on('headersReceive', (header) => {
      console.info('响应头:', JSON.stringify(header));
    });

    // 发起 GET 请求
    let response = await httpRequest.request(url, {
      method: http.RequestMethod.GET,
      // 连接超时时间，单位毫秒，默认 60000
      connectTimeout: 60000,
      // 读取超时时间，单位毫秒，默认 60000
      readTimeout: 60000,
      // 请求头
      header: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      // 期望接收的响应数据类型，默认为 HTTPDataType.STRING
      expectDataType: http.HttpDataType.STRING,
      // 是否使用证书验证，默认为 true
      caPath: '',
      // 客户端证书配置
      clientCert: {
        certPath: '',
        keyPath: ''
      }
    });

    // 处理响应
    if (response.responseCode === http.ResponseCode.OK) {
      console.info('请求成功:', response.result);
      return response.result as string;
    } else {
      console.error('请求失败:', response.responseCode);
      return null;
    }
  } catch (error) {
    console.error('请求异常:', JSON.stringify(error));
    return null;
  } finally {
    // 销毁请求对象
    httpRequest.destroy();
  }
}

// 使用示例
getRequest('https://api.example.com/data').then(data => {
  if (data) {
    console.info('获取数据成功:', data);
  }
});
```

#### 4. 发起 POST 请求

```typescript
async function postRequest(url: string, data: Object) {
  let httpRequest = http.createHttp();

  try {
    let response = await httpRequest.request(url, {
      method: http.RequestMethod.POST,
      header: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      // POST 请求的额外数据
      extraData: JSON.stringify(data),
      connectTimeout: 60000,
      readTimeout: 60000
    });

    if (response.responseCode === http.ResponseCode.OK) {
      console.info('POST 请求成功:', response.result);
      return response.result as string;
    } else {
      console.error('POST 请求失败:', response.responseCode);
      return null;
    }
  } catch (error) {
    console.error('POST 请求异常:', JSON.stringify(error));
    return null;
  } finally {
    httpRequest.destroy();
  }
}

// 使用示例
postRequest('https://api.example.com/users', {
  name: '张三',
  age: 25
}).then(data => {
  if (data) {
    console.info('创建用户成功:', data);
  }
});
```

### 完整示例

```typescript
import http from '@kit.NetworkKit';

@Entry
@Component
struct NetworkDemo {
  @State responseData: string = '';
  @State isLoading: boolean = false;

  // GET 请求示例
  async fetchData() {
    this.isLoading = true;
    let httpRequest = http.createHttp();

    try {
      // 构建请求 URL，可以添加查询参数
      let url = 'https://api.example.com/data';
      url += '?param1=value1&param2=value2';

      // 订阅响应头
      httpRequest.on('headersReceive', (header) => {
        console.info('响应头:', JSON.stringify(header));
      });

      let response = await httpRequest.request(url, {
        method: http.RequestMethod.GET,
        header: {
          'Content-Type': 'application/json'
        },
        connectTimeout: 60000,
        readTimeout: 60000,
        expectDataType: http.HttpDataType.STRING
      });

      if (response.responseCode === http.ResponseCode.OK) {
        this.responseData = response.result as string;
        console.info('请求成功:', this.responseData);
      } else {
        this.responseData = `请求失败: ${response.responseCode}`;
      }
    } catch (error) {
      this.responseData = `请求异常: ${JSON.stringify(error)}`;
      console.error('请求异常:', JSON.stringify(error));
    } finally {
      httpRequest.destroy();
      this.isLoading = false;
    }
  }

  // POST 请求示例
  async postData() {
    this.isLoading = true;
    let httpRequest = http.createHttp();

    try {
      let response = await httpRequest.request(
        'https://api.example.com/users',
        {
          method: http.RequestMethod.POST,
          header: {
            'Content-Type': 'application/json'
          },
          extraData: JSON.stringify({
            name: '张三',
            age: 25,
            email: 'zhangsan@example.com'
          }),
          connectTimeout: 60000,
          readTimeout: 60000
        }
      );

      if (response.responseCode === http.ResponseCode.OK) {
        this.responseData = response.result as string;
        console.info('POST 请求成功:', this.responseData);
      } else {
        this.responseData = `POST 请求失败: ${response.responseCode}`;
      }
    } catch (error) {
      this.responseData = `POST 请求异常: ${JSON.stringify(error)}`;
    } finally {
      httpRequest.destroy();
      this.isLoading = false;
    }
  }

  build() {
    Column({ space: 20 }) {
      Text('网络请求示例')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)

      Row({ space: 10 }) {
        Button('GET 请求')
          .onClick(() => {
            this.fetchData();
          })
          .enabled(!this.isLoading)

        Button('POST 请求')
          .onClick(() => {
            this.postData();
          })
          .enabled(!this.isLoading)
      }

      if (this.isLoading) {
        LoadingProgress()
          .width(50)
          .height(50)
      }

      Scroll() {
        Text(this.responseData)
          .fontSize(14)
          .fontColor(Color.Gray)
      }
      .width('100%')
      .layoutWeight(1)
      .padding(10)
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}
```

### HTTP 请求方法对比

| 方法 | 说明 | 使用场景 |
|------|------|----------|
| GET | 获取资源 | 查询数据、获取页面 |
| POST | 创建资源 | 提交表单、创建数据 |
| PUT | 更新资源 | 更新完整数据 |
| DELETE | 删除资源 | 删除数据 |
| PATCH | 部分更新 | 更新部分数据 |

### 响应处理

```typescript
// 处理不同类型的响应
async handleResponse(url: string) {
  let httpRequest = http.createHttp();

  try {
    // 期望接收 JSON 字符串
    let response = await httpRequest.request(url, {
      method: http.RequestMethod.GET,
      expectDataType: http.HttpDataType.STRING
    });

    if (response.responseCode === http.ResponseCode.OK) {
      // 解析 JSON 响应
      let jsonData = JSON.parse(response.result as string);
      console.info('解析后的数据:', JSON.stringify(jsonData));

      return jsonData;
    }
  } catch (error) {
    console.error('处理响应失败:', JSON.stringify(error));
  } finally {
    httpRequest.destroy();
  }
}

// 处理二进制数据
async handleBinaryData(url: string) {
  let httpRequest = http.createHttp();

  try {
    // 期望接收 ArrayBuffer
    let response = await httpRequest.request(url, {
      method: http.RequestMethod.GET,
      expectDataType: http.HttpDataType.ARRAY_BUFFER
    });

    if (response.responseCode === http.ResponseCode.OK) {
      let arrayBuffer = response.result as ArrayBuffer;
      console.info('接收到的数据长度:', arrayBuffer.byteLength);

      return arrayBuffer;
    }
  } catch (error) {
    console.error('处理二进制数据失败:', JSON.stringify(error));
  } finally {
    httpRequest.destroy();
  }
}
```

## WebSocket

WebSocket 提供了全双工通信能力，适合实时通信场景。

### 基本用法

```typescript
import webSocket from '@kit.NetworkKit';

// 创建 WebSocket 连接
let ws = webSocket.createWebSocket();

// 连接到 WebSocket 服务器
ws.connect('ws://example.com/socket', (err, value) => {
  if (!err) {
    console.info('WebSocket 连接成功');
    // 连接成功后的操作
  } else {
    console.error('WebSocket 连接失败:', JSON.stringify(err));
  }
});

// 监听 WebSocket 消息
ws.on('message', (err, value) => {
  if (!err) {
    console.info('收到消息:', value);
    // 处理收到的消息
  }
});

// 监听连接打开事件
ws.on('open', (err, value) => {
  if (!err) {
    console.info('WebSocket 连接已打开');
  }
});

// 监听连接关闭事件
ws.on('close', (err, value) => {
  if (!err) {
    console.info('WebSocket 连接已关闭');
  }
});

// 监听错误事件
ws.on('error', (err) => {
  console.error('WebSocket 错误:', JSON.stringify(err));
});

// 发送消息
ws.send('Hello WebSocket', (err) => {
  if (!err) {
    console.info('消息发送成功');
  } else {
    console.error('消息发送失败:', JSON.stringify(err));
  }
});

// 关闭连接
ws.close((err) => {
  if (!err) {
    console.info('WebSocket 连接关闭成功');
  }
});
```

### 完整示例

```typescript
import webSocket from '@kit.NetworkKit';

@Entry
@Component
struct WebSocketDemo {
  @State messages: string[] = [];
  @State isConnected: boolean = false;
  @State inputMessage: string = '';

  private ws: webSocket.WebSocket = webSocket.createWebSocket();

  aboutToAppear() {
    this.connectWebSocket();
  }

  aboutToDisappear() {
    this.closeWebSocket();
  }

  connectWebSocket() {
    this.ws.on('open', (err, value) => {
      if (!err) {
        this.isConnected = true;
        console.info('WebSocket 连接已打开');
      }
    });

    this.ws.on('message', (err, value) => {
      if (!err) {
        this.messages.push(`收到: ${value}`);
        console.info('收到消息:', value);
      }
    });

    this.ws.on('close', (err, value) => {
      if (!err) {
        this.isConnected = false;
        console.info('WebSocket 连接已关闭');
      }
    });

    this.ws.on('error', (err) => {
      console.error('WebSocket 错误:', JSON.stringify(err));
      this.isConnected = false;
    });

    this.ws.connect('ws://echo.websocket.org', (err, value) => {
      if (!err) {
        console.info('WebSocket 连接成功');
      } else {
        console.error('WebSocket 连接失败:', JSON.stringify(err));
      }
    });
  }

  sendMessage() {
    if (!this.isConnected || !this.inputMessage) {
      return;
    }

    this.ws.send(this.inputMessage, (err) => {
      if (!err) {
        this.messages.push(`发送: ${this.inputMessage}`);
        this.inputMessage = '';
        console.info('消息发送成功');
      } else {
        console.error('消息发送失败:', JSON.stringify(err));
      }
    });
  }

  closeWebSocket() {
    this.ws.close((err) => {
      if (!err) {
        console.info('WebSocket 连接关闭成功');
      }
    });
  }

  build() {
    Column({ space: 20 }) {
      Text('WebSocket 示例')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)

      Text(`连接状态: ${this.isConnected ? '已连接' : '未连接'}`)
        .fontColor(this.isConnected ? Color.Green : Color.Red)

      Row({ space: 10 }) {
        TextInput({ placeholder: '请输入消息', text: this.inputMessage })
          .layoutWeight(1)
          .onChange((value: string) => {
            this.inputMessage = value;
          })
          .enabled(this.isConnected)

        Button('发送')
          .onClick(() => {
            this.sendMessage();
          })
          .enabled(this.isConnected && this.inputMessage.length > 0)
      }
      .width('100%')

      Divider()

      Text('消息记录')
        .fontSize(18)
        .fontWeight(FontWeight.Bold)

      List() {
        ForEach(this.messages, (message: string) => {
          ListItem() {
            Text(message)
              .fontSize(14)
          }
        }, (message: string) => message)
      }
      .width('100%')
      .layoutWeight(1)
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}
```

## 文件上传下载

### 文件上传

```typescript
import http from '@kit.NetworkKit';
import request from '@ohos.request';

// 使用 HTTP 上传文件
async function uploadFile(serverUrl: string, filePath: string) {
  let httpRequest = http.createHttp();

  try {
    // 读取文件内容
    let file = await fs.readFile(filePath);

    let response = await httpRequest.request(serverUrl, {
      method: http.RequestMethod.POST,
      header: {
        'Content-Type': 'multipart/form-data'
      },
      extraData: {
        file: {
          fileName: 'upload.jpg',
          mimeType: 'image/jpeg',
          data: file
        }
      }
    });

    if (response.responseCode === http.ResponseCode.OK) {
      console.info('文件上传成功');
      return true;
    }
  } catch (error) {
    console.error('文件上传失败:', JSON.stringify(error));
  } finally {
    httpRequest.destroy();
  }

  return false;
}

// 使用 request 模块上传大文件
async function uploadLargeFile(serverUrl: string, filePath: string) {
  let uploadConfig: request.upload.Config = {
    url: serverUrl,
    method: 'POST',
    header: {
      'Content-Type': 'multipart/form-data'
    },
    files: [
      {
        filename: 'large_file.zip',
        name: 'file',
        uri: `file://${filePath}`,
        type: 'zip'
      }
    ]
  };

  try {
    let uploadTask = await request.uploadFile(getContext(), uploadConfig);

    // 监听上传进度
    uploadTask.on('progress', (uploadedSize, totalSize) => {
      console.info(`上传进度: ${(uploadedSize / totalSize * 100).toFixed(2)}%`);
    });

    // 等待上传完成
    let uploadResult = await uploadTask.complete();
    console.info('文件上传完成:', uploadResult);
  } catch (error) {
    console.error('文件上传失败:', JSON.stringify(error));
  }
}
```

### 文件下载

```typescript
import request from '@ohos.request';

// 下载文件
async function downloadFile(url: string, savePath: string) {
  let downloadConfig: request.download.Config = {
    url: url,
    filePath: savePath
  };

  try {
    let downloadTask = await request.downloadFile(getContext(), downloadConfig);

    // 监听下载进度
    downloadTask.on('progress', (receivedSize, totalSize) => {
      console.info(`下载进度: ${(receivedSize / totalSize * 100).toFixed(2)}%`);
    });

    // 等待下载完成
    let downloadResult = await downloadTask.complete();
    console.info('文件下载完成:', downloadResult);

    return downloadResult;
  } catch (error) {
    console.error('文件下载失败:', JSON.stringify(error));
    return null;
  }
}

@Entry
@Component
struct DownloadDemo {
  @State downloadProgress: number = 0;
  @State isDownloading: boolean = false;

  async startDownload() {
    this.isDownloading = true;
    this.downloadProgress = 0;

    let url = 'https://example.com/largefile.zip';
    let savePath = '/data/storage/el2/base/haps/entry/files/download.zip';

    try {
      let downloadTask = await request.downloadFile(getContext(), {
        url: url,
        filePath: savePath
      });

      // 监听下载进度
      downloadTask.on('progress', (receivedSize, totalSize) => {
        this.downloadProgress = Math.floor(receivedSize / totalSize * 100);
        console.info(`下载进度: ${this.downloadProgress}%`);
      });

      await downloadTask.complete();
      console.info('文件下载完成');
    } catch (error) {
      console.error('文件下载失败:', JSON.stringify(error));
    } finally {
      this.isDownloading = false;
    }
  }

  build() {
    Column({ space: 20 }) {
      Text('文件下载示例')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)

      if (this.isDownloading) {
        Column({ space: 10 }) {
          Text(`下载进度: ${this.downloadProgress}%`)
            .fontSize(18)

          Progress({
            value: this.downloadProgress,
            total: 100,
            type: ProgressType.Linear
          })
            .width('100%')
        }
      }

      Button('开始下载')
        .onClick(() => {
          this.startDownload();
        })
        .enabled(!this.isDownloading)
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}
```

## 网络状态监听

### 获取网络状态

```typescript
import connection from '@kit.NetworkKit';
import wifi from '@kit.NetworkKit';

// 获取网络连接类型
async function getNetworkType() {
  let netType = await connection.getDefaultNet();
  console.info('默认网络:', JSON.stringify(netType));
}

// 监听网络状态变化
function observeNetworkChange() {
  connection.getDefaultNet().then((netHandle) => {
    connection.on('netAvailable', (data) => {
      console.info('网络可用:', JSON.stringify(data));
    });

    connection.on('netCapabilitiesChange', (data) => {
      console.info('网络能力变化:', JSON.stringify(data));
    });

    connection.on('netConnectionPropertiesChange', (data) => {
      console.info('网络连接属性变化:', JSON.stringify(data));
    });

    connection.on('netLost', (data) => {
      console.info('网络丢失:', JSON.stringify(data));
    });
  });
}

// 获取网络详细信息
async function getNetworkDetails() {
  let netHandle = await connection.getDefaultNet();
  let networkProperties = await connection.getNetworkProperties(netHandle);

  console.info('网络详细信息:', JSON.stringify(networkProperties));

  // 获取网络类型
  let networkType = connection.getNetworkType(networkProperties);
  console.info('网络类型:', networkType);
}
```

### 完整示例

```typescript
import connection from '@kit.NetworkKit';

@Entry
@Component
struct NetworkStatusDemo {
  @State networkStatus: string = '未知';
  @State networkType: string = '未知';

  aboutToAppear() {
    this.checkNetworkStatus();
    this.observeNetworkChange();
  }

  async checkNetworkStatus() {
    try {
      let netHandle = await connection.getDefaultNet();
      let hasNetCap = await connection.hasDefaultNet();

      if (hasNetCap) {
        this.networkStatus = '已连接';
        let networkProperties = await connection.getNetworkProperties(netHandle);
        this.networkType = this.getNetworkTypeName(networkProperties);
      } else {
        this.networkStatus = '未连接';
        this.networkType = '无';
      }
    } catch (error) {
      console.error('检查网络状态失败:', JSON.stringify(error));
    }
  }

  observeNetworkChange() {
    // 监听网络可用性变化
    connection.on('netAvailable', (data) => {
      this.networkStatus = '网络可用';
      console.info('网络可用:', JSON.stringify(data));
    });

    // 监听网络丢失
    connection.on('netLost', (data) => {
      this.networkStatus = '网络丢失';
      this.networkType = '无';
      console.info('网络丢失:', JSON.stringify(data));
    });

    // 监听网络能力变化
    connection.on('netCapabilitiesChange', async (data) => {
      console.info('网络能力变化:', JSON.stringify(data));
      await this.checkNetworkStatus();
    });
  }

  getNetworkTypeName(properties: connection.ConnectionProperties): string {
    let networkType = connection.getNetworkType(properties);

    switch (networkType) {
      case connection.NetworkType.CELLULAR:
        return '蜂窝网络';
      case connection.NetworkType.WIFI:
        return 'WiFi';
      case connection.NetworkType.ETHERNET:
        return '以太网';
      case connection.NetworkType.VPN:
        return 'VPN';
      case connection.NetworkType.UNKNOWN:
      default:
        return '未知';
    }
  }

  build() {
    Column({ space: 20 }) {
      Text('网络状态示例')
        .fontSize(24)
        .fontWeight(FontWeight.Bold)

      Row({ space: 20 }) {
        Text('网络状态:')
          .fontSize(18)

        Text(this.networkStatus)
          .fontSize(18)
          .fontColor(this.networkStatus === '已连接' ? Color.Green : Color.Red)
      }

      Row({ space: 20 }) {
        Text('网络类型:')
          .fontSize(18)

        Text(this.networkType)
          .fontSize(18)
      }

      Button('刷新状态')
        .onClick(() => {
          this.checkNetworkStatus();
        })
    }
    .width('100%')
    .height('100%')
    .padding(20)
  }
}
```

## 最佳实践

### 1. 网络请求封装

```typescript
import http from '@kit.NetworkKit';

// 封装 HTTP 请求类
class HttpClient {
  private static instance: HttpClient;
  private baseUrl: string = '';

  private constructor() {}

  static getInstance(): HttpClient {
    if (!HttpClient.instance) {
      HttpClient.instance = new HttpClient();
    }
    return HttpClient.instance;
  }

  setBaseUrl(url: string) {
    this.baseUrl = url;
  }

  async request<T>(options: {
    method: http.RequestMethod;
    url: string;
    data?: Object;
    header?: Object;
  }): Promise<T | null> {
    let httpRequest = http.createHttp();

    try {
      let fullUrl = this.baseUrl + options.url;
      let response = await httpRequest.request(fullUrl, {
        method: options.method,
        header: {
          'Content-Type': 'application/json',
          ...options.header
        },
        extraData: options.data ? JSON.stringify(options.data) : undefined,
        connectTimeout: 60000,
        readTimeout: 60000,
        expectDataType: http.HttpDataType.STRING
      });

      if (response.responseCode === http.ResponseCode.OK) {
        return JSON.parse(response.result as string) as T;
      } else {
        console.error('请求失败:', response.responseCode);
        return null;
      }
    } catch (error) {
      console.error('请求异常:', JSON.stringify(error));
      return null;
    } finally {
      httpRequest.destroy();
    }
  }

  async get<T>(url: string, header?: Object): Promise<T | null> {
    return this.request<T>({
      method: http.RequestMethod.GET,
      url,
      header
    });
  }

  async post<T>(url: string, data: Object, header?: Object): Promise<T | null> {
    return this.request<T>({
      method: http.RequestMethod.POST,
      url,
      data,
      header
    });
  }
}

// 使用示例
let httpClient = HttpClient.getInstance();
httpClient.setBaseUrl('https://api.example.com');

// GET 请求
let data = await httpClient.get<UserInfo>('/user/123');

// POST 请求
let result = await httpClient.post<Response>('/users', {
  name: '张三',
  age: 25
});
```

### 2. 错误处理

```typescript
// 统一的错误处理
class ApiError {
  code: number;
  message: string;

  constructor(code: number, message: string) {
    this.code = code;
    this.message = message;
  }

  static fromResponse(response: http.HttpResponse): ApiError {
    return new ApiError(response.responseCode, response.result as string);
  }
}

async function safeRequest<T>(requestFn: () => Promise<T>): Promise<T> {
  try {
    return await requestFn();
  } catch (error) {
    if (error instanceof ApiError) {
      // 处理 API 错误
      console.error('API 错误:', error.message);

      // 根据错误码进行不同处理
      switch (error.code) {
        case 401:
          // 未授权，跳转到登录页
          break;
        case 403:
          // 权限不足
          break;
        case 404:
          // 资源不存在
          break;
        case 500:
          // 服务器错误
          break;
      }
    } else {
      // 处理其他错误
      console.error('未知错误:', JSON.stringify(error));
    }

    throw error;
  }
}
```

### 3. 请求重试

```typescript
// 带重试机制的请求
async function requestWithRetry<T>(
  requestFn: () => Promise<T>,
  maxRetries: number = 3,
  delay: number = 1000
): Promise<T> {
  let lastError: Error;

  for (let i = 0; i < maxRetries; i++) {
    try {
      return await requestFn();
    } catch (error) {
      lastError = error as Error;
      console.error(`请求失败，第 ${i + 1} 次重试:`, JSON.stringify(error));

      // 如果不是最后一次重试，等待一段时间
      if (i < maxRetries - 1) {
        await new Promise(resolve => setTimeout(resolve, delay));
        delay *= 2; // 指数退避
      }
    }
  }

  throw lastError;
}
```

### 4. 缓存策略

```typescript
// 简单的内存缓存
class RequestCache {
  private static cache: Map<string, { data: any; timestamp: number }> = new Map();
  private static maxAge: number = 5 * 60 * 1000; // 5 分钟

  static get(key: string): any | null {
    let cached = this.cache.get(key);
    if (cached && Date.now() - cached.timestamp < this.maxAge) {
      return cached.data;
    }
    return null;
  }

  static set(key: string, data: any): void {
    this.cache.set(key, {
      data,
      timestamp: Date.now()
    });
  }

  static clear(): void {
    this.cache.clear();
  }
}

// 使用缓存的请求
async function cachedRequest<T>(url: string): Promise<T> {
  // 先检查缓存
  let cachedData = RequestCache.get(url);
  if (cachedData) {
    console.info('使用缓存数据:', url);
    return cachedData as T;
  }

  // 缓存未命中，发起网络请求
  let data = await httpClient.get<T>(url);

  // 存入缓存
  if (data) {
    RequestCache.set(url, data);
  }

  return data;
}
```

### 5. 请求队列

```typescript
// 请求队列管理
class RequestQueue {
  private static queue: Array<() => Promise<any>> = [];
  private static isProcessing: boolean = false;
  private static maxConcurrent: number = 5; // 最大并发数

  static async add(requestFn: () => Promise<any>): Promise<any> {
    return new Promise((resolve, reject) => {
      this.queue.push(async () => {
        try {
          let result = await requestFn();
          resolve(result);
        } catch (error) {
          reject(error);
        }
      });

      this.process();
    });
  }

  private static async process() {
    if (this.isProcessing || this.queue.length === 0) {
      return;
    }

    this.isProcessing = true;

    let concurrent = Math.min(this.queue.length, this.maxConcurrent);

    let promises = [];
    for (let i = 0; i < concurrent; i++) {
      let requestFn = this.queue.shift();
      if (requestFn) {
        promises.push(requestFn());
      }
    }

    await Promise.all(promises);

    this.isProcessing = false;

    // 继续处理队列
    if (this.queue.length > 0) {
      this.process();
    }
  }
}
```

## 常见问题

### Q1: 如何处理网络请求的超时？

**答**：在请求配置中设置 `connectTimeout` 和 `readTimeout` 参数：

```typescript
let response = await httpRequest.request(url, {
  connectTimeout: 10000,  // 连接超时 10 秒
  readTimeout: 30000,     // 读取超时 30 秒
  // ...其他配置
});
```

### Q2: 如何处理 HTTPS 证书验证？

**答**：可以通过 `caPath` 参数指定证书路径，或者设置 `ignoreSSLError` 忽略证书错误（仅用于开发环境）：

```typescript
let response = await httpRequest.request(url, {
  // 指定 CA 证书路径
  caPath: '/path/to/ca.crt',

  // 或者忽略 SSL 错误（不推荐用于生产环境）
  // ignoreSSLError: true
});
```

### Q3: 如何实现请求取消？

**答**：使用 `request.destroy()` 方法可以取消请求：

```typescript
let httpRequest = http.createHttp();

// 发起请求
let promise = httpRequest.request(url, options);

// 取消请求
httpRequest.destroy();

try {
  await promise;
} catch (error) {
  console.error('请求已取消');
}
```

## 总结

HarmonyOS 提供了完善的网络请求能力，包括：

1. **HTTP/HTTPS 请求**：支持 GET、POST 等多种方法
2. **WebSocket**：全双工实时通信
3. **文件上传下载**：支持大文件传输
4. **网络状态监听**：实时监控网络状态变化
5. **完善的 API**：提供丰富的配置选项和事件回调

合理使用网络请求功能，遵循最佳实践，可以构建高效、稳定的网络应用。

## 参考资源

- [HTTP 网络请求官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/http-network-request-V5)
- [WebSocket 官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/websocket-V5)
- [上传下载官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/file-upload-download-V5)
- [网络连接管理官方文档](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/network-connectivity-V5)
