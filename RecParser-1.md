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



							
