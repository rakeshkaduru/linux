
86. Write a C program to demonstrate dynamic memory allocation using malloc().
```
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr = malloc(sizeof(int));
    if (ptr == NULL)
    {
	    return 1;
   
    }
    *ptr=42;
    printf("Value: %d\n", *ptr);
    free(ptr);
    return 0;
}
```

87. Implement a C program to allocate memory for an array dynamically using calloc().
```
#include <stdio.h>
#include <stdlib.h>
int main() {
    int *arr = calloc(5, sizeof(int));
    if (arr == NULL)
    {
	    return 1;
    }
    for(int i=0;i<5;i++)
    {
	    arr[i]=i+3;
    }
    for (int i = 0; i < 5; i++)
    {
	    printf("%d ", arr[i]);
    }
    free(arr);
    return 0;
}
```
88. Write a C program to resize dynamically allocated memory using realloc().
```
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr = malloc(2 * sizeof(int));
    arr[0] = 1;
    arr[1] = 2;
    arr = realloc(arr, 4 * sizeof(int));
    arr[2] = 3;
    arr[3] = 4;
    for (int i = 0; i < 4; i++) printf("%d ", arr[i]);
    free(arr);
    return 0;
}
```
89. Develop a program in C to allocate memory for a linked list node dynamically.
```
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *next;
};

int main() {
    struct Node *node = malloc(sizeof(struct Node));
    node->data = 10;
    node->next = NULL;
    printf("Node data: %d\n", node->data);
    free(node);
    return 0;
}
```

90. Implement a C program to simulate memory allocation using the first-fit algorithm.
```
#include <stdio.h>
#define MAX 10
int main() {
    int blockSize[MAX] = {100, 500, 200, 300, 600};
    int processSize[MAX] = {212, 417, 112, 426};
    int allocation[MAX];
    int i, j;
    for (i = 0; i < 4; i++) 
	    allocation[i] = -1;
    for (i = 0; i < 4; i++)
    {
        for (j = 0; j < 5; j++)
       	{
            if (blockSize[j] >= processSize[i])
	    {
                allocation[i] = j;
                blockSize[j] -= processSize[i];
                break;
            }
        }
    }
    for (i = 0; i < 4; i++) {
        printf("Process %d -> ", i+1);
        if (allocation[i] != -1)
            printf("Block %d\n", allocation[i]+1);
        else
            printf("Not Allocated\n");
    }
    return 0;
}
```
91. Write a C program to simulate memory allocation using the best-fit algorithm.
```
#include <stdio.h>
#define MAX 5
int main()
{
    int blocks[MAX] = {100, 500, 200, 300, 600};
    int processes[4] = {212, 417, 112, 426};
    int allocation[4], i, j;
    for (i = 0; i < 4; i++)
	    allocation[i] = -1;
    for (i = 0; i < 4; i++) 
    {
        int bestIdx = -1;
        for (j = 0; j < MAX; j++) 
	{
            if (blocks[j] >= processes[i]) 
	    {
                if (bestIdx == -1 || blocks[j] < blocks[bestIdx])
                    bestIdx = j;
            }
        }
        if (bestIdx != -1) 
	{
            allocation[i] = bestIdx;
            blocks[bestIdx] -= processes[i];
        }
    }

    for (i = 0; i < 4; i++)
    {
        printf("Process %d -> Block %d\n", i + 1, allocation[i] + 1);
    }
    return 0;
}
```
92. Develop a C program to simulate memory allocation using the worst-fit algorithm.
```
#include <stdio.h>
#define MAX 5
int main() {
    int blocks[MAX] = {100,500,200,300,600};
    int processes[4] = {212, 417, 112, 426};
    int allocation[4], i, j;

    for (i = 0; i < 4; i++) 
	    allocation[i] = -1;
     int worstIdx = -1;
    for (i = 0; i < 4; i++) {
        int worstIdx = -1;
        for (j = 0; j < MAX; j++) {
            if (blocks[j] >= processes[i]) {
                if (worstIdx == -1 || blocks[j] > blocks[worstIdx])
                    worstIdx = j;
            }
        }
        if (worstIdx != -1) {
            allocation[i] = worstIdx;
            blocks[worstIdx] -= processes[i];
        }
    }

    for (i = 0; i < 4; i++)
    {
	    printf("Process %d  " ,i+1);
	    if(allocation[i]!=-1)
		    printf("block size of %d\n",allocation[i]+1);
	    else
		    printf("not allocated\n");
    }
    return 0;
}
```
93. Implement a C program to simulate memory allocation using the next-fit algorithm.
```
#include <stdio.h>
#define MAX 5

int main() {
    int blocks[MAX] = {100, 500, 200, 300, 600};
    int processes[4] = {212, 417, 112, 426};
    int allocation[4], i, j, lastPos = 0;

    for (i = 0; i < 4; i++)
	    allocation[i] = -1;

    for (i = 0; i < 4; i++) 
    {
        j = lastPos;
        int count = 0;
        while (count < MAX)
       	{
            if (blocks[j] >= processes[i]) 
	    {
                allocation[i] = j;
                blocks[j] -= processes[i];
                lastPos = j;
                break;
            }
            j = (j + 1) % MAX;
            count++;
        }
    }
      

     for (i = 0; i < 4; i++)
    {
            printf("Process %d  " ,i+1);
            if(allocation[i]!=-1)
                    printf("block size of %d\n",allocation[i]+1);
            else
                    printf("not allocated\n");
    }
    return 0;

}
```
95. Develop a C program to implement a memory allocator using a custom memory management algorithm.
```
#include <stdio.h>
#include <stdint.h>
#define HEAP_SIZE 1024  // 1 KB heap
#define BLOCK_SIZE sizeof(Block)
typedef struct {
    uint16_t size;  // size of block
    uint8_t free;   // 1 = free, 0 = used
} Block;
char heap[HEAP_SIZE];
void init_heap() {
    Block* block = (Block*)heap;
    block->size = HEAP_SIZE - BLOCK_SIZE;
    block->free = 1;
}
void* my_malloc(uint16_t size) {
    char* ptr = heap;
    while (ptr < heap + HEAP_SIZE) {
        Block* block = (Block*)ptr;

        if (block->free && block->size >= size) {
            block->free = 0;
            return ptr + BLOCK_SIZE;
        }

        ptr += BLOCK_SIZE + block->size;
    }
    return NULL; // no space
}
void my_free(void* mem) {
    if (mem == NULL) return;

    Block* block = (Block*)((char*)mem - BLOCK_SIZE);
    block->free = 1;
}
int main() {
    init_heap();

    void* p1 = my_malloc(100);
    void* p2 = my_malloc(200);

    printf("Allocated p1: %p\n", p1);
    printf("Allocated p2: %p\n", p2);

    my_free(p1);
    printf("Freed p1\n");

    void* p3 = my_malloc(50);
    printf("Allocated p3: %p\n", p3);

    return 0;
}
```
96. Write a C program to demonstrate memory mapping using mmap().
```
#include<stdio.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <sys/stat.h>
#include <unistd.h>
#include <string.h>
int main() {
    const char *filename = "example.txt";
    int fd;
    struct stat sb;
    char *mapped;
    fd = open(filename, O_RDWR|O_CREAT,0666);
    if (fd == -1) {
        perror("open");
        return 1;
    }
    ftruncate(fd,4096);
    if (fstat(fd, &sb) == -1) {
        perror("fstat");
        close(fd);
        return 1;
    }
    mapped = mmap(NULL, sb.st_size, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (mapped == MAP_FAILED) {
        perror("mmap");
        close(fd);
        return 1;
    }
    strcpy(mapped, "Hello from mmap!");
    printf("mapped file content :%s\n",mapped);
    if (munmap(mapped, sb.st_size) == -1) {
        perror("munmap");
    }
    close(fd);
    return 0;
}
```
97. Implement a C program to read from and write to a memory-mapped file.
```
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/mman.h>
#include <string.h>
#include <sys/stat.h>
#define FILEPATH "mmap_demo.txt"
#define SIZE 4096  // Memory size (4 KB)
int main() {
    int fd;
    char *mapped;
    fd = open(FILEPATH, O_RDWR | O_CREAT, 0666);
    if (fd == -1) {
        perror("open");
        exit(1);
    }
    if (ftruncate(fd, SIZE) == -1) {
        perror("ftruncate");
        close(fd);
        exit(1);
    }
    mapped = mmap(NULL, SIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (mapped == MAP_FAILED) {
        perror("mmap");
        close(fd);
        exit(1);
    }
    const char *text = "Hello from mmap!";
    memcpy(mapped, text, strlen(text));
    printf("Data read from memory-mapped file: %s\n", mapped);
    if (munmap(mapped, SIZE) == -1) {
        perror("munmap");
    }
    close(fd);
    return 0;
}
```
98. Develop a C program to demonstrate shared memory usage using shmget() and shmat().
```
server...
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
client...
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
```
99. Write a C program to create a shared memory segment and synchronize access using semaphores. 
```
server....
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<sys/sem.h>
#include<sys/types.h>
#define SHM_KEY 25223
#define SEM_KEY 46532
void main()
{
	int shmid,semid;
	char *shmptr=NULL;
	struct sembuf smop;
	shmid=shmget(SHM_KEY ,512,IPC_CREAT|0666);
	semid=semget(SEM_KEY,2,IPC_CREAT|0666);
	shmptr=shmat(shmid,NULL,0);
	semctl(semid,0,SETVAL,0);
	semctl(semid,1,SETVAL,0);
	smop.sem_num=0;
	smop.sem_op=-1;
	smop.sem_flg=0;
	printf("waiting for message from client\n");
	semop(semid,&smop,1);
	printf("reviced %s\n",shmptr);
}
client...
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<sys/sem.h>
#include<sys/types.h>
#define SHM_KEY 25223
#define SEM_KEY 46532
void main()
{
        int shmid,semid;
        char *shmptr;
        struct sembuf smop;
        shmid=shmget(SHM_KEY ,512,0);
        semid=semget(SEM_KEY,2,0);
        shmptr=shmat(shmid,NULL,0);
        //semctl(semid,0,SETVAL,0);
        //semctl(semid,1,SETVAL,0);
        smop.sem_num=0;
        smop.sem_op=1;
        smop.sem_flg=0;
        printf("enter a message for server\n");
	scanf("%s",shmptr);
        semop(semid,&smop,1);
        printf("message to server\n");
}
```
100. Implement a C program to simulate page replacement algorithms like FIFO, LRU, and optimal.
```
#include <stdio.h>
#define SIZE 10  
#define FRAME 3  
int isInFrame(int frames[], int page) 
{
    for (int i = 0; i < FRAME; i++)
        if (frames[i] == page) 
		return 1;
    return 0;
}
int fifo(int pages[]) {
    int frames[FRAME] = {-1, -1, -1};
    int pos = 0, faults = 0;

    for (int i = 0; i < SIZE; i++) {
        if (!isInFrame(frames, pages[i])) 
	{
            frames[pos] = pages[i];
            pos = (pos + 1) % FRAME;
            faults++;
        }
    }
    return faults;
}
int lru(int pages[]) {
    int frames[FRAME] = {-1, -1, -1};
    int time[FRAME] = {0}, faults = 0;
    for (int i = 0; i < SIZE; i++)
    {
        int found = 0;
        for (int j = 0; j < FRAME; j++)
       	{
            if (frames[j] == pages[i])
	    {
                found = 1;
                time[j] = i;
                break;
            }
        }
        if (!found) 
	{
            int lru = 0;
            for (int j = 1; j < FRAME; j++)
                if (time[j] < time[lru]) lru = j;
            frames[lru] = pages[i];
            time[lru] = i;
            faults++;
        }
    }
    return faults;
}
int optimal(int pages[]) {
    int frames[FRAME] = {-1, -1, -1};
    int faults = 0;
    for (int i = 0; i < SIZE; i++)
    {
        if (isInFrame(frames, pages[i]))
	       	continue;
        int replace = -1, farthest = i;
        for (int j = 0; j < FRAME; j++) 
	{
            int k;
            for (k = i + 1; k < SIZE; k++)
                if (frames[j] == pages[k]) 
			break;
            if (k > farthest)
	    {
                farthest = k;
                replace = j;
            }
            if (frames[j] == -1)
	    {
                replace = j;
                break;
            }
        }
        frames[replace] = pages[i];
        faults++;
    }
    return faults;
}

int main() {
    int pages[SIZE] = {7, 0, 1, 2, 0, 3, 0, 4, 2, 3};
    printf("FIFO Page Faults: %d\n", fifo(pages));
    printf("LRU Page Faults: %d\n", lru(pages));
    printf("Optimal Page Faults: %d\n", optimal(pages));
    return 0;
}
```
