### 1. linux怎样运行自启动脚本？

（1）crontab -e

```
@reboot echo "hi" > /usr/reboot.txt 2>&1
```

（2）reboot

（3）cat /usr/reboot.txt