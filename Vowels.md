%{
#include<stdio.h>
int v=0;
int c=0;
%}

%%

[\t \n] ;
[aeiouAEIOU] {v++;}
[^aeiouAEIOU] {c++;}

%%

int yywrap()
{
return 1;
}
int main()
{
printf("Enter the string:");
yylex();
printf("\nNo of vowels=%d\nNo of consonants=%d",v,c);
}
