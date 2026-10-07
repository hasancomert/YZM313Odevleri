# MemGraphRAG – Google Colab Ödevi

Bu klasörde, [XMUDeepLIT/MemGraphRAG](https://github.com/XMUDeepLIT/MemGraphRAG) reposunu (KDD 2026, [arXiv:2606.00610](https://arxiv.org/abs/2606.00610)) Google Colab'da uçtan uca çalıştıran notebook bulunur.

| Dosya | Açıklama |
|---|---|
| `MemGraphRAG_Colab.ipynb` | Teslim edilecek Colab notebook'u (Türkçe açıklamalar, kod ve değerlendirme) |

## Notebook neler içeriyor?

1. MemGraphRAG'ın çalışma mantığı: üç katmanlı bellek, çok ajanlı indeksleme, PPR tabanlı erişim
2. Dil modeli seçimi ve gerekçesi: **Qwen2.5-7B-Instruct** (Ollama, 4-bit, ücretsiz T4 GPU). İsteğe bağlı olarak GPT-4o-mini veya OpenAI uyumlu başka bir servis de seçilebilir.
3. Repo klonlama ve Colab ile uyumlu bağımlılık kurulumu (`requirements.txt` Python 3.13'te kurulamıyor)
4. Kaynak koda uygulanan 10 küçük yama (4 çökme, 3 mantık/tasarım, 2 dayanıklılık, 1 log düzeltmesi) ve LLM sunucusu kapanırsa onu yeniden başlatıp indekslemeyi tekrar deneyen otomatik kontrol
5. HotpotQA alt kümesinin README'nin beklediği formatta hazırlanması
6. Repodaki örneğin çalıştırılması: `code/index.py` (indeksleme) → `code/retrieval_dataset_test.py` (erişim + QA)
7. Ara çıktıların incelenmesi: OpenIE üçlüleri, şema katmanı, ontoloji filtreleme, çelişki tespiti/çözümü, graf
8. Değerlendirme: EM / F1 / içerme doğruluğu / Recall@k; klasik dense RAG ve 3 MemGraphRAG varyantı
9. Karşılaşılan hatalar ve çözümleri
10. MemGraphRAG ile klasik RAG ve GraphRAG'ın teknik karşılaştırması

## Nasıl çalıştırılır?

1. Notebook'u Colab'da açın. İki yol var:
   - Colab'da `Dosya → Not defteri yükle` ile `MemGraphRAG_Colab.ipynb` dosyasını yükleyin.
   - Doğrudan GitHub'dan açın: [Colab'da aç](https://colab.research.google.com/github/hasancomert/RandomOdevler/blob/ccr-d50f9cf5-gvz6tq/MemGraphRAG-Odev/MemGraphRAG_Colab.ipynb). Repo gizliyse Colab sizden GitHub yetkisi ister; dal birleştirildikten sonra bağlantıdaki dal adını `main` olarak değiştirin.
   - Teslim için notebook'un **kendi Google Drive'ınızda** bir kopyası olmalı (`Dosya → Drive'a kopya kaydet`). Paylaşım bu kopya üzerinden yapılır.
2. `Çalışma zamanı → Çalışma zamanı türünü değiştir → T4 GPU` seçin.
3. `Çalışma zamanı → Tümünü çalıştır`. T4 üzerinde toplam süre tahminen 30–45 dakikadır (gerçek indeksleme süresi Bölüm 9'un çıktısında yazdırılır); sürenin çoğu indeksleme adımında harcanır.
4. GPT-4o-mini kullanmak isterseniz, sol menüdeki 🔑 **Secrets** bölümüne `OPENAI_API_KEY` ekleyin ve yapılandırma hücresinde `LLM_BACKEND = "openai"` yapın.

## Teslim öncesi kontrol listesi

- [ ] Notebook'un en üstüne ad, soyad ve öğrenci numarası yazıldı.
- [ ] Tüm hücreler Colab'da hatasız çalıştı ve çıktılar notebook'ta kayıtlı (`Dosya → Kaydet`).
- [ ] Bölüm 7.1'in geçerli JSON, Bölüm 9'un "İndeksleme süresi" yazdırdığı ve Bölüm 12–15 hücrelerinin çıktı ürettiği kontrol edildi.
- [ ] Bölüm 12.1'deki otomatik özet okundu; Bölüm 15–16'daki yorumlar bu sayılarla çelişmiyor (gerekirse bir-iki cümle eklendi).
- [ ] Kendi çalıştırmanızda farklı bir hata aldıysanız Bölüm 14'teki tabloya eklendi.
- [ ] `Paylaş` düğmesiyle **coskunmustafa@ankara.edu.tr** ve **betulerkantarci@ankara.edu.tr** adreslerine erişim verildi.
- [ ] Son teslim: **8 Ekim 2026, 12:00**.

## Test notu

Notebook, Colab'ın güncel sürümleriyle (Python 3.13, torch 2.11, transformers 5.13) aynı bir ortamda baştan sona çalıştırılarak test edildi. Bu test ortamında Hugging Face ve Ollama sunucularına erişim yoktu. Bu yüzden testte LLM yerine OpenAI uyumlu bir sahte (mock) sunucu, BGE yerine de küçük, rastgele ağırlıklı bir model kullanıldı. Bu test kurulumu, kodun ve yamaların doğru çalıştığını doğrular; elde edilen sayılar ise anlamlı değildir. **Gerçek sonuçlar notebook Colab'da T4 GPU ile çalıştırıldığında üretilir.**
