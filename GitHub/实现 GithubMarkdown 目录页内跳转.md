# 实现 GithubMarkdown 目录/页内跳转

- **GitHub支持的语法在标准markdown语法的基础上进行了修改，称为Github Flavored Markdow(GFM)**

- Github通过Anchor（锚点）来实现页内跳转，每个标题就是一个Anchor
  - [GFM Anchor的代码实现](https://github.com/jch/html-pipeline/blob/master/lib/html/pipeline/toc_filter.rb)

```json
对于标题#Hello或##Hello
正确的链接方式是[Hello](#hello)

对于标题 #Hello World
正确的链接方式为[Hello World](#hello-world)

注意：()中的字母均为小写，单词之间有间隔用 '-' 连接
```

