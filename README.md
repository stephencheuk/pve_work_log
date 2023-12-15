# PVE 8.0

- apt-get dist-upgrade
- apt-get install xfce4 chromium lightdm
- systemctl enable lightdm
- systemctl start lightdm

# Gitlab

## 一、GitLab安装
服务器配置4C8G，Ubuntu22.04。
```
#切换到root用户
sudo su -

#更新
apt update
apt upgrade -y

#安装和配置必须的依赖项
apt install build-essential curl file git ca-certificates -y

#配置极狐GitLab 软件源镜像
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

#下载GitLab安装包
wget --content-disposition https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/bionic/gitlab-ce_14.1.2-ce.0_amd64.deb/download.deb

#安装
dpkg -i gitlab-ce_14.1.2-ce.0_amd64.deb
```

## 二、配置Gitlab
```
#加载配置文件
gitlab-ctl reconfigure

#配置gitlab地址，修改external_url为'http://服务器ip地址:端口'
vim /etc/gitlab/gitlab.rb
external_url 'http://192.168.31.20:2080'

#重新加载配置文件
gitlab-ctl reconfigure

#重启服务
gitlab-ctl restart

#查看gitlab服务状态
gitlab-ctl status
```
## 三、放行gitlab服务端口
```
#放行gitlab服务https、http
ufw allow https
ufw allow http

#我这里配置的是2080端口
ufw allow 2080

#开启ufw
ufw enable

#查看ufw状态
ufw status
```
## 四、修改root密码
Gitlab默认账号用户名为root，密码使用下面命令获得：

登录账户：root
密码：QfYw3GVeAgQwx/2KW6bLJejwQIZTbBn2dPkfhPX2Pwo=

```
#查看第一次安装GitLab生成的默认密码
cat /etc/gitlab/initial_root_password
Password: QfYw3GVeAgQwx/2KW6bLJejwQIZTbBn2dPkfhPX2Pwo=
```

此时访问http://192.168.31.20:2080进行登录并修改密码。因为Gitlab很内存，貌似需要4G多，建议给8G，它启动也有点慢，如果出现502可以耐心等待一下就好啦。

![image](https://github.com/stephencheuk/pve_work_log/assets/11772697/9a76803c-4af6-446e-b62c-1cf422ad259e)
