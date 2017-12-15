# Stack-Based-Buffer-Overflow
## Smashing the Stack - Stack Based Buffer Overflow Part 1. 

Part 2 is building the ROP chain to control execution and invoke a shell. 

I wanted to make a quick and easy guide to exploiting x64 ELF executables (using free and opensource tools). I chose to use SECCON 2017 CTF's Baby Stack challenge because it incorporated a good buffer overflow example and provided the ability to chain ROP gadgets to complete the exploit. 

There have been several really good write-ups on this challenge already (specifically TeamRocketIST) but most of them required some sort of commercial software that students, like myself don't usually have the funds for. So lets get started!


