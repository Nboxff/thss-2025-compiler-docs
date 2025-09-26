# PA2: 手写词法分析器

## Soft Deadline

**2025 年 10 月 20 日 23：59**

## 实验描述

在PA1中，我们已经学习了ANTLR4 g4文件的写法，并利用ANTLR4自动化生成了词法分析器。而在本次PA中，我们希望同学们用 C++ 语言，自行实现一个简单的词法分析器。

!!! tip "为什么要手写词法分析器？"

	同学们可能有疑问，明明在PA1中我们已经能自动化生成词法分析器了，为什么还要手写一个呢？事实上，手写一个词法分析器并不复杂，在gcc等编译器中，开发人员仍会选择自己手写词法分析器。

在开始本次实验之前，请确保你已经准备好 C++17 的开发环境，相信对于大三的同学来说这并不复杂。同时，你需要安装Python3，方便你将代码打包成ZIP压缩包上传到GradeScope。

### 词法规则描述

为了降低大家的任务量，我们选择了改编自龙书的一段词法规则**DragonLexerGramar**，我们用g4文件的形式给出DragonLexer的词法。

```g4 title="DragonLexerGrammar.g4"
lexer grammar DragonLexerGrammar;

WS : [ \t\r\n]+ -> skip;

IF : 'if' ;
ELSE : 'else' ;
ID : LETTER (LETTER | DIGIT)* ;

INT : DIGITS ;

EQ : '=' ;
NE : '<>' ;
LT : '<' ;
LE : '<=' ;
GT : '>' ;
GE : '>=' ;

DOT : '.' ;
POS : '+' ;
NEG : '-' ;

REAL : DIGITS ('.' DIGITS)? ;
SCI : DIGITS ('.' DIGITS)? ([eE] [+-]? DIGITS)? ;

fragment DIGIT :  [0-9] ;
fragment DIGITS : [0-9]+ ;
fragment LETTER : [a-zA-Z] ;
```

### 输入输出要求

输入是一段符合DragonLexerGramar词法的一段程序，你需要输出词法分析的结果，每个词法单元(token)占一行，格式为`TOKEN类型 TOKEN内容 at Line 行数.`。这一部分的内容已经在框架代码中为你写好，你无需改动。

#### 输入样例

```
123
0543
123+xyz
3<=4
3abc3
123.45E-67
123.45-67
123.45E-
```

#### 输出样例

```
INT 123 at Line 1.
INT 0543 at Line 2.
INT 123 at Line 3.
PLUS + at Line 3.
IDENT xyz at Line 3.
INT 3 at Line 4.
LE <= at Line 4.
INT 4 at Line 4.
INT 3 at Line 5.
IDENT abc3 at Line 5.
SCI 123.45E-67 at Line 6.
REAL 123.45 at Line 7.
MINUS - at Line 7.
INT 67 at Line 7.
REAL 123.45 at Line 8.
IDENT E at Line 8.
MINUS - at Line 8.
```

### Part1: 框架代码导读

你需要使用`git`下载实验的框架代码：

```shell
git clone [Todo]
```

### Part2: 完善词法分析器

在本任务中，你需要补全`Lexer.cpp`和`DragonLexer.cpp`两个文件。你可以修改框架代码中的其他文件，但请保证不要改变输入输出的格式。

我们在你需要完成代码编写的地方增加了`TODO`注释，如果你使用 VSCode 进行编程，你可以安装 **TODO Highlight** 这个插件。

### 运行与测试

PA2采用CMake构建项目，你也可以自行编译所有的.cpp文件。下面以CMake构建为例，在项目根目录中执行下面的命令：

```shell
mkdir build
cd build
cmake ..
make
```

此时，可以看到`build/`目录下生成了可执行文件`dragon_lexer`或`dragon_lexer.exe`(Windows)，执行`./dragon_lexer`运行词法分析器。

当然，你可以配置你的Clion/VSCode等现代编程工具，利用GUI图形界面完成构建与运行。

对于PA2，我们只提供上面的样例，不提供具体的测试数据，你需要自行编写测试用例进行测试。

### 提交与评分

在项目根目录运行`python package.py`生成ZIP压缩包，将压缩包上传到GradeScope上。

压缩包预期的结构如下：

```
.
├── CMakeLists.txt
└── src
    ├── DragonLexer.cpp
    ├── DragonLexer.hpp
    ├── KeywordTable.cpp
    ├── KeywordTable.hpp
    ├── Lexer.cpp
    ├── Lexer.hpp
    ├── Token.cpp
    ├── Token.hpp
    └── main.cpp
```
GradeScope上共有 10 个测试用例，每个10分。