# 关于

虽然网站的构建并不复杂，但是在过程中确实多少还是被一些问题卡住过。于是就想在此记录一下。

##  站点访问不到
```Github``` 在个人设置中有两处对 ```用户名```的设置，一个可以姑且理解为 ```昵称```，只要符合命名规范，取什么名字都是可以的。重点是另一处设置，把它理解为 ```账户名```，它的作用就很大了，它可以作为登录 ```Github``` 时的参数，即账号+密码登录。它可以作为 ```Github``` 为每个注册用户提供的唯一标识，也就意味着 ```账户名``` 是不能重名的，于是乎 ```昵称``` 和 ```账户名``` 的差异就显而易见了。
那么 ```用户名``` 对构建此网站有什么影响呢？原来采用 ```Github Pages``` 构建静态用户站点，需要遵守 [Github文档-Github页](https://docs.github.com/zh/pages/getting-started-with-github-pages/about-github-pages) 的要求，即```若要发布用户站点，必须创建名为 <username>.github.io 的个人帐户拥有的存储库。``` 所以啊 ```username``` 就是上面介绍的 ```账户名``` 。如果搞错 ```昵称``` 和 ```账户名``` ，那么站点就会访问失败。

> 设置 ```昵称```：点击头像 -->  Settings --> Public profile  -->  Name -->  点击输入框，输入昵称  -->  下滑，点击 Update profile  
> 设置 ```账户名```：点击头像 -->  Settings  -->  Account -->  点击 Change username -->  点击 弹出的对话框（告知修改后会产生的影响）下方按钮 -->  输入修改的账户名后点击按钮确定

## 仓库更新文章后，访问站点内看不到
首先，检查 ```mkdocs.yml``` 中有没有配置 ```nav```，发现没有问题。于是回去快速浏览了[GitHub Pages 个人网站构建与发布](https://www.bilibili.com/video/BV1hL4y1w72r)，发现需要进行下仓库设置：
Pages --> Branch --> 选择 gh-pages。原来本地提交代码的时候默认是提交到 main分支，实现资源的更新，但在 ```PublishMySite.yml``` 配置的 job是使用 mkdocs-material部署到 gh-pages分支，并且没有设置分支合并，所以最后经过渲染的页面资源都在 gh-pages分支上。
