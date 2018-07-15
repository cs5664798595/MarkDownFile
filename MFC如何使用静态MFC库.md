MFC如何使用静态MFC库
你看到的这个文章来自于http://www.cnblogs.com/ayanmw

大部分MFC程序都是使用 在共享DLL中使用MFC ，但是VS每一个版本都需要一个 MFC运行库，实在是有点烦人。
所以我选择了使用静态MFC库，虽然文件会大一些，但是至少不麻烦了。

VS这个做的不够好，默认情况下居然报错：
复制代码
VC编译错误：

1>uafxcw.lib(afxmem.obj) : error LNK2005: "void * __cdecl operator new(unsigned int)" (??2@YAPAXI@Z) 已经在 LIBCMT.lib(new.obj) 中定义
1>uafxcw.lib(afxmem.obj) : error LNK2005: "void __cdecl operator delete(void *)" (??3@YAXPAX@Z) 已经在 LIBCMT.lib(delete.obj) 中定义
1>uafxcw.lib(afxmem.obj) : error LNK2005: "void * __cdecl operator new[](unsigned int)" (??_U@YAPAXI@Z) 已经在 LIBCMT.lib(new2.obj) 中定义
1>../bin/TLLogger_Unicode_Release.exe : fatal error LNK1169: 找到一个或多个多重定义的符号
复制代码
网上搜索后，发现，都没有明显的说明一个问题，那就是MFC是unicode还是muiltibytes。总之 VS隐藏了许多细节，但我们要了解这些细节，才能解决问题！

在此我做一个完整的补充：

 首先确定两个库：前一个是 mfc的静态库，后一个是 C的静态库
复制代码
Unicode Debug
===========
uafxcwD.lib;libcmtD.lib;

Unicode Release
===========
uafxcw.lib;libcmt.lib;


MultiBytes Debug
===========
nafxcwD.lib;libcmtD.lib;


MultiBytes Release
===========
nafxcw.lib;libcmt.lib;
复制代码
然后解决方案是：
复制代码
（分三步）

一、首先配置MFC的使用为在静态库中使用MFC：

属性->常规->MFC的使用，选择“在静态库中使用 MFC”

二、然后，配置运行库：

属性->C/C++->代码生成->运行库，选择“多线程(/MT)”

三、最后，在附加依赖项中加入nafxcw.lib和libcmt.lib两个库文件：（注意：库nafxcw.lib必须先于库libcmt.lib，前者为mfc静态链接库,后者为c运行时库）

属性->链接器->输入->附加依赖项，添加nafxcw.lib和libcmt.lib
复制代码


复制代码
解决方法：

http://blog.vckbase.com/zaboli/archive/2010/02/05/40921.aspx

原因：

CRT 库对 new、delete 和 DllMain 函数使用弱外部链接。MFC 库也包含 new、delete 和 DllMain 函数。这些函数要求先链接 MFC 库，然后再链接 CRT 库。

当 C 运行时 (CRT) 库和 Microsoft 基础类 (MFC) 库的链接顺序有误时，可能会出现以下 LNK2005 错误。

解决方法：

强制链接器按照正确的顺序链接库！

project->properties->Linker->Ignore Specific Library 添加 uafxcwd.lib Libcmtd.lib （输入- 忽略特定库）

在Additional Dependencied添加uafxcwd.lib Libcmtd.lib （输入- 附加选项 ）
复制代码


参考链接：
http://blog.csdn.net/lejun2011/article/details/8115463
http://blog.csdn.net/liucanrui/article/details/6453986
---恢复内容结束---
转载请注明出处：http://www.cnblogs.com/ayanmw 我会很高兴的！
------------------------------------------------------------------------------------------------
一定要专业!本博客定位于 ,C语言,C++语言,Java语言,Android开发和少量的Web开发,之前是做Web开发的，其实就是ASP维护，发现EasyASP这个好框架，对前端后端数据库 都很感觉亲切啊。. linux，总之后台开发多一点。以后也愿意学习 cocos2d-x 游戏客户端的开发。