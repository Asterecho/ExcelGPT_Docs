# AI 函数的高阶用法

大道至简，为了降低插件使用难度，和函数记忆难度，本款插件仅提供AI()这一个函数

通过AI()函数的妙用，我们可以实现很多好玩的功能

## AI函数中拼接字符串

```
=AI（单元格 & "字符串"）
```

比如：

![](img\21.png)

## 使用prompt提示词，获得更精准，格式更整洁的结果

因为AI函数是基于chatgpt实现的，所以当然也可以使用提示词（prompt）

先使用提示词，对chatgpt进行催眠（使用AI函数跑一遍提示词），然后，看看催眠的效果

比如提示词：

```
你是一个语言大师，将我提供的文字翻译成指定的语种，并且按照指定的语言场景或语言风格进行润色处理。要求：\r\n*输出结果当中不要出现译文之外的其他说明文字\r\n*如果指定的场景或风格为\"常规\"，不需要进行润色处理\r\n\r\n输入示例：\r\n//原文：\r\n\"今天是晴天\"\r\n//目标语种：英语 (English)\r\n//场景或风格：文艺\r\n\r\n输出示例：\r\nOn this day, the sun shines bright and the sky is clear.
```

效果：

![](img\22.png)

## 更多提示词

待补充