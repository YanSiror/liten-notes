### 1 Git 同时绑定 GItee 与 Github 进行 push 操作

#### 1) 查看远程仓库

**查看下当前项目的远程仓库**

```git remote
git remote
```



**查看绑定的远程仓库**

>  该命令会显示读写远程仓库的名称和地址，指向仓库为 Github。

```cmd
git remote -v
>>>
origin  https://github.com/yanSir/demos.git (fetch)
origin  https://github.com/yanSir/demos.git (push)
```



#### 2) 远程仓库重命名

**修改远程仓库名称**

```cmd
git remote rename <old_remote> <new_remote>
git remote rename origin github
```



**查看远程仓库**

```cmd
git remote -v
>>>
github  https://github.com/gozhuyinglong/blog-demos.git (fetch)
github  https://github.com/gozhuyinglong/blog-demos.git (push)
```




#### 3) 添加另一个远程仓库

**添加远程仓库命令**

> 添加Gitee上的远程仓库，在Gitee上创建一个同名的仓库,在【克隆】处复制地址。

```cmd
git remote add <remote> <url>
git remote add gitee https://gitee.com/gozhuyinglong/blog-demos.git
>>>
gitee   https://gitee.com/gozhuyinglong/blog-demos.git (fetch)
gitee   https://gitee.com/gozhuyinglong/blog-demos.git (push)
github  https://github.com/gozhuyinglong/blog-demos.git (fetch)
github  https://github.com/gozhuyinglong/blog-demos.git (push)
```




#### 4) 多个远程仓库的推送/拉取
**多个远程仓库 push 与 pull 的命令** 

```cmd
git push <remote> <branch>、git pull <remote> <branch>：
git push github main
git pull github main
git push gitee main
git pull gitee main
```


#### 5) 移除一个远程仓库
**移除远程仓库命令**

> 执行移除远程仓库后，该仓库在本地的所有分支与配置信息也会一并删除。

```cmd
git remote remove <remote> | git remote rm <remote>
git remote remove gitee
```





### 2 Github 挂代理之后无法 push 的问题

#### 问题描述

> 使用代理之后 push 代码提示
>
> `unable to access 'xx.git': Failed to connect to chromium.googlesource.com port 443: Timed out`

#### 解决办法

> 清空代理, 重新 push

```cmd
git config --global --unset http.proxy
git config --global --unset https.proxy
```

> 如果无效, 删除 github remote 链接, 重新加入 remote 链接, 然后重新 push

```cmd
git remote remove <remote-name>
git remote add <remote-name> <url>
```

