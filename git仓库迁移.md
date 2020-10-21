## GIT仓库完整迁移

### 一、Git仓库完整迁移流程
使用以下方法，可以保留提交的历史信息&&原有分支。

#### 1. 从原地址克隆一份裸仓库，便于节省储存空间
```
git clone --bare https://github.com/oldGroup/oldProject.git
```
--bare 创建的克隆版本库都不包含工作区，直接就是版本库的内容。
#### 2. 在新git服务器上创建新项目
#### 3. 变更远程仓库地址 
```cd oldProject.git
// 方法一：修改远程仓库地址(推荐)
git remote set-url --push origin git@xxx.github.com:newGroup/newProject.git 


// 方法二：先删除远程仓库地址，然后再添加
git remote rm origin git@xxx.github.com:oldGroup/oldProject.git 
git remote add origin url git@xxx.github.com:newGroup/newProject.git

```
#### 4. 以镜像推送的方式将代码上传至新远程服务器上
```
git push --mirror
```
--mirror 克隆出来的裸仓库对上游版本库进行了注册，这样就可以在裸半本库中使用git fetch命令与上游版本保持同步。

#### 5. 查看远程仓库地址
```
git remote -v
```
6. 删除本地代码
```
rm -rf oldProject.git
```
7. 从新git克隆到本地
```
git clone git@xxx.github.com:newGroup/newProject.git
```

### 二、更改现有远程仓库
```
cd OldProject
git remote set-url origin git@xxx.github.com:newGroup/newProject.git 
git push origin branchName
```

### 三、git 克隆各个方法的对比
#### 1. git clone <repository> <directory>
将<repository>指向的版本库克隆到一个<directory>目录。目录<directory>相当于克隆版本库的工作区，文件都会检出，版本库位于工作区的.git目录中。
#### 2. git clone --bare <repository> <directory.git>
该方式克隆下的仓库，不再生成.git目录，而只是生成.git目录下面的版本历史纪录，这些版本历史纪录文件不再存放在.git目录下面，而是直接存放在版本库的根目录下面。


该方式生成的仓库，只是一个裸仓库，只用于记录版本库历史纪录，而不会包含实际项目源文件的拷贝，因此该版本仓库不能称为工作目录，不允许用户在上面进行各种git操作，如果我们我们进行其他操作的话，就会得到诸如“fatal: This operation must be run in a work tree”的提示。


#### 3. git clone --mirror <repository> <directory.git>
 
总结 
方法2和方法3创建的克隆版本都不包含工作区，直接就是版本库的内容，这样的版本库称为裸仓库。一般裸版本库目录名以.git做后缀，所以以该方式克隆出得裸版本库目录名写作<directory.git>。

区别在于方法3克隆出的配置文件具有fetch键，这就意味着可以通过git remote update 或者git fetch --all 与上游版本保持同步。

命令是否生成.git目录是否包含源文件是否可以进行其他git操作可否与上游版本库进行同步git clone <repository> <directory>truetruetruetruegit clone --bare <repository> <directory.git> falsefalsefalsefalsegit clone --mirror <repository> <directory.git> false
falsefalsetrue

