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

<img width="295" height="225" alt="image" src="https://github.com/user-attachments/assets/2395e1a3-03ba-4e67-8237-ec69fd1d8c26" />
ㅤ⠀

<img width="510" height="83" alt="image" src="https://github.com/user-attachments/assets/37d64871-267d-4572-8258-f33ee4ec8015" />
ㅤ⠀
this is the source code of the vulnerable function.
ㅤ⠀

<img width="480" height="50" alt="image" src="https://github.com/user-attachments/assets/c17f9a73-aad1-4bec-a268-0514622e8bf8" />
ㅤ⠀

and this is the dangerous functions `fgets` ,`strcpy` and `system` .

5. Dynamic Analysis & Debugging

<img width="636" height="359" alt="image" src="https://github.com/user-attachments/assets/99ad5bae-1a39-4cd2-9b77-effc84349b1e" />

If we look at the input location, we will see that the difference between our input and the `uname` command is only 10 bytes. This means that if we write more than 10 bytes,
we will affect the value of `uname`.

6. Exploit Construction

Define the payload architecture (e.g., `[Padding] + [/bin/sh]`).

7. Execution & Proof
exploit code :

<img width="545" height="152" alt="image" src="https://github.com/user-attachments/assets/f3f9137e-4f01-4740-baff-bf2d1a77f0e9" />

exploit running :

<img width="631" height="264" alt="image" src="https://github.com/user-attachments/assets/f614732e-50d2-42d3-a85f-e3aa451071b8" />
