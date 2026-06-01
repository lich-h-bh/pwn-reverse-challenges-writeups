# pwn-reverse-challenges-writeups

**Note: There are many similarities to the previous version.**

### 1. Challenge Metadata

- **Title:** **Input Injection 2**
- **Category:** pwn
- **Platform:** CyLab
- **Architecture:** ELF 64-bit LSB
- **Difficulty:** Medium

### 2. Executive Summary

The program is running a command before ending , the goal is smashing the heap and traying to overwriting the instruction to change it to /bin/sh 

### 3. Initial Triage & Reconnaissance

**`file` output:** ELF 64-bit LSB executable, x86-64, dynamically linked.

**`checksec` output:** NX enabled, SHSTK Enabled, IBT, Enabled.

NX is enabled, preventing execution of shellcode on the stack.

PIE is disabled, allowing hardcoded memory addresses to be used.

### **4. Static Analysis**

**source code :**

<img width="443" height="348" alt="image" src="https://github.com/user-attachments/assets/d78dfd14-c2aa-4cbe-9316-8c74658b5d11" />

**main functions :** 

<img width="488" height="81" alt="image" src="https://github.com/user-attachments/assets/cf4da2e6-487b-48ed-9a19-8c753feea795" />

<img width="569" height="81" alt="image" src="https://github.com/user-attachments/assets/4b558451-6843-4f2d-96ad-7e4b66320f78" />

<img width="506" height="51" alt="image" src="https://github.com/user-attachments/assets/827b2397-a0d1-49c6-9378-8036218a81af" />

### 5. Dynamic Analysis & Debugging

<img width="589" height="152" alt="image" src="https://github.com/user-attachments/assets/69aed4e1-c6d8-47ae-8108-d2eee769b86e" />

We used a 100-byte cyclic pattern to determine the command offset, then examined the variable's location in the heap to discover that the offset was 48 bytes.

### **6. Exploit Construction**

Define the payload architecture (e.g., `[Padding] + [/bin/sh]`).

<img width="967" height="153" alt="image" src="https://github.com/user-attachments/assets/0aae3935-e4a0-4fe6-8b54-a17d0bb85fb3" />

### **7. Execution & Proof**

**Proof of execution :**

<img width="619" height="252" alt="image" src="https://github.com/user-attachments/assets/ae9cbbb3-9119-4ba5-a0a4-4f8e2c70f262" />
