# DDK_file_system
* To make a simple file system.
* I do not have a teacher; however, I still want to make a file system, ...
* So let us see how to do it.
* I also do not want to see the codes from others.
* Because we are a creator, not just using a tool or asking a question to people "do you know pthread?". 
* Actually, I would reply "I know how to park the car in the department store when having an anniversary sale, <br>
  and I also know how to lock the car door.".
* So we only need a concept about "how to store something in your life?".
* Think about how did you store your money? money with lock? ,Or just put everything in your drawers or cabinets?


## Before we start, we should learn something
* http://linux.vbird.org/linux_basic/0230filesystem.php
* First, we know that a computer do not has eyes and has to use wires or something following a sequence of steps to read or write files.
* If we do not know the address of a file, we need to find it from 0x0 to 0x..N and see whether this address stored the file we wanted.
* So we should give this file a specific address and number.
* An address matching a wire can just take a constant time "O(1)" to get that address, but we do not have such this kind of disk.
* This means we should do like a "For Loop with O(n)" to get that address.
* Otherwise, we also have a lot of files with different size to store.
* So one of our strategies is to partition every file.
* Now, we know that making a system to management files is a challenge.
* So, first, we should know memories have been partitioned into numerous boxes with same size such as 4K.
* I know we say blocks or pages.
* And a file would be partitioned into small files stored in some boxes.
* We should make a structure to record which box we stored, so the boxes would have their specificed numbers.
* Let us assume that each box is 4K and we have 1000 boxes, so the total size of memories would be 4M (1024K = 1M). 
```C
struct space{
	unsigned int box_num;
	unsigned int box_size;
	unsigned int space_size;
};

struct space memory={
	.box_num=1000,
	.box_size = 4*1024,
	.space_size = memory.box_size*memory.box_num
};
```
* Now, let us think about the first box of a file.
```C
struct box{
  unsigned int No;	
  //...
};
```
* And we also need a table to record the information of a file.
```C
struct file_table{
  //...
};
```
* We have two types to link every parts of file:
  1. box->next;
  2. file_table{  //Store all the addresses here.  };
* I prefer second type, so ...
* 
```C
struct node{
  struct box* part_file;
  struct node* next;
};

struct file_table{
  int file_name;
  int file_size;
  struct node* file;
  //...
};
```

