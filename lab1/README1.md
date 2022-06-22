# Ван Чжзнхао (P3304C)  Вариант 1
# Лабораторная работа 1

**Название:Ван Чжзнхао** "Разработка драйверов символьных устройств"

**Цель работы:** Write a character device driver that meets the requirements

## Описание функциональности драйвера

When writing text to the character device /dev/var1 file, count the total number of characters, and then write the result to the /proc/var1 file, and the content read from the kernel buffer is consistent with the content of the /proc/var1 file

## Инструкция по сборке

1. Enter the source program directory
2. execute make
3. execute insmod var1.ko
4. View device number: 
	cat /proc/devices | grep var1  ===> 235 var1
5. Create a device node in the /dev directory according to the device number
    cd /dev
    mknod var1 c 235 0
    ls | grep var1  ===> var1

## Инструкция пользователя

1. write test program
2. Read and write the /dev/var1 device driver file in the test program.
3. gcc test.c; ./a.out

## Примеры использования

test file in the code directory:

#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
int main(int argc, char *argv[])
{
    char buf[4096] = {0};
    char line[512] = {0};
    fgets(line, 512, stdin);    
    line[strlen(line)-1] = '\0';
	FILE* fp = fopen("/dev/var1", "r+");
	if(fp == NULL)
    {
		printf("open var1 failed!!!\n");
		return -1;
	}
	
    fwrite(line, strlen(line), 1, fp);
	fseek(fp,0,SEEK_SET);
    fread(buf, sizeof(buf), 1, fp);
    printf("%s\n", buf);
    fclose(fp);
	return 0;
}
