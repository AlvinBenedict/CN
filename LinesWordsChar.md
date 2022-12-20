%{
#include<stdio.h>
#include<string.h>
int l=0,c=0,w=0;
%}
%%
[\n] {l++;c++;}
[a-zA-z]+ {w++;c+=strlen(yytext);}
. {c++;}
%%
int yywrap()
{
return 1;
}
int main()
{
printf("Enter the string:");
yylex();
printf("\nLines=%d\nWords=%d\nCharacters=%d",l,w,c);
}

