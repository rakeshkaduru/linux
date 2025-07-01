//3. Write a C program to demonstrate the use of fork() system call.
#include<stdio.h>
int main()
{
	int pid;
	pid=fork();
	if(pid<0)
	{
		printf("failed to create ");
	}
	else if(pid==0)
	{
		printf("child is create\n");
	}
	else
	{
		printf("parent is create\n");
	}
}
//10. Write a program in C to create a child process using fork() and print its PID.
#include<stdio.h>
#include<unistd.h>
int main()
{
	int fd;
	pid_t pid=fork();
	if(pid==0)
	{
		printf("child process%d\n",getpid());
	}
	else
	{
		printf("parent process%d\n",getpid());

	}
}
//15. Write a C program to create multiple child processes using fork() and display their PIDs.
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
int main()
{
	for(int i=0;i<5;i++)
	{
		pid_t pid=fork();
		if(pid==0)
		{
			printf("child %d PID=%d parent PID=%d\n",i+1,getpid(),getppid());
			return 0;
		}
		else if(pid<0)
		{
			printf("fork failed");
			return 1;
		}
	}
		sleep(1);
		printf("parent process 	PID=%d\n",getpid());
		return 0;

	
}
//19. Write a program in C to create a zombie process and explain how to avoid it.
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        printf("error");
    } 
    else if (pid == 0) {
        printf("Child process (PID: %d) exiting...\n", getpid());
        exit(0);  
    } 
    else {
	    printf("this parent process%d\n",getpid());
	    sleep(5);
    }

    return 0;
}

//22. Write a C program to demonstrate the use of the waitpid() function for process synchronization.
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main()
{
	pid_t pid;
	int status;
	pid=fork();
	if(pid<0)
	{
		printf("failed");
		exit(1);
	}
	else if(pid==0)
	{
		printf("child %d\n",getpid());
		sleep(4);
		printf("child is exist is sucess\n");
		exit(42);
	}
	else
	{
		printf("parent :waiting for to finish\n");
		pid_t wpid=waitpid(pid,&status,0);
		if(wpid==-1)
		{
			printf("faild wait pid\n");
		}

		if(WIFEXITED(status))
		{
			printf("parent :child exist status %d\n",WEXITSTATUS(status));
		}
		else
		{
			printf("parent :child is failed to exit");
		}
	}
}

//25. Write a program in C to create a daemon process.
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
int main()
{
	pid_t pid;
	pid=fork();
	if(pid<0)
	{
		printf("failed open fd");
		exit(1);
	}
	else if(pid==0)
	{
		printf("create child  process");
	}
	else
	{
		printf("parent process\n");
		exit(1);
	}
	setsid();
	chdir("/");
	umask(0);
	close(0);
	close(1);
	close(2);
	while(1)
	{
		sleep(10);
	}
	return 0;
}

	
//28. Write a C program to demonstrate the use of the system() function for executing shell commands. 
#include<stdio.h>
#include<stdlib.h>
int main()
{
	printf("list file in current working directory\n");
	system("ls -l");

	printf(" display date & time of file\n");
	system("date");

	printf(" create a empty file as rakesh.txt\n");
	system("touch rakesh.txt");
	printf(" rakesh.txt os existing or not\n");
	system("ls -l rakesh.txt");
	return 0;
}
//32. Write a C program to create a process using fork() and pass arguments to the child process. 
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main() {
    pid_t pid;
    pid = fork();
    if (pid < 0) {
        perror("fork failed");
        exit(1);
    }
    if (pid == 0) {
        printf("Child: Executing 'ls -l /'\n");
        char *args[] = {"ls", "-l", "/", NULL};
        execvp("ls", args);
        perror("exec failed");
        exit(1);
    } else {
        printf("Parent: Created child process with PID %d\n", pid);
        wait(NULL);  
        printf("Parent: Child process finished.\n");
    }
    return 0;
}
//35. Write a program in C to demonstrate process synchronization using semaphores.
#include<stdio.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<sys/sem.h>
#define SHM_KEY 25223
#define SEM_KEY 46532
void main()
{
	int shmid,semid;
	char *shmptr=NULL;
	struct sembuf smop;
	shmid=shmget(SHM_KEY,512,IPC_CREAT|0666);
	semid=semget(SEM_KEY,2,IPC_CREAT|0666);
	shmptr=shmat(shmid,NULL,0);
	semctl(semid,0,SETVAL,0);
	semctl(semid,1,SETVAL,0);
	smop.sem_num=0;
	smop.sem_op=-1;
	smop.sem_flg=0;
	printf("waiting message from client\n");
	semop(semid,&smop,1);
	printf("recevied %s\n",shmptr);

//39. Write a C program to demonstrate the use of the execvpe() function.
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    char *args[] = {"ls", "-l", NULL};
    char *env[] = {
        "MYVAR=HelloWorld",
        NULL
    };
    printf("Before execvpe()...\n");
    if (execvpe("ls", args, env) == -1) {
        perror("execvpe failed");
        exit(EXIT_FAILURE);
    }
    printf("This line won't print if execvpe is successful.\n");
    return 0;
}

//42. Write a C program to create a process group and change its process group ID (PGID).
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<stdlib.h>
int main()
{
	pid_t pid;
	pid=fork();
	if(pid<0)
	{
		printf("error pid");
	}
	if(pid==0)
	{
		printf("\n___child process___\n");
		printf("child process %d\n",getpid());
		printf("child process %d\n",getpgid(0));
		if(setpgid(0,0)==0)
		{
			printf("child pid  is become own  child %d\n",getpgid(0));
		}
		else
		{
			printf("errror");
		}
		sleep(2);
	}
	else
	{
		printf("\n____parent process __\n");
		printf("parent process%d\n",getpid());
		printf("parent process%d\n",getpgid(0));
		sleep(2);
		printf("parent send to change of %d\n",getpgid(pid));
	}
}
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<sys/types.h>
#define SHM_KEY 25223
void main()
{
        int shmid;
        char *shmptr=NULL;
        shmid=shmget(SHM_KEY,512,IPC_CREAT|0660);
        shmptr=shmat(shmid,NULL,0);
        printf("waiting for message from client\n");
        printf("received %s\n",shmptr);
}

#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<sys/types.h>
#define SHM_KEY 25223
void main()
{
        int shmid;
        char *shmptr;
        shmid=shmget(SHM_KEY,512,0);
        shmptr=shmat(shmid,NULL,0);
        printf("enter a message for server \n");
        scanf("%s",shmptr);

}
//49. Write a C program to create a child process using vfork() and demonstrate its usage.
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main() {
    int x = 10;
    pid_t pid;
    printf("Before vfork: x = %d\n", x);
    pid = vfork();  // Create a child process using vfork()
    if (pid < 0) {
        perror("vfork failed");
        exit(1);
    } else if (pid == 0) {
        printf("Child process: PID = %d\n", getpid());
        x += 5;  // Modifying x is dangerous in vfork — for demo only!
        printf("Child changed x to %d\n", x);
        _exit(0);  // Use _exit() instead of exit()
    } else {
        printf("Parent process: PID = %d\n", getpid());
        printf("Back in parent: x = %d\n", x);
    }
    return 0;
}
//52. Write a C program to create a pipeline between two processes using the pipe() system call. 
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
int main() {
    int fd[2]; 
    pid_t pid;
    char write_msg[] = "Hello from parent!";
    char read_msg[100];
    if (pipe(fd) == -1) {
        perror("pipe");
        exit(EXIT_FAILURE);
    }
    pid = fork();
    if (pid < 0) {
        perror("fork");
        exit(EXIT_FAILURE);
    }
    if (pid == 0) {
        close(fd[1]); 
        read(fd[0], read_msg, sizeof(read_msg));
        printf("Child received: %s\n", read_msg);
        close(fd[0]); 
    } else {
        close(fd[0]);
        write(fd[1], write_msg, strlen(write_msg) + 1);
        close(fd[1]); 
    }
    return 0;
}

//55. Write a program in C to demonstrate the use of the nice() system call for adjusting process priority.
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<errno.h>
int main()
{
	int current_nice;
	errno=0;
	if(current_nice==-1 && errno!=0)
	{
		printf("nice(get)");
		exit(EXIT_FAILURE);
	}
	int new_nice=0;
	if(new_nice==-1&&errno!=0)
	{
		printf("nice (set)");
		exit(EXIT_FAILURE);
	}
	printf("new assign value nice(0):%d\n",new_nice);
	for(int i=0;i<5;i++)
	{
		printf("working ___%d\n",i+1);
		sleep(2);
		

	}
	return 0;
}

