# matlab结构化程式|变量与档案存取

# matlab脚本(Matlab script)

- 脚本文件: <file>`.m`
- matlab左上角`新建脚本`
- 脚本命名与变量名一致
- `run`执行整个脚本文件
- 代码执行从上到下
- 程式结束有一个`end`
- 程式运行进入死循环中可以按`ctrl + c`停止代码运行

```matlab
%第一个matlab程式
for i=1 :10
x=linspace(0,10,101);
plot(x , sin(x+i));
print(gcf , ' -deps ',strcat( 'plot' ,num2str(i) , '.ps '));
end
```

# 程式片段(section)

**通过`run section`执行程式片段**

```matlab
%程式片段1
%%
for i=1 :10
x=linspace(0,10,101);
plot(x , sin(x+i));
print(gcf , ' -deps ',strcat( 'plot' ,num2str(i) , '.ps '));
end
%程式片段二
%%
for i=1 :10
x=linspace(0,10,101);
plot(x , sin(x+i));
print(gcf , ' -deps ',strcat( 'plot' ,num2str(i) , '.ps '));
end
%%
```

# 调试(debug)

**脚本文件编写的左边设置断点,运行时会停留在断点处**

![debug](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506110646689.png)

# 流程控制(Flow control)

```matlab
% if elseif else
% > < == <= >= ~=不等于 && ||
if i<10
	disp("小于10")
elseif i>10
	disp("大于10")
else 
	disp(10)
end

% for嵌套
for i=1 :10
	for j=1:2:10
	%语句体
	end
end
% switch,case
% otherwise
try,catch
while
break
continue
end
pause
return
```

## if语句

![if语句](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506111541650.png)

## for语句

![image-20210506113925361](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506113925361.png)

## switch

![switch](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506112510863.png)

## while

![image-20210506112755287](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506112755287.png)

# 预宣告(Pre-allocating)

**定义的数组需要先给空间以节省matlab扩展数组的时间--matlab扩展数组先将源数组拷贝到新的地址空间**

```matlab
%无预宣告
tic
for i=1:2000
	for j=1:2000
		arr(i,j)=i+j;
    end
end
toc

%预宣告arr的大小
tic
arr=zeros(2000,2000);
for i=1:2000
    for j=1:2000
        arr(i,j)=i+j;
    end
end
toc
```



# 函数

## 常用函数

```matlab
rem(n,m) %n对m取余
prod(n:m) %从n乘到m,参数为一个数组,将数组元素相乘
input("字符串") %提示用户并读取用户输入,函数返回值是读取结果
isempty(n) %判断输入n是否为空
num2str(n) %数字n转字符串
disp("字符串") %输出字符串到控制台
plot(x,f(x)) %绘图
exp(x) %e^x次方
strcmp(x,y) %比较字符串相等
reverse(x) %字符串反转
magic(n) %随机获得n*n的矩阵
```

## 自定义函数

### 查看函数源码

```matlab
edit(which('mean.m'))
```

### 自定义函数

**关键字:`function`**

```matlab
function 返回值 = 函数名(参数) %函数名和文件名一致
%举个栗子
function y = mean(x) %文件名为mean.m
function [a F] = acc() %返回值可以是多个
```

**技巧:使用`.*`计算变量乘法**

### 默认函参(Function default variable)

```matlab
nargin %调用函数参数个数
varargin %函数参数列表
```



![image-20210506133816821](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210506133816821.png)

## 函数指针(Function handle)

```matlab
f = @(x) exp(-2*x) %f指向的是exp(-2*x),@(x)表示输入
```

# 变量进阶(Variable)

## string|structure|cell

### int & double

```matlab
double %defaule variable
int8 %8字节整数
uint8 %无符号整形8字节整数
int16 | uint16 | int32 | uint32 | int64 | uint64 %其他类型
```

**类型转换**

`以下均为函数,通过函数对变量进行类型转换`

![image-20210507165352801](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210507165352801.png)

### 字符(char)

```matlab
%字符或字符串可以使用单引号或双引号包裹
s = 'a'
whos %查看变量
uint16(s) %字符转整数
ss = 'string' %字符串
sss = [s ss] %字符串拼接
ssss = ['ad' ; 'ab'] %串联字符串数组维度要一致,即两个字符串的长度要一致
ss(3) %取单个字符
sss=='i' %取出每个字符和i比较[0 0 0 1 0 0]
strcmp("ad",'ad') %比较字符串相等
reverse("abcd") %字符串反转
str = 'aba'
str(str == 'a') = 'c' %将str中的a字符替换为c字符
```

### 结构体(structure)

 ```matlab
 %声明结构体
 stu.id = '123'
 stu.name = "xiaoming"
 stu.age = 20
 stu.score = [80.9,90.4]
 %声明第二个结构体
 stu(2).id = '456'
 stu(2).name = "xiaohong"
 stu(2).age = 21
 stu(2).score = [82.3,89.1] %stu(2).score(1) == 82.3
 %结构体嵌套
 stu.name= "xiaoming"
 stu.family.father = "xiaodong"
 ```

**结构体常用方法**

```matlab
fieldnames(stu) %查看结构体stu所有字段,返回值为字符串数组
rmfueld(stu,'id') %删除结构体某个字段
```

![image-20210507185904988](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210507185904988.png)

### 阵列(Cell array)

**声明方式**

![image-20210507192618327](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210507192618327.png)

**这里cell array变量其实是一个指针**

```matlab
%读取cell array
B = A{1,1} %B为矩阵
C = A{1,1}(1,1) %C为A{1,1}矩阵中第一个元素
```

**cell array 转结构体**

```

![image-20210507193848178](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210507193848178.png)

**三维阵列**

​```matlab
A{1,2,3}%行列高
B= [1 2;3 4] C=[5 6;7 8]
D = cat(3,A,B) %拼接数组
reshap() %参数: 阵列或数组,row,con
```

![image-20210507200443214](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210507200443214.png)

### 检查变量类型

![image-20210507201536370](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210507201536370.png)

# 结果持久化(data access)

>**way**
>
>| 文件      | 导出     | 导入    |
>| --------- | -------- | ------- |
>| mat文件   | save     | load    |
>| excel文件 | xlswrite | xlsread |
>| txt文件   | fprintf  | fscanf  |
>

 ```matlab
  % .mat
 save(filename)
 save(filename,variables)
 save(filename,variables,fmt)
 
 save data.mat [-ascii] %存储工作区所有变量[指定编码格式]
 save 'data.mat' A B %存储变量A B
 save('data.mat','A','B') %存储变量A B
 load ('data.mat','-ascii') %加载变量,参数二和文件编码一致也可以没有
 
 % .xlsx
 score = xlsread('data.xlsx')
 score = xlsread('data.xlsx','B2:D4') %读取某个区间
 xlswrite('data.xlsx',A,1,'E2:E4') %将变量A写入表1的E2到E4
 xlswrite('data.xlsx',{'标题'},1,'E1') %表1的E1的标题为标题一
 [content head] = xlsread('data.xlsx') %读取内容和表头,heard是cell array
 
 % 档案
 fid = fopen('data.txt','r+') %返回值为fid,第一个参数是文件名,第二个参数是权限有r+,w+
 fprintf(fid,"%f %f\n",x,y) %格式写入文件,与C语言fprintf大致相同
 fsacnf(fid,格式,大小)
 fclose(fid) %关闭文件
 ```

![image-20210507213353766](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210507213353766.png)

