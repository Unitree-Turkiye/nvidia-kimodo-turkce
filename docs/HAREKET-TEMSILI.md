> Bu belge nv-tlabs/kimodo deposundaki `docs/source/key_concepts/motion_representation.md` dosyasının Türkçe çevirisidir (Apache-2.0, değişiklik: Türkçeleştirme).

# Hareket temsili

Kimodo, yumuşatılmış bir kök temsilini küresel eklem konumları, dönüşleri ve çeşitli yardımcı özniteliklerle birleştiren bir hareket temsili kullanır.
Tüm ayrıntılar için [teknik rapora](https://research.nvidia.com/labs/sil/projects/kimodo/assets/kimodo_tech_report.pdf) bakın.

Temsil `kimodo/motion_rep/reps/kimodo_motionrep.py` içinde uygulanır ve bu öznitelik temsiline girip çıkmayı kolaylaştırır.

## Koordinat sistemi

Tüm hareket öznitelikleri şu özelliklerde sağ elli bir koordinat sistemi kullanır:

- **Y yukarı**
- **+Z ileri**

## Yumuşatılmış kök temsili

Yol izleme kısıtlarını daha doğal ve denetlenebilir kılmak için küresel kök konumunda
yumuşatılmış bir kök yörüngesi kullanıyoruz. Yumuşatma, genel hareket yönünü korurken
leğen kemiğindeki yüksek frekanslı titremeyi giderir. Böylece kullanıcıların çizdiği
2B ara noktalar ya da yollar temiz kalır ve üretim sırasında kolayca eşleşir, leğen kemiği de
yumuşatılmış eğrinin çevresinde doğal biçimde hareket etmeye devam eder.

![Yumuşatılmış kök temsilinin karşılaştırması](https://raw.githubusercontent.com/nv-tlabs/kimodo/main/docs/source/_static/smoothed_root.png)

## Poz özniteliği

Her karede poz öznitelik vektörü şunların birleştirilmesidir:

- **Yumuşatılmış kök konumu** (`smooth_root_pos`, 3): Yumuşatılmış leğen kemiği/kök konumu.
  x/z bileşenleri zemin düzlemindeki hareketi izler, y yüksekliği saklar.
- **Küresel kök yönelimi** (`global_root_heading`, 2): Kökün `[cos(theta), sin(theta)]`
  yönelim yönü.
- **Yerel eklem konumları** (`local_joints_positions`, `J x 3`): Yumuşatılmış kök x/z
  uzaklığı uygulanmış, leğen kemiğine göreli uzaydaki eklem konumları.
- **Küresel eklem dönüşleri** (`global_rot_data`, `J x 6`): Her eklemin küresel yönelimi için 6B
  dönüş temsili.
- **Eklem hızları** (`velocities`, `J x 3`): Küresel eklem hızları.
- **Ayak temasları** (`foot_contacts`, 4): Sol/sağ ayak temas noktaları için ikili
  temas göstergeleri.

---

**Unitree G1 Türkiye:** [Unitree G1 insansı robot ürün sayfası](https://www.robotlar.org/tr/insansi-robotlar/unitree-g1) · [G1 EDU için teklif alın](https://www.robotlar.org/tr/fiyat/unitree-g1-edu-fiyat) · Unitree Robotics Türkiye Resmi Distribütörü Robotlar.org
