# Webpack

webpack编译流程

1. 输入：从文件系统读入代码

2. 模块递归处理：调用 Loader 转译 Module 内容，并将结果转换为 AST，从中分析模块依赖关系

   然后进一步递归调用模块处理过程，直到所有依赖文件都处理完毕

3. 后处理：包括模块合并、注入运行时、产物优化等，最终输出 chunk 结合

4. 将 chunk 写到外部文件系统

# vite

1. 使用 esbuild 进行依赖预构建