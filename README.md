# pwn-reverse-challenges-writeups
1. Challenge Metadata

- Title: Input Injection 1
- Category: pwn
- Platform: CyLab
- Architecture: ELF 64-bit LSB
- Difficulty: Medium

2. Executive Summary
The program is running a command before ending , the goal is smashing the stack and traying to overwriting the instruction to change it to /bin/sh

3. Initial Triage & Reconnaissance
`file` output:** ELF 64-bit LSB executable, x86-64, dynamically linked.
`checksec` output: NX enabled, SHSTK Enabled, IBT, Enabled.
NX is enabled, preventing execution of shellcode on the stack.
PIE is disabled, allowing hardcoded memory addresses to be used.

4. Static Analysis

!image.png

this is the source code of the vulnerable function

!image.png

!image.png

and this is the dangerous functions `fgets` ,`strcpy` and `system` .

5. Dynamic Analysis & Debugging

!image.png

If we look at the input location, we will see that the difference between our input and the `uname` command is only 10 bytes. This means that if we write more than 10 bytes,
we will affect the value of `uname`.

6. Exploit Construction

Define the payload architecture (e.g., `[Padding] + [/bin/sh]`).

7. Execution & Proof
exploit code :

!image.png

exploit running :
!image.png
