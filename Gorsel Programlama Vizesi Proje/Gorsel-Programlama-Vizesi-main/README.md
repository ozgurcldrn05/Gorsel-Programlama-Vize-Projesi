Resmi Tatil Takipçisi (C# Console App)

Bu proje, Türkiye’nin 2023–2025 yılları arasındaki resmi tatillerini bir API üzerinden çekip kullanıcıya sunan bir C# konsol uygulamasıdır.
Tatil bilgileri Nager.Date API üzerinden alınır ve uygulama içinde saklanarak çeşitli arama–listeleme işlemleri yapılabilir.

📌 Özellikler

✔️ API'den tatil verilerini çekme (2023–2025)
✔️ Yıla göre tatil listeleme
✔️ Gün–Ay formatında (gg-aa) tarih arama
✔️ Tatil adına göre arama
✔️ Tüm tatilleri bir arada listeleme
✔️ Kullanıcı dostu, menü tabanlı arayüz

🛠️ Kullanılan Teknolojiler

💻 C# (.NET Console App)

🌐 HttpClient ile API istemcisi

📄 System.Text.Json ile JSON ayrıştırma

🔍 LINQ ile filtreleme ve arama işlemleri

🔗 Kullanılan API

Uygulama resmi tatil verilerini şu adresten alır:

https://date.nager.at/api/v3/PublicHolidays/{yil}/TR

📂 Proje Yapısı
📁 tatiluygulama
 ├── Program.cs
 ├── Tatil.cs
 └── README.md

🚀 Çalıştırma

Projeyi bilgisayarınıza indirin:

git clone https://github.com/ozgurcldrn05/Gorsel-Programlama-Vize-Projesi.git


Visual Studio veya VS Code ile açın.

Uygulamayı çalıştırın:

dotnet run

📸 Uygulama Menüsü
===== Resmi Tatil Takipçisi =====
1. Tatil listesini göster (Yıl seçmeli)
2. Tarihe göre tatil ara (gg-aa formatı)
3. İsme göre tatil ara
4. Tüm tatilleri 3 yıl boyunca göster (2023–2025)
5. Çıkış

🔍 Örnek Çıktı
[2024-10-29] Cumhuriyet Bayramı (Republic Day)
[2024-04-23] Ulusal Egemenlik ve Çocuk Bayramı (National Sovereignty and Children's Day)

🤝 Katkı Sağlama

İsteyen herkes projeyi fark edip geliştirme yapabilir.
Pull request göndermekten çekinme! ✨

📜 Lisans

Bu proje MIT lisansı ile paylaşılmıştır.