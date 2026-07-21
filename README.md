# Debian autoinstall

## Inloggning på nyinstallerad maskin

- Användare: `debadmin` & `docker`
- Engångslösenord: `installtemp` – tvingas bytas vid första inloggning
- Root är avstängt, använd `sudo`
- Hostname blir `DEB-yymmdd-xxxx` (genereras automatiskt)

## Skapa ny ISO (vid ny Debian-version)

1. Ladda ner ny netinst-ISO från debian.org
2. Öppna i AnyBurn med **"Edit image file"** (aldrig "Create from folder")
3. Ändra två filer:

### boot/grub/grub.cfg

Överst:
```
set default=0
set timeout=1
```

Linux-raden på posterna **Graphical install** och **Install**:
```
linux /install.amd/vmlinuz auto=true priority=critical locale=sv_SE.UTF-8 keyboard-configuration/xkb-keymap=se vga=788 preseed/url=https://raw.githubusercontent.com/teknikguide/preseed/main/preseed.cfg ---
```

### isolinux/spkgtk.cfg

```
timeout 10
ontimeout /install.amd/vmlinuz auto=true priority=critical locale=sv_SE.UTF-8 keyboard-configuration/xkb-keymap=se vga=788 initrd=/install.amd/gtk/initrd.gz preseed/url=https://raw.githubusercontent.com/teknikguide/preseed/main/preseed.cfg ---
```

4. CRLF-tvätt i WSL före bygget:
```
cd /mnt/c/TEMP/ISO/DebianISO
sed -i 's/\r$//' boot/grub/grub.cfg isolinux/*.cfg
```

5. Spara ny ISO i AnyBurn och testa i VirtualBox

## Kom ihåg

- Konfigändringar (paket, tidszon, lösenord) görs i detta repo – ingen ny ISO behövs
- GitHub-raw cachar ~5 min efter commit
- Felsökning under installation: Ctrl+Alt+F4 visar loggen
