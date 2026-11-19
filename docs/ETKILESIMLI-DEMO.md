> Bu belge nv-tlabs/kimodo deposundaki `docs/source/interactive_demo/index.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Etkileşimli demo

Web tabanlı etkileşimli demo, Kimodo model çeşitlerinin herhangi biriyle hareket üretmek için sezgisel bir arayüz sunar.

![Demo arayüzü](https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/overview.png)
*[Viser](https://github.com/viser-project/viser) ile kurulmuş etkileşimli demo arayüzü*

> **Not:** Demoyu çalışırken görmek için aşağıdaki [kurulum yönergelerini](DEMO-CALISTIRMA.md) izleyip yerelde başlatın. Başlattıktan sonra demoyu bir web tarayıcısında http://127.0.0.1:7860 adresinde açın ya da bir sunucuda çalışıyorsanız bağlantı noktası yönlendirmesi (port forwarding) kullanın.

Demo, metin istemlerini ve kısıtları düzenlemek için zaman çizelgesi tabanlı bir arayüz sunar
ve gerçek zamanlı 3B görselleştirme içerir. Başlıca özellikleri şunlardır:

- **Birden çok karakter**: Kimodo'nun SOMA, G1 ve SMPL-X sürümleriyle üretimi destekler
- **Metin istemleri**: İstenen hareketlerin bir ya da daha çok doğal dil tanımını zaman çizelgesine girin
- **Zaman çizelgesi düzenleyicisi**: Birden çok kısıt izinde anahtar kareler ve kısıtlı aralıklar ekleyip düzenleyin
- **Kısıt türleri**:
  - Tam gövde: Belirli karelerde tüm eklem konumu kısıtları
  - 2B kök: Zemin düzleminde izlenecek ara noktaları ya da tam yolları tanımlayın
  - Uç işlevciler: El ve ayak konumlarını/dönüşlerini denetleyin
- **Kısıt düzenleme**: Düzenleme kipi kısıtların yeniden pozlanmasını ya da ara noktaların ayarlanmasını sağlar
- **3B görselleştirme**: Üretilen hareketlerin iskelet ve deri bağlanmış ağ (mesh) seçenekleriyle gerçek zamanlı çizimi
- **Oynatma denetimleri**: Üretilen hareketleri ayarlanabilir oynatma hızıyla önizleyin
- **Birden çok örnek**: Birden çok hareket çeşidi üretip karşılaştırın
- **Örnekler**: Kimodo'nun yeteneklerini daha iyi anlamak için hazır örnekleri yükleyin
- **Dışa aktarma**: Kısıtları ve üretilen hareketleri sonra kullanmak üzere kaydedin


## Hızlı bağlantılar

- [Demoyu başlatma](DEMO-CALISTIRMA.md)
- [Arayüze genel bakış](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/ui_overview.md)
- [Örnekler](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/examples.md)

Demonun öteki sayfaları (özgün belgeler): [model seçimi](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/model_selection.md) · [üretim](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/generation.md) · [kısıtlar](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/constraints.md) · [sonuçları dışa aktarma](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/interactive_demo/export_results.md)

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
