# Proje Çalışma Süreci ve Git-Flow Kuralları

Bu belge, takımımızın SENG 321 projesinde uygulayacağı geliştirme, versiyon kontrol ve kod inceleme (review) süreçlerini tanımlamaktadır.

## 1. Branch Düzeni
*   **`main` Branch:** Sadece `develop` branch'inden gelen başarılı Pull Request'ler ile güncellenir. Doğrudan commit atılması yasaktır. Sprint sonu teslim sürümleri burada etiketlenir (Release tag).
*   **`develop` Branch:** Aktif geliştirme sürecinin yürütüldüğü ana daldır. Yeni özellikler ve belgeler buraya birleştirilir. Doğrudan commit atılması yasaktır.
*   **`feature/*` Branch'leri:** Her yeni görev veya belge için `develop` branch'inden ayrılarak oluşturulur. İsimlendirme standardı: `feature/<gorev-adi>` şeklindedir.

## 2. Görev Takip ve Geliştirme Süreci
1.  **Issue Açma:** Her görev için GitHub'da sorumlusu ve amacı belli olan bir Issue açılır.
2.  **Branch Açma:** İlgili Issue için bir `feature/` branch'i oluşturulur.
3.  **Pull Request (PR):** Çalışma bittiğinde `develop` branch'ine doğru bir PR açılır.

## 3. Kod İnceleme (Review) Kuralları
*   Açılan tüm Pull Request'ler, birleştirilmeden önce **en az 1 (bir)** takım arkadaşı tarafından incelenmeli ve onaylanmalıdır. Onay alınmadan PR merge edilemez.
