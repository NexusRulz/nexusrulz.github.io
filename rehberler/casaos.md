---

title: CasaOS Kurulum Rehberi
parent: Rehberler
nav_order: 1
------------

# CasaOS Kurulum Rehberi (Debian Üzerine)
Bu rehberde **sıfırdan Debian kurarak CasaOS kurulumu**, temel ağ ayarları, disk hazırlığı, Docker kontrolü ve HTTPS (Cloudflare + Nginx Proxy Manager) altyapısına kadar tüm adımları detaylı şekilde anlatıyorum.

---

# Gereksinimler
* Eski veya yeni bir bilgisayar / mini PC / sunucu
* En az **2 GB RAM**
* En az **20 GB boş disk alanı**
* İnternet bağlantısı
* USB bellek (Debian kurmak için)

**Bu rehberde örnek sistem:**
* Debian 12 (Bookworm)
* Ethernet bağlantısı
* Statik IP
* CasaOS son sürüm

---

# Debian 12 Kurulumu:

## Debian ISO indir
Resmi Debian sitesinden **Debian 12 netinst ISO** dosyasını indir.

**Kurulum USB’si oluşturmak için:**
* Rufus (Windows)
* balenaEtcher
* Ventoy

USB’yi hazırlayıp bilgisayarı USB’den başlat.

---

## Kurulum sırasında

Önerilen ayarlar:
* Language: English
* Location: Turkey
* Keyboard: Turkish Q

Hostname:
```text
casaos
```

Domain name boş bırakılabilir.

Kullanıcı oluştur:
```text
Username: ayaz
Password: güçlü bir parola
```

---

## Disk bölümlendirme

Kolay yöntem:
```text
Guided - use entire disk
```

Dosya sistemi:
```text
Ext4
```

Kurulum bittikten sonra sistemi yeniden başlat.

---

# İlk Açılış
Sunucu açıldığında kullanıcı ile giriş yap.

Önce sistemi güncelle.
```bash
sudo apt update
sudo apt upgrade -y
```

Gerekli araçları kur.
```bash
sudo apt install curl wget git nano htop -y
```

---

# IP Adresini Öğren

Mevcut IP’yi kontrol et.
```bash
ip a
```

veya

```bash
hostname -I
```

Örnek çıktı:
```text
192.168.1.50
```

Bu adresi not et.

---

# Statik IP Ayarlama

Debian 12’de ağ ayarları genellikle **NetworkManager** ile yönetilir.

Mevcut bağlantıları listele.
```bash
nmcli connection show
```

Bağlantı adını öğren.

Örnek:
```text
Wired connection 1
```

Statik IP ver.
```bash
sudo nmcli connection modify "Wired connection 1" \
ipv4.addresses 192.168.1.50/24 \
ipv4.gateway 192.168.1.1 \
ipv4.dns "1.1.1.1 8.8.8.8" \
ipv4.method manual
```

Bağlantıyı yeniden başlat.
```bash
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

Kontrol et.
```bash
ip a
```

---

# CasaOS Kurulumu

CasaOS’un resmi kurulum komutu:
```bash
curl -fsSL https://get.casaos.io | sudo bash
```

Kurulum birkaç dakika sürebilir.

Kurulum tamamlandığında şu adres üzerinden erişebilirsin:
```text
http://192.168.1.50
```

---

# İlk Giriş
Tarayıcıdan IP adresini aç.

Örnek:
```text
http://192.168.1.50
```

İlk açılışta kullanıcı oluştur.

Örnek:
* Username: ayaz
* Password: güçlü bir parola

Artık CasaOS paneline giriş yapabilirsin.

---

# CasaOS Güncelleme

Sistemi güncelle.

```bash
sudo apt update
sudo apt upgrade -y
```

CasaOS servislerini yeniden başlat.

```bash
sudo systemctl restart casaos
```

Durumu kontrol et.

```bash
sudo systemctl status casaos
```

---

# Docker Kontrolü
CasaOS Docker kullanır.

Docker sürümünü kontrol et.
```bash
docker --version
```

Çalışıyor mu kontrol et.
```bash
sudo systemctl status docker
```

Docker’ı yeniden başlat.
```bash
sudo systemctl restart docker
```

---

# Disk Ekleme

Yeni diskleri listele.
```bash
lsblk
```

Örnek:
```text
sda
sdb
```

Diski biçimlendir.
```bash
sudo mkfs.ext4 /dev/sdb
```

Bağlama klasörü oluştur.
```bash
sudo mkdir /mnt/data
```

Diski bağla.
```bash
sudo mount /dev/sdb /mnt/data
```

Kalıcı yapmak için UUID öğren.
```bash
sudo blkid
```

`/etc/fstab` dosyasını düzenle.

```bash
sudo nano /etc/fstab
```

Şu satırı ekle.
```text
UUID=xxxxxxxx /mnt/data ext4 defaults 0 2
```

Kaydet ve test et.
```bash
sudo mount -a
```

---

# Uygulama Kurulumu
CasaOS panelinden **App Store** aç.

Önerilen ilk uygulamalar:
* Immich
* Nextcloud
* Jellyfin
* File Browser
* Portainer

Kurulum tek tıklama ile yapılabilir.

---

# Portainer Kurulumu

Docker yönetimi için Portainer önerilir.
```bash
docker volume create portainer_data

docker run -d \
  --name portainer \
  --restart=always \
  -p 9000:9000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

Erişim:
```text
http://192.168.1.50:9000
```

---

# Nginx Proxy Manager Kurulumu
HTTPS için en kolay yöntem.

Docker Compose oluştur.
```yaml
version: '3'

services:
  npm:
    image: jc21/nginx-proxy-manager:latest
    restart: always
    ports:
      - 80:80
      - 81:81
      - 443:443
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

Çalıştır.
```bash
docker compose up -d
```

Panel:
```text
http://192.168.1.50:81
```

Varsayılan giriş:
```text
Email: admin@example.com
Password: changeme
```

İlk girişte değiştir.

---

# Cloudflare ile Alan Adı
Cloudflare hesabına alan adını ekle.
DNS kaydı oluştur.

Tür:
```text
A
```

Name:
```text
casa
```

IPv4:
```text
192.168.1.50
```

Yerel ağ yerine internet erişimi istiyorsan modemde **Port Forwarding** yap:
* 80
* 443

Nginx Proxy Manager’da yeni Proxy Host oluştur.

Domain:
```text
casa.seninalanadin.com
```

Forward Host:
```text
192.168.1.50
```

Forward Port:
```text
80
```

SSL sekmesinden **Let’s Encrypt** sertifikası al.
Artık erişim:

```text
https://casa.seninalanadin.com
```

---

# Faydalı Komutlar

## CasaOS durumu
```bash
sudo systemctl status casaos
```

## Docker durumu
```bash
sudo systemctl status docker
```

## Yeniden başlat
```bash
sudo reboot
```

## Kapat
```bash
sudo shutdown now
```

## Disk kullanımı
```bash
df -h
```

## RAM kullanımı
```bash
free -h
```

## Çalışan konteynerler
```bash
docker ps
```

---

# Sonuç

Bu noktada sisteminde:
* Debian 12
* CasaOS
* Docker
* Statik IP
* Ek disk
* Portainer
* Nginx Proxy Manager
* Cloudflare
* HTTPS erişimi

hazır durumda olacaktır.
Bir sonraki rehberde **Immich + Nextcloud + CasaOS üzerinde otomatik yedekleme ve SSL sertifikalarının Cloudflare üzerinden yönetimi** anlatılacaktır.
