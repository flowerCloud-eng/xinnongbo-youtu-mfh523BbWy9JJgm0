
背景是使用学习版IDM下载器，版本6\.41\.2，地址备份：https://github.com/glucyzz/IDM


下载完成后导入chrome浏览器，但是发现挂了小猫之后浏览器立马就把此插件自动更新到最新版本6\.42\.22了，导致下载时因版本不匹配提示无法完成下载。


网上的方法试过的：


1\.更改浏览器目录下Extensions中插件ID文件夹中文件夹名为最新版本号，并更改manifast.js文件等


　　试过之后发现该方法已被chrome绕开，你更新文件夹名称为6\.42\.22\_0（他第一次生成的文件名直接复制），第二次他仍然会下载最新版本并把文件名命名为6\.42\.22\_1


2\.更改文件夹属性为只读


　　更新后发现只顶用一小会，没细研究chrome是怎么绕过的


3\.更改hosts文件


　　此方法按理说是可行的，但是因为小猫的缘故，转发都先经过了小猫插件，通过小猫插件的规则直接转发了，所以被绕过


 


最后成功方法：


　　使用小猫插件的规则，将chrome更新插件的地址转发改成DIRECT，就能走本地hosts规则了，无法访问更新地址的情况下就无法更新插件了。


　　小猫插件中有个“配置文件预处理”的功能（https://docs.gtk.pw/contents/parser.html\#%E7%89%88%E6%9C%AC%E8%A6%81%E6%B1%82）


　　路径为：Settings——Profiles——Parsers——点击右侧Edit


　　![](https://img2024.cnblogs.com/blog/679123/202501/679123-20250104200105280-1870235908.png)




```
parsers: # array
  - url: 自己的订阅地址
    yaml:
      prepend-rules:
        - DOMAIN,clients.google.com,DIRECT
        - DOMAIN,clients1.google.com,DIRECT
        - DOMAIN,clients2.google.com,DIRECT
        - DOMAIN,clients3.google.com,DIRECT
        - DOMAIN,clients4.google.com,DIRECT
        - DOMAIN,update.googleapis.com,DIRECT
```


　　prepand\-rules是在规则文件的rules之前插入的规则，因为rules里面有DOMAIN\-SUFFIX匹配google.com，所以要用DOMAIN来匹配这些更新地址，并且放在前面，DIRECT意思是不走代理走直连的方式，这样写既可以达成绕过的目的，也可以不被订阅规则更新影响


　　


 本博客参考[PodHub豆荚加速器官方网站](https://rikeduke.com)。转载请注明出处！
