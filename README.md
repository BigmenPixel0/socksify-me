![socksify-me](extra/logo.png)

## Features
* Full IPv4 and IPv6 support.
* Support for exclusion by UID method.
* Support for exclusion method by server IP address.
* Support for custom scripts after start/stop.
* Easy integration with NetworkManager-dispatcher.
* Maybe more...

## Dependencies
* iproute2
* [tun2socks](https://github.com/xjasonlyu/tun2socks)

## Installation
If you are using ArchLinux then:

``` 
$ git clone https://github.com/BigmenPixel0/socksify-me.git
$ cd socksify-me/archlinux
$ makepkg -sric
``` 
Or use your AUR helper.

If you are not using ArchLinux then download the latest release and copy the script to /usr/local/bin/ and give it execute permission.

## Usage
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

Optional options:

  --disable-ipv6	Disable IPv6 (true or false)
  --expert		Run user scripts from /etc/socksify-me/expert-start and /etc/socksify-me/expert-stop (true or false)
```
Example:
```
$ socksify-me --uid 43 --socks5-port 9050 --net-device wlan0
```
Note: you should run `socksify-me` as root.

## Integration with NetworkManager-dispatcher
If you want the script to run itself when the network is up, then:
```
$ sudoedit /etc/NetworkManager/dispatcher.d/socksify-me
```
```
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
```
```
$ sudo chmod +x /etc/NetworkManager/dispatcher.d/socksify-me
```
You need to select the `--uid` option or `--server-ipv4` and/or `--server-ipv6`.

Make sure you have enabled `NetworkManager-dispatcher.service`:
```
$ sudo systemctl enable NetworkManager-dispatcher.service
```

## Options
See [here](https://github.com/BigmenPixel0/socksify-me/wiki/Options).

## Credits
Thanks to [tun2socks](https://github.com/xjasonlyu/tun2socks) developers.
