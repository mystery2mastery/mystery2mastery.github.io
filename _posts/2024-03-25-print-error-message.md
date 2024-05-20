---
title: Print Error Message
date: 2024-03-25
categories: [Code Snippets, C] # You can add only 2 categories, comma separated. First is main category and Second is sub-category under the main category.
tags: [code snippet, c] # You can add any number of tags, comma separated. TAG names should always be lowercase. # To add assets to this post in future, create a folder named assets\blogpost_assets\2024-03-25-print-error-message\ and drop your assets in there.
---

# Print Error Message

A useful code snippet to resolve the error message corresponding to an error code for Windows API. I wrote 3 functions namely:

1. PrintError(): Prints the error message to console.
2. PrintErrorColor(): Prints the error message to console with color. Uses red by default.
3. ShowErrorMsg(): Spawns a message box to show the error message. Useful for GUI programs.



## Example usage:

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



For the full code or if you want to use them in a larger project, I have created the corresponding .c and .h file for each of these functions. So, you can easily use them in your projects.

Link to GitHub repo: [PrintError](https://github.com/mystery2mastery/PrintError)
