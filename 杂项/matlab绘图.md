# matlab绘图

# 绘图

## `plot()`绘图

```matlab
plot(x,y) %绘制二维平面图像
plot(y) %x从1开始自增,x属于[1,2,...n] n=length(y)
plot(cos(0:pi/20:2*pi)) %基础绘图
%matlab绘图前会清除上次绘图
hold on %保留绘图结果
plot(cos(0:pi/20:2*pi))
plot(sin(0:pi/20:2*pi))
hold off
```

## 绘图样式

### 图像样式

官网地址:[https://ww2.mathworks.cn/help/matlab/ref/linespec.html?s_tid=doc_ta](https://ww2.mathworks.cn/help/matlab/ref/linespec.html?s_tid=doc_ta)

---

```matlab
plot(y,'or') %用红色圈圈绘图
plot(y,'xg') %用绿色的叉叉绘图
plot(y,'or--') %用红色圈圈绘图,虚线连接绘图点
plot(y,'xg:') %用绿色的叉叉绘图,点连接绘图点
%举例
x=0:0.5:4*pi;
y=sin(x); 
h=cos(x);
w=1./(1+exp (-x));
g=(1/(2*pi*2)^0.5) .*exp ((-1.* (x-2*pi).^2)./ (2*2^2));
plot(x,y,'bd-',x, h,'gp:',x,w,'ro-',x,g,'c^-'); %画多个函数
legend ( 'sin (x)','cos(x)','Sigmoid',' Gauss function'); %标识图像名称
tf([1,2],[1])
rlous(g)
```

![image-20210508123342820](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210508123342820.png)

### 图像线条标注

`legend`

```matlab
legend ( 'sin (x)','cos(x)','Sigmoid',' Gauss function'); %标识图像名称
```

### 图像标题及标签

```matlab
title() %参数为字符串,定义图像的标题
xlabel() %定义x轴
ylabel() %定义y轴
zlabel() %定义z轴
```

### 图像标注(text & annotation)

**`latex` code可以使用`math type`去转换得到**

```matlab
text(x,y,str,'Interpreter','latex') %文字显示的x坐标,y坐标,显示的文字,后两个参数表示以latex方式显示
annotation('arrow','X',[0.32,0.5],'Y',[0.6,0.4]) %表示使用箭头,后面的参数表示箭头的起始x,y的坐标比例(32%,60%))和终止x,y的坐标比例
```

### 图像属性(modify)

图像属性:[https://ww2.mathworks.cn/help/matlab/graphics-object-properties.html](https://ww2.mathworks.cn/help/matlab/graphics-object-properties.html)

---

```matlab
% 可以在图像界面点击edit编辑figure properties
% matlab代码修改图像属性
id = plot(x,y) %获取到这个图像的线条id(handle)通过handle去修改属性
get(id) %获得对象属性
set(id,'属性',值) %设置对象属性
% 常用属性 gca
set(gca,'XLim',[1,2]) %设置gca的x轴
set(gca,'YLim',[1,2]) %y轴
set(gca,'XTick',0:pi/2:2*pi) %设置gca的x轴间距
set(gca,'XTickLabel',0:90:360) %等间距设置gca的x轴标签
% 设置图像x轴坐标为字符串
set(gca,'FontName','symbol') %设置x轴的坐标为字符
set(gca,'XTickLabel',{'0','p/2','p'}) %设置gca的x轴间距上的字符串,p在symbol中是π

set(gca,'YTick',0:0.1:2) %y轴间距
set(gca,'FontSize',25) %字体大小
% 函数线条的属性 id
set(id, 'LineStyle','-.') %设置线条的样式,get(id)查看属性并修改
%函数的marker
plot(x,'-md','MarkerEdgeColor','k','MarkerFaceColor','g','MarkerSize',10)
```

![image-20210508133759380](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210508133759380.png)

### 多图形

```matlab
figure, plot(x,y) %打开一个新窗口绘制
%多个图像的时候注意gcf和gca指向的是最后一个图像,之前的figure的gac和gcf已经丢失
%指定图像位置
figure('Position',[left,bottom,width,height]) %数组前两个表示位置,后两个表示图像大小

%一个图像画多个figure
subplot(x,y,1) %第三个参数是位置,如果图像里含有2*3个figure,那么参数从1到6
```

![image-20210508184352640](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210508184352640.png)

![image-20210508230758913](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210508230758913.png)

### 图像存储

`saveas`: [https://ww2.mathworks.cn/help/matlab/ref/saveas.html?searchHighlight=saveas&s_tid=srchtitle](https://ww2.mathworks.cn/help/matlab/ref/saveas.html?searchHighlight=saveas&s_tid=srchtitle)

![image-20210508231038847](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210508231038847.png)

### 图像类型

![类型](https://images-1300732204.cos.ap-chengdu.myqcloud.com/20200612230233230.png)

### 特殊图像 (special plots)

```matlab
logspace(-1,1,100) %从10^-1到10^1取100个数logN
semilogx() %以x为对数
semilogy() %以y为对数
loglog() %取两个轴为对数
%双y轴图
[AX,H1,H2] = plotyy(x,y1,x,y2) %画在同一张图,返回值axs,第一条线,第2条线
set(get(AX(1),'Ylabel'),'String','标题1')
set(get(AX(2),'Ylabel'),'String','标题2')
set(H1,'LineStyle','--')
set(H2,'LineStyle',':')
%柱状图
hist(y,bins) %bins为number,表示柱状图柱子的数量
```

#### 柱状图

![image-20210509000204727](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509000204727.png)

![image-20210508235830555](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210508235830555.png)

![image-20210509003538418](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509003538418.png)

#### 极点图

![image-20210509004316422](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509004316422.png)

![image-20210509125949165](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509125949165.png)

![image-20210509135857253](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509135857253.png)

**color map本身是矩阵**

![image-20210509140445670](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509140445670.png)

![image-20210508233226091](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210508233226091.png)



#### 3D![image-20210509140747698](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509140747698.png)

![image-20210509140955683](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509140955683.png)

![image-20210509141908724](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509141908724.png)

![image-20210509142402740](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509142402740.png)

![image-20210509143002344](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509143002344.png)

#### 视图

![image-20210509144218144](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509144218144.png)

![image-20210509144506560](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509144506560.png)

#### 空间多面体

![image-20210509145728445](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509145728445.png)

# 图形用户界面(GUI)

> matlab输入`guide` 

# 影像处理

![image-20210509160537979](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210509160537979.png)