1. 初始化参数

   ```js
   const LANGUAGE_ID = 'json';
   const MODEL_URI = 'inmemory://model.json';
   // monaco 解析后的 uri
   const MONACO_URI = monaco.Uri.parse(MODEL_URI);
   // code
   const value = "let a = 'welcome';\nconsole.log(a);\nfunction a() {\n    debugger\n    console.log('a')\n    debugger\n}\ndebugger;\nconsole.log(b);";
   ```

   

2. 注册 monaco 语言服务

   ```js
   monaco.languages.register({
       id: LANGUAGE_ID,
       extensions: ['.json', '.jsonc'],
       aliases: ['JSON', 'json'],
       mimetypes: ['application/json']
   });
   ```

   

3. 创建 model

   ```javascript
   monaco.editor.createModel(value, LANGUAGE_ID, MONACO_URI)
   ```

   

4. 创建 monaco editor 实例

   ```js
   monaco.editor.create(domElement, option)
   
   // example
   monaco.editor.create(document.getElementById('container'), {
       model,
       glyphMargin: true,
       lightbulb: {
           enabled: true
       },
       automaticLayout: true
   });
   ```

   

5. 安装 monaco 语言客户端服务

   ```js
   import { MonacoServices } from 'monaco-languageclient';
   // install Monaco language client services
   MonacoServices.install();
   ```

6. 创建 web socket 连接

   ```js
   const url = createUrl('localhost', 3000, '/sampleServer');
   const webSocket = new WebSocket(url);
   
   function createUrl(hostname: string, port: number, path: string): string {
       const protocol = location.protocol === 'https:' ? 'wss' : 'ws';
       return normalizeUrl(`${protocol}://${hostname}:${port}${path}`);
   }
   ```

7. 监听 web socket 打开，创建 MonacoLanguageClient 实例并启动

   ```js
   import { MonacoLanguageClient } from "monaco-languageclient";
   import { toSocket, WebSocketMessageReader, WebSocketMessageWriter } from "vscode-ws-jsonrpc";
   
   webSocket.onopen = () => {
   	const socket = toSocket(webSocket);
   	const reader = new WebSocketMessageReader(socket);
   	const writer = new WebSocketMessageWriter(socket);
   	const languageClient = createLanguageClient({
   		reader,
   		writer,
   	});
   	languageClient.start();
   	reader.onClose(() => languageClient.stop());
   };
   
   function createLanguageClient(transports: MessageTransports): MonacoLanguageClient {
   	return new MonacoLanguageClient({
   		name: "Sample Language Client",
   		clientOptions: {
   			// use a language id as a document selector
   			documentSelector: ["json"],
   			// disable the default error handler
   			errorHandler: {
   				error: () => ({ action: ErrorAction.Continue }),
   				closed: () => ({ action: CloseAction.DoNotRestart }),
   			},
   		},
   		// create a language client connection from the JSON RPC connection on demand
   		connectionProvider: {
   			get: () => {
   				return Promise.resolve(transports);
   			},
   		},
   	});
   }
   
   ```

   
