---
title: Malware development part 4 - anti static analysis tricks
layout: post
---
# Simple Reverse Shell implementation

This is one of my first C++ code projects. Working example of reverse shell. Tested on 2 VM's - Windows SP1 and Kali.

## Header 2

> I've spend hours on mking it work.
>
> The main struggle here was the WINAPI user defined datatypes aka typedef implementations: FARPOC, SOCKET etc.

### C++ Code

```cpp
// CPP code with syntax highlighting.
#include <iostream>
int main(int argc, char* argv[])
{
	std::cout<<"Hellow"<<std::endl;
	return 0;
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
