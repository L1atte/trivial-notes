# Monaco-editor

- ## add some colors

- ## autoComplete

  ```javascript
  manaco.languages.resigsterCompletionItemProvider
  ```

- ## document

  ```javascript
  monaco.languages.registerHoverProvider("language", {
  	providerHover: monacoProviderFunction() => {}
  })
  ```




# Monarch

Monaco Editor的Monarch指的是一种语法高亮显示系统，用于将不同类型的代码区分开来。在Monaco Editor中，Monarch是一种使用正则表达式和语法规则定义的语言，它可以识别代码中的不同语法元素，并使用不同的颜色或样式进行高亮显示。

Monarch可以用于支持各种编程语言，包括JavaScript、TypeScript、C++、Java、Python等等。在Monaco Editor中，用户可以自定义Monarch规则，以支持自定义语言或特定的代码样式。这使得Monaco Editor成为了一款非常灵活和可定制的代码编辑器。