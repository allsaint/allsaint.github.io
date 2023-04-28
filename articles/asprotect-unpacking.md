---
layout: default
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

## Intro

Initial executable launch poping up a message box, notifying us that debgger is being detected:

![debugger_detected.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/debugger_detected.png)

## Initial static analysis

![3_protection_levels.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/3_protection_levels.png)

![asprotect_features.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/asprotect_features.png)

Comparison of protected and unprotected executable.

Protected:

![die_protected.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/die_protected.png)

Unprotected:

![die_unprotected.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/die_unprotected.png)

## Anti-Debugger protection bypass
Here is the instruction section referencing kernel32.dll 
`IsDebbugerPresent`
 function call.
Return from this function is checked and used for raising exception, in result program flow is stopped.

![antidebug.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/antidebug.png)


![isdebugpresent_jump.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/isdebugpresent_jump.png)

Modification of return value from 
`IsDebuggerPresent` 
function, placed in the 
`EAX` 
register.
![isdebugger_ret_mod.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/isdebugger_ret_mod.png)


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
Just after memory has been dumped, we have to fix section representation in address space. For this task PEeditor is used.

![peditor.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/peditor.png)

## IAT restoration

![iat.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/iat.png)


## Function call restoration

After we dumped memory and restored IAT, we have to fix antidumps. Some intermodular calls referrencing non existent memory spaces as shown below.

![olly_access_violation.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/olly_access_violation.png)

OllyDBG gives us hint in the form of an 
`ACCESS VIOLATION`
 exception.

![olly_access_violation_2.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/olly_access_violation_2.png)

Now to fix this we have to locate correct intermodular calls that of correct ones referrencing existing memory spaces. It can be achieved using idc script or in a manual way. I chosed a noob way. 
Below are shown 2 exact same memory spaces from dumped executable and one from an original one.  
Dumped executable:
![improper_call.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/improper_call.png)

Original executable:
![proper_call.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/proper_call.png)

Using x86dbg we can list all the impoper calls, maching predefined byte sequence. By pressing  
`Ctrl+B` 
we then enter the following byte string
`E8 ?? ?? ?? 04`
By doing this we end up with the following list.
![improper_call_list.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/improper_call_list.png)

All the restored calls are shown below.
![patched_calls.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/patched_calls.png)


Before: 

![before.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/before.png)


After:

![after.png](https://raw.githubusercontent.com/allsaint/allsaint.github.io/master/images/after.png)









[back](./)
