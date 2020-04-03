# 关键词
在基础游戏中有一些关键词，比如“虚弱”，“易伤”等等。它们是高亮的比且有提示信息。

# API
`addKeyword(String[] names, String description)` - 仅在你从 `EditKeywordsSubscriber` 拿到 ` receiveEditKeywords` 回调后或者 that you get from `EditKeywordsSubscriber` 否则你的关键词将不会被正确初始化。

* `names` - 名称，这是一个用于存放相似单词的数组 `(例如： {"block", "blocks"})`  ）必须使用小写
* `description` - 描述

# 例如
在关键词编辑方法中，假设我们要创建“Ice”这个关键词，描述为“Will apply a buff to the next attack with Ice in its description.”，代码如下：

```java
BaseMod.addKeyword({"ice"},"Will apply a buff to the next attack with Ice in its description.");
```

非常简单！例子和方法有 `@Blank The Evil`提供。