## Git Commands (Remote)

###本地与远端仓库创建连接

```bash
git remote -v #查看远端仓库配置
git remote add origin https://github.com/darkcar/GitCommands.git

# 向远程仓库进行push
git push origin --all # 上传所有的分支
git push origin remote-b # remote-b 只上传remote-b分支
```

### Git Fetch 命令

```bash
git fetch origin master # 只fetch某个分支
git fetch origin 				# fetch所有的remote分支
git fetch 							# 缺省情况下也是可以的
```
