![socksify-me](extra/logo.png)

## **Dependencies**
* iproute2
* [tun2socks](https://github.com/xjasonlyu/tun2socks)

## **Installation**
If you are using ArchLinux then:

``` 
$ git clone https://github.com/BigmenPixel0/socksify-me.git
$ cd socksify-me/archlinux
$ makepkg -sric
``` 
Or use your AUR helper.

If you are not using ArchLinux then download the latest release and copy the script to /usr/local/bin/ and give it execute permission.

## **Usage**
```
$ socksify-me --help

socksify-me - A simple script that socksifies your traffic

Usage: socksify-me start|stop [options]

Required options:

  --server-ipv4		IPv4 address of the server to which the socks5 client connects (conflicts: --uid)
  --server-ipv6         IPv6 address of the server to which the socks5 client connects (conflicts: --uid)
  --uid			ID of the user who is running the socks5 client (conflicts: --server-ipv4 and --server-ipv6)
  --socks5-port		Local socks5 port
  --net-device		Your network device
  ```
Example:
```
socksify-me --uid 43 --socks5-port 9050 --net-device wlan0
```

## **Integration with NetworkManager-dispatcher**
If you want the script to run itself when the network is up, then:
```
$ sudoedit /etc/NetworkManager/dispatcher.d/socksify-me

#!/usr/bin/env bash

interface=$1 status=$2
case $status in
  up)
    socksify-me start --server-ipv4 CHANGE_ME --server-ipv6 CHANGE_ME --uid CHANGE_ME --socks5-port CHANGE_ME --net-device $interface
    ;;
  down)
    socksify-me stop
    ;;
esac

$ sudo chmod +x /etc/NetworkManager/dispatcher.d/socksify-me
```
You need to select the `--uid` option or `--server-ipv4` and/or `--server-ipv6`

## **--uid or --server-ipv4/6?**
The `--uid` option is more flexible because all traffic of the selected user will be excluded from the proxy (for example, so that your socks5 client can easily connect to a domain with a changing IP address). It is necessary to run the socks5 client as the selected user.

The `--server-ipv4/6` option is simpler, you only need to enter the IP address of your socks5 server, but this will not work with a domain that has IP addresses that change or with TOR.
