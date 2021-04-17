# BIMK CPU服务器使用说明
[EN](../en/BIMK-Server-HandBook-en.md)
| 维护人员      | 最后一次维护时间 | 文档状态 |
| ----------- | ----------- |-----------|
| @DestinyMy @Anonymone| 2021.04.17| 有效|

## 1. 连接远程服务器
1）命令提示符输入`mstsc`，弹出远程桌面连接<br>
2）输入CPU服务器IP地址和用户名、密码（用户可自行修改密码）

## 2. 常用软件
1）Matlab 2020b已经安装，双击D:\Program Files\Polyspace\R2020b\bin\matlab.exe即可运行<br>
2）Java已经安装JDK1.8版本。（命令行测试可运行）<br>
3）Eclipse已安装在（C:\Program Files\java-photon\eclipse）目录下

## 3. 常见问题
### 1）登录时出现身份验证错误，要求的函数不受支持（可能由于CredSSP加密数据库修正）
命令提示符输入`gpedit.msc`（仅支持windows专业版）进入本地组策略编辑器，依次进入计算机配置>管理模板>系统>凭据分配>加密数据库修正，选择保护级别易受攻击，点击应用，再次登录即可
### 2）文件传输
两种方案可以选择：<br>
1）从本地（远程桌面）使用鼠标右键复制，在远程桌面（本地）使用鼠标右键粘贴。<br>
2）在远程桌面连接框中进入显示选项>详细信息，勾选对应驱动器即可将本地驱动器连接到远程桌面

## 4. 注意事项
### 1）每个用户的文件必须保存在E盘下，可在E盘建立相应目录存放文件
### 2）所有用户不得私自安装、卸载软件，不可将本地软件整个目录打包传至服务器，需要安装软件请与管理员联系。



# BIMK GPU服务器使用说明

## 1. 平台
BIMK有两台GPU服务器，一台服务器内含4块Tesla P100 GPU，运行在Centos 7系统上，已安装cuda 10.1；另外一台服务器内含2块Nvidia 3090 GPU，运行在Ubuntu 18.04桌面版系统上，已安装cuda 11.0。

## 2. 创建账号
由于安全性考虑，我们不支持用户自行注册账号，有创建账号的需求请联系管理员。
得到使用许可之后，你可以使用`ssh`命令登录服务器，具体命令如下：

```
ssh USER_NAME@IP -p PORT
```
用户可使用`passwd`命令自行修改用户密码

## 3. 文件传输
服务器上有单独的空间用于存放数据集，比如CIFAR，ImageNet等，方便用户之间共享。Centos 7上存放数据的目录为`/data/`，Ubuntu 18.04上为`/home/data/`，请尽量将数据上传至相应的目录。
### 1）scp、pscp
可使用`scp`、`pscp`命令进行文件的上传和下载，具体格式如下（`scp`和`pscp`格式一样）：
上传：
```
scp [-r] D:\files USER_NAME@IP:/home/USER_NAME/ 
```
下载：
```
scp [-r] USER_NAME@IP:/home/USER_NAME/files D:\
```
### 2）Winscp
`Winscp`是windows环境下使用SSH的开源图形化SFTP客户端，同时支持SCP协议，主要功能是在本地和远程服务器之间安全的进行文件传输。下载地址：https://winscp.net/eng/docs/lang:chs

## 4. 运行实验
请务必在虚拟环境下进行实验，服务器预装`anaconda3.7`。之后会考虑使用docker或者singularity等
### 1）conda
`anaconda`自带，常用命令有：<br>
`conda list`  # 查看安装的包<br>
`conda env list`  # 查看当前有哪些`conda`虚拟环境<br>
1）创建虚拟环境

```
conda create --prefix=/home/USER_NAME/ENV_NAME python=3.x
```
2）激活虚拟环境

```
conda activate /home/USER_NAME/ENV_NAME
```
3）对指定虚拟环境安装包
```
conda install -n ENV_NAME [package]
```
4）卸载指定虚拟环境的包
```
conda remove --name ENV_NAME [package]
```
5）关闭虚拟环境
```
conda deactivate
```
6）删除虚拟环境
```
conda remove -n ENV_NAME --all
```
### 2）virtualenv
1）安装`virtualenv`
```
pip install virtualenv
```
创建 的虚拟环境在当前目录下<br>
2）创建虚拟环境
```
virtualenv ENV_NAME
```
3）激活虚拟环境

```
source ENV_NAME/bin/activate
```
4）对指定虚拟环境安装包
激活对应的虚拟环境后，运行以下命令即可在对应的虚拟环境安装包
```
pip install [package]
```
5）卸载指定虚拟环境的包
激活对应的虚拟环境后。运行以下命令即可在对应的虚拟环境卸载包
```
pip uninstall [package]
```
6）关闭虚拟环境

```
deactivate
```
7）删除虚拟环境
直接删除虚拟环境对应的文件夹即可（也可使用`virtualenvwrapper`）
