```
curl https://packagecloud.io/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packagecloud.io/grafana/stable/debian/ stretch main"
sudo apt-get update
apt-cache policy grafana
sudo apt-get install grafana
sudo systemctl enable grafana-server
sudo vi /etc/grafana/grafana.ini
```

`grafana.ini` file changes

```
[auth.anonymous]
enabled = false
# disable user signup / registration
allow_sign_up = false
```

