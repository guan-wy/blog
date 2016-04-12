---
title: Setting up a custom domain for blog
date: 2016/4/12 20:00:00
categories: [Technical notes]
tag: [GitHub, Blog, Hexo, Domain]
---

为博客建个自己的域名，以这次博客为例，同样也适用于其他的项目。最开始部署用的是 [插件 hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)，但是发现每次都得输入 github username & password，有点儿烦就改用 [gulp-gh-pages](https://github.com/shinnn/gulp-gh-pages) 了。下面列下实际步骤。

1. 执行 `$ hexo generate`，会在项目根目录生成一个名为 public 的文件夹，里面就生成的所有静态文件了。 [官方文档参考](https://hexo.io/zh-cn/docs/generating.html)

2. 在 GitHub Repo 中新建一个 `gh-pages` 分支

3. 在根目录 source 里面新建一个文件 `CNAME`，里面写上自定义的域名，比如 `blog.h2so4.me`。（先想清楚是要顶级域名还是二级域名，涉及到配置文件的改动）

4. 修改 `_config.yml` 里面的 url 和 root。
```
url: http://blog.h2so4.me
root: /
```

5. 在命令行里输入 `dig YOUR-USERNAME.github.io +nostats +nocomments +nocmd`。
里面的 YOUR-USERNAME 是 GitHub 用户名，运行完了可以看到：
`> YOUR-USERNAME.github.io     3600    CNAME	github.map.fastly.net.`
为自定义域名添加一条 A 纪录
```
Type: CNAME
Host: blog.h2so4.me
Answer: github.map.fastly.net
```

6. 安装 [gulp-gh-pages](https://github.com/shinnn/gulp-gh-pages)  `npm install --save-dev gulp-gh-pages`

7. 新建 `gulpfile.js` 文件，其中 public 就是存放静态文件的地方。
```
var gulp = require('gulp');
var ghPages = require('gulp-gh-pages');

gulp.task('deploy', function() {
  return gulp.src('./public/**/*')
    .pipe(ghPages());
});
```

8. 为了部署方便，在 package.json 里面加上 scripts 合并两条命令。
```
"scripts": {
  "deploy": "hexo generate && gulp deploy"
},
```

9. 运行 `npm run deploy` 直接部署。

10. Good! （强行满十条）
