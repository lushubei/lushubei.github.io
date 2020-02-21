---
layout:     post   
title:      "新文章"   
subtitle:   "a new blog"  
date:       2020-02-21  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - 奇淫巧计  
---

#### 设置自动编辑markdown的头
>1. intellij 选择 File  
>2. 选择Setting
>3. 选择''File and Code Templates"
>4. 右侧在Files标签中点击"+"
>5. Name中录入： markdown jekyll， Extension中录入： md  (Name可以自由定义)
>6. 录入头文件
>7. 点击确定

```
---
layout:     post  
title:      "新文章"  
subtitle:   "a new blog"  
date:       ${YEAR}-${MONTH}-${DAY}  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - 无分类  
---
``` 

![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-02-21/579414984809800.png?Expires=4735875106&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=zhiLxJ1CWctbvdP7XZDZgieJUus%3D)

#### 小试牛刀_新建一个markdown文件
>1. intellij 选择 File  
>2. 选择 New,可以看到我们自定义的文件格式

![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-02-21/582980107122500.png?Expires=4735878671&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=Rhu3ZFDjal1ggl2dq%2F3ZavwjP8o%3D)

可以看到新建的文件已经自动填入我们预设的标识，我设置的标识是博客的格式头，少做了很多重复的事