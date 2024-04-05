# compiling kernel arch
Get kernel from https://kernel.org

```unxz --keep linux-*.tar.xz```

```gpg2 --locate-keys torvalds@kernel.org gregkh@kernel.org```

```gpg2 --verify linux-*.tar.sign```

```tar -xf linux-*.tar```

```cd linux-*/```

```zcat /proc/config.gz > .config```

```make olddefconfig```

```./scripts/config --file .config --set-str LOCALVERSION "-<name>"```

```make -j$(nproc) 2>&1 | tee log```

```sudo make modules_install -j$(nproc)```

```sudo make headers_install```

```sudo install -Dm644 "$(make -s image_name)" /boot/vmlinuz-<kernel_release>-<name>```

```sudo cp -vf System.map /boot/System.map-<kernel_release>-<name>```

```sudo cp -vf .config /boot/config-<kernel_release>-<name>```

Create a file /etc/mkinitcpio.d/linux-\<name\>.preset

```ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/vmlinuz-<kernel_release>-<name>"

PRESETS=('default' 'fallback')

default_image="/boot/initramfs-<kernel_release>-<name>.img"
fallback_options="-S autodetect"
```

```sudo mkinitcpio -p linux-<name>```

```sudo grub-mkconfig -o /boot/grub/grub.cfg```
