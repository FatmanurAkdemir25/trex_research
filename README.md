## 1. Modern Yazılım Geliştirme Pratikleri

Git koddaki değişiklikleri kontrol eden bir versiyon kontrol sistemidir. GitHub ise git ile
oluşturulan projelerin internet ortamında saklanabileceği ve paylaşılabileceği bir
platformdur. Temel git komutları :

-   init : Klasörü git projesi haline getirir.
-   clone : GitHub'daki bir projeyi bilgisayarına kopyalar.
-   add : Değişiklikleri commit için hazırlar. (git add <dosya_adi> tek bir dosyayı
stage'ler, git add . hepsini birden stage'ler.)
-   commit : Yapılan değişiklikleri Git geçmişine kaydeder. Projenin o andaki
durumunun bir snapshot'ını (anlık görüntüsünü) oluşturur. Önceki commitlerle
karşılaştırılarak hangi dosyalarda ve ne gibi değişiklikler yapıldığı görülebilir.
-   push : Bilgisayardaki commitleri GitHub'a gönderir
-   pull : GitHub'daki güncel değişiklikleri bilgisayara çeker.
-   branch : Belirli bir commit'i işaret eden hareketli işaretçilerdir (pointer).
-   merge : İki branch'teki değişiklikleri birleştirmek için kullanılır.

### Merge conflict nedir, nasıl çözülür?

Merge conflict, Git iki branch'i birleştirirken aynı dosyanın aynı kısmında birbiriyle
çelişen değişiklikler bulduğunda oluşur. Nasıl çözüleceğine bakacak olursak :

Örneğin:

git switch main

git merge feature

Conflict oluşursa Git sana hangi dosyada sorun olduğunu söyler. Dosyanın içerisinde
şuna benzer bir yapı görünür :

<<<<<<< HEAD          (Şu an bulunduğun branch'in değişikliği)

Merhaba Dünya

=======        (İki değişikliği ayırmak için kullanılır.)

Merhaba GitHub

>>>>>>> feature       (Birleştirdiğin branch'in değişikliği)

Öncelikle hangi değişikliğin kalacağına karar ver. Örneğin Merhaba GitHub olarak
bırakmak istiyorsan conflict işaretlerini (<<<<<<<, =======, >>>>>>>) sil. Daha sonra

dosyayı kaydet, git add . ile çözülen dosyayı staging area'ya ekle ve git commit ile merge
işlemini tamamla.

### CI/CD Nedir ?

CI/CD (Continuous Integration / Continuous Delivery veya Continuous Deployment),
yazılım geliştirme süreçlerinde kod değişikliklerinin otomatik olarak derlenmesi, test
edilmesi ve gerektiğinde uygulamanın hedef ortama dağıtılması için kullanılan
uygulama ve otomasyon yaklaşımıdır.

CI/CD'nin temel amacı, yazılım geliştirme sürecini daha hızlı, düzenli, tekrarlanabilir ve
güvenilir hâle getirmektir.

1- Continuous Integration (CI)
Continuous Integration (Sürekli Entegrasyon), geliştiricilerin yaptıkları kod
değişikliklerinin sık sık ortak kod deposuna aktarılması ve bu değişikliklerin
otomatik olarak kontrol edilmesi sürecidir. Basit bir CI süreci :
Kod değişikliği
↓
Git Push
↓
Kaynak kodu al
↓
Restore
↓
Build
↓
Test
↓
Başarılı / Başarısız
2- Continuous Delivery (CD)
Continuous Delivery (Sürekli Teslimat), CI aşamasından başarıyla geçen
uygulamanın deployment için hazır hâle getirilmesidir. Örneğin:
Kod
↓
Build
↓
Test
↓
Package / Artifact
↓
Deployment için hazır

Burada uygulama otomatik olarak production ortamına gönderilmek zorunda
değildir. Son deployment işlemi manuel bir onaydan sonra gerçekleştirilebilir.
3- Continuous Deployment (CD)
Continuous Deployment (Sürekli Dağıtım), testlerden ve gerekli kontrollerden
başarıyla geçen kodun otomatik olarak production ortamına dağıtılmasıdır.
Örneğin:
Git Push
↓
Build
↓
Test
↓
Başarılı
↓
Otomatik Deploy
↓
Production

Bu nedenle Continuous Delivery ve Continuous Deployment aynı şey değildir.

•    Continuous Delivery: Deploy edilmeye hazır hâle getirir; son adım manuel
olabilir.
•    Continuous Deployment: Başarılı kontrollerden sonra otomatik olarak deploy
eder.

### Pipeline Nedir?

Pipeline, CI/CD sürecindeki işlemlerin belirli bir sırayla otomatik olarak çalıştırıldığı iş
akışıdır. Örneğin bir .NET projesinin pipeline'ı şöyle olabilir:

GitHub

↓

Kod değişikliği

↓

Restore

↓

Build

↓

Test

↓

Deploy

↓

Sunucu

### GitHub Actions ile CI/CD Pipeline Örnekleri

GitHub Actions, GitHub repository'lerinde otomatik iş akışları oluşturmak için kullanılan
bir otomasyon ve CI/CD platformudur. Süreç:

GitHub'a Push

↓

GitHub Actions çalışır

↓

Repository alınır

↓

.NET kurulumu

↓

dotnet restore

↓

dotnet build

↓

dotnet test

Testlerden biri başarısız olursa pipeline başarısız olarak sonuçlanır.

### Azure DevOps ile CI/CD Pipeline Örnekleri

Azure DevOps, Microsoft tarafından sunulan yazılım geliştirme ve DevOps
platformudur. İçerisinde CI/CD süreçleri için Azure Pipelines kullanılabilir. Pipeline:

main branch'e Push

↓

Azure Pipeline

↓

.NET SDK kurulumu

↓

Restore

↓

Build

↓

Test

### CI/CD’nin .NET Projesinde Uygulanması

Geliştirici yeni bir özellik geliştirdiğinde GitHub Actions veya Azure Pipelines otomatik
olarak çalışır.

-   CI Aşaması
Öncelikle projenin bağımlılıkları yüklenir:
dotnet restore
Daha sonra proje derlenir:
dotnet build
Ardından testler çalıştırılır:
dotnet test
Her şey başarılıysa uygulama deployment aşamasına geçebilir.
-   CD Aşaması
Uygulama build edildikten ve testlerden geçtikten sonra bir deployment işlemi
gerçekleştirilebilir.

Böylece genel süreç:

Developer

↓

GitHub

↓

GitHub Actions / Azure Pipelines

↓

Restore

↓

Build

↓

Test

↓

Deploy

↓

Uygulama

şeklinde olur.

### CI/CD Kullanmanın Avantajları

•   Kod değişiklikleri daha hızlı test edilebilir.
•   Hatalar daha erken tespit edilebilir.
•   Build ve test işlemleri otomatikleştirilebilir.
•   Manuel deployment işlemleri azaltılabilir.
•   Aynı işlemler her seferinde standart şekilde uygulanabilir.
•   Yazılımın yeni sürümlerinin yayınlanması kolaylaşır.
•   Ekip içerisindeki geliştirme süreci daha düzenli hâle gelir.

### Software Development Life Cycle (SDLC)

SDLC, bir yazılımın fikir aşamasından başlayarak geliştirilmesi, test edilmesi, kullanıma
sunulması ve bakımının yapılmasına kadar geçen süreci tanımlayan yaklaşımdır.

Genel olarak planlama, analiz, geliştirme, test, dağıtım, bakım aşamalarından oluşur.
Aşamaların isimleri ve sıralamaları kullanılan SDLC modeline göre değişebilir.

1- Planlama (Planning)
Bu aşamada projenin ne yapacağı, neden yapacağı ve nasıl yapacağı belirlenir.
Projenin amacı, kullanıcı ve iş ihtiyaçları, gerekli kaynaklar, olası riskler vb.
durumlar bu aşamada değerlendirilir.
Yazılımcı ise bu aşamada doğrudan kod yazmak yerine teknik açıdan projeye
katkıda bulunabilir. Örneğin kullanılabilecek teknolojileri değerlendirebilir, teknik
gereksinim ve riskleri belirlemeye yardımcı olabilir.

2- Analiz (Analysis)
Bu aşamada kullanıcının ve sistemin ihtiyaçları ayrıntılı olarak incelenir. “Ne
yapılmalı?” sorusuna cevap aranır. Örneğin sistemi kimler kullanacak, hangi
veriler tutulacak, güvenlik gereksinimleri neler gibi ihtiyaçlar belirlenir.
Yazılımcı ise bu aşamada gereksinimlerin teknik olarak uygulanabilirliğini
değerlendirir.

3- Tasarım ve Geliştirme (Design & Development)
Bu aşamada analiz sonucunda belirlenen gereksinimler teknik tasarıma ve daha
sonra koda dönüştürülür.
Tasarım aşamasında sistem mimarisi belirlenir, veritabanı tasarlanır, API yapısı
belirlenir, kullanılacak teknolojiler belirlenir ve istenirse arayüz tasarımı
yapılabilir.
Geliştirme aşamasında ise yazılımcılar belirlenen tasarıma göre kodu geliştirir.
Bu aşama yazılımcının en aktif olduğu aşamadır. Kod yazar, veritabanı işlemlerini
geliştirir API’leri oluşturur, kullanıcı arayüzünü geliştirir vb.

4- Test (Testing)
Geliştirilen yazılımın gereksinimleri karşılayıp karşılamadığı ve hatalarının olup
olmadığı kontrol edilir.
Test türlerinden bazıları:
o Unit Test: Küçük kod parçalarının test edilmesi.
o Integration Test: Birden fazla bileşenin birlikte çalışmasının test edilmesi.
o System Test: Sistemin bir bütün olarak test edilmesi.
o Acceptance Test: Yazılımın kullanıcı gereksinimlerini karşılayıp
karşılamadığının kontrol edilmesi.
Yazılımcı bu aşamada unit testler yazabilir, hataları düzeltir, kodun test edilebilir
olmasını sağlar.

5- Dağıtım (Deployment)
Testleri tamamlanan yazılımın kullanıcıların erişebileceği ortama yüklenmesi ve
kullanıma sunulmasıdır. Yazılım production ortamına deploy edildiğinde gerçek
kullanıcılar tarafından kullanılabilir. Deployment işlemleri manuel yapılabileceği
gibi CI/CD pipeline'ları ile otomatikleştirilebilir.
Yazılımcı bu aşamada deployment sürecine destek olur, ortam değişkenlerini ve
yapılandırmaları yönetir, deployment sonrası oluşabilecek hataları takip eder.

6- Bakım (Maintenance)
Yazılım kullanıma sunulduktan sonra süreç bitmez. Kullanıcıların karşılaştığı
hatalar giderilir ve yeni ihtiyaçlara göre yazılım geliştirilir. Bakım kapsamında
hata düzeltmeleri, güvenlik güncellemeleri, performans iyileştirmeleri, yeni
özellikler, teknoloji güncellemeleri yapılabilir.

### Agile Nedir?

Agile (Çevik), yazılım geliştirme süreçlerinde değişen gereksinimlere hızlı şekilde uyum
sağlamayı ve yazılımı küçük parçalar hâlinde, sürekli geri bildirim alarak geliştirmeyi
amaçlayan yaklaşımlar bütünüdür.

### Scrum Nedir?

Scrum, Agile prensiplerini uygulamak için kullanılan bir **framework (çerçeve)**dir.
Scrum'da proje genellikle Sprint adı verilen kısa geliştirme dönemlerine bölünür.
Örneğin:

Sprint 1 → Login

Sprint 2 → Kullanıcı yönetimi

Sprint 3 → Raporlama

Sprint 4 → Bildirim sistemi

Scrum’daki temel roller:

-   Product Owner: Ürünün ihtiyaçlarını ve önceliklerini belirlemeye yardımcı olur.
-   Scrum Master: Scrum sürecinin doğru uygulanmasına yardımcı olur ve takımın
karşılaştığı engellerin kaldırılmasını destekler.
-   Developers: Ürünü geliştiren ekip üyeleridir.

Scrum’daki temel kavramlar:

-   Product Backlog: Projede yapılması gereken işlerin/özelliklerin listesidir.
-   Sprint: Takımın belirli bir süre içerisinde gerçekleştirmeyi hedeflediği çalışma
dönemidir. (Örneğin 2 haftalık sprint)
-   Daily Scrum: Takımın kısa günlük toplantısıdır. Dün ne yaptım, bugün ne
yapacağım, önümde bir engel var mı gibi konular konuşulur.
-   Sprint Review: Sprint sonunda ortaya çıkan ürün/incelenebilir çıktı değerlendirilir
ve paydaşlardan geri bildirim alınır.
-   Sprint Retrospective: Takımın kendi çalışma sürecini değerlendirdiği toplantıdır.
Örneğin neyi iyi yaptık, neyi daha iyi yapabiliriz gibi konular konuşulur.

### Kanban Nedir?

Kanban, işlerin görsel olarak takip edilmesini sağlayan bir Agile yaklaşımıdır. Genellikle
bir Kanban Board kullanılır. Bir iş başladığında TO DO → IN PROGRESS,
tamamlandığında IN PROGRESS → DONE şeklinde iletilir.

Kanban'da Scrum'daki gibi zorunlu Sprint dönemleri bulunmaz. İşler tamamlandıkça
yeni işler sürece alınabilir.

### Agile-Scrum-Kanban İlişkisi

| Özellik | Agile | Scrum | Kanban |
|---|---|---|---|
| Tür | Yaklaşım/prensipler | Framework | İş akışı yöntemi |
| Sprint | Zorunlu değil | Var | Zorunlu değil |
| Roller | Belirli zorunlu roller yok | Product Owner, Scrum Master, Developers | Belirli zorunlu roller yok |
| İş takibi | Genel | Backlog + Sprint | Kanban Board |
| Temel amaç | Değişime uyum | Sprintlerle iteratif geliştirme | İş akışını görselleştirmek |

## 2. .NET Ekosistemi

### .NET Nedir?

.NET, Microsoft tarafından geliştirilen, farklı türlerde uygulamalar oluşturmak için
kullanılan açık kaynaklı, ücretsiz ve platformlar arası bir yazılım geliştirme
platformudur. .NET ile web uygulamaları, masaüstü uygulamaları, mobil uygulamalar,
oyun ve IoT uygulamaları vb. Geliştirilebilir.

.NET tek başına bir programlama dili değildir. C#, F# ve Visual Basic gibi diller .NET
platformu üzerinde kullanılabilir.

### .NET Tarihçesi

.NET'in gelişimini anlamak için birkaç önemli dönüm noktası vardır.

-   .NET Framework
Microsoft, 2002 yılında .NET Framework 1.0 sürümünü yayımladı. Başlangıçta
özellikle Windows uygulamaları ve web uygulamaları geliştirmek amacıyla
kullanıldı.

-      .NET Core

Daha sonra Microsoft, modern ve platformlar arası bir yapı olan .NET Core'u
geliştirdi. .NET Core Windows, Linux, macOS üzerinde çalışabilecek sekilde
tasarlandı.

-       .NET 5

2020 yılında Microsoft, .NET Core ismindeki "Core" ifadesini kaldırarak      platformu
.NET 5 adı altında birleştirmeye başladı. Sonrasında .NET 6, 7, 8 ... şeklinde devam
etti. Bu nedenle günümüzde “.NET” dediğimizde genellikle        modern, birleşik .NET
platformu kastedilir.

### .NET Framework, .NET Core ve Modern .NET Farkları

| Özellik | .NET Framework | .NET Core | Modern .NET (.NET 5+) |
|---|---|---|---|
| İlk dönem | 2002 | 2016 | 2020 |
| Platform | Ağırlıklı Windows | Windows, Linux, macOS | Windows, Linux, macOS |
| Açık kaynak | Kısmen/sonradan | Evet | Evet |
| Performans | Eski mimari | Yüksek | Yüksek |
| Web geliştirme | ASP.NET | ASP.NET Core | ASP.NET Core |
| Modern geliştirme | Eski projelerde yaygın | Geçiş teknolojisi | Güncel tercih |
| Güncel geliştirme | Yeni projeler için önerilmez | Artık eski adlandırma | Aktif geliştirme |

### .NET Platformlar Arası Çalışabilir mi?

Evet. Modern .NET Windows, Linux, macOS üzerinde çalışabilir. Bu özelliğin önemli
avantajlarından biri, geliştiricinin uygulamayı yalnızca Windows’a bağımlı olmadan
geliştirebilmesidir. Örneğin uygulama Windows üzerinde geliştirilip Linux tabanlı bir
sunucuda çalıştırılabilir.

### dotnet –info Nedir?

Bilgisayarda kurulu .NET SDK ve Runtime bilgilerini görmek için dotnet –info komutu
kullanılır. Örnek çıktı:

.NET SDK:      // SDK, uygulama geliştirmek için gereken araçları içerir.

Version:       9.0.302       // Bilgisayarda .NET 9 SDK yüklü olduğunu gösterir.

Commit:         bb2550b9af

Workload version: 9.0.300-manifests.183aaee6

MSBuild version: 17.14.13+65391c53b

Çalışma Zamanı Ortamı:       // Runtime, geliştirilmiş bir .NET uygulamasını
çalıştırmak için gereken ortamdır.

OS Name:       Windows

OS Version: 10.0.26200

OS Platform: Windows         // İşletim sisteminin Windows olduğunu gösterir.

RID:       win-x64

Base Path: C:\Program Files\dotnet\sdk\9.0.302\

.NET iş yükleri yüklendi:

Görüntülenecek yüklü iş yükü yok.

Host:

Version:   9.0.7

Architecture: x64     // .NET’in 64-bit mimaride çalıştığını gösterir.

Commit:     3c298d9f00

.NET SDKs installed:

9.0.302 [C:\Program Files\dotnet\sdk]

.NET runtimes installed:

Microsoft.AspNetCore.App 8.0.18 [C:\Program
Files\dotnet\shared\Microsoft.AspNetCore.App]

Microsoft.AspNetCore.App 9.0.7 [C:\Program
Files\dotnet\shared\Microsoft.AspNetCore.App]

Microsoft.NETCore.App 8.0.18 [C:\Program           // .NET 8 runtime’ın yüklü
Files\dotnet\shared\Microsoft.NETCore.App]

Microsoft.NETCore.App 9.0.7 [C:\Program
Files\dotnet\shared\Microsoft.NETCore.App]

Microsoft.WindowsDesktop.App 8.0.18 [C:\Program
Files\dotnet\shared\Microsoft.WindowsDesktop.App]

Microsoft.WindowsDesktop.App 9.0.7 [C:\Program
Files\dotnet\shared\Microsoft.WindowsDesktop.App]

### Senkron Programlama

Senkron programlamada işlemler sırayla gerçekleştirilir. Bir işlem tamamlanmadan
sonraki işlem devam etmeyebilir. Örneğin:

Console.WriteLine("1");

Thread.Sleep(3000);

Console.WriteLine("2");

Console.WriteLine("3");

Çıktı:

1

(3 saniye bekleme)

2

3

### Asenkron Programlama

Asenkron programlama, özellikle bekleme gerektiren işlemlerde programın gereksiz
yere bloklanmasını önlemeye yardımcı olur. Örneğin bir web API'den veri çekiyorsun:

API isteği

↓

Sunucunun cevap vermesini bekle

Bu sırada uygulamanın ilgili thread'i gereksiz şekilde bloklanmak yerine başka işler
yapabilir.

C#'ta bunun için özellikle:

•    async
•    await
•    Task

Kullanılır.

-    async Nedir?

Bir metodun asenkron işlemler gerçekleştirebildiğini belirtmek için kullanılır.

async Task GetDataAsync()

{

// asenkron işlemler

}

Ancak async tek başına işlemi asenkron hâle getirmez. Genellikle await ile
birlikte kullanılır.

-    await nedir?

Bir Task'ın tamamlanmasını beklemek için kullanılır. Örneğin:

async Task GetDataAsync()

{

await Task.Delay(3000);

Console.WriteLine("Veri geldi");

}

Burada await, ilgili asenkron işlemin sonucunu beklerken metodun uygun
şekilde devam etmesini sağlar.

-   Task Nedir?

Task, devam eden veya ileride tamamlanacak asenkron bir işlemi temsil eder.

async Task<string> GetDataAsync() //işlem bittiğinde string türünde bir
sonuç                             döndürüleceğini belirtir.

{

await Task.Delay(1000);

return "Veri";

}

-   ConfigureAwait Nedir?
ConfigureAwait, bir await işleminden sonra devam eden kodun belirli bir
synchronization context'e geri dönüp dönmeyeceğini kontrol etmek için
kullanılır. Örneğin:
```csharp
await GetDataAsync().ConfigureAwait(false);
```
false, devam eden kodun mevcut context'e geri dönmesinin zorunlu olmadığını
belirtir. Özellikle library (kütüphane) kodlarında gereksiz context dönüşlerinden
kaçınmak amacıyla kullanılabilir.

### => Arrow Function C#’ta Ne İşe Yarar?

C#'ta => işareti lambda expression ve expression-bodied member gibi farklı yapılarda
kullanılır.

-   Lambda expression
```csharp
var numbers = new[] { 1, 2, 3, 4 };
var result = numbers.Select(x => x * 2);
```

Burada x => x * 2 bir lambda expression’dır.
-   Expression-bodied method
=> metotlarda da kullanılabilir:
```csharp
int Square(int x) => x * x;
```
bu
int Square(int x)
{
return x * x;
}
ile aynı mantıktadır.

## 3-Backend Geliştirme Temelleri

### Backend Nedir?

Backend, bir uygulamanın kullanıcı tarafından doğrudan görülmeyen; verilerin
işlenmesi, iş kurallarının uygulanması, veritabanı işlemleri ve API'lerin çalıştırılması gibi
işlemleri gerçekleştiren kısmıdır. Backend tarafında kullanılabilecek teknolojilere örnek
olarak C# / ASP.NET Core, Java / Sprıng Boot, Python / Django, Flask, FastAPI,
JavaScript / Node.js, PHP / Laravel verilebilir.

Frontend ise kullanıcının etkileşimde bulunduğu arayüzdür.

### Frontend ve Backend Farkı

| Özellik | Frontend | Backend |
|---|---|---|
| Kullanıcıyla doğrudan etkileşim | Evet | Genellikle hayır |
| Görevi | Arayüzü oluşturmak | İş mantığını ve verileri yönetmek |
| Çalıştığı yer | Kullanıcının cihazı/tarayıcısı | Sunucu |
| Örnek teknolojiler | HTML, CSS, JavaScript, React | C#, ASP.NET Core, Java, Python |
| Veritabanı erişimi | Genellikle doğrudan değil | Evet |
| API | API'leri tüketir | API'leri oluşturabilir |

### Web Sunucusu Nedir?

Web sunucusu (web server), HTTP/HTTPS üzerinden gelen istekleri karşılayan ve
istemcilere yanıt gönderen yazılım veya sunucu sistemidir. Örneğin kullanıcı tarayıcıya:

https://example.com

yazdığında tarayıcı sunucuya bir HTTP isteği gönderir. Sunucu da:

HTTP Request

↓

Web Server

↓

HTTP Response

↓

Browser

şeklinde cevap verir.

Yaygın web sunucuları Nginx, Apache, Microsoft IIS’tir. ASP.NET Core uygulamalarında
ise uygulamanın kendi web sunucusu olarak Kestrel kullanılabilir.

### API Nedir?

API (Application Programming Interface), farklı yazılım bileşenlerinin birbiriyle iletişim
kurmasını sağlayan arayüzdür. Web uygulamalarında API, frontend ile backend
arasındaki iletişimde sıklıkla kullanılır. Örneğin:

Frontend

↓

GET /api/products

↓

Backend

↓

Database

↓

Ürünler

↓

JSON Response

↓

Frontend

Frontend'in veritabanına doğrudan erişmesi yerine backend üzerinden API ile iletişim
kurması yaygın bir mimaridir.

### API Türleri

Web geliştirmede sık karşılaşılan API yaklaşımları:

•   REST API
•   SOAP API
•   GraphQL API

### HTTP Nedir?

HTTP (Hypertext Transfer Protocol), istemci ve sunucu arasındaki iletişimde kullanılan
bir uygulama katmanı protokolüdür. Örneğin:

Client

↓

HTTP Request

↓

Server

↓

HTTP Response

↓

Client

Web API'lerde istemci, HTTP üzerinden sunucuya istek gönderir.

HTTPS ise HTTP'nin TLS ile güvenli hâle getirilmiş biçimidir.

### HTTP Metodları

HTTP metodları, istemcinin sunucudan hangi işlemi gerçekleştirmesini istediğini
belirtmek için kullanılır.

En sık kullanılanlar: GET, POST, PUT, DELETE

-   GET: Veri almak için kullanılır. Örneğin:
GET /api/products/15        // 15 numaralı ürünü getir.

Örnek response:
```json
{
"id": 15,
"name": "Laptop",
"price": 25000
}
```
-   POST: Yeni bir kaynak oluşturmak için kullanılır. Örneğin:
POST /api/products
Gönderilen JSON:
{
"name": "Laptop",
"price": 25000
}

Backend bu bilgileri kullanarak yeni bir ürün oluşturabilir.

-   PUT: Var olan bir kaynağı güncellemek veya tamamen değiştirmek amacıyla
kullanılır. Örneğin:
PUT /api/products/15
Gönderilen veri:
{
"name": "Gaming Laptop",
"price": 30000
}

Burada 15 numaralı ürün güncellenebilir.

Not: Kısmi güncellemeler için REST API'lerde genellikle PATCH de kullanılır.

-   DELETE: Bir kaynağı silmek için kullanılır. Örneğin:
DELETE /api/products/15
Anlamı: 15 numaralı ürünü sil.

### HTTP Metodlarını Özetlersek

| Metot | Amaç | Örnek |
|---|---|---|
| GET | Veri almak | GET /products/15 |
| POST | Yeni veri oluşturmak | POST /products |
| PUT | Veriyi güncellemek | PUT /products/15 |
| DELETE | Veri silmek | DELETE /products/15 |

### RESTful Servisler

REST (Representational State Transfer), web servislerinin tasarlanmasında kullanılan
bir mimari yaklaşımdır.

REST'te sistemdeki veriler genellikle resource (kaynak) olarak düşünülür.

Örneğin bir sistemde:

/products

/users

/orders

gibi kaynaklar olabilir. Örneğin ürün API’si:

GET /api/products

GET /api/products/15

POST /api/products

PUT /api/products/15

DELETE /api/products/15

Şeklinde tasarlanabilir. Burada URL, genellikle yapılacak işlemin kendisinden çok
kaynağı temsil eder. Örneğin:

/getProducts            yerine       GET /products

/deleteProduct          yerine       DELETE /products/15

RESTful yaklaşımda HTTP metodunun anlamından yararlanılır.

### JSON Nedir?

JSON (JavaScript Object Notation), verilerin yapılandırılmış şekilde temsil edilmesini
sağlayan, insanlar tarafından okunabilir bir veri formatıdır. Web API'lerde veri
alışverişinde oldukça yaygın kullanılır.

JSON'da nesneler { }, diziler ise [ ] ile gösterilir. Örneğin:

{

"id": 15,

"name": "Laptop",

"price": 25000,

"categories": [

"Computer",

"Electronics"

]

}

JSON'ın avantajlarından biri, farklı programlama dillerinin JSON verisini kolayca okuyup
işleyebilmesidir.

### SOAP Nedir?

SOAP (Simple Object Access Protocol), web servisleri arasında iletişim için kullanılan,
XML tabanlı bir mesajlaşma protokolüdür. SOAP mesajları XML formatındadır.
Basitleştirilmiş bir SOAP mesajı:

<soap:Envelope>

<soap:Body>

<GetProduct>

<ProductId>15</ProductId>

</GetProduct>

</soap:Body>

</soap:Envelope>

SOAP’ın REST’ten en önemli farklarından biri: REST bir mimari yaklaşımdır; SOAP ise bir
protokoldür.

### GraphQL Nedir?

GraphQL, istemcinin ihtiyaç duyduğu verileri sorgulayabildiği bir API sorgulama dili ve
çalışma ortamıdır.

REST’te:

GET /api/users/15

Gibi endpoint’ler kullanılır.

GraphQL’de ise istemci hangi alanlara ihtiyacı olduğunu sorgulayabilir. Örneğin:

query {

user(id: 15) {

name

email

}

}

İstemci burada sadece name ve email alanlarını istediğini belirtir. Bu yaklaşım özellikle
istemcinin farklı veri ihtiyaçlarının bulunduğu uygulamalarda faydalı olabilir.

| Özellik | REST | SOAP | GraphQL |
|---|---|---|---|
| Tür | Mimari yaklaşım | Protokol | Sorgulama dili + çalışma ortamı |
| Veri formatı | Genellikle JSON | XML | Genellikle JSON response |
| Endpoint | Genellikle birden fazla resource endpoint'i | Servis/metot tabanlı olabilir | Genellikle tek endpoint |
| Veri seçimi | Endpoint'in döndürdüğü yapıya bağlı | Önceden tanımlı mesaj yapıları | İstemci istediği alanları sorgular |
| HTTP kullanımı | Çok yaygın | HTTP dahil çeşitli taşıma mekanizmaları kullanılabilir | Genellikle HTTP |
| Öğrenme | Görece kolay | Daha karmaşık | REST'ten farklı bir sorgulama modeli |
| Kullanım | Web API'lerde çok yaygın | Kurumsal entegrasyonlar vb. | Karmaşık/farklı veri ihtiyaçları olan API'ler |

## 4. ASP.NET

### ASP.NET Nedir?

Microsoft tarafından geliştirilen, web uygulamaları ve web servisleri geliştirmek için
kullanılan bir web geliştirme teknolojisidir. ASP.NET ile web siteleri, web uygulamaları,
REST API’ler, Web servisleri, MVC uygulamaları geliştirlebilir. ASP.NET üzerinde özellikle
C# kullanılır.

### ASP.NET Core Nedir?

ASP.NET'in modern, yeniden tasarlanmış ve platformlar arası çalışan web geliştirme
çatısıdır. Windows’a bağlı değildir. Windows, Linux, macOS üzerinde çalışabilir.
Günümüzde yeni projelerde genel olarak ASP.NET Core tercih edilir.

### ASP.NET ve ASP.NET Core Farkları

| Özellik | ASP.NET | ASP.NET Core |
|---|---|---|
| Platform | Büyük ölçüde Windows | Windows, Linux, macOS |
| Yapı | Eski ASP.NET teknolojileri | Modern ve yeniden tasarlanmış |
| Açık kaynak | Kısmen | Evet |
| Performans | İyi | Genellikle daha yüksek |
| Hosting | IIS ağırlıklı | Kestrel, IIS, Nginx vb. |
| Dependency Injection | Yerleşik değil / sonradan çözümler | Yerleşik |
| Middleware | Klasik ASP.NET'ten farklı | Temel yapı taşlarından biri |
| Modern kullanım | Legacy/var olan projeler | Yeni projeler |

ASP.NET Core, ASP.NET'in sadece yeni bir sürümü değildir; mimarisi önemli ölçüde
yeniden tasarlanmış modern bir web framework'üdür.

### MVC Nedir?

MVC = Model – View – Controller

Web uygulamasını farklı sorumluluklara ayırmak için kullanılan bir mimari yaklaşımdır.

-   Model: Uygulamanın verilerini ve veriyle ilgili kurallarını temsil eder.
-   View: Kullanıcıya gösterilen arayüzdür. ASP.NET MVC'de genellikle Razor View
kullanılır.
-   Controller: Kullanıcıdan gelen isteği karşılar, gerekli işlemleri yapar ve uygun
sonucu döndürür.

### MVC Akışı

Kullanıcı

↓

Controller

↓

Model / Service

↓

Database

↓

Controller

↓

View

↓

Kullanıcı

### Middleware Nedir?

Middleware, ASP.NET Core'da HTTP isteklerinin ve cevaplarının işlenme sürecine dahil
olan yazılım bileşenleridir. Bir isteğin uygulamaya gelmesinden cevabın kullanıcıya
dönmesine kadar araya girerek çeşitli işlemler gerçekleştirebilir. Örneğin middleware
ile: authentication, authorization, HTTPS yönlendirmesi, routing gibi işlemler yapılabilir.
Middleware'ler pipeline şeklinde çalışır.

### Program.cs İçindeki Middleware Sıralaması

Modern ASP.NET Core uygulamalarında middleware'ler genellikle Program.cs içerisinde
tanımlanır.Burada sıralama önemidir. Örneğin:

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

var app = builder.Build();

app.UseHttpsRedirection();           // HTTP isteklerini HTTPS'e yönlendirmek için
kullanılır.

app.UseAuthentication();             // Kullanıcının kim olduğunu belirlemeye
çalışır.

app.UseAuthorization();              // Kullanıcının istenen işlemi yapmaya yetkisi
olup olmadığını kontrol eder.

app.MapControllers();                 // Controller'ların endpoint'lerini uygulamaya
bağlar.

app.Run();

### Middleware Sırası Neden Önemlidir?

Middleware'ler sırayla çalıştığı için yanlış sıra beklenmeyen sonuçlara yol açabilir.
Örneğin önce kullanıcının kim olduğu sonra yetkisinin olup olmadığı belirlenir. Bu sıra
mantıklıdır.

NOT: Middleware'ler ASP.NET Core request pipeline içerisinde belirli bir sırayla çalışır.
Bir middleware kendisinden sonraki middleware'e isteği aktarabilir ve response
dönerken tekrar işlem yapabilir. Bu nedenle middleware'lerin sıralaması uygulamanın
doğru çalışması açısından önemlidir.

### Dependency Injection (DI) nedir?

Dependency Injection (DI), bir sınıfın ihtiyaç duyduğu nesneleri kendi içerisinde
oluşturmak yerine dışarıdan almasını sağlayan tasarım yaklaşımıdır. Örneğin kötü bir
yaklaşım:

public class ProductController

{

private ProductService service;

public ProductController()

{

service = new ProductService();

}

}

Burada Controller doğrudan new ProductService() oluşturuyor. Bu durumda sınıflar
birbirine sıkı şekilde bağlıdır.

DI ile önce bir interface oluşturulur:

public interface IProductService

{

List<Product> GetProducts();

}

Service:

public class ProductService : IProductService

{

public List<Product> GetProducts()

{

// İş mantığı

return new List<Product>();

}

}

Controller:

public class ProductController : Controller

{

private readonly IProductService _productService;

public ProductController(IProductService productService)

{

_productService = productService;

}

public IActionResult Index()

{

var products = _productService.GetProducts();

return View(products);

}

}

Burada Controller new ProductService() yapmıyor. ASP.NET Core, DI container
üzerinden gerekli IProductService nesnesini oluşturup Controller'a veriyor. Buna
Constructor Injection denir.

### DI'nin Faydaları

- Sınıflar arasındaki bağımlılığı azaltır.
- Test yazmayı kolaylaştırır.
- Kodun değiştirilmesini kolaylaştırır.
- Daha modüler bir yapı sağlar.
- Interface kullanımını destekler.
### DI Yaşam Süreleri

ASP.NET Core'da DI için sık kullanılan üç yaşam süresi vardır:

-   Transient
Her istendiğinde yeni nesne oluşturulur.
```csharp
builder.Services.AddTransient<IService, Service>();
```
-   Scopet
Her HTTP request için bir instance oluşturulur.
```csharp
builder.Services.AddScoped<IService, Service>();
```
-   Singleton
Uygulama boyunca aynı instance kullanılır.
```csharp
builder.Services.AddSingleton<IService, Service>();
```

### Katmanlı Mimari (Layered Architecture)

Katmanlı mimari, uygulamayı farklı sorumluluklara sahip katmanlara ayırır. Temel
olarak:

Presentation

UI / API / Controller

↓

Business

Service / İş Mantığı

↓

Data Access

Repository / Database

-   Presentetion Layer
Kullanıcıyla veya dış sistemlerle iletişim kurar. Görevi dışarıdan gelen isteği
almak ve sonucu dışarıya vermek. Örneğin controller, API endpoint, view.

-   Business Layer
Uygulamanın iş kuralları ve iş mantığı burada bulunur. Örneğin bir ürünün fiyatı
10.000 TL üzerindeyse %10 indirim uygula bir business rule'dur. Service sınıfları
da genellikle burada bulunur.
-   Data Access Layer
Veritabanı ile iletişimden sorumludur. Burada repository, SQL sorguları gibi
yapılar bulunabilir.

### Service ve Repository Pattern

Repository

Veriye erişim işlemlerini soyutlamak için kullanılır. Örneğin:

ProductRepository

↓

Database

Su işlemleri yapabilir: GetAll(), GetById(), Add()...

Service

Uygulamanın iş mantığını yönetir.

ProductController

↓

ProductService

↓

ProductRepository

↓

Database

Yani kısaca:

-   Controller → isteği alıyor
-   Service → iş kurallarını uyguluyor
-   Repository → veritabanına erişiyor

### Clean Architecture
Clean Architecture, uygulamanın iş kurallarını dış teknolojilerden mümkün olduğunca
bağımsız tutmayı amaçlayan mimari yaklaşımdır. Genellikle şu katmanlarla gösterilir:
- API -> Controllers / HTTP

- Infrastructure -> DB / EF Core / External APIs
- Application -> Services / Use Cases / DTOs
- Domain -> Entities / Business Rules
Clean Architecture'da bağımlılıklar içeriye doğru akar. Yani dış katmanlar iç katmanlara
bağımlı olabilir; iç katmanlar dış katmanlara bağımlı olmamalıdır. Örneğin:
API ─> Application

Infrastructure ─> Application

Application ─> Domain

Domain ─> hiçbir dış katmana

### Clean Architecture Katmanları

-   Domain
En merkezi katmandır. Burada Entity'ler, Business rules, Domain modelleri
bulunur.
-   Application
Uygulamanın kullanım senaryolarını ve iş akışlarını içerir. Burada use case'ler
bulunabilir ya da interface'ler tanımlanabilir.
-   Infrastructure
Teknolojik detaylar burada bulunur.
-   API
Dış dünyaya açılan katmandır. Örneğin HTTP, JSON, Authentication burada
bulunur.

### Layered Architecture ile Clean Architecture Farkı

Layered Architecture ile Clean Architecture arasındaki temel fark, bağımlılıkların nasıl
düzenlendiğidir. Layered Architecture uygulamayı genellikle Presentation, Business ve
Data Access gibi katmanlara ayırır ve katmanlar çoğunlukla üstten alta doğru birbirine
bağımlıdır. Clean Architecture ise Domain ve Application katmanlarını merkeze alır;
veritabanı, framework ve diğer teknik detayların merkeze bağımlı olmasını hedefler.
Böylece iş mantığı dış teknolojilerden daha bağımsız hale gelir.

## 5.Veritabanı ve ORM

### SQL Nedir?

SQL (Structured Query Language), ilişkisel veritabanlarında veri oluşturmak,
sorgulamak, güncellemek ve silmek için kullanılan bir dildir. En temel SQL işlemleri:

-   SELECT → Veri getirme
-   INSERT → Veri ekleme

- UPDATE → Veri güncelleme
- DELETE → Veri silme
### İlişkisel ve İlişkisel Olmayan Veritabanları

-   İlişkisel Veritabanı (Relational Database)
Veriler tablolar halinde tutulur ve tablolar arasında ilişkiler kurulabilir. Örnek
olarak SQL Server, MySQL, PostgreSGL, Oracle verilebilir.
-   İlişkisel olmayan (NoSQL) Veritabanı
Veriler klasik tablo-satır-sütun yapısına bağlı olmak zorunda değildir. Doküman,
key-value, graph gibi farklı veri modelleri kullanılabilir. Örneğin MangoDB
document, Redis key-value, Neo4j graph veri modelini kullanır.

### İlişkisel Veritabanı ve İlişkisel Olmayan Veritabanı Arasındaki Temel Farklar

| Özellik | İlişkisel | NoSQL |
|---|---|---|
| Veri yapısı | Tablo | Döküman, key-value vb. |
| Şema | Genellikle belirli | Daha esnek |
| İlişkiler | Güçlü ilişki desteği | Modele göre değişir |
| Örnek | SQL Server | MongoDB |

### ORM Nedir?

ORM (Object-Relational Mapping), programlama dilindeki nesneler ile veritabanındaki
tablolar arasında bağlantı kurulmasını sağlayan yaklaşımdır.

Normalde SQL ile SELECT * FROM Products; yazarken ORM kullanıldığında C# nesneleri
üzerinden çalışabiliriz. Örneğin var products = context.Products.ToList(); ORM, C#
tarafındaki Product nesnesini veritabanındaki Products tablosuyla ilişkilendirebilir.

ORM'nin Avantajları

•   SQL kodunu azaltabilir.
•   C# nesneleri üzerinden veritabanıyla çalışmayı kolaylaştırır.
•   CRUD işlemlerini kolaylaştırır.
•   Kodun daha düzenli olmasını sağlayabilir.

### Entity Framework Core nedir?

Entity Framework Core (EF Core), Microsoft tarafından geliştirilen, .NET
uygulamalarında kullanılan bir ORM framework'üdür. C# sınıfları ile veritabanı arasında
bağlantı kurmayı sağlar.

### DbContext nedir?

DbContext, EF Core'un veritabanıyla iletişim kurmasını sağlayan temel sınıftır.
Veritabanındaki tablolarla C# tarafındaki entity'ler arasında bağlantı kurar. Basit akış:

C# Kodları

↓

DbContext

↓

EF Core

↓

Database

### LINQ nedir?

LINQ (Language Integrated Query), C# içerisinde koleksiyonlar ve veriler üzerinde
sorgulama yapmayı sağlayan bir sorgulama özelliğidir. Örneğin:

var products = context.Products

.Where(p => p.Price > 1000)

.ToList();

Burada fiyatı 1000'den büyük ürünler seçilir.

EF Core kullanıldığında bu LINQ sorgusu uygun SQL sorgusuna çevrilebilir.

### En çok kullanılan LINQ ifadeleri

-   Where()
Filtreleme yapar.
```csharp
var products = context.Products
.Where(p => p.Price > 1000)
.ToList();
```

Yaklaşık SQL karşılığı:

SELECT *

FROM Products

WHERE Price > 1000;

-   Select()
Belirli alanları seçer.
```csharp
var names = context.Products
.Select(p => p.Name)
.ToList();
```

Yaklaşık SQL karşılığı:

SELECT Name

FROM Products;

-   OrderBy()
Artan sıralama yapar.
```csharp
var products = context.Products
.OrderBy(p => p.Price)
.ToList();
```

Yaklaşık SQL karşılığı:

SELECT *

FROM Products

ORDER BY Price ASC;

Azalan sıralama:

```csharp
.OrderByDescending(p => p.Price)
```

-   FirstOrDefault()
İlk sonucu getirir. Sonuç yoksa null dönebilir.
```csharp
var product = context.Products
.FirstOrDefault(p => p.Id == 5);
```

Yaklaşık SQL karşılığı:

SELECT TOP 1 *

FROM Products

WHERE Id = 5;

-   Any()
Belirli bir şartı sağlayan kayıt olup olmadığını kontrol eder.
```csharp
bool exists = context.Products
.Any(p => p.Price > 10000);
```

Yaklaşık SQL karşılığı:

SELECT CASE

WHEN EXISTS (

SELECT 1

FROM Products

WHERE Price > 10000

)

THEN 1 ELSE 0

END;

-   Count()
Kayıt sayısını bulur.
```csharp
int count = context.Products.Count();
```

Yaklaşık SQL karşılığı:

```sql
SELECT COUNT(*)
FROM Products;
```

### Code-First nedir?

Code-First yaklaşımında öncelikle C# sınıfları oluşturulur, daha sonra bu sınıflardan
veritabanı yapısı oluşturulur.

EF Core migration kullanılarak veritabanı oluşturulabilir.

C# Entity

↓

EF Core Migration

↓

Database

### Database-First nedir?

Database-First yaklaşımında veritabanı önceden oluşturulmuştur.

EF Core kullanılarak mevcut veritabanından C# entity sınıfları ve DbContext
oluşturulabilir.

Database

↓

EF Core Scaffold

↓

C# Entity + DbContext

| Özellik | Code-First | Database-First |
|---|---|---|
| Başlangıç noktası | C# kodu | Veritabanı |
| Veritabanını kim yönlendirir? | Kod/migration | Mevcut DB yapısı |
| Yeni projeler | Sık tercih edilir | Kullanılabilir |
| Mevcut DB | Daha az uygun olabilir | Özellikle uygun |
| EF Core | Migration | Scaffold |

### Temel SQL Sorguları

-   SELECT – Veri Çekme
```sql
SELECT *
FROM Products;
```

Belirli alanları almak:

SELECT Name, Price

FROM Products;

Filtreleme:

SELECT *

FROM Products

WHERE Price > 1000;

-   INSERT – Veri Ekleme
```sql
INSERT INTO Products (Name, Price)
VALUES ('Laptop', 25000);
```
-   UPDATE – Veri Güncelleme
```sql
UPDATE Products
SET Price = 27000
WHERE Id = 1;
```
-   DELETE – Veri Silme
```sql
DELETE FROM Products
WHERE Id = 1;
```

## 6. GÜVENLİK VE PERFORMANS
### 6.1 Authentication ve Authorization
Authentication (Kimlik Doğrulama), sisteme giriş yapan kullanıcının gerçekten kim
olduğunu doğrulama işlemidir. Kullanıcı adı-şifre, JWT veya başka kimlik doğrulama
yöntemleri kullanılabilir.

Authorization (Yetkilendirme) ise kimliği doğrulanmış kullanıcının hangi kaynaklara
veya işlemlere erişebileceğini belirler.

Örneğin bir kullanıcı sisteme giriş yaptığında Authentication ile kullanıcının kimliği
doğrulanır. Kullanıcının sadece yöneticilere açık olan /admin sayfasına erişip
erişemeyeceği ise Authorization ile belirlenir.

Kısaca:

•   Authentication → “Sen kimsin?”
•   Authorization → “Neleri yapmaya yetkin var?”

### 6.2 JWT (JSON Web Token) Nedir?
JWT (JSON Web Token), sistemler arasında kullanıcı veya başka bilgiler hakkında
güvenilir şekilde veri taşımak için kullanılan, özellikle web uygulamalarında kimlik
doğrulamada yaygın olarak kullanılan bir token formatıdır.

Kullanıcı giriş yaptığında sunucu başarılı kimlik doğrulamanın ardından bir JWT
oluşturabilir. İstemci bu token'ı sonraki isteklerde sunucuya gönderir.

Genellikle:

Authorization: Bearer <token>

şeklinde gönderilir.

JWT üç temel bölümden oluşur:

Header.Payload.Signature

1. Header

Token'ın türü ve kullanılan algoritma hakkında bilgi içerir.

Örneğin:

```json
{
"alg": "HS256",
"typ": "JWT"
}
```

2. Payload

Kullanıcı veya token hakkında bilgiler, yani claim'ler bulunur.

Örneğin:

```json
{
"sub": "123",
"name": "Fatmanur",
"role": "Admin"
}
```

3. Signature

Token'ın değiştirilmediğini doğrulamak için kullanılan imzadır.

Basit gösterim:

JWT
│
├── Header
├── Payload
└── Signature

Önemli olarak JWT'nin Payload bölümü şifrelenmiş olmak zorunda değildir. Bu
nedenle parola gibi gizli bilgiler Payload içerisine konulmamalıdır. JWT'nin bütünlüğü
imza ile korunur.

### 6.3 OAuth ve OAuth 2.0
OAuth, bir uygulamanın kullanıcının parolasını doğrudan bilmesine gerek kalmadan
başka bir hizmetteki belirli kaynaklara erişebilmesini sağlayan yetkilendirme
yaklaşımıdır.

Örneğin bir uygulamanın Google hesabındaki belirli verilere erişmesine izin verilmesi
OAuth mantığıyla gerçekleştirilebilir.

OAuth 2.0, OAuth'un yaygın kullanılan modern sürümüdür ve erişim yetkilendirmesi için
standart bir çerçeve sağlar.

OAuth temel olarak Authorization (yetkilendirme) ile ilgilidir. Tek başına kimlik
doğrulama standardı değildir.

### 6.4 OpenID ve OpenID Connect
OpenID Connect (OIDC), OAuth 2.0 üzerine kurulmuş bir kimlik doğrulama
protokolüdür.

OAuth 2.0:

Bir uygulamanın hangi kaynaklara erişebileceğini belirlemeye odaklanır.

OpenID Connect:

Kullanıcının kim olduğunu doğrulamaya ve kullanıcı hakkında kimlik bilgileri sağlamaya
odaklanır.

Bu nedenle modern uygulamalarda OAuth 2.0 ve OpenID Connect birlikte kullanılabilir.

### 6.5 OpenIddict Nedir?
OpenIddict, .NET uygulamalarında OAuth 2.0 ve OpenID Connect tabanlı kimlik
doğrulama ve yetkilendirme sistemleri oluşturmak için kullanılabilen açık kaynaklı bir
kütüphanedir.

Örneğin bir ASP.NET Core uygulamasında kendi authorization server'ını oluşturmak için
kullanılabilir.

İlişkiyi basitleştirirsek:

OAuth 2.0
↓
Yetkilendirme çerçevesi

OpenID Connect
↓
Kimlik doğrulama protokolü
↓
OAuth 2.0 üzerine kuruludur

OpenIddict
↓

.NET uygulamalarında OAuth 2.0 / OIDC
uygulamak için kullanılan kütüphane

### 6.6 Performans Artırma Teknikleri
Web uygulamalarında performansı artırmak için sorguların, veritabanı erişiminin, ağ
iletişiminin ve uygulamanın kaynak kullanımının optimize edilmesi gerekir.

AsNoTracking
Entity Framework Core'da sadece okunacak veriler için AsNoTracking() kullanılabilir.

```csharp
var products = context.Products
.AsNoTracking()
.ToList();
```

Entity Framework bu nesneleri değişiklik takibine almadığı için özellikle sadece veri
okunan sorgularda gereksiz tracking maliyeti azaltılabilir.

### Asenkron Programlama
Veritabanı veya ağ gibi I/O işlemlerinde asenkron metotlar kullanılarak thread'lerin
gereksiz yere beklemesi azaltılabilir.

Örneğin:

```csharp
var products = await context.Products.ToListAsync();
```

Bu yöntem özellikle aynı anda çok sayıda isteğin işlendiği web uygulamalarında faydalı
olabilir.

Caching
Sık kullanılan ve kısa sürede değişmeyen verilerin tekrar tekrar veritabanından alınması
yerine geçici olarak saklanmasına caching denir.

Örneğin:

İstek
↓
Cache'te veri var mı?
↓
Evet → Cache'den getir
Hayır → Database → Cache'e kaydet

Böylece veritabanına yapılan gereksiz sorgular azaltılabilir.

Redis
Redis, verileri bellekte tutabilen hızlı bir veri deposudur ve caching amacıyla sık
kullanılır.

Örneğin sık görüntülenen bir ürün listesinin Redis'te tutulması sayesinde her istekte
doğrudan veritabanına gitmek yerine cache'den veri alınabilir.

Profiling
Profiling, uygulamanın hangi bölümlerinin ne kadar süre ve kaynak kullandığını analiz
etme işlemidir.

Örneğin:

Database Query → 1200 ms
Business Logic → 50 ms
API Response   → 30 ms

gibi ölçümler yapılarak performans problemi oluşturan bölüm tespit edilebilir.

IAsyncEnumerable
IAsyncEnumerable<T>, verilerin tamamının aynı anda belleğe alınması yerine
asenkron olarak parça parça işlenmesine olanak sağlar.

Özellikle büyük veri kümelerinde kaynak kullanımının azaltılmasına yardımcı olabilir.

Örneğin:

```csharp
await foreach (var product in GetProductsAsync())
{
// Ürünü işle
}
```

Performans tekniklerinin özeti

| Teknik | Amaç |
|---|---|
| AsNoTracking | Gereksiz EF Core tracking maliyetini azaltmak |
| Async/Await | I/O işlemlerinde thread'lerin gereksiz beklemesini azaltmak |
| Caching | Tekrarlanan veri erişimini azaltmak |
| Redis | Hızlı bellek tabanlı cache kullanmak |
| Profiling | Performans problemlerini tespit etmek |
| IAsyncEnumerable | Büyük verileri parça parça asenkron işlemek |

### 6.7 OWASP Top 10
OWASP (Open Worldwide Application Security Project), web uygulamalarının
güvenliği konusunda çalışmalar ve kaynaklar sağlayan bir kuruluştur. OWASP Top 10,
web uygulamalarında önemli güvenlik risklerini sınıflandıran ve farkındalık amacıyla
kullanılan bir listedir.

Aşağıdaki başlıklar OWASP Top 10'un 2021 sürümündeki kategorilerdir:

1. Broken Access Control

Kullanıcının sahip olmaması gereken kaynaklara veya işlemlere erişebilmesi
durumudur.

Örneğin normal bir kullanıcının yönetici işlemlerini gerçekleştirebilmesi.

Önlem: ASP.NET Core Authorization, role/policy kontrolleri ve sunucu tarafı erişim
kontrolleri kullanılmalıdır.

2. Cryptographic Failures

Hassas verilerin yeterince korunmaması veya yanlış kriptografik yöntemlerin
kullanılmasıdır.

Örneğin parolaların düz metin olarak saklanması.

Önlem: Güvenli hash algoritmaları, HTTPS ve uygun şifreleme yöntemleri
kullanılmalıdır.

3. Injection

Kullanıcıdan alınan verilerin güvenli şekilde işlenmemesi sonucunda saldırganın komut
veya sorgu ekleyebilmesidir. SQL Injection bunun önemli örneklerinden biridir.

Önlem: Parametreli sorgular, ORM'ler ve uygun input validation kullanılmalıdır.

4. Insecure Design

Uygulamanın tasarım aşamasında güvenlik gereksinimlerinin yeterince dikkate
alınmamasıdır.

Önlem: Güvenlik gereksinimleri tasarım aşamasından itibaren ele alınmalı ve tehdit
modelleme yapılmalıdır.

5. Security Misconfiguration

Güvenlik ayarlarının yanlış veya varsayılan şekilde bırakılmasıdır.

Örneğin gereksiz servislerin açık olması veya ayrıntılı hata bilgilerinin production
ortamında gösterilmesi.

Önlem: Güvenli varsayılan ayarlar kullanılmalı ve production ortamı düzenli olarak
kontrol edilmelidir.

6. Vulnerable and Outdated Components

Güvenlik açığı bulunan veya güncel olmayan kütüphanelerin ve framework'lerin
kullanılmasıdır.

Önlem: Kullanılan NuGet paketleri ve diğer bağımlılıklar düzenli olarak
güncellenmelidir.

7. Identification and Authentication Failures

Kimlik doğrulama ve oturum yönetimindeki hatalardır.

Örneğin zayıf parola politikaları veya hatalı session yönetimi.

Önlem: Güvenli Authentication mekanizmaları, güçlü parola politikaları ve uygun
token/session yönetimi kullanılmalıdır.

8. Software and Data Integrity Failures

Yazılım veya verilerin güvenilirliğinin yeterince doğrulanmamasıyla ilgili risklerdir.

Örneğin güvenilmeyen bir kaynaktan paket veya güncelleme kullanılması.

Önlem: Bağımlılıklar güvenilir kaynaklardan alınmalı ve CI/CD süreçlerinde bütünlük
kontrolleri uygulanmalıdır.

9. Security Logging and Monitoring Failures

Güvenlikle ilgili olayların yeterince loglanmaması veya izlenmemesidir.

Bu durum saldırıların geç fark edilmesine neden olabilir.

Önlem: Authentication, authorization ve kritik işlemler loglanmalı ve loglar düzenli
olarak izlenmelidir.

10. Server-Side Request Forgery (SSRF)

Sunucunun saldırgan tarafından kontrol edilen bir adresi veya iç ağdaki başka bir
kaynağı istemeye zorlanmasıdır.

Önlem: İzin verilen URL'ler sınırlandırılmalı, kullanıcı tarafından verilen adresler
doğrulanmalı ve sunucunun iç kaynaklara erişimi kontrol edilmelidir.

### SQL Injection, XSS ve CSRF

SQL Injection

Kullanıcı girdisinin güvenli şekilde işlenmemesi sonucunda saldırganın SQL sorgusunun
yapısını değiştirebilmesidir.

Önlem: EF Core gibi ORM'lerin güvenli sorgulama mekanizmaları, parametreli sorgular
ve input validation kullanılmalıdır.

XSS (Cross-Site Scripting)

Saldırganın web sayfasında başka kullanıcıların tarayıcılarında çalışabilecek zararlı
JavaScript kodu çalıştırabilmesidir.

Önlem: Kullanıcı girdileri doğrulanmalı ve uygun şekilde encode/sanitize edilmelidir.

CSRF (Cross-Site Request Forgery)

Kullanıcının oturumundan yararlanılarak, kullanıcının istemediği bir işlemin onun adına
gerçekleştirilmesidir.

Önlem: ASP.NET Core'da Anti-Forgery mekanizmaları ve uygun cookie ayarları
kullanılabilir.

## 7. LOGGING VE HATA YÖNETİMİ
### 7.1 Loglama Neden Yapılır?
Logging, uygulamanın çalışma sırasında gerçekleştirdiği işlemlerin ve oluşan olayların
kayıt altına alınmasıdır.

Loglama sayesinde:

•    Hatalar tespit edilebilir.
•    Uygulamanın çalışma durumu izlenebilir.
•    Kullanıcı işlemleri takip edilebilir.
•    Performans problemleri araştırılabilir.
•    Güvenlik olayları incelenebilir.

Örneğin:

2026-09-22 10:15:20
INFO - User logged in successfully.

veya:

2026-09-22 10:16:02
ERROR - Database connection failed.

### 7.2 Log Seviyeleri
ASP.NET Core'da yaygın log seviyeleri şunlardır:

| Seviye | Açıklama |
|---|---|
| Trace | En ayrıntılı log seviyesidir. Detaylı teknik bilgiler için kullanılır. |
| Debug | Geliştirme ve hata ayıklama sırasında kullanılan teknik bilgiler. |
| Information | Uygulamanın normal çalışma akışını gösteren bilgiler. |
| Warning | Henüz hata olmayan ancak dikkat edilmesi gereken durumlar. |
| Error | Bir işlemin başarısız olduğu hata durumları. |
| Critical | Uygulamanın veya önemli bir bölümünün çalışmasını etkileyen ciddi hatalar. |

Örneğin:

```csharp
logger.LogInformation("User logged in.");
logger.LogWarning("Password attempt limit is near.");
logger.LogError("Database connection failed.");
```

### 7.3 ASP.NET Core Logging Altyapısı
ASP.NET Core'da yerleşik bir ILogger altyapısı bulunur.

Örneğin:

public class ProductService
{
private readonly ILogger<ProductService> _logger;

public ProductService(ILogger<ProductService> logger)
{
_logger = logger;
}

public void GetProducts()
{
_logger.LogInformation("Products are being retrieved.");
}
}

Burada ILogger<ProductService> DI aracılığıyla Service sınıfına aktarılır.

ASP.NET Core farklı logging sağlayıcılarıyla çalışabilir. Örneğin Console ve Debug gibi
sağlayıcılar kullanılabilir; ayrıca Serilog gibi harici çözümler de entegre edilebilir.

### 7.4 Global Exception Handling
Uygulamada her Controller içerisinde ayrı ayrı try-catch yazmak yerine hataları
merkezi bir noktada yönetmek için global exception handling kullanılabilir.

ASP.NET Core'da bunun için UseExceptionHandler kullanılabilir.

Örneğin:

var app = builder.Build();

if (!app.Environment.IsDevelopment())
{
app.UseExceptionHandler("/error");
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();

UseExceptionHandler, uygulamada oluşan işlenmemiş exception'ların merkezi
şekilde ele alınmasını sağlar.

Böylece kullanıcıya teknik hata detaylarını göstermek yerine kontrollü bir hata cevabı
döndürülebilir.

Örneğin:

500 Internal Server Error

gibi bir cevap üretilebilir.

ILogger ile birlikte kullanım

try
{
// İşlem
}
catch (Exception ex)
{
_logger.LogError(ex, "An error occurred while processing the
request.");
throw;
}

Burada exception hem loglanır hem de uygun şekilde işlenmeye devam eder.

Önemli: Production ortamında kullanıcıya stack trace gibi ayrıntılı teknik hata bilgileri
gösterilmemelidir.

## 8. YAZILIM GELİŞTİRME PRENSİPLERİ

### 8.1 SOLID Prensipleri
SOLID, nesne yönelimli programlamada daha anlaşılır, sürdürülebilir, test edilebilir ve
esnek kod yazmayı amaçlayan beş temel prensibin baş harflerinden oluşur.

•    S → Single Responsibility Principle
•    O → Open/Closed Principle
•    L → Liskov Substitution Principle
•    I → Interface Segregation Principle
•    D → Dependency Inversion Principle

S – Single Responsibility Principle
Bir sınıfın yalnızca bir temel sorumluluğu olmalıdır.

Örneğin kullanıcı bilgilerini yöneten bir sınıfın aynı zamanda e-posta göndermesi ve
rapor oluşturması doğru bir tasarım olmayabilir.

Yanlış:

class User
{
void SaveToDatabase() { }
void SendEmail() { }
void GenerateReport() { }
}

Bunun yerine sorumluluklar ayrılabilir:

UserRepository → Veritabanı işlemleri
EmailService   → E-posta işlemleri
ReportService → Rapor işlemleri

O – Open/Closed Principle
Bir sınıf geliştirmeye açık, mevcut kodunu değiştirmeye kapalı olmalıdır.

Yeni bir davranış eklemek için mevcut sınıfı sürekli değiştirmek yerine yeni sınıflar veya
implementasyonlar eklenebilmelidir.

Örneğin ödeme sisteminde:

IPayment
├── CreditCardPayment
├── PaypalPayment
└── BankTransferPayment

Yeni bir ödeme yöntemi eklenirken mevcut ödeme sınıfını değiştirmek yerine yeni bir
sınıf oluşturulabilir.

L – Liskov Substitution Principle
Bir alt sınıf, kullanıldığı yerde üst sınıfın yerine geçebilmeli ve programın beklenen
davranışını bozmamalıdır.

Örneğin:

Animal
├── Dog
└── Cat

Animal beklenen bir yerde Dog veya Cat kullanıldığında sistemin davranışı
bozulmamalıdır.

I – Interface Segregation Principle
Bir sınıfın ihtiyaç duymadığı metotları içeren büyük bir interface'e bağımlı
olmaması gerekir.

Örneğin:

interface IWorker
{
void Work();
void Eat();
void Fly();
}

Bir robotun Eat() metoduna ihtiyacı yoksa bu interface gereğinden fazla sorumluluk
içeriyor olabilir.

Bunun yerine:

IWorkable
IEatable
IFlyable

gibi daha küçük interface'ler oluşturulabilir.

D – Dependency Inversion Principle
Üst seviye sınıflar doğrudan alt seviye sınıflara bağımlı olmamalı; abstraction'lara
bağımlı olmalıdır.

Örneğin:

public class ProductService
{
private readonly IProductRepository _repository;

public ProductService(IProductRepository repository)
{
_repository = repository;
}
}

Burada ProductService, doğrudan SqlProductRepository sınıfına değil:

IProductRepository

interface'ine bağımlıdır.

Bu prensip Dependency Injection ile birlikte sıkça uygulanır.

### 8.2 Design Patterns
Design Pattern, yazılım geliştirmede sık karşılaşılan problemlere yönelik tekrar
kullanılabilir tasarım çözümleridir.

Singleton Pattern
Bir sınıftan uygulama boyunca yalnızca tek bir nesne oluşturulmasını amaçlayan
pattern'dir.

Örneğin uygulama genelinde tek bir configuration yöneticisi kullanılmak istenebilir.

ASP.NET Core DI'da:

builder.Services.AddSingleton<IConfigurationService,
ConfigurationService>();

şeklinde Singleton yaşam süresi kullanılabilir.

Repository Pattern
Veritabanı erişim işlemlerini uygulamanın diğer bölümlerinden ayırmak için
kullanılabilir.

Örneğin:

```csharp
public interface IProductRepository
{
Product GetById(int id);
List<Product> GetAll();
}
```

Controller doğrudan SQL işlemleri yapmak yerine Repository üzerinden veri erişebilir.

Controller
↓
Service
↓
Repository
↓
Database

Factory Pattern
Nesne oluşturma işlemlerini doğrudan new kullanarak gerçekleştirmek yerine nesne
oluşturma sorumluluğunu ayrı bir yapıya vermek için kullanılabilir.

Örneğin farklı ödeme türlerine göre farklı nesneler oluşturulabilir:

PaymentFactory
↓
┌────┼─────────┐
↓    ↓         ↓
Card PayPal     Bank

Böylece nesne oluşturma mantığı tek bir yerde yönetilebilir.

### 8.3 Clean Code Nedir?
Clean Code, okunması, anlaşılması, test edilmesi ve değiştirilmesi kolay kod yazma
yaklaşımıdır.

Temel amaç sadece kodun çalışması değil, kodun diğer geliştiriciler tarafından da kolay
anlaşılabilmesidir.

Clean Code uygulamalarına örnekler

Anlamlı isimler kullanmak:

Kötü:

int x;

Daha iyi:

int productCount;

Metotları gereksiz büyütmemek:

Kötü:

CreateUserAndSendEmailAndGenerateReport()

Bunun yerine işlemler ayrı metotlara bölünebilir.

Gereksiz yorumlardan kaçınmak:

Kod zaten ne yaptığını açıkça ifade ediyorsa gereksiz yorumlar kullanılmamalıdır.

Tekrarlanan kodları azaltmak:

Aynı kod parçasını birçok yerde tekrar etmek yerine ortak bir metot veya sınıf
oluşturulabilir.

Tutarlı kodlama:

Değişken, metot ve sınıf isimlendirmelerinde tutarlı bir yapı kullanılmalıdır.

### 8.4 Yazılım Mimari Desenleri
Yazılım mimarisi, uygulamanın genel yapısının ve bileşenler arasındaki ilişkilerin nasıl
düzenleneceğini belirler.

Layered Architecture
Uygulama farklı katmanlara ayrılır.

Presentation
↓
Business
↓
Data Access

Kullanım: Küçük ve orta ölçekli, yapısı nispeten geleneksel olan uygulamalarda tercih
edilebilir.

### Clean Architecture
İş kurallarını merkeze alır ve dış teknolojilere olan bağımlılığı azaltmayı amaçlar.

API
↓
Application
↓
Domain
↑
Infrastructure

Kullanım: Uzun süre geliştirilecek, test edilebilirliği ve iş mantığının bağımsızlığını
önemseyen uygulamalarda tercih edilebilir.

Microservices
Uygulama tek bir büyük sistem yerine birbirinden bağımsız çalışabilen küçük
servislerden oluşturulur.

Örneğin:

User Service
│
Product Service
│
Order Service
│
Payment Service

Her servis kendi görevinden sorumlu olabilir.

Kullanım: Büyük ve bağımsız şekilde ölçeklenmesi gereken sistemlerde tercih
edilebilir. Ancak küçük projelerde gereksiz operasyonel karmaşıklık oluşturabilir.

Event-Driven Architecture
Sistem içerisindeki bileşenlerin event (olay) üzerinden iletişim kurduğu mimaridir.

Örneğin:

OrderCreated
↓
┌───┴─────────┐
↓             ↓
Email       Inventory
Service      Service

Sipariş oluşturulduğunda OrderCreated olayı yayınlanır ve ilgili servisler bu olaya göre
kendi işlemlerini yapar.

Kullanım: Bileşenlerin birbirinden daha bağımsız çalışmasının ve asenkron işlemlerin
önemli olduğu sistemlerde kullanılabilir.

Hexagonal Architecture (Ports & Adapters)
Hexagonal Architecture, uygulamanın temel iş mantığını dış sistemlerden ayırmayı
amaçlar.

Merkezde uygulama mantığı bulunur. Dış sistemlerle iletişim Port ve Adapter yapıları
üzerinden gerçekleştirilir.

Database
↓
Adapter
↓
Port
↓
Application
↑
Port
↑
Adapter
↑
Web API

Örneğin veritabanı değiştirildiğinde temel iş mantığının bundan mümkün olduğunca
etkilenmemesi amaçlanır.

8.5 Yazılım Mimarilerinin
Karşılaştırılması
| Mimari | Temel Özellik | Uygun Olabilecek Senaryo |
|---|---|---|
| Layered | Katmanlara ayrılmış yapı | Küçük ve orta ölçekli klasik uygulamalar |
| Clean Architecture | İş mantığını dış detaylardan ayırır | Uzun ömürlü ve test edilebilir uygulamalar |
| Microservices | Bağımsız servisler | Büyük ve ölçeklenebilir sistemler |
| Event-Driven | Event'ler üzerinden iletişim | Asenkron ve gevşek bağlı sistemler |
| Hexagonal | Ports & Adapters ile dış bağımlılıkları ayırır | Test edilebilirliği ve teknolojiden bağımsızlığı önemseyen sistemler |

### Genel karşılaştırma

Layered Architecture, yapısı daha basit ve anlaşılır olduğu için geleneksel
uygulamalarda kullanılabilir. Clean Architecture, iş mantığını veritabanı ve framework
gibi dış detaylardan ayırmaya odaklanır. Microservices, büyük uygulamaları bağımsız

servisler halinde geliştirmeyi amaçlar. Event-Driven Architecture, bileşenlerin olaylar
üzerinden iletişim kurmasını sağlar. Hexagonal Architecture ise uygulamanın çekirdek
mantığını dış sistemlerden Port ve Adapter'lar aracılığıyla ayırır. Mimari seçim yapılırken
projenin büyüklüğü, ekip yapısı, ölçeklenebilirlik ihtiyacı, bakım gereksinimleri ve
operasyonel karmaşıklık dikkate alınmalıdır.
