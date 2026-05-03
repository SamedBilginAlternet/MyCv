# n11 Final Case — 10 Dakikalık Sunum Script'i

**Toplam hedef süre:** 10:00
**Canlı URL:** https://n11proje.samedbilgin.com

---

## Bölüm 1 — Açılış

| # | Süre | Script (söylenecek) | Ekranda |
|---|------|---------------------|---------|
| 0 | 0:00–0:25 | Merhaba, ben Samed Bilgin. Bootcamp kapsamında yaptığım projemden bahsedeceğim. Akışta önce uygulamamın feature'larını canlı olarak göstereceğim. Daha sonrasında bu feature'ları nasıl ve neden bu şekilde implement ettiğimden devam edeceğim. Son olarak error tracking, deployment ve test gibi süreçlerden bahsedip anlatımımı sonlandıracağım. | Mağaza ana sayfası tam ekran (incognito) + URL bar `n11proje.samedbilgin.com` görünür. 0:14–0:24 arası 3 hızlı flash: storefront → admin paneli → Jaeger trace ekranı (trailer mantığı). |

---

## Bölüm 2 — Canlı Demo

| # | Süre | Script | Ekranda |
|---|------|--------|---------|
| 1 | 0:25–0:32 | Login sayfasıyla başlayalım, burada üç seçeneğimiz bulunuyor: Google OAuth2, telefon SMS ve klasik email/şifre girişi yapılabiliyor. | Login sayfası — 3 buton (Google / Telefon / Email) net görünür, mouse her birinin üzerinden hızlıca geçer. |
| 2 | 0:32–0:42 | Google ile giriyorum. Spring Security OAuth2 Client üzerinden — başarılı callback'te backend kendi JWT'sini mintliyor, frontend Google token'ını görmüyor. | "Google ile giriş yap" butonuna tıkla → Google consent ekranı (CapCut'ta 2x hızlandır) → callback → anasayfa. Sağ üstte avatar görünür. |
| 3 | 0:42–0:50 | Anasayfa açıldı — kategoriler, öne çıkan ürünler. Görseller MinIO'dan, CDN üzerinden geliyor. | Anasayfada yavaşça scroll, kategorileri ve banner'ı göster. Bir kategoriye tıkla → ürün listesi. |
| 4 | 0:50–1:08 | Sağ altta AI shopping assistant bulunuyor. "Kırmızı spor ayakkabı öner" diye soruyorum. Bot function calling ile gerçek katalogdan ürünleri çekti — uydurma değil. Sohbet PostgreSQL'de UUID session altında saklanıyor — sayfa yenilense devam ediyor. | Chatbot ikonu tıkla → panel açılır → "kırmızı ayakkabı öner" yaz → bot ürünlerle cevap verir → ürün linkine fareyle git. |
| 5 | 1:08–1:21 | Ürün detayına geçiyorum. Galeri, fiyat, stok, kullanıcı yorumları. Favorilere ekliyorum — wishlist user'a bağlı, kalıcı. | Ürün detay sayfası: galeri resimleri, fiyat, yorumlar bölümü. Kalp ikonuna tıkla → favori animasyonu. |
| 6 | 1:21–1:36 | Stok kontrolü iki katmanda var. Sepete eklerken cart-service product-service'e soruyor, mevcut stoktan fazla istenirse direkt hata. Asıl atomic kontrol checkout anında: backend conditional UPDATE ile stoğu anında düşürüyor — race condition imkansız. | Sepete ekleme butonu — sayaç artışı görünür. (İsteğe bağlı flash: 2-sekme yan yana, kırmızı "stok yetersiz" toast'ı 1 sn göster.) |
| 7 | 1:36–1:58 | Sepete ekliyorum — cart-service'ten geliyor. Giriş yapmasak bile sepete ekleyebiliyoruz (guest cart), login/logout yapsak da sepet korunuyor. UX prensiplerinde büyük e-ticaret markalarından ilham aldım: JIT data collection — kullanıcı kaydı ve adres bilgisi ödeme anında isteniyor, kayıt sırasında değil. Ödemeye geçiyorum. | Sepet ikonuna tıkla → sepet sayfası → "Ödemeye Geç" butonu. |
| 8 | 1:58–2:11 | Adres ekleme kısmında il ve ilçe sabit listeden geliyor — Türkiye'deki bütün şehir ve ilçeleri kataloga koydum, kafanıza göre yazamıyorsunuz. Adres tipi de Ev, Ofis veya Diğer olarak seçiliyor. Telefonu yanlış girersem hemen uyarıyor. Kaydedip ödemeye geçiyorum. | "Yeni adres ekle" modal'ı açılır → İl: İstanbul, İlçe: Kadıköy seç → Ev/Ofis pill (3 buton tıkla) → telefon yanlış gir (kırmızı uyarı görünür) → doğru gir → Kaydet. |
| 9 | 2:11–2:25 | İyzico'nun sandbox ortamıyla ödüyorum. Test kartını giriyorum, ödeme geçiyor. conversationId olarak sipariş ID'mi gönderiyorum — aynı ödeme yanlışlıkla iki kez tetiklenirse İyzico tek bir işleme bağlıyor. Idempotency vendor seviyesinde sağlanmış oluyor. Sipariş "ödeme bekleniyor" durumundan "onaylandı" durumuna geçti. | İyzico sandbox ödeme sayfası → test kartı `5528 7900 0000 0008` (önceden doldurulmuş) → "Öde" → success → sipariş onay sayfası. |
| 10 | 2:25–2:35 | Mail kutuma geçiyorum — sipariş onay maili gelmiş. Bunu RabbitMQ üzerinden asenkron gönderiyoruz, ödeme akışı mail için beklemiyor. Resend SMTP servisi üzerinden çıkıyor. | Mail kutusu sekmesi (önceden açık) → yeni "Sipariş Onaylandı" maili → mail aç, içerik göster. |
| 11 | 2:35–2:41 | **[YENİ — admin geçişi]** Şimdi admin paneline geçiyorum — `/admin` path'i altından açılıyor, Caddy ile path-based mount yaptım. | URL bar'a `/admin` ekle → admin paneli açılır → Siparişler sekmesine geç. |
| 12 | 2:41–2:55 | Statüyü "Kargoya verildi" yapıp tracking numarasını giriyorum. Üçüncü mail geldi — bu sefer içinde kargo takip linki var. Tracking number sipariş tablosunda saklanıyor, mail template'i Thymeleaf ile dinamik render ediliyor. | Sipariş detay → statü dropdown → "Kargoya verildi" + tracking no `TR1234567890` → kaydet → mail kutusuna dön → 3. mail görünür → mail aç, takip linkini fareyle göster. |
| 13 | 2:55–3:07 | Son detay: bir adresi silmek isteyince özel onay penceresi açılıyor. Tarayıcının default'u yerine kendi tasarladığım dialog — tüm popup'lar aynı bileşenden, animasyonlu, Escape ile kapanır. | Profil → Adreslerim → bir adresin sil ikonuna tıkla → ConfirmDialog animasyonla açılır → Escape'e bas → kapanır. |

---

## Bölüm 3 — Mimari & Tasarım Kararları

| # | Süre | Script | Ekranda |
|---|------|--------|---------|
| 14 | 3:07–3:18 | Demo kısmı bitti. Şimdi bunun arkasında ne var, nasıl çalışıyor ona geçelim. İlk olarak genel mimariyi göstereyim — sistemde kaç servis var, birbirleriyle nasıl konuşuyorlar. | 0.5 sn fade-to-black → "Bölüm 2: Mimari & Tasarım Kararları" title kartı (1 sn) → boş diyagram çerçevesi (3 katman başlıkları görünür: Client / Services / Storage). |
| 15 | 3:18–3:34 | Diyagramda gördüğünüz gibi sistem 3 katmandan oluşuyor: solda client tarafı (mağaza ve admin paneli React'le yazılmış), ortada 7 mikroservis, sağda storage katmanı. 7 servis şunlar: auth, product, cart, order, payment, notification, chatbot. Hepsinin sorumluluğu net — her biri kendi domain'ine bakıyor. Önlerinde bir API Gateway var, tek giriş noktası — JWT doğrulama burada, route table burada. | Diyagram fade-in: Client kutuları → Services kutuları (7 servis isimle) → API Gateway kutusu vurgulu → Storage kutuları. |
| 16 | 3:34–4:24 | Servisler arası iletişimi iki ana kategoride düşünelim. Senkron tarafta REST kullanıyorum — toplam 7 farklı çağrı var. Mesela cart-service her sepet işleminde product-service'ten ürün bilgisi çekiyor. Order-service checkout'ta üç farklı servisle konuşuyor: cart'tan sepeti alıyor, auth'tan teslimat adresini, product'tan da stoğu rezerve ediyor — bu sonuncu internal token ile korunuyor. Chatbot ise hem Groq LLM API'sine hem product-service'e gidiyor — function calling'le gerçek katalogdan öneriyor. Asenkron tarafta RabbitMQ — saga.exchange adında bir topic exchange var, 9 farklı event akıyor. Order-service sipariş yaşam döngüsünü yayınlıyor: order.created, confirmed, cancelled, processing, shipped, delivered. Payment-service ödeme sonuçlarını: payment.succeeded, payment.failed. Product-service de günlük cron ile low-stock-report çıkartıyor. Consumer tarafına bakarsak: notification-service 4 mail event'ini ve düşük stok alarmını dinliyor. Cart-service order.confirmed'da sepeti temizliyor, order.created'da kuponu atomic increment ediyor, order.cancelled'da release ediyor. Product-service order.cancelled'ı dinleyip stoğu geri ekliyor — bu saga compensation. Payment-service ise order.created'ı dinleyip ödemeyi başlatıyor. Bu yapıya saga choreography deniyor — ortada bir orchestrator yok, her servis kendi kuyruğunu dinleyip kendi işini yapıyor. Mesaj kabul edilemezse DLX (dead-letter exchange)'e düşüyor, manuel inceleme için durup bekliyor. | Diyagramda animasyonlu oklar: önce mavi düz REST okları (cart→product, order→cart/auth/product, chatbot→product/groq) tek tek belirir → sonra kırmızı kesikli RabbitMQ okları (saga.exchange merkezde, event'ler servislere dağılır). RabbitMQ kutusu vurgulanır. DLX kutusu sağ alta belirir. |

---

## Bölüm 4 — Teknik Derinlik

| # | Süre | Script | Ekranda |
|---|------|--------|---------|
| 17 | 4:24–4:39 | İki token var: access 15 dakikalık, refresh 7 günlük ve HttpOnly cookie'de — JavaScript erişemiyor, XSS'te bile çalınamıyor. Her refresh'te yeni token üretiyorum, eskisi geçersiz oluyor. Biri eski token'ı ikinci kez kullanmaya kalkarsa çalındı kabul edip tüm aileyi iptal ediyorum — hem saldırgan hem gerçek kullanıcı düşer, log'a alarm gider. | DevTools Network tab — login response'un Set-Cookie header'ı (HttpOnly + Secure + SameSite vurgulu). Sonra ekran değişir: "rotation" diyagramı (token1 → kullanıldı → token2 → eğer token1 tekrar kullanılırsa → tüm aile X). |
| 18 | 4:39–4:54 | Telefon SMS girişi için Firebase Authentication kullanıyorum. Frontend Firebase'den ID token alıyor, backend bu token'ı service account ile doğrulayıp kendi JWT'sini mintliyor — yani kullanıcının firebase token'ı sistemden hiç geçmiyor. | Firebase phone OTP login screenshot'ı (kısa flash, 5 sn) + arka planda backend'in service account credential dosyasını referansla göster. |
| 19 | 4:54–5:13 | Redis'i distributed cache olarak kullanıyorum. Ürün detayları 5 dakika, kategoriler 1 saat ömürlü. `@Cacheable` ile otomatik cache, admin bir ürünü güncellediğinde `@CacheEvict` ile 4 cache key birden temizleniyor. Görseller için MinIO — S3 API uyumlu object storage. Caddy üstüne CDN endpoint ekledim, browser'da 30 gün cache'leniyor. | IDE: `CacheConfig.java` TTL bloğu → `ProductAdminService.java` 4'lü `@CacheEvict` bloğu kırmızı çerçevede. Ekran değişir: MinIO console + Caddyfile CDN block. |
| 20 | 5:13–5:36 | Infisical'ı secrets management için kullanıyorum — kodda hiçbir şifre yok, droplet boot'unda `sync-env.sh` Infisical'dan çekiyor. Secret rotate etmek istediğimde sadece UI'dan değiştiriyorum, kod değişikliği gerekmiyor. Sentry ile error tracking — frontend ve backend için ayrı projeler, release tagging deploy SHA'sından geliyor. Yani "hangi commit hangi hatayı üretti" direkt görünüyor. Jaeger ile distributed tracing — bir checkout request'i 6 servisten geçiyor, hepsi tek trace'te birleşiyor. Correlation ID RabbitMQ event'lerine de biniyor, mailin neden gönderildiğini tek ID ile takip edebiliyorum. | 3 ekran sırayla: Infisical UI → Sentry dashboard (release tagged) → Jaeger trace tree (6 span'li checkout). |

---

## Bölüm 5 — Deploy

| # | Süre | Script | Ekranda |
|---|------|--------|---------|
| 21 | 5:36–5:40 | Şimdi deploy tarafına geçelim — kod nasıl GitHub'dan canlıya çıkıyor? | "Bölüm 5: Deploy" başlık kartı (1 sn) → GitHub Actions workflow runs sayfası açılır. |
| 22 | 5:40–5:59 | GitHub Actions kullanıyorum. Push olduğunda matrix build çalışıyor — 7 backend servisi paralel olarak Jib ile imaja çevriliyor, frontend ve admin paneli ayrı Docker build'leriyle. Hepsi GHCR (GitHub Container Registry)'a push ediliyor. Bütün build cache'lenmiş, ortalama 3 dakikada bitiyor. | GitHub Actions sayfası: workflow run, matrix grid (7 backend servis + 2 frontend) yeşil checkmarkler. GHCR packages sayfası flash. |
| 23 | 5:59–6:22 | Build bittiğinde DigitalOcean droplet'ine SSH ile bağlanıp `docker compose pull` + `up -d` çalıştırıyor. Caddyfile değişmişse force-recreate yapıyor ki bind-mount edilmiş config kesin yenisini okusun. Caddy reverse proxy olarak çalışıyor — Let's Encrypt'ten otomatik TLS alıyor, `/api/*` backend'e, `/admin/*` admin paneline, kalan her şey storefront'a yönleniyor. DB migration ise Flyway ile her servisin kendi `db/migration/` klasöründen otomatik. | Caddyfile snippet (matcher satırı sarı highlight) → DigitalOcean droplet dashboard → Flyway migration listesi (`V1__...sql`, `V2__...sql`). |

---

## Bölüm 6 — Test

| # | Süre | Script | Ekranda |
|---|------|--------|---------|
| 24 | 6:22–7:02 | **[YENİ — test segmenti]** Test tarafına gelelim. Unit testler Mockito ile — atomic stok kontrolü, validator'lar gibi kritik path'ler. Asıl güçlü kısım integration testleri: Testcontainers ile her test öncesi gerçek PostgreSQL + RabbitMQ + Redis container'ı ayağa kalkıyor, mock kullanmıyorum. En karmaşık testim saga compensation E2E — checkout → payment fail → stock release event → product DB stok geri yükleniyor. Bu testler GitHub Actions'da her PR'da otomatik koşuyor, yeşil olmadan merge yok. | IDE: `StockReservationServiceTest.java` 4 testi göster → terminal `mvn test` çıktısı (47 tests passed, 2.3s). Ekran değişir: Testcontainers boot logları ("Starting postgres container") → GitHub Actions test job yeşil checkmark. |

---

## Bölüm 7 — Kapanış

| # | Süre | Script | Ekranda |
|---|------|--------|---------|
| 25 | 7:02–7:32 | **[YENİ — kapanış]** Özetle: 7 Spring Boot mikroservis, React storefront ve admin panel, PostgreSQL + Redis + RabbitMQ + MinIO storage, Jaeger + Sentry observability, Caddy + GitHub Actions + Infisical ile deploy edilmiş. Üçüncü parti olarak İyzico, Firebase, Groq, Resend entegre. Hepsi tek DigitalOcean droplet'inde canlı. Kod ve canlı URL açıklamada. Beni dinlediğiniz için teşekkürler. | Tech logo grid (15 sn boyunca yavaşça doluyor): Spring, React, PostgreSQL, Redis, RabbitMQ, MinIO, Jaeger, Sentry, Caddy, Docker, GitHub Actions, İyzico, Firebase, Groq, Resend. Son 5 sn: canlı URL büyük ekranda + GitHub URL alt başlık. |

---

## Süre Özeti

| Bölüm | Başlangıç | Bitiş | Süre |
|-------|-----------|-------|------|
| Açılış | 0:00 | 0:25 | 0:25 |
| Demo (13 sahne) | 0:25 | 3:07 | 2:42 |
| Mimari (3 cümle) | 3:07 | 4:24 | 1:17 |
| Teknik Derinlik (4 cümle) | 4:24 | 5:36 | 1:12 |
| Deploy (3 cümle) | 5:36 | 6:22 | 0:46 |
| Test | 6:22 | 7:02 | 0:40 |
| Kapanış | 7:02 | 7:32 | 0:30 |
| **TOPLAM** | | | **7:32** |

**Buffer: 2:28** — opsiyonel eklemeler için kullanılabilir (saga compensation görsel demo, atomic UPDATE SQL flash, rate limiter, aggregated Swagger).

---

## Kayıt Öncesi Hazırlık Listesi

### Tarayıcı / sistem
- [ ] Tarayıcı incognito + zoom %110
- [ ] Bookmark bar gizli
- [ ] Notifications kapalı (Slack, mail, telefon)
- [ ] Pencere boyutu 1920x1080
- [ ] İkinci ekranda script açık olsun

### Test verisi
- [ ] Görselli, yorumlu, stok=1+ olan bir test ürünü
- [ ] Chatbot test edilmiş, "kırmızı ayakkabı" sorusuna cevap veriyor
- [ ] İyzico test kartı not edilmiş: `5528 7900 0000 0008`
- [ ] Mail kutusu temiz inbox + ayrı sekmede açık
- [ ] Admin panel sekmesinde önceden login
- [ ] Adres ekle modal'ı önceden test edilmiş (validation çalışıyor)

### Çekim
- [ ] Voiceover ayrı kayıt (ekran sessiz, üstüne ses)
- [ ] Demo bölümlerini 1.25x speed ile kes
- [ ] Mikrofon test 3 sn → dinle
- [ ] Background music ~%15 volume (lo-fi, sessiz çekime karşı)
- [ ] Subtitle ekle (CapCut otomatik) — ses kapalı izleyenler için

### Editing (CapCut)
- [ ] Bölüm geçişlerinde 0.5–1 sn fade-to-black
- [ ] Diyagram fade-in animasyonları (mimari + teknik bölümlerde)
- [ ] Highlight overlay'ler (kod ekranlarında sarı dikdörtgenler)
- [ ] Tech logo grid kapanışta (final montaj)

---

## Bilinen Eksikler / Opsiyonel Eklemeler

Bu script şu an 7:32 sürüyor, 2:28 buffer var. Eğer eklemek istersen:

1. **Saga compensation görsel demo** (~15 sn) — checkout sırasında ödemeyi bilerek başarısız yap, stok geri eklenmesini canlı göster
2. **Atomic UPDATE SQL flash** (~12 sn) — `UPDATE product SET stock = stock - :qty WHERE id = :id AND stock >= :qty` — tek SQL ekrana 2 sn
3. **Token bucket rate limiter** (~10 sn) — login endpoint'lerini brute force'a karşı koruyan custom filter
4. **Aggregated Swagger** (~8 sn) — gateway'de tek dropdown'dan tüm 7 servisin endpoint'i

Hepsi eklenirse: ~9:27 — hâlâ 10:00 bütçesinde.
