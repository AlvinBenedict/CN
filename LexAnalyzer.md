#include<stdio.h>
#include<string.h>
#include<ctype.h>
void main(){
    char token[50],c;
    int i=0;
    FILE *fp=fopen("input.txt","r");
    c=fgetc(fp);
    while(c!=EOF){
        if(c=='/'){
            c=getc(fp);
            if(c=='/'){
                do{
                    c=getc(fp);
                  }while(c!='\n' && c!=EOF);
            }
            else{
                printf("\n/\tOPERATOR");
            }
        }
        else if(isalpha(c) && c!=' ' && c!='\n'){
            i=0;
            while(isalpha(c) && c!=' ' && c!='\n'){
                token[i++]=c;
                c=fgetc(fp);
            }
            token[i]='\0';
            if(strcmp(token,"void")==0 || strcmp(token,"int")==0 || 
            	   strcmp(token,"char")==0 || strcmp(token,"float")==0)
                printf("\n%s\tKEYWORD",token);
            else
                printf("\n%s\tIDENTIFIER",token);
        }
        else if(c=='=' || c=='+' || c=='-' || c=='*'){
            printf("\n%c\tOPERATOR",c);
            c=fgetc(fp);
        }
        else if(c=='(' || c==')' || c=='{' || c=='}' || c==';'){
            printf("\n%c\tSPECIAL CHARACTER",c);
            c=fgetc(fp);
        }
        else if(isdigit(c)&& c!=' ' && c!='\n'){
            i=0;
            while(isdigit(c) && c!=' ' && c!='\n')
            {
                token[i++]=c;
                c=fgetc(fp);
            }
            token[i]='\0';
            printf("\n%s\tNUMBER",token);
        }
        else
        {
            c=fgetc(fp);
        }
    }
    fclose(fp);
}

*************************************************************************







INPUT FILE(input.txt)
---------------------
void main()
{
int x=95; //Comment
print(x);
}




OUTPUT
------
void	KEYWORD
main	IDENTIFIER
(	SPECIAL CHARACTER
)	SPECIAL CHARACTER
{	SPECIAL CHARACTER
int	KEYWORD
x	IDENTIFIER
=	OPERATOR
95	NUMBER
;	SPECIAL CHARACTER
print	IDENTIFIER
(	SPECIAL CHARACTER
x	IDENTIFIER
)	SPECIAL CHARACTER
;	SPECIAL CHARACTER
}	SPECIAL CHARACTER
