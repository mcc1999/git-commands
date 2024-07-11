## 1. 初始化仓库

```Shell
git init
```




## 2. 全局或本地的用户信息

```Shell
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

git config user.name "Your Name"
git config user.email "your.email@example.com"
```


```Shell
git config --global --get user.name
git config --global --get user.email

git config --get user.name
git config --get user.email
```




## 3. 远程仓库

```Shell
git remote
```


```Shell
git remote -v
```


```Shell
# git remote add <name> <url>
git remote add origin https://github.com/username/repository.git
```


```Shell
# git remote remove <name>
git remote remove origin
```


```Shell
# git remote rename <old name> <new name>
git remote rename origin upstream
```


```Shell
# git remote set-url <remote> <newurl>
git remote set-url origin https://github.com/username/new-repository.git
```




## 4. 拉取代码

```Shell
# git fetch <remote> <branch>
git fetch origin master
```


```Shell
# git pull <remote> <branch>
git pull origin master
```




## 5. 推送代码

```Shell
# git push <remote> <branch>
git push origin master
```


```Shell
# git push -u <remote> <branch>
git push -u origin master

# 后续简化拉取/推送命令
git pull
git push
```




## 6. 合并代码

  1. **`Fast-forward merge`**

    使用场景：

    - 从`master`切出一个dev分支；

    - `dev`上新增一个或多个`commit`；

    - `master`上无新增`commit`

    - 这时切回`master`，`git merge dev`时，才是`fast-forward merge`

    结果：

    - `master`的指针，只是直接移动到`dev`最新的`commit`上

    ```Shell
git merge <branch>
```


    ![image.png](https://flowus.cn/preview/c5cd1d2e-3bfe-4a96-a390-7ee6016062b6)

  2. `True merge， Non-fast-forward merge`

    使用场景：

    - 从`master`切出一个`dev`分支；

    - `dev`上新增一个或多个`commit`；

    - `master`上无论是否新增`commit`

    - 这时切回`master`，`git merge --no-ff dev`时，会新增一个`commit`

    结果：

    - 新增的`commit`的两个`parent`是`master`和`dev`最新`commit`

    - `master`的指针，只是指向新增的`commit`上，dev还在自己新增的`commit`上

    ```Shell
git merge --no-ff <branch>
```


    ![image.png](https://flowus.cn/preview/766fb30f-9d52-4427-a716-2c8e9fbcad62)

  3. `Squash merge`

    使用场景：

    - 从`master`切出一个`dev`分支；

    - `dev`上新增一个或多个`commit`；

    - `master`上无论是否新增`commit`

    - 这时切回`master`，`git merge --squash dev`时

    - `dev`新增的所有`commit`会全部出现在`master`的暂存区

    - `master`需要手动`commit`一次，将`dev`所有`commit`合成一个`commitX`

    结果：

    - `commitX`的`parent`只有`master`的上一个`commit`

    - `master`上新增一个`commitX`，指针指向`commitX`

    - `dev`上新增多个`commit`

```Shell

git merge --squash <branch>
```


![image.png](https://flowus.cn/preview/12ec4352-af37-4573-9923-b9dece220c3a)



## 7. 变基操作Rebase

  使用场景：

  - 从`master`切出一个`dev`分支；

  - `dev`上新增一个或多个`commit`；

  - `master`上已经新增一个或多个`commit`

  - 为使得master在merge dev时，仍是Fast forward，dev分支需要变基

  结果：

  - `dev`变基完后，`master`再`merge` `dev`分支时，就可以进行`Fast forward merge`

  注：Rebase操作不要再主分支上操作，会影响从主分支分出去的其他分支；

  注：`-i` 选项能对当前分支的commit做更详细的操作，`reword`、`squash`、`drop`等

  ```Shell
git rebase <branch>
```


  ![image.png](https://flowus.cn/preview/7b4822ec-bf0e-4a24-8390-5980868c361f)



## 8. 代码提交

  本地代码会存在于三个区：`工作区`、`暂存区`、`本地仓库`

  - `工作区`-->`暂存区`

    ```Shell
git add <filename>
```


    ```Shell
git add .
```


  - `暂存区`-->`工作区`

    ```Shell
git restore --staged <filename>
// 或
git reset <filename>
```


    ```Shell
git restore --staged .
// 或
git reset
```


  - `暂存区`-->`本地仓库`

    ```Shell
git commit
```


  - `本地仓库`-->`暂存区`

    ```Shell
git reset --soft <commit id>
```


    ```Shell
git reset --soft HEAD~N
```


  - `本地仓库`-->`工作区`

    ```Shell
git reset --mixed <commit id>
```


    ```Shell
git reset --mixed HEAD~N
```


  - `本地仓库`直接删除回滚到某次commit，不可撤销

    ```Shell
git reset --hard <commit id>
```


    ```Shell
git reset --hard HEAD~N
```




## 9. 选择commit合并Cherry-pick

  使用场景：

  - 从`master`切出一个`dev`分支；

  - `dev`上新增`commitAcommitBcommitCcommitD`；

  - `master`只要合并单个或其中部分`commitBcommitC`

  ```Shell
git cherry-pick commitC
```


  ```Shell
git cherry-pick commitB commitD
```


  ```Shell
git cherry-pick commitB^..commitD
```

