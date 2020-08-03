#### 解决Mac系统下vscode中vim插件无法连续移动的问题

1. 首先是在终端中输入如下命令

   ```shell l
   defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false
   ```

   注意执行之后需要重启vscode

2. 如果要解除上面命令只需要执行如下命令

   ```shell
   defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool true
   ```

上面内容来自这个[回答](https://stackoverflow.com/questions/39972335/how-do-i-press-and-hold-a-key-and-have-it-repeat-in-vscode)

