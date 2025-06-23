---
icon: indent
cover: .gitbook/assets/Screenshot 2025-06-23 at 19.57.55.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# İçindekiler (Index)

#### Bölüm 1: Nesne Yönelimli Programlamaya Giriş

1. **Giriş ve Kitap Hakkında**
   * Kitabın Amacı ve Hedef Kitlesi
   * TypeScript Neden OOP için Mükemmel Bir Seçim?
   * OOP'nin Yazılım Geliştirmedeki Rolü ve Önemi
2. **Temel Kavramlar**
   * Nesne Nedir? Sınıf Nedir?
   * Özellik (Property) Nedir? Metot (Method) Nedir?
   * Örnek (Instance) Nedir?
3. **OOP Paradigmasının Tarihsel Gelişimi**
   * Prosedürel Programlamadan OOP'ye Geçiş
   * OOP'nin Avantajları ve Dezavantajları
4. **TypeScript'e Hızlı Bir Bakış**
   * TypeScript Neden Var? (JavaScript'in Eksiklikleri)
   * Temel TypeScript Tipleri (`string`, `number`, `boolean`, `array`, `tuple`, `enum`, `any`, `unknown`, `void`, `null`, `undefined`, `object`)
   * TypeScript Derleyicisi (TSC) ve Çalışma Ortamı

***

#### Bölüm 2: Sınıflar (Classes) - Nesnelerin Temeli

1. **Sınıf Tanımlama ve Söz Dizimi**
   * `class` Anahtar Kelimesi ve Sınıf Yapısı
   * **Örnekler:** Basit bir `Person` veya `Product` sınıfı tanımı.
2. **Özellikler (Properties) ve Üyeler (Members)**
   * Özellik Bildirimi ve Tip Tanımlamaları
   * Varsayılan Değerler, Opsiyonel Özellikler (`?`) ve Salt Okunur Özellikler (`readonly`)
   * **Örnekler:** Bir `Book` sınıfının `title`, `author`, `pageCount` özelliklerinin tanımlanması ve kullanımı.
3. **Kurucu Metotlar (Constructors)**
   * `constructor` Anahtar Kelimesi ve Nesne Oluşturma Süreci
   * Parametreli Kurucular ve Parametre Özellikleri (Parameter Properties)
   * **Örnekler:** `Student` sınıfının `name`, `surname`, `studentId` ile başlatılması.
4. **Metotlar (Methods)**
   * Metot Tanımlama, Çağırma ve `this` Anahtar Kelimesi
   * Metot Parametreleri ve Geri Dönüş Tipleri
   * **Örnekler:** Bir `Account` sınıfının `deposit()`, `withdraw()`, `getBalance()` metotları.
5. **Erişim Belirleyiciler (Access Modifiers)**
   * **`public`:** Varsayılan Erişim.
   * **`private`:** Sadece sınıf içinden erişim. Kapsülleme prensibi.
   * **`protected`:** Sınıf içinden ve miras alan alt sınıflardan erişim.
   * Erişim belirleyicilerin OOP'deki rolü ve güvenlik açısından önemi.
6. **Getters ve Setters (Accessor Properties)**
   * Özelliklere erişimi kontrol etme ve Veri Doğrulama.
   * **Örnekler:** Bir `Temperature` sınıfının Celsius ve Fahrenheit dönüşümleri için getter/setter kullanımı.
7. **Statik Üyeler (Static Members)**
   * Statik Özellikler ve Statik Metotlar.
   * Yardımcı Metotlar ve Sabit Değerler için Kullanımı.
   * **Örnekler:** `Math` sınıfına benzer bir `UtilityFunctions` sınıfı, sayaç uygulamaları.

***

#### Bölüm 3: Kapsülleme (Encapsulation) - Veri Güvenliği

1. **Kapsülleme Nedir?**
   * Veri ve Metotları Bir Araya Getirme.
   * Bilgi Saklama (Information Hiding) Prensibi.
   * Modülerlik ve Bakım Kolaylığı Üzerindeki Etkileri.
2. **Neden Kapsülleme?**
   * Veri Bütünlüğünü Sağlama ve Nesne Durumunu Kontrol Altında Tutma.
   * Değişikliklerin Etkisini Azaltma (Low Coupling).
3. **TypeScript'te Kapsülleme Teknikleri**
   * **`private` ve `protected` Erişim Belirleyiciler:** Derinlemesine inceleme ve örnekler.
   * **Getters ve Setters:** Kapsüllemeyi nasıl destekler? Veri doğrulama senaryoları.
   * **Readonly Özellikler:** Değişmez (Immutable) Veri.
   * **Örnek:** Bir `BankAccount` sınıfında `balance` (bakiye) özelliğinin `private` yapılması ve `deposit()`, `withdraw()` metotları ile yönetilmesi.
4. **Kapsülleme Uygulama Örnekleri**
   * Bir **`GameCharacter`** sınıfı: Can (health), enerji (energy) gibi özelliklerin güvenli yönetimi.
   * Bir **`Order`** sistemi: Sipariş durumu, ürün listesi gibi verilerin bütünlüğü.
5. **Kapsüllemenin Dezavantajları ve Aşırı Kapsülleme**
   * Esneklik Kaybı?
   * Aşırı Kapsüllemenin Sakıncaları.
   * Doğru Dengeyi Bulma.

***

#### Bölüm 4: Miras Alma (Inheritance) - Kod Yeniden Kullanımı

1. **Miras Alma Nedir?**
   * "Is-A" (Bir ... dır) İlişkisi.
   * Temel Sınıf (Base Class / Parent Class) ve Türetilmiş Sınıf (Derived Class / Child Class).
   * Kod Tekrarını Azaltma (DRY Prensibi).
2. **`extends` Anahtar Kelimesi**
   * Söz Dizimi ve Kullanımı.
   * Tekli Miras (Single Inheritance) - Neden TypeScript'te sadece tekli miras var?
   * **Örnek:** `Animal` sınıfından `Dog` ve `Cat` sınıflarına miras alma.
3. **Kurucu Metotlarda Miras Alma (`super()`)**
   * Üst Sınıf Kurucusunu Çağırma Zorunluluğu.
   * Parametrelerin Aktarılması.
   * **Örnekler:** `Vehicle` sınıfından `Car` ve `Bicycle` sınıflarına miras alma ve `super()` kullanımı.
4. **Metot Geçersiz Kılma (Method Overriding)**
   * Alt Sınıfta Üst Sınıf Metodunu Yeniden Tanımlama.
   * `super.methodName()` ile Üst Sınıf Metoduna Erişim.
   * **Örnekler:** `Shape` sınıfından türeyen `Circle` ve `Rectangle` sınıflarının `draw()` metodunu farklı şekilde uygulaması.
5. **Erişim Belirleyicilerin Mirasta Davranışı**
   * `public`, `private`, `protected` üyelerin miras alma esnasındaki erişim farklılıkları.
   * `private` üyelerin miras alınmaması.
6. **Soyut Sınıflar (Abstract Classes)**
   * `abstract` Anahtar Kelimesi.
   * Doğrudan Nesne Oluşturulamaması.
   * Soyut Metotlar ve Özellikler: Alt Sınıflar tarafından Uygulama Zorunluluğu.
   * Soyut sınıfların interface'lerden farkı.
   * **Örnekler:** `Employee` soyut sınıfından `Manager` ve `Engineer` türetme ve `calculateSalary()` soyut metodunu uygulama.
7. **Miras Alma ve Kompozisyon (Composition over Inheritance)**
   * Miras almanın potansiyel sorunları (Tight Coupling).
   * Kompozisyonun avantajları (Has-A İlişkisi).
   * Hangi durumda hangisini tercih etmeli?
   * **Örnekler:** Bir `Engine` sınıfını bir `Car` sınıfına dahil etme (kompozisyon).

***

#### Bölüm 5: Çok Biçimlilik (Polymorphism) - Esnek Tasarım

1. **Çok Biçimlilik Nedir?**
   * "Many Forms" (Çok Biçim).
   * Aynı Arayüze Farklı Davranışlar.
   * Dinamik Bağlama (Dynamic Binding) veya Geç Bağlama (Late Binding).
2. **TypeScript'te Çok Biçimlilik Uygulamaları**
   * **Miras Alma ile Çok Biçimlilik (Method Overriding):**
     * Bir üst sınıf referansı üzerinden alt sınıf metotlarını çağırma.
     * **Örnekler:** `Animal` dizisinde farklı hayvanların `makeSound()` metodunu çağırma ve çıktılarının farklı olması.
   * **Arayüzler (Interfaces) ile Çok Biçimlilik:**
     * Arayüzlerin bir sözleşme olarak kullanılması.
     * Farklı sınıfların aynı arayüzü uygulayarak çok biçimli davranması.
     * **Örnekler:** `Printable` arayüzünü uygulayan `Document` ve `Image` sınıfları.
   * **Fonksiyonel Çok Biçimlilik (Overloading):**
     * Aynı isimde farklı imzalı metotlar (TypeScript'te bu nasıl taklit edilir?).
     * Union Tipler ve Fonksiyon Aşırı Yüklemeleri (Function Overloads).
3. **Jenerikler (Generics) ve Çok Biçimlilik İlişkisi**
   * Jeneriklerin kodun genel ve yeniden kullanılabilir olmasını nasıl sağladığı.
   * Tip Parametreleri ile esneklik.
   * **Örnekler:** Jenerik bir `List` sınıfı veya jenerik bir `dataProcessor` fonksiyonu.
4. **Soyut Sınıflar ve Arayüzlerin Çok Biçimlilikteki Rolü**
   * Ortak arayüzler tanımlayarak esneklik sağlama.
   * Gevşek Bağlı (Loosely Coupled) Sistem Tasarımı.
5. **Çok Biçimliliğin Faydaları**
   * Esneklik ve Genişletilebilirlik.
   * Kod Tekrarının Azaltılması.
   * Daha Anlaşılır ve Bakımı Kolay Kod.
   * Test Edilebilirliğin Artırılması.
6. **Gerçek Dünya Çok Biçimlilik Senaryoları**
   * Bir ödeme sistemi: Farklı ödeme yöntemleri (kredi kartı, PayPal, havale) için ortak bir arayüz.
   * Bir kullanıcı arayüzü kütüphanesi: Farklı UI bileşenleri (Button, Checkbox) için ortak bir olay işleyici.

***

#### Bölüm 6: Soyutlama (Abstraction) - Karmaşıklığı Yönetme

1. **Soyutlama Nedir?**
   * Temel Özellikleri Ortaya Çıkarma, Detayları Gizleme.
   * Kullanıcının Sadece Bilmesi Gerekenleri Gösterme.
   * Odaklanmayı Kolaylaştırma.
2. **Neden Soyutlama?**
   * Karmaşıklığı Azaltma.
   * Sistemleri Daha Anlaşılır Hale Getirme.
   * Değişikliklerin Etkisini Azaltma (Implementation Details Gizleme).
3. **TypeScript'te Soyutlama Teknikleri**
   * **Arayüzler (Interfaces):**
     * Sadece bir sözleşme sunar, uygulama detaylarını gizler.
     * **Örnekler:** Bir `DataRepository` arayüzü ve farklı veri tabanları için uygulamaları.
   * **Soyut Sınıflar (Abstract Classes):**
     * Ortak yapıyı tanımlarken, bazı metotların uygulamasını alt sınıflara bırakır.
     * **Örnekler:** Bir `User` soyut sınıfı ve `Admin` ve `Guest` sınıfları.
   * **Erişim Belirleyiciler (`private`, `protected`):**
     * Dahili uygulama detaylarını gizleyerek soyutlamaya katkıda bulunur.
   * **Modüller ve İçe Aktarma/Dışa Aktarma:**
     * Sadece gerekli olanları dışarıya açma.
4. **Soyutlama Düzeyleri**
   * Yüksek Seviye Soyutlama vs. Düşük Seviye Soyutlama.
   * Uygun Soyutlama Düzeyini Belirleme.
5. **Gerçek Dünya Soyutlama Senaryoları**
   * Bir grafik kütüphanesi: Çizim fonksiyonları (drawCircle, drawRectangle) alttaki piksel manipülasyonunu soyutlar.
   * Bir işletim sistemi: Dosya sistemi, bellek yönetimi gibi karmaşık detayları kullanıcıdan gizler.
   * Bir web servisi: API'ler, alttaki veritabanı veya iş mantığını soyutlar.
6. **Soyutlamanın Aşırı Kullanımı ve Sakıncaları**
   * Gereksiz katmanlar oluşturma.
   * Anlaşılması zorlaşan kod.

***

#### Bölüm 7: SOLID Prensipleri

1. **SOLID Prensipleri - Temel OOP Tasarım Rehberleri**
   * **S - Single Responsibility Principle (Tek Sorumluluk Prensibi):**
     * Bir sınıfın veya modülün yalnızca bir nedeni olmalı değişmek için.
     * **Örnek:** Bir `User` sınıfının sadece kullanıcı bilgisi tutması, e-posta gönderme görevinin başka bir sınıfta olması.
   * **O - Open/Closed Principle (Açık/Kapalı Prensibi):**
     * Yazılım varlıkları geliştirmeye açık, değiştirmeye kapalı olmalıdır.
     * **Örnek:** Yeni bir ödeme yöntemi eklendiğinde mevcut kodun değiştirilmesi yerine yeni bir sınıf eklenmesi.
   * **L - Liskov Substitution Principle (Liskov Yerine Geçme Prensibi):**
     * Temel türlerin örnekleri, türetilmiş türlerin örnekleriyle değiştirilebilir olmalıdır.
     * **Örnek:** `Bird` sınıfından türeyen `Penguin` sınıfının `fly()` metodunu desteklememesi.
   * **I - Interface Segregation Principle (Arayüz Ayırma Prensibi):**
     * Müşteriler, kullanmadıkları arayüzlere bağımlı olmaya zorlanmamalıdır.
     * **Örnek:** Çok büyük bir arayüz yerine küçük, amaca özel arayüzler oluşturma.
   * **D - Dependency Inversion Principle (Bağımlılık Tersine Çevirme Prensibi):**
     * Üst düzey modüller alt düzey modüllere bağlı olmamalıdır; her ikisi de soyutlamalara bağlı olmalıdır.
     * **Örnek:** Bir `ReportGenerator` sınıfının doğrudan bir veritabanı sınıfına bağımlı olmak yerine bir `DataSource` arayüzüne bağımlı olması.
2. **SOLID Prensiplerinin Faydaları**
   * Daha Esnek ve Genişletilebilir Sistemler.
   * Daha Kolay Bakım ve Test Edilebilirlik.
   * Kod Kalitesini Artırma.
3. **SOLID Prensiplerini Uygulama Örnekleri**
   * Her prensip için TypeScript ile somut kod örnekleri.
   * Ortak hatalardan kaçınma yolları.

***

#### Bölüm 8: TypeScript'te İleri OOP Konuları ve En İyi Pratikler

1. **Mixins (Karışımlar)**
   * Çoklu miras sorununu çözme.
   * Sınıflara ek yetenekler dinamik olarak ekleme.
   * TypeScript'te Mixin implementasyonu ve örnekleri.
2. **Dekoratörler (Decorators)**
   * Deneysel bir özellik: Sınıflara, metotlara, özelliklere veya parametrelere metadata ekleme.
   * Kullanım Alanları (Logging, Authentication, Validation).
   * **Örnekler:** Bir metot çalışmadan önce veya sonra bir şeyler yapan bir dekoratör.
3. **Namespace'ler ve Modüller**
   * Kod organizasyonu için farklı yaklaşımlar.
   * Modüllerin (ES Modules) modern TypeScript projelerindeki önemi.
4. **Type Guards ve `instanceof` Operatörü**
   * Çalışma zamanında nesnenin tipini kontrol etme.
   * Çok biçimlilikle birleşimi.
5. **OOP ile Test Edilebilirlik**
   * Birim Testleri (Unit Tests) ve Entegrasyon Testleri.
   * Bağımlılık Enjeksiyonu (Dependency Injection) prensibi ve testlere etkisi.
6. **En İyi Pratikler ve Anti-Pattern'ler**
   * DRY (Don't Repeat Yourself) Prensibi.
   * KISS (Keep It Simple, Stupid) Prensibi.
   * YAGNI (You Ain't Gonna Need It) Prensibi.
   * Code Smells ve Refactoring.
   * Global Durumdan Kaçınma.

***

#### Bölüm 9: Pratik Bir Proje Örneği

1. **Örnek Proje Girişi**
   * Basit bir senaryo seçimi (Örn: Bir E-Ticaret Sepeti, Kütüphane Yönetim Sistemi).
   * Projenin gereksinimleri.
2. **OOP Prensiplerini Uygulama**
   * Projede kullanılacak sınıfların ve arayüzlerin belirlenmesi.
   * Kapsülleme, miras, çok biçimlilik ve soyutlama prensiplerinin projede nasıl uygulandığının adım adım gösterimi.
   * SOLID prensiplerine uygun tasarım kararları.
3. **Adım Adım Geliştirme ve Açıklamalar**
   * Sınıf tanımlamaları, metot implementasyonları.
   * Farklı senaryolar için testler.
   * Karşılaşılan zorluklar ve çözüm yaklaşımları.
4. **Proje Sonucu ve Değerlendirme**
   * Elde edilen avantajlar.
   * Daha fazla geliştirme fikirleri.

***

#### Ekler (Appendix)

* **TypeScript Geliştirme Ortamı Kurulumu:** Node.js, npm, VS Code, TypeScript derleyicisi.
* **Temel Git Komutları:** GitBook için gerekli temel Git bilgileri.
* **Önerilen Kaynaklar:** Kitaplar, bloglar, online kurslar.
* **Terimler Sözlüğü (Glossary)**
