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
