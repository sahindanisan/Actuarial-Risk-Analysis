# Actuarial-Risk-Analysis
Sigorta risk modellemesi: GLM ve Makine Öğrenmesi (Random Forest) ile hasar olasılığı tahmini ve non-lineer aktüeryal etkilerin karşılaştırması.
# Sigorta Hasar Olasılığı Modellemesi: Aktüeryal GLM ve Makine Öğrenmesi Kıyaslaması
##  Proje Amacı
Bu proje, sigortacılık sektörünün temel problemlerinden biri olan **Hasar Olasılığı (Claim Probability)** tahminlemesini ele almaktadır. Projede; geleneksel parametrik aktüeryal modelleme (GLM) ile modern makine öğrenmesi (Random Forest) yaklaşımlarının, doğrusal olmayan (non-linear) riskleri yakalama ve şeffaflık kapasiteleri karşılaştırılmıştır.

## Metodoloji ve Veri Üretimi
Gerçek müşteri verileri KVKK kapsamında gizlilik politikalarıyla korunduğu için, bu projede sektörel dinamikleri yansıtan 5.000 satırlık sentetik bir veri seti üretilmiştir. 

**Aktüeryal Varsayım:** Trafik sigortalarında 18-25 yaş arası genç sürücüler (tecrübesizlik) ve 65+ yaş üstü sürücüler (refleks zayıflaması) istatistiksel olarak en yüksek risk grubunu oluşturur. Bu gerçeği yansıtmak adına simülasyonda `Yaş` değişkeni ile 'Hasar Olasılığı' arasında bilerek **U-şekilli (quadratic)** bir ilişki tanımlanmıştır.

## Veri Mühendisliği ve Ön İşleme
Makine öğrenmesi modellerinin sağlığını korumak adına şu adımlar atılmıştır:
* **Eksik Veri Yönetimi (Imputation):** Yaş değişkenindeki eksik (NaN) değerler, olası uç değerlerin (outliers) istatistiksel sapma yaratmasını engellemek amacıyla ortalama (mean) yerine **medyan (ortanca)** ile doldurulmuştur.
* **Kategorik Dönüşüm:** Kategorik veriler One-Hot Encoding ile sayısallaştırılmış, ancak **Kukla Değişken Tuzağı'ndan (Dummy Variable Trap - Multicollinearity)** kaçınmak için `drop_first=True` parametresi kullanılarak referans (base) sınıflar oluşturulmuştur.

## Modelleme Takası: Açıklanabilirlik vs. Tahmin Gücü
Endüstride yasal otoritelerin şeffaflık beklentisi ile veri biliminin performans arayışı arasındaki zıtlık şu 3 model üzerinden test edilmiştir:

1. **Klasik GLM (Sadece Doğrusal Yaş):** Modele sadece doğrusal `Yaş` değişkeni verildiğinde, model yaşlılardaki artan riski göremeyip U-şekilli riski yakalamada tamamen başarısız olmuştur (Grafikteki Kırmızı Çizgi).
2. **Polinomsal GLM (Aktüeryal Müdahale):** Klasik modele bir aktüeryal feature engineering adımı olarak `Yaş²` (quadratic) değişkeni eklendiğinde, GLM parabolik bükülmeyi başarmış ve gerçek dünya riskine kusursuz uyum sağlamıştır (Mavi Çizgi). SEDDK gibi otoritelerin talep ettiği şeffaflık ve doğru fiyatlama burada sağlanır.
3. **Random Forest:** Ağaç tabanlı bu model, hiçbir manuel `Yaş²` müdahalesine ihtiyaç duymadan U-şekilli ilişkiyi doğası gereği başarıyla kavramıştır (Yeşil Çizgi). 

## Analitik Bulgu: Dışadeğerleme (Extrapolation) Zafiyeti
Grafik incelendiğinde, Random Forest'ın (Yeşil Çizgi) 70+ yaş gibi verinin seyrekleştiği uç (ekstrem) noktalarda gerçeği yansıtmayan ani bir düşüş yaşadığı görülmüştür. Ağaç tabanlı modeller **dışadeğerleme (extrapolation)** yapamazlar; yani daha önce görmedikleri veya çok az gördükleri veri aralıklarında mantık kurmak yerine ezbere (overfitting) düşerler. 

Bu bulgu, veri azlığının yaşandığı ekstrem risk gruplarında matematiksel bir fonksiyona dayanan parametrik **GLM modellerine neden hala güvenmemiz gerektiğini** kanıtlamaktadır.

## 🛠️ Kullanılan Teknolojiler
`Python` `Pandas` `NumPy` `Statsmodels` `Scikit-Learn` `Matplotlib`
