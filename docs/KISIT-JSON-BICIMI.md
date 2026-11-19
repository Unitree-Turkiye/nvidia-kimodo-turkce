> Bu belge nv-tlabs/kimodo deposundaki `docs/source/user_guide/constraints.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Kısıt JSON biçimi

CLI'daki `--constraints` bayrağı, kısıt nesnelerinden oluşan bir liste içeren JSON dosyası bekler.
Biçimi görmenin en kolay yolu, demoyla birlikte gelen örneklere bakmaktır. Çeşitli model türleri için bu örnekler `kimodo/assets/demo/examples` içindedir.

> İpucu: Geçerli bir kısıt dosyası elde etmenin en kolay yolu, kısıtları etkileşimli demoda oluşturup `Save Constraints` düğmesine tıklamaktır.

## Üst düzey yapı

- Dosya bir JSON dizisidir: `[{...}, {...}, ...]`
- Her öğe en az şunları içeren bir nesnedir:
  - `type` (dize)
    - `root2d`, `fullbody`, `left-hand`, `right-hand`, `left-foot`, `right-foot`, `end-effector`
  - `frame_indices` (tam sayı dizisi): Üretilen klip içindeki 0 tabanlı kare dizinleri.


> **Not:** SOMA modellerinde kısıtlar tam `somaskel77` iskeleti üzerinde hazırlanıp gösterilebilir, ama Kimodo onları modele vermeden önce indirgenmiş `somaskel30` temsiline dönüştürür. Ayrıntı için [iskelet](ISKELETLER.md) bölümüne bakın.

## Koordinat uzayı ve birimler

Kısıtlardaki tüm uzamsal değerler, Kimodo'nun iç hareket temsiliyle aynı koordinat sistemini kullanır:

- **Eksenler**: **Y yukarı**, yürüyüş/hareket (lokomosyon) **XZ zemin düzleminde**. Y ekseni yukarıyı gösterir, X ve Z yatay zemin düzlemini oluşturur.
- **Birimler**: **Metre**. Eklem konumları, kök (root) ötelemeleri ve 2B kök koordinatlarının hepsi metre cinsindendir.

### Kanonikleştirme

Eğitim sırasında her hareket, (yumuşatılmış) kök 0. karede XZ başlangıç noktası `(0, 0)` üzerinden başlayacak şekilde *kanonikleştirilir*.
Başlangıçtaki gövde yönelimi (heading, yüzün baktığı yön) rastgele döndürülür ve modele açık bir girdi (`first_heading_angle`) olarak verilir. Böylece model rastgele başlangıç yönelimlerine karşı dayanıklı olur.

Çıkarımda kısıtlar **bu kanonik başlangıç noktasına göre** hazırlanmalıdır:
- 0. karedeki `smooth_root_2d` değerleri `(0, 0)` olmalı, sonraki kareler oradan yer değiştirmeyi ifade etmelidir.
- `root_positions` XZ bileşenleri aynı kuralı izler. Y ise **kalçanın zeminden mutlak yüksekliğidir** (ayakta duruş için genellikle yaklaşık 0,9 m, çömelme ya da oturma için daha düşük).
- `first_heading_angle` (kısıt JSON'unun parçası olmayan bir üretim parametresi) varsayılan olarak `0.0` radyandır (+Z'ye bakar), ama başlangıçtaki bakış yönünü değiştirmek için herhangi bir değere ayarlanabilir.

### Alanlara özgü notlar

| Alan | Uzay | Notlar |
|-------|-------|-------|
| `smooth_root_2d` | `[x, z]` zemin düzlemi (metre) | Kanonik başlangıç noktasına göre. |
| `root_positions` | `[x, y, z]` (metre) | Y, kalçanın zeminden mutlak yüksekliğidir. XZ kanonik başlangıç noktasına göre. |
| `global_root_heading` | `[cos(θ), sin(θ)]` | Ham radyan değeri **olmaz**. Her kare için 2 öğeli kosinüs/sinüs çifti olmalıdır (yani yönelim yön vektörü). |
| `local_joints_rot` | eksen-açı (radyan) | İskeletin dinlenme pozu çerçevesindeki yerel eklem dönüşleri. |

### 0. karede olmayan kısıtlar

0. kareye kısıt eklemek **zorunlu değildir**. İlk kısıtlı kare dizinin daha ilerisindeyse (ör. 45. kare), Kimodo ilk kareleri öğrendiği dağılımdan serbestçe üretir. Bu kareler XZ = (0, 0) yakınından ve `first_heading_angle` ile belirlenen yönelimle başlar. Kısıtın, metin istemi ve hareket süresi göz önüne alındığında bu başlangıç yapılandırmasından ulaşılabilir olması yeterlidir.

## Kısıt türleri
`type` değerine göre ek alanlar zorunlu ya da isteğe bağlıdır. Tüm sayısal diziler düz iç içe JSON listeleridir. Aşağıdaki tanımlarda `T` kısıtlı kare sayısı (yani `frame_indices` sayısı), `J` ise iskelet eklemi sayısıdır.


### `root2d`
2B kök ara noktalarını (waypoint) ve 2B kök yollarını tutar. Şunu gerektirir:

- `smooth_root_2d` (`[T, 2]` biçimli dizi): Verilen `frame_indices` karelerinde zemin düzlemindeki yumuşatılmış kök konumları `[x, z]`.

isteğe bağlı olarak da:
- `global_root_heading` (`[T, 2]` biçimli dizi): Verilen `frame_indices` karelerinde küresel kök yönelim yönü `[cos, sin]`.

### `fullbody`
Eklem konumları üzerinde tam gövde anahtar kare (keyframe) kısıtlarını tutar. Şunları içerir:

- `local_joints_rot` (`[T, J, 3]` biçimli dizi): Kare ve eklem başına **eksen-açı** yerel dönüşleri (radyan). Kısıt eklem konumları bunlardan türetilir.
- `root_positions` (`[T, 3]` biçimli dizi): Kök (kalça) ötelemesi `[x, y, z]`.
- `smooth_root_2d` (isteğe bağlı; `[T, 2]` dizisi): Yumuşatılmış kök konumları `[x, z]`. Verilmezse `root_positions` içindeki `[x, z]` bileşenleri alınır.

`local_joint_rot` doğrudan kısıtlanmaz. Kısıt, verilen eklem dönüşleriyle yapılan ileri kinematikten (FK) çıkan eklem konumları üzerindedir.

### `left-hand` / `right-hand` / `left-foot` / `right-foot`
El ve ayak eklemlerinin konumları ile küresel dönüşleri üzerinde uç işlevci (end-effector) kısıtlarını tutar.

Bunlar `fullbody` ile aynı alanları kullanır. Ancak arka planda yalnız ilgili uç işlevcileri ve kalçayı etkiler. Bu türlerin her biri, eklem adları önceden ayarlanmış bir `end-effector` kısayoludur.

### `end-effector`
Ek bir alan gerektiren genel bir uç işlevci kısıtıdır:

- `joint_names` (dize dizisi): Hangi uç işlevcilerin kısıtlanacağı (ör. `["left_hand"]`, `["right_foot", "left_foot"]`). Kullanılabilir adlar iskelete bağlıdır. Tam eşleme için iskeletin `expand_joint_names()` işlevine bakın.

Bunun dışında `fullbody` ile aynı alanları kullanır (`local_joints_rot`, `root_positions`, isteğe bağlı `smooth_root_2d`).

## Örnekler

### 2B kök ara noktaları

```json
[
  {
    "type": "root2d",
    "frame_indices": [0, 30, 60],
    "smooth_root_2d": [[0.0, 0.0], [0.5, 0.0], [1.0, 0.1]]
  }
]
```

### Tam gövde anahtar karesi

```json
[
  {
    "type": "fullbody",
    "frame_indices": [60],
    "root_positions": [[0.0, 0.96, 1.5]],
    "local_joints_rot": [[[0.0, 0.0, 0.0], "... one [3] per joint ..."]]
  }
]
```

Burada `root_positions` kalçayı x=0, y=0,96 m (ayakta duruş yüksekliği), başlangıç noktasından ileride z=1,5 m konumuna yerleştirir. `local_joints_rot`, iskeletteki her eklem için eksen-açı dönüşlerinden oluşan `[T, J, 3]` biçimli bir dizidir.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
