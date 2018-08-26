# C++编程风格
## 命名约定
### 通用命名规则
函数命名、变量命名、文件命名应具有描述性，不要过度缩写，类型和变量应该是名词，函数名可以用”命令性“动词。
如何命名？尽可能给出描述性名称，不要节约空间，让别人很快理解你的代码更重要，好的命名选择：
``` c++
int numErrors; // Good
int numCompletedConnections; // Good
```
丑陋的命名使用模糊的缩写或随意的字符：
``` c++
int n; // Bad - 指代不明
int nerr; // Bad - 含混的缩写
int compConns; // Bad - 含混的缩写
```
类型和变量名一般为名词：如FileOpener、numErrors。
函数名通常是指令性的，如openFile()、setNumErrors()，访问函数需要描述的更细致，要与其访问的变量相吻合。

尽量不要使用缩写，除非放到项目外也非常明了，例如：
``` c++
// Good
int numDnsConnections; // 基本上大家都知道DNS是什么

// Bad!
int svrhostConnections; // svrhost只在项目组内有意义
```
不要用省略字母的缩写：
``` c++
int errorCount; // Good
int errorCnt; // Bad
```

### 文件命名
文件名要全部小写，可以包含下划线（_）。
可接受的文件命名：

``` c++
my_useful_class.cpp
myusefulclass.cpp
```

C++文件以.cpp结尾，头文件以.h结尾。不要使用已经存在于/usr/include下的文件名。
通常，尽量让文件名更加明确，`http_server_logs.h`就比`logs.h`要好。
定义类时文件名一般成对出现，如`foo_bar.h`和`foo_bar.cpp`，对应类FooBar。

### 类型命名
每个单词以大写字母开头，不包含下划线：MyExcitingClass、MyExcitingEnum。
所有类型命名：类、结构体、类型定义（typedef）、枚举，使用相同约定，例如：

``` c++
// classes and structs
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...
// typedefs
typedef hash_map<UrlTableProperties *, string> PropertiesMap;
// enums
enum UrlTableErrors { ...
```

### 变量命名
变量名以小写字母开头，后跟的单词以大写字母开头，类的成员变量以下划线结尾，如：
myExcitingLocalVariable、myExcitingMemberVariable_。

普通变量命名：
``` c++
string table_name; // Bad
string tablename; // Bad
string tableName; // Ok
```

结构体的数据成员可以和普通变量一样，不用像类那样接下划线：
``` c++
struct UrlTableProperties
{
    string name;
	int numEntries;
}
```

静态全局变量以`s_`开头，全局变量以`g_`开头。

### 常量命名
### 函数命名
函数名以小写字母开头，后跟的单词首字母大写：
``` c++
addTableEntry()
deleteUrl()
```

### 命名空间
命名空间命名全小写，单词间以下划线（_）连接。

### 枚举命名
枚举值全部大写，单词间以下划线相连：MY_EXCITING_ENUM_VALUE。
枚举名称属于类型，因此大小写混合：UrlTableErrors。

``` c++
enum UrlTableErrors {
    OK = 0,
	ERROR_OUT_OF_MEMORY,
	ERROR_MALFORMED_INPUT,
};
```

### 宏命名
宏命名全部大写，单词间以下划线相连：
``` c++
#define ROUND(x) ...
#define PI_ROUNDED 3.0
```

### 命名约定总结

  1. 总体规则：不要随意缩写，如果说ChangeLocalValue写作ChgLocVal还有情可原的话，
     把ModifyPlayerName写作MdfPlyNm就太过分了，除函数名可适当为动词外，其他命名尽量使用清晰易懂的名词；
  1. 宏、枚举等使用全部大写+下划线；
  1. 类、结构体的单词首字母大写，不加下划线；
  1. 文件、命名空间等使用全部小写+下划线；
  1. 变量（含类、结构体成员变量）、函数首字母大写，后跟的单词以大写开头，不加下划线。
     类成员变量以下划线结尾，静态全局变量以`s_`开头，全局变量以`g_`开头；

## 头文件
### #define保护
所有头文件都应该使用#define防止头文件被多重包含（multiple inclusion），命名格式是：`__<FILE>_H__`。

### 头文件依赖
使用前置声明（forward declarations）尽量减少.h文件中#include的数量。

当一个头文件被包含的同时也引入了一项新的依赖（dependency），只要该头文件被修改，代码就要重新编译。
如果你的头文件包含了其他头文件，这些头文件的任何改变也将导致那些包含了你的头文件的代码重新编译。
因此，我们应尽量少包含头文件，尤其是那些包含在其他头文件中的头文件。

使用前置声明可以显著减少需要包含的头文件数量。
如：头文件中用到类File，但不需要访问File的声明，则头文件中只需前置声明class File;
而无需`#include "file/base/file.h"`。

在头文件如何做到使用类Foo而无需访问类的定义？
  1. 将数据成员类型声明为Foo *或Foo &；
  1. 参数、返回值类型为Foo的函数只声明，但不定义实现；
  1. 静态数据成员的类型可以被声明为Foo，因为静态数据成员的定义在类定义之外；

另一方面，如果你的类是Foo的子类，或者含有类型为Foo的非静态数据成员，则必须为之包含头文件。
有时，使用指针成员（pointer members）替代对象成员（object members）的确更有意义。
然而，这样的做法会降低代码可读性及执行效率。如果仅仅为了少包含头文件，还是不要这样替代的好。
当然，.cpp文件无论如何都需要所使用类的定义部分，自然也就会包含若干头文件。
总之，能依赖声明的就不要依赖定义。

### 内联函数
只有当函数只有10行甚至更少时才会将其定义为内联函数（inline function）。
当函数被声明为内联函数之后，编译器可能会将其内联展开，无需按通常的函数调用机制调用内联函数。

优点：当函数体比较小的时候，内联该函数可以令目标代码更加高效。

缺点：滥用内联将导致程序变慢，内联有可能是目标代码量或增或减，这取决于被内联的函数的大小。
内联较短小的存取函数通常会减少代码量，但内联一个很大的函数将增加代码量。
现代处理器，由于能更好的利用指令缓存（instruction cache），小巧的代码往往执行更快。

对于内联函数，一个比较得当的处理规则是，不要内联超过10行的函数。
另外，内联那些包含循环或switch语句的函数是得不偿失的，除非在大多数情况下，这些循环或switch语句从不执行。
虚函数和递归函数不应被声明为内联函数，很多编译器并不支持虚函数和递归函数内联。

### 函数参数顺序

定义函数时，参数顺序为：输入参数在前，输出参数在后。
C/C++函数参数分为输入参数和输出参数两种，有时输入参数也会输出（值结果参数）。
输入参数一般传值或常数引用（const references），输出参数为非常数指针（non-const pointers）。
应该总是将所有输入参数置于输出参数之前，不要仅仅因为是新添加的参数，就将其置于最后，而应该依然置于输出参数之前。

### 包含文件的次序
将包含次序标准化可增强可读性、避免隐藏依赖，次序如下：C库、C++库、其他库的.h、项目内的.h。
