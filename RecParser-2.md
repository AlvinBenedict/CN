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
