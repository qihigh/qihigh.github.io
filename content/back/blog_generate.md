+++
date = "2015-10-26T15:34:59+08:00"
draft = true
title = "blog生成命令相关"

+++

### jekyll 方式

+ 新建一篇可以进行评论的文章```rake post title="nginx study" ```

+ 运行blog的命令
	```
		jekyll serve
	```

### hugo 方式
+ 新建一个侧边栏

	在config.yaml中添加一个新的links就可以

+ 新建一篇文章

	``` hugo new android/new.md ```

+ 运行服务

	实时更新方式（当进行修改之后，后台会发通知给浏览器，浏览器自动更新，不需要手动刷新）
	``` hugo server --theme=herring-cove --buildDrafts --watch ```