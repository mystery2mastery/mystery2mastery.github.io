---
title: Print Error Message
date: 2024-03-25
categories: [Code Snippets, C] # You can add only 2 categories, comma separated. First is main category and Second is sub-category under the main category.
tags: [code snippet, c] # You can add any number of tags, comma separated. TAG names should always be lowercase.
---

# Print Error Message

A useful code snippet to resolve the error message corresponding to an error code and print it to the console.

## Print error message to console without color

```c
#include <windows.h>

// Helper function to print the error message (without color) to console
// PrintError("String to show. This is usually an API name")
// Ex: PrintError("GetProcessId()");
/* 
[X] GetProcessId() failed!
[-] Error code: 6
[-] Error message: The handle is invalid. 
*/

void PrintError(LPTSTR lpszFunction)
{
    DWORD errorCode = GetLastError();
	LPSTR messageBuffer = NULL;
    DWORD dwBufferLength = FormatMessageA(
        FORMAT_MESSAGE_ALLOCATE_BUFFER |
        FORMAT_MESSAGE_FROM_SYSTEM |
        FORMAT_MESSAGE_IGNORE_INSERTS,
        NULL,
        errorCode,
        MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT),
        (LPSTR)&messageBuffer,
        0,
        NULL
    );

    if (dwBufferLength) {
        printf("[X] %s failed!\n", lpszFunction);
		printf("[-] Error code: %lu\n", errorCode);
        printf("[-] Error message: %s\n", messageBuffer);

        // Free the buffer allocated by FormatMessage
        LocalFree(messageBuffer);
    }
}
```



## Print error message to console with color

```c
#include <windows.h>

// Helper function to print the error message (with color) to console
// PrintErrorColor("String to show. This is usually an API name")
// Ex: PrintErrorColor("GetProcessId()");

void PrintErrorColor(LPTSTR lpszFunction)
{    
	DWORD errorCode = GetLastError();
	LPSTR messageBuffer = NULL;
    DWORD dwBufferLength = FormatMessageA(
        FORMAT_MESSAGE_ALLOCATE_BUFFER |
        FORMAT_MESSAGE_FROM_SYSTEM |
        FORMAT_MESSAGE_IGNORE_INSERTS,
        NULL,
        errorCode,
        MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT),
        (LPSTR)&messageBuffer,
        0,
        NULL
    );
	
	//Just for showing colored output in console
	HANDLE  h_stdout;
	h_stdout = GetStdHandle(STD_OUTPUT_HANDLE);

    if (dwBufferLength) {
        SetConsoleTextAttribute(h_stdout, 4); // 4 = red
		printf("[X] %s failed!\n", lpszFunction);
		printf("[-] Error code: %lu\n", errorCode);
        printf("[-] Error message: %s\n", messageBuffer);
		SetConsoleTextAttribute(h_stdout, 7); // 7 = default = white

        // Free the buffer allocated by FormatMessage
        LocalFree(messageBuffer);
    }
}
```



## Typical usage:

```c
#include <windows.h>

void PrintErrorColor(LPTSTR lpszFunction)
{    
	DWORD errorCode = GetLastError();
	LPSTR messageBuffer = NULL;
    DWORD dwBufferLength = FormatMessageA(
        FORMAT_MESSAGE_ALLOCATE_BUFFER |
        FORMAT_MESSAGE_FROM_SYSTEM |
        FORMAT_MESSAGE_IGNORE_INSERTS,
        NULL,
        errorCode,
        MAKELANGID(LANG_NEUTRAL, SUBLANG_DEFAULT),
        (LPSTR)&messageBuffer,
        0,
        NULL
    );
	
	//Just for showing colored output in console
	HANDLE  h_stdout;
	h_stdout = GetStdHandle(STD_OUTPUT_HANDLE);

    if (dwBufferLength) {
        SetConsoleTextAttribute(h_stdout, 4); // 4 = red
		printf("[X] %s failed!\n", lpszFunction);
		printf("[-] Error code: %lu\n", errorCode);
        printf("[-] Error message: %s\n", messageBuffer);
		SetConsoleTextAttribute(h_stdout, 7); // 7 = default = white

        // Free the buffer allocated by FormatMessage
        LocalFree(messageBuffer);
    }
}

// ... your code
// Lets say you want to know if an API call was successful or not.

    if(!GetProcessId(NULL)){
		PrintErrorColor("GetProcessId()");		
	}

/* The result: 
[X] GetProcessId() failed!
[-] Error code: 6
[-] Error message: The handle is invalid. 
*/
```

