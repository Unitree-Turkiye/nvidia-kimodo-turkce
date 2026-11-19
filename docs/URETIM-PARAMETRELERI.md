> Bu belge nv-tlabs/kimodo deposundaki `docs/source/user_guide/configuration.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Üretim parametreleri

Kimodo; demo arayüzünde, komut satırı aracında (`kimodo_gen` / `python -m kimodo.scripts.generate`) ve düşük düzeyli Python API'sinde hareket üretimi için bazı gelişmiş yapılandırmalar sunar.

## Sınıflandırıcısız yönlendirme

Metin ve kısıt yönlendirmesinin gücünü denetleyin:

```python
output = model(
    prompt="A person jumps",
    num_frames=150,
    cfg_weight=[2.0, 2.0],  # [text_weight, constraint_weight]
    cfg_type="separated",  # Options: "nocfg", "regular", "separated"
    num_denoising_steps=100,
)
```

Bu ayarlar, isteme uymak ile kısıtları tutturmak arasında bir denge kurmanız gerektiğinde işe yarar.

CFG seçenekleri şunlardır:
- `cfg_type="nocfg"`: Yönlendirme yok (daha hızlı, daha az denetlenebilir)
- `cfg_type="regular"`: "Standart" sınıflandırıcısız yönlendirme
    - Denklem: `out_uncond + w * (out_text_and_constraint - out_uncond)`
- `cfg_type="separated"`: Metin ve kısıtlar için ayrı ağırlıklar
    - Denklem: `out_uncond + w_text * (out_text - out_uncond) + w_constraint * (out_constraint - out_uncond)`

### CLI

Aynı seçenekler komut satırında `--cfg_type` ve `--cfg_weight` olarak bulunur. Örnekler, doğrulama kuralları ve `--input_folder` kullanırken `meta.json` dosyasının açık bayraklarla nasıl etkileştiği için [CLI kullanıcı rehberine (CFG)](KOMUT-SATIRI-ARAYUZU.md#sınıflandırıcısız-yönlendirme-cfg) bakın.

## Gürültü giderme adımları
DDIM örneklemesinde kullanılan gürültü giderme adımı sayısı, hız ile kalite arasındaki dengeyi denetler:
- Daha az adım (50-100): Daha hızlı çıkarım, biraz daha düşük kalite
- Daha çok adım (100-200): Daha yüksek kalite, daha yavaş çıkarım

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
