# 在阿里云centOS7.4上搭建FTP服务。 :rainbow:

搭建FTP真的是一波三折。
可能是一开始没有感觉会很麻烦。:smile:



安装了vsftpd以后，就直接狂配置，关闭匿名权限，直接创建用户。（useradd 用户=> passwd） <br>
然后分配目录可以访问的目录local_root=/home/demo 和权限   chmod 755 目录  <br>
然后这个顺序如果反了，先创建了用户，获得了主目录的写入权限的话， <br>
就会一直被禁止访问。 <br>

<p>
一番折腾，好吧，返回快照。重新开始。:cry:
</p>
<p>
首先，默认下防火墙系统是firewalld 而不是 iptables 而且是默认关闭的,这样对于端口限制问题不用考虑先。<br>
SElinux也是关闭的。
###### 但是，但是！ 阿里云是有安全组的，需要在阿里云上开启20/21端口。
这里直接采用的是主动模式访问FTP，需要在windows访问之前，在Internet选项中关闭 [被动ftp....balabala]的选项。
阿里云上配置的授权如果是0.0.0.0/0表示的是授权所有用户，显然我不会这么干，我只授权了自己电脑的IP。233333
</p>
<p>
然后安装vsftpd，这时候直接试着匿名访问，看能不能进入系统看到Pub目录，如果有，那就有戏啦。
这时候，你要开启匿名的读写功能也可以，但是最好不要这么做。
我就直接进入了创建用户的步骤。
首先是在vsftpd.conf 配置项中进行配置，关闭匿名访问。
开启chroot_local_user= true这一条和下面的两条。
local_root=/home/demo 这里不要有空格，好像他不识别空格诶。   指定登录后上来的主目录。
然后创建用户，useradd  passwd  具体的用户可以在#vim  /etc/passwd 中看到。
在指定的local_root目录下面创建一个开放权限的目录，
这时候试着用用户来访问ftp发现你可以看到进来的demo目录的，但是你还是没有读写权限。
这时候在vsftpd.conf配置项中加入  allow_writeable_chroot=YES 开启读写权限，或者说去移除这个目录下的所有用户权限chmod a-w /home/user。
参考地址[https://www.cnblogs.com/mrcln/p/6179673.html]
</p>
<font color="Hotpink">每次配置完后记得重启一下vsftpd</font>

