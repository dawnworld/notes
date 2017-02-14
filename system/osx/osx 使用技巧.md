OSX 使用技巧
---
### 1. OSX 10.12 中文件已损坏的处理方法
```bash
xattr -cr /Applications/BetterZip.app/
```

### 2. OSX 10.12 Home End 键
```bash
Home : Command + Left Array
End  : Command + Right Array
```

### 3. OSX Sublime Text3 从命令行启动
修改 ~/.bash_profile
```bash
# edit ~/.bash_profile
alias subl='/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl'

# after edit
. ~/.bash_profile
```