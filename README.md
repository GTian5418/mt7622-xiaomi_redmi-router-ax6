# mt7622-xiaomi_redmi-router-ax6
1.通过MIWIFIRepairTool刷入0-1.2.7.bin  
2.进入管理器后台获取SN  
3.root密码计算:https://miwifi.dev/ssh  
输入小米路由器管理后台首页显示的序列号（SN），点击 Calc 即可计算出密码  
4.MobaXterm_Personal建立telnet连接开启ssh:  
nvram set ssh_en=1 & nvram set uart_en=1 & nvram set boot_wait=on & nvram set bootdelay=3 & nvram set flag_try_sys1_failed=0 & nvram set flag_try_sys2_failed=1  
nvram set flag_boot_rootfs=0 & nvram set "boot_fw1=run boot_rd_img;bootm"  
nvram set flag_boot_success=1 & nvram commit & /etc/init.d/dropbear enable & /etc/init.d/dropbear start  
5.MobaXterm_Personal建立ssh连接:  
上传1-factory.bin到/tmp 后执行  
mtd -r write /tmp/1-factory.bin firmware  
6.MobaXterm_Personal建立ssh连接:  
上传2-factory.bin到/tmp 后执行  
mount -o remount,ro /  
mount -o remount,ro /overlay  
cd /tmp  
dd if=factory.bin bs=1M count=4 | mtd write - kernel  
dd if=factory.bin bs=1M skip=4 | mtd -r write - ubi  
7.进入luci界面升级刷入3-sysupgrade.itb