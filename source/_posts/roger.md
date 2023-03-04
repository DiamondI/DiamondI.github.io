---
title: 罗杰疑案——麻将的bug
categories: Misc
---

# 罗杰疑案——麻将的bug

在罗杰疑案中，有一段四个人在打麻将的过程中聊有关案件的八卦的情节。在这个情节中，医生Shepperd胡了一次天胡，而这次天胡却极可能是阿婆笔下的一个bug。

<!-- more -->
**天胡原文如下[^1]：**

>I did not answer for a moment. I was overwhelmed and intoxicated. I had read of there being such a thing as The Perferct Winning--going Mah Jong on one's original hand. I had nerver hoped to hold the hand myself.
>
>With suppressed triumph I laid my hand face upwards on the table.
>
>'As they say in Shanghai Club,' I remarked--'The Tinho--The Perfect Winning!'

这里的`I`指的就是医生Shepperd，而这里的`The Tinho`和`The Perfect Wining`就是指”天胡“。

## 何为天胡？

在麻将中，天胡指的是洗完牌之后，抓完起始牌，就发现已经胡牌了。这是一种只有庄家能胡的牌，因为只有庄家抓完起始牌后拥有14张牌，而其他人都只有13张牌，少了牌是不可能胡的。

简而言之，**天胡**要求胡牌的人必须是**庄家**！

那么，谁是庄家呢？

## 庄家——East Wind

在麻将中，如果庄家胡牌，那么将会继续坐庄；如果是非庄家的玩家胡牌了，那么就会轮到庄家的下家坐庄。在英语中，庄家被称作East Wind。这是因为在中国，庄家又被称作“东风”的关系。

下面是mahjong wiki里对连庄与轮庄的说明

>For the next hand, **if the winner is not the dealer**, rotate the dealer and seat winds counterclockwise. The South player of the first round becomes the new East, and the other players change seat winds accordingly. Shuffle the tiles, rebuild the wall, and start again! **If the winner is the dealer**, he continues to be the dealer and the dealer is not rotated.

![](http://mahjong.wikidot.com/local--files/index:winds/hand1.png)

>The player sitting at the star is East in the first hand.

![](http://mahjong.wikidot.com/local--files/index:winds/hand2.png)

>The player sitting at the star is now South



在罗杰疑案的麻将情景中，有一个经常被提及的术语——East Wind：

**开局时，James，即医生坐庄[^1]：**

>'Go on, James,' said Caroline at last. 'You're East Wind.'

**Caroline胡的第一局[^1]：**

>'East Wind passes', said Caroline. 'I've got an idea of my own about Ralph Paton. Three Characters. But I'm keeping it to myself for the present.'

**Miss Gannett胡的第一局[^1]：**

>East Wind passed， and we started a new hand in silence.

**Miss Gannett胡的第二局[^1]：**

>East Wind passed, and we set to once more. Annie brought in the tea things. Caroline and Miss Gannett were both slightly ruffled as is often the case during one of these festive evenings.

这里的East Wind passed字面上看是指东风吹了过去。既然东风是庄家的意思，那么这里的实际意思自然是指轮到下家坐庄了。也就是说，至少小说中的麻将是会轮流坐庄的。

另外值得一提的一点是，从原文中可以看出，他们是遵守上海俱乐部的麻将规则来打的。而上海麻将的坐庄规则和其他地方不太一样。

**上海麻将坐庄规则[^2]：**

>**庄家：**
>
>第一局系统随机选择庄家。
>
>谁胡牌下盘当庄家，若流局则上盘庄家下盘继续当庄家。
>
>炮胡且一炮2响或3响，点炮者最近的一个胡牌者下家为下盘庄家。

那么原文里看得出来这一点吗？其实是能看出来。从前面引用的原文的Caroline胡的第一局来看，她胡了之后换了庄家，虽然没有明说庄家是谁，但是显然是她出的第一张牌——“Three Character”，即三万。也就是说，她在胡牌之后就成为了庄家。虽说也有可能她恰好是医生的下家，但是一来文中看不出这一点，二来谁胡谁坐庄也与上海麻将的规则一致。因此一定程度上可以确定，他们的规则就是谁胡谁坐庄的上海规则。

## 医生Shepperd是庄家吗？

**假如按照上海规则**

这时谁胡谁坐庄，显然上一次胡牌的人是Miss Gannett，庄家不可能是医生！

**假如按照其他地方规则**

如果小说中的麻将**遵守连庄规则**的话，那么医生也不可能是庄家！

在医生天胡之前，有这样一段描写[^1]：

>The situation became more strained. It was annoyance at Miss Gannett's going Mah Jong for the third time running which prompted Caroline to say to me as we built a fresh wall: 
>'You are too tiresome, James. You sit there like a dead-head, and say nothing at all'

也就是说在医生Shepperd天胡之前，Miss Gannett已经连续胡了三次了。这意味着，医生Shepperd这时候不可能是庄家！

如果Miss Gannett胡牌的某一次轮到自己坐庄了，那么她就连庄了，不可能由医生来坐庄。只能考虑这三次都不是Miss Gannett坐庄，那么最可能的情况下是，Miss Gannett第一次胡牌的时候是在自己的下家，而医生恰好是她的上家；可是即便如此，Miss Gannett胡了三次牌之后，也是轮到自己坐庄而不是医生坐庄。

因此，这可能是阿婆笔下的一个**bug**！

不过，如果小说中的麻将并没有连庄机制的话，也就是说不管谁胡牌，都换下家做庄家的话，那么医生坐庄就合情合理了。因为文中明确写明开局的时候，医生是东风。后面医生的姐姐胡了一把，Miss Gannett连续胡了三局。四局结束之后，便恰好又轮到医生坐庄。

不过我还是倾向于这是阿婆笔下的一个bug，没有连庄机制的麻将，实在是难以想象！我觉得零食行人是不会答应的吧？

> Perhaps tinted with the West’s fascination with exotic oriental cultures, mahjong became all the rage in the 1920s in the West and was promoted as the crystallization of ancient Chinese wisdom. [^3]

[^1]: The murder of roger ackroyd, HarperCollinsPublishers, Edition 2019 1

[^2]: 上海麻将百度百科，链接: [https://baike.baidu.com/item/上海麻将/1688864](https://baike.baidu.com/item/%E4%B8%8A%E6%B5%B7%E9%BA%BB%E5%B0%86/1688864)

[^3]: 陳熙遠. 從馬吊到馬將: 小玩意與大傳統交織的一段歷史因緣[J]. 2009.
