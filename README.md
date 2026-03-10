# Linux Syslog Yönetimi: Çift Yönlü Bağlı Liste (Doubly Linked List) Simülasyonu

Bu proje, Linux işletim sistemlerindeki sistem günlüklerinin (Syslog) bellekte nasıl tutulabileceğini ve yönetilebileceğini göstermek amacıyla C dilinde geliştirilmiş bir veri yapıları simülasyonudur.

## 📌 Proje Amacı
Linux tabanlı sistemlerde çekirdek (kernel) ve servis logları kronolojik olarak üretilir ve sürekli dosya sonuna eklenir. Sistem yöneticileri ise hata ayıklama (debugging) yaparken genellikle en güncel loglardan geçmişe doğru okuma yaparlar. 

Bu projenin amacı, **Çift Yönlü Bağlı Liste (Doubly Linked List)** veri yapısını kullanarak:
1.  Sistemi yormadan, $O(1)$ karmaşıklığı ile yeni logları listeye eklemek.
2.  Hata ayıklama senaryoları için logları sondan başa doğru (en güncelden en eskiye) okuyabilmektir.

## ⚙️ Teknik Mimari ve Veri Yapısı
Projede, standart tek yönlü liste yerine çift yönlü bağlı liste mimarisi tercih edilmiştir.

* **`LogNode` (Düğüm) Yapısı:** Her bir log kaydı için bellekte dinamik olarak oluşturulur. İçerisinde logun üretildiği zaman damgası (`timestamp`), önem derecesi (`priority`), log mesajı (`message`) ve çift yönlü dolaşım için `next` / `prev` işaretçilerini (pointer) barındırır.
* **`SyslogList` (Kontrol) Yapısı:** Listenin en başını (`head`) ve en sonunu (`tail`) tutan merkezi yapıdır. `tail` işaretçisi sayesinde yeni gelen loglar, listeyi baştan sona taramaya gerek kalmadan doğrudan kuyruğa eklenir.

## 🚀 Öne Çıkan Özellikler
* **Dinamik Zaman Damgası:** `<time.h>` kütüphanesi kullanılarak logların eklendiği anın sistem saati gerçek zamanlı olarak düğüme işlenir.
* **Yüksek Performanslı Ekleme:** `tail` pointer mimarisi ile `O(1)` hızında log ekleme garantisi.
* **Çift Yönlü Dolaşım:** `display_logs_forward` ile kronolojik, `display_logs_backward` ile tersine okuma yeteneği.
* **Güvenli Bellek Yönetimi:** `malloc` ile ayrılan tüm bellek alanları, program sonlanırken memory leak (bellek sızıntısı) oluşumunu engellemek adına `free()` ile sisteme iade edilir.

## 💻 Kurulum ve Çalıştırma
Projeyi kendi ortamınızda (Terminal üzerinden) derleyip çalıştırmak için aşağıdaki adımları izleyebilirsiniz:

1. Repoyu bilgisayarınıza klonlayın:
   ```bash
   git clone <sizin-github-repo-linkiniz>
   cd <repo-klasoru>
