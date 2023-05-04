---
title: Shellcoding Part 1
date: 2023-05-03
categories: [ALL, Shellcoding]
tags: [shellcode]     # TAG names should always be lowercase
---

# Welcome to Shellcoding part 1

**Windows Shellcode Development**


## Art of Shellcoding: 5. calc.exe universal shellcode

- Our approach to calc.exe shellcode:
- Actual implementation of code

#### Our approach to calc.exe shellcode:

1. Get the base address of kernel32.dll
2.  For running calc.exe we will make use of 'WinExec' API from kernel32.dll and we will close our shellcode with 'ExitProcess' API also from kernel32.dll. For obtaining the addresses of these APIs we will implement a hashing function and use it to find the addresses of these APIs. Store the addresses of  resolved APIs on the stack for future use.
3.  Find the base address of other dlls and resolve functions from those dlls.
4.  Write the code to achieve what we want with our shellcode.
***

1. Get the base address of kernel32.dll:
There are many methods to achieve this. But I will use the method discussed below for obtaining kernel32.dll which is very reliable. Whenever a new thread is created within a process, a new TEB data strucure containing information about that thread is also created within the process address space. It can be accessed by FS register. FS:[0] points to the beginning of the TEB structure. Note: In case of 64bit executables, GS:[0] points to the beginning of the TEB structure.

Look at this code snippet:
<span style="color:blue">some *blue* text</span>.

```assembly_x86
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```


```{.assembly_x86 .numberLines startFrom="10"}
//nasm
mov eax, 0
mov ecx, eax
xor ebx, ebx
```


:::note
asdsadadsadsadad
:::

:::tip
asdsadasd
:::

```x86asm
find_kernel32_base_address_by_InMemoryOrder: ;Finding kernel32.dll base address (works in all versions of Windows > NT)
  	xor ebx, ebx ; trick to avoid null byte in the below instruction. Also don't use eax register. Why? continue reading to find the answer.
	mov ebx, dword ptr FS:[ebx+30h] ; At FS[30h] is the pointer to PEB structure.
	mov ebx, dword ptr [ebx + 0Ch] ; Pointer to PEB_LDR_DATA Structure
	mov ebx, dword ptr [ebx + 14h] ; Pointer to 1st entry in InMemoryOrderList => current.exe's info.
	mov ebx, dword ptr [ebx] ; If you use eax register, then "mov eax, dword ptr [eax]" will have a nullbyte! The opcode for this instruction with eax is 8B00, with ebx it is 8B1B, with ecx it is 8B09, with edx it is 8B12. So, you can use any register except eax. My favourite is ebx! Pointer to 2nd entry => ntdll.dll's info
	mov ebx, dword ptr [ebx] ; Pointer to 3rd entry => kernel32.dll 's info (we land at 0x08 within the kernel32.dll's info.
	mov ebx, dword ptr [ebx + 10h] ; At 0x18 i.e., 0x10 from the above address is the 'DllBase' address i.e., kernel32.dll's base address
	;Finally ebx contains the base address of kernel32.dll
```

This is the value at `mov eax, 0x00` adsdsadsadas adsa

- [x] Task 1
- [ ] Task 2
- [ ] Task3
- [x] Task4

[dsfdsfdssdfdsffsdf]



gfhdhdhghgh
#### sadsadsad

[](url)
