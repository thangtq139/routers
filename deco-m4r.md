# Flashing debug firmware
- Do not power on the Deco unit, connect the PC directly into one of the ethernet port of the Deco unit.
- Set a static IP address of 192.168.0.2/255.255.255.0 on the PC’s ethernet adapter.
- On the PC, open a browser, access http://192.168.0.1, you would be able to see the firmware recovery page, you may upload the firmware and start the firmware upgrade, the webpage will tell you once the upgrade is finished.

# Flashing openwrt firmware
- Get the sysupgrade openwrt image for deco-m4-v(1|2)
- Split to kernel.bin and rootfs.bin
```
dd if=openwrt-firmware.bin of=kernel.bin skip=0 count=8192 bs=256
dd if=openwrt-firmware.bin of=rootfs.bin skip=8192 bs=256
```
- Access to the router using telnet, but first, connect to its default wifi `DECO_XXX`
```
telnet 192.168.68.1
```
- Copy the `kernel.bin` and `rootfs.bin` to /tmp
- Flash
```
mtd write kernel.bin kernel
mtd write rootfs.bin rootfs
```
- Reboot

# Assemble an openwrt image (optional)
- Link: https://openwrt.org/docs/guide-user/additional-software/imagebuilder
- Used command:
```
make image PROFILE=tplink_deco-m4r-v1 PACKAGES="luci luci-app-opkg uci mtd uhttpd uhttpd-mod-ubus -firewall4 -odhcp6c -odhcpd-ipv6only -ppp -ppp-mod-pppoe -dnsmasq -nftables" FILES="./files" EXTRA_IMAGE_NAME="custom-dump-ap"
```
- Struct of files:
```
files
└── etc
    ├── config
    │   ├── network
    │   └── wireless
    └── dropbear
        └── authorized_keys
```
