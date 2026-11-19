> Bu belge nv-tlabs/kimodo deposundaki `docs/source/getting_started/installation_virtual_env.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Sanal ortamla kurulum

> Not: Depo Python 3.10+ ve PyTorch 2.0+ ile test edildi.

## Ortamı oluşturun
Bağımlılık çakışmalarını önlemek için Kimodo'ya ayrı bir sanal ortam kurmanızı öneririz.

### venv ile
```bash
python -m venv venv
source venv/bin/activate
```

### Conda ile
```bash
conda create -n kimodo python=3.10
conda activate kimodo
```

## Bağımlılıkları kurun

### PyTorch'u kurun
Önce sisteminizle ve CUDA sürümünüzle çalışan bir [PyTorch](https://pytorch.org/get-started/locally/) sürümü kurun. PyTorch 2.0 ve üstünü öneririz. Hareketleri makul sürede üretmek için GPU destekli bir PyTorch sürümü kullanmanızı önemle öneririz.

### (İsteğe bağlı) Değiştirilmiş Viser kütüphanesini klonlayın
Etkileşimli demo, zaman çizelgesi arayüzü ve başka özellikler ekleyen [bir Viser çatalını (fork)](https://github.com/nv-tlabs/kimodo-viser) kullanır. Viser'in bu sürümünü düzenlenebilir kurmak istiyorsanız (yani üzerinde değişiklik yapacaksanız) onu `kimodo` dizini içinde şu komutlarla klonlayıp kurun:
```bash
git clone https://github.com/nv-tlabs/kimodo-viser.git
pip install -e kimodo-viser
```

### Kimodo'yu kurun
Ardından Kimodo'yu kurmak için deponun kök dizininde şu komutu çalıştırın:
```bash
pip install -e .
```
Bu komut, Kimodo ve MotionCorrection paketi için tek bir düzenlenebilir kurulum oluşturur.

Demoyu kullanmayı planlıyorsanız bunun yerine şu komutu çalıştırabilirsiniz:
```bash
pip install -e ".[all]"
```
Bu komut [Viser çatalımızı](https://github.com/nv-tlabs/kimodo-viser) (önceki adımda kurulmadıysa) ve [SOMA gövde modelini](https://github.com/NVlabs/SOMA-X) kurar.

Ardından kurulumunuzu birkaç hareket üreterek sınamak için [Hızlı Başlangıç](HIZLI-BASLANGIC.md) sayfasına geçin.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
