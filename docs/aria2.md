# Mac 上使用 aria2

- [Mac 上使用 aria2](#mac-上使用-aria2)
  - [下载 aria2](#下载-aria2)
  - [配置修改](#配置修改)
  - [批量下载](#批量下载)

## 下载 aria2

[https://github.com/aria2/aria2/releases](https://github.com/aria2/aria2/releases)

Mac 版本：`osx-darwin.dmg`

## 配置修改

> vim ~/.aria2/aria2.conf

```
#允许rpc
enable-rpc=false

#文件保存路径, 默认为当前启动位置
dir=/Users/wufan/Downloads
```

## 批量下载

> aria2c --input-file=down.txt

down.txt
```
https://vod.lycheer.net/f100030.mp4?tmp=f30.mp4
 out=vagrant/缘起.mp4
https://vod.lycheer.net/f100030.mp4?tmp=f30.mp4
 out=vagrant/修炼.mp4

```
