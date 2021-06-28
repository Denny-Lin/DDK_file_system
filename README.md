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
  char data[4*1024];
  //...
};
```
* And we also need a table to record the information of a file.
* This table was also stored in a box with size less than 4K.
```C
struct file_table{
  //...
};
```
* We have two types to link every parts of file:
  1. box->next;
  2. file_table{  //Store all the addresses here.  };
* I prefer second type, so ...
* We use linked list to connect each part of file.
```C
struct node{
  struct box* part_file;
  struct node* next;
};

struct file_table{
  int file_name;
  unsigned int file_size;
  struct node* file;  //32bits 4bytes, 64bits 8bytes
  //...
};
```

## Now, what will we do? 
* We will create a file, called "space" used open(rw);
* The size of this file would be 4K * 1000, and we will partition it.
* finally, we will read a file and store in this "sapce", and read data from this "space" to merge into a original file.

## Function of Format() 
* You are right. This behavior we called it "format".
* We do it in user space, but this file will be stored in the real disk.
* I am thinking of the strategy of "bitmap".
* If we formated the space and next time we use this file, we just read the space informations in the first box.
* The first address is dynamic allocated, becuase we use ram.
* So, the first box will also store all the addresses or an offset of the first address for all the first address of boxes. 
* It means we will have an initial function to do something.
* ...
```C
if "Space.ddkfs" do not exist then
	struct space memory={.....}
	create space.ddkfs 
	//open, 
	//partition,
	//save info in the first box,
	//close

open Space.ddkfs

"initial" all the address of boxes
...
...
save "initial_data" in the first box

read_function()
write_function()
```
* This is one type we could used, but it is not a good idea of "Module".
* Because we have a problem of dynamic address of the Space.ddkfs allocated in the ram.

## Can we do another method?
* "Space.ddkfs" just be a file.
* Actually we do not need to care about the address, we could just use OPEN and SEEK "Space.ddkfs" in binary mode. 
* Linux: dd if=/dev/zero of=/tmp/Space.ddkfs bs=4K count=1000
* We could do something like this "dd" command.
* BY THE WAY ##I just think maybe I could create my personal SQL_file_system ##
## "I will not use this eazy way!!!!"

## We will use initial_data() to avoid the problem of address.
* Since we want to build a real file system, we should have address.
* Our "memory management unit" need to know the phiscal address.
* So our initial_data() will find all the dynamic address of the boxes and store them.
* This dynamic address means the phiscal address.
* It looks like we have phiscal address. <br>
![image](https://user-images.githubusercontent.com/67073582/123671180-9274d880-d870-11eb-91e5-a7dcec2cc021.png) <br>
* Someday, if this application can format a disk using our method by OS driver, we can also get the information of phiscal address stored in one of blocks.
* ...
