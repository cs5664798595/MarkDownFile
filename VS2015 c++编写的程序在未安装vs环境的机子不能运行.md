### 问题：VS2015 c++编写的程序在未安装vs环境的机子不能运行，缺少mfc140d.dll xxx140d.dll等的解决方法

> 解决方法：
> 应用程序采用MFC静态编译;



静态编译涉及到的两个库的添加：前一个是 mfc的静态库，后一个是 C的静态库 ，在不同的编译模式下，具体位置不同的文件，总结如下：

* #### Unicode Debug


uafxcwD.lib;libcmtD.lib;

* #### Unicode Release
uafxcw.lib;libcmt.lib;


* ####  MultiBytes Debug
===========
nafxcwD.lib;libcmtD.lib;


* ####  MultiBytes Release
  ===========
  nafxcw.lib;libcmt.lib;
### 具体操作流程（分三步）：

```
一、首先配置MFC的使用为在静态库中使用MFC：

属性->常规->MFC的使用，选择“在静态库中使用 MFC”

二、然后，配置运行库：

属性->C/C++->代码生成->运行库，选择“多线程(/MT)”

三、最后，在附加依赖项中加入nafxcw.lib和libcmt.lib两个库文件：（注意：库nafxcw.lib必须先于库libcmt.lib，前者为mfc静态链接库,后者为c运行时库）

属性->链接器->输入->附加依赖项，添加nafxcw.lib和libcmt.lib
```