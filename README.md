# Alvin-blog

> This is Alvin-Blog backup


Blog framework
- hexo: https://hexo.io/


Using themes
- Next: http://theme-next.iissnan.com/getting-started.html

gitignore
- themes/next

Push next _config.yml  to config/

常用的命令
- hexo s 启动hexo本地的服务器
- hexo n 新建一篇文章  hexo new "my first article"
- hexo g 生成
- hexo d 部署
- hexo p 发布 Moves a draft post from _drafts to _posts folder.
- hexo clean 清楚所有的缓存


## 问题解决
### Next 主题代码高亮
场景：本项目使用的是NEXT主题，发现在升级之前代码是可以自动高亮的，但是升级后就不行了，查询NEXT的知道配置发现配置正确并没有什么问题，于是翻阅hexo-theme-next的issue，参考https://github.com/iissnan/hexo-theme-next/issues/989 解决
解决：将站点配置文件_config.yml中的`auto_detect: false`改为`true`, 之后执行，`hexo clean`，`hexo g`, 刷新页面即可 
eg：

```
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:
```

### 生成的文章解构目录点击无效并且报错
解决:  参考https://github.com/celsomiranda/hexo-renderer-markdown-it/issues/40
安装markdown-it-named-headings
```
npm install markdown-it-named-headings --save
```
在Node_module中hexo-renderer-markdown-it/lib/renderer.js中加入：
```
 parser.use(require('markdown-it-named-headings'))
```