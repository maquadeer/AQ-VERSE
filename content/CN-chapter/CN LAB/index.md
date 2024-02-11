---
title: "CN LAB"
date: 2023-11-10T11:20:05+05:30
draft: false
---
{{< alert >}}
Click <a href= "https://ashwithbash.vercel.app">test-Llama-2-70B</a> to view detailed explanation of below programs.
{{< /alert >}}

## Program-1 : Network Command Explanation

* `tcpdump`

`tcpdump` is a packet analyzer tool that allows you to capture and analyze network traffic. You can adjust the interface and packet count as needed for your specific use case.

* `netstat`

`netstat` is a command-line utility for displaying network statistics, including open ports and active network connections. The command `netstat -tuln` shows listening TCP and UDP ports.

* `ifconfig`

`ifconfig` is used to display and configure network interface parameters, including IP addresses, netmasks, and more. It provides information about the network interfaces on your system.

* `nslookup`

`nslookup` is a network utility for performing DNS (Domain Name System) lookups. In the provided example, it performs a DNS lookup for 'example.com', returning information about the domain's IP addresses.

* FTP

FTP *(File Transfer Protocol) is a network protocol used for transferring files between a client and a server. To interact with FTP programmatically, you should use dedicated FTP client libraries in C, as the command-line `ftp` client may not be suitable for automation.

* TELNET

T*ELNET is a network protocol that allows terminal access to remote systems. To interact with TELNET servers programmatically, you'll need to use a TELNET library or API, as it's not straightforward to automate using a command-line program.

* `traceroute`

`traceroute` is a network diagnostic tool used to trace the route that packets take from your computer to a destination host (e.g., 'example.com'). It displays a list of intermediate routers and their response times.

Please note that some of these services/commands may require additional configuration, and some may not be available on all systems. The specific options and usage may vary depending on your operating system and network setup.

{{< alert >}}
 I’ve tried making a program demonstrating all these commands
{{< /alert >}}

```c
// Program containing all commands in switch 
#include <stdio.h>
#include <stdlib.h>

int main() {
    int choice;
    
    while (1) {
        printf("Select an option:\n");
        printf("1. Run tcpdump\n");
        printf("2. Run netstat\n");
        printf("3. Run ifconfig\n");
        printf("4. Run nslookup\n");
        printf("5. Run FTP\n");
        printf("6. Run TELNET\n");
        printf("7. Run traceroute\n");
        printf("8. Exit\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);
        
        switch (choice) {
            case 1:
                system("tcpdump -i eth0"); // Replace 'eth0' with your network interface
                break;
            case 2:
                system("netstat -tuln");
                break;
            case 3:
                system("ifconfig");
                break;
            case 4:
                printf("Enter the hostname to look up: ");
                char hostname[256];
                scanf("%s", hostname);
                char nslookupCommand[256];
                sprintf(nslookupCommand, "nslookup %s", hostname);
                system(nslookupCommand);
                break;
            case 5:
                system("ftp"); // This will start an interactive FTP session
                break;
            case 6:
                system("telnet"); // This will start an interactive TELNET session
                break;
            case 7:
                printf("Enter the destination IP or hostname for traceroute: ");
                char dest[256];
                scanf("%s", dest);
                char tracerouteCommand[256];
                sprintf(tracerouteCommand, "traceroute %s", dest);
                system(tracerouteCommand);
                break;
            case 8:
                exit(0);
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}
```

## Program-2a,2b,2c : UDP-client
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<sys/types.h>
#include<string.h>
main(int argc, char * argv[])
{
	struct sockaddr_in s;
	int rval,sockid,slen;
	char m1[20],m2[20];
	system("clear");
	if(argc<3)
	{
		printf("\n USAGE : %s IP_Address port \n",argv[0]);
		exit(1);
	}
	sockid=socket(AF_INET,SOCK_DGRAM,0);
	if(sockid ==-1)
	{
		perror("SOCK-CRE-ERR:");
		exit(1);
	}
	s.sin_family=AF_INET;
	s.sin_addr.s_addr=inet_addr(argv[1]);
	s.sin_port=htons(atoi(argv[2]));
	printf("\n 	ENTER THE REQUEST MESSAGE ::");
	scanf("%s",m1);
	slen=sizeof(s);
	rval=sendto(sockid,m1,sizeof(m1),0,(struct sockaddr *)&s,slen);
	if(rval==-1)
	{
		perror("MSG-SEND-ERR:");
		exit(1);
	}
	printf("\n message sent sucessfully \n");
	strncpy(m2," ",20);
	rval=recvfrom(sockid,m2,sizeof(m2),0,(struct sockaddr *)&s,&slen);
	if(rval==-1)
	{
		perror("MSG-RCV-ERR:");
		exit(1);
	}
	printf("\n message received is %s \n",m2);
	close(sockid);
}
```
## Program-2d : UDP-TIME
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<sys/types.h>
#include<string.h>
main(int argc, char * argv[])
{
	unsigned long timeval,tempval;
	struct sockaddr_in s;
	int rval,sockid,slen;
	char m1[20],m2[100];
	system("clear");
	if(argc<3){
		printf("\nUSAGE :%s IP-Address Port #\n", argv[0]);
		exit(1);
	}
	sockid = socket(AF_INET, SOCK_DGRAM,0);
	if(sockid ==  -1){
		perror("SOCK-CRE-ERR:");
		exit(1);
	}
	s.sin_family= AF_INET;
	s.sin_addr.s_addr = inet_addr(argv[1]);
	s.sin_port = htons(atoi(argv[2]));
	printf("\nSocket created");
	slen = sizeof(s);
	rval = sendto(sockid,m1,sizeof(m1),0,(struct sockaddr *)&s,slen);
	if(rval == -1){
		perror("\nMSG-SEND-ERR:");
		exit(1);
	}
	printf("\nMessage sent successfully\n");
	rval = recvfrom(sockid,&tempval,sizeof(tempval),0, (struct sockaddr *)&s,&slen);
	if(rval == -1){
	perror("MSG-RCV-ERR");
	exit(1);
	}
	timeval = htonl(tempval);
	printf("\nMessage received is :%u\n",timeval);
	close(sockid);
}

```

## Program-4 : UDP-SERVER
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<string.h>
main(int argc,char * argv[])
{
	int sockid,rval,clen;
	char buffer[20];
	struct sockaddr_in s,c;
	if(argc<3){
		printf("\nUSAGE:%s IP_ADDRESS PORT #\n",argv[0]);
		exit(0);
	}
	sockid = socket(AF_INET,SOCK_DGRAM,0);
	if(sockid == -1){
		perror("SOCK-CRE-ERR");
		exit(1);
	}
	s.sin_family = AF_INET;
	s.sin_addr.s_addr = inet_addr(argv[1]);
	s.sin_port = htons(atoi(argv[2]));
	printf("socket created\n");
	rval = bind(sockid,(struct sockaddr *)&s,sizeof(s));
	if(rval == -1){
		perror("BIND-ERR");
		close(sockid);
		exit(1);
	}
	printf("socket binded\n");
	clen = sizeof(c);
	rval = recvfrom(sockid,buffer,sizeof(buffer),0,(struct sockaddr *)&c,&clen);
	if(rval == -1){
		perror("MSG-RCV-ERR");
		exit(1);
	}
	printf("\nRequest received\nRequest message is :%s\n",buffer);
	strcat(buffer," Quadeer");
	rval = sendto(sockid,buffer,sizeof(buffer),0,(struct sockaddr *)&c,clen);
	if(rval == -1){
		perror("MSG-SND-ERR");
		exit(1);
	}
	printf("\nResponse sent successfully\n");
	close(sockid);
}
```
## Program-5 : UDP-SERVER-DTS
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <sys/types.h>
#include <string.h>
main(int argc, char *argv[])
{
    unsigned long timeval, tempval;
    struct sockaddr_in s;
    int rval, sockid, slen;
    char m1[20], m2[20];
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP-Address Port#\n", argv[0]);
        exit(1);
    }
    sockid = socket(AF_INET, SOCK_DGRAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    s.sin_family = AF_INET;
    s.sin_port = htons(atoi(argv[2]));
    s.sin_addr.s_addr = inet_addr(argv[1]);
    printf("Socket Created\n");
    printf("\nEnter the request message : ");
    scanf("%s", m1);
    slen = sizeof(s);
    rval = sendto(sockid, m1, sizeof(m1), 0, (struct sockaddr *)&s, slen);
    if (rval == -1)
    {
        perror("MSG-SEND-ERR:");
        exit(1);
    }
    printf("\nMessage sent successfully\n");
    rval = recvfrom(sockid, &tempval, sizeof(tempval), 0, (struct sockaddr *)&s, &slen);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        exit(1);
    }
    timeval = htonl(tempval);
    printf("\nMessage received is : %u\n", timeval);
    close(sockid);
}
```

## Program-3a,3b,3c: TCP-CLIENT
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    struct sockaddr_in s;
    int sockid, rval;
    char m1[20], m2[20];
    sockid = socket(AF_INET, SOCK_STREAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR");
        exit(1);
    }
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
        exit(0);
    }
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    rval = connect(sockid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("CONN-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nEnter the request message : ");
    scanf("%s", m1);
    rval = send(sockid, m1, sizeof(m1), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nMessage sent successfully\n");
    rval = recv(sockid, m2, sizeof(m2), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nServer response is : %s\n", m2);
    close(sockid);
}
```
## Program-3d: TCP-TIME
```c

#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    unsigned long timeval, tempval;
    int sockid, rval;
    char m1[20], m2[20];
    sockid = socket(AF_INET, SOCK_STREAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR");
        exit(1);
    }
    struct sockaddr_in s;
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
        exit(0);
    }
    s.sin_family = AF_INET;
    s.sin_port = htons(atoi(argv[2]));
    s.sin_addr.s_addr = inet_addr(argv[1]);
    rval = connect(sockid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("CONN-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nEnter the request message : ");
    scanf("%s", m1);
    rval = send(sockid, m1, sizeof(m1), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nMessage sent successfully\n");
    rval = recv(sockid, &tempval, sizeof(tempval), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
    timeval = htonl(tempval);
    printf("\nServer response is : %u\n", timeval);
    close(sockid);
}
```

## Program-6: TCP-SERVER
```C
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval;
    struct sockaddr_in s, c;
    char buffer[20];
    int clen;
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    clen = sizeof(c);
    sid1 = accept(sid, (struct sockaddr *)&c, &clen);
    rval = recv(sid1, buffer, sizeof(buffer), 0);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
    }
    else
    {
        printf("\nClient request is %s\n", buffer);
    }
    rval = send(sid1, buffer, sizeof(buffer), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
    }
    else
    {
        printf("\nResponse sent\n");
    }
    close(sid);
    close(sid1);
}
```

## Program-7: TCP-SERVER-DTS
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <time.h>
#include <string.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval;
    time_t t = time(0);
    struct sockaddr_in s, c;
    char smsg[30];
    strcpy(smsg, ctime(&t));
    int clen;
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }   /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    clen = sizeof(c);
    sid1 = accept(sid, (struct sockaddr *)&c, &clen);
    strcpy(smsg, ctime(&t)); // const time_t* if error
    rval = send(sid1, smsg, sizeof(smsg), 0);
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
    }
    else
    {
        printf("\nResponse sent\n");
    }
    close(sid);
    close(sid1);
}
```

## Program 8: TCP-ITERATIVE SERVER
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval, itr, i; // sid is half association. sid1 is full association
    struct sockaddr_in s, c;
    char buffer[20];
    int clen; // accept() uses value-result parameter
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
        printf("\nEnter the number of clients to serve/ server iterations : ");
    scanf("%d", &itr);
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    for (i = 1; i <= itr; i++)
    {
        clen = sizeof(c);
        sid1 = accept(sid, (struct sockaddr *)&c, &clen);
        /*sid1 is a full association tuple and has information of client,server and communication
        protocol i.e serving socket*/
        rval = recv(sid1, buffer, sizeof(buffer), 0);
        if (rval == -1)
        {
            perror("MSG-RCV-ERR:");
        }
        else
        {
            printf("\nClient request is %s\n", buffer);
        }
        rval = send(sid1, buffer, sizeof(buffer), 0);
        if (rval == -1)
        {
            perror("MSG-SND-ERR:");
        }
        else
        {
            printf("\nResponse sent\n");
        }
        close(sid1); // closing the serving socket
    }
    close(sid); // closing the listening socket
}
```

## Program 8: TCP-CONC SERVER
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
main(int argc, char *argv[])
{
    int sid, sid1, rval, itr, i, pid; // sid is half association. sid1 is full association
    struct sockaddr_in s, c;
    char buffer[20];
    int clen; // accept() uses value-result parameter
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDRESS PORT#\n", argv[0]);
        exit(0);
    }
    printf("\nEnter the number of clients to serve/ server iterations : ");
    scanf("%d", &itr);
    sid = socket(AF_INET, SOCK_STREAM, 0);
    if (sid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    /*DEFINING NAME OF THE SERVICE*/
    s.sin_family = AF_INET;
    s.sin_addr.s_addr = inet_addr(argv[1]);
    s.sin_port = htons(atoi(argv[2]));
    /*BIND SOCKET- indicates the process that is listening*/
    rval = bind(sid, (struct sockaddr *)&s, sizeof(s));
    if (rval == -1)
    {
        perror("BIND-ERR:");
        close(sid);
        exit(1);
    }
    rval = listen(sid, 5); // range : 1-5
    if (rval == -1)
    {
        perror("LISTEN-ERR:");
        close(sid);
        exit(1);
    }
    for (i = 1; i <= itr; i++)
    {
        clen = sizeof(c);
        sid1 = accept(sid, (struct sockaddr *)&c, &clen);
        if (sid1 == -1)
        {
            perror("ACCEPT-ERR:");
            close(sid);
            exit(1);
        }
        pid = fork();
        if (pid == -1)
        {
            perror("FRK-ERR:");
            close(sid1);
            close(sid);
            exit(1);
        }
        /*sid1 is a full association tuple and has information of client,server and communication
        protocol i.e serving socket*/
        if (pid == 0) // CHILD
        {
            rval = recv(sid1, buffer, sizeof(buffer), 0);
            if (rval == -1)
            {
                perror("MSG-RCV-ERR:");
            }
            else
            {
                printf("\nClient request is %s\n", buffer);
            }
            rval = send(sid1, buffer, sizeof(buffer), 0);
            if (rval == -1)
            {
                perror("MSG-SND-ERR:");
            }
            else
            {
                printf("\nResponse sent\n");
            }
            close(sid1); // closing the serving socket
            exit(0);
        }
        else             // PARENT
            close(sid1); /*parent also has a copy of the serving socket. So close it here.*/
    }
    close(sid); // closing the listening socket
    exit(0);
}
```

## Program 10: DNS
==DNS CLIENT==
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <string.h>
main(int argc, char *argv[])
{
    struct sockaddr_in dnss;
    int sockid, rval;
    char sym[20], IP[20];
    int slen;
    system("clear");
    if (argc < 3)
    {
        printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
        exit(1);
    }
    sockid = socket(AF_INET, SOCK_DGRAM, 0);
    if (sockid == -1)
    {
        perror("SOCK-CRE-ERR:");
        exit(1);
    }
    dnss.sin_family = AF_INET;
    dnss.sin_port = htons(atoi(argv[2]));
    dnss.sin_addr.s_addr = inet_addr(argv[1]);
    printf("\nEnter the symbolic name of resource : ");
    scanf("%s", sym);
    rval = sendto(sockid, sym, sizeof(sym), 0, (struct sockaddr *)&dnss, sizeof(dnss));
    if (rval == -1)
    {
        perror("MSG-SND-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nWaiting to receive from DNS Server\n");
    slen = sizeof(dnss);
    strncpy(IP, " ", 20);
    rval = recvfrom(sockid, IP, sizeof(IP), 0, (struct sockaddr *)&dnss, &slen);
    if (rval == -1)
    {
        perror("MSG-RCV-ERR:");
        close(sockid);
        exit(1);
    }
    printf("\nEquivalent IP adrress of %s is %s\n", sym, IP);
    close(sockid);
}
```
==DNS SERVER==
```c
    /*DNS SERVER PROGRAM*/
    #include <stdio.h>
    #include <stdlib.h>
    #include <sys/types.h>
    #include <sys/socket.h>
    #include <netinet/in.h>
    #include <string.h>
    main(int argc, char *argv[])
    {
        struct sockaddr_in dnss, dnsc;
        int rval, sockid, flag = 0, clen;
        char sym[20], IP[20], dnsFile[20], dnsName[20];
        FILE *fptr;
        system("clear");
        if (argc < 3)
        {
            printf("\nUSAGE : %s IP_ADDR PORT#\n", argv[0]);
            exit(1);
        }
        dnss.sin_family = AF_INET;
        dnss.sin_addr.s_addr = inet_addr(argv[1]);
        dnss.sin_port = htons(atoi(argv[2]));
        sockid = socket(AF_INET, SOCK_DGRAM, 0);
        if (sockid == -1)
        {
            perror("SOCK-CRE-ERR:");
            exit(1);
        }
        rval = bind(sockid, (struct sockaddr *)&dnss, sizeof(dnss));
        if (rval == -1)
        {
            perror("BIND-ERR:");
            close(sockid);
            exit(1);
        }
        printf("\nDNS Server waiting for request\n");
        printf("\nEnter the DNS file name : ");
        scanf("%s", dnsFile);
        /*OPEN THE FILE*/
        fptr = fopen(dnsFile, "r");
        if (fptr == NULL)
        {
            perror("FILE-OPEN-ERR:");
            close(sockid);
            exit(1);
        }
        clen = sizeof(dnsc);
        rval = recvfrom(sockid, sym, sizeof(sym), 0, (struct sockaddr *)&dnsc, &clen);
        if (rval == -1)
        {
            perror("MSG-RCV-ERR:");
            close(sockid);
            fclose(fptr);
            exit(1);
        }
        printf("\nIP requested for %s\n", sym);
        while ((fscanf(fptr, "%s%s", dnsName, IP) != EOF))
        {
            if (strcmp(dnsName, sym) == 0)
            {
                rval = sendto(sockid, IP, sizeof(IP), 0, (struct sockaddr *)&dnsc, clen);
                if (rval == -1)
                {
                    perror("MSG-SND-ERR:");
                    fclose(fptr);
                    close(sockid);
                    exit(1);
                }
                flag = 1;
            }
            printf("\n flag value in loop is %d\n", flag);
            if (flag == 1) // INDICATES THAT MATCH IS FOUND
                break;
        }
        if (flag == 0)
        {
            printf("\n invalid domain name case\n");
            rval = sendto(sockid, "NOT FOUND", sizeof("NOT FOUND"), 0, (struct sockaddr *)&dnsc, clen);
            if (rval == -1)
            {
                perror("MSG-SND-ERR:");
                fclose(fptr);
                close(sockid);
                exit(1);
            }
        }
        fclose(fptr);
        close(sockid);
    }
```