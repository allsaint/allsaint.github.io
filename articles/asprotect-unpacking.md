---
title: Asprotect Unpacking
layout: post
---

# Introduction
In this study, a number of tools of different orientation will be used, since there is no universal tool for solving all problems in the field of reverse engineering. We have:
IDA Pro 7.7 is a debugger for static and dynamic analysis.
x32dbg - debugger for dynamic analysis
ScyllaHide – plugin for bypassing anti-analysis
Scylla – import table reconstructor
Editor - editor of PE file sections
PEbear – analyzer of sections and information about a PE file
DetecItEasy - Software for static analysis of an executable file
Since each researcher of malicious files has its own analysis strategy, there is no single agreement on the methodology. In my research, I will start from the data known to me at the moment, so a unified methodology will be derived only towards the end of the study.
Asprotect has 6 options for protecting PE-file. We will unpack it using only antidebug option enabled.


## Key points  

*   Protect Original EntryPoint
*   Anti-Debugger protection.
*   CheckSum Protection
*   Resources Protection
*   Preserve Extra Data
*   Advanced Import protection
*   Emulate standart system functions


## Anti-Debugger protection bypass
Here is the instruction section referencing kernel32.dll 
`IsDebbugerPresent`
 function call.
Return from this function is checked and used for raising exception, in result program flow is stopped.

![antidebug.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/antidebug.png)




## Locating OEP

OEP is located by leveraging proped breakpoints on memory sections. In our case section sized in 32kb is the one containing unpacked code.



![oep.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/oep.png)





## Memory dump

Mempory dump is implemented using idc script.

![memdump.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/memdump.png)





### C Code for memory dump.
```cpp
// idc script for memory dumping.
static main() 
{ 
	auto fp, ea; 
	fp = fopen("notepad_ASP_DBG_dump1.bin", "wb"); 
	for ( ea=0x1000000; ea < 0x105F000; ea++ ) 
		fputc(Byte(ea), fp); 
}
```

## IAT restoration

![iat.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/iat.png)

## Function call restoration


Before: 
![before.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/before.png)


After:
![after.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/after.png)









[back](./)
