---
title: "Git_note"
date: 2024-09-29T15:43:12+08:00
type: "post"
showTableOfContents: true
---
## git fetch
```bash
# 拉取
git fetch origin master
git log -p master..origin/master #查看本地master和origin master的差异
git merge origin/master
```

前面两步操作相当于：git pull

## 查看单个文件的修改历史
```
git log -p filename
```

## 暂存操作
```
# 暂存
git stash save "备注"

# 查看暂存记录
git stash list

# 恢复指定 id 的 stash 内容，但不会删除缓存条目
git stash apply stash@{id}

# 恢复缓存，且删除缓存条目
git stash pop

# 删除指定缓存
git stash drop stash@{id}

# 删除所有缓存
git stash clear
```

## 推送本地分支到远程
```
git push -u origin sub_branch
```

和另一种方法
```
git push --set-upstream origin newbranch
```

## 拉取远程分支，并切换到新拉取的分支
```
git checkout -b feature-branch origin/feature-branch
```


## 本地仓库和 github 仓库如何关联
- 在本地生成 ssh 密钥对;
	```
	ssh-keygen -t ed25519 -C 'youremail@xxx.com'
	```
- 打开 id_ed25519.pub 文件，复制其内容;
	把文本内容复制到 github 中，
	{{< figure src="imgs/ssh_github.png" width="70%" >}}
	添加成功后推送代码就不需要每次都输入账号和密码了。
	
- 本地仓库和远程仓库关联
	```
	git remote add origin 'your_github_repository.git'
	```
	- note1: 
		由于 Github 上创建的主分支名称为 main，而本地创建的仓库主分支名称为 master。
		需要把本地分支的名称从 master 改为 main
		```
		git branch -m master main
		```
	- note2:
		由于远程服务器上已有提交（创建的 readme 文件），本地的仓库也有提交，此时 pull 和 push 会失败。
		```
		git pull --rebase origin main
		```
		最后，就可以 pull 拉取远端代码， push 推送本地代码到远端了。

		pull 的时候可能会要求你输入 yes,直接输入 yes 就可以了
		
## github 仓库可见性转换
	可见性从 private 改为 public
	
	进入 github 仓库的主页，点击仓库的 setting 按钮;
	
	{{< figure src="imgs/change_visible.png" width="70%" >}}
	跳转到 setting 按钮;
	
	关掉 protection rules 点击 Change repository visibility  就可以了;
	
	

