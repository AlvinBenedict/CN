                                                            LEXICAL ANALYSER USING LEX TOOL
                                                            
%{
int COMMENT=0;
%}
identifier [a-zA-Z][a-zA-Z0-9]*
%%
#.* {printf("\n%s is a preprocessor directive",yytext);}
int |
float |
char |
double |
while |
for |
struct |
typedef |
do |
if |
break |
continue |
void |
switch |
return |
else |
goto {printf(" keyword");}
"/*" {COMMENT=1;}{printf("comment");}
\+ {if(!COMMENT)printf(" op-plus");}
\- {if(!COMMENT)printf(" op-sub");}
\* {if(!COMMENT)printf(" op-mul");}
\/ {if(!COMMENT)printf(" op-div");}
{identifier}\( {if(!COMMENT)printf("fun");}
\{ {if(!COMMENT)printf("block begins");}
\} {if(!COMMENT)printf("block ends");}
{identifier}(\[[0-9]*\])? {if(!COMMENT) printf(" identifier");}
\".*\" {if(!COMMENT)printf("str");}
[0-9]+ {if(!COMMENT) printf("num");}
\)(\:)? {if(!COMMENT)printf("\n\t");ECHO;printf("\n");}
\( ECHO;
= {if(!COMMENT)printf(" op-equals");}
\<= |
\>= |
\< |
== |
\> {if(!COMMENT) printf("rel-op");}
%%
int main(int argc, char **argv)
{
FILE *file;
file=fopen("input.c","r");
if(!file)
{
printf("could not open the file");
exit(0);
}
yyin=file;
yylex();
printf("\n");
return(0);
}
int yywrap()
{
return(1);

}



                                                        VOWELS
                                                        
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



                                                              abc to ABC
                                                              
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




                                                                NO. OF LINES,WORDS,CHARACTERS
                                                                
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



                                                                
