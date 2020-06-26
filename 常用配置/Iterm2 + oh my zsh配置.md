# Iterm2 + oh my zsh

- 官网下载iTerm2 https://www.iterm2.com/ -> 点击安装 -> 安装完成。

- 安装oh my zsh (github 地址：https://github.com/ohmyzsh/ohmyzsh) 
  curl 安装，shell中输入：

  ```bash
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```

- 设置iTerm2的字体样式，iTerm2 -> Preferences -> Profiles -> Text -> change font(18,monaco)。

- 安装字体Meslo，[https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf](https://github.com/powerline/fonts/blob/master/Meslo Slashed/Meslo LG M Regular for Powerline.ttf)，点击 view raw 下载，安装成功后重复3步骤更改字体样式

- 安装zsh自动补全插件

  ```bash
  cd ~/.oh-my-zsh/custom/plugins
  git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
  ```

- 添加zsh自动补全插件

  ```bash
  cd ~
  vim .zshrc
  ```

  把plugins中添加zsh-autosuggestions， 例如plugins=(zsh-autosuggestions git)，修改成功后执行source ~/.zshrc或重启 shell 使配置生效

- 安转及添加zsh-syntax-highlighting

  ```bash
  cd ~/.oh-my-zsh/custom/plugins/
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
  cd ~
  vim .zshrc
  ```

  将zsh-syntax-highlighting添加进 plugins，如plugins=(zsh-autosuggestions zsh-syntax-highlighting git)

  修改成功后，执行source ~/.zshrc或重启 shell 使配置生效

- （可选）shell中vim highlight及配置行号

  ```bash
  vim ~/.vimrc
  ```

  修改成以下内容，然后保存即可自动生效
  ![img](https://img2020.cnblogs.com/blog/1092972/202006/1092972-20200625181504991-1557940871.png)