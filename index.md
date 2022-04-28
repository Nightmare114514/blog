### 欢迎来到Nightmare114514的博客

### [点此处传送至主站](nightmare114514.github.io)

炮姐镇楼(＿`·ω·′)＿

<img src="炮姐.png" alt="炮姐.png" title="炮姐天下第一！">

### [20220424] 如何给你的路由器刷机？

路由器刷机也不是特别难，这里使用红米AX6S进行演示，其他的路由器也大同小异。
  
准备材料:路由器×1
       电脑或者手机×1
       Telnet和SSH的连接工具×1（电脑推荐termius，手机推荐juicessh，最好还是用电脑吧，手机到了传固件那一步会很难，电脑上拖一下就好了，我用的手机，最后只能用一个非常野鸡的方案，用手机搭建一个临时的文件服务器，然后curl到路由器上）
       路由器固件×1（可以去[恩山无线论坛找](https://www.right.com.cn/)）
       聪明的脑袋和灵活的手×1
         
固件下载：[https://www.right.com.cn/forum/thread-8187405-1-1.html](https://www.right.com.cn/forum/thread-8187405-1-1.html)

首先要刷入红米AX6S的开发版固件以便开启telnet，固件在上面的链接里，下载下来在路由器后台选择升级，等待一会即可。

接着打开你使用的终端软件，使用telnet链接，IP为你路由器的IP，用户为root，密码需要在[https://www.oxygen7.cn/miwifi/](https://www.oxygen7.cn/miwifi/)
输入你的SN码（可以在路由器底下的贴纸处找到），点击GO，所得的就是你的密码。

进入Telnet后，输入下面的命令即可打开SSH。

```markdown
nvram set ssh_en=1 & nvram set uart_en=1 & nvram set boot_wait=on & nvram set bootdelay=3 & nvram set flag_try_sys1_failed=0 & nvram set flag_try_sys2_failed=1
```

``` markdown
nvram set flag_boot_rootfs=0 & nvram set "boot_fw1=run boot_rd_img;bootm"
```

```markdown
nvram set flag_boot_success=1 & nvram commit & /etc/init.d/dropbear enable & /etc/init.d/dropbear start
```

打开SSH后，退出现在的Telnet会话，新建一个SSH会话，IP还是你路由器的IP，密码还是你Telnet的密码。

进入SSH后，先把固件中的factory.bin上传到路由器，接着输入一下命令进行底包刷写。

```markdown
mtd -r write /tmp/factory.bin firmware
```

刷写完成后，你就会看到一个SSID为Openwrt开头的WIFI，连接它，默认IP为192.168.6.1，用户为root，密码为password，即可进入路由器管理界面。

重启路由器，然后使用固件包里的mt76.bin或者miwifi.bin或者openmt76.bin进行不保留配置更新。
注:openmt76.bin和mt76.bin是开源固件，miwifi.bin是闭源固件，开源固件会有一些新特性，闭源固件相较于开源固件会更稳定但可能会缺少一些新特性（比如WPA3）。

更新完之后重启路由器，Enjoy！

提示：更改无线设置后最好重启路由器，否则可能会不稳定。

### [点此处传送到评论区](https://github.com/Nightmare114514/blog/issues)
