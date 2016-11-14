---
layout: post
title:  "Calling a function from an injected dll"
date:   2016-02-29 20:00:00
categories: hacking memory
---

I've got this very basic c++ application.

{% highlight c++ %}
#include <Windows.h>
#include <iostream>

double myDoubleFunction(double arg)
{
	std::cout << "MYDOUBLEFUNCTION " << arg << std::endl;
	return arg;
}

int main()
{
	for (;;)
	{
		myDoubleFunction(3.14);
		Sleep(2500);
	}

	return 0;
}
{% endhighlight %}


In IDA, we can find the function using the hardcoded string:

![alt text](/public/images/2016-02-29-calling-function-from-injected-dll_01.png "IDA Strings Screenshot")

Using XREF's we can easily find the caller, it's our function located at 0x412450. *See the == S U B R O U T I N E ==* 

{% highlight text %}
.text:0041242F ; ---------------------------------------------------------------------------
.text:00412430                 db 20h dup(0CCh)
.text:00412450
.text:00412450 ; =============== S U B R O U T I N E =======================================
....
.text:00412484                 push    offset Str      ; "MYDOUBLEFUNCTION "
....
.text:004124C8                 mov     esp, ebp
.text:004124CA                 pop     ebp
.text:004124CB                 retn
.text:004124CB sub_412450      endp
.text:004124CB
.text:004124CB ; ---------------------------------------------------------------------------
.text:004124CC                 db 24h dup(0CCh)
.text:004124F0
{% endhighlight %}

