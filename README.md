# Smashing the Stack
## Stack Based Buffer Overflow Part 1

Part 2 is building the ROP chain to control execution and invoke a shell. 

I wanted to make a quick and easy educational guide to exploiting x64 ELF executables (**using free and opensource tools**). Specifically the process I go through when reversing and testing executables. I chose to use SECCON 2017 CTF's Baby Stack challenge because it incorporated a good buffer overflow example and provided the ability to chain ROP gadgets to complete the exploit (demo coming soon). 

There have been several really good write-ups on this challenge already (specifically TeamRocketIST) but most of them required some sort of commercial software that students, like myself don't usually have the funds for. So lets get started!

Tools used in this demo: 
  Linux Ubuntu VM (this really doesn't matter, pick an OS you're comfortable in)
  * [pwntools](https://github.com/Gallopsled/pwntools.git) - Really useful python library for exploit dev 
  * GDB - Debugger
  * objdump - Binary Utility 

Before we get started make sure you download the CTF file - 
The first thing I always do when I'm testing a CTF file is run it and see what it does.
