> Bu belge nv-tlabs/kimodo deposundaki `docs/source/user_guide/motion_convert.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Hareket biçimi dönüştürme

`kimodo_convert` komutu, [Çıktı biçimleri](CIKTI-BICIMLERI.md) belgesinde anlatılan biçimler arasında dönüştürme yapar: **Kimodo NPZ**, **AMASS NPZ** (SMPL-X), **SOMA BVH** ve **G1 MuJoCo CSV**.

## Kare hızı (30 Hz Kimodo NPZ)

**Kimodo NPZ'ye** yapılan her dönüştürme (AMASS, SOMA BVH ya da G1 CSV kaynağından) hareketi, Kimodo'nun olağan üretim hızına uygun olarak **30 Hz'de yazar**. Algılanan kaynak hızı farklıysa araç zaman ekseninde **yeniden örnekleme** (resampling) yapar, sonra türetilen kanallar (temaslar, yumuşatılmış kök, yönelim) ileri kinematikle (FK) yeniden hesaplanır.

Yeniden örnekleme gerekirse varsayılan kaynak hızını, girdi ve çıktı kare sayılarını ve otomatik algılama yanlışsa `--source-fps` bayrağının **kaynak** hızını ayarladığını hatırlatan bir **uyarı** verilir. Kaynak zaten yaklaşık 30 Hz ise ve kare sayısı aynıysa uyarı gösterilmez (hareket yalnız tutarlılık için FK ile yeniden türetilir).

<details>
<summary>Yeniden örnekleme stratejisinin ayrıntıları</summary>

Yeniden örnekleyici, `source_fps / target_fps` oranına göre iki stratejiden birini seçer:

- **Tam sayı oranlı hızlı yol:** Oran 2 ya da daha büyük bir tam sayıya yakınsa (0,05 tolerans içinde) yeniden örnekleyici yalnız her *step*. kareyi alır (`frames[::step]`). Örneğin 120 Hz → 30 Hz için oran 4'tür, bu yüzden her 4. kare tutulur. Bu yol tam sonuç verir ve çok hızlıdır.
- **Ara değerleme yedeği:** Öteki durumlarda çıktı zaman çizelgesi girdi aralığı üzerine doğrusal aralıklarla yayılır. Kök konumları doğrusal ara değerlemeyle, yerel eklem dönüşleri dördey (quaternion) slerp ile hesaplanır. Bu yol keyfi hız dönüşümlerini karşılar (ör. 50 Hz → 30 Hz).

İki durumda da `complete_motion_dict` hedef hızda yeniden çalıştırılır. Böylece türetilen tüm kanallar (hızlar, ayak temasları, yönelim, yumuşatılmış kök) yeni kare aralığıyla tutarlı kalır.

</details>

## Kullanım

```bash
kimodo_convert INPUT OUTPUT [options]
```

Biçimler dosya uzantılarından ve (`.npz` için) dosya içeriğinden çıkarılır. Bunu `--from` ve `--to` ile geçersiz kılabilirsiniz.

### Desteklenen dönüştürmeler

| Kaynak | Hedef | Notlar |
|------|-----|--------|
| AMASS `.npz` | Kimodo `.npz` | SMPL-X, 22 eklem. Varsayılan olarak `--z-up` kullanır (Kimodo'nun AMASS dışa aktarımıyla aynı). |
| Kimodo `.npz` | AMASS `.npz` | 22 eklemli `local_rot_mats` gerektirir (SMPL-X). |
| SOMA `.bvh` | Kimodo `.npz` | **Kimodo'nun dışa aktardığı** bir SOMA BVH bekler (`save_motion_bvh` ile aynı hiyerarşi). BVH dinlenme pozu olarak standart T-pozu kullanıyorsa `--bvh_standard_tpose` verin. |
| Kimodo `.npz` | SOMA `.bvh` | 77 eklemi (tam SOMA) ya da 30 eklemi (somaskel30, gevşek el dinlenme pozlarıyla otomatik olarak 77'ye genişletilir) kabul eder. Çıktı BVH'nin dinlenme pozu olarak standart T-pozu kullanmasını istiyorsanız `--bvh_standard_tpose` verin. |
| G1 `.csv` | Kimodo `.npz` | `(36,)` biçimli satırlar = kök xyz + kök dördeyi + 29 eklem açısı ([çıktı biçimleri](CIKTI-BICIMLERI.md#kimodo-g1-için-csv-biçimi) belgesine bakın). |
| Kimodo `.npz` | G1 `.csv` | 34 eklem gerektirir (G1). |

### Ortak seçenekler

- **`--source-fps`**: Hz cinsinden kaynak hareket kare hızı (Kimodo NPZ için 30 Hz'e yeniden örneklemeden önce kullanılır). Verilmezse araç hızı `mocap_frame_rate` (AMASS) ya da `Frame Time` (BVH) değerinden otomatik algılar, yoksa varsayılan **30** Hz kullanılır. Eski `--fps` takma adı geriye dönük uyumluluk için halen kabul edilir.
- **`--no-z-up`**: AMASS için Y yukarı ↔ Z yukarı dönüşümünü kapatır (veriyi zaten Kimodo Y yukarı, +Z ileri düzeninde kabul eder).
- **`--mujoco-rest-zero`**: G1 CSV için, CSV yazılırken kullanılan `mujoco_rest_zero` bayrağıyla eşleşir (`MujocoQposConverter.dict_to_qpos` işlevine bakın).
- **`--bvh_standard_tpose`**: Girdi ya da çıktı BVH ise: BVH dosyası dinlenme pozu olarak BONES-SEED dinlenme pozu yerine standart T-pozu kullanır.

### Örnekler

```bash
# AMASS → Kimodo NPZ
kimodo_convert motion_amass.npz motion_kimodo.npz

# Kimodo NPZ → AMASS
kimodo_convert motion_kimodo.npz motion_out_amass.npz

# Kimodo SOMA NPZ → BVH
kimodo_convert motion_kimodo.npz motion.bvh

# BVH → Kimodo NPZ
kimodo_convert motion.bvh motion_kimodo.npz

# G1 CSV → Kimodo NPZ
kimodo_convert motion.csv motion_kimodo.npz

# Kimodo G1 NPZ → CSV
kimodo_convert motion_kimodo.npz motion.csv
```

Girdi ve çıktının ikisi de `.npz` olduğunda araç, girdi AMASS ise **AMASS → Kimodo**, girdi zaten bir Kimodo NPZ ise **Kimodo → AMASS** varsayar. Ayrım gerekirse `--from` / `--to` kullanın.

## Sınırlamalar

- **BVH içe aktarma**, Kimodo'nun ürettiği BVH dosyaları (`Root` sarmalayıcı + SOMA77 eklem adları) için tasarlandı. Aynı iskelet hiyerarşisini kullanan BONES-SEED veri setiyle de uyumludur. Farklı eklem adları ya da hiyerarşi taşıyan keyfi BVH dosyaları bu içe aktarmanın kapsamı dışındadır.
- **G1 CSV** yalnız MuJoCo'da açılan serbestlik derecelerini kodlar. Ters yol, yerel dönüşleri bu açılardan yeniden kurar (`to_qpos` ile aynı kural).

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
