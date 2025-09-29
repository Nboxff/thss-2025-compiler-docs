# PA3: 代码格式化（基于ANTLR4的语法分析）

## Soft Deadline

**2025 年 10 月 28 日 23：59**

## 前置要求

✅ 完成PA1: 基于ANTLR4的词法分析器

## 实验描述

代码格式化是指​​按照统一的风格规范对源代码进行重新排版的过程​​，目的是提高代码的可读性、一致性和可维护性，而不改变代码的实际功能和行为。现代的IDE均提供了代码格式化的功能，开源软件在合并PR前也往往要求待合并的代码已被格式化。

在PA1中，你已经使用ANTLR4构建了一个词法分析器，能够识别C语言代码的基本词法单元。在本实验中，你将基于ANTLR4的语法分析能力，构建一个代码格式化工具，能够对输入的无语法错误的C语言代码进行自动格式化，使其符合指定的代码风格规范。

### Part1: 编写SysYParser.g4文件

在进行代码格式化任务之前，你需要完成`SysYParser.g4`文件的编写，以完成对 SysY 语言的语法分析。

#### SysY语法规则介绍

在本次PA中，我们采用扩展的 Backus 范式（EBNF，Extended Backus-Naur Form），来表示SysY语言的语法规则，其中：

- 符号`[...]`表示方括号内包含的为可选项 
-  符号`{...}`表示花括号内包含的为可重复 0 次或多次的项 
-  终结符或者是由单引号括起的串，或者是 Ident、InstConst 这样的记号

```
CompUnit       ->     [CompUnit] (Decl | FuncDef)
Decl           ->     ConstDecl | VarDecl

TODO:


Exp            ->     AddExp
Cond           ->     LorExp
Lval           ->     Ident {'[' Exp ']'}
PrimaryExp     ->     '('Exp')'|LVal|Number
Number         ->     IntConst
UnaryExp       ->     PrimaryExp | Ident '('[FuncRParams]')' | UnaryOp UnaryExp
UnaryOp        ->     '+' | '−' | '!'
FuncRParams    ->     Exp { ',' Exp }
MulExp         ->     UnaryExp | MulExp ('*' | '/' | '%') UnaryExp
AddExp         ->     MulExp | AddExp ('+' | '−') MulExp
RelExp         ->     AddExp | RelExp ('<' | '>' | '<=' | '>=') AddExp
EqExp          ->     RelExp | EqExp ('==' | '!=') RelExp
LAndExp        ->     EqExp | LAndExp '&&' EqExp
LOrExp         ->     LAndExp | LOrExp '||' LAndExp
ConstExp       ->     AddExp 
```
### Part2: 代码格式化

在 Part 2 中，你需要完成代码格式化的任务，格式化要求如下：

1. 注释规则：
    - 你需要删除源代码中所有的注释信息和`EOF`标识符
2. 换行规则：
    - 每一条语句(decl/stmt)独占一行
    - 左花括号`{`采用 K&R 风格，即不换行，且`{`前有一个空格
    - 对于单独存在的代码块，左花括号独占一行，且前面没有额外的空格
    - 右花括号`}`另起一行
    - 除第一行外，每个函数前有一个空行
    - 每个声明语句独占一行，声明语句之间没有空行
    - 每条声明语句中的花括号都在同一行，即使它们存在嵌套关系（如`{{1, 2, 3}, {1, 2, 3}}`;）

3. 缩进规则：
    - 初始情况（最外层）不需要缩进
    - 每一级缩进使用2个空格
    - 语句块(block)内缩进一层，即每进入一个block，缩进两个空格
    - if、else、while等语句下无花括号包裹的语句，也要缩进一层
    - 形如`else if (cond) block`的情况，`else if`在同一行，且block缩进保持不变，block内缩进正常+1

4. 空格规则：
    - 下列关键字后面需要添加一个空格：
        ```c
        const, int, void, if, else, while, return
        ```
        注：`return`后面跟分号的时候不需要加空格。
    - 二元运算符的两侧均有一个空格，注意比较运算符、部分逻辑运算符也是二元运算符
    - 一元运算符的两侧不需要加空格
    - 函数定义部分，其返回值和函数名之间有一个空格
    - 每行的末尾不要加额外的空格
    - 逗号的后面有一个空格，即对于函数形参、函数实参、初始化列表等情况有多于一个项的时候，用于分隔它们的逗号后面有一个空格，逗号前面没有空格
    - 除缩进以外，两个词法单元之间不会出现连续的多个空格

#### 输入样例

```c
const int a=1; int b= 2;
int arr[10]={1,2,3};

void plus(int x, int y) 
{
return x + y;
}

void print() {}

int main() 
{
  int c;
  {
    cond=1;
    if (cond == 1) 
    {
        c=a+b;
    } 
    else if (cond==2) 
    {
        c=a-b;
    }
    else c=a*b;
  }
  print();   return 0;
}
```

#### 输出样例

```c
const int a = 1;
int b = 2;
int arr[10] = {1, 2, 3};

void plus(int x, int y) {
  return x + y;
}

void print() {
}

int main() {
  int c;
  {
    cond = 1;
    if (cond == 1) {
      c = a + b;
    } 
    else if (cond == 2) {
      c = a - b;
    }
    else
      c = a * b;
  }
  print();
  return 0;
}
```
