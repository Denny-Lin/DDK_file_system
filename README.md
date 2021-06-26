# DDK_file_system
* To make a simple file system.
* I do not have a teacher; however, I still want to make a file system, so let us see how to do it.
* I do not want to see the codes from others, because we are a creator, not jsut using a tool or asking a question to people "do you know pthread?". 
* So we only need a concept about how to store something in your life.
* Think about how did you store your money? money with lock? ,Or just put everything in your drawers or cabinets?


## Before we start, we shold learn something
* http://linux.vbird.org/linux_basic/0230filesystem.php
* First, we need to know that a computer do not has eyes and has to use wires or something following a sequence of steps to read or write files.
* If we do not know the address of a file, we need to find it from 0x0 to 0x..N and see whether this address stored the file we wanted.
* So we should give this file a specific address and number.
* An address matching a wire can have just take a constant time "O(1)" to get that address, but we do not have such this kind of disk.
* This means we should do like a "For Loop" to get that address.
* Otherwise, we also have a lot of files with different size to store.
* So one of our strategies is to partition every file.
* Now, we know that making a system to management files is a challenge.
* So, first, we should know memories have been partitioned into numerous boxes with same size such as 4K.
* I know we say blocks or pages.
* And a file would be partitioned into small files stored in some boxes.
* We should make a structure to record which box we stored, so the boxes would have their specificed numbers.
* ...
