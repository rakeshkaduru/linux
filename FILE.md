//1. Write a C program to create a new text file and write "Hello, World!" to it?
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
        int fd;
        int ret;
        char buf[64]="hello,world!";
        fd=open("anji.txt",O_WRONLY|O_CREAT,0666);
        if(fd<0)
        {
                printf("failed to open the file\n");
                return ;
        }
        printf("file opened sucessfully\n");
        ret=write(fd,buf,strlen(buf));
//ret=read(fd,buf,64);
        if(ret<0)
        {
                printf("failed to read\n");
                return ;
        }
        buf[ret]=NULL;
        printf("read %d bytes from file%s\n",ret,buf);
        close(fd);
}

//2. Develop a C program to open an existing text file and display its contents?
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
        int fd;
        int ret;
        char buf[64];
        fd=open("file.txt",O_RDONLY);
        if(fd<0)
        {
                printf("failed to open the file\n");
                return ;
        }
        printf("file opened sucessfully\n");
//        ret=write(fd,buf,strlen(buf));
ret=read(fd,buf,64);
        if(ret<0)
        {
                printf("failed to read\n");
                return ;
        }
        buf[ret]=NULL;
        printf("read %d bytes from file%s\n",ret,buf);
        close(fd);
}

//3. Implement a C program to create a new directory named "Test" in the current directory?
#include<stdio.h>
#include<sys/stat.h> //mkdir
int main()
{
	if(mkdir("teste",0777)==0)
	{
		printf("directory is create sucess");
	}
	else
	{
		printf("failed to create");
	}
}
//4. Write a C program to check if a file named "sample.txt" exists in the current directory?
#include <stdio.h>
#include<unistd.h>
int main() {
    // Check file existence using access()
    if (access("sample.txt", F_OK) == 0) {
        printf("File 'sample.txt' exists in the current directory.\n");
    } else {
        printf("File 'sample.txt' does NOT exist in the current directory.\n");
    }

    return 0;
}

//5. Develop a C program to rename a file from "oldname.txt" to "newname.txt"?
#include <stdio.h>
int main() {
    if (rename("old.txt", "new.txt") == 0)
        printf("Renamed successfully.\n");
    else
        perror("Rename failed");

    return 0;
}

//6. Implement a C program to delete a file named "delete_me.txt"?

#include <stdio.h>

int main() {
    const char *filename = "delete_me.txt";

    // Attempt to delete the file
    if (remove(filename) == 0) {
        printf("File '%s' deleted successfully.\n", filename);
    } else {
        perror("Error deleting the file");
    }

    return 0;
}
//7. Write a C program to copy the contents of one file to another?
#include <stdio.h>

int main() {
    FILE *src, *dest;
    int ch;

    // Open source file in read mode
    src = fopen("source.txt", "r");
    if (src == NULL) {
        perror("Error opening source file");
        return 1;
    }

    // Open destination file in write mode
    dest = fopen("destination.txt", "w");
    if (dest == NULL) {
        perror("Error opening destination file");
        fclose(src); // close source if dest fails
        return 1;
    }

    // Copy character by character
    while ((ch = fgetc(src)) != EOF) {
        fputc(ch, dest);
    }

    printf("File copied successfully.\n");

    // Close both files
    fclose(src);
    fclose(dest);

    return 0;
}

//8. Develop a C program to move a file from one directory to another?

#include <stdio.h>
int main() {
    const char *source = "/home/rakesh/linuxsystem.c/filemanagemnt/source_dir/sample.txt";       // Original location
    const char *destination = "/home/rakesh/linuxsystem.c/filemanagemnt/target_dir/sample.txt";  // New location

    // Attempt to move (rename) the file
    if (rename(source, destination) == 0) {
        printf("File moved successfully.\n");
    } else {
        perror("Error moving file");
    }

    return 0;
}
//9. Implement a C program to list all files in the current directory?

#include <stdio.h>
#include <dirent.h>

int main() {
    struct dirent  *de;  // Pointer to directory entry
    DIR *dr = opendir("..");  // Open current directory (dot represents current)

    if (dr == NULL) {
        perror("Could not open current directory");
        return 1;
    }

    printf("Files in current directory:\n");

    // Read and print all files and directories
    while ((de = readdir(dr)) != NULL) {
        printf("%s\n", de->d_name);
    }

    closedir(dr);
    return 0;
}
//10. Write a C program to get the size of a file named "file.txt"?
#include<stdio.h>
int main()
{
	FILE *fd;
	long size;
	fd=fopen("file.txt","r");
	if( fd==NULL)
	{
		printf("unable to open file\n");
	}
	fseek(fd,0,SEEK_END);
	size=ftell(fd);
	fclose(fd);
	printf("size is %ld bytes\n,",size);
	return 0;
}

//11. Develop a C program to check if a directory named "Test" exists in the current directory? 
#include<stdio.h>
#include<sys/stat.h>
#include<sys/types.h>
int main()
{
	struct stat st;
	if(stat("test",&st)==0)
	{
		if(S_ISDIR(st.st_mode))
		{
			printf(" is directory present ");
		}
		else

		{
			printf(" is present not directory");
		}
	}
	else
	{
		printf("unable to open the file\n");
	}
}
//12. Implement a C program to create a new directory named "Backup" in the parent directory?
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
int main()
{
	const char *rakesh="../backup";
	if(mkdir(rakesh,0755)==0)
	{
		printf("directory backup is create in parent directory");
	}
	else
	{
		printf("failed to create directory");
	}
}
//13. Write a C program to recursively list all files and directories in a given directory?
#include<stdio.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<string.h>
#include <limits.h>
#include<dirent.h>
void listall(const char *path)
{
	DIR *folder;
	struct dirent *entry;
	struct stat info;
	char fullpath[PATH_MAX];
	folder=opendir(path);
	if(folder==NULL)
	{
		printf("unable open the file %s\n",path);
	}
	while((entry=readdir(folder))!=NULL)
	{
		if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
					{
					continue;
					}
	snprintf(fullpath, sizeof(fullpath),"%s/%s",path,entry->d_name);
	printf("%s\n",fullpath);
	stat(fullpath,&info);
	if(S_ISDIR(info.st_mode))
	{	
	listall(fullpath);
	}
	}
	closedir(folder);
}
int main()
{
	listall(".");
}

//14. Develop a C program to delete all files in a directory named "Temp"?
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <unistd.h>
#include <sys/stat.h>

int main() {
    const char *path = "Temp";
    DIR *folder;
    struct dirent *entry;
    char fullpath[1024];
    struct stat info;

    folder = opendir(path);
    if (folder == NULL) {
        perror("Unable to open directory");
        return 1;
    }

    while ((entry = readdir(folder)) != NULL) {
        // Skip . and ..
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Build full path
        snprintf(fullpath, sizeof(fullpath), "%s/%s", path, entry->d_name);

        // Get file status
        if (stat(fullpath, &info) == 0 && S_ISREG(info.st_mode)) {
            // If it's a regular file, delete it
            if (remove(fullpath) == 0) {
                printf("Deleted file: %s\n", path);
            } else {
                perror("Failed to delete file");
            }
        }
    }

    closedir(folder);
    return 0;
}

//15. Implement a C program to count the number of lines in a file named "data.txt"?
#include<stdio.h>
int main()
{
	FILE *fp;
	char ch;
	int lines=0;
	fp=fopen("data.txt","r");
	if(fp==NULL)
	{
		printf("unable to open the file");
		return 1;
	}
	while((ch=fgetc(fp))!=EOF)
	{
		if(ch=='\n')
		{
			lines++;
		}
	}
	fclose(fp);
	printf("total number of lines %d",lines);
	return 0;
}
//16. Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"?
#include<stdio.h>
int main()
`{
	FILE *fp;
	fp=fopen("message.txt","a");
	if(fp==NULL)
	{
		printf("unable to open the file \n");
	}
	fprintf(fp,"GOODBYE!");
	fclose(fp);
}
//17. Implement a C program to change the permissions of a file named "file.txt" to read only?
#include<stdio.h>
#include<sys/stat.h>
int main()
{
	if(chmod("filem.txt",0444)==0)
	{
		printf("sucess");
	}
	else
	{
		printf("failure");
	}
}

//18. Write a C program to change the ownership of a file named "file.txt" to the user "user1"? 
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <pwd.h>
int main() {
    const char *filename = "file.txt";
    const char *username = "user1";
    // Get user info from username
    struct passwd *pwd = getpwnam(username);
    if (pwd == NULL) {
        printf("User %s not found.\n", username);
        return 1;
    }
    uid_t uid = pwd->pw_uid;
    gid_t gid = pwd->pw_gid;
    // Change ownership of the file
    if (chown(filename, uid, gid) == 0) {
        printf("Ownership of %s changed to user %s\n", filename, username);
    } else {
        perror("Failed to change ownership");
        return 1;
    }
    return 0;
}
//19. Develop a C program to get the last modified timestamp of a file named "file.txt"?
#include<stdio.h>
#include<sys/stat.h>
#include<time.h>
int main()
{
	struct stat st;

	if(stat("3.c",&st)==0)
	{
		char *rakhi=ctime(&st.st_mtime);
	printf("last modified time is %s ",rakhi);
	}
	else
	{
		printf("error");
	}
}

//20. Implement a C program to create a temporary file and write some data to it?
#include <stdio.h>
#include <stdlib.h>
int main() {
    // Create a temporary file
    FILE *filename = tmpfile();
    if (filename == NULL) {
        printf("Failed to create temporary file.\n");
        return 1;
    }
    // Write some data to the temporary file
    fprintf(filename, "This is temporary data.\n");
    // Move to the beginning of the file to read what was written
    rewind(filename);
    char buffer[100];
    while (fgets(buffer, sizeof(buffer), filename) != NULL) {
        printf("Read from temp file: %s", buffer);
    }
    fclose(filename);
    return 0;
}
//21. Write a C program to check if a given path refers to a file or a directory?

#include<stdio.h>
#include<sys/stat.h>
int main()
{
	char path[256];
	struct stat info;
	printf("enter a path");
	scanf("%s",path);
	if(stat(path,&info)!=0)
	{
		printf("error: this can't acess\n");
		return 1;
	}

	if(S_ISREG(info.st_mode))
	{
		printf("it is regular file");
	}
	else if(S_ISDIR(info.st_mode))
	{
		printf("it is directory");
	}
	else
	{
		printf("it is neither file or directory");
	}
	return 0;

}
//22. Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"?
#include<stdio.h>
int main()
{
	if(link("hardlink.txt","source.txt")==0)
	{
		printf("it is sucess");
	}
	else
	{
		printf("failure");
}
}
//23. Implement a C program to read and display the contents of a CSV file named "data.csv"?
#include<stdio.h>
#include<stdlib.h>
int main()
{
	FILE *rak;
	int ch;
	rak=fopen("data.csv","r");
	if(rak==NULL)
	{
		printf("failed to open file");
	}
	while((ch=fgetc(rak))!=EOF)
	{
		putchar(ch);
	}
	fclose(rak);
	return 0;
}
//24. Write a C program to get the absolute path of the current working directory?
#include<stdio.h>
#include<unistd.h>
#include<limits.h>
int main()
{
	char path[PATH_MAX];
	if(getcwd(path,sizeof(path))!=NULL)
	{
		printf("current working directory%s ",path);
	}
	else
	{
		printf("not current working directry");
	}
}
//25. Develop a C program to get the size of a directory named "Documents"?
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>
#include <unistd.h>
#include <limits.h>

// Recursive function to calculate directory size
long long getDirectorySize(const char *path) {
    struct dirent *entry;
    struct stat info;
    DIR *dir;
    char fullPath[PATH_MAX];
    long long totalSize = 0;

    dir = opendir(path);
    if (dir == NULL) {
        perror("Unable to open directory");
        return 0;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip . and ..
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        snprintf(fullPath, sizeof(fullPath), "%s/%s", path, entry->d_name);

        if (stat(fullPath, &info) == 0) {
            if (S_ISDIR(info.st_mode)) {
                // Recursively add size of subdirectory
                totalSize += getDirectorySize(fullPath);
            } else {
                // Add file size
                totalSize += info.st_size;
            }
        } else {
            perror("stat failed");
        }
    }

    closedir(dir);
    return totalSize;
}

int main() {
    char currentPath[PATH_MAX];

    // Get the current working directory
    if (getcwd(currentPath, sizeof(currentPath)) == NULL) {
        perror("getcwd failed");
        return 1;
    }

    long long size = getDirectorySize(currentPath);
    printf("Total size of current directory '%s': %lld bytes\n", currentPath, size);

    return 0;
}

//26. Implement a C program to recursively copy all files and directories from one directory to another?

#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

// Copy file from src to dst
void copy_file(const char *src, const char *dst) {
    int in = open(src, O_RDONLY);
    int out = open(dst, O_WRONLY | O_CREAT | O_TRUNC, 0644);
    char buf[1024];
    int n;

    while ((n = read(in, buf, sizeof(buf))) > 0) {
        write(out, buf, n);
    }

    close(in);
    close(out);
}

// Copy directory from src to dst
void copy_dir(const char *src, const char *dst) {
    mkdir(dst, 0755);
    DIR *d = opendir(src);
    struct dirent *e;
    struct stat s;
    char src_path[256], dst_path[256];

    while ((e = readdir(d))) {
        if (strcmp(e->d_name, ".") == 0 || strcmp(e->d_name, "..") == 0)
            continue;

        snprintf(src_path, sizeof(src_path), "%s/%s", src, e->d_name);
        snprintf(dst_path, sizeof(dst_path), "%s/%s", dst, e->d_name);
        stat(src_path, &s);

        if (S_ISDIR(s.st_mode)) {
            copy_dir(src_path, dst_path);
        } else {
            copy_file(src_path, dst_path);
        }
    }

    closedir(d);
}

int main() {
    copy_dir("source_dir", "target_dir");
    printf("Copy done.\n");
    return 0;
}

//27. Write a C program to get the number of files in a directory named "Images"?
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat file_stat;
    char path[1024];
    int file_count = 0;

    // Open the "Images" directory
    dir = opendir("Images");
    if (dir == NULL) {
        perror("Unable to open directory");
        return 1;
    }

    // Iterate through directory entries
    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Build full path to entry
        snprintf(path, sizeof(path), "Images/%s", entry->d_name);

        // Get entry info
        if (stat(path, &file_stat) == 0) {
            if (S_ISREG(file_stat.st_mode)) {
                file_count++;
            }
        }
    }

    closedir(dir);

    printf("Number of files in 'Images' directory: %d\n", file_count);

    return 0;
}
//28. Develop a C program to create a FIFO (named pipe) named "myfifo" in the current directory?
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<errno.h>
int main()
{
	const char *fifoname="filemanagemnt/myfifo";
	if(mkfifo(fifoname,0666)==-1)
	{
		if(errno==EEXIST)
		{
			printf("file are exisr already %s\n",fifoname);
		}
		else
		{
			printf("not exist");
		}
	}
	else
	{
		printf("file is create newly %s\n",fifoname);
	}
}

//29. Implement a C program to read data from a FIFO named "myfifo"?
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>
#include<sys/types.h>
int main()
{
	int ch;
	FILE *fp;
	fp=fopen("myfifo","r");
	if(fp==NULL)
	{
		printf("uable to raed the file");
	}
	while((ch=fgetc(fp))!=EOF)
	{
		putchar(ch);
	}
	printf("read is sucees");
	return 0;
}
//30. Write a C program to truncate a file named "file.txt" to a specified length?
#include<stdio.h>
#include<sys/types.h>
#include<string.h>
int main()
{
	const char *filename="rakesh.txt";
	off_t newlength;
	printf("enter a length");
	if(scanf("%ld",&newlength)!=1|| newlength<0)
	{
		printf("invalid");
		return 1;
	}
	if(truncate(filename,newlength)==-1)
	{
		printf("t failed");
	}
	printf("is %s is  %ld",filename,newlength);
}
//31. Develop a C program to search for a specific string in a file named "data.txt" and display the line number(s) where it occurs
#include<stdio.h>
#include<string.h>
int main()
{
	FILE *fp;
	 char line[1024];
	char search[100];
	int line_number=0;
	int found=0;

	fp=fopen("data.txt","r");
	if(fp<0)
	{
		printf("failed to open the file\n");
	}
	printf("enter the string to search:");
	scanf("%s",search);
	while(fgets(line,sizeof(line),fp))
	{
		line_number++;
		if(strstr(line,search))
		{
			printf("found thw at line %d is %s",line_number,line);
			found=1;
		}
	}
	if(!found)
	{
		printf("string is not in file\n");
	}
	fclose(fp);
	return 0;
}
//32. Implement a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path? 
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <unistd.h>

int main() {
    char path[256];
    struct stat info;

    // Input path
    printf("Enter a file or directory path: ");
    scanf("%s", path);

    // Use lstat() to also identify symbolic links
    if (lstat(path, &info) != 0) {
        perror("Error accessing path");
        return 1;
    }

    // Check file type
    if (S_ISREG(info.st_mode)) {
        printf("It is a regular file.\n");
    } else if (S_ISDIR(info.st_mode)) {
        printf("It is a directory.\n");
    } else if (S_ISLNK(info.st_mode)) {
        printf("It is a symbolic link.\n");
    } else if (S_ISCHR(info.st_mode)) {
        printf("It is a character device.\n");
    } else if (S_ISBLK(info.st_mode)) {
        printf("It is a block device.\n");
    } else if (S_ISFIFO(info.st_mode)) {
        printf("It is a FIFO (named pipe).\n");
    } else if (S_ISSOCK(info.st_mode)) {
        printf("It is a socket.\n");
    } else {
        printf("Unknown file type.\n");
    }

    return 0;
}
//33. Write a C program to create a new empty file named "empty.txt"?

#include <stdio.h>

int main() {
    FILE *file;

    // Open the file in write mode; creates the file if it doesn't exist
    file = fopen("empty.txt", "w");

    if (file == NULL) {
        printf("Error: Could not create the file.\n");
        return 1;
    }

    printf("File 'empty.txt' created successfully.\n");

    // Close the file
    fclose(file);

    return 0;
}
//34. Develop a C program to get the permissions (mode) of a file named "file.txt"?
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;

    // Use stat to get information about the file
    if (stat("source_dir", &st) != 0) {
        perror("Error getting file information");
        return 1;
    }

    printf("Permissions for 'file.txt':\n");

    printf( (S_ISDIR(st.st_mode)) ? "d" : "-");
    printf( (st.st_mode & S_IRUSR) ? "r" : "-");
    printf( (st.st_mode & S_IWUSR) ? "w" : "-");
    printf( (st.st_mode & S_IXUSR) ? "x" : "-");
    printf( (st.st_mode & S_IRGRP) ? "r" : "-");
    printf( (st.st_mode & S_IWGRP) ? "w" : "-");
    printf( (st.st_mode & S_IXGRP) ? "x" : "-");
    printf( (st.st_mode & S_IROTH) ? "r" : "-");
    printf( (st.st_mode & S_IWOTH) ? "w" : "-");
    printf( (st.st_mode & S_IXOTH) ? "x" : "-");

    printf("\n");

    return 0;
}
//35. Implement a C program to recursively delete a directory named "Temp" and all its contents? 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>
#include <errno.h>

// Recursive function to delete a directory and its contents
int delete_directory(const char *path) {
    DIR *dir = opendir(path);
    if (dir == NULL) {
        perror("opendir");
        return -1;
    }

    struct dirent *entry;
    char full_path[1024];

    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".." entries
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Build the full path
        snprintf(full_path, sizeof(full_path), "%s/%s", path, entry->d_name);

        struct stat info;
        if (stat(full_path, &info) != 0) {
            perror("stat");
            continue;
        }

        if (S_ISDIR(info.st_mode)) {
            // It's a directory, call recursively
            delete_directory(full_path);
        } else {
            // It's a file or link, delete it
            if (remove(full_path) != 0) {
                perror("remove file");
            }
        }
    }

    closedir(dir);

    // Remove the directory itself
    if (rmdir(path) != 0) {
        perror("rmdir");
        return -1;
    }

    return 0;
}

int main() {
    const char *target = "Temp";

    if (delete_directory(target) == 0) {
        printf("Directory '%s' deleted successfully.\n", target);
    } else {
        printf("Failed to delete directory '%s'.\n", target);
    }

    return 0;
}
//36. Write a C program to read and display the first 10 lines of a file named "log.txt"?
#include <stdio.h>

int main() {
    FILE *file;
    char line[1024];
    int count = 0;

    file = fopen("log.txt", "r");
    if (file == NULL) {
        perror("Error opening file");
        return 1;
    }

    while (fgets(line, sizeof(line), file) != NULL && count < 10) {
        printf("%s", line);
        count++;
    }

    fclose(file);
    return 0;
}
//37. Develop a C program to read data from a text file named "input.txt" and write it to another file named "output.txt" in reverse order? 
#include <stdio.h>
int main() {
    FILE *input, *output;
    long file_size;
    int ch;

    // Open input file in read mode
    input = fopen("input.txt", "r");
    if (input == NULL) {
        perror("Error opening input.txt");
        return 1;
    }

    // Move to the end of the file to get its size
    fseek(input, 0, SEEK_END);
    file_size = ftell(input);

    // Open output file in write mode
    output = fopen("output.txt", "w");
    if (output == NULL) {
        perror("Error opening output.txt");
        fclose(input);
        return 1;
    }

    // Read file in reverse order and write to output
    for (long i = file_size - 1; i >= 0; i--) {
        fseek(input, i, SEEK_SET);
        ch = fgetc(input);
        fputc(ch, output);
    }

    fclose(input);
    fclose(output);

    printf("Data has been written in reverse order to output.txt\n");

    return 0;
}
//38. Implement a C program to create a new directory named with the current date in the format "YYYY-MM-DD"?

#include <stdio.h>
#include <time.h>
#include <sys/stat.h>
#include <errno.h>

int main() {
    time_t now;
    struct tm *t;
    char dirname[20];

    // Get current time
    time(&now);
    t = localtime(&now);

    // Format date as YYYY-MM-DD
    strftime(dirname, sizeof(dirname), "%Y-%m-%d", t);

    // Create the directory
    if (mkdir(dirname, 0755) == 0) {
        printf("Directory '%s' created successfully.\n", dirname);
    } else {
        perror("Failed to create directory");
    }

    return 0;
}
//39. Write a C program to read and display the contents of a binary file named "binary.bin"?
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file;
    unsigned char buffer[1024];
    size_t bytesRead;

    // Open the binary file in read-binary mode
    file = fopen("binary.bin", "rb");
    if (file == NULL) {
        perror("Error opening binary.bin");
        return 1;
    }

    printf("Contents of binary.bin (in hex):\n");

    // Read and display content in hexadecimal
    while ((bytesRead = fread(buffer, 1, sizeof(buffer), file)) > 0) {
        for (size_t i = 0; i < bytesRead; i++) {
            printf("%02X ", buffer[i]);  // Print each byte in hex
        }
    }

    printf("\n");

    fclose(file);
    return 0;
}

//40. Develop a C program to get the size of the largest file in a directory?
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat file_stat;
    char path[1024];
    long long max_size = 0;
    char largest_file[1024] = "";

    const char *dirname = ".";  // You can change this to any directory path, e.g., "Documents"

    dir = opendir(dirname);
    if (dir == NULL) {
        perror("Unable to open directory");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        snprintf(path, sizeof(path), "%s/%s", dirname, entry->d_name);

        if (stat(path, &file_stat) == 0) {
            if (S_ISREG(file_stat.st_mode)) {  // Check if it's a regular file
                if (file_stat.st_size > max_size) {
                    max_size = file_stat.st_size;
                    strcpy(largest_file, entry->d_name);
                }
            }
        }
    }

    closedir(dir);

    if (max_size > 0) {
        printf("Largest file: %s\n", largest_file);
        printf("Size: %lld bytes\n", max_size);
    } else {
        printf("No regular files found in the directory.\n");
    }

    return 0;
}

//41. Implement a C program to check if a file named "data.txt" is readable?
#include <stdio.h>
#include <unistd.h>  // For access()

int main() {
    const char *filename = "data.txt";

    // Check read permission using access()
    if (access(filename, R_OK) == 0) {
        printf("The file '%s' is readable.\n", filename);
    } else {
        perror("Read check failed");
    }

    return 0;
}
//42. Write a C program to create a new directory named "Logs" and move all files with the ".log" extension into it?
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>

int has_log_extension(const char *filename) {
    const char *ext = strrchr(filename, '.');
    return ext && strcmp(ext, ".log") == 0;
}

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat info;
    char source[512], destination[512];
    const char *target_dir = "Logs";

    // Create "Logs" directory if it doesn't exist
    if (mkdir(target_dir, 0755) != 0 && access(target_dir, F_OK) != 0) {
        perror("Failed to create Logs directory");
        return 1;
    }

    dir = opendir(".");
    if (dir == NULL) {
        perror("Unable to open current directory");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Check if it is a regular file with .log extension
        if (stat(entry->d_name, &info) == 0 && S_ISREG(info.st_mode)) {
            if (has_log_extension(entry->d_name)) {
                snprintf(source, sizeof(source), "./%s", entry->d_name);
                snprintf(destination, sizeof(destination), "./%s/%s", target_dir, entry->d_name);

                // Move the file
                if (rename(source, destination) == 0) {
                    printf("Moved: %s -> %s\n", entry->d_name, destination);
                } else {
                    perror("Failed to move file");
                }
            }
        }
    }

    closedir(dir);
    return 0;
}


//43. Develop a C program to check if a file named "config.ini" is writable?
#include <stdio.h>
#include <unistd.h>  // For access()

int main() {
    const char *filename = "config.ini";

    // Check write permission using access()
    if (access(filename, W_OK) == 0) {
        printf("The file '%s' is writable.\n", filename);
    } else {
        perror("Write check failed");
    }

    return 0;
}
//44. Implement a C program to read the contents of a text file named "instructions.txt" and execute the instructions as shell commands? 

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    FILE *file;
    char command[1024];

    // Open the instructions file
    file = fopen("instructions.txt", "r");
    if (file == NULL) {
        perror("Error opening instructions.txt");
        return 1;
    }

    // Read and execute each line as a shell command
    while (fgets(command, sizeof(command), file) != NULL) {
        // Remove trailing newline character
        command[strcspn(command, "\n")] = '\0';

        if (strlen(command) == 0) continue; // Skip empty lines

        printf("Executing: %s\n", command);
        int result = system(command);  // Execute the command

        if (result != 0) {
            fprintf(stderr, "Command failed: %s\n", command);
        }
    }

    fclose(file);
    return 0;
}

//45. Write a C program to get the number of hard links to a file named "file.txt"?
#include <stdio.h>
#include <sys/stat.h>

int main() {
    const char *filename = "file.txt";
    struct stat file_info;

    // Get file information using stat()
    if (stat(filename, &file_info) != 0) {
        perror("Error accessing file.txt");
        return 1;
    }

    // Display number of hard links
    printf("Number of hard links to '%s': %lu\n", filename, (unsigned long)file_info.st_nlink);

    return 0;
}
//46. Develop a C program to copy the contents of all text files in a directory into a single file named "combined.txt"?
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>

int has_txt_extension(const char *filename) {
    const char *ext = strrchr(filename, '.');
    return ext && strcmp(ext, ".txt") == 0;
}

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat file_stat;
    FILE *source, *dest;
    char buffer[1024];
    size_t bytes;

    const char *output_file = "combined.txt";

    // Open the output file for writing
    dest = fopen(output_file, "w");
    if (dest == NULL) {
        perror("Unable to create combined.txt");
        return 1;
    }

    dir = opendir(".");
    if (dir == NULL) {
        perror("Unable to open current directory");
        fclose(dest);
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip if not a regular file or not a .txt file
        if (stat(entry->d_name, &file_stat) == 0 && S_ISREG(file_stat.st_mode)) {
            if (has_txt_extension(entry->d_name) && strcmp(entry->d_name, output_file) != 0) {
                source = fopen(entry->d_name, "r");
                if (source == NULL) {
                    perror("Unable to open input file");
                    continue;
                }

                // Print info and copy contents
                printf("Adding file: %s\n", entry->d_name);
                fprintf(dest, "\n--- Start of %s ---\n", entry->d_name);

                while ((bytes = fread(buffer, 1, sizeof(buffer), source)) > 0) {
                    fwrite(buffer, 1, bytes, dest);
                }

                fprintf(dest, "\n--- End of %s ---\n", entry->d_name);

                fclose(source);
            }
        }
    }

    closedir(dir);
    fclose(dest);

    printf("All .txt files have been combined into '%s'.\n", output_file);
    return 0;
}
//47.Implement a C program to recursively calculate the total size of all files in a directory and its subdirectories? 
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>
#include <string.h>
#include <sys/stat.h>
#include <unistd.h>

long long get_directory_size(const char *path) {
    struct dirent *entry;
    struct stat file_stat;
    char fullpath[1024];
    DIR *dir = opendir(path);
    long long total_size = 0;

    if (dir == NULL) {
        perror("opendir");
        return 0;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        snprintf(fullpath, sizeof(fullpath), "%s/%s", path, entry->d_name);

        if (stat(fullpath, &file_stat) == 0) {
            if (S_ISDIR(file_stat.st_mode)) {
                // Recursively process subdirectory
                total_size += get_directory_size(fullpath);
            } else if (S_ISREG(file_stat.st_mode)) {
                // Add size of regular file
                total_size += file_stat.st_size;
            }
        }
    }

    closedir(dir);
    return total_size;
}

int main() {
    const char *dirname = ".";  // Current directory
    long long total_size = get_directory_size(dirname);
    printf("Total size of all files in '%s' and subdirectories: %lld bytes\n", dirname, total_size);
    return 0;
}
//48. Write a C program to get the number of bytes in a file named "data.bin"?
#include <stdio.h>

int main() {
    FILE *file;
    long size;
    file = fopen("data.bin", "rb");
    if (file == NULL) {
        perror("Error opening data.bin");
        return 1;
    }
    fseek(file, 0, SEEK_END);
    size = ftell(file);
    fclose(file);
    printf("The size of 'data.bin' is %ld bytes.\n", size);
    return 0;
}
//49. Develop a C program to create a new directory named with the current timestamp in the format "YYYY-MM-DD-HH-MM-SS"
#include <stdio.h>
#include <time.h>
#include <sys/stat.h>
#include <errno.h>

int main() {
    time_t now;
    struct tm *t;
    char dirname[32];

    // Get current time
    time(&now);
    t = localtime(&now);

    // Format timestamp as "YYYY-MM-DD-HH-MM-SS"
    strftime(dirname, sizeof(dirname), "%Y-%m-%d-%H-%M-%S", t);

    // Create the directory
    if (mkdir(dirname, 0755) == 0) {
        printf("Directory '%s' created successfully.\n", dirname);
    } else {
        perror("Failed to create directory");
    }

    return 0;
}



//50. Write a C program to create a new directory named "Documents" in the current directory?
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>

int main() {
    int result;

    // 0777 = rwxrwxrwx permissions (you can adjust this)
    result = mkdir("Documents", 0777);

    if (result == 0) {
        printf("Directory 'Documents' created successfully.\n");
    } else {
        perror("Error creating directory");
    }

    return 0;
}
//51. Develop a C program to check if a file named "file.txt" exists in the current directory?
#include <stdio.h>
#include <unistd.h>  // For access()
#include <errno.h>

int main() {
    if (access("file.txt", F_OK) == 0) {
        printf("The file 'file.txt' exists in the current directory.\n");
    } else {
        printf("The file 'file.txt' does not exist.\n");
    }

    return 0;
}
//52. Implement a C program to open a file named "data.txt" in read mode and display its contents?
#include <stdio.h>

int main() {
    FILE *fp;
    char ch;

    // Open the file in read mode
    fp = fopen("data.txt", "r");
    if (fp == NULL) {
        perror("Unable to open file");
        return 1;
    }

    printf("Contents of 'data.txt':\n");

    // Read and display character by character
    while ((ch = fgetc(fp)) != EOF) {
        putchar(ch);
    }

    // Close the file
    fclose(fp);
    return 0;
}
//53. Write a C program to create a new text file named "output.txt" and write "Hello, World!" to it? 
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
void main()
{
        int fd;
        int ret;
        char buf[64]="hello,world!";
        fd=open("output.txt",O_WRONLY|O_CREAT,0666);
        if(fd<0)
        {
                printf("failed to open the file\n");
                return ;
        }
        printf("file opened sucessfully\n");
        ret=write(fd,buf,strlen(buf));
//ret=read(fd,buf,64);
        if(ret<0)
        {
                printf("failed to read\n");
                return ;
        }
        buf[ret]=NULL;
        printf("read %d bytes from file%s\n",ret,buf);
        close(fd);
}
//54. Develop a C program to delete a file named "delete_me.txt" from the current directory?
#include<stdio.h>
int main()
{
	if(rename("delete.txt")==0)
	{
		printf("remove sucess");
		}
	else
	{
		printf("failured to remove");
	}

}
//55. Implement a C program to rename a file from "old_name.txt" to "new_name.txt"?
#include<stdio.h>
int main()
{
	if(rename("old.txt","new.txt")==0)
	{
		printf("rename is sucess");
	
	}
	else
	{
		printf("failed to rename");
	}
}

//56. Write a C program to copy the contents of one file to another file?
#include<stdio.h>
int main()
{
	FILE *src,*dest;
	int ch;
	src=fopen("source.txt","r");
	if(src==NULL)
	{
		printf("failed to read file");
		return 1;
	}
	dest=fopen("destination.txt","w");
	if(dest==NULL)
	{
		printf("failed to write file");
		fclose(src);
		return 1;
	}
	while((ch=fgetc(src))!=EOF)
	{
		fputc(ch,dest);
	}
	printf("file copied sucessfully.\n");
	fclose(src);
	fclose(dest);
	return 0;
}

//57. Develop a C program to move a file named "file.txt" to a directory named "Backup"?
#include <stdio.h>
#include <stdlib.h>

int main() {
    const char *source = "file.txt";
    const char *destination = "Backup/file.txt";

    // Move the file using rename
    if (rename(source, destination) == 0) {
        printf("File moved successfully to Backup directory.\n");
    } else {
        perror("Error moving the file");
        return 1;
    }

    return 0;
}
//58. Implement a C program to list all files and directories in the current directory?
#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>

int main() {
    DIR *dir;
    struct dirent *entry;

    // Open current directory
    dir = opendir(".");
    if (dir == NULL) {
        perror("Unable to open current directory");
        return 1;
    }

    printf("Contents of current directory:\n");

    // Read each entry
    while ((entry = readdir(dir)) != NULL) {
        printf("%s\n", entry->d_name);
    }

    // Close the directory stream
    closedir(dir);

    return 0;
}
//59. Write a C program to get the size of a file named "data.txt"?
#include<stdio.h>
int main()
{
        FILE *fp;
        long size;
        fp=fopen("file.txt","r");
        if(fp==NULL)
        {
                printf("unable to open file\n");
        }
        fseek(fp,0,SEEK_END);
        size=ftell(fp);
        printf("size of %ld bytes\n",size);
        fclose(fp);
        return 0;
}

//60. Develop a C program to create a new directory named "Pictures" in the parent directory? 
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<sys/types.h>
int main()
{
	const char *path="../pictures";
	if(mkdir(path,0777)==0)
	{
		printf("directory 'pictures created sucesddfully in parent directory.\n");
	}
	else
	{
		printf("error in creating directory");
	}
	return 0;
}
//61. Develop a C program to count the number of lines in a file named "log.txt"?
#include<stdio.h>
int main()
{
        FILE *fp;
        char ch;
        int lines=0;
        fp=fopen("data.txt","r");
        if(fp==NULL)
        {
                printf("unable to open the file");
                return 1;
        }
        while((ch=fgetc(fp))!=EOF)
        {
                if(ch=='\n')
                {
                        lines++;
                }
        }
        fclose(fp);
        printf("total number of lines %d",lines);
        return 0;
}

//62. Implement a C program to append "Goodbye!" to the end of an existing file named "message.txt"? 
#include<stdio.h>
int main()
{
	FILE *fp;
	fp=fopen("message.txt","a");
	if(fp<0)
	{
		printf("failed to append");
		return 1;
	}
	fprintf(fp,"GOODBYE!");
	fclose(fp);
	return 0;
}
//63. Write a C program to create a symbolic link named "link.txt" to a file named "target.txt"?
#include <stdio.h>
#include <unistd.h>

int main() {
    const char *target = "target.txt";
    const char *linkname = "link.txt";

    // Create symbolic link
    if (symlink(target, linkname) == 0) {
        printf("Symbolic link '%s' created successfully pointing to '%s'.\n", linkname, target);
    } else {
        perror("Error creating symbolic link");
        return 1;
    }

    return 0;
}
//64. Develop a C program to change the permissions of a file named "file.txt" to read-only?
#include<stdio.h>
int main()
{
	if(chmod("filem.txt",0444)==0)
	{
		printf(" change to reading is sucess");
	}
	else
	{
		printf("failed to read");
	}

}
//65. Implement a C program to change the ownership of a file named "file.txt" to the user "user1?
#include<stdio.h>
#include<sys/types.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<pwd.h>
int main()
{
        const char *filename="file.txt";
        const char *username="user1";
        struct passwd *pwd=getpwnam(username);
        uid_t uid=pwd->pw_uid;
        gid_t gid=pwd->pw_gid;
        if(chown(filename,uid,gid)==0)
        {
                printf("ownership is has %s changed to %s ",filename,username);
        }
        else{
                printf("not chanaged the permission");
        }
}

//66. Write a C program to get the last modified timestamp of a file named "file.txt"?
#include<stdio.h>
#include<sys/stat.h>
#include<time.h>
int main()
{
        struct stat st;

        if(stat("3.c",&st)==0)
        {
                char *rakhi=ctime(&st.st_mtime);
        printf("last modified time is %s ",rakhi);
        }
        else
        {
                printf("error");
        }
}
//67. Develop a C program to create a temporary file and write some data to it?
#include <stdio.h>
#include <stdlib.h>
int main() {
    // Create a temporary file
    FILE *filename = tmpfile();
    if (filename == NULL) {
        printf("Failed to create temporary file.\n");
        return 1;
    }
    // Write some data to the temporary file
    fprintf(filename, "This is temporary data.\n");
    // Move to the beginning of the file to read what was written
    rewind(filename);
    char buffer[100];
    while (fgets(buffer, sizeof(buffer), filename) != NULL) {
        printf("Read from temp file: %s", buffer);
    }
    fclose(filename);
    return 0;
}
//68. Implement a C program to get the size of a file named "image.jpg"?
#include <stdio.h>
#include <sys/stat.h>

int main() {
    const char *filename = "image.jpg";
    struct stat file_stat;

    // Get file information
    if (stat(filename, &file_stat) == 0) {
        printf("Size of '%s' is %lld bytes.\n", filename, (long long)file_stat.st_size);
    } else {
        perror("Error getting file size");
        return 1;
    }
}
//69. Write a C program to create a new text file named "notes.txt" and write multiple lines of text to it? 
#include <stdio.h>

int main() {
    FILE *fp;

    // Open the file for writing ("w" mode creates/overwrites)
    fp = fopen("notes.txt", "w");
    if (fp == NULL) {
        perror("Unable to create file");
        return 1;
    }

    // Write multiple lines to the file
    fprintf(fp, "Line 1: This is a note.\n");
    fprintf(fp, "Line 2: This file was created using C.\n");
    fprintf(fp, "Line 3: You can add as many lines as needed.\n");

    // Close the file
    fclose(fp);

    printf("File 'notes.txt' created and text written successfully.\n");

    return 0;
}
//70. Develop a C program to count the number of words in a file name"essay.txt"?
#include <stdio.h>
#include <ctype.h>

int main() {
    FILE *fp;
    char ch;
    int in_word = 0;
    int word_count = 0;

    fp = fopen("essay.txt", "r");
    if (fp == NULL) {
        perror("Unable to open file");
        return 1;
    }

    while ((ch = fgetc(fp)) != EOF) {
        if (isspace(ch)) {
            // We're leaving a word
            in_word = 0;
        } else if (!in_word) {
            // We found the start of a new word
            in_word = 1;
            word_count++;
        }
    }

    fclose(fp);

    printf("Number of words in 'essay.txt': %d\n", word_count);

    return 0;
}
//71. Write a C program to create a symbolic link named "shortcut.txt" to a file named "target.txt"? 
#include <unistd.h>

int main() {
    const char *target = "shortcut.txt";
    const char *linkname = "link.txt";

    // Create symbolic link
    if (symlink(target, linkname) == 0) {
        printf("Symbolic link '%s' created successfully pointing to '%s'.\n", linkname, target);
    } else {
        perror("Error creating symbolic link");
        return 1;
    }

    return 0;
}
//72. Develop a C program to change the permissions of a file named "important.doc" to read and write for the owner only? 
#include <stdio.h>
#include <sys/stat.h>

int main() {
    const char *filename = "important.doc";

    // Set permissions: read & write for owner only (0600)
    if (chmod(filename, 0600) == 0) {
        printf("Permissions changed successfully to read/write for owner only.\n");
    } else {
        perror("Error changing permissions");
        return 1;
    }

    return 0;
}
//73. Write a C program to get the last access time of a file named "data.txt"?
#include <stdio.h>
#include <sys/stat.h>
#include <time.h>

int main() {
    const char *filename = "data.txt";
    struct stat file_stat;

    // Get file status
    if (stat(filename, &file_stat) != 0) {
        perror("Error getting file information");
        return 1;
    }

    // Convert access time to human-readable format
    char *access_time = ctime(&file_stat.st_atime);
    if (access_time == NULL) {
        perror("Error converting access time");
        return 1;
    }

    printf("Last access time of '%s': %s", filename, access_time);

    return 0;
}
//74. Develop a C program to read and display the contents of a CSV file named "data.csv"?
#include<stdio.h>
#include<stdlib.h>
int main()
{
        FILE *rak;
        char hm[1024];
        rak=fopen("data.csv","r");
        if(rak==NULL)
        {
                printf("failed to open file");
        }
        while(fgets(hm,sizeof(hm),rak)!=NULL)
        {
                printf("%s",hm);
        }
        fclose(rak);
}
//75. Implement a C program to truncate a file named "file.txt" to a specified length?
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
int main()
{
        const char *filename="rakesh.txt";
        off_t newlength;
        printf("enter a length");
        if(scanf("%ld",&newlength)!=1|| newlength<0)
        {
                printf("invalid");
                return 1;
        }
        if(truncate(filename,newlength)==-1)
        {
                printf("t failed");
        }
        printf("is %s is  %ld",filename,newlength);
}

//76. Write a C program to search for a specific word in a file named "data.txt" and display the line number(s) where it occurs? 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LINE 1024

int main() {
    FILE *fp;
    char line[MAX_LINE];
    char word[100];
    int line_num = 0;
    int found = 0;

    // Open the file for reading
    fp = fopen("data.txt", "r");
    if (fp == NULL) {
        perror("Unable to open file");
        return 1;
    }

    // Get the word to search for
    printf("Enter the word to search: ");
    scanf("%s", word);
 
    // Read each line and check for the word
    while (fgets(line, sizeof(line), fp)) {
        line_num++;
        if (strstr(line, word)) {
            printf("Found on line %d: %s", line_num, line);
            found = 1;
        }
    }

    fclose(fp);

    if (!found) {
        printf("The word '%s' was not found in the file.\n", word);
    }

    return 0;
}


//77. Develop a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path? 
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <unistd.h>

int main() { 
    char path[256];
    struct stat info;

    // Input path
    printf("Enter a file or directory path: ");
    scanf("%s", path);

    // Use lstat() to also identify symbolic links
    if (lstat(path, &info) != 0) {
        perror("Error accessing path");
        return 1;
    }

    // Check file type
    if (S_ISREG(info.st_mode)) {
        printf("It is a regular file.\n");
    } else if (S_ISDIR(info.st_mode)) {
        printf("It is a directory.\n");
    } else if (S_ISLNK(info.st_mode)) {
        printf("It is a symbolic link.\n");
    } else if (S_ISCHR(info.st_mode)) {
        printf("It is a character device.\n");
    } else if (S_ISBLK(info.st_mode)) {
        printf("It is a block device.\n");
    } else if (S_ISFIFO(info.st_mode)) {
        printf("It is a FIFO (named pipe).\n");
    } else if (S_ISSOCK(info.st_mode)) {
        printf("It is a socket.\n");
    } else {
        printf("Unknown file type.\n");
    }

    return 0;
}
//78. Implement a C program to read and display the contents of a binary file named "binary.bin"? 
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *file;
    unsigned char buffer[1024];
    size_t bytesRead;

    // Open the binary file in read-binary mode
    file = fopen("binary.bin", "rb");
    if (file == NULL) {
        perror("Error opening binary.bin");
        return 1;
    }

    printf("Contents of binary.bin (in hex):\n");

    // Read and display content in hexadecimal
    while ((bytesRead = fread(buffer, 1, sizeof(buffer), file)) > 0) {
        for (size_t i = 0; i < bytesRead; i++) {
            printf("%02X ", buffer[i]);  // Print each byte in hex
        }
    }

    printf("\n");

    fclose(file);
    return 0;
}

//79. Implement a C program to create a new directory named "Logs" and move all files with the ".log" extension into it?
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>

int has_log_extension(const char *filename) {
    const char *ext = strrchr(filename, '.');
    return ext && strcmp(ext, ".log") == 0;
}

int main() {
    DIR *dir;
    struct dirent *entry;
    struct stat info;
    char source[512], destination[512];
    const char *target_dir = "Logs";

    // Create "Logs" directory if it doesn't exist
    if (mkdir(target_dir, 0755) != 0 && access(target_dir, F_OK) != 0) {
        perror("Failed to create Logs directory");
        return 1;
    }

    dir = opendir(".");
    if (dir == NULL) {
        perror("Unable to open current directory");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        // Skip "." and ".."
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0)
            continue;

        // Check if it is a regular file with .log extension
        if (stat(entry->d_name, &info) == 0 && S_ISREG(info.st_mode)) {
            if (has_log_extension(entry->d_name)) {
                snprintf(source, sizeof(source), "./%s", entry->d_name);
                snprintf(destination, sizeof(destination), "./%s/%s", target_dir, entry->d_name);

                // Move the file
                if (rename(source, destination) == 0) {
                    printf("Moved: %s -> %s\n", entry->d_name, destination);
                } else {
                    perror("Failed to move file");
                }
            }
        }
    }

    closedir(dir);
    return 0;
}
//80. Write a C program to check if a file named "config.ini" is writble?
#include <stdio.h>
#include <unistd.h>  // For access()

int main() {
    const char *filename = "config.ini";

    // Check write permission using access()
    if (access(filename, W_OK) == 0) {
        printf("The file '%s' is writable.\n", filename);
    } else {
        perror("Write check failed");
    }

    return 0;
}
//81. Develop a C program to read the contents of a text file named "instructions.txt" and execute the instructions as shell commands?
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    FILE *file;
    char command[1024];

    // Open the instructions file
    file = fopen("instructions.txt", "r");
    if (file == NULL) {
        perror("Error opening instructions.txt");
        return 1;
    }

    // Read and execute each line as a shell command
    while (fgets(command, sizeof(command), file) != NULL) {
        // Remove trailing newline character
        command[strcspn(command, "\n")] = '\0';

        if (strlen(command) == 0) continue; // Skip empty lines

        printf("Executing: %s\n", command);
        int result = system(command);  // Execute the command

        if (result != 0) {
            fprintf(stderr, "Command failed: %s\n", command);
        }
    }

    fclose(file);
    return 0;
}
//82. Develop a C program to read data from a binary file named "data.bin" and display it in hexadecimal format?
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp;
    unsigned char buffer[16];  // Read in chunks
    size_t bytesRead;
    int offset = 0;

    fp = fopen("data.bin", "rb");
    if (fp == NULL) {
        perror("Unable to open file");
        return 1;
    }

    printf("Hex dump of 'data.bin':\n");

    // Read and print in hexadecimal format
    while ((bytesRead = fread(buffer, 1, sizeof(buffer), fp)) > 0) {
        printf("%08X  ", offset);  // Print offset address
        for (size_t i = 0; i < bytesRead; i++) {
            printf("%02X ", buffer[i]);
        }

        // Fill remaining space if less than 16 bytes
        for (size_t i = bytesRead; i < 16; i++) {
            printf("   ");
        }

        // Print ASCII representation (optional)
        printf(" |");
        for (size_t i = 0; i < bytesRead; i++) {
            if (buffer[i] >= 32 && buffer[i] <= 126)
                printf("%c", buffer[i]);
            else
                printf(".");
        }
        printf("|\n");

        offset += bytesRead;
    }

    fclose(fp);
    return 0;
}
//83. Develop a C program to recursively copy all files and directories from one directory to another? 
#include <stdio.h>
#include <dirent.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

// Copy file from src to dst
void copy_file(const char *src, const char *dst) {
    int in = open(src, O_RDONLY);
    int out = open(dst, O_WRONLY | O_CREAT | O_TRUNC, 0644);
    char buf[1024];
    int n;

    while ((n = read(in, buf, sizeof(buf))) > 0) {
        write(out, buf, n);
    }

    close(in);
    close(out);
}

// Copy directory from src to dst
void copy_dir(const char *src, const char *dst) {
    mkdir(dst, 0755);
    DIR *d = opendir(src);
    struct dirent *e;
    struct stat s;
    char src_path[256], dst_path[256];

    while ((e = readdir(d))) {
        if (strcmp(e->d_name, ".") == 0 || strcmp(e->d_name, "..") == 0)
            continue;

        snprintf(src_path, sizeof(src_path), "%s/%s", src, e->d_name);
        snprintf(dst_path, sizeof(dst_path), "%s/%s", dst, e->d_name);
        stat(src_path, &s);

        if (S_ISDIR(s.st_mode)) {
            copy_dir(src_path, dst_path);
        } else {
            copy_file(src_path, dst_path);
        }
    }

    closedir(d);
}

int main() {
    copy_dir("source_dir", "target_dir");
    printf("Copy done.\n");
    return 0;
}
//84. Develop a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path? 
#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <unistd.h>

int main() {
    char path[256];
    struct stat info;

    // Input path
    printf("Enter a file or directory path: ");
    scanf("%s", path);

    // Use lstat() to also identify symbolic links
    if (lstat(path, &info) != 0) {
        perror("Error accessing path");
        return 1;
    }

    // Check file type
    if (S_ISREG(info.st_mode)) {
        printf("It is a regular file.\n");
    } else if (S_ISDIR(info.st_mode)) {
        printf("It is a directory.\n");
    } else if (S_ISLNK(info.st_mode)) {
        printf("It is a symbolic link.\n");
    } else if (S_ISCHR(info.st_mode)) {
        printf("It is a character device.\n");
    } else if (S_ISBLK(info.st_mode)) {
        printf("It is a block device.\n");
    } else if (S_ISFIFO(info.st_mode)) {
        printf("It is a FIFO (named pipe).\n");
    } else if (S_ISSOCK(info.st_mode)) {
        printf("It is a socket.\n");
    } else {
        printf("Unknown file type.\n");
    }

    return 0;
}
 
