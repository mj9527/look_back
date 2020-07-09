# 基本命令
书籍初始化 gitbook init

书籍构建 gitbook build

启动书籍预览 gitbook serve

# nginx 服务配置
静态配置，指定gitbook生成的静态页面目录
```
   location /book {
           alias /Users/mjzheng/Documents/look_back/_book;
           index  index.html index.htm;
           autoindex on;
      }
```

# nginx 预览地址
[首页](http://localhost:8080/book/)



