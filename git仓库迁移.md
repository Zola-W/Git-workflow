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

