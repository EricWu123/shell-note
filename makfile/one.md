# 基本介绍

### make 的内嵌函数

语法：$(FUNCTION ARGUMENTS) 或者 ${FUNCTION ARGUMENTS}，多个参数以,分隔。

```shell
$(subst FROM,TO,TEXT) 
函数名称：字符串替换函数—subst。
函数功能：把字串“TEXT”中的“FROM”字符替换为“TO”。
返回值：替换后的新字符串。
示例：
$(subst ee,EE,feet on the street)
替换“feet on the street”中的“ee”为“EE”，结果得到字符串“fEEt on the strEEt”。

函数名称：模式替换函数—patsubst。
函数功能：搜索“TEXT”中以空格分开的单词，将否符合模式“TATTERN”替换为“REPLACEMENT”。参数“PATTERN”中可以使用模式通配符“%”来代表一个单词中的若干字符。如果参数“REPLACEMENT”中也包含
一个“%”，那么“REPLACEMENT”中的“%”将是“TATTERN”中的那个“%”所代表的字符串。在“TATTERN”和“REPLACEMENT”中，只有第一个“%”被作为模式字符来处理，之后出现的不再作模式
字符（作为一个字符）。在参数中如果需要将第一个出现的“%”作为字符本身而不作为模式字符时，可使用反斜杠“\”进行转义处理。
返回值：替换后的新字符串。
函数说明：参数“TEXT”单词之间的多个空格在处理时被合并为一个空格，并忽略
前导和结尾空格。
示例：
$(patsubst %.c,%.o,x.c.c bar.c)
把字串“x.c.c bar.c”中以.c 结尾的单词替换成以.o 结尾的字符。函数的返回结果
是“x.c.o bar.o”
本文的第六章在 变量的高级用法的第一小节 中曾经讨论过变量的替换引用，它是
一个简化版的“patsubst”函数在变量引用过程的实现。变量替换引用中：
$(VAR:PATTERN=REPLACEMENT)
就相当于：
$(patsubst PATTERN,REPLACEMENT,$(VAR))
而另外一种更为简单的替换字符后缀的实现：
$(VAR:SUFFIX=REPLACEMENT)
它等于：
$(patsubst %SUFFIX,%REPLACEMENT,$(VAR))
例如我们存在一个代表所有.o 文件的变量。定义为“objects = foo.o bar.o baz.o”。
为了得到这些.o 文件所对应的.c 源文件。我们可以使用以下两种方式的任意一个：
$(objects:.o=.c)
$(patsubst %.o,%.c,$(objects))
```

