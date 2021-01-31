---
title: SVN
date: 2020-07-20 15:50:39
tags: 版本控制
---
#### 1.SVN常见操作：
<!-- more -->
- a.发布项目（share project）：项目组长将本机的项目第一次发布到中央仓库。
- b.下载项目（检出项目、check out）：组员将中央仓库的项目第一次下载到本地。
- c.提交（commit）：将本地修改的内容，同步到服务器中（本地-->服务器）。

    编写完某一个小功能、每天下班前提交
- d.更新（updata）：将服务器中的最新代码同步到本地，（服务器-->本地）。

    编写功能之前、每天上班前更新
> 注意：编写之前先更新、写完之后立即提交。

> 提交、更新：一定要及时。
#### 2.svn安装：
- 下载地址：https://sourceforge.net/projects/win32svn/
- 配置环境变量Path(bin)
- 验证是否成功：cmd中输入 svn --version
#### 3.设置中央仓库
- 将本地目录设置为 中央仓库（用于保存项目的各个历史版本）
- cmd中输入：svnadmin create E:\\\\svn
#### 4.启动svn服务
- a.命令行方式：
        svnserve -d -r E:\\\\svn
- b.注册系统方式（推荐）：

    以管理员方式运行cmd

    sc create 服务名 binpath= "F:\My direction\Subversion\bin\svnserve.exe --service -r E:\\\\svn" start= auto depend= Tcpip

    注意：等号后面的空格

    启动：sc start 服务名

    关闭：sc start 服务名

    删除：sc delete 服务名
#### 5.访问项目
- a.匿名访问
    仓库\\..\conf\svnserve.conf

    开启匿名访问：19行附近

    (注意：一定要顶格写，不要留空格)

    anon-access = read 只读

    anon-access = write 可读可写
        
    anon-access = none 无权做任何操作
        
    一般为none
- b.授权访问
    svnserve.conf

    20行附近 auth-access = write 注释打开

    27行附近 password-db = passwd 注释打开（表示 授权人的用户名密码 存放在 passwd 文件中）

    36行附近 authz-db = authz 注释打开（表示权限文件是 authz）

    svnserve.conf

    编写用户文件：

    passwd中[user]下编写  用户名=密码

    编写授权文件authz:

    分组：[groups] dev=zs,ls

    权限

    [/]

    @dev=rw

    *= 
#### 6.Eclipse中使用SVN
在eclipse中安装插件

- a. 离线方式

    eclipse_svn_site-1.10.5.zip 解压到 eclipse\dropins
- b. 在线方式
    help-eclipse marketplace 搜：subversion/subeclipse
- c. 使用：

    项目组长：发布项目

    右键要发布的项目-team-share project - svn - ... - 输入发布的地址 svn://ip地址

    真正的发布/提交

    组员：检出项目（下载）
        
    file - import - 搜 svn

    更新:右键待更新的文件/项目： team-更新
    
    提交：右键带提交的文件/项目： team-提交

    黄色圆柱：本地无未提交的代码

    */灰色箭头：本地有未提交的代码

    红色叹号：冲突

    蓝色箭头：服务端有最新代码，本地还没有更新

    修改svn用户名密码：

    删除C:\users\adm..\Appdata\Roaming\Subversion\auth

    冲突：

     右键项目 - 与资源库同步

     选中 有红色标识的文件，右键 - 编辑冲突 - 修改 - 右键 - team - 编辑为解决

     冲突： 更新时或提交时 发现冲突 - 右键编辑冲突 - 更新提交
#### 7. 恢复/查看历史版本

* 选中需要恢复/查看的文件 - team - 如果要恢复成历史版本（获取内容），如果此操作报错：解决方法：需要将svnserve.conf 文件中的 anon-access=none
#### 8. 将SVN发布到外网

- a. nat123等软件 将内网映射成外网
- b. 租一台互联网服务器（新网、万网、阿里云），将项目发布到服务器中
- c. svn托管网站 http://www.svnchina.com/ 
