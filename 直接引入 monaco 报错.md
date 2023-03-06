- 直接引入 monaco 报错
  `import * as monaco from "monaco-editor";` 

  ```js
  editorSimpleWorker.js:469 Uncaught (in promise) Error: Unexpected usage
      at EditorSimpleWorker.loadForeignModule (editorSimpleWorker.js:469:31)
      at webWorker.js:38:30
      at async tsMode.js:88:16
      at async WorkerManager.getLanguageServiceWorker (tsMode.js:94:20)
      at async DiagnosticsAdapter._doValidate (tsMode.js:353:20)
  ```

  改为`import * as monaco from "monaco-editor/esm/vs/editor/editor.api";`

- monaco-languageclient

  X [ERROR] Could not resolve "vscode"

      node_modules/.pnpm/vscode-base-languageclient@4.4.0/node_modules/vscode-base-languageclient/lib/protocolDocumentLink.js:17:19:
        17 │ var code = require("vscode");
           ╵                    ~~~~~~~~

    You can mark the path "vscode" as external to exclude it from the bundle,   
    which will remove this error. You can also surround this "require" call     
    with a try/catch block to handle this failure at run-time instead of        
    bundle-time.

  解决：
  
  - https://stackoverflow.com/questions/56589711/strange-unresolved-dependencies-to-vscode-when-wrapping-monaco-editor-and-mona
  - https://github.com/TypeFox/monaco-languageclient/issues/242
  
- monaco-languageClient 不支持高版本 monaco-editor 

  ```js
  monaco-languageclient/lib/vscode-compatibility is nowhere to be found.
  
  {
  	detail: undefined,
  	id: "",
  	location: {
  		column: 23,
  		file: "node_modules/vscode-languageclient/lib/common/semanticTokens.js",
  		length: 8,
  		line: 8,
  		lineText: 'const vscode = require("vscode");',
  		namespace: "",
  		suggestion: "",
  	},
  	notes: [
  		{
  			location: null,
  			text: 'You can mark the path "vscode" as external to exclude it from the bundle, which will remove this error. You can also surround this "require" call with a try/catch block to handle this failure at run-time instead of bundle-time.',
  		},
  	],
  	pluginName: "",
  	text: 'Could not resolve "vscode"',
  };
  ```

  解决：将 monaco-editor 降级为 0.34.0

  https://github.com/TypeFox/monaco-languageclient/issues/460
  
- ```js
  Uncaught TypeError: antlr4__WEBPACK_IMPORTED_MODULE_0__.default.PredictionContextCache is not a constructor
      at ./src/ANTLR/SQLiteParser.ts (SQLiteParser.ts:395:1)
      at __webpack_require__ (bootstrap:24:1)
      at fn (hot module replacement:62:1)
      at ./src/ANTLR/index.ts (SQLiteParserListener.ts:451:1)
      at __webpack_require__ (bootstrap:24:1)
      at fn (hot module replacement:62:1)
      at ./src/example/sqlite.js (Visitor.js:42:1)
      at __webpack_require__ (bootstrap:24:1)
      at fn (hot module replacement:62:1)
      at ./src/index.js (sqlite.js:15:1)
  ```

  解决：`const sharedContextCache = new antlr4.PredictionContextCache();`更改为 `const sharedContextCache = new antlr4.atn.PredictionContextCache();`

- ```js
  SQLiteParser.js:299 Uncaught TypeError: antlr4__WEBPACK_IMPORTED_MODULE_0__.default.PredictionContextCache is not a constructor
  ```

  解决：将 antlr4 降级，从4.12.0降级为4.10.1