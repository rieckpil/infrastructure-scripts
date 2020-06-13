# General

```
// show only security updates
apt-get -s dist-upgrade |grep "^Inst" |grep -i securi

// install kept back updates
sudo apt-get update && sudo apt-get dist-upgrade
```