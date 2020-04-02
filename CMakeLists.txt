

# cmake学习  https://hijk.tech/category/cmake/
# created by fjut on 2020.04.01

# cmake中所有变量都是string
# 变量的引用：${变量名}
set(var 123)
message("joke = ${var}") # joke = 123
# 移除
unset(var)
message("joke1 = ${var}") # joke1 = 

# 拼接
set(me 123)
message("abc ${me} 789") # -> abc 123 789
message(abc ${me} 789)   # -> abc123789

# cmake 列表  
# set(列表名 值1 值2 ... 值N) 或 set(列表名 "值1;值2;...;值N")。
set(list 1 2 3 4)
message("list = ${list}") # list = 1;2;3;4

set(list 1;2;3;4)
message("list1 = ${list}") # list1 = 1;2;3;4

# cmake布尔值, 大小写敏感
# true: 1, ON, YES, TRUE, Y, 非0值
# false: 0,OFF,NO,FALSE,N,IGNORE,NOTFOUND,空字符串,以-NOTFOUND结尾的字符串

# 条件语句, 缩进和空格对语句解析没有影响
set(par1 OFF)
set(par2 OFF)
set(par3 ON)
if (${par1})
	message("par1 is true")
elseif (${par2})
	message("par2 is true")
else (${par3})
	message("par3 is true") # print here
endif (${par1})

# while (NOT par1)
	# message("par1 is true")
# endwhile()

# foreach(循环变量 参数1 参数2 ... 参数N)
# COMMAND(ARGS...)
# endforeach(循环变量)
foreach (item 1 2 3 4)
	message("item = ${item}") #print1 ~ 4
endforeach(item)

# 取值范围为[0, total]
foreach (item2 RANGE 5)
	message("item2 = ${item2}") #print 0 ~ 5
endforeach(item2)

# foreach(循环变量 RANGE start stop step)
# 循环范围为[start, stop],循环增量为step。
foreach (item3 RANGE 1 5 2)
	message("item3 = ${item3}") #print 1 3 5
endforeach(item3)

# foreach(循环变量 IN LISTS 列表)
set(list2 1 2 3 4)
foreach(item4 IN LISTS list2)
	message("item4 = ${item4}")
endforeach(item4)


# 函数
# 自定义函数命令格式：
# function([arg1 [arg2 [arg3 ...]]])
# COMMAND()
# endfunction()

function(func x y z)
	# message(WARNING "callfunction func")
	message("x = ${x}")     # x = 1
	message("ARGC = ${ARGC}")   # ARGC = 3
	message("arg1 = ${ARGV0} arg2 = ${ARGV1} arg3 = ${ARGV2}")  # arg1 = 1 arg2 = 2 arg3 = 3
	message("all args = ${ARGV}")   # all args = 1;2;3
endfunction(func)
# 调用函数
func(1 2 3)

# 自定义宏命令格式：
# macro([arg1 [arg2 [arg3 ...]]])
macro(ma x y x)
	message("call macro ma")
endmacro(ma)
# 调用
ma(1 2 3)

# 定义列表变量时的拼接，是直接拼接的参数的值，不管参数本身中是否已经存在分号，也不管参数是什么类型
set(ABC a b c;d "e;f" [[g;h]])
message("ABC = ${ABC}") # ABC = a;b;c;d;e;f;g;h

message(a;b;c)     # abc
message(a\;b\;c\;) # a;b;c
message("a;b;c")   # a;b;c
message([[a;b;c]]) # a;b;c

# 传递给命令的参数可以有三种类型：普通参数，引号参数，括号参数  [[]]为括号参数
message([[hello]]) # hello 

# 打印环境变量PATH的值
message("evn_path = $ENV{PATH}")
# 设置环境变量
# set(ENV{PATH} $ENV{PATH}:/opt/bin)

set(A YES)
if (${A})
	message("A is True")
endif(${A})


# 大小判断
# if(<variable|string> <op> <variable|string>)
# 根据<op>的值不同，有两种判断：
# 当<op>为 LESS, GREATER, EQUAL, LESS_EQUAL, GREATER_EQUAL时，按数值判断大小关系。
# 当<op>为 STRLESS, STRGREATER, STREQUAL, STRLESS_EQUAL, STRGREATER_EQUAL时，按字符串判断大小关系。
set(A 1)
set(B 2)

# if(A LESS B)  ?
if(${A} LESS ${B}) # true
	message("A LESS B")
endif(${A} LESS ${B})

# 文件判断
# if(EXISTS <path-to-file-or-directory>)
# 第一个参数为固定的EXISTS关键字，第二个参数可以是文件或目录的全路径，用于判断文件或目录是否存在，存在为真，否则为假。
if(EXISTS c:/windows)
	message("c:/windows is exists")
endif(EXISTS c:/windows)

# 与或非
# if命令中的表达式也可以使用AND，OR，NOT，和括号进行组合，形成复杂的表达式。例如：
# if(NOT ((1 LESS 2) AND (3 LESS 4)))

# 数学运算
# math(EXPR <output-variable> <math-expression>)
#其中第一个参数EXPR关键字是固定的，第二个参数接收一个变量名，第三个参数是要运算的表达式，
# math命令计算表达式后，将结果设置到第二个参数对应的变量中。例如
math(EXPR NUM 5*2)
message("NUM = ${NUM}") # NUM值为10
# math命令的功能还是比较简单，支持的运算符有+, -, *, /, %, |, &, ^, ~, <<, >>, (, ) 含义和C语言中的运算符相同

# 指定cmake最低版本, 最低兼容的版本
cmake_minimum_required(VERSION 3.6)

# project命令用于指定代码工程的基本信息，每个工程都需要调用此命令。project命令的完整形式如下：
# project(<PROJECT-NAME>
        # [VERSION <major>[.<minor>[.<patch>[.<tweak>]]]]
        # [DESCRIPTION <project-description-string>]
        # [LANGUAGES <language-name>...])
# 第一个参数是项目名，随后可以跟三个可选的参数段。

project("Joke")
message("project name: ${PROJECT_NAME}")

# project命令还会自动定义一些变量：
# PROJECT_NAME 存储当前工程的名称。
# PROJECT_SOURCE_DIR 存储当前工程的代码目录。
# PROJECT_BINARY_DIR 存储当前工程的构建目录。
# PROJECT_VERSION 存储当前工程的版本信息。

# mode参数是可选 STATUS  WARNING  SEND_ERROR  FATAL_ERROR
# message([<mode>] "message to display" ...)


# add_executable命令用于添加一个可执行程序类型的target  eg:

# add_executable(app foo.cpp foo.h main.cpp)
# 表示增加一个可执行程序类型的target，名为app，需要用到的文件为foo.cpp，foo.h和main.cpp。
# cmake默认在当前处理的CMakeList.txt所在的目录内查找文件，因此可以直接指定文件名或者文件相对路径，当然也可以使用绝对路径。


# 创建库 add_library
# 表示添加一个程序库target，名为lib_name，用到的文件为source1 source2等等。中间的参数用来指定这个库的类型：
# STATIC表示静态库，SHARED表示动态库，MODULE表示插件库（特殊类型的动态库，一般用于运行时的显式加载调用）。
# add_library(<lib_name><STATIC | SHARED | MODULE> source1 [source2...])

# 链接库 target_link_libraries
# target_link_libraries(<tt> [PRIVATE | PUBLIC | INTERFACE] <lib>...)

# eg:
# cmake_minimum_required(VERSION 3.0)
# project(P2 LANGUAGES CXX)
# add_library(student STATIC student.cpp student.h)
# add_executable(app main.cpp)
# target_link_libraries(app PRIVATE student)


继续：https://hijk.tech/cmake/04/























