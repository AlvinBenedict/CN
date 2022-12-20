%{
#include<stdio.h>
#include<string.h>
%}
%%
abc {strcpy(yytext,"ABC");ECHO;}
%%
int yywrap()
{
return 1;
}
int main()
{
printf("Enter the string:");
yylex();
}
