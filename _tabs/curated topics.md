---
# the default layout is 'page'
icon: fas fa-book
order: 5

---

This is a curated list of posts arranged in tutorial format.

# <span style="color:lightgreen">The Art of Windows:</span>

## Shellcoding:

0. <details><summary>Prerequisites / Basic Concepts:</summary> Before diving into the world of shellcoding, one is expected to know a few concepts. If you are unsure, please go through this absolutely essential basic concepts section.</details>
1. What is a shellcode?
2. Why should I learn shellcoding?
3. What do I need to learn shellcoding?
4. Enter the Dargon: How do you write a universal shellcode for Windows OS?
5. calc.exe universal shellcode
6. Much more useful reverse_tcp universal shellcode
7. Tips and Tricks to avoid null bytes

<details>
<summary>Section A.B</summary>
<details>
<summary>Section A.B.C</summary>
<details>
<summary>Section A.B.C.D</summary>
  Done!
</details>
</details>
</details>



## Exploit Development:

0. Terminology associated with Exploit Development
1. Prerequisites for Windows Exploit Development
2. Exploit Mitigations for Windows OS.
3. Stack Exploitation:
   0. Stack Buffer Overflow: Theory
   1. Stack Buffer Overflow: Direct-RET/EIP overwrite.
      SBO-DR/SEH + ASLR => Partial EIP overwrite or Memory Address Leak or anything that reveals the base address of kernel32.dll or kernelbase.dll or any other API within them etc.
   2. Stack Buffer Overflow: SEH overwrite.
      SBO-DR/SEH + ASLR => Partial EIP overwrite or Memory Address Leak or anything that reveals the base address of kernel32.dll or kernelbase.dll or any other API (offset) within them etc.
   3. Stack Buffer Overflow: Direct-RET/EIP overwrite with 'DEP' -  Bypass by ROP
      SBO-DR + DEP + ASLR => get memory address somehow and then apply ROP OR heap spray and jump to it by ROP.
   4. Stack Buffer Overflow: SEH overwrite with 'DEP' - Bypass by ROP
      SBO-SEH + DEP + ASLR => get memory address somehow and then apply 'stack-pivot + ROP' OR heap spray and jump to it by ROP.

Heap spray: Heap spray != Heap exploitation. Also, it has nothing to do with stack exploitation. Heap spray is a payload delivery technique. Heap spray is generally used when there is not enough space to deliver a payload via the usual route (i.e., stack). In this technique, we spray the program's heap with our payload until it is allocated at our predicted address.Then we jump to that address and execute our payload.

4. Heap Exploitation: Heap Exploitation is not as popular as stack exploitation for the simple reason that we don't gain anything immediately by exploiting a buffer on heap. A stack buffer overflow may lead to control over EIP but we don't gain control over EIP by overflowing a buffer on heap. Also, stack operations are pretty much the same for all kinds of programs whereas the heap operations may vary from program to program. Based on the size of memory allocations and the frequency of allocations, the heap may be handled by different heap managers within the OS. Also, the program itself might handle the heap allocations with its own proprietary code. So, most of the time, heap corruption vulnerabilities are not generic and are very specific to the application. It needs a lot of study to understand how the application handles those heap allocation requests in order to exploit it. If I get more free time, I will try to make some posts on heap exploitation. As of now I won't be making any posts on this topic.


## Malware Analysis:

0. Malware Analysis Lab Setup
1. Basic Malware Analysis Methodology
2. Analyzing PE Malware: 
   1. Anti-analysis techniques and how to defeat them (v.imp section) - I will be adding more and more techniques. So, this section will always be evolving.
   2. Art of Unpacking:
      1. Introduction
      2. How to detect packers or packing?
      3. Generic (manual) unpacking tricks and tips
      4. Packers :
         1. Manually unpacking some popular compressors:
            1. UPX
            2. ASPack
            3. PECompact
            4. NSPack
            5. PEDiminisher
            6. MEW
            7. FSG 1.3
            8. FSG2.0 etc.

         2. Manually unpacking some popular protectors: This section requires advanced knowledge on how code virtualization happens in virtual machines. The earlier versions of these protectors were slightly straightforward and easy to devirtualize but the latest versions include a lot of anti-debugging and anti-analysis tricks embedded in them along with the code virtualization. Also, based on the level of protection measures and code virtualization in the program, we may not be able to unpack 100% of the program. So, I won't be making any posts on this topic for now. As of now, even I don't know how to explain these in a easy to understand manner, because they keep changing with versions and builds. Unpacking these protectors is so time consuming that most of the manual unpacking is also done with scripts.
            1. Software Passport - Armadillo (discontinued)
            2. Oreans - Themida
            3. Oreans - WinLicense
            4. Oreans - Code Virtualizer
            5. Obsidium
            6. VMProtect
            7. The Enigma Protector etc.

      5. Understanding some common code snippets:
         1. Malware API patterns
         2. Process Injection techniques

3. Analyzing malicious Office documents:

4. Analyzing malicious PDF documents:


5. Analyzing malicious scripts:

    1. Powershell scripts
    2. AutoIt scripts
    3. VBA scripts etc.
    


## Windows Internals:




# Linux:
