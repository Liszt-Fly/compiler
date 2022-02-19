# Compiler Overview

编辑器分为 <u>前端</u>，<u>优化部分</u>，<u>后端</u>三个部分
![OhColY](https://mikes.oss-cn-beijing.aliyuncs.com/uPic/OhColY.png)
其中前端部分在完成后会生成一棵 **AST(Abstract Sematic Tree)语法树**

| Human Language | Programming Language |
| -------------- | -------------------- |
| word           | token                |
| sentence       | statement            |

## FrontEnd

### scanning 词法分析原理剖析

Eg: x12 = a3 + b45
分析的原理是编程语言的各种规则，identifier 的名称不能包含关键词`=`，所以说`x12`既可以被区分出来，`a3`，`b45`的分析原理相同
所以最后的 token 列表为

- x12
- =
- a3
- b45

### Parsing 原理分析

Parsing 简而言之就是 toekn->statement，对比人类语言就是词汇成句
Eg:x12=a+b\*/ 就不是一个句子
常见的语法:

- Expression=identifier operator identifier
- Identifier assignOperator Expression
  由于不存在`identifier operator operator `的这种情况，所以很容易分析这不是一个 statement

### Semantic analysis 语义分析

比如 a=b+c 经过了词法分析，句法分析部分都是正确的，接下来需要进行语义分析部分

这时候需要引入一个 symbol table 的概念，里面存放了各种各样的 declaration

```c
int a
float b
Student c
```

显然这样相加是没有意义的，在语义分析部分不会通过

## AST 语法树

在前端部分完成之后就会生成一颗 AST 语法树
![BOl8SR](https://mikes.oss-cn-beijing.aliyuncs.com/uPic/BOl8SR.png)

## Optimization

举一个例子：Common Subexpression Elimination

```js
x = a + b + c
y = d / (a + b)
```

CSE 技术将 a+b 存储为`t=a+b`,这样操作不一定能达到优化的目的，只能说可能会有优化效果，因为 t 不一定存放在 cache 中，存放在 memory 中也会降低速度，而且寄存器的数量是有限的，如果将 t 变量放在寄存器中，就有其他的变量无法放在寄存器中，这种优化方式一般在寄存器数量多的电脑有较大作用

## BackEnd

### Insturction Selection&Insturction Scheduling

举一个例子，给出如下 AST 语法树

![7awiZq](https://mikes.oss-cn-beijing.aliyuncs.com/uPic/7awiZq.png)
汇编语言过程

```x86asm
load a->r1
load b->r2
mult r1,r2->r3
load c->r4
load d->r5
mult r4,r5->r6
add r3,r6->r7
store x->r7
```

- 选择哪些指令的过程是 instruction Selection
- 而确定这些汇编语言指令的过程是 instruction Scheduling

### Register Allocation

上述写的代码都是 virtual register 对虚拟寄存器的操作，而 Register Allocation 指的是 Virtual register into Real register
将虚拟寄存器的操作映射到真实寄存器上，最后形成机器代码

## 总结

总的来说，编译过程是从 abstract(virtual)的过程到最后 real 的过程(高级语言->中间语言->汇编语言->机器语言)
