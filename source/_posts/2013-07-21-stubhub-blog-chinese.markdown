---
layout: post
title: "Python 如何使基于 Java 的 StubHub 受益"
date: 2013-07-21 00:05
comments: true
categories: [Python, Work, English, StubHub]
---


## Python 如何使基于 Java 的 StubHub 受益

自2006年以来，Python 已经相当流行，你可以看到越来越多的初创公司在他们开始自己的业务时选择 Python作为主要语言，例如：

**Netflix** - 在线电视节目和电影公司  
**Dropbox** - 最流行的文件同步和共享工具  
**YouTube** - 分享在线视频  
**Disqus** - 在线讨论和评论服务  
**OpenStack** - 用于构建公共云和私有云的全开放源代码，全 Python 基础构建

当这些初创公司把这个优雅利落的语言作为基础设施来支持其业务的快速增长而得到越来越多的利益时，我在考虑我们 StuhbHub （一个基于 Java 生态系统的公司）怎样也可以从中获益，节省工作时间，并显着提高生产力，我会解释 Python 是什么？为什么选择 Python ，并向你展示用 Python 为我们的日常工作提供解决方案。

<!--more-->

## Python是什么 ？ 为什么选择Python ？

Python是一种解释型的，面向对象的动态语言，像Java一样同样是跨平台的。与传统的主流语言如 Java / C + + 相比，程序员们喜欢它是由于以下原因：

### 1.  Python 是一种多用途的语言

我们知道，每一种语言都有其自身的优势或劣势,例如有些人会用 C++ 写运行在Windows操作系统平台上的游戏，但没有人会用 C++ 去建网站。好消息是你几乎可以利用 Python 处理任何工作，例如：web 应用，桌面 GUI 应用，Linux 脚本或其他任何方便的工具，并且作为”胶水语言”你甚至可以在 Python 代码中调用像 Java/C++ 等其他语言，这意味着你已有的代码库可以重用。

### 2. Python 更有生产力

一般来说, 当我们谈及 Java 和 Python时，最显著的区别是作为动态语言 Python 不需要编译这一步。这其实就意味着”生产力”。

还记得我们如何验证 Java 代码的修改吗? 尤其是在 StubHub，我们有一个相当大的代码库。

1. 修改你的 Java 代码 （1分钟）
2. 使用 ant/maven 把你的 Java 代码编译为字节码 （5分钟）
3. 重启 Jboss/Tomcat 来部署你的应用程序（5分钟）
4. 打开浏览器查看变化

这里的痛点是：假设你有一个 bug 修复需要1分钟，但你必须等待至少10分钟，才能在浏览器中看到变化，更糟糕的是，如果所做的修复并不能正常工作，因此，又要花费下一个10分钟只是为了做构建和部署。

当你使用 Python 处理的话，就相当容易了。

1. 修改你的 Python 代码（1分钟）
2. F5 刷新浏览器查看变化

**恭喜！ 你在每次迭代修改上都节省了十分钟的时间。**

考虑一下每位开发者每天有多少次修改代码，像 StubHub 这样的庞大组织又有多少开发者，你可以计算下你总共可以节省多少工时，这可能大得超乎你的想象。

### 3.  优雅，整齐，紧凑的 Python

还有另一个主要的优点。Python的语法相当酷，我曾有一次用 Python 和 Jave 去实现同样的功能，与 Jave 相比 Python 仅仅用了一半的代码行数做了同样的事情。 基于此，这就是为什么人们喜欢用伪代码来验证想法或通过编写 Python 代码，实现一个快速原型。这能很快地让你知道你的想法或原型是否可行，然后你可以为你的生产环境用 Java 重写代码。这总好过你在一开始用 Java 编码，却最终发现你的原型是不可行的。

## StubHub 的 Python 故事

StubHub 走的技术路线如下：

第一代： coldfusion  
第二代：Java, 基于流的框架  
第三代：Java, Tapestry + Spring + Hibernate 各种现代技术框架

你可以看到整个技术生态系统的发展是基于Java的。作为一名工程师，有时你不可能说服你的团队或架构师放弃已有的代码库或把底层架构从 Java 转为其他的，但是，仍旧有一些改善的空间，你可以把事情做得又好又快，让我给你分享一下来自我在 StubHub 个人工作经历的几个故事。

### 故事1：使用 Python 处理 Java 源代码

2011年 StubHub 的技术团队举行了为期一周名为 **fixit** 的活动，活动要求所有的开发者在一周之内为已有的代码写尽可能多的测试用例以提高整体的测试覆盖率，但在此之前，我们需要先把一些测试用例标记为”broken”， 因为如果有任何测试用例失败，都会导致分析工具无法正常产生覆盖率报告。


问题是给测试用例标记上”broken”很容易, 只要给 @Test annotations 加上 broken 的属性，如下所示：

From
```java
public class SomeTest {
...
}
```
To
```java
@Test(groups = {"broken"})
public class SomeTest {
...
}
```

但在 StubHub 的代码库中有成百上千的测试用例代码，那意味着你首先得把他们都找出来，从代码库中检出一份案例，修改源代码，提交回修改，然后为下一个案例重复此步骤直到处理完所有成百上千个案例。

这事相当枯燥，我认为Python 更擅长这种重复的文件处理而不是我，以下是自动处理的Python代码片段：

```python
def start(path_name):
    for root, dirs, files in os.walk('src/' + path_name):
        if files:
            for file in files:
                filename = root + '/' + file
                print 'filename: ' + filename
                os.system('p4 edit //depot/project/pb_fixit_2011/gen31/test/' + filename)
                with open(filename, 'r+') as javafile:
                    fileContent = javafile.read()
                    matcher = re.search(r'@Test([\w\W]*?)public class', fileContent)
                    if not matcher:
                        fileContent = re.sub(r'public class', '@Test(groups = {"broken"} )\npublic class',  fileContent)
                        javafile.seek(0) # return to 0 file position
                        javafile.write(fileContent)
```

仅仅14行代码，通过遍历给定的路径，并使用 `p4 edit ...` 从 Peforce 检出文件，从Java源文件读取文件内容，使用正则表达式把测试用例标记为 broken，然后把更改写回源文件。我想这个脚本节约了我一天的工作。

### 故事2：测试第三方的API

2012年我加入了一个名为礼物卡的项目，有一个名为 Black Hawk 的第三方技术合作方，他们提供兑换钱的Web Service API。在这个项目刚开始的时候，他们希望确保 StubHub 的测试服务器和 Black Hawk 服务器之间的 API 调用是通畅的。由于这仅仅只是做个验证，并不需要很严谨的写下正规的，能够很好处理异常的 Java 代码，也不需要在我们已经安装了 JRE 和 HttpClient 库的环境下部署我们的代码并测试API（顺便说一下,  Python 作为基础应用被预装在大多数的Linux分发版中），当我认为这并不是很正式的代码，以后也不会被别人重用或者维护，就有了可以做同样事情的 Python 代码：

```python
from httplib2 import Http
def call_bh(url):
    try:
        http = Http()

        # Post request parameters
        body = """<?xml version="1.0" encoding="UTF-8"?> 
            <bhnums:request 
            ......
            </bhnums:request> 
        """
        headers = {'Content-type': 'text/xml;charset=UTF-8'}

        response, content = http.request(url, "POST", headers=headers, body=body)

        # Expected post response
        print content
    except Exception, e:
        print e
        print 'fail to call black hawk.'
    else:
        print 'success to call black hawk.'
```

通过 Python 来调用服务很令人愉快。尽管第一次失败了，由于代码的短小和易读性我只需要拷贝Python代码段到邮件里，发送给第三方的人询问:”这段代码有什么毛病吗？”他们更正了某个 http 请求包的问题，给我回了邮件，然后我在测试服务器上修改代码，再次测试，它正常工作了。这期间并没有用到 JRE/IDE/编译/构建/部署，仅仅通过 ssh 连接到你的 Linux 服务器，使用 vi/emacs 修改你的代码， 然后 **运行!** 

### 故事3：给客户重新发送奖励邮件

2012年，我还参与了另外一个名为 Rewards 的项目，旨在给参加这项活动的用户发送打折信息。他们加入这项活动以后，应该会收到一封关于活动详情的邮件。不幸的是由于服务器的问题，有 1285 位用户没有成功收到邮件，因此尽管他们已经加入了 Rewards 这个活动，却可能无法了解到如何获得折扣。因此，我被要求重新发送邮件来补救这个问题。

这项任务很紧急，如果我们能及时重新发送邮件，对用户来说也很有价值。但是如果我们选择传统的 Java 代码，我们需要通过 JDBC 或 Hibernate 来调用 SQL 从数据库获得没收到邮件的用户，写下重新发送邮件的代码， 弄清楚在生产环境上，哪里构建/部署这段代码比较合适，然后回滚这次部署，因为这段代码只会使用一次。这很丑陋而且我能想象到，这至少需要花费我们2天时间来开发部署。

选择Python的话事情就简单多了，它还是一段运行在Linux服务器上的脚本。为了避免读取数据库，我们可以首先从数据库中取出用户 id 并像这样作为一个列表写到脚本中：

```python
users = [556483, 556480, 556477, 556379, 556378, 556471, 556374, 469686, 556369, 556466, 556365, 556364, 556462, 556460, 556362, 556360, 556456, ...]
```

1285个用户 ID 看起来是比较大的数目，但把他们全部放在 Python 的一个列表中作为脚本的一部分还是可以接受，然后让我们给用户重新发送邮件：

```python
def sendmail_to_all(users, url):
    for user in users:
        time.sleep(1)
        sendmail(user, url)
```

最终，仅仅一个晚上，我想所有的用户应该都收到了他们遗漏的 Rewards 邮件。

### 故事4：推荐原型

在 2010 年我的一位擅长数学的同事为 StubHub 提出了一个推荐算法，大致思路是：当用户浏览 StubHub 上的分类/活动信息时，系统会猜测用户对什么最感兴趣并有可能购买，我想可能我们又需要使用 Python 构建一个快速应用来验证他的推荐算法是否有效。

因此我花费了15分钟跟他讨论算法并尝试理解底下的神奇的规则，15分钟后，我搞清楚了，然后开始用 Python编码。

第一, 我只是想为了验证算法，让原型能工作起来，我不会去用 Java 做一些服务器端的变更，数据模型的变更，对于我来说那更像是一个正式的项目，而不是一个原型。因此，我觉得我可以建一个内置浏览器的GUI桌面应用程序，人们可以操作内置浏览器并查看显示在GUI应用上（而非浏览器上）的推荐信息，这样可以使我们避免服务器端更改。幸运的是，用 Python 来构建一个 GUI 应用程序并不难。

其次，我需要从数据库获取历史订单信息并将这些处理后的数据应用于算法上。你可能知道 Python 在处理数据上同样比较擅长。

最终，在坚持不懈地使用 Python 编码 2 天后，我得到了一份如下的粗略原型：

GUI 应用的左侧是一个内置的浏览器显示了洛杉矶湖人的比赛，GUI 应用右侧显示了基于内置浏览器中当前的比赛，人们可能感兴趣的推荐赛事。推荐赛事里第一位的比赛是2010年的76队和湖人队之间的比赛，我猜你能告诉我这个推荐结果对于 NBA 粉丝来说是不是还算合理。：）

![recommendation-screenshot][1]

让我们就假设结果是不理想的，但至少在两天内，我们就有机会知道该算法可行与否。这比我们用 Java 在两周内得到一个好算法更重要。你越快证明你错了，你越快可以筛选出更好的算法。这才是关键的地方。假如您的 Java 原型失败会怎样？你将失去整两个星期的时间而非两天，这个代价太昂贵了。

## Python故事的结尾

这些就是我在 StubHub 所有的 Python 故事，他们可能很微不足道，但是我相信故事还会继续。我们并非在讨论 Java，Python 或 C++ 哪个更好，我一点也不关心这个，我只知道我越多武装自己，我就会变得越强大，去处理各种各样的问题，而且不仅要努力地工作，也要聪明地工作。 

这让生活会更轻松一点。


  [1]: https://lh6.googleusercontent.com/-adGChNMTZxM/UcuRJzYDQrI/AAAAAAAAAOU/iotr53MsUPE/s0/recomm_U.jpg "recomm_U.jpg"
