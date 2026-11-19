> Bu belge nv-tlabs/kimodo deposundaki `docs/source/getting_started/installation_smpl.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Kimodo-SMPLX modelini kullanma

[Kimodo-SMPLX-RP-v1](https://huggingface.co/nvidia/Kimodo-SMPLX-RP-v1) modelini kullanmak birkaç ek kurulum adımı gerektirir.

## Model erişimi isteyin

Kimodo'nun SMPL-X sürümüne erişim onaya bağlıdır (gated). Bu yüzden onunla CLI'da ya da demoda hareket üretmeyi denemeden önce [Hugging Face model sayfasına](https://huggingface.co/nvidia/Kimodo-SMPLX-RP-v1) gidin ve erişim isteyin. Modele erişiminizin doğrulanabilmesi için [kurulum](KURULUM.md) sürecinde anlatıldığı gibi HF belirtecinizin doğru kurulduğundan emin olun.

## SMPL-X gövde modelini indirin
Üretilen SMPL-X hareketlerini demoda görselleştirmek istiyorsanız SMPL-X gövde modelini indirmeniz gerekir.
[SMPL-X](https://smpl-x.is.tue.mpg.de/) web sayfasına gidin, oturum açın ya da bir hesap oluşturun ve "Download" sayfasına geçin.
"Download SMPL-X with removed head bun (NPZ)" bağlantısına tıklayın, sonra `SMPLX_NEUTRAL.npz` dosyasını Kimodo kod tabanında `kimodo/kimodo/assets/skeletons/smplx22/SMPLX_NEUTRAL.npz` konumuna kopyalayın.

Kimodo'yu kod tabanını indirmeden paket olarak kurduysanız, varlıklar (assets) dizininin yerini şu komutla bulun:
```bash
python -c "from kimodo.assets import skeleton_asset_path; print(skeleton_asset_path('smplx22'))"
```

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
