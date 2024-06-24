# Nasıl Kullanılır

Günaydın!

Mahsullerinizi sulama zamanı.

**Sulama Kabı**nızı çıkarın ve güzel mahsullerinizden herhangi birine **sol tıklayın**.

Mahsulleriniz için duyduğunuz sevginin bir mucizesiyle, bir düğmeye bile basmadan geri kalanlarını sulamaya kapılırsınız.

Sulama kabınızı içgüdüsel olarak doldurmak için daha fazla su otomatik olarak alınır, hala açık bir zihin durumunda olursunuz.

Herhangi bir düğmeye basarak erken uyanabilirsiniz.

Endişelenmeyin, sulamaktan bayılmayacaksınız, dayanıklılığınız bitmeden duracaksınız.

# Uygulama

### 1. Tetikleyici

Mod, oyuncunun `sağ tıklamasını` dinleyerek başlar. Oyuncu sulama kabıyla bir bitkiye tıklıyorsa, bot başlar.

### 2. Oyuncunun Çiftliğini Yükleme

Bot önce çiftlik haritası verilerini tarar, karodan karoya geçerek aşağıdaki özellikleri işaretler:

| Özellik  | Açıklama                                          |
|----------|--------------------------------------------------|
| Sulanabilir | Karoya su vermek gerekiyor mu?                     |
| Engel     | Karo, oyuncunun üzerine yürümesine izin veriyor mu? |
| Su        | Oyuncu burada Sulama Kabını doldurabilir mi?         |

Tüm karolar 2D bir diziye yerleştirilir. Sulanabilir mahsuller de kendi dizilerine yerleştirilir.

<img src="https://raw.githubusercontent.com/andyruwruw/stardew-valley-water-bot/main/documentation/implementation/load_map.gif">

Yukarıdaki resim için karolar aşağıdaki gibi renklendirilmiştir:

| Renk       | Sulanabilir | Engel | Su    |
|------------|-------------|-------|-------|
| Yeşil      | Doğru       | Yanlış| Yanlış|
| Koyu Yeşil | Doğru       | Doğru | Yanlış|
| Turkuaz    | Yanlış      | Doğru | Doğru |
| Koyu Mavi  | Yanlış      | Yanlış| Doğru |
| Kırmızı    | Yanlış      | Doğru | Yanlış|

### 3. Gruplanmış Mahsulleri Bulma

Sulama gerektiren mahsul bulunan karolar, derinlemesine arama kullanılarak bitişiklik bazında gruplanır.

<img src="https://raw.githubusercontent.com/andyruwruw/stardew-valley-water-bot/main/documentation/implementation/find_groups.gif">

### 4. Gruplar Arası Seyahat Maliyeti

Bot daha sonra bir gruptan diğerine seyahat maliyetini belirlemek için A* yol bulma algoritmasını kullanır.

Algoritma, her grubun merkezine en yakın karodan başlar.

Her gruba seyahat maliyeti, oyuncunun mevcut konumundan da hesaplanır.

<img src="https://raw.githubusercontent.com/andyruwruw/stardew-valley-water-bot/main/documentation/implementation/cost_matrix.gif">

Bu bize güzel bir maliyet matrisi verir!

|        | Oyuncu | Mor | Sarı | Mavi |
|--------|--------|-----|------|------|
| Oyuncu | -1     | 11  | 5    | 5    |
| Mor    | 11     | -1  | 6    | 8    |
| Sarı   | 5      | 6   | -1   | 6    |
| Mavi   | 5      | 8   | 6    | -1   |

Bu noktada ulaşılamayan gruplar dikkate alınmaz.

### 5. Seyahat Eden Su Adamı

Tüm gruplar arasında en kısa yolu bulmamız gerekiyor, başlangıç noktası oyuncunun mevcut konumu.

Bot, seyahat eden satıcı problemini çözmek için açgözlü bir yaklaşım kullanır.

Başlangıç noktası oyuncunun konumudur.

### 6. Grubu Sulama

Her grup için derinlemesine arama uygulanarak karolar doldurulur.

Her karoda, tüm bitişik (artık köşegenler dahil) de sulanır. Bu, her bloğa yürümeyi atlayıp etrafımızdaki her şeyi sulayabileceğimiz anlamına gelir.

Bir blok üzerine basılamıyorsa, bot en iyi seçeneği seçer ve oradan sular.

<img src="https://raw.githubusercontent.com/andyruwruw/stardew-valley-water-bot/main/documentation/implementation/fill_group.gif">

### 7. Su Bitti!

Sulama kabı azaldığında, bot en yakın su kaynağına gidip dolum yapar.

En yakın doldurma noktası, oyuncunun konumundan genişleme araması kullanılarak bulunur.

Nokta bulunduğunda, bot oyuncuyu suya en yakın noktaya yönlendirir, doldurur ve ardından sulamaya devam eder.

<img src="https://raw.githubusercontent.com/andyruwruw/stardew-valley-water-bot/main/documentation/implementation/refill_water.gif">
