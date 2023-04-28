---
title: Malware development part 4 - anti static analysis tricks
layout: post
---
# Asprotect Unpacking

Asprotect has 6 options for protecting PE-file. We will unpack it using only antidebug option enabled.

## Header 2

> Protect Original EntryPoint
> Anti-Debugger protection.
> CheckSum Protection
> Resources Protection
> Preserve Extra Data
> Advanced Import protection
> Emulate standart system functions

### C Code for memory dump.

```cpp
// CPP code for memory dumping.
static main() 
{ 
	auto fp, ea; 
	fp = fopen("notepad_ASP_DBG_dump1.bin", "wb"); 
	for ( ea=0x1000000; ea < 0x105F000; ea++ ) 
		fputc(Byte(ea), fp); 
}
```


```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```







[back](./)
