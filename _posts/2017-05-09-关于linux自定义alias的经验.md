---
layout: default
title: 关于在linux/mac下自定义命令alias并永久生效的经验
comments: true
---
## 关于在linux/mac下自定义命令alias并永久生效的经验

用linux或者mac做开发的时候，总会遇到一些常用的但是比较长的指令，输入起来很耗费时间，这种情况下我们可以自己定义一个别名来代表这段命令，如：

```
alias gpush='git push origin master'
```

这样我们每次要提交代码时，只需要在终端输入gpush即可，但是使用这种方法定义的命令，在终端关闭后就会失效，那要怎么保存起来使它永久生效呢？

解决办法是在~/.bashrc文件中添加我们需要的自定义命令，首先我们可以去到～目录下看看是否存在.bashrc文件，输入：

```
cd ~
ls -a
```

如果找不到，那我们就自己创建一个即可，然后用vi编辑，注意这里要编辑.开头的文件需要root权限，因此在其他用户登录的状态下需要sudo：

```
touch ~/.bashrc
sudo vi ~/.bashrc
```
进去后，把这段命令写入文件中：

```
alias gpush='git push origin master'
```
然后退出，并在终端输入：

```
source ~/.bashrc
```
使其生效，这样就完成了。

如果执行完以上步骤，还是无法达成关闭终端重启后仍然可以使用命令，那是因为缺少了~/.bash_profile文件，这个文件是用户登录终端的时候会自动执行的文件，因此我们可以在这个文件中执行使.bashrc生效的命令，如下：

```
touch ~/.bash_profile
sudo vi ~/.bash_profile
```

在文件中加入：

```
source ~/.bashrc
```

大功告成。