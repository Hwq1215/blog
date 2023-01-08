# Git
分布式的版本控制工具
## 将库拉取下来,此时会自动建立仓库
```sh
git pull "herf"
``` 
## 建立一个仓库

```sh
cd ./target/master
git init  # 在master下建立仓库
```

## 创建分支
```sh
git branch 'new_branch_name'
```

## 切换本仓库的分支
```sh
git branch 'exist_barch_name'
```

## 设置远程源
```sh
git remote add -t "barch_name" "remote_name" "remote_url" #新增加一个远程源
git remote set-url "remote_name" "newurl"
git remote remove "remote_name"
```

## 每次提交的方法
```sh
git add . #全部文件都放到commit中
git commit #提交到仓库
git push  #提交到remote，通常是一个默认remote
git push "remote_name' 指定一个remote 
```