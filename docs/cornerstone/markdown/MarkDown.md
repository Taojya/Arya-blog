#  一、结构类操作

## 多级标题

快捷键 ctrl+1~6 清除：Ctrl +0

# 这是一级标题

## \## 这是二级标题

### 这是三级标题				   ###  这是三级标题

#### 这是四级标题							#### 这是四级标题

##### 这是五级标题										 ##### 这是五级标题

###### 这是六级标题										 ###### 这是六级标题

## 有序列表

快捷键： ctrl+shift+[

1. 打开冰箱	

   1. 插电	
   2. 开机
   3. 开门

2. 把大象放进冰箱

3. 关闭冰箱

   

1. “.”后面一定加空格；
2. 点击回车，下一行自动生成有序列表；
3. 双击回车，下一行取消有序列表模式；
4. 点击下键，下一行取消有序列表模式；
5. 添加删除后，序号会自动调整；
6. 降级快捷键：tab键；
7. 升级快捷键：shift+tab键；

## 无序列表

快捷键： ctrl+shift+]

* 青菜
  * 白菜
  * 生菜

- 水果

  1. 苹果

  2. 梨子

     - 国产菠萝

     - 进口菠萝



* “*”或“-”加空格，生成无序列表
* 无序列表和有序列表可通过快捷ctrl+shift+]、ctrl+shift+[ 快捷键快捷生成，并实现穿插使用

## 任务列表

无快捷键，通过鼠标操作：选中文字 --> 段落 -->任务列表

- [x] 清除

- [x] 添加

- [ ] 删除



1. “- [ ] ”生成任务列表
2. “-”、“[”、“]”每个符号后面必须加一个空格

## 表格

快捷键：ctrl+t

| 姓名 | 学号 | 分数 |
| ---- | ---- | ---- |
| 张三 | 01   | 100  |
| 李四 | 02   | 60   |



1. 创建方式： |姓名|学号|分数| ——>回车
2. ctrl+回车，添加下一行

# 二、代码类操作

## 行内代码

快捷键：ctrl+shift+`

`Java`是一门编程语言。

1. 创建方式：`行内代码 `

## 代码块

快捷键：ctrl+shift+k

```html
<div>
  <p>代码块</p>
</div>
```

```javascript
function demo (val){
	return val
}
```



1. 创建方式：```java或者~~~java
2. 相关设置：文件 -> 偏好设置 -> Markdown  -> 代码块 勾选【显示行号】 ->  勾选【代码块自动换行】 -> 重启Tyora

# 三、元素类操作

## 插入图片

快捷键：ctrl+shift+i

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/markdown01.19g3ek3cj874.webp" style="zoom:25%;" />



1. 创建方式： "\!\[\]\(\)" -> 选择图片
2. 相关设置：文件 -> 偏好设置 -> 选择【复制图片./$(filename).assets文件】-> 勾选【优先使用相对路径】 -> 重启Typora



## 超链接

快捷键：ctrl+k

[百度一下](https://www.baidu.com) 

[百度](https://www.baidu.com/)

https://www.baidu.com/

1. 创建方式： 
   - 直接粘贴链接
   - “[显示内容]” + “(网址)”
2. 访问方式：ctrl+左键单击

## 水平分割线

没有快捷键

---

1. 创建：---（减号） —>回车

## 引用

快捷键：ctrl+shift+q

> 横眉冷对千夫指，俯首甘为孺子牛。 ——鲁迅

> 君不见黄河之水天上来，奔流到海不复回。

1. 创建： > 引用内容
2. 注意：大于号后需要有空格

# 四、样式类操作

## 加粗、斜体、高亮

### 加粗

快捷键：ctrl+b

**加粗内容** __加粗__ 内**容**

1. 创建：\**加粗内容** 或  \__加粗内容__(两个下划线)

### 斜体

快捷键：ctrl+i

斜体 *斜体内容*

1. 创建： \*斜体内容* 或 \_斜体内容_(一个下划线)
2. 既加粗又倾斜三个"*"，或三个"_"，或者 ctrl+b 加 ctrl+i

### 高亮

没有快捷键

==高亮内容==

1. 创建：\==高亮内容==
2. 相关设置：文件->偏好设置->勾选【高亮】->重启Typora

## 下划线、删除线

### 下划线

快捷键：ctrl+u

<u>下划线内容</u>

1. 创建:\<u>内容\</u>

### 删除线

快捷键：alt+shift+5

~~删除线内容~~

~~删除线~~

1. 创建：\~~文本内容~~

### 清除格式

快捷键：ctrl+\\(反斜杠)

# 五、视图类操作

## 大纲边框

快捷键：ctrl+shift+1

1. 相关设置：文件->偏好设置->外观->【侧边栏的大纲视图允许折叠和展开】->重启Typora

## 源代码模式

快捷键：ctrl+/

## 专注模式

快捷键：F8

## 打字机模式

快捷键：F9

# 六、导出类操作

## 导出PDF文件

文件 -> 导出 -> PDF -> 命名 -> 保存

# 七、快捷键汇总

| 功能 | 创建| 创建是否需要空格 | macOS快捷键| Windows快捷键| Windows备注                                                  |
| :--------- | -------------------- | ---------------- | :---------------- | :--------------------- | :----------------------------------------------------------- |
| 多级标题   | #                    | 是               | `cmd` +`1~6`      | `ctrl`+`1~6`           | ==清除：ctrl+0==                                             |
| 有序列表   | 序号 + .             | 是               | `cmd`+`opt`+`0`   | `ctrl`+`shift`+`[`     | ==1.tab降级；<br/>2. tab+shift升级；==                   |
| 无序列表   | -或==*==             | 是               | `cmd`+`opt`+`u`   | `ctrl`+`shift`+`]`     | ==有序列表、无序列表可穿插使用==                             |
| 任务列表   | - [ ]                | 是               | `cmd`+`opt`+`x`   | ==无==                 |                                                              |
| 表格       | \|姓名\|序号\|       | 否               |                   | `ctrl`+`t`             | 1. 表格插入行：ctrl+回车；<br/>==2. 表格删除行：ctrl+shift+delete== |
|| 行内代码   | `` | 否 |`cmd`+`shift`+`|`cmd`+`shift`+`|||
| 代码块     | \~~~java或==```java== | 否 |`cmd`+`shift`+`c`|==`ctrl`+`shift`+`k`==|||||
| 插入图片   | ==\!\[\]\()==        | ==否==           | `cmd`+`ctrl`+`i`  | ==`ctrl`+`shift`+`i`== |                                                              |
| 超级链接   | \[标题](网址)        | 否               | `cmd`+`k`         | ==`ctrl`+`k`==         | 访问：ctrl+鼠标左键                                          |
| 水平分割线 | ---                  | 否               | `cmd`+`shift`+`-` | 无                     |                                                              |
| 引用       | ==> 内容==           | 是               | `cmd`+`shift`+`q` | ==`ctrl`+`shift`+`q`== |                                                              |
| 加粗       | \**文字**或\__文字__ | 否               | `cmd`+`b`         | `ctrl`+`b`             |                                                              |
| 斜体       | \*文字*或者\_文字_   | 否               | `cmd`+`i`         | `ctrl`+`i`             |                                                              |
| 高亮       | \==高亮==            | 否               | `cmd`+`shift`+`h` | 无                     |                                                              |
| 下划线     | \<u>文字\</u>        | 否               |                   | `ctrl`+`u`             |                                                              |
| 删除线     | ==\~\~文字~~==       | 否               |                   | ==`alt`+`shift`+`5`==  |                                                              |
| 清除格式   | 无                   | 无               |                   | ==`ctrl`+`\`==         |                                                              |
| 大纲边框   | 无                   | 无               | `cmd`+`shift`+`1` | ==`ctrl`+`shift`+`1`== |                                                              |
| 源代码模式 | 无                   | 无               | `cmd`+`/`         | ==`ctrl`+`/`==         |                                                              |
| 专注模式   | 无                   | 无               | `F8`              | ==`F8`==               |                                                              |
| 打字机模式 | 无                   | 无               | `F9`              | ==`F9`==               |                                                              |



# 八、插入公式

首先设置：文件 -> 偏好设置 -> Markdown -> 勾选【Markdown 扩展语法】所有选项 -> 重启 Typora

## 行内公式

- 书写方式： \$ 公式\$，例如：\$A+B=C\$，直接输入，没有排版问题，推荐

行内公式演示：$A+B=C$

## 行间公式

- 书写方式：\$\$+`enter`+公式，例如：\$\$A+B=C$\$。默认居中显示，排版不便，不推荐。

行间公式演示：
$$
A+B=C
$$
==全角半角标点符号 高效切换 快捷键：ctrl+.==

##  常用算式

| 语法      | 说明 | 输入                                             | 显示                                               |
| --------- | ---- | ------------------------------------------------ | -------------------------------------------------- |
| ^         | 上表 | a\^2+b\^2                                        | $a^2+b^2= c^2$                                     |
| _         | 下表 | a_1+b_1=c_1                                      | $a_1+b_1=c_1$                                      |
| {}        | 分组 | a^{10}                                           | $a^{10}$                                           |
| \sum      | 累加 | \sum_{i=0}^{10} 或 \sum\limits\_{i=0}^{10}       | $\sum_{i=0}^{10}$   $\sum\limits_{i=0}^{10}$       |
| \prod     | 累乘 | \prod_{i=1}^{10} 或 \prod\limits\_{i=1}^{10}     | $\prod_{i=1}^{10}$   $\prod\limits_{i=1}^{i=10}$   |
| \frac     | 分式 | \frac{a}{b}   放大： \dfrac{a}{b}                | $\frac{a}{b}$ $\dfrac{a}{b}$                       |
| \sqrt     | 根式 | \sqrt{x}                                         | $\sqrt{10}$                                        |
| \vec      | 向量 | \vec{A}                                          | $\vec{A}$                                          |
| \lim      | 极限 | \lim_{n\to+\infin} 或 \lim\limits\_{n\to+\infin} | $\lim_{0\to+\infin} $  $\lim\limits_{n\to+\infin}$ |
| \int      | 积分 | \int_0^nf(x)dx 或 \int_0^nf(x)\text dx           | $\int_0^nf(x)dx$   $\int_0^nf(x)\text dx$          |
| \overline | 均值 | \overline{x}                                     | $\overline{x}$                                     |
| \quad     | 空格 | A\quad B                                         | $A\quad B$                                         |

## 常用运算符

| 运算符        | 语法        |
| ------------- | ----------- |
| $\pm$         | \pm         |
| $\times$      | \times      |
| $\div$        | \div        |
| $\leq$        | \leq        |
| $\ge$         | \ge         |
| $\neq$        | \neq        |
| $\Rightarrow$ | \Rightarrow |
| $\Leftarrow$  | \Leftarrow  |
| $\subset$     | \subset     |
| $\in$         | \in         |
| $\notin$      | \notin      |
| $\cup$        | \cup        |
| $\cap$        | \cap        |

## 古希腊字母

| 大写       | 语法     | 小写          | 语法        |
| ---------- | -------- | ------------- | ----------- |
| $\Alpha$   | \Alpha   | $\alpha$      | \alpha      |
| $\Beta$    | \Beta    | $\beta$       | \beta       |
| $\Gamma$   | \Gamma   | $\gamma$      | \gramma     |
| $\Delta$   | \Delta   | $\delta$      | \delta      |
| $\Epsilon$ | \Epsilon | $\epsilon$    | \epsilon    |
|            |          | $\varepsilon$ | \varepsilon |
| $\Zeta$    | \Zeta    | $\zeta$       | \zeta       |
| $\Eta$     | \Eta     | $\eta$        | \eta        |
| $\Theta$   | \Theta   | $\theta$      | \theta      |
| $\Iota$    | \Iota    | $\iota$       | \iota       |
| $\Kappa$   | \kappa   | $\kappa$      | \kappa      |
| $\Lambda$  | \Lambda  | $\lambda$     | \lambda     |
| $\Mu$      | \Mu      | $\mu$         | \mu         |
| $\Nu$      | \Nu      | $\nu$         | \nu         |
| $\Xi$      | \Xi      | $\xi$         | \xi         |
| $\Omicron$ | \Omicron | $\omicron$    | \omicron    |
| $\Pi$      | \Pi      | $\pi$         | \pi         |
| $\Rho$     | \Rho     | $\rho$        | \rho        |
| $\Sigma$   | \Sigma   | $\sigma$      | \sigma      |
| $\Tau$     | \Tau     | $\tau$        | \tau        |
| $\Upsilon$ | \Upsilon | $\upsilon$    | \upsilon    |
| $\Phi$     | \Phi     | $\phi$        | \phi        |
| $\Chi$     | \Chi     | $\chi$        | \chi        |
| $\Psi$     | \Psi     | $\psi$        | \psi        |
| $\Omega$   | \Omega   | $\omega$      | \omega      |



## 公式实战

[Markdown 公式指导手册](https://www.zybuluo.com/codeep/note/163962#cmd-markdown-%E5%85%AC%E5%BC%8F%E6%8C%87%E5%AF%BC%E6%89%8B%E5%86%8C)

```markdown
$x_1=\dfrac{-b\pm \sqrt{b^2-4ac}}{2a}$
```

$x_1=\dfrac{-b\pm \sqrt{b^2-4ac}}{2a}$

```markdown
$J_\alpha(x)=\sum\limits_{m=0}^{\infin}\dfrac{(-1)^m}{m!\Gamma(m+\alpha+1)}(\dfrac{x}{2})^{2m+\alpha}$
```

$J_\alpha(x)=\sum\limits_{m=0}^{\infin}\dfrac{(-1)^m}{m!\Gamma(m+\alpha+1)}(\dfrac{x}{2})^{2m+\alpha}$

~~~markdow
$\iint_Df(x,y)\text dx\text dy=\int_a^b\left[\int_{\varphi_1(x)}^{\varphi_2(x)}f(x,y)\text dy\right]\text dx$
~~~

$\iint_Df(x,y)\text dx\text dy=\int_a^b\left[\int_{\varphi_1(x)}^{\varphi_2(x)}f(x,y)\text dy\right]\text dx$

```markdown
(x,y,z)=3y_2z \left(3+\dfrac{7x+5}{1+y^2}\right)
```

$f(x,y,z)=3y_2z \left(3+\dfrac{7x+5}{1+y^2}\right)$



































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































