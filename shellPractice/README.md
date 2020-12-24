
```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#define READ 0
#define WRITE 1

int main(int argc,char*argv[])
{
    // 나의 쉘 만들기
	while(1){
	char str[1024];
	char *command1,*command2;
	int fd[2];
	int fd2;
	int amper=0;
	int status;
	char buf[1024];
	printf("[prompt] ");
    //입력 받기
	fgets(str,sizeof(str),stdin);
	str[strlen(str)-1] = '\0';
	
    //종료 코드
	if(strcmp(str,"exit")==0||strcmp(str,"logout")==0)
		exit(0);
	
    //&일 경우(백그라운드에서)
	if(strchr(str,'&')!=NULL){
		amper=1;  // amper로 이후에 백그라운드 인지 포그라운드인지 비교
		command1 = strtok(str,"&");
		printf(str);
		printf("\n");
		printf("%d",amper);
		}

    //자식 생성 
	if(fork()==0){
        //리다이렉션 '>'일 경우
		if(strchr(str,'>')!=NULL){
			command1 = strtok(str,">");
			command2 = strtok(NULL,">");
	
			if((fd2 = creat(command2,0644))<0)
				perror("creat error");
			close(1);
			dup2(fd2,1);
			close(fd2);
			execlp(command1,command1,NULL);
			perror("> error");
		}

        //리다이렉션 '<'일 경우
		else if(strchr(str,'<')!=NULL){
			command1 = strtok(str,"<");
			command2 = strtok(NULL,"<");

			if((fd2 =open(command2,O_RDWR))<0)
				perror("open error");
			close(0);
			dup2(fd2,0);
			close(fd2);
			execlp(command1,command1,NULL);
			perror("< error");
		}

        // pipe 일 경우
	    if(strchr(str,'|')!=NULL){
			command1 = strtok(str,"|");
			command2 = strtok(str,"|");
		
			pipe(fd);
			if(fork()==0){ // 표준 출력을 dup로 가르키는 곳을 바꾼다
				close(fd[READ]);
				dup2(fd[WRITE],1);
				close(fd[WRITE]);
				execlp(command1,command1,NULL);
				perror("pipe");	
			}else{  // 표준입력을 dup로 바꿈
				close(fd[WRITE]);
				dup2(fd[READ],0);
				close(fd[READ]);
				execlp(command2,command2,NULL);
				perror("pipe2");
			}
		}

        //pipe가 아니면 바로 exec한다
		execlp(str,str,NULL);
		perror("fork error");

	}


// 포그라운드 일 경우 해당 쉘에서 그것이 끝날 때까지 기다려야 함으로 wait
	if(amper==0)
		wait(&status);

	else if(amper == 1){
		printf("no wait");
//		exit(1);
		}
	}
	
}
```

## 마이 쉘 실행 이미지

![shell](https://user-images.githubusercontent.com/40445477/103080527-6191bd80-4619-11eb-9bcc-15efaacf5bf4.PNG)
