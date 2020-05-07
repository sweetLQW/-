# git的配置
## 配置（如用户名和邮箱）
git config [--local | --global | --system] user.name 'your name'
git config [--local | --global | --system] user.email 'your email'

## 查看配置
git config --list [--local | --global | --system]

## 清除配置
git config --unset [--local | --global | --system] user.name

### local、global、system的作用范围（优先级依次递减）
local：本仓库，路径在：仓库的.git/config
global: 当前登陆用户的所有仓库（常用)，路径在：路径在：~/.gitconfig
system: 本系统的所有用户，路径在：/etc/gitconfig

# git的仓库
## 建立仓库，git init的两种方式
cd project文件夹 && git init
or
git init project文件夹

## 工作区、暂存区、版本库
git add .  将文件添加到暂存区（或者git add -u，只包含被git跟踪的文件，即不包括新建的文件）
git commit -m "your commit message"



