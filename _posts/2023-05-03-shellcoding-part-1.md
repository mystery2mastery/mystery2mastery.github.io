---
title: Shellcoding Part 1
date: 2023-05-03
categories: [ALL, Shellcoding]
tags: [shellcode]     # TAG names should always be lowercase
---

# Welcome to Shellcoding part 1

```nasm
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```

```nasm?start_inline=5
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```


```{.nasm .numberLines startFrom="10"}
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
