
密钥
--

    ssh-keygen
    cat ~/.ssh/id_rsa.pub

git初始
-----

    git init  #在打包目录执行git初始化
    #-------------新建一个仓库-------------
    #如果是新提交一个软件包，可以用以下命令clone,aur服务器上将建立一个名为package_name的新仓库 （会有创建仓库的提示信息）
    git clone git+ssh://aur@aur.archlinux.org/package_name
    #然后连接远程仓库
    git remote add origin git+ssh://aur@aur.archlinux.org/package_name
    #-------------获取存在的仓库-----------
    #如果是aur服务器上已经存在的 git 仓库，先连接仓库
    git remote add origin git+ssh://aur@aur.archlinux.org/package_name
    #再从服务器同步下来
    git pull origin master

修改makepkg配置
-----------

`/etc/makepkg.conf查的`

修改`#PACKAGER=`

提交校验
----

    updpkgsums 
    makepkg --printsrcinfo > .SRCINFO #生成源文件信息文件
    git add .SRCINFO PKGBUILD # 提交变动到暂存区
    git commit -m 'some description'     #增加快照
    git push   #推

图标
--

### exe

    yay -S icoextract
    icoextract any.exe any.png

### jar

    jar -xvf any.jar
