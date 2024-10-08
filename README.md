# ADS-B 9G feed client

- These scripts aid in setting up your current ADS-B receiver to feed ADS-B 9G.
- They will not disrupt any existing feed clients already present

## 1: Find coordinates / elevation:

<https://www.freemaptools.com/elevation-finder.htm>

## 2: Install the adsb9g feed client

```
curl -L -o /tmp/axfeed.sh https://raw.githubusercontent.com/rkarikari/adsb-9g/master/feed.sh
sudo bash /tmp/axfeed.sh
```

## 3: Check  if your feed is working




 

```
sudo systemctl status adsb9g-feed


sudo systemctl status adsb9g-mlat
```

###  local interface for your data http://192.168.X.XX/tar1090

Install / Update:
```
sudo bash -c "$(wget -nv -O - https://github.com/rkarikari/tar1090/raw/master/install.sh)"
```
Remove:
```
sudo bash -c "$(wget -nv -O - https://github.com/rkarikari/tar1090/raw/master/uninstall.sh)"
```

### Update the feed client without reconfiguring

```
curl -L -o /tmp/axupdate.sh https://github.com/feed-update.sh
sudo bash /tmp/axupdate.sh
```


### If you encounter issues, please do a reboot and then supply these logs on the forum (last 20 lines for each is sufficient):

```
sudo journalctl -u adsb9g-feed --no-pager
sudo journalctl -u adsb9g-mlat --no-pager
```


### Display the configuration

```
cat /etc/default/adsb9g
```

### Changing the configuration

This is the same as the initial installation.
If the client is up to date it should not take as long as the original installation,
otherwise this will also update the client which will take a moment.

```
curl -L -o /tmp/axfeed.sh https://adsb9g.com/feed.sh
sudo bash /tmp/axfeed.sh
```

### Disable / Enable adsb9g MLAT-results in your main decoder interface (readsb / dump1090-fa)

- Disable:

```
sudo sed --follow-symlinks -i -e 's/RESULTS=.*/RESULTS=""/' /etc/default/adsb9g
sudo systemctl restart adsb9g-mlat
```
- Enable:

```
sudo sed --follow-symlinks -i -e 's/RESULTS=.*/RESULTS="--results beast,connect,127.0.0.1:30104"/' /etc/default/adsb9g
sudo systemctl restart adsb9g-mlat
```

### Other device as a data source (networked standalone receivers):

https://github.com/adsbxchange/wiki/wiki/Datasource-other-device

### Restart

```
sudo systemctl restart adsb9g-feed
sudo systemctl restart adsb9g-mlat
```


### Systemd Status

```
sudo systemctl status adsb9g-mlat
sudo systemctl status adsb9g-feed
```


### Removal / disabling the services:

```
sudo bash /usr/local/share/adsb9g/uninstall.sh
```

If the above doesn't work, you may be using an old version that didn't have the uninstall script, just disable the services and the scripts won't run anymore:

```
sudo systemctl disable --now adsb9g-feed
sudo systemctl disable --now adsb9g-mlat
```
