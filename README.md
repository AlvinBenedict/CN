                                                                RECURSIVE DESCENT PARSER-I

#include<stdio.h>
char str[20];
int i=0,x,y;
int A(){
    if(str[i]=='a'){
        ++i;
        if(str[i]=='b'){
            ++i;
            return 1;
        }
        return 1;
    }
    return 0;
}    
int S(){
    if(str[i]=='a'){
        ++i;
        y=A();
        if(y==1 && str[i]=='d')
            return 1;
        else
            return 0;
    }
    return 0;
}

int main(){
    printf("Enter String:");
    scanf("%s",str);
    x=S();
    if(x==1)
        printf("Accepted");
    else
        printf("Not accepted");
}


*************************************************************************
OUTPUT
------

Enter String:aabd
Accepted

Enter String:abd
Not accepted



							RECURSIVE DESCENT PARSER-II

#include<stdio.h>
#include<string.h>
#include<ctype.h>
char input[30];
int i=0,e=0;
void E();
void Edash();
void T();
void Tdash();
void F();
int main(){
	printf("Enter string:");
	scanf("%s",input);
	E();
	if(strlen(input)==i && e==0)
		printf("Accepted\n");
	else	printf("Not Accepted\n");
	return 0;
}
void E(){
	T();
	Edash();
}
void Edash(){
	if(input[i]=='+'){
		i=i+1;
		T();
		Edash();
	}
	else	return;
}
void T(){
	F();
	Tdash();
}
void Tdash(){
	if(input[i]=='*'){
		i++;
		F();
		Tdash();
	}
	else	return;
}

void F(){
	if(input[i]=='('){
		i++;
		E();
		if(input[i]==')')
			i++;
		else	e=1;
	}
	else if(isalnum(input[i]))
		i++;
	else	e=1;
}

*************************************************************************
OUTPUT
------
Enter string:(5*6)+(6+8)
Accepted

Enter string:5+(5-8)
Not Accepted



								LEXICAL ANALYZER

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



								FIRST & FOLLOW
								
#include<stdio.h>
#include<math.h>
#include<string.h>
#include<ctype.h>
#include<stdlib.h>
int n,m=0,i=0,j=0;
char a[10][10],f[10];
void follow(char c);
void first(char c);
void firstT(char c,int x,int y);
void firstF(char c,char z,int x,int y);
int main(){
	int i,z;
	char c,ch;
	printf("Enter the no of productions: ");
	scanf("%d",&n);
	printf("Enter the productions: ");
	for(i=0;i<n;i++)
	scanf("%s%c",a[i],&ch);
	do{
		m=0;
		printf("Enter elemants whose first and follow is to be found:");
		scanf("%c",&c);
		first(c);
		printf("First(%c)= ",c);
		for(i=0;i<m;i++)
			printf("%c ",f[i]);
		printf("\n");
		strcpy(f,"");
		m=0;
		follow(c);
		printf("Follow(%c)= ",c);
		for(i=0;i<m;i++)
			printf("%c ",f[i]);
		printf("\n\n");
		printf("Continue(0/1)?");
		scanf("%d%c",&z,&ch);
	}while(z==1);
	return 0;
}
void first(char c){
	int k;
	if(!isupper(c))
		f[m++]=c;
	for(k=0;k<n;k++){
		if(a[k][0]==c){
			if(a[k][2]=='#')
				f[m++]='#';
			else if(islower(a[k][2]))
				f[m++]=a[k][2];
			else
				firstT(a[k][2],k,3);
		}
	}
}
void firstT(char c,int x,int y){
	int k;
	if(!isupper(c))
		f[m++]=c;
	for(k=0;k<n;k++){
		if(a[k][0]==c){
			if(a[k][2]=='#'){
				if(a[x][y]!='\0')
					firstT(a[x][y],x,y+1);
				else
					f[m++]='#';
			}
			else if(islower(a[k][2]))
				f[m++]=a[k][2];
			else
				firstT(a[k][2],k,3);
		}
	}
	
}
void follow(char c){
	if(a[0][0]==c)
		f[m++]='$';
	for(i=0;i<n;i++) {
		for(j=2;j<strlen(a[i]);j++){
			if(a[i][j]==c){
				if(a[i][j+1]!='\0')
					firstF(a[i][j+1],a[i][0],i,j+2);
				if(a[i][j+1]=='\0' && c!=a[i][0])
					follow(a[i][0]);
			}
		}
	}
}
void firstF(char c,char z,int x,int y){
	int k;
	if(!isupper(c))
		f[m++]=c;
	for(k=0;k<n;k++){
		if(a[k][0]==c){
			if(a[k][2]=='#'){
				if(a[x][y]!='\0')
					firstF(a[x][y],z,x,y+1);
				else
					follow(a[x][0]);
			}
			else if(islower(a[k][2]))
				f[m++]=a[k][2];
			else
				firstF(a[k][2],a[k][0],k,3);
		}
	}
	
}

*************************************************************************

OUTPUT
------

Enter the no of productions: 8
Enter the productions: E=TA
A=+TA
A=#
T=FB
B=*FB
B=#
F=(E)
F=i
Enter elemants whose first and follow is to be found:E
First(E)= ( i 
Follow(E)= $ ) 

Continue(0/1)?1
Enter elemants whose first and follow is to be found:A
First(A)= + # 
Follow(A)= $ ) 

Continue(0/1)?1
Enter elemants whose first and follow is to be found:T
First(T)= ( i 
Follow(T)= + $ ) 

Continue(0/1)?1
Enter elemants whose first and follow is to be found:B
First(B)= * # 
Follow(B)= + $ ) 

Continue(0/1)?1
Enter elemants whose first and follow is to be found:F
First(F)= ( i 
Follow(F)= * + $ ) 

Continue(0/1)?0



								CONSTANT PROPOAGATION
								
#include<stdio.h>
#include<string.h>
#include<ctype.h>
void input();
void output();
void change(int p,char *res);
void constant();
struct expr{
	char op[2],op1[5],op2[5],res[5];
	int flag;
}arr[10];
int n;
void main(){
	input();
	constant();
	output();
}
void input(){
	int i;
	printf("\n\nEnter the maximum number of expressions : ");
	scanf("%d",&n);
	printf("\nEnter the input : \n");
	for(i=0;i<n;i++){
		scanf("%s",arr[i].op);
		scanf("%s",arr[i].op1);
		scanf("%s",arr[i].op2);
		scanf("%s",arr[i].res);
		arr[i].flag=0;
	}
}
void constant(){
	int i;
	int op1,op2,res;
	char op,res1[5];
	for(i=0;i<n;i++){
		if(strcmp(arr[i].op,"=")==0){
			op1=atoi(arr[i].op1);
			op2=atoi(arr[i].op2);
			op=arr[i].op[0];
			res=op1;
			sprintf(res1,"%d",res);
			arr[i].flag=1; 
			change(i,res1);
		}
	}
}
void output(){
	int i=0;
	printf("\nOptimized code is : ");
	for(i=0;i<n;i++){
		if(!arr[i].flag){
			printf("\n%s %s %s %s",arr[i].op,
				arr[i].op1,arr[i].op2,arr[i].res);
		}
	}
}
void change(int p,char *res){
	int i;
	for(i=p+1;i<n;i++){
		if(strcmp(arr[p].res,arr[i].op1)==0)
			strcpy(arr[i].op1,res);
		else if(strcmp(arr[p].res,arr[i].op2)==0)
			strcpy(arr[i].op2,res);
	}
}
*************************************************************************

OUTPUT
------
Enter the maximum number of expressions : 5

Enter the input : 
= 3 - a
= 4 - b
+ a b t1
+ a c t2
+ t1 t2 t3

Optimized code is : 
+ 3 4 t1
+ 3 c t2
+ t1 t2 t3



								SHIFT REDUCE PARSER
								
#include<stdio.h>
#include<string.h>
int k=0,z=0,i=0,j=0,c=0;
char a[16],ac[20],stk[15],act[10];
void check();
int main(){
      puts("GRAMMAR is E->E+E \n E->E*E \n E->(E) \n E->id");
      puts("enter input string ");
      scanf("%s",a);
      c=strlen(a);
      strcpy(act,"SHIFT->");
      puts("stack \t input \t action");
      for(k=0,i=0; j<c; k++,i++,j++){
         if(a[j]=='i' && a[j+1]=='d'){
              stk[i]=a[j];
              stk[i+1]=a[j+1];
              stk[i+2]='\0';
              a[j]=' ';
              a[j+1]=' ';
              printf("\n$%s\t%s$\t%sid",stk,a,act);
              check();
         }
         else{
              stk[i]=a[j];
              stk[i+1]='\0';
              a[j]=' ';
              printf("\n$%s\t%s$\t%ssymbols",stk,a,act);
              check();
         }
      }
}
void check(){
     strcpy(ac,"REDUCE TO E");
     for(z=0; z<c; z++)
       if(stk[z]=='i' && stk[z+1]=='d'){
           stk[z]='E';
           stk[z+1]='\0';
           printf("\n$%s\t%s$\t%s",stk,a,ac);
           j++;
       }
     for(z=0; z<c; z++)
       if(stk[z]=='E' && stk[z+1]=='+' && stk[z+2]=='E'){
           stk[z]='E';
           stk[z+1]='\0';
           stk[z+2]='\0';
           printf("\n$%s\t%s$\t%s",stk,a,ac);
           i=i-2;
       }
     for(z=0; z<c; z++)
       if(stk[z]=='E' && stk[z+1]=='*' && stk[z+2]=='E'){
           stk[z]='E';
           stk[z+1]='\0';
           stk[z+1]='\0';
           printf("\n$%s\t%s$\t%s",stk,a,ac);
           i=i-2;
       }
     for(z=0; z<c; z++)
       if(stk[z]=='(' && stk[z+1]=='E' && stk[z+2]==')'){
           stk[z]='E';
           stk[z+1]='\0';
           stk[z+1]='\0';
           printf("\n$%s\t%s$\t%s",stk,a,ac);
           i=i-2;
       }
}
*************************************************************************

OUTPUT
------
GRAMMAR is E->E+E 
 E->E*E 
 E->(E) 
 E->id
 
enter input string 
id+id*id

stack 	 input 	 action
$id	  +id*id$	SHIFT->id
$E	  +id*id$	REDUCE TO E
$E+	   id*id$	SHIFT->symbols
$E+id	     *id$	SHIFT->id
$E+E	     *id$	REDUCE TO E
$E	     *id$	REDUCE TO E
$E*	      id$	SHIFT->symbols
$E*id	        $	SHIFT->id
$E*E	        $	REDUCE TO E
$E	        $	REDUCE TO E
