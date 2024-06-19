# WeRead2Notion-Pro 使用教程

WeRead2Notion-Pro不仅仅是将微信读书笔记同步到Notion，它是一个强大的管理系统。

WeRead2Notion和WeRead2Notion-Pro有什么区别主要有如下

- WeRead2Notion不支持在笔记中添加自己的笔记，每次有新笔记会删除原有的笔记
- WeRead2Notion-Pro支持添加自己的笔记，每次更新不会覆盖笔记。
- WeRead2Notion功能更加简洁，同步速度更快。
- WeRead2Notion-Pro支持按照年、月、周、日的阅读时长、笔记数阅读数的时间统计，支持数据可视化。所以选用哪个看个人喜好，也可以两个都用。

---

获取微信读书Cookie

- 浏览器打开网页版微信读书扫码登录
- 按F12进入开发者模式，依次点网络->文档，然后选中weread.qq.com，下拉找到Cookie，复制Cookie值

**如果没有内容显示，请刷新下浏览器。建议使用Chrome浏览器，有的小伙伴使用QQ浏览器拿到的Cookie一直不能用。**
![1](https://wowpb.pages.dev/file/6bdfff4d68eea5282c5ca.jpg)

---

授权

- 浏览器打开 https://api.notion.com/v1/oauth/authorize?client_id=f86ce456-f9cb-4cd5-8e4b-07bd9e18a8f8&response_type=code&owner=user&redirect_uri=https%3A%2F%2Fnotion-auth.malinkang.com%2Fweread2notionpro-oauth-callback

- 然后点击Next->Allow access
![2](https://wowpb.pages.dev/file/7629cfb912360fdf4707e.png)
![3](https://wowpb.pages.dev/file/d4f5eb4aa59c5d1614e76.png)

- 点击复制按钮，复制NOTION_TOKEN和NOTION_PAGE的值。
![4](https://wowpb.pages.dev/file/4d7a82b48c6550346497a.jpg)

---

在Github的Secrets中添加变量

- 打开你fork的工程，点击Settings->Secrets and variables->New repository secret
![5](https://wowpb.pages.dev/file/1fc2f879fd57e68916737.jpg)

- Name输入WEREAD_COOKIE，Secret输入框中填入你前面获取的微信读书Cookie，然后点击Add secret
![6](https://wowpb.pages.dev/file/7507b053c07dd29ad4a50.jpg)

- 同理，继续点击New repository secret，分别增加变量NOTION_TOKEN和NOTION_PAGE。最终的结果如下图所示。
![7](https://wowpb.pages.dev/file/a07c95582863503f413c7.jpg)

**注意这三个变量名一定要填写正确，一个字母都不能错，否则会同步失败。**

---

运行

上面配置完成之后，打开你Fork的项目，依次点击Actions->weread note sync-> Run workflow，就可以运行了。
![8](https://wowpb.pages.dev/file/735c01764455fad643e65.jpg)

以相同的方式运行read time sync。weread note sync主要用来同步书籍、笔记和划线。read time sync 用来生成热力图和同步阅读时长。
![9](https://wowpb.pages.dev/file/85400e38828e07be5ae6d.jpg)

---

问题排查

- 可以点击你Fork项目的Action，查看运行状态，绿色是成功，红色是失败。
![10](https://wowpb.pages.dev/file/4dd9964e113d1562a9771.jpg)

**warning:运行成功，只代表程序没有出错，不代表就一定同步数据，比如微信Cookie过期就不会报错。所以如果运行成功，Notion中没有数据的话，也可以通过下面第2步来查看日志**

- 可以点进去查日志，来自行排查问题。
![11](https://wowpb.pages.dev/file/380a01c3769a5ed580342.jpg)
![12](https://wowpb.pages.dev/file/ec681fbc32a0582950e0d.jpg)

---

问题解答

- 如何自动运行

Github Action提供每日定时自动运行程序的功能。之前有的朋友会问，我关了电脑会同步吗，该脚本运行在Github的服务器上，并不是运行在你的电脑上，所以你关机并不会影响程序自动运行。

    
- 每天何时同步

笔记设置的是utc时间的0点同步，如果你在中国，那就是每天8点同步。不过据我观察，Github这个可能有延迟。时间同步是每3个小时运行一次。你也可以自行修改同步时间，具体参考这里。需要注意的是Github每个月2000分钟有免费的额度，如果改的过于频繁可能会导致额度不够。

    
- 模板中哪些可以修改

Database中的Formula和Rollup类型的属性名可以修改，其他的属性名不支持修改，因为代码中是通过属性的名字来增加修改这个属性的，修改了名字程序就无法正常运行。要修改数据库的名字需要按照以下步骤。 依次选择Settings->Secrets and variables -> variables-> New repository variable。


具体的变量名可以参考下表中的变量名，值为你想要修改的名字。

| 变量名                  | 当前数据库的名字 |
|-----------------------|--------------|
| BOOK_DATABASE_NAME    | 书架          |
| REVIEW_DATABASE_NAME  | 笔记          |
| BOOKMARK_DATABASE_NAME| 划线          |
| DAY_DATABASE_NAME     | 日            |
| WEEK_DATABASE_NAME    | 周            |
| MONTH_DATABASE_NAME   | 月            |
| YEAR_DATABASE_NAME    | 年            |
| CATEGORY_DATABASE_NAME| 分类          |
| AUTHOR_DATABASE_NAME  | 作者          |
| CHAPTER_DATABASE_NAME | 章节          |



除此之外，其他数据可以任意修改，包括页面的布局都不会影响程序运行。

- 年月周天中的进度是什么

这里的进度我设置的是每天1小时，每周7小时，每月30小时，每年365小时。你可以自行修改公式设置你的进度。


- 表中的时间是什么

如果这本书你读完了，时间是读完的时间，如果没有读完，就是你最后阅读的时间。表中的日，周，月，年都是根据这个时间来设置的。



