---
layout: post
title: 莫尔斯电码，比特，国际商品编码
date: 2019-04-01 14:43:32 +0800
categories: ['Programming']
tags: ['Programming']
---

<audio controls>
  <source src="/assets/code-the-hidden-language-of-computer-hardware-and-software/morse-sos.wav" type="audio/wav">
Your browser does not support the audio element.
</audio> 

如果你仔细聆听这段小音频，你会发现这段音频是由“3短”，“3长”和“3短”组成。可能你第一次听不出来，不过没有关系，好奇的你一定会再重复听几遍的。

我们在电视或者电影中的一些谍战片中，一般都会听到“滴答”和“滴...答”的各种电报声。是的，我刚刚听到的也是一段小小的电报声，其中的含义就是目前国际通用的莫尔斯电码求救信号：SOS。

莫尔斯电码由两种符号“点（dot）”和“dash（划线）”的各种组合进行编码。如下图所示，我们可以找到 “SOS”三个字母的莫尔斯电码码。我们刚刚听到的电报声就是用3个短音，3个长音，3个短音表示莫尔斯电码的3个点号，3个划线，3个点号。

![International Morse Code](https://upload.wikimedia.org/wikipedia/commons/b/b5/International_Morse_Code.svg)

单词“bit（比特）”被创造出来是为了代表单词“binary digit”，表示一个二进制数字位。莫尔斯电码中的“点”和“线”的二元符号，我们可以用二进制数的“0”和“1”进行表示。比如我们就可以用“000111000”表示我们刚刚听到的电报声。

我们生活中有一个最常见的二进制数的应用：条形码。当我们在超市或者零售店买东西时，我们会发现在结算时，收款员会拿起一种扫描仪录入我们购物车或手中的各种物品的信息进行快速结算。有时候，有些商品无法通过扫描仪自动录入，这个时候，收款员通常会盯着商品上的条码，然后用数字键快速的手动录入。

我们最常见的条形码的编码标准是 EAN-13。EAN 的英文全称是“European Article Number”，原来只是欧洲范围使用，后来在全球范围开始使用，表示全球范围内产品交易的商品代码，所以 EAN 也被称为 IAN （International Article Number）。数字13表示是13位数字的EAN编码规范。现在你就可以随手拿起身边的一件有条码的物品，让我们一起认识一下条形码 EAN-13。当然，如果你也可以拿起身旁的一本书，图书背后一般也会有条形码，书籍的条形码是 EAN-13 的一种，叫做：ISBN-13。这里再次说明一下，我看到的条形的下面要有13个数字。如果你拿到的是一个口香糖，你可能会发现口香糖的条码下面只有8个数字，这个通常就是EAN-8编码规范了。

下面以一个“500ml新鲜屋鲜牛奶”的光明鲜牛奶为例来看一下EAN-13的隐藏的奥义。

![500ml新鲜屋鲜牛奶](/assets/code-the-hidden-language-of-computer-hardware-and-software/500ml新鲜屋鲜牛奶.gif)

我们看到的条形码是由很多不同宽度的竖直黑色条纹组成的，这也是我们直观的称之为条形码的原因。扫描仪需要读取并识别的并不是条形码下方的一串数字，而是需要更精密的光学识别技术（OCR, Optical Character Recognition）去读取这些条纹。其实条形码之所以要这么长，主要还是为了方面扫描仪对准，扫描仪真正需要是条形码中的一条窄带。因此，我们从条形码中截取的如下一小段：

![500ml新鲜屋鲜牛奶](/assets/code-the-hidden-language-of-computer-hardware-and-software/500ml-bar-space-code.png)

你这么聪明和机智，一定会发现这些条纹和莫尔斯电码非常的类似。我们直观地注意到是一些黑色条纹，其实反过来想，这些黑色条纹之间的空隙，我们也可以认为是白色条纹。

我们先看下条形码下面的13个数字6901209302503的含义，然后，和莫尔斯电码一样，我们用二进制数的“0”表示白色条纹，“1”表示黑色条纹将条形码编码。

EAN-13个数字组成如下：

- 前3位数字代码国家代码，中国大陆大的数字是690-699。
- 后4位数字表示生产商代码，比如1209代表“光明乳业股份有限公司”。
- 在接着后5位表示产品代码，比如30250可以表示“500ml新鲜屋鲜牛奶”。
- 最后一位是校验位。

校验位的算法如下：

1. 求EAN-13的前12位的奇数位的数字之和并记为 X，如 X = 6 + 0 + 2 + 9 + 0 + 5 = 22
1. 求EAN-13的前12位的偶数位的数字之和并记为 Y，如 Y = 9 + 1 + 0 + 3 + 2 + 0 = 15
1. 求 1  × X + 3 × Y 并记为 S，即 1 × 22 + 3 × 15 = 67
1. 用10减去S的个位数的数值即为校验码，即 10 - 67 % 10 = 3

所以“690120930250?”的校验码计算如下：

```
10 - [(6 × 1 + 9 × 3 + 0 × 1 + 1 × 3 + 2 × 1 + 0 × 3 + 9 × 1 + 3 × 3 + 0 × 1 + 2 × 3 + 5 × 1 + 0 × 3) % 10]
= 10 - [(6 + 27 + 0 + 3 + 2 + 0 + 9 + 9 + 0 + 6 + 5 + 0) % 10]
= 10 - [(36 + 20 + 11) % 10]
= 10 - [67 % 10]
= 10 - 7
= 3
```

和莫尔斯电码一样，我们用二进制数的“0”表示白色条纹，“1”表示黑色条纹将条形码编码。EAN-13由95个等宽的条纹构成，从左到右结构如下：
1. 3位起始标记: 101
1. 42位（每7位一个数字）用于编码第2个到第7个数字，第一个数字不在条码中直接编码，而是间接编码。
1. 5位中间区标记: 01010
1. 42位（每7位一个数字）用于编码第8个到第13个数字
1. 3位结束标记：101

下面就是6901209302503的二进制编码：

```
101000101101001110110011001101100011010001011100001011100101101100100111011100101000010101
```

看到这个一串0和1，内心是不是很崩溃？

下面是将一串崩溃的数字进行简化：

```
101-0001011-0100111-0110011-0011011-0001101-0001011-1000010-1110010-1101100-1001110-1110010-1000010-101
```

还是崩溃？我们再进一步简化：

```
101
L9      G0      G1      G2      L0      L9
0001011-0100111-0110011-0011011-0001101-0001011
01010
R3      R0      R2      R5      R0      R3
1000010-1110010-1101100-1001110-1110010-1000010
101
```

好奇的你一定在想这些“L”，“G”，“R”是什么意思？（对的，你就是这么好奇。）

L 表示这些二进制数中有奇数个1，G和R表示这些二进制数中有偶数个1。因为条形码的左侧总是以L开始，而右侧是R开始，所以扫描仪在读取识别的时候会知道我们是不是把条形码拿反了，并自行纠正。当然，也有些智障的扫描仪会告警，让我们人工矫正一下再行扫描。

那么第一个数字6是怎么编码的呢？

我们可以把上面的L，G，R合并一下，即LGGGLLRRRRRR，这个规则便是数字6的编码。下表是EAN-13的编码结构：

<style>
  table, th, td {
    border: 1px solid black;
  }
</style>

第一位数字      | 左边6位数字                | 右边6位数字
----------------|----------------------------|-----------------------
0               | LLLLLL                     | RRRRRR
1               | LLGLGG                     | RRRRRR
2               | LLGGLG                     | RRRRRR
3               | LLGGGL                     | RRRRRR
4               | LGLLGG                     | RRRRRR
5               | LGGLLG                     | RRRRRR
6               | LGGGLL                     | RRRRRR
7               | LGLGLG                     | RRRRRR
8               | LGLGGL                     | RRRRRR
9               | LGGLGL                     | RRRRRR

那么数字0-9是如何像莫尔斯电码一样进行编码的呢？

数字    | L-code      | G-code     |  R-code
--------|-------------|------------|--------
0       | 0001101     | 0100111    |  1110010
1       | 0011001     | 0110011    |  1100110
2       | 0010011     | 0011011    |  1101100
3       | 0111101     | 0100001    |  1000010
4       | 0100011     | 0011101    |  1011100
5       | 0110001     | 0111001    |  1001110
6       | 0101111     | 0000101    |  1010000
7       | 0111011     | 0010001    |  1000100
8       | 0110111     | 0001001    |  1001000
9       | 0001011     | 0010111    |  1110100

如果你读到这里，我还是很佩服你的。那么条形码对我们有什么意义呢？我们可以看EAN-13的前3位编码是不是69开头，就可以知道这个商品是不是在产地是不是中国大陆呀。

从条形码6901209302503，我们可以查询到下面这些信息：

- 商品条码号：6901209302503
 
- 商品名称：500ml新鲜屋鲜牛奶
 
- 公司名称：光明乳业股份有限公司
 
- 注册地址：上海市吴中路578号

**参考**:

1. https://en.wikipedia.org/wiki/International\_Article\_Number
1. https://en.wikipedia.org/wiki/International\_Standard\_Book\_Number
1. https://en.wikipedia.org/wiki/List\_of\_GS1\_country\_codes
1. http://tiaoma.cnaidc.com/