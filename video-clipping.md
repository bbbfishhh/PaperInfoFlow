# 最小实践

## 总结先行

商业化成功案例：AI剪辑

https://clip.opus.pro/dashboard

看官方案例很丝滑

1. 直播切片剪辑 （直播采访切片）
2. Gaming 切片（篮球赛、游戏比赛等竞技比赛）
3. 综艺切片（speed、Mr beast）

【总】：1. 影评解说
2. 商业广告切片
3. 知识付费切片
4. 生活娱乐切片




## 具体实践

今天尝试用 react （用 lovable 前端 ai 快速生成） + fastapi （用 gemini 生成）

打通最小视频剪辑 mvp （存储到本地，不涉及数据库，gcs）

## nvm
The only requirement is having Node.js & npm installed - install with nvm
1. Windows 可以用 git bash 用类 unix 命令下载依赖
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash
```

2. Git bash 虽然方便，但是不能直接在windows 环境中用
-  1. Git bash 中 which node 找到下载地址
-  2. 环境变量 path 中添加 /path/to/your/node/bin
-  3. Cmd 打开用 node -v / npm  -v 验证

3. Node.js、 npm、 nvm 三者的关系

node.js Javascript 运行环境

npm 包管理器（Node Package Manager）

nvm 版本管理工具（Node Version Manager）

4.  npm i  （即 npm install）

等同于读取 package.json，安装所有 dependencies 和 devDependencies

```
added 396 packages, and audited 397 packages in 1m

73 packages are looking for funding
  run `npm fund` for details

4 moderate severity vulnerabilities

To address all issues, run:
  npm audit fix

Run `npm audit` for details.
```

## 前后端对接文档

### 1. 项目背景：
我们开发一个视频分析应用，用户可以上传视频文件或输入视频 URL，选择模型和 temperature，点击“分析视频”后，后端会分析视频并返回：

- Highlights 文本（JSON 格式）。
- 剪辑好的视频（一个可访问的 URL）。

前端展示 Highlights 和视频，并支持下载。  
前端：React + Vite + TypeScript + shadcn-ui + Tailwind CSS，运行在 http://localhost:8080。  
后端：FastAPI，运行在 http://localhost:8000。

### 2. 后端需要提供的接口

#### 接口 ：/analyze
- 作用：接收视频（文件或 URL）、模型参数，分析视频，返回 Highlights 和剪辑好的视频 URL。
- 请求方法：POST
- 请求参数（FormData 格式）：
  - videoFile（可选，文件）：用户上传的视频文件。
  - videoUrl（可选，字符串）：视频的 URL（例如 YouTube 链接）。
    - 注意：video_file 和 video_url 二选一，不能同时为空或同时提供。
  - model（字符串，默认值 "gemini-1.5-pro-002"）：选择的模型。
  - temperature（浮点数，默认值 0.7）：Temperature 参数。
- 返回值（JSON 格式）：
  - highlights（对象）：分析生成的 Highlights 文本。
{
  "title": "Epic Moments",
  "description": "Check out these highlights!",
  "clips": [
    {"start_time": "00:10", "end_time": "00:20", "description": "First highlight"},
    {"start_time": "00:30", "end_time": "00:40", "description": "Second highlight"}
  ]
}
  - video_url（字符串）：剪辑好的视频 URL。
- 示例请求（使用 curl 测试）：

curl -X POST http://localhost:8000/analyze \
  -F "video_url=https://www.youtube.com/watch?v=example" \
  -F "model_id=gemini-1.5-pro-002" \
  -F "temperature=0.7"
- 示例响应：

```
{
  "highlights": {
    "title": "Epic Moments",
    "description": "Check out these highlights!",
    "clips": [
      {"start_time": "00:10", "end_time": "00:20", "description": "First highlight"},
      {"start_time": "00:30", "end_time": "00:40", "description": "Second highlight"}
    ]
  },
  "video_url": "https://storage.googleapis.com/bucket/clipped_video.mp4"
}
```

#### 注意事项

- CORS 配置：确保后端允许前端请求，设置 allow_origins=["http://localhost:8080"]。
- 视频存储：剪辑好的视频需要上传到云存储（例如 Google Cloud Storage），并返回公开可访问的 URL。
- 默认参数：从 app.core.constants 模块中读取默认值，例如 DEFAULT_MODEL_ID、DEFAULT_TEMPERATURE。
- 二选一验证：确保 video_file 和 video_url 二选一。

### 前端实现

#### 页面功能

```
1. 输入部分：
  - 提供两个选项：上传视频文件（ <input type="file">）或输入视频 URL（<input type="text">）。
  - 提供模型选择（<input> 或 <select>）和 temperature 输入（<input type="number">）。
  - 一个“分析视频”按钮，点击后发送请求到后端。
2. 结果展示：
  - 展示 Highlights 文本（JSON 格式，使用 <pre> 标签）。
  - 展示剪辑好的视频（使用 <video> 标签）。
  - 提供两个下载按钮：
    - 下载 Highlights（JSON 文件）。
    - 下载视频（直接下载视频文件）。
```

#### 文件修改
- 新建组件：src/components/VideoAnalyzer.tsx
  - 包含表单（上传文件/输入 URL、模型选择、temperature 输入）。
  - 向后端发送请求（使用 axios）
  - 展示结果（Highlights 和视频）。
  - 实现下载功能。
- 主页面：src/pages/App.tsx
  - 引入 VideoAnalyzer 组件并渲染。
- 环境变量：.env
  - 添加 VITE_API_URL=http://localhost:8000。

#### 注意事项
- 请求格式：使用 FormData 发送请求，确保文件上传和参数正确传递。
- 下载功能：
  - Highlights 下载：将 JSON 转为 Blob，触发下载。
  - 视频下载：使用标签的 download 属性。
- 样式：使用 shadcn-ui 和 Tailwind CSS 美化页面。

### 前后端对接

1. 后端启动：
  - 运行 uvicorn main:app --reload。
  - 确保 http://localhost:8000/analyze 可访问（可以用 Postman 测试）。
2. 前端启动：
  - 运行 npm run dev。
  - 访问 http://localhost:8080。
3. 测试对接：
  - 在前端页面上传视频或输入 URL，选择模型和 temperature，点击“分析视频”。
  - 确认后端收到请求并返回结果。
  - 确认前端正确展示 Highlights 和视频，并支持下载。


## React 进阶实践

最重要的前后端对接的四个文件

### 1. App.tsx

- <QueryClientProvider client={queryClient}>: 将上面创建的 queryClient 实例提供给应用中的所有子组件。这样，任何子组件都可以使用 React Query 的 hooks (如 useQuery, useMutation) 来与 API 交互。
- <Route path="/" element={<Index />} />: 定义根路径 (/)。当用户访问应用的根 URL 时，渲染 Index 组件。


### 2. src/pages/index.tsx

好的，直接看重点。在这个 react-frontend/src/pages/Index.tsx 文件中，对接后端 API 主要修改和关注以下地方：


1. 导入 API 函数:
```
// ...
// 确保从你的 API 文件 (如 @/lib/api) 导入了需要调用的函数
import { getHelloMessage, postSplitUrl, analyzeVideo, AnalyzePayload } from '@/lib/api';
// 可能还需要导入相关的 TypeScript 类型
import type { ParsedUrlData, AnalysisResult } from '@/lib/api'; // 或其他地方定义的类型
// ...
```

2. useQuery (获取数据):

```
// ...
  const { data: helloData, isLoading: helloLoading, ... } = useQuery({
    queryKey: ['helloMessage'], // 1. 唯一标识符 (用于缓存)
    queryFn: getHelloMessage     // 2. 调用哪个 API 函数来获取数据
  });
// ...
// 3. 在 JSX 中使用 data (helloData), isLoading (helloLoading), isError 等状态
```

3. useMutation (发送/修改数据):
```
// ...
  const {
    mutate: analyzeVideoMutate, // 1. 调用这个函数来触发 API 请求
    data: analysisResults,      // 4. 成功后，数据在这里
    isPending: isAnalyzing,     // 5. 加载状态
    isError: analysisIsError,   // 6. 错误状态
    error: analysisError        // 7. 错误详情
  } = useMutation<AnalysisResult, Error, AnalyzePayload>({ // 类型参数
    mutationFn: analyzeVideo, // 2. 调用哪个 API 函数来发送数据
    // 3. 可选：在这里处理成功 (onSuccess) 或失败 (onError) 的回调 (例如显示提示)
    onSuccess: (data) => { /* ... */ },
    onError: (error) => { /* ... */ },
  });
// ...
// 8. 在 JSX 中使用 isPending (isAnalyzing), data (analysisResults), isError 等状态
```

4. 事件处理函数 (触发 Mutation):
```
// ...
  // 例如：表单提交时
  const handleAnalysisSubmit = async (data: AnalyzePayload) => {
    // 调用 useMutation 返回的 mutate 函数，并传入需要发送的数据
    analyzeVideoMutate(data);
  };
// ...
// 在 JSX 中将此函数绑定到表单的 onSubmit 或按钮的 onClick
<AnalysisForm onSubmit={handleAnalysisSubmit} isLoading={isAnalyzing} />
```

总结: 对接后端主要就是 **导入 API 函数**，然后用 useQuery 获取数据，用 useMutation 发送数据，并在**事件处理函数**中调用 mutate 来触发 useMutation。最后，利用这些 hooks 返回的**数据**和**状态**来更新你的界面 (JSX)。

### 3. src/lib/api.ts


1. **配置 `baseURL`**:
   ```typescript
   const apiClient = axios.create({
     baseURL: 'http://localhost:8000/api', // 确认这是你后端 API 的正确基础地址
     // ...
   });
   ```
   确保 `baseURL` 指向你的 FastAPI 或其他后端服务的根 API 路径。

2. **为每个 API 端点创建导出函数**:
   ```typescript
   // 为你要调用的每个后端路由创建一个 async 函数
   export const getHelloMessage = async (...) => { ... };
   export const postSplitUrl = async (...) => { ... };
   export const analyzeVideo = async (...) => { ... };
   ```
   每个函数对应一个后端操作。

3. **使用正确的 HTTP 方法和路径**:
   ```typescript
   // 在函数内部，使用 apiClient 调用相应的方法 (get, post, put, delete 等)
   // 并提供相对于 baseURL 的路径
   const response = await apiClient.get('/hello'); // GET 请求到 /api/hello
   const response = await apiClient.post('/analyze', payload); // POST 请求到 /api/analyze
   ```

4. **传递请求数据 (Payload)**:
   ```typescript
   // 对于 POST, PUT 等需要发送数据的请求，将数据作为第二个参数传递
   const response = await apiClient.post('/analyze', payload); // payload 是要发送的数据对象
   ```
   确保发送的数据结构与后端期望的一致。

5. **定义 TypeScript 类型 (推荐)**:
   ```typescript
   // 为请求数据 (Payload) 和响应数据 (Promise 返回值) 定义接口或类型
   interface AnalyzePayload { /* ... */ }
   export const analyzeVideo = async (payload: AnalyzePayload): Promise<AnalysisResult> => { ... };
   ```
   这能提供类型安全和更好的代码提示。

6. **返回 `response.data`**:
   ```typescript
   // 通常直接返回 response.data，Axios 会自动处理 JSON 解析
   return response.data;
   ```

### 总结
核心就是配置好 `baseURL`，然后为每个后端接口创建一个函数，在函数里用 `apiClient` 调用正确的**方法**和**路径**，传递必要的**数据**，并定义好**类型**。


### 4. src/app.tsx

1. **`<QueryClientProvider>`**:
   ```typescript
   import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
   // ...
   const queryClient = new QueryClient();

   const App = () => (
     <QueryClientProvider client={queryClient}> {/* 这个 Provider 必须包裹整个应用 */}
       {/* ... 其他 Providers 和路由 ... */}
     </QueryClientProvider>
   );
   ```
   这个 Provider 是 React Query 的核心，**必须**存在并且包裹住所有需要调用 API 的组件（通常是整个应用）。它使得子组件可以使用 `useQuery` 和 `useMutation`。

2. **路由 (`<Routes>`, `<Route>`)**:
   ```typescript
   import { BrowserRouter, Routes, Route } from "react-router-dom";
   import Index from "./pages/Index"; // 路由指向的页面组件 (如 Index)
   // ...
       <Routes>
         <Route path="/" element={<Index />} /> {/* 实际的 API 调用发生在 Index 组件内部 */}
         {/* ... */}
       </Routes>
   ```
   路由本身不直接对接后端，但它决定了渲染哪个页面组件（例如 Index）。**实际的 API 调用逻辑（使用 `useQuery`/`useMutation`）应该写在这些页面组件内部**，而不是写在 `App.tsx` 里。

### 总结
`App.tsx` 主要负责**提供 React Query 的基础环境 (`QueryClientProvider`)** 并设置路由。它不包含具体的 API 调用代码，而是**为子组件（页面）进行 API 调用做好准备**。