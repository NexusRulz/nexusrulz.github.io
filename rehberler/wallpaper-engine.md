---

title: Ventoy ile Format Atma Rehberi
parent: Rehberler
nav_order: 3
------------

# Ventoy ile Format Atma Rehberi
Bu rehberde **Ventoy kullanarak USB üzerinden format atmayı** adım adım anlatacağız. Ventoy sayesinde USB belleği her seferinde yeniden yazdırmadan, ISO dosyasını doğrudan kopyalayarak kurulum başlatabilirsiniz.

> Bu rehber Windows, Linux ve diğer ISO tabanlı işletim sistemleri için geçerlidir.

---

# Ventoy Nedir?
Ventoy, USB belleği önyüklenebilir hale getiren ücretsiz bir araçtır. Bir kez kurulduktan sonra USB'ye istediğiniz ISO dosyasını kopyalamanız yeterlidir.

Avantajları:
* USB'yi tekrar tekrar yazdırmanız gerekmez.
* Aynı USB içinde birden fazla ISO bulundurabilirsiniz.
* Windows, Linux ve kurtarma araçlarını tek USB'de taşıyabilirsiniz.

Resmi site:
https://www.ventoy.net/

---

# Gerekli Dosyalar
* Ventoy kurulu USB bellek
* Kurmak istediğiniz işletim sisteminin ISO dosyası
* Bilgisayarda USB'den önyükleme desteği

---

# ISO Dosyasını USB'ye Kopyalama
Ventoy kurulu USB belleği bilgisayara takın.
Kurmak istediğiniz ISO dosyasını USB'nin ana dizinine kopyalayın.

Örnek:
```text
VentoyUSB/
├── Windows11.iso
├── ArchLinux.iso
├── Ubuntu.iso
```

ISO dosyalarını klasör içine koymanız da mümkündür.

---

# BIOS / Boot Menüsüne Girme
Bilgisayarı yeniden başlatın.

Açılış sırasında aşağıdaki tuşlardan biriyle **Boot Menu** veya **BIOS** ekranına girin:
| Marka    | Tuş |
| -------- | --- |
| ASUS     | F8  |
| MSI      | F11 |
| Gigabyte | F12 |
| Lenovo   | F12 |
| HP       | F9  |
| Dell     | F12 |
| Acer     | F12 |

---

# Ventoy USB'sini Seçme
Boot menüsünde **USB** veya **Ventoy** olarak görünen aygıtı seçin.
Ventoy menüsü açılacaktır.

---

# ISO Dosyasını Başlatma
Ventoy ekranında USB içindeki ISO dosyaları listelenir.

Örnek:
```text
Windows11.iso
ArchLinux.iso
Ubuntu.iso
```

Kurmak istediğiniz ISO dosyasını seçin ve **Enter** tuşuna basın.
Ventoy ISO'yu doğrudan başlatacaktır.

---

# Kurulum Ekranı
ISO açıldıktan sonra artık işletim sisteminin kendi kurulum ekranındasınız.

Örneğin:
* Windows için **Install Now**
* Ubuntu için **Try or Install Ubuntu**
* Arch Linux için **Arch Linux install medium**

Bu aşamadan sonra kurulum, seçtiğiniz işletim sisteminin normal kurulum adımlarıyla devam eder.

---

# Ventoy'un Sağladığı Kolaylık

Geleneksel yöntem:
```text
ISO indir
↓
Rufus ile USB yazdır
↓
Kurulum
↓
Başka ISO için USB'yi tekrar yazdır
```

Ventoy yöntemi:
```text
Ventoy kur
↓
ISO dosyasını USB'ye kopyala
↓
Boot menüsünden Ventoy'u aç
↓
ISO'yu seç ve kuruluma başla
```

---

# Sorun Giderme

## USB görünmüyor
* USB'yi farklı porta takın.
* BIOS'ta USB Boot etkin olsun.
* Secure Boot geçici olarak kapatılabilir.

## ISO açılmıyor
* ISO dosyasının bozuk olmadığını kontrol edin.
* ISO'yu yeniden indirin.
* Ventoy sürümünü güncelleyin.

## UEFI / Legacy uyumsuzluğu
* Modern sistemlerde **UEFI** kullanılması önerilir.
* Gerekirse BIOS'tan Boot Mode ayarını kontrol edin.

---

# Kontrol Listesi
* Ventoy USB hazır
* ISO dosyası USB'ye kopyalandı
* Boot menüsünden USB seçildi
* Ventoy açıldı
* ISO dosyası çalıştı
* Kurulum ekranı geldi

Artık Ventoy kullanarak istediğiniz işletim sisteminin kurulumunu USB üzerinden başlatabilirsiniz.
