### 个人博客
----
#### 安装 **Hexo**
```
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server
```
----
#### 自动化部署配置（**Travis CI**）
_config.yml:
> deploy:    
>>    type: git   
>> repository: https://github.com/CoolLittle/blog.git  
>> branch: gh-pages
>> message: 部署 {{now("YYYY-MM-DD HH:mm:ss")}}    
----
#### 运行命令：
```
$ npm install hexo-deployer-git --save
## 生成
$ hexo g
## 部署
$ hexo d
```
----
#### 访问
博客首页： <https://coollittle.github.io/blog/>