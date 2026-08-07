---

title: Arch Linux Kurulum Rehberi
parent: Rehberler
nav_order: 2
---

# Arch Linux Kurulum Rehberi
Bu rehberde UEFI sistem üzerine **Arch Linux** kurulumunu adım adım yapacağız. Rehber, yeni başlayanlar için hazırlanmıştır ve kurulum sonunda çalışan bir Arch Linux sistemine sahip olacaksınız.

> Bu rehber UEFI sistemler içindir. BIOS (Legacy) sistemlerde bazı adımlar farklı olabilir.

---

## Gereksinimler
* En az **8 GB USB bellek**
* İnternet bağlantısı
* Arch Linux ISO dosyası
* Rufus veya balenaEtcher ile hazırlanmış önyüklenebilir USB

Arch Linux ISO: https://archlinux.org/download/

---

## USB ile başlatma
Bilgisayarı USB bellekten başlatın ve **Arch Linux install medium** seçeneğini seçin.

Sistem açıldıktan sonra terminal ekranı gelecektir.

---

## İnternet bağlantısını kontrol et

Kablolu bağlantı kullanıyorsanız:
```bash
ping archlinux.org
```

Kablosuz bağlantı için:
```bash
iwctl
```

Ardından:
```bash
device list
station wlan0 scan
station wlan0 get-networks
station wlan0 connect WiFi_ADI
exit
```

Tekrar kontrol edin:
```bash
ping archlinux.org
```

---

## Diskleri görüntüle
```bash
lsblk
```

Örnek disk:
```text
sda      512G
```
Bu rehberde `/dev/sda` kullanılacaktır.

---

## Diski bölümle

`cfdisk` aracını açın:
```bash
cfdisk /dev/sda
```

GPT seçin ve şu bölümleri oluşturun:
| Bölüm |      Boyut | Tür              |
| ----- | ---------: | ---------------- |
| EFI   |       512M | EFI System       |
| Root  | Kalan alan | Linux filesystem |

Yazdırın (**Write**) ve çıkın (**Quit**).

---

## Bölümleri biçimlendir

EFI:
```bash
mkfs.fat -F32 /dev/sda1
```

Root:
```bash
mkfs.ext4 /dev/sda2
```

---

## Bölümleri bağla
```bash
mount /dev/sda2 /mnt
mkdir -p /mnt/boot
mount /dev/sda1 /mnt/boot
```

Kontrol:
```bash
lsblk
```

---

## Temel sistemi kur
```bash
pacstrap -K /mnt base linux linux-firmware nano networkmanager grub efibootmgr
```

Bu işlem birkaç dakika sürebilir.

---

## fstab oluştur
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Kontrol etmek için:
```bash
cat /mnt/etc/fstab
```

---

## Yeni sisteme geç
```bash
arch-chroot /mnt
```

Artık kurulu sistemin içindesiniz.

---

## Saat dilimi

Türkiye için:
```bash
ln -sf /usr/share/zoneinfo/Europe/Istanbul /etc/localtime
hwclock --systohc
```

---

## Dil ayarları

`/etc/locale.gen` dosyasını açın:
```bash
nano /etc/locale.gen
```

Şu satırın başındaki `#` işaretini kaldırın:
```text
en_US.UTF-8 UTF-8
```

İsterseniz:
```text
tr_TR.UTF-8 UTF-8
```

Kaydedin ve çalıştırın:
```bash
locale-gen
```

Ardından:
```bash
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

---

## Hostname

Bilgisayar adı:
```bash
echo nexuspc > /etc/hostname
```

`/etc/hosts` dosyasını düzenleyin:
```bash
nano /etc/hosts
```

İçine:
```text
127.0.0.1 localhost
::1 localhost
127.0.1.1 nexuspc.localdomain nexuspc
```

---

## Root şifresi
```bash
passwd
```

Şifrenizi girin.

---

## Kullanıcı oluştur
```bash
useradd -m -G wheel -s /bin/bash ayaz
passwd ayaz
```

---

## sudo yetkisi
```bash
EDITOR=nano visudo
```

Şu satırın başındaki `#` işaretini kaldırın:
```text
%wheel ALL=(ALL:ALL) ALL
```

---

## NetworkManager
```bash
systemctl enable NetworkManager
```

---

## GRUB kurulumu

EFI sistem için:
```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

Yapılandırma oluştur:
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

---

## Kurulumu tamamla
Chroot ortamından çık:

```bash
exit
```

Bölümleri ayır:
```bash
umount -R /mnt
```

Sistemi yeniden başlat:
```bash
reboot
```

USB belleği çıkarın.

---

# İlk açılış sonrası
Kullanıcı ile giriş yapın.

Sistemi güncelleyin:
```bash
sudo pacman -Syu
```

Yaygın paketler:
```bash
sudo pacman -S git wget curl htop neofetch
```

---

# KDE Plasma kurulumu (isteğe bağlı)
```bash
sudo pacman -S plasma kde-applications sddm
sudo systemctl enable sddm
reboot
```

---

# Kontrol listesi
* Arch Linux açılıyor
* İnternet çalışıyor
* Kullanıcı hesabı mevcut
* sudo çalışıyor
* GRUB açılıyor
* Sistem güncel

Kurulum tamamlandı. Artık çalışan bir **Arch Linux** sistemine sahipsiniz.
