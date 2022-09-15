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


                                      DISTANCE VECTOR ROUTING

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


                            FILE TRANSFER PROTOCOL
			    
SERVER:

#include<stdio.h>
#include<arpa/inet.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<netdb.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#define SERV_TCP_PORT 5035
#define MAX 60
int i, j, tem;
char buff[4096], t;
FILE *f1;
int main(int afg, char *argv)
{
       int sockfd, newsockfd, clength;
       struct sockaddr_in serv_addr,cli_addr;
       char t[MAX], str[MAX];
       strcpy(t,"exit");
       sockfd=socket(AF_INET, SOCK_STREAM,0);
       serv_addr.sin_family=AF_INET;
       serv_addr.sin_addr.s_addr=INADDR_ANY;
       serv_addr.sin_port=htons(SERV_TCP_PORT);
       printf("\nBinded");
       bind(sockfd,(struct sockaddr*)&serv_addr, sizeof(serv_addr));
       printf("\nListening...");
       listen(sockfd, 5);
       clength=sizeof(cli_addr);
       newsockfd=accept(sockfd,(struct sockaddr*) &cli_addr,&clength);
       close(sockfd);
       read(newsockfd, &str, MAX);
       printf("\nClient message\n File Name : %s\n", str);
       f1=fopen(str, "r");
       while(fgets(buff, 4096, f1)!=NULL)
       {
            write(newsockfd, buff,MAX);
            printf("\n");
       }
       fclose(f1);
       printf("\nFile Transferred\n");
       return 0;
}

CLIENT:

#include<stdio.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<netdb.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#define SERV_TCP_PORT 5035
#define MAX 60
int main(int arg,char*argv[])
{
       int sockfd,n;
       struct sockaddr_in serv_addr;
       struct hostent*server;
       char send[MAX],recvline[MAX],s[MAX],name[MAX];
       sockfd=socket(AF_INET,SOCK_STREAM,0);
       serv_addr.sin_family=AF_INET;
       serv_addr.sin_addr.s_addr=inet_addr("127.0.0.1");
       serv_addr.sin_port=htons(SERV_TCP_PORT);
       connect(sockfd,(struct sockaddr*)&serv_addr,sizeof(serv_addr));
       printf("\nEnter the source file name : \n");
       scanf("%s",send);
       write(sockfd,send,MAX);
       while((n=read(sockfd,recvline,MAX))!=0)
       {
          printf("%s",recvline);
       }
       close(sockfd);
       return 0;
}

       
                           SLIDING WINDOW:
		 
STOP AND WAIT:

#include<stdio.h>

#define MAXSIZE 100
typedef struct
{
unsigned char data[MAXSIZE];
}packet;
typedef enum{data,ack}frame_kind;
typedef struct
{
    frame_kind kind;
    int sq_no;
    int ack;
    packet info;
}frame;
typedef enum{frame_arrival}event_type;
typedef enum{true_false}boolean;
void frame_network_layer(packet *p)
{
    printf("\n from network arrival");
}
void to_physical_layer(frame *f)
{
    printf("\n to physical layer");
}
void wait_for_event(event_type *e)
{
    printf("\n waiting for event n");
}
void sender(void)
{
    frame s;
    packet buffer;
    event_type event;
    printf("\n ***SENDER***");
    frame_network_layer(&buffer);
    s.info=buffer;
    to_physical_layer(&s);
    wait_for_event(&event);
}
void from_physical_layer(frame *f)
{
    printf("from physical layer");
}
void to_network_layer(packet *p)
{
    printf("\n to network layer");
}
void receiver(void)
{
    frame r,s;
    event_type event;
    printf("\n ***RECEIVER***");
    wait_for_event(&event);
    from_physical_layer(&r);
    to_network_layer(&r.info);
    to_physical_layer(&s);
}
int main()
{
    sender();
    receiver();
return (0);
}

GO BACK N:

#include<stdio.h>
int main()
{
	int windowsize,sent=0,ack,i;
	printf("enter window size\n");
	scanf("%d",&windowsize);
	while(1)
	{
		for( i = 0; i < windowsize; i++)
			{
				printf("Frame %d has been transmitted.\n",sent);
				sent++;
				if(sent == windowsize)
					break;
			}
			printf("\nPlease enter the last Acknowledgement received.\n");
			scanf("%d",&ack);
			
			if(ack == windowsize-1)
				break;
			else
				sent = ack+1;
	}
return 0;
}

SELECTIVE REPEAT:

#include<stdio.h>
#include<stdlib.h>
#include<math.h>
#include<unistd.h>
int n,r;
struct frame
{
char ack;
int data;
}frm[10];
int sender(void);
void recvack(void);
void resend(void);
void resend1(void);
void goback(void);
void selective(void);
int main()
{
int c;
do
{
printf("\n\n1.Selective repeat ARQ\n2.exit");
printf("\nEnter your choice:");
scanf("%d",&c);
switch(c)
{
case 1:selective();
break;
case 2:exit(0);
break;
}
}while(c>=3);
}
void selective()
{
sender();
recvack();
resend();
printf("\nAll packets sent successfully");
}
int sender()
{
int i;
printf("\nEnter the no. of packets to be sent:");
scanf("%d",&n);
for(i=1;i<=n;i++)
{
printf("\nEnter data for packets[%d]",i);
scanf("%d",&frm[i].data);
frm[i].ack='y';
}
return 0;
}
void recvack()
{
int i;
rand();
r=rand()%n;
frm[r].ack='n';
for(i=1;i<=n;i++)
{
if(frm[i].ack=='n')
printf("\nThe packet number %d is not received\n",r);
}
}
void resend() //SELECTIVE REPEAT
{
printf("\nresending packet %d",r);
sleep(2);
frm[r].ack='y';
printf("\nThe received packet is %d",frm[r].data);
}


                                    MULTI-USER CHAT SYSTEM
				    
CLIENT:

#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<string.h>
#include<netinet/in.h>
#define PORT 4444
#define BUF_SIZE 2000
int main(int argc, char**argv) 
{
struct sockaddr_in addr, cl_addr;
int sockfd, ret;
char buffer[BUF_SIZE];
struct hostent * server;
char * serverAddr;
if (argc < 2) {
printf("usage: client < ip address >\n");
exit(1);
}
serverAddr = argv[1];
sockfd = socket(AF_INET,SOCK_STREAM, 0);
if(sockfd<0) 
{
printf("Error creating socket!\n");
exit(1);
}
printf("Socket created...\n");
memset(&addr, 0, sizeof(addr));
addr.sin_family = AF_INET;
addr.sin_addr.s_addr = inet_addr(serverAddr);
addr.sin_port = PORT;
ret = connect(sockfd, (struct sockaddr *) &addr, sizeof(addr));
if (ret < 0) {
printf("Error connecting to the server!\n");
exit(1); }
printf("Connected to the server...\n");
memset(buffer, 0, BUF_SIZE);
printf("Enter your message(s): ");
while (fgets(buffer, BUF_SIZE, stdin) != NULL) {
ret = send(sockfd, buffer, BUF_SIZE, 0);
if (ret < 0) {
printf("Error sending data!\n\t-%s", buffer);
}
ret = recv(sockfd, buffer, BUF_SIZE, 0);
if (ret < 0) {
printf("Error receiving data!\n");
}
else
{
printf("Received: ");
fputs(buffer, stdout);
printf("\n"); }
}
return 0; }

SERVER:

#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<string.h>
#include<netinet/in.h>
#define PORT 4444
#define BUF_SIZE 2000
#define CLADDR_LEN 100
void main() {
struct sockaddr_in addr, cl_addr;
int sockfd, len, ret, newsockfd;
char buffer[BUF_SIZE];
pid_t childpid;
char clientAddr[CLADDR_LEN];
sockfd = socket(AF_INET, SOCK_STREAM, 0);
if (sockfd < 0) {
printf("Error creating socket!\n");
exit(1);
}
printf("Socket created...\n");
memset(&addr, 0, sizeof(addr));
addr.sin_family = AF_INET;
addr.sin_addr.s_addr = INADDR_ANY;
addr.sin_port = PORT;
ret = bind(sockfd, (struct sockaddr *) &addr, sizeof(addr));
if (ret < 0) {
printf("Error binding!\n");
exit(1); }
printf("Binding done...\n");
printf("Waiting for a connection...\n");
listen(sockfd, 5);
for (;;) { 
len = sizeof(cl_addr);
newsockfd = accept(sockfd, (struct sockaddr *) &cl_addr, &len);
if (newsockfd < 0) {
printf("Error accepting connection!\n");
exit(1);
}
printf("Connection accepted...\n");
inet_ntop(AF_INET, &(cl_addr.sin_addr), clientAddr, CLADDR_LEN);
if ((childpid = fork()) == 0) { 
close(sockfd);
for (;;) {
memset(buffer, 0, BUF_SIZE);
ret = recv(newsockfd, buffer, BUF_SIZE, 0);
if(ret < 0) {
printf("Error receiving data!\n");
exit(1); }
printf("Received data from %s: %s\n", clientAddr, buffer);
ret = send(newsockfd, buffer, BUF_SIZE, 0);
if (ret < 0) {
printf("Error sending data!\n");
exit(1);
}
printf("Sent data to %s: %s\n", clientAddr, buffer);
} } close(newsockfd);
}}
