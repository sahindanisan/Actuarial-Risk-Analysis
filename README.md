# 📊 Sigorta Hasar Olasılığı Modellemesi: GLM ve Random Forest Kıyaslaması

## 📝 Projenin Amacı
Bu çalışmada, sigortacılık veri bilimindeki en temel konulardan biri olan "Hasar Olasılığı" (Claim Probability) tahminlemesi üzerine çalıştım. Geleneksel aktüeryal yaklaşım olan GLM (Genelleştirilmiş Doğrusal Model) ile Makine Öğrenmesi (Random Forest) algoritmalarını karşılaştırarak, hangisinin karmaşık riskleri daha iyi yakaladığını analiz ettim.

## ⚙️ Veri Seti ve Kurgu
Gerçek sigorta verileri KVKK kapsamında gizli olduğu için, sektörel dinamikleri yansıtan 5.000 satırlık sentetik bir veri seti kurguladım.
* **Temel Varsayım:** Trafikte genç sürücülerin tecrübesizlikten, ileri yaştaki sürücülerin ise refleks kaybından dolayı daha riskli olduğu gerçeğinden yola çıkarak; Yaş ile Hasar Olasılığı arasına bilerek **U-şekilli (quadratic)** bir ilişki yerleştirdim.

## 🧹 Veri Temizliği ve Ön İşleme
* **Eksik Veriler:** Yaş değişkenindeki boş (NaN) değerleri, olası uç değerlerden (outliers) etkilenmemesi için ortalama yerine **medyan** ile doldurdum.
* **Kategorik Dönüşüm:** Şehir ve Cinsiyet gibi verileri One-Hot Encoding ile sayısal hale getirdim. Burada Kukla Değişken Tuzağı'na (Multicollinearity) düşmemek için `drop_first=True` parametresini kullanarak referans sınıfları belirledim.

## 🧠 Model Karşılaştırması ve Sonuçlar
Modelleri kurarken şeffaflık ve tahmin gücü (AUC) arasındaki farkları test ettim:

1. **Klasik GLM:** Sadece doğrusal "Yaş" verisiyle eğitildiğinde model U-şekilli riski göremedi ve başarısız oldu (Kırmızı Çizgi).
2. **Polinomsal GLM:** Aktüeryal bir bakış açısıyla modele `Yaş²` değişkenini manuel olarak eklediğimde, model gerçek dünya riskine mükemmel uyum sağladı (Mavi Çizgi).
3. **Random Forest:** Doğası gereği karar ağaçlarıyla çalıştığı için hiçbir karesel müdahaleye gerek duymadan U-şeklini kendiliğinden öğrendi (Yeşil Çizgi).

## 🔍 Önemli Bulgu: Dışadeğerleme (Extrapolation) Sorunu
Random Forest genel olarak daha iyi bir skor verse de, grafikte görüldüğü üzere 70+ yaş gibi verinin az olduğu uç noktalarda mantıksız bir düşüş yaşadı (Ezberleme/Overfitting). Bu durum bana, ekstrem risk gruplarında neden hala matematiksel fonksiyonlara dayanan parametrik GLM modellerine ihtiyacımız olduğunu uygulamalı olarak gösterdi.
## 🛠️ Kullanılan Teknolojiler
`Python` `Pandas` `NumPy` `Statsmodels` `Scikit-Learn` `Matplotlib`
