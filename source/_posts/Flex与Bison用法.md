---
title: Flex与Bison用法
date: 2025-05-08
tags: 编译原理
#categories: 编译原理
---

### flex

1. 编写a.lex文件

```l
%{
int wordCount=0;
int numcount=0;
%}
chars [A-Za-z\_\'\.\"]
numbers ([0-9])+
delim [" "\n\t]
whitespace {delim}+ 
words {chars}+ 
%% 
while  {printf("%s\n",yytext);}
{words} { wordCount++;  /* increase the word count by one*/ } 
{whitespace} { /* do nothing*/ } 
([0-9])+ { numcount++; /* one may want to add some processing           here*/ } 
%%  
void main() { 
printf("ok1\n");  
yylex(); /* start the  analysis*/  
printf("ok2\n"); 
printf("No of words: %d\n     number: %d\n", wordCount, numcount);  
return 0;  
}
int yywrap() 
{ 
return 1; 
}
```

2. 第一步命令  `flex a.lex`得到`lex.yy.c`

3. 第二步命令  `gcc -o a lex.yy.c`得到`a.exe` （**词法分析器**）
4. 待分析文件`b.c`

```c
asd asdf 23 q 
a1 
b2
!#@
while
```

5. 使用命令 `a.exe <b.c> a.txt`  ， 分析结果保存在`a.txt`

```txt
ok1
!#@while
ok2
No of words: 5
    number: 3
```

注意在`cmd`下运行，`linux`更优，不要在`Powershell`，会有`<`字符冲突

### bison

1. 首先使用flex工具输入构词规则序列 ， 文件`token.l`

```l
%{
#include "expr.tab.h"
%}

%%
"q"   return STOP;
"("   return LP;
")"   return RP;
"\+"  return PLUS;
"\-"  return MINUS; 
"\*"  return MUL; 
"\/"  return DIV;

[0-9]+ {yylval=atoi(yytext); return DIGIT;} 
%%
```

2. 使用命令 `flex token.l`，得到`lex.yy.c`
3. 输入上下文无关文法，文件`expr.y`

```y
%{
#include <stdio.h>
%}

%token DIGIT STOP LP RP PLUS MINUS MUL DIV

%%
start: expr STOP {printf("expr=%d\n", $1);  exit(1);}
;
expr: expr PLUS expr  {$$=$1+$3;}
    |expr MINUS expr  {$$=$1-$3;}
    |expr MUL expr {$$=$1*$3;}
    |expr DIV expr {$$=$1/$3;}
    |LP expr RP  {$$=$2;}
    |DIGIT {$$=$1;}
;
%%

main(){
    printf("Type something followed by Return. Type 'q' to end.\n");
    printf("\n");
    return(yyparse());      /*Start the parser*/
}

yyerror(s)
char *s; {
    printf("yacc error: %s\n",s);
}

yywrap(){
    return(0);
}
```

4. 第二个命令 `bison -d expr.y`, 得到 `expr.tab.c`及`expr.tab.h`

5.  `gcc lex.yy.c expr.tab.c -o expr` 得到**语法分析程序**`expr.exe`

6. 使用语法分析程序分析输入文件`b.c`   `expr <b.c> a.txt`， 得到`a.txt`

- 输入

  ```
  55+(3*20)q
  ```

- 输出

  ```
  Type something followed by Return. Type 'q' to end.
  expr=115
  ```