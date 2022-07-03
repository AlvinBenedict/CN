# CN
		     CLIENT SERVER COMMUNICATION - TCP

CLIENT

#include <sys/socket.h>
#include <netinet/in.h>
#include <stdio.h>
#include <string.h>
#include<stdlib.h>
int main()
{
char buf[100];
int k;
int sock_desc;
struct sockaddr_in client;
sock_desc=socket(AF_INET,SOCK_STREAM,0);
if(sock_desc==-1)
printf("error in socket creation");
client.sin_family=AF_INET;
client.sin_addr.s_addr=INADDR_ANY;
client.sin_port=3003;
k=connect(sock_desc,(struct sockaddr*)&client,sizeof(client));
if(k==-1)
printf("error in connecting to server");
printf("\nenter data to be send:");
fgets(buf,100,stdin);
k=send(sock_desc,buf,100,0);
printf("error in sending");
close(sock_desc);
return 0;
}


SERVER
#include<sys/socket.h>
#include <netinet/in.h>
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
int main()
{
char buf[100];
int k;
socklen_t len;
int sock_desc,temp_sock_desc;
struct sockaddr_in server,client;
sock_desc=socket(AF_INET,SOCK_STREAM,0);
if(sock_desc==-1)
printf("enter in socketcreation");
server.sin_family=AF_INET;
server.sin_addr.s_addr=INADDR_ANY;
server.sin_port=3003;
client.sin_family=AF_INET;
client.sin_addr.s_addr=INADDR_ANY;
client.sin_port=3003;
k=bind(sock_desc,(struct sockaddr*)&server,sizeof(server));
if(k==-1)
printf("error in binding");
k=listen(sock_desc,5);
if(k==-1)
printf("error in listening");
len=sizeof(client);
temp_sock_desc=accept(sock_desc,(struct sockaddr*)&client,&len);
if(temp_sock_desc==-1)
printf("error in temporary socket creation");
k=recv(temp_sock_desc,buf,100,0);
if(k==-1)
printf("error is receiving");
printf("message got from client : %s",buf);
close(temp_sock_desc);
return 0;
}

OUTPUT

CLIENT
student@student-OptiPlex-3020:~/Desktop/network$ ./cli
enter data to be send: hello
error in sendingstudent@student-OptiPlex-3020:~/Desktop/network$

SERVER
student@student-OptiPlex-3020:~/Desktop/network$ ./sev
message got from client: hello
student@student-OptiPlex-3020:~/Desktop/network$ ^C


                       CLIENT-SERVER COMMUNICATION -UDP
                                                                                          
CLIENT

#include<stdio.h>
#include<string.h>
#include<sys/socket.h>
#include<stdlib.h>
#include<netdb.h>
int main(int argc,char* argv[])
{
    struct sockaddr_in server,client;
    if(argc!=3)
    printf("Input format not correct");
    int sockfd=socket(AF_INET,SOCK_DGRAM,0);
    if(sockfd==-1)
    printf("Error in socket();");
    server.sin_family=AF_INET;
    server.sin_addr.s_addr=INADDR_ANY;
    server.sin_port=htons(atoi(argv[2]));
    char buffer[100];
    printf("Enter a message to be sent to server : ");
    fgets(buffer,100,stdin);
    if(sendto(sockfd,buffer,sizeof(buffer),0,(struct sockaddr*)&server,sizeof(server))<0)
    printf("Error in sendto");
    return 0;
}

********************************************************************************

SERVER


#include<stdio.h>
#include<string.h>
#include<sys/socket.h>
#include<stdlib.h>
#include<netdb.h>
int main(int argc,char* argv[])
{
    struct sockaddr_in server,client;
    if(argc!=2)
    printf("Input format not correct");
    int sockfd=socket(AF_INET,SOCK_DGRAM,0);
    if(sockfd==-1)
    printf("Error in socket();");
    server.sin_family=AF_INET;
    server.sin_addr.s_addr=INADDR_ANY;
    server.sin_port=htons(atoi(argv[1]));
    if(bind(sockfd,(struct sockaddr*)&server,sizeof(server))<0)
    printf("Error in bind()! \n");
    char buffer[100];
    socklen_t server_len=sizeof(server);
    printf("server waiting.....");
    if(recvfrom(sockfd,buffer,100,0,(struct sockaddr*)&server,&server_len)<0)
    printf("Error in recvfrom()!");
    printf("Got a datagram:%s",buffer);
    return 0;
}

********************************************************************************
OUTPUT

SERVER
student@student-OptiPlex-3010:~/Desktop/networkpgms$ cc udps.c -o sev
student@student-OptiPlex-3010:~/Desktop/networkpgms$ ./sev 2050
server waiting.....Got a datagram:hai

********************************************************************************
CLIENT
student@student-OptiPlex-3010:~/Desktop/networkpgms$ cc udpc.c -o cli
student@student-OptiPlex-3010:~/Desktop/networkpgms$ ./cli localhost 2050
Enter a message to be sent to server : hai

********************************************************************************


Distance Vector Routing in this
program is implemented using
Bellman Ford Algorithm:-

#include<stdio.h>

struct node
{
unsigned dist[20];
unsigned from[20];
}rt[1 0];
int main()
{
int costmat[20][20];
int nodes,ij,k,count=0;

printf("\nEnter the number of nodes : ");
scanf("%d",&nodes);
printf("\nEnter the cost matrix:\n");

for(i=0;i<nodes;i++)
{
for(j=0;j<nodes;j++)
{
scanf("%d",&costmattil[j]);
costmat[i][i]=0;
rt[i].dist[]=costmatti][j];//
 
initialise the distance equal to cost
matrix


rt{i].from[j]=j;
}
}
do
{
count=0;
for(i=0;i<nodes;i++)
for(j=0;j<nodes;j++)
for(k=0;k<nodes;k++)
if(rt[i].dist[j]>costmat[i][k]+rt|k].dist[j])
{
rt{i].dist[j]=rt[i].dist[k]+rt[k].dist[j];
rt[i].from[j]=k;
count++;

}while(count!=0);
for(i=0;i<nodes;i++)

printf("\n\n For router
%d\n",i+1); |
for(j=0;j<nodes;j++)
{
printf("\t\n node %d via %d Distance %d",j+1,rt[i].from[j]+1,rt[i].dist[j]);
}
}
printf)("\n\n");
getch();
}


