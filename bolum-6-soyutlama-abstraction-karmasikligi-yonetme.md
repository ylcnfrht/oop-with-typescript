# Bölüm 6: Soyutlama (Abstraction) - Karmaşıklığı Yönetme

Nesne Yönelimli Programlama'nın (OOP) dört ana prensibinden sonuncusu **soyutlama (abstraction)**'dır. Soyutlama, bir sistemin karmaşık detaylarını gizleyerek, yalnızca en temel ve ilgili bilgileri dış dünyaya sunma prensibidir. Amaç, kullanıcıların bir nesneyle veya sistemle etkileşim kurarken gereksiz detaylarla boğuşmasını engellemek, böylece odaklanmaları gereken şeylere odaklanmalarını sağlamaktır.

Gerçek dünyadan bir örnek verelim: Bir arabayı kullanırken, motorun iç işleyişi (pistonlar nasıl hareket eder, bujiler nasıl ateşlenir vb.) hakkında detaylı bilgi sahibi olmamıza gerek yoktur. Sadece direksiyon, gaz pedalı, fren ve vites kolu gibi "soyutlanmış" arayüzler aracılığıyla arabayı kullanırız. Motorun karmaşıklığı bizden gizlenmiştir. Soyutlama, yazılımda da bu prensibi uygular; karmaşık detayları gizlerken, kullanımı kolay ve anlaşılır bir arayüz sunar.

***

#### 6.1 Soyutlama Nedir? Neden Önemlidir?

**Soyutlama**, bir sınıfın veya fonksiyonun iç işleyişini, yani "nasıl çalıştığını" kullanıcıdan gizleyip, sadece "ne yaptığını" veya "ne sunduğunu" gösteren bir prensiptir. Bu, kullanıcıların bir API'yi veya bir nesneyi kullanırken karmaşık detaylarla uğraşmasını engeller, böylece kodun daha temiz, daha kolay anlaşılır ve daha az hataya açık olmasını sağlar.

**Soyutlamanın Temel Konseptleri:**

1. **Gereksiz Detayları Gizleme (Information Hiding):** Kapsülleme ile çok yakından ilişkilidir, ancak soyutlama daha geniş bir kavramdır. Kapsülleme, veri ve metotları bir araya getirirken ve özel üyeleri gizlerken, soyutlama bir adım daha ileri giderek tüm bir sistemin veya modülün nasıl çalıştığının karmaşıklığını gizler ve sadece temel işlevselliği sunar.
2. **Yüksek Seviyeli Bakış Açısı:** Bir sistemin veya bileşenin yüksek seviyeli bir görünümünü sağlar. Kullanıcı, detaylara takılmadan işlevselliği genel hatlarıyla anlayabilir ve kullanabilir.
3. **Arayüz Odaklı Tasarım:** Kullanıcıların etkileşim kurduğu "arayüz" üzerine odaklanır. Bu arayüz, genellikle public metotlar ve özellikler aracılığıyla tanımlanır.

**Soyutlamanın Nedenleri ve Önemi:**

* **Karmaşıklığı Yönetme:** Büyük ve karmaşık yazılım sistemlerinde, tüm detayları aynı anda anlamak imkansızdır. Soyutlama, bu karmaşıklığı yönetilebilir parçalara ayırır ve geliştiricilerin sadece o anda ilgilendikleri kısma odaklanmalarını sağlar.
* **Kullanım Kolaylığı:** Bir modül veya sınıfın kullanımı, içindeki yüzlerce satırlık kodun detaylarına inmeden mümkün olur. Sadece sunulan arayüzü bilmek yeterlidir.
* **Bakım ve Genişletilebilirlik:** İç detaylar gizlendiği için, bir sınıfın veya modülün dahili implementasyonu değiştirildiğinde, bu değişiklik dışarıdaki kodu etkilemez. Bu, bakım ve gelecekteki genişletmeleri çok daha kolay hale getirir. "Açık/Kapalı Prensibi" (Open/Closed Principle) ile uyumludur: bir varlık genişlemeye açık, ancak değişikliğe kapalı olmalıdır.
* **Hata Azaltma:** Kullanıcıların doğrudan dahili detaylarla etkileşime geçememesi, yanlış kullanımdan kaynaklanan hataların olasılığını azaltır.
* **İşlevselliği İyileştirme Fırsatı:** Dahili implementasyon gizlendiği için, performansı artırmak veya algoritmayı iyileştirmek gibi değişiklikler, kullanıcı kodunu etkilemeden yapılabilir.

**Soyutlama Nasıl Sağlanır?**

TypeScript'te soyutlama çeşitli yollarla sağlanır:

1. **Arayüzler (Interfaces):** Bir nesnenin dışarıya sunduğu davranış sözleşmesini tanımlar. İç implementasyon detayları hakkında hiçbir bilgi vermez. (Bölüm 5'te gördük.)
2. **Soyut Sınıflar (Abstract Classes):** Bir temel sınıfın kendisinin doğrudan örneklenemeyeceği (nesne oluşturulamayacağı) ve bazı metotlarının implementasyonunun (gövdesinin) alt sınıflara bırakılacağı durumu tanımlar.
3. **Erişim Belirleyiciler (`private`, `protected`):** Kapsülleme ile birlikte, belirli özellik ve metotları dış dünyadan gizleyerek soyutlamaya katkıda bulunur.
4. **Basit Fonksiyonlar/Metotlar:** Karmaşık mantığı tek bir iyi isimlendirilmiş fonksiyonun içine gizlemek.

Bu bölümde özellikle **soyut sınıflara** odaklanacağız, çünkü bunlar soyutlamanın en doğrudan OOP araçlarından biridir.

***

#### 6.2 Soyut Sınıflar (Abstract Classes) ve Metotlar

**Soyut sınıflar (Abstract Classes)**, doğrudan nesne örnekleri oluşturulamayan sınıflardır. Genellikle bir sınıf hiyerarşisinin başında yer alır ve alt sınıflar için ortak bir arayüz veya temel implementasyon sağlamak amacıyla tasarlanırlar.

**Soyut bir sınıfın temel özellikleri:**

* `abstract` anahtar kelimesi ile tanımlanır.
* Doğrudan `new AbstractClass()` şeklinde nesne oluşturulamaz.
* Hem **somut (concrete)** metotlara (implementasyonu olan metotlar) hem de **soyut (abstract)** metotlara (implementasyonu olmayan metotlar) sahip olabilir.
* Soyut metotlar, sadece bir imza (ad, parametreler, geri dönüş tipi) tanımlar ve gövdesi olmaz.
* Soyut metotları içeren bir sınıfın kendisi de soyut olmak zorundadır.
* Bir soyut sınıftan miras alan (türeyen) somut bir sınıf, temel sınıftaki tüm soyut metotları implemente etmek (gövdesini sağlamak) zorundadır. Aksi takdirde, türetilmiş sınıf da soyut olmak zorunda kalır.

**Neden Soyut Sınıflar Kullanılır?**

1. **Ortak Davranışın Şablonu:** Bir grup ilgili sınıf için ortak bir "şablon" veya "iskelet" görevi görür. Ortak özellikleri ve metotları soyut sınıfta tanımlayarak kod tekrarını azaltırız.
2. **Zorunlu Kılma:** Alt sınıfların belirli metotları implemente etmesini zorunlu kılar. Bu, belirli bir davranışın tüm türetilmiş sınıflarda bulunduğunu garanti eder.
3. **Polimorfizm Desteği:** Çok biçimlilik için güçlü bir temel sağlar. Soyut sınıf tipinde bir değişken tanımlayabilir ve bu değişkene alt sınıfların nesnelerini atayarak, soyut metotları polimorfik olarak çağırabiliriz.

**Örnek: Hesaplama Sistemi**

Farklı türdeki hesaplamaların (Ödeme, Vergi, İndirim) yapılacağı bir sistemi soyut sınıflarla modelleyelim. Genel bir `Calculator` soyut sınıfı oluşturalım.

```typescript
// Soyut Sınıf: Calculator
abstract class Calculator {
    protected name: string;

    constructor(name: string) {
        this.name = name;
    }

    // Soyut metot: calculate()
    // Her alt sınıf kendi hesaplama mantığını implemente etmek zorundadır.
    abstract calculate(amount: number, ...args: any[]): number;

    // Somut metot: displayCalculationType()
    // Bu metot tüm alt sınıflar için aynı kalır veya ezilebilir.
    displayCalculationType(): void {
        console.log(`Performing a '${this.name}' calculation.`);
    }
}

// Somut Sınıf: PaymentCalculator (Ödeme Hesaplayıcı)
// Calculator sınıfından miras alır ve calculate metodunu implemente eder.
class PaymentCalculator extends Calculator {
    constructor() {
        super("Payment");
    }

    // calculate metodunun implementasyonu
    calculate(baseAmount: number, taxRate: number = 0.18): number {
        // Ödeme = Temel Tutar + Vergi
        return baseAmount * (1 + taxRate);
    }
}

// Somut Sınıf: DiscountCalculator (İndirim Hesaplayıcı)
// Calculator sınıfından miras alır ve calculate metodunu implemente eder.
class DiscountCalculator extends Calculator {
    constructor() {
        super("Discount");
    }

    // calculate metodunun implementasyonu
    calculate(originalPrice: number, discountPercentage: number): number {
        // İndirimli Fiyat = Orijinal Fiyat - (Orijinal Fiyat * İndirim Yüzdesi)
        if (discountPercentage < 0 || discountPercentage > 100) {
            throw new Error("Discount percentage must be between 0 and 100.");
        }
        return originalPrice * (1 - (discountPercentage / 100));
    }
}

// const genericCalculator = new Calculator("Generic"); // Hata: Cannot create an instance of an abstract class.

const paymentCalc = new PaymentCalculator();
paymentCalc.displayCalculationType(); // Çıktı: Performing a 'Payment' calculation.
const totalPayment = paymentCalc.calculate(1000);
console.log(`Total Payment: $${totalPayment.toFixed(2)}`); // Çıktı: Total Payment: $1180.00 (1000 + %18 KDV)

const discountCalc = new DiscountCalculator();
discountCalc.displayCalculationType(); // Çıktı: Performing a 'Discount' calculation.
const discountedPrice = discountCalc.calculate(200, 25);
console.log(`Discounted Price: $${discountedPrice.toFixed(2)}`); // Çıktı: Discounted Price: $150.00 (200 - %25 indirim)

try {
    discountCalc.calculate(100, 120);
} catch (error: any) {
    console.error(`Error: ${error.message}`); // Çıktı: Error: Discount percentage must be between 0 and 100.
}

console.log("\n--- Polymorphic Usage ---");
const calculators: Calculator[] = [];
calculators.push(new PaymentCalculator());
calculators.push(new DiscountCalculator());

calculators.forEach(calc => {
    calc.displayCalculationType();
    if (calc instanceof PaymentCalculator) {
        console.log(`Calculated value: $${calc.calculate(500).toFixed(2)}\n`);
    } else if (calc instanceof DiscountCalculator) {
        console.log(`Calculated value: $${calc.calculate(300, 10).toFixed(2)}\n`);
    }
});
```

Bu örnekte:

* `Calculator` sınıfı `abstract` olarak tanımlanmıştır ve doğrudan nesne oluşturulamaz.
* `calculate()` metodu `abstract` olarak tanımlanmıştır, bu da bir gövdesinin olmadığı ve tüm alt sınıflar tarafından implemente edilmesi gerektiği anlamına gelir.
* `displayCalculationType()` metodu ise `Calculator` sınıfında somut bir implementasyona sahiptir, bu sayede tüm alt sınıflar bu metodu doğrudan kullanabilir veya isterlerse ezebilirler.
* `PaymentCalculator` ve `DiscountCalculator` sınıfları `Calculator`'dan miras alır ve her ikisi de kendi `calculate()` metotlarını farklı mantıklarla implemente eder. Bu, her bir hesaplayıcının kendi soyutlanmış davranışını sunmasını sağlar.
* `calculators` dizisi `Calculator[]` tipindedir ve polimorfik olarak farklı hesaplayıcı türlerini tutabilir. `instanceof` operatörü kullanılarak nesnenin gerçek tipi kontrol edilebilir, ancak daha iyi bir tasarım genellikle bu tür koşullu kontrolleri gerektirmez; metotları doğrudan çağırırsınız ve polimorfizm işi halleder.

**Soyut Sınıflar ve Arayüzler Arasındaki Temel Fark:**

* **Arayüzler (`interface`):** Yalnızca bir sözleşme veya bir davranış setini tanımlar. Hiçbir implementasyon (kod) içermez. Bir sınıf birden fazla arayüzü uygulayabilir. Daha çok "bir nesne NE yapabilir?" sorusuna odaklanır.
* **Soyut Sınıflar (`abstract class`):** Hem soyut metotlar (implementasyon içermeyen) hem de somut metotlar (implementasyon içeren) ve özellikler içerebilir. Bir sınıf sadece tek bir soyut sınıftan miras alabilir. Daha çok "bu sınıf bir ŞEYDİR ve bazı temel davranışları vardır, bazılarını ise alt sınıflar tamamlamalıdır" sorusuna odaklanır. Soyut sınıflar, ortak kodu paylaşmak ve bir tür hiyerarşisi oluşturmak için kullanılır.

Her ikisi de soyutlama ve polimorfizm için kullanılır, ancak farklı kullanım senaryolarına sahiptirler.

***

#### 6.3 Soyutlama ile Tasarımın İyileştirilmesi

Soyutlama, yazılım tasarımının kalitesini önemli ölçüde artırır. Geliştiricilerin ve son kullanıcıların, bir sistemin yalnızca ilgili bölümlerine odaklanmasını sağlayarak karmaşıklığı azaltır.

**Soyutlamanın Tasarım Üzerindeki Etkileri:**

1. **Gereksiz Detayların Gizlenmesi:**
   * Bir modül veya bileşen kullanılırken, geliştiricinin iç işleyiş mekanizmalarını bilmesine gerek kalmaz. Sadece sunulan arayüzü ve beklenen davranışı anlaması yeterlidir. Bu, bilişsel yükü azaltır.
   * **Örnek:** Bir web tarayıcısında bir URL girdiğinizde, tarayıcının ağ isteklerini nasıl yönettiği, HTML'i nasıl ayrıştırdığı, CSS'i nasıl uyguladığı gibi detaylarla ilgilenmeyiz. Sadece web sitesinin görüntülenmesini bekleriz. Tarayıcı bu karmaşık detayları bizden soyutlar.
2. **API Tasarımında Sadakat:**
   * Harici olarak açığa çıkarılan (public) arayüzün sade, tutarlı ve kullanımı kolay olmasını sağlar. Gereksiz veya dahili metotlar/özellikler gizlenir.
   * **Örnek:** Bir e-ticaret uygulamasında, bir `PaymentProcessor` (Ödeme İşlemcisi) sınıfı olabilir. Bu sınıfın dışarıya açık `processPayment(amount: number, cardDetails: CardInfo)` gibi bir metodu varken, dahili olarak `private` veya `protected` metotlar (örneğin `validateCardDetails()`, `sendToGateway()`, `logTransaction()`) içerebilir. Kullanıcının sadece `processPayment` metodunu bilmesi yeterlidir.
3. **Bakım ve Evrim:**
   * Bir sistemin iç detayları soyutlandığında, bu detaylar değiştirildiğinde (örneğin, bir veritabanı türü değiştiğinde, bir algoritma optimize edildiğinde), dış arayüz aynı kaldığı sürece, sistemi kullanan kodun yeniden yazılması gerekmez. Bu, yazılımın daha dayanıklı ve geleceğe dönük olmasını sağlar.
   * **Örnek:** Bir `DataStore` (Veri Deposu) arayüzü tanımladınız ve `MongoDBDataStore` ve `PostgreSQLDataStore` sınıfları bu arayüzü uyguladı. Kodunuz `DataStore` arayüzünü kullanarak verilere eriştiği sürece, temel alınan veritabanı teknolojisini değiştirdiğinizde, kodunuzun geri kalanını güncellemeniz gerekmez.
4. **Yeniden Kullanılabilirlik ve Modülerlik:**
   * Soyutlama, daha küçük, bağımsız ve yeniden kullanılabilir modüller oluşturmayı teşvik eder. Her modül kendi sorumluluğunu yerine getirir ve diğer modüllerle sadece iyi tanımlanmış, soyut arayüzler aracılığıyla etkileşime girer.
   * **Örnek:** Bir grafik çizim kütüphanesinde, `Shape` (Şekil) soyut sınıfı olabilir ve `draw()` soyut metodu içerebilir. `Circle`, `Rectangle` gibi alt sınıflar `draw()` metodunu implemente eder. Kullanıcılar, her şeklin nasıl çizildiğini bilmek zorunda kalmadan, `Shape` dizisi üzerinde `draw()` çağırarak tüm şekilleri çizebilir.
5. **Test Edilebilirlik:**
   * Soyutlamanın sağladığı gevşek bağlılık, birim testlerini kolaylaştırır. Bir bileşeni test ederken, onun bağımlı olduğu karmaşık alt sistemler yerine, soyut arayüzlerini uygulayan basit "mock" (sahte) veya "stub" (taklit) objeleri kullanabilirsiniz.

**Örnek Senaryo: Uzaktan API Entegrasyonu**

Bir uygulamanın harici bir hava durumu API'siyle entegre olması gerektiğini düşünelim.

```typescript
// Arayüz: IWeatherService
// Hava durumu servisinin dış dünyaya sunduğu sözleşme
interface IWeatherService {
    getCurrentTemperature(city: string): Promise<number>;
    getForecast(city: string, days: number): Promise<string[]>;
}

// Somut Sınıf: OpenWeatherMapService (Gerçek API implementasyonu)
// Arayüzü uygulayarak, OpenWeatherMap API'sine özel detayları içerir
class OpenWeatherMapService implements IWeatherService {
    private apiKey: string;

    constructor(apiKey: string) {
        this.apiKey = apiKey;
    }

    async getCurrentTemperature(city: string): Promise<number> {
        // API çağrısı, JSON parse etme, hata yönetimi gibi karmaşık detaylar burada
        // Gerçekte fetch/axios gibi kütüphaneler kullanılır
        console.log(`(Simulating API call to OpenWeatherMap for ${city})...`);
        const temperature = Math.floor(Math.random() * 20) + 15; // Rastgele sıcaklık
        return Promise.resolve(temperature);
    }

    async getForecast(city: string, days: number): Promise<string[]> {
        console.log(`(Simulating API call to OpenWeatherMap for ${city}'s ${days}-day forecast)...`);
        const forecast = [`Day 1: Sunny`, `Day 2: Cloudy`, `Day 3: Rain`];
        return Promise.resolve(forecast.slice(0, days));
    }
}

// Somut Sınıf: AccuWeatherService (Başka bir API implementasyonu)
// Başka bir servis için farklı bir implementasyon
class AccuWeatherService implements IWeatherService {
    private token: string;

    constructor(token: string) {
        this.token = token;
    }

    async getCurrentTemperature(city: string): Promise<number> {
        console.log(`(Simulating API call to AccuWeather for ${city})...`);
        const temperature = Math.floor(Math.random() * 18) + 18; // Farklı sıcaklık aralığı
        return Promise.resolve(temperature);
    }

    async getForecast(city: string, days: number): Promise<string[]> {
        console.log(`(Simulating API call to AccuWeather for ${city}'s ${days}-day forecast)...`);
        const forecast = [`Tomorrow: Clear`, `Next Day: Breezy`];
        return Promise.resolve(forecast.slice(0, days));
    }
}

// Uygulamanın hava durumu özelliğini kullanan ana kısım
class WeatherApp {
    private weatherService: IWeatherService; // Buradaki tip Arayüzdür!

    constructor(service: IWeatherService) {
        this.weatherService = service;
    }

    async showCurrentWeather(city: string): Promise<void> {
        console.log(`\nFetching current weather for ${city}...`);
        const temp = await this.weatherService.getCurrentTemperature(city);
        console.log(`The current temperature in ${city} is ${temp}°C.`);
    }

    async showWeatherForecast(city: string, days: number): Promise<void> {
        console.log(`\nFetching ${days}-day forecast for ${city}...`);
        const forecast = await this.weatherService.getForecast(city, days);
        console.log(`Forecast for ${city}:`);
        forecast.forEach((day, index) => console.log(`  ${day}`));
    }
}

// Uygulamayı farklı servislerle başlatma
console.log("--- Using OpenWeatherMap Service ---");
const openWeatherService = new OpenWeatherMapService("my_openweathermap_api_key");
const appWithOpenWeather = new WeatherApp(openWeatherService);
appWithOpenWeather.showCurrentWeather("Istanbul");
appWithOpenWeather.showWeatherForecast("Istanbul", 3);

console.log("\n--- Using AccuWeather Service ---");
const accuWeatherService = new AccuWeatherService("my_accuweather_token");
const appWithAccuWeather = new WeatherApp(accuWeatherService);
appWithAccuWeather.showCurrentWeather("Ankara");
appWithAccuWeather.showWeatherForecast("Ankara", 2);
```

Bu örnekte:

* `IWeatherService` arayüzü, hava durumu servisinden beklenen ortak davranışları soyutlar (`getCurrentTemperature`, `getForecast`). Kullanıcı, bu arayüzü uygulayan somut servislerin (OpenWeatherMapService, AccuWeatherService) nasıl çalıştığını bilmek zorunda değildir.
* `OpenWeatherMapService` ve `AccuWeatherService` sınıfları, bu arayüzü implemente ederek kendi API'lerine özgü detayları kapsüllerler.
* `WeatherApp` sınıfı, doğrudan bir `OpenWeatherMapService` veya `AccuWeatherService`'e bağımlı olmak yerine, `IWeatherService` arayüzüne bağımlıdır. Bu, `WeatherApp`'in hangi hava durumu servisiyle çalıştığını bilmesine gerek kalmadan işini yapmasını sağlar.
* Bu sayede, gelecekte yeni bir hava durumu servisi (`DarkSkyService` gibi) entegre edilmek istendiğinde, sadece yeni servisi `IWeatherService` arayüzünü uygulayarak oluşturmanız yeterlidir. `WeatherApp` sınıfında hiçbir değişiklik yapmanıza gerek kalmaz. İşte bu, soyutlamanın getirdiği esneklik ve bakım kolaylığıdır.

Soyutlama, yazılımın karmaşık yüzeyini azaltan, geliştiricilerin daha verimli çalışmasını sağlayan ve sistemlerin daha kolay evrimleşmesine olanak tanıyan bir prensiptir. Tüm OOP prensipleri gibi, soyutlama da daha iyi, daha sağlam ve sürdürülebilir yazılım sistemleri oluşturmak için bir yol göstericidir.

***

Böylece, Nesne Yönelimli Programlama'nın dört temel prensibini (Kapsülleme, Miras Alma, Çok Biçimlilik ve Soyutlama) tamamlamış bulunuyoruz. Her bir prensip, bir diğerini tamamlayarak daha güçlü ve esnek yazılım tasarımları oluşturmamıza olanak tanır.

Bu prensipleri anladığınızda, sadece TypeScript veya herhangi bir OOP dilini öğrenmekle kalmaz, aynı zamanda daha iyi yazılım mühendisleri olursunuz. Kodunuzun neden belirli bir şekilde organize edildiğini, neden belirli yapıları kullandığınızı ve gelecekteki değişikliklere nasıl adapte olabileceğinizi daha iyi anlarsınız.

Umarım bu kapsamlı anlatım serisi, Nesne Yönelimli Programlama'yı ve TypeScript'teki uygulamasını derinlemesine anlamanıza yardımcı olmuştur.
