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

<span style="color:red">*some red italic text*</span>

<div class="warning" style='background-color:#E9D8FD; color: #69337A; border: solid #805AD5 4px; border-radius: 4px; padding:0.7em;'>
<span>
<p style='margin-top:1em; text-align:center'>
<b>On the importance of sentence length</b></p>
<p style='margin-left:1em;'>
This sentence has five words. Here are five more words. Five-word sentences are fine. But several together bocome monotonous. Listen to what is happening. The writing is getting boring. The sound of it drones. It's like a stuck record. The ear demands some variety.<br><br>
    Now listen. I vary the sentence length, and I create music. Music. The writing sings. It has a pleasent rhythm, a lilt, a harmony. I use short sentences. And I use sentences of medium length. And sometimes when I am certain the reader is rested, I will engage him with a sentence of considerable length, a sentence that burns with energy and builds with all the impetus of a crescendo, the roll of the drums, the crash of the cymbals -- sounds that say listen to this, it is important.
</p>
<p style='margin-bottom:1em; margin-right:1em; text-align:right; font-family:Georgia'> <b>- Gary Provost</b> <i>(100 Ways to Improve Your Writing, 1985)</i>
</p></span>
</div>




```nasm
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```
{: .nolineno}


```nasm
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```
{: .lineno=5}


```nasm
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```
{: .lineno:5}


```nasm
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```
{: lineno:5}



```nasm
  mov eax, dword ptr FS:[30h] ; the opcode for this is 64 A1 30 00 00 00. 
  
  
  ;It has three NULL bytes. So, to avoid this, we do the following:
  xor eax, eax ; make eax = 0
  mov eax, dword ptr FS:[eax + 30h] ; 0+30 = 30
```
{: lineno=5}


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
