## SS5 windows移植
### SS5 代理服务器
[SS5](http://ss5.sourceforge.net) 是一款linux 下的代理工具，支持 `SOCKS V4` 和 `SOCKS V5`。[官网](http://ss5.sourceforge.net) 介绍说，它支持telnet、ftp、finger、 whois、gohper和 WWW。
<!-- more -->
### Windows 下的纠结
这么好的一个开源代理软件只有linux版，并没有windows版，可能出于某种方面的考虑吧。在面对windows版的需求，我想到第一个就是拿它放到 cygwin 下去编译，发现总是编译不过去，SSL库不支持。对于只要其代理功能的我，看了一遍它的代码，发现其用很多宏，再往深的看发现对于核心代理功能模块并没有特殊的代码，决定自己来移植吧。

### Windows 下的移植
对于要移植的代码，最好的就是大面积复用以前的代码，保证功能代码不变。对于此，首先我们就要自己写个入口，作为服务， 你当然也得写成 windows 服务的模式。
``` c
#if SERVICEMODE
void main()
{
	SERVICE_TABLE_ENTRY ServiceTable[2];
	ServiceTable[0].lpServiceName = "Socks Server 5";
	ServiceTable[0].lpServiceProc = (LPSERVICE_MAIN_FUNCTION)ServiceMain;

	ServiceTable[1].lpServiceName = NULL;
	ServiceTable[1].lpServiceProc = NULL;

	StartServiceCtrlDispatcher(ServiceTable);
}
void ServiceMain(int argc, char** argv)
#else
int main(int argc, char **argv, char **envp)
#endif
```
这里我用一个宏来区分服务模式和调试模式，当然这里最好是用参数来代替，但当初时间紧就没加，不过思路是一样的。对于 windows 的service 程序必须得加些 Service要的东西。

下面就是socket 相关的，但这个不分平台的，只是包含的头文件路径不同而已。再往下就遇到了`fork`，这个linux 下特色函数，在windows 下也就只有线程来模拟。涉及到线程，如果使用 windows 下自带的线程，那移植的代码量就太多了。网上搜了下，发现 `pthread`有 windows 版，果断下下来就`pthread`，自己平时都是写 linux 下的程序，所以pthread 用起来还是蛮亲切的。

再添加核心功能代码，这个也是特别烦，就那么几个文件，看其代码量惊人，但因本人只需要其 Socks v5 的部分，剔掉好多代码，就会发现其代码量并不多。

#### 核心代码理解
对于实现 socks协议的部分我就不多说了，这个可以直接看文档，我只说说ss5代理 是怎么实现的。主要部分一共三个 socket，一个socket 负责发 socks 协议指令的，一个负责与客户端传输数据，最后一个负责远端请求的。`SS5ProxyData`则是数据交换的中心，剩下的好像也没什么难的了，如果有不理解的地方，请发评论，我会及时解答。

### 发布
写好程序要发布，这里我使用的是`Advanced Installer`，把安装软件配置成服务类的程序，再简单的给安装包添加些信息。好了，一个  windows 版 ss5 就出炉了。

### 结束语
因为代码涉及些特殊业务，就不公布太多了，这里主要谈下思路。写代码最重要的是思路，你要是真编码起来，其实并不难。最后再啰嗦两句，一般说到移植，人们都会想到大量的代码工作，我也有过畏却，但最后狠下心来做的时候，发现也就so so 了，所以在代码界混的人，要有强大的动手能力，这样才能把自己的基本功练扎实。

