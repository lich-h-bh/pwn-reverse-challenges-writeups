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

/

<img width="510" height="83" alt="image" src="https://github.com/user-attachments/assets/37d64871-267d-4572-8258-f33ee4ec8015" />

this is the source code of the vulnerable function.


<img width="480" height="50" alt="image" src="https://github.com/user-attachments/assets/c17f9a73-aad1-4bec-a268-0514622e8bf8" />


and this is the dangerous functions `fgets` ,`strcpy` and `system` .

5. Dynamic Analysis & Debugging

<img width="636" height="359" alt="image" src="https://github.com/user-attachments/assets/99ad5bae-1a39-4cd2-9b77-effc84349b1e" />

If we look at the input location, we will see that the difference between our input and the `uname` command is only 10 bytes. This means that if we write more than 10 bytes,
we will affect the value of `uname`.

6. Exploit Construction

Define the payload architecture (e.g., `[Padding] + [/bin/sh]`).

<img width="619" height="214" alt="image" src="https://github.com/user-attachments/assets/ff0266f2-188a-4551-93c9-30330a997aa4" />

7. Execution & Proof
exploit execution :

<img width="631" height="264" alt="image" src="https://github.com/user-attachments/assets/4ede71fe-24a9-4181-a4b1-8ea409ccb5f7" />

