> Bu belge nv-tlabs/kimodo deposundaki `docs/source/user_guide/seed_dataset.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# BONES-SEED BVH verisini yükleme

[BONES-SEED veri seti](https://huggingface.co/datasets/bones-studio/seed), [SOMA 77 eklemli iskeleti](ISKELETLER.md) ile BVH dosyaları olarak dağıtılan, herkese açık bir optik hareket yakalama veri setidir. Bu sayfa bir SEED BVH dosyasını ayrıştırıp Kimodo'nun iç hareket temsiline dönüştürme adımlarını anlatır.

Kıyaslamanın SEED verisinden gerçek referans (ground-truth) hareketleri çıkarmak için kullandığı akış da buna benzer ([kıyaslama akışına](https://github.com/nv-tlabs/kimodo/blob/main/docs/source/benchmark/pipeline.md) bakın).

## Adım adım dönüştürme

### 1. BVH dosyasını ayrıştırın

`parse_bvh_motion` bir BVH dosyasını okur ve yerel eklem dönüş matrislerini, kök ötelemesini (metre cinsinden) ve kaynak kare hızını döndürür.

```python
from kimodo.skeleton.bvh import parse_bvh_motion

local_rot_mats, root_trans, bvh_fps = parse_bvh_motion(bvh_path)
```

### 2. 30 FPS'ye alt örnekleyin

Kimodo 30 Hz'de çalışır. Kaynak BVH farklı bir kare hızındaysa (BONES-SEED için 120 FPS), adım atlayarak alt örnekleyin:

```python
fps = 30
step = round(bvh_fps / fps)
root_trans = root_trans[::step]
local_rot_mats = local_rot_mats[::step]
```

### 3. Standart T-poza dönüştürün

SEED BVH dinlenme pozu, Kimodo'nun kanonik T-pozundan farklıdır. `to_standard_tpose` işlevi yerel dönüşleri buna göre yeniden eşler, hem yerel hem küresel dönüş matrislerini döndürür:

```python
from kimodo.skeleton import SOMASkeleton77

skeleton = SOMASkeleton77()
local_rot_mats, global_rot_mats = skeleton.to_standard_tpose(local_rot_mats)
```

### 4. Kimodo hareket özniteliklerini hesaplayın

Modelin kullandığı hareket öznitelik tensörünü kurun. Öznitelik düzeni [Hareket temsili](HAREKET-TEMSILI.md) belgesinde anlatılır.

```python
from kimodo.motion_rep import KimodoMotionRep

motion_rep = KimodoMotionRep(skeleton, fps)
feats = motion_rep(local_rot_mats, root_trans, to_normalize=False)
```

### 5. (İsteğe bağlı) kanonikleştirin ve hareket sözlüğünü geri elde edin

Hareketin başlangıç noktasından +Z'ye bakarak başlaması için kanonikleştirin, sonra öznitelikleri tersine çevirerek tam bir hareket sözlüğü elde edin:

```python
can_feats = motion_rep.canonicalize(feats)
motion_dict = motion_rep.inverse(can_feats, is_normalized=False)
```

`motion_dict`; `local_rot_mats`, `global_rot_mats`, `posed_joints`, `root_positions`, `smooth_root_pos`, `foot_contacts` gibi anahtarları olan bir sözlüktür. Kimodo NPZ düzeninin ayrıntıları için [Çıktı biçimleri](CIKTI-BICIMLERI.md) belgesine bakın.

## Tam betik

```python
from kimodo.motion_rep import KimodoMotionRep
from kimodo.skeleton import SOMASkeleton77
from kimodo.skeleton.bvh import parse_bvh_motion

# 1. BVH'yi ayrıştır
local_rot_mats, root_trans, bvh_fps = parse_bvh_motion(bvh_path)

# 2. 30 fps'ye alt örnekle
fps = 30
step = round(bvh_fps / fps)
root_trans = root_trans[::step]
local_rot_mats = local_rot_mats[::step]

# 3. Standart T-poza dönüştür
skeleton = SOMASkeleton77()
local_rot_mats, global_rot_mats = skeleton.to_standard_tpose(local_rot_mats)

# 4. Hareket özniteliklerini hesapla
motion_rep = KimodoMotionRep(skeleton, fps)
feats = motion_rep(local_rot_mats, root_trans, to_normalize=False)

# 5. Kanonikleştir ve tam hareket sözlüğünü al
can_feats = motion_rep.canonicalize(feats)
motion_dict = motion_rep.inverse(can_feats, is_normalized=False)
```

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
