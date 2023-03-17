# AST

- ## Lexical analysis aka Tokennization

  这个过程，你的代码会被转译为一系列标记的集合。这些标记不关心它们的组合关系，纯粹是代码的组成部分。你可以把它想象成一个不同类型标记的列表或数组



# 编译过程

1. 字符流
2. 经过词法分析器
3. 符号流
4. 经过语法分析
5. 语法树
6. 经过语义分析
7. 语法树
8. 经过中间代码生成器
9. 中间表示形式
10. 经过代码生成器
11. 目标机器语言
12. 经过及其相关代码优化器
13. 目标机器语言



# Antlr4

![compiler](/Users/latte/Documents/image/compiler.png)

查看图表我们发现，为了为我们的语言创建编译器，我们需要：

- 为该语言提出一个***\*上下文无关的语法\****
- 针对给定的源代码运行具有该语法的词法***\*分析器\****以生成一组***\*标记\****
- 将这些***\*标记\****输入***\*解析器\****以生成***\*抽象语法树 (AST)\****
- 让我们的***\*语义分析器\****检查 AST 以确保它在***\*语义上是有效的\****
- ***\*可选地通过优化器\****提供 AST以***\*裁剪掉不必要的代码\****
- 最后从该 AST 生成***\*可执行代码\****并将其作为输出返回



ANTLR 最让我着迷的是，因为它可以针对 JavaScript 运行时，我们可以在前端使用相同的语法来解析传入的源代码，将其转换为一组标记，在语法和语义上验证代码，并且将所有内容与 Microsoft 的[Monaco Editor](https://microsoft.github.io/monaco-editor/)等解决方案集成，以在我们的网页上创建**内置代码编辑器**，完全支持**语法突出显示**和我们自定义语言的**代码验证。**



## Visitor vs Listener

ANTLR提供了两种机制来访问生成的语法树：Listener 和 Visitor

使用Listener模式来访问语法树时，ANTLR内部的`ParserTreeWalker`在遍历语法树的节点过程中，在遇到不同的节点中，会调用提供的listener的不同方法；

而使用Visitor模式时，visitor需要自己来指定如果访问特定类型的节点

> Listener 是被动方式，Visitor 是主动方式

举例：

ANTLR生成的解析器源码中包含了默认的Visitor基类/接口`ExprVisitor.ts`，在使用过程中，只需要对感兴趣的节点实现visit方法即可，比如我们需要访问到`exprStat`节点，只需要实现如下接口：

```typescript
export interface ExprVisitor<Result> extends ParseTreeVisitor<Result> {
  ...

  /**
   * Visit a parse tree produced by `ExprParser.exprStat`.
   * @param ctx the parse tree
   * @return the visitor result
   */
  visitExprStat?: (ctx: ExprStatContext) => Result;
  
  ...
}
```





## explain of common antlr4 API

- **`antlr4.CharStream.fromString`**
- **`antlr4.InputStream`**
- **`antlr4.CommonTokenStream`**
- **`antlr4.tree.ParseTreeWalker.DEFAULT.walk`**

## 物料:

**antlr4  docs**: https://www.antlr.org/api/Java/org/antlr/v4/runtime/package-summary.html

**the antlr4 for JavaScript runtime**: https://github.com/antlr/antlr4/blob/master/doc/javascript-target.md