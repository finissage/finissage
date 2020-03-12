#  On my Zsh
> 项目托管地址：https://github.com/robbyrussell/oh-my-zsh

## 最终效果
![](https://upload-images.jianshu.io/upload_images/2411388-b39ceb1c2dc5a569.png)

# 安装

## 通过 curl 安装
```shell
sh -c “ $（ curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh ） ”
```

## 通过 wget 安装
```shell
sh -c “ $（ wget -O- https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh ） ”
```

## 查看当前是哪个 shell
```shell
echo $SHELL
```

## 查看当前支持的所有 shell
```shell
cat /etc/shells
```

## 修改默认的 shell
```shell
chsh -s 哪个zsh
```

## 配置文件
```shell
vim ./zshec 

# 主题
ZSH_THEME=""

# 插件
plugins=(git)
```

> 主题： https://github.com/robbyrussell/oh-my-zsh/wiki/Themes  
> 插件： https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins

> 外部主题：https://github.com/robbyrussell/oh-my-zsh/wiki/External-themes  
> 外部插件： https://github.com/robbyrussell/oh-my-zsh/wiki/External-plugins