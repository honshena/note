# Matlab基本操作与矩阵输入

# Matlab官网

> 官网地址: [https://www.mathworks.com/](https://www.mathworks.com/)

# 命令行(Command line terminal)

1. 运算符: + - * / ^
2. 运算优先级:  先次方后乘除再加减,括号优先级最高
3. 函数帮助: `help 函数名` 查看函数的使用方法等

**命令行模式下按住方向键上或下可以直接显示之前使用过的命令无需重复输入**

```matlab
A = 10 %行尾无分号表示显示计算结果
A = 10; %行尾有分号表示显示计算结果
clc %清除命令行
```

**函数**

```matlab
cos(N) sin(N) tan(N) %余弦,正弦,正切值
```

三角函数: [https://www.mathworks.com/help/matlab/trigonometry.html?s_tid=CRUX_lftnav](https://www.mathworks.com/help/matlab/trigonometry.html?s_tid=CRUX_lftnav)

```matlab
sqrt(N) %开根号
(N)^0.5 %开根号
log(N) %lnN
log2(N) %log2N
exp(N) %e^N
```

π: `pi`

# 嵌套函数(Embedding Functions)

**将长整式存放于变量中**

```matlab
sin(cos(pi)) %等价于 cos(pi) sin(ans)
```

# 关键字(Keyword)

```matlab
iskeyword %查看matlab中定义的关键字
```

**以下20个关键字**

```matlab
 	{'break'     }
    {'case'      }
    {'catch'     }
    {'classdef'  }
    {'continue'  }
    {'else'      }
    {'elseif'    }
    {'end'       }
    {'for'       }
    {'function'  }
    {'global'    }
    {'if'        }
    {'otherwise' }
    {'parfor'    }
    {'persistent'}
    {'return'    }
    {'spmd'      }
    {'switch'    }
    {'try'       }
    {'while'     }
```

# 变量(Variables)

**声明变量不需要标识符或类型**

```matlab
A = 10 %声明变量A=10
```

## 变量命名规则

1. 大小写敏感
2. 第一个字符必须为英文且变量名长度小于31个字符
3. 变量名可以有连字符,数字但不能有空格和标点

## 预定义变量

**自定义的变量优先级大于预定义变量**

> 定义cos="123"后调用函数cos(pi)得到的结果是"123"

### matlab calling priority(呼叫优先级)

![优先级](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-05-05_01-40-49.png)

```matlab
clear [all] %清除所有变量,注意可能会清除已经计算得到的变量
clear 变量名 %清除该变量
```

![预定义类型](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-05-05_01-18-14.png)

## 数据类型

**matlab默认数据类型是double**

```matlab
who %查看已经定义的变量
whos %查看变量类型及其详细信息
```

![数据类型](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-05-05_01-21-22.png)

# 数据格式(Numeric dispaly format)

```matlab
format long
pi %查看很长pi数据
```

| 格式   | 类型                         |
| ------ | ---------------------------- |
| short  | 小数点后4位                  |
| long   | 小数点后15位                 |
| shortE | 科学技术形式小数点后4位 10^N |
| longE  | 科学计数形式小数点后15位     |
| bank   | 小数点后两位                 |
| hex    | 十六进制                     |
| rat    | 分数                         |

# 矩阵及向量(Array)

## 输入矩阵

```matlab
arr = [1,2,3,4] %方式一:只有一行的矩阵
arr = [1;2;3;4] %方式二:只有一列的矩阵
arr = [1,2,3,4;5,6,7,8] %方式三:二行四列
```

## 获取元素

```matlab
arr = 
[	1,2,3;
	4,5,6;
    7,8,9`]
arr(5) %arr[1][1]==5 1->4->7->2->5 第五个元素
arr([1 3 5]) %取多个数组元素: 1,3,5
arr([1 3;2 2]) %构造新的二维矩阵: [1,7;4,4]
arr(1,2) %arr[0][1]==5
arr([1,3],[2,3]) %在arr中截取第1行第3行和第2列第4列相交部分[2,3;8,9]
```

## 数组操作

### 冒号运算符(Colon operator)

**等差运算**

```matlab
A = [1:100] %A=[1,2,3,...100] [i:j]==[i,i+1,i+2...j]
A = [1:2:100] %A=[1,3,5,7...99] [i:j:k]==[i,i+j,i+2j...i+m*j]
```

**取元素**

```matlab
A(3,:) = []%取A第三行全部元素,并赋值为空即删除第三行元素
```

### 增广矩阵

```matlab
A=[1,2;3,4]
B=[5,6;7,8]
F= [A B] %F=[1,2,5,6;3,4,7,8]
F= [A;B] %F=[1,2;3,4;5,6;7,8]
```

### 矩阵运算

```matlab
+ - * /
^ . '
A = [1,2,3;4,5,6;7,8,9]
B = [1,2,3;4,5,6;7,8,9]
A.*B %[1,4,9;1625,36;49,64,81] 相同位置相乘
A./B %[1,1,1;1,1,1;1,1,1] 相同位置相除

%矩阵和实数运算
A+2 %所有元素+2
A^2 %等于A*A

%矩阵的转置
A' %得到A的转置矩阵
```

### 特殊矩阵

```matlab
linspace() %随机产生数组参数为开始元素,结束元素,元素个数
eye(n) %nxn的矩阵只有主对角线上的值为1其余为0
zeors(n1,n2) %n1xn2的矩阵矩阵元素全为0
ones(n1,n2) %n1xn2的矩阵矩阵元素全为1
diag() %只有主对角线上有值其余都为0,diag([1,2,3,4])得到4x4的矩阵主对角线上为1,2,3,4
rand()
```

**矩阵函数**

```matlab
max(A) %求A数组列最大值
min(A) %求A数组列最小值
max(max(A)) %求A中的元素最大值
sum(A) %对A的列所有元素求和
mean(A) %对A列求平均
sort(A) %对列排序从小到大
sortrows(A) %对行排序从小到大
size(A) %求A占用的尺寸大小返回数组[行数,列数]
length(A) %求A的每一行长度只有行的长度
find(A) %求A的所有元素,可以是表达式find(A==5)返回数组下标
```

### 矩阵换行

```matlab
%当前矩阵长度太长,...换行
A=[1,2,3,4;...
	5,6,7,8]
```

