> Bu belge nv-tlabs/kimodo deposundaki `docs/source/getting_started/installation.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Kurulum

> Not: Bu proje ek üçüncü taraf açık kaynak yazılım projelerini indirir ve kurar. Kullanmadan önce bu açık kaynak projelerin lisans koşullarını inceleyin.

> Not: Bu depo Linux üzerinde geliştirildi ve en çok Linux üzerinde test edildi.

Kimodo'yu iki yolla kurabilirsiniz: (1) paket olarak ya da (2) kaynak kodu indirip kurarak.
İki yol da üretim sırasında metin kodlayıcıyı (text encoder) kullanmak için bir Hugging Face belirteci (token) gerektirir.

## Hugging Face belirtecini hazırlayın

Kimodo metin kodlayıcısı, erişimi onaya bağlı (**gated**) `meta-llama/Meta-Llama-3-8B-Instruct` modelini kullanır. Bunun için:
- HF hesabınıza [model sayfasında](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct) erişim izni verilmiş olmalı.
- Çalışma sırasında bir HF belirteci sağlamalısınız.

Llama deposuna erişim izni aldıktan sonra [buradan](https://huggingface.co/settings/tokens/new?tokenType=read) bir erişim belirteci oluşturun. Sonra bu belirteçle komut satırında oturum açın:

```bash
hf auth login
```
Bunun yerine belirteci ``~/.cache/huggingface/token`` dosyasına da yapıştırabilirsiniz. `hf` kurulu değilse önce `pip install --upgrade huggingface_hub` komutunu çalıştırın.

## Kimodo kurulum seçeneği 1: Paket kurulumu

Başlamanın en kolay yolu, kod tabanını klonlamadan Kimodo'yu bir paket olarak kurmaktır. Bu yolla hareket üretebilir ve demoyu bir kara kutu olarak çalıştırabilirsiniz.

Kurulum için yeni bir Python ortamı oluşturmanızı öneririz, örneğin `venv` ya da conda ile:
```bash
conda create -n kimodo python=3.10
conda activate kimodo
```

Sisteminize ve CUDA sürümünüze uyan bir [PyTorch](https://pytorch.org/get-started/locally/) sürümü için, Kimodo'yu kurmadan önce size en uygun PyTorch sürümünü elle kurmanız önerilir. PyTorch 2.0 ve üstü yeterlidir. Hareketleri makul sürede üretmek için GPU destekli bir PyTorch sürümü kullanmanızı önemle öneririz.

Temel Kimodo paketini kurduğunuzda hareketleri komut satırından üretebilirsiniz:
```bash
pip install git+https://github.com/nv-tlabs/kimodo.git
```

Etkileşimli demoyu da çalıştırmak istiyorsanız ek bağımlılıkları kuran şu komutu kullanın:
```bash
pip install "kimodo[all] @ git+https://github.com/nv-tlabs/kimodo.git"
```

Artık Kimodo'yu kullanmaya hazırsınız. Hareket üretmeyi görmek için [hızlı başlangıç rehberine](HIZLI-BASLANGIC.md) bakın.

Yukarıdaki kurulum yolunda paket ya da sistem uyumluluğuyla ilgili bir sorunla karşılaşırsanız, kod tabanını indirip aşağıda anlatılan Docker kurulumunu kullanmanızı öneririz.

## Kimodo kurulum seçeneği 2: Kaynak koddan kurulum

Kimodo'nun üzerine geliştirme yapmayı ya da kod tabanını incelemeyi planlıyorsanız depoyu klonlayıp kurun.

### Kimodo deposunu klonlayın

```bash
git clone https://github.com/nv-tlabs/kimodo.git
cd kimodo
```

### Kurulum yolunu seçin
Kimodo'yu bir sanal ortam (ör. `conda`) üzerinden derleyip çalıştırarak ya da bir Docker kapsayıcısı (container) içinde kurabilirsiniz.

- [Sanal ortamla kurulum](KURULUM-SANAL-ORTAM.md)
- [Docker ile kurulum](KURULUM-DOCKER.md)
- [Kimodo-SMPLX modelini kullanma](KIMODO-SMPLX-KURULUMU.md)

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
