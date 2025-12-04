


















    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Text.Json;
    using System.Threading.Tasks;

    namespace tatiluygulama
    {
        public class Tatil
        {
            // APIden gelen bilgilerin aktarıldığı kısım
            public string date { get; set; }
            public string localName { get; set; }
            public string name { get; set; }
            public string countryCode { get; set; }
            public bool @fixed { get; set; }
            public bool global { get; set; }

            // ekrana yazdırmak için kısaltılmış kod
            public override string ToString()
            {
            return $"[{date}] {localName} ({name})";
            }
        }

    class Program
    {
        // tüm tatillerin tutulduğu ana liste
        private static List<Tatil> tumTatiller = new List<Tatil>();

        private static readonly HttpClient istemci = new HttpClient();

        static async Task Main(string[] argumanlar)
        {
            Console.WriteLine("apiden veri alınıyor");

            // verilerin çekildiği kısım
            await TatilleriYukleAsync();

            Console.Clear();
            bool cikisYapilsinMi = false;

            while (!cikisYapilsinMi)
            {
                Console.WriteLine("\n===== Resmi Tatil Takipçisi =====");
                Console.WriteLine("1. Tatil listesini göster (Yıl seçmeli)");
                Console.WriteLine("2. Tarihe göre tatil ara (gg-aa formatı)");
                Console.WriteLine("3. İsme göre tatil ara");
                Console.WriteLine("4. Tüm tatilleri 3 yıl boyunca göster (2023–2025)");
                Console.WriteLine("5. Çıkış");
                Console.Write("Seçiminiz: ");

                string secim = Console.ReadLine();

                switch (secim)
                {
                    case "1":
                        YilaGoreTatilleriGoster();
                        break;
                    case "2":
                        TariheGoreAra();
                        break;
                    case "3":
                        IsmeGoreAra();
                        break;
                    case "4":
                        TumTatilleriGoster();
                        break;
                    case "5":
                        cikisYapilsinMi = true;
                        Console.WriteLine("Uygulamadan çıkılıyor...");
                        break;
                    default:
                        Console.WriteLine("Geçersiz seçim, lütfen tekrar deneyin.");
                        break;
                }
            }
        }

        // apıden verilerin alındığı kısım
        private static async Task TatilleriYukleAsync()
        {
            int[] yillar = { 2023, 2024, 2025 };

            foreach (var yil in yillar)
            {
                string adres = $"https://date.nager.at/api/v3/PublicHolidays/{yil}/TR";
                try
                {
                    // apiden string olarak json verisi alınıyor
                    string jsonCevap = await istemci.GetStringAsync(adres);

                    // jsonu tatil listesine dönüştürüyor
                    var oYilinTatilleri = JsonSerializer.Deserialize<List<Tatil>>(jsonCevap);

                    if (oYilinTatilleri != null)
                    {
                        tumTatiller.AddRange(oYilinTatilleri);
                    }
                }
                catch (Exception hata)
                {
                    Console.WriteLine($"Hata ({yil}): Veri çekilemedi. {hata.Message}");
                }
            }
            Console.WriteLine($"Toplam {tumTatiller.Count} tatil kaydı hafızaya alındı.");
        }

        // yıla göre listeleme
        private static void YilaGoreTatilleriGoster()
        {
            Console.Write("Listelemek istediğiniz yılı girin (2023, 2024, 2025): ");
            string girilenYil = Console.ReadLine();

            // kullanıcının girdisiyle eşleşip eşleşmediğini kontrol ediyor
            var sonuclar = tumTatiller.Where(t => t.date.StartsWith(girilenYil)).ToList();

            if (sonuclar.Any())
            {
                Console.WriteLine($"\n--- {girilenYil} Yılı Tatilleri ---");
                foreach (var tatil in sonuclar)
                {
                    Console.WriteLine(tatil);
                }
            }
            else
            {
                Console.WriteLine("Bu yıl için kayıt bulunamadı veya hatalı giriş.");
            }
        }

        // tarihe göre arama
        private static void TariheGoreAra()
        {
            Console.Write("Aramak istediğiniz tarih (gg-aa örn: 29-10): ");
            string girilenTarih = Console.ReadLine();

            // kullanıcıdan gün-ay isteniyor ama apide yıl-ay-gün formatında
            var sonuclar = tumTatiller.Where(t => {
                DateTime tarihNesnesi;
                // string tarihi DateTime a çevir
                if (DateTime.TryParse(t.date, out tarihNesnesi))
                {
                    // kullanıcınınkiyle karşılaştır
                    return tarihNesnesi.ToString("dd-MM") == girilenTarih;
                }
                return false;
            }).ToList();

            if (sonuclar.Any())
            {
                Console.WriteLine($"\n--- {girilenTarih} Tarihindeki Tatiller (3 Yıllık) ---");
                foreach (var tatil in sonuclar)
                {
                    Console.WriteLine(tatil);
                }
            }
            else
            {
                Console.WriteLine("Bu tarihte herhangi bir resmi tatil bulunamadı.");
            }
        }

        // isme göre arama
        private static void IsmeGoreAra()
        {
            Console.Write("Tatil adı (veya bir kısmı): ");
            string anahtarKelime = Console.ReadLine().ToLower();

            // isme göre filtreleme
            var sonuclar = tumTatiller
                .Where(t => t.localName.ToLower().Contains(anahtarKelime) || t.name.ToLower().Contains(anahtarKelime))
                .ToList();

            if (sonuclar.Any())
            {
                Console.WriteLine($"\n--- '{anahtarKelime}' İçeren Tatiller ---");
                foreach (var tatil in sonuclar)
                {
                    Console.WriteLine(tatil);
                }
            }
            else
            {
                Console.WriteLine("Eşleşen tatil bulunamadı.");
            }
        }

        // tüm listeyi gösterme-
        private static void TumTatilleriGoster()
        {
            Console.WriteLine("\n--- 2023-2025 Tüm Resmi Tatiller ---");
            foreach (var tatil in tumTatiller)
            {
                Console.WriteLine(tatil);
            }
        }
    }
}
