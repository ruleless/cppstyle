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
