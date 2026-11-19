> Bu belge nv-tlabs/kimodo deposundaki `docs/source/user_guide/output_formats.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Çıktı biçimleri

## Biçimler arasında dönüştürme

Aşağıda anlatılan biçimler arasında dönüştürme için [Hareket biçimi dönüştürme](HAREKET-BICIMI-DONUSTURME.md) (`kimodo_convert`) belgesine bakın.

## Kimodo NPZ biçimi

Üretilen hareketler, şunları içeren NPZ dosyaları olarak saklanır (örnek başına bir dosya, ör. `motion_00.npz`):

- `posed_joints`: Küresel eklem konumları `[T, J, 3]`
- `global_rot_mats`: Küresel eklem dönüş matrisleri `[T, J, 3, 3]`
- `local_rot_mats`: Yerel (üst ekleme göre) eklem dönüş matrisleri `[T, J, 3, 3]`
- `foot_contacts`: Ayak teması etiketleri [sol topuk, sol parmak ucu, sağ topuk, sağ parmak uçları] `[T, 4]`
- `smooth_root_pos`: Modelin çıkardığı yumuşatılmış kök temsilleri `[T, 3]`
- `root_positions`: Gerçek kök ekleminin (ör. leğen kemiği, pelvis) yumuşatılmamış yörüngesi `[T, 3]`
- `global_root_heading`: Modelin çıkardığı yönelim yönü `[T, 2]`

Burada:

- `T`: kare sayısı
- `J`: dışa aktarılan iskelet temsilindeki eklem sayısı (SOMA NPZ dışa aktarımlarında `77`, G1 için `34`, SMPL-X için `22`)

Birden çok örnek üretilirse dosyalar `_00`, `_01` gibi son eklerle kaydedilir.

SOMA modellerinde model kendi içinde indirgenmiş **`somaskel30`** iskeletiyle çalışsa da dışa aktarılan NPZ tam **`somaskel77`** iskeletini kullanır. Yani kaydedilen `posed_joints`, `global_rot_mats` ve `local_rot_mats` dizileri 77 eklemli SOMA düzeninde yazılır. Daha eski 30 eklemli SOMA NPZ dosyaları da bulunabilir ve geriye dönük uyumluluk için yüklenebilir kalır.

SOMA modellerinde çıktı hareketi, dinlenme pozu (yani sıfır pozu) Kimodo'nun içte kullandığı standart T-poz olacak şekilde de kaydedilir. Bu, BVH dışa aktarımının varsayılan davranışından (aşağıya bakın) farklıdır. BVH dışa aktarımı BONES-SEED veri seti biçimiyle tutarlı bir dinlenme pozu kullanır. Standart T-poz bir BVH dosyası olarak [depo varlıklarında](https://github.com/nv-tlabs/kimodo/tree/main/kimodo/assets/skeletons/somaskel77) da bulunur.

## Kimodo-SOMA için BVH biçimi

Bir SOMA modeli kullanıp CLI üretimine `--bvh` bayrağını verdiğinizde Kimodo NPZ çıktısının yanına bir BVH dosyası da yazar.

- BVH dışa aktarımı **yalnız SOMA modellerinde** desteklenir
- dışa aktarılan hiyerarşi tam **`somaskel77`** iskeletini kullanır
- hareket hala iç `somaskel30` biçimindeyse Kimodo onu BVH yazmadan önce `somaskel77` biçimine dönüştürür
- dosya, klip için kök ötelemesini ve eklem başına yerel dönüşleri üretilen kare hızında saklar
- varsayılan olarak kaydedilen BVH dosyasının dinlenme pozu (yani sıfır pozu) BONES-SEED veri seti biçimiyle tutarlıdır. Dinlenme pozu olarak standart T-pozu tercih ediyorsanız üretirken `--bvh_standard_tpose` verin.

Dışa aktarıcı standart düz metin bir BVH dosyası yazar ve eklem uzaklıklarını (offset) ile kök hareketini metreden santimetreye ölçekler (SEED veri seti sürümüyle aynı biçim). Birden çok örnek üretilirse dosyalar `_00`, `_01` gibi son eklerle kaydedilir.

## Kimodo-G1 için CSV biçimi

`Kimodo-G1` modellerini kullanıp CLI üretimine `--output` verdiğinizde dışa aktarıcı MuJoCo `qpos`
verisini bir CSV dosyasına yazar. Her satır hareketteki bir poza karşılık gelir ve 36 değer içerir:

- Kök ötelemesi `[x, y, z]`
- Kök dönüş dördeyi (quaternion) `[w, x, y, z]`
- 29 eklemin tek serbestlik dereceli (1-DoF) değerleri (G1 eklem sırasında)

CSV, MuJoCo koordinat sistemini kullanır (z yukarı, +x ileri). Birden çok örnek üretilirse dosyalar `_00`, `_01` gibi son eklerle kaydedilir.


## Kimodo-SMPLX için AMASS NPZ biçimi

`Kimodo-SMPLX-RP` modelini kullanıp CLI üretiminde `--output` belirttiğinizde dışa aktarıcı
AMASS tarzı bir SMPL-X `.npz` dosyası yazar. Anahtarlar şunlardır:

- `trans`: Kök ötelemesi `[T, 3]`
- `root_orient`: Kök yönelimi, eksen-açı `[T, 3]`
- `pose_body`: Gövde pozu, eksen-açı `[T, 63]` (21 eklem x 3)
- `pose_hand`: El pozu, eksen-açı `[T, 90]` (15 eklem x 2 el x 3)
- `pose_jaw`: Çene pozu, eksen-açı `[T, 3]`
- `pose_eye`: Göz pozu, eksen-açı `[T, 6]`
- `betas`: Şekil katsayıları
- `num_betas`: Şekil katsayısı sayısı
- `gender`: `neutral`
- `surface_model_type`: `smplx`
- `mocap_frame_rate`: Kare hızı (fps)
- `mocap_time_length`: Saniye cinsinden hareket süresi

Dışa aktarıcı, Kimodo koordinat sisteminden (y yukarı, +z ileri)
AMASS koordinatlarına (z yukarı, +y ileri) dönüştürür. Birden çok örnek üretilirse dosyalar `_00`, `_01` gibi son eklerle kaydedilir.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
