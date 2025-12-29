<div align="center">
  
  <img src="https://img.icons8.com/external-flaticons-lineal-color-flat-icons/100/external-car-automotive-dealership-flaticons-lineal-color-flat-icons-3.png" alt="AI-Araba-Danismani" width="100" />

  <h1>🚗 AI-Araba-Danismani</h1>
  <h3>RAG (Retrieval-Augmented Generation) Mimarisi ile İkinci El Araç Danışmanı</h3>

  <p>
    <img src="https://img.shields.io/badge/Status-MVP%20Ready-success?style=for-the-badge" alt="Status" />
    <img src="https://img.shields.io/badge/Stack-n8n-ff6e5c?style=for-the-badge&logo=n8n&logoColor=white" alt="n8n" />
    <img src="https://img.shields.io/badge/DB-Supabase%20(Vector)-3ecf8e?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase" />
    <img src="https://img.shields.io/badge/AI-Google%20Gemini%20Pro-4285F4?style=for-the-badge&logo=google&logoColor=white" alt="Gemini" />
  </p>

  <p>
    <a href="#-proje-hakkında">Proje Hakkında</a> •
    <a href="#-teknik-mimari">Teknik Mimari</a> •
    <a href="#-veritabanı-yapısı">Veritabanı</a> •
    <a href="#-kurulum">Kurulum</a>
  </p>
</div>

---

## 💡 Proje Hakkında

Bu proje, ikinci el araç arayan kullanıcıların karmaşık filtreleme sistemleri (Yıl, Yakıt, Vites vb.) arasında kaybolmadan, **doğal dildeki taleplerini anlayıp** en doğru araç önerilerini sunan yapay zeka tabanlı bir satış danışmanıdır.

> **Kullanıcı:** *"800 bin TL bütçem var, az yakan, sanayiye götürmeyen bir aile aracı istiyorum."* > **Sistem:** *"Bütçenize uygun (720k-775k bandında) Fiat Egea ve Renault Clio modellerini buldum. Egea'yı genişliği, Clio'yu yakıt tasarrufu için öneririm..."*

### ✨ Temel Özellikler
* **🗣️ Doğal Dil İşleme (NLP):** Kullanıcının sohbet havasındaki mesajını analiz eder.
* **🔍 Hibrit Arama (Hybrid Search):** Hem *anlamsal aramayı* (Vector Search) hem de *fiyat filtresini* (Metadata Filtering) aynı anda kullanır.
* **⚖️ Etik Veri Kullanımı:** Veri kazıma (scraping) yapılmamış, sentetik veri üretim motoru ile **GDPR/KVKK uyumlu** özgün veri seti oluşturulmuştur.

---

## 🛠 Teknik Mimari

Proje, **Low-Code** ve **High-Code** yaklaşımlarını birleştiren modern bir RAG mimarisine sahiptir.

| Bileşen | Teknoloji | Açıklama |
| :--- | :--- | :--- |
| **Orkestrasyon** | <img src="https://img.shields.io/badge/-n8n-ff6e5c?style=flat-square&logo=n8n&logoColor=white" height="20"/> | API trafiğini ve mantıksal akışı yöneten ana beyin. |
| **Veritabanı** | <img src="https://img.shields.io/badge/-Supabase-3ecf8e?style=flat-square&logo=supabase&logoColor=white" height="20"/> | PostgreSQL üzerinde `pgvector` eklentisi ile vektör ve ilişkisel veri saklama. |
| **Yapay Zeka** | <img src="https://img.shields.io/badge/-Gemini%20Pro-4285F4?style=flat-square&logo=google&logoColor=white" height="20"/> | Embedding (Vektörleştirme) ve Son Kullanıcı Yanıt Üretimi (Generation). |

### 🔄 Çalışma Mantığı (Workflow)

```mermaid
graph LR
A[Kullanıcı Mesajı] --> B(Niyet & Fiyat Analizi - LLM)
B --> C{Fiyat Var mı?}
C -- Evet --> D[Fiyat Filtresi + Vektör Arama]
C -- Hayır --> E[Sadece Vektör Arama]
D --> F[Supabase RPC]
E --> F
F --> G[Sonuçları Birleştir - Aggregate]
G --> H[Danışman Yorumu - Gemini]
H --> I[Kullanıcıya Yanıt]
