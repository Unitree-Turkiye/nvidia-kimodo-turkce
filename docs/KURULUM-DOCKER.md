> Bu belge nv-tlabs/kimodo deposundaki `docs/source/getting_started/installation_docker.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Docker ile kurulum

> Not: Docker ile ilk derleme ve çalıştırma birkaç dakika sürebilir, lütfen sabırlı olun.

## Değiştirilmiş Viser kütüphanesini klonlayın
Etkileşimli demo, zaman çizelgesi arayüzü ve başka özellikler ekleyen [bir Viser çatalını](https://github.com/nv-tlabs/kimodo-viser) kullanır. Docker ile derlemeden önce onu `kimodo` dizini içinde şu komutla klonlayın:
```bash
git clone https://github.com/nv-tlabs/kimodo-viser.git
```

## Hızlı kurulum

Docker'ı çalıştırmadan önce Hugging Face belirtecinizin ana makinede (host)
`~/.cache/huggingface/token` konumunda bulunduğundan emin olun. Bunun için örneğin kapsayıcının dışında
bir kez `hf auth login` çalıştırabilirsiniz ([Kurulum](KURULUM.md) yönergelerine bakın).

Etkileşimli demo web uygulamasını (metin kodlayıcı hizmetiyle birlikte) tek komutla derleyip hemen çalıştırmanın en kolay yolu şudur:

```bash
docker compose up -d --build
```

## Adım adım kurulum

Bunun yerine önce şu komutla derleyebilirsiniz:

```bash
docker compose build
```

Bu komut, sırasıyla metin kodlama hizmetine ve etkileşimli hareket hazırlama web uygulamasına karşılık gelen `text-encoder` ve `demo` kapsayıcılarını derler. Bunlarla ilgili ayrıntı için [hızlı başlangıç rehberine](HIZLI-BASLANGIC.md) bakın.

<details>

<summary>Bağımlılıkların gelişmiş yapılandırması</summary>

Bu depo şunları kullanır:
- `docker_requirements.in`: elle tutulan üst düzey bağımlılıklar
- `docker_requirements.txt`: sürümleri sabitlenmiş kilit dosyası (lockfile, otomatik üretilir)

Notlar:
- **Yeniden üretilebilir Docker derlemeleri** için bir kilit dosyası tutuyoruz (böylece bir hafta sonraki yeniden derleme aynı bağımlılıkları çeker).
- Kilit dosyası, Docker temel imajı (`nvcr.io/nvidia/pytorch`) test edilmiş bir PyTorch derlemesini zaten sağladığı için **`torch`/CUDA wheel dosyalarını** bilerek dışarıda bırakır (bu, yavaş kurulumları ve CUDA uyumsuzluklarını önler).

</details>
<br>

Derlemeden sonra, herhangi bir hareket üretiminden önce metin kodlayıcı hizmetini elle başlatmanız gerekir:
```bash
docker compose up text-encoder
```
Not: Bu komutun ilk çalıştırılması, Llama tabanlı metin kodlayıcı indirildiği için uzun sürer.

Son olarak etkileşimli demoyu başlatmak için:
```bash
docker compose up demo
```

Docker kurulumunu kullanma hakkında ayrıntı için sıradaki [Hızlı Başlangıç](HIZLI-BASLANGIC.md) rehberine bakın.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
