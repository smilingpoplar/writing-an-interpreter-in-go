《用 Go 语言自制解释器》

跟着敲一遍

---

text =lexer=> token =parser=> AST =evaluator=> 内部 object

## lexer

invariant：当前位置`position`在解析前指向 token 的第一个字节，在解析后指向 token 的最后一个字节

## parser

自顶向下 pratt 解析法

`parseExpression(precedence int)`在解析中缀时：

```go
// 对当前token来说，`precedence`是左边token算符的优先级，`p.peekPrecedence()`是右边token算符的优先级。
// 哪边的优先级更高，当前token就被拉到哪边AST子树使用。比如右边token的优先级更高，当前token将作为右边子树的左节点。
for !p.peekTokenIs(token.SEMICOLON) && precedence < p.peekPrecedence() { // 若右边token的优先级更高，就不断继续
```

## 宏扩展

在 parser 和 evaluator 间修改 AST

宏调用和函数的区别在于调用时不求值参数
