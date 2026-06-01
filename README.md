pwn-reverse-challenges-writeups

**Note: There are many similarities to the previous version.**

1. Challenge Metadata

- Title: Input Injection 2
- Category: pwn
- Platform: CyLab
- Architecture: ELF 64-bit LSB
- Difficulty:  Medium

### 2. Executive Summary

The program is running a command before ending , the goal is smashing the heap and traying to overwriting the instruction to change it to /bin/sh 

### 3. Initial Triage & Reconnaissance

**`file` output:** ELF 64-bit LSB executable, x86-64, dynamically linked.

**`checksec` output:** NX enabled, SHSTK Enabled, IBT, Enabled.

NX is enabled, preventing execution of shellcode on the stack.

PIE is disabled, allowing hardcoded memory addresses to be used.

### **4. Static Analysis**

**source code :**

!image.png

**main functions :** 

!image.png

!image.png

!image.png

### 5. Dynamic Analysis & Debugging

!image.png

We used a 100-byte cyclic pattern to determine the command offset, then examined the variable's location in the heap to discover that the offset was 48 bytes.

### **6. Exploit Construction**

Define the payload architecture (e.g., `[Padding] + [/bin/sh]`).

!image.png

### **7. Execution & Proof**

**Proof of execution :**
