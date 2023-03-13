# Monaco 杂记

## 自定义语言高亮主题

```javascript
const monaco = require("monaco-editor");
monaco.languages.register({ id: "properties", aliases: ["properties"] });
monaco.languages.register({ id: "yaml1", aliases: ["YAML", "yaml", "YML", "yml", "yaml1"], mimetypes: ["application/x-yaml", "text/x-yaml"] });
monaco.languages.setMonarchTokensProvider("properties", {
	ignoreCase: true,
	tokenizer: {
		root: [
			[/^\s*(\#).*/, "notes"],
			//每对小括号代表一个分词
			[/(.*\s*)(\s*=\s*)(\w*[^\s]*)(\s*[\#]*\w*)/, ["key", "sym", "value", "comment"]],
		],
	},
});
monaco.languages.setMonarchTokensProvider("yaml1", {
	ignoreCase: true,
	tokenizer: {
		root: [
			[/^\s*(\#).*/, "comment"],
			[/(\w*)(\s*:\s*)(\w*[^\s]*)(\s*[\#]*\w*)/, ["key", "sym", "value", "comment"]],
		],
	},
});

monaco.editor.defineTheme("newTheme", {
	base: "vs",
	inherit: false,
	rules: [
		{ token: "key", foreground: "#0000FF", fontStyle: "bold" },
		{ token: "sym", foreground: "#f5a623" },
		{ token: "value", foreground: "#4EC9B0" },
		{ token: "notes", foreground: "#6A9955" },
		{ token: "comment", foreground: "#6A9955" },
	],
});
```

以上代码适用于注册新的语言以及增加已有语言的语法规则的高亮主题的方法。
首先需要通过`monaco.languages.register`注册新的语言，其主要参数为：

- `id`：新增的语言的key
- `aliases`：别名，注册的新语言的别名
- `extensions`：扩展，其他语言的id可以写在这里，会被识别为新的语言，可以用来覆盖原有语言的分词方式

然后通过`monaco.languages.setMonarchTokensProvider`设置Monarch分词器，其主要参数为：

- `ignoreCase`：忽略分词器的大小写

- `tokenizer`：分词器，通过正则表达式可以划分`monaco-editor`每行的词条。其中属性`root`可以上面的代码方式进行分词。

  > 我们在 tokenizer 中定义了一个 **root** 属性，**root** 是 **tokenizer** 中的一个 **state** , 这就是我们用来编写解析规则（**rule**）的地方，在 **rule** 中，我们可以编写匹配文本的**正则表达式**，然后再给匹配到的文本设置一个执行动作的 **action** ，在 **action** 中，我们可以给匹配到的文本设置 **token class** 。

最后通过`monaco.editor.defineTheme`设置语言的主题，相当于通过规则设置已分词汇的背景色，字体颜色等。其主要参数为：

- `base`：要继承的基础主题，即内置的三个：vs、vs-dark、hc-black
- `inherit`：是否继承
- `rules`：高亮规则，即给代码里不同token类型的代码设置不同的显示样式，常见的token有`string`（字符串）、`comment`（注释）、`keyword`（关键词）等等。查找样式可以通过按F1或鼠标右键点击`Command Palette`，然后再找到并点击`Developer: Inspect Tokens`，接下来鼠标点哪一块代码，就会显示对应的信息，包括token类型，当前应用的颜色等。
- `colors`：非代码部分的其他部分的颜色，比如背景、滚动条等

高阶些的自定义主题用法可以查看该链接文章：[闲谈Monaco Editor-自定义语言之Monarch](https://link.segmentfault.com/?enc=WoObriSG1TiUu2SRTjA96A%3D%3D.ww9hqlxa5xxyoBTBd6kUDiej64tvNIDAivWvWV9LidqmIClP0GIVE08uVo8zUDhb)