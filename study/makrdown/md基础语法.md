原文链接
https://www.markdownguide.org/basic-syntax/
# 主题  前面加#
主题2 下一行加任意数量的==
====================
段落使用空白行，不要使用空格或者制表符

行中止使用>=2的空格然后回车  
我觉得enter应该也可以
测试发现不可以
**强调使用两个****
*斜体使用一个**
***强调且斜体三个******  
****那四个呢，没有任何反应****  

> 块引用前面加>，
>>块中引用块使用>>
>>>那么三个会有什么效果呢>>>

有序的列表，大概是类似这种"1.aaa","2.bbb"
我喜欢的姑娘

1. 美丽  
    1. 短头发
        1. 短头发
            1. 更短的头发
            3. 光头是不好的
        3. 美丽的头发
    2. 美丽的眼睛
3. 聪明

无序列表
* 语文
+ 数学
- 英语

代码块一般使用四个空格或一个tab，但是如果在list中的话，空格或者tab加倍，并且前后要有空格行

    public void main(){
        System.out.println("hello world");
    }
图片`![apache](http://www.apache.org/img/ASF20thAnniversary.jpg)`
![未载入图片时的文本](图片链接)
![apache](http://www.apache.org/img/ASF20thAnniversary.jpg)

我最喜欢的搜索引擎是`[google](https://www.google.com/)`
[google](https://www.google.com/ "the best search engine")
