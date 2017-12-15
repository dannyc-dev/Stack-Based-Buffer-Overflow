# Smashing the Stack
## Stack Based Buffer Overflow Part 1

Part 2 is building the ROP chain to control execution and invoke a shell (coming soon). 

I wanted to make a relatively quick and easy educational guide to exploiting x64 ELF executables (**using free and opensource tools**). Specifically the process I go through when reversing and testing executables. I chose to use SECCON 2017 CTF's Baby Stack challenge because it incorporated a good buffer overflow example and provided the ability to chain ROP gadgets to complete the exploit. But this tutorial is really about the process and techniques used to achieve execution control, so hopefully you can recreate it in any environment.

There have been several really good write-ups on this challenge already (specifically TeamRocketIST) but most of them required some sort of commercial software that students, like myself don't usually have the funds for. So I made this in the hopes that other students can get started without the upfront cost of commercial software.

So lets get started!

Tools used in this demo: 
  * Linux Ubuntu VM (this really doesn't matter, pick any OS you're comfortable in)
  * [pwntools](https://github.com/Gallopsled/pwntools.git) - Really useful python library for exploit dev 
  * GDB - Debugger
  * objdump - Binary Utility 

Before we get started make sure you download the CTF file [baby_stack](baby_stack-7b078c99bb96de6e5efc2b3da485a9ae8a66fd702b7139baf072ec32175076d8.dms) and make it executable

The first thing I always do when I'm testing a file is see what kind of file it is. Here's a sample of what that looks like for our test file:

![alt text](screenshot/2.png)

So now we know it's a statically linked (we can't jump into libc since they are embedded in the binary) 64-bit ELF executable and we can run the readelf command and the checksec command (if you get an error be sure to install the pwntools library above) to gain even more insight on the file. The output was:

![alt text](screenshot/4.png) ![alt text](screenshot/3.png)

Now we have the entry address (from readelf) which could come in handy in the future. And more importantly we know that stack canary is disabled (yay!) and NX is enabled which stands for non-executable segment. It means that the application, when loaded in memory, does not allow any of its segments to be both writable and executable (hence why we need ROP in Part 2, we will come back to this later).

Next let's try running it and see what it actually does:

![alt text](screenshot/1.png)

Nothing too exciting. It looks like it's just taking in two inputs and simply reformatting and printing them. So let's try inputting a longer buffer and see what happens. These were the results:

![alt text](screenshot/5.png)

So you can see we were able to overflow the buffer but if you read the stack trace we get a segmentation fault but we aren't getting it because we are replacing the return address. It's actually because we are changing the parameters of the memmove() function which changes the paramters for the print function. Another important thing to note here is that we have a go routine (GO executable). 

At this point we want to dissasemble the executable and see if we can find the scanner bufio function calls (where the program asks for user input). This is where most commercial dissasmblers come in handy to be able to quickly find these points of input but we're going to use objdump with the -S (source) flag and combine it with grep to find the things we need. This is how it should look: 


