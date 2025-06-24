# Bölüm 1: Nesne Yönelimli Programlamaya Giriş

#### 1.1 Giriş ve Kitap Hakkında

Merhaba sevgili okuyucu! Yazılım geliştirme dünyasında sağlam, bakımı kolay ve ölçeklenebilir uygulamalar oluşturmak, her geliştiricinin hedefidir. Bu hedefe ulaşmada bize yol gösteren en güçlü yaklaşımlardan biri **Nesne Yönelimli Programlama (OOP)**'dır. Bu kitap, modern web geliştirmenin vazgeçilmezi olan **TypeScript** dilini kullanarak OOP prensiplerini derinlemesine keşfetmenizi sağlayacak.

TypeScript, JavaScript'in üzerine inşa edilmiş, statik olarak tipleştirilmiş bir dil olup, büyük ölçekli uygulamaların geliştirilmesini çok daha kolay ve güvenli hale getirir. OOP prensipleriyle birleştiğinde ise kodunuzun kalitesi, okunabilirliği ve yeniden kullanılabilirliği katlanarak artar.

**Bu Kitabın Amacı:**

* **OOP Temellerini Sağlamlaştırmak:** Sınıflar, nesneler, özellikler, metotlar gibi temel kavramları net bir şekilde anlamanızı sağlamak.
* **Dört Ana Prensibi Derinlemesine İncelemek:** **Kapsülleme**, **Miras Alma**, **Çok Biçimlilik** ve **Soyutlama** prensiplerinin teorik altyapısını ve TypeScript'teki uygulamalarını detaylandırmak.
* **Pratik Uygulamalar Sunmak:** Her prensibi, gerçek dünya senaryolarını yansıtan zengin ve açıklayıcı TypeScript kod örnekleriyle pekiştirmek.
* **En İyi Uygulamaları Öğretmek:** SOLID prensipleri gibi ileri seviye konuları ele alarak, temiz ve sürdürülebilir kod yazma becerilerinizi geliştirmek.

**Hedef Kitlesi:**

* JavaScript bilen ve OOP prensiplerine giriş yapmak veya bilgilerini pekiştirmek isteyen geliştiriciler.
* TypeScript ile nesne yönelimli projeler geliştirmek isteyenler.
* Yazılım mimarisi hakkında bilgi edinmek isteyenler.
* Daha düzenli, modüler ve yönetilebilir kod yazma arayışında olan herkes.

Bu yolculuğun sonunda, TypeScript'in gücünü kullanarak karmaşık problemleri OOP yaklaşımıyla çözebilen, daha yetkin bir yazılımcı olacaksınız. Hazırsanız, nesnelerin büyüleyici dünyasına adım atalım!

***

#### 1.2 Nesne Yönelimli Programlama Nedir?

Nesne Yönelimli Programlama (OOP), yazılım tasarımında ve geliştirmesinde kullanılan bir **paradigma**, yani bir yaklaşımdır. Bu yaklaşım, gerçek dünyadaki varlıkları ve bunların arasındaki ilişkileri bilgisayar programlarına modellemeyi amaçlar. Geleneksel prosedürel programlamanın aksine, OOP, **veri** (özellikler) ve bu veriler üzerinde işlem yapan **davranışları** (metotlar) tek bir birimde, yani **nesnelerde** bir araya getirir.

Bir düşünün: Çevrenizdeki her şey birer nesnedir. Bir **araba**, bir **insan**, bir **telefon**, hatta bir **kitap**. Her nesnenin belirli **özellikleri** (verileri) ve yapabildiği **davranışları** (işlevleri) vardır.

* **Araba:**
  * **Özellikler:** Renk, marka, model, motor hacmi, hız...
  * **Davranışlar:** Hızlanmak, fren yapmak, dönmek, park etmek...
* **İnsan:**
  * **Özellikler:** Ad, soyad, yaş, boy, kilo...
  * **Davranışlar:** Konuşmak, yürümek, yemek yemek, uyumak...
* **Kitap:**
  * **Özellikler:** Başlık, yazar, sayfa sayısı, basım yılı, ISBN...
  * **Davranışlar:** Sayfa çevirmek (okuyucu tarafından), bilgiyi depolamak (kitabın rolü), kendini tanıtmak (dijital versiyon için).

OOP, bu gerçek dünya benzetmelerini yazılıma taşır. Her bir nesne, kendine özgü verilerini içerir ve bu verileri işlemek için tanımlanmış metotları kullanır.

#### 1.2.1 Temel Kavramlar

OOP'yi anlamak için bazı temel yapı taşlarını kavramak şarttır:

* **Nesne (Object):** Bir sınıfın somutlaşmış halidir. Bellekte yer kaplayan, belirli özelliklere (veri) ve metotlara (davranışlar) sahip bağımsız bir birimdir. Yukarıdaki araba, insan, kitap örnekleri birer nesnedir.
* **Sınıf (Class):** Nesneler için bir **şablon** veya **plan**dır. Nesnelerin hangi özelliklere sahip olacağını ve hangi davranışları sergileyeceğini tanımlar. Bir sınıf, doğrudan kullanılamaz; ondan **nesneler** türetilmelidir. Örneğin, "Car" sınıfı, belirli bir arabanın özelliklerini (marka, model vb.) ve davranışlarını (hızlanma, frenleme vb.) tanımlar. Bu sınıftan türetilen "Sahin", "BMW" veya "Mercedes" ise birer **nesnedir**.
* **Özellik (Property / Attribute / Field):** Bir nesnenin durumunu veya niteliğini tanımlayan verilerdir. Sınıf içerisinde değişkenler olarak tanımlanırlar. Örneğin, bir `Car` sınıfının `brand`, `model`, `color` gibi özellikleri olabilir.
* **Metot (Method / Behavior / Function):** Bir nesnenin yapabileceği eylemleri veya davranışları tanımlayan fonksiyonlardır. Sınıf içerisinde fonksiyonlar olarak tanımlanırlar. Örneğin, bir `Car` sınıfının `accelerate()`, `brake()` gibi metotları olabilir.
* **Örnek (Instance):** Bir sınıftan oluşturulan somut bir nesnedir. Bir sınıfı bir kurabiye kalıbına benzetirsek, her bir kurabiye o kalıptan çıkan bir "örnek"tir. `new` anahtar kelimesi ile bir sınıftan örnek oluşturulur.

**Örnek:**

```typescript
// Class Definition: Bir 'Book' nesnesinin nasıl olacağını tanımlayan şablon
class Book {
    // Properties (data): Kitabın başlığı, yazarı, sayfa sayısı ve yayıncısı
    title: string;
    author: string;
    pageCount: number;
    publisher: string;

    // Constructor Method: Yeni bir Book nesnesi oluşturulduğunda çalışır
    constructor(title: string, author: string, pageCount: number, publisher: string) {
        this.title = title;
        this.author = author;
        this.pageCount = pageCount;
        this.publisher = publisher;
    }

    // Method (behavior): Kitap bilgilerini konsola yazdıran metot
    displayBookInfo(): void {
        console.log(`Title: ${this.title}`);
        console.log(`Author: ${this.author}`);
        console.log(`Page Count: ${this.pageCount}`);
        console.log(`Publisher: ${this.publisher}`);
    }

    // Method (behavior): Belirtilen sayıda sayfa çevrildiğini bildiren metot
    turnPages(numberOfPages: number): void {
        console.log(`Turned ${numberOfPages} pages.`);
    }
}

// Object (instance) Creation: Book sınıfından somut nesneler yaratma
const hobbit = new Book("The Hobbit", "J.R.R. Tolkien", 310, "George Allen & Unwin");
const littlePrince = new Book("The Little Prince", "Antoine de Saint-Exupéry", 96, "Reynal & Hitchcock");

// Calling methods of the objects: Nesnelerin metotlarını çağırma
console.log("--- Hobbit Info ---");
hobbit.displayBookInfo();
hobbit.turnPages(15);

console.log("\n--- The Little Prince Info ---");
littlePrince.displayBookInfo();
```

***

#### 1.3 OOP Paradigmasının Tarihsel Gelişimi

OOP, yazılım geliştirme tarihinde önemli bir dönüm noktasıdır. Gelin, neden ortaya çıktığına ve ne gibi avantajlar sunduğuna kısaca göz atalım.

#### 1.3.1 Prosedürel Programlamadan OOP'ye Geçiş

Bilgisayar programcılığının ilk dönemlerinde **prosedürel (veya yapısal) programlama** yaygın olarak kullanılıyordu. Bu yaklaşımda, programlar bir dizi adım adım talimattan (fonksiyonlar veya prosedürler) oluşur. Veri, genellikle global değişkenler aracılığıyla fonksiyonlar arasında paylaşılırdı.

**Prosedürel Programlamanın Zorlukları:**

* **Büyük Kod Tabanları:** Program büyüdükçe, global verileri yönetmek zorlaşır, fonksiyonlar arası bağımlılıklar artar.
* **Bakım Zorluğu:** Bir veri yapısında yapılan değişiklik, o veriyi kullanan birçok fonksiyonu etkileyebilir. Bu da hatalara ve uzun debug süreçlerine yol açar.
* **Yeniden Kullanım Zorluğu:** Fonksiyonlar genellikle belirli bir veri yapısına sıkıca bağlı olduğu için, farklı projelerde veya farklı veri yapılarıyla tekrar kullanımları zordur.
* **Karmaşıklık Yönetimi:** Büyük sistemlerde, programın genel akışını ve veri değişimlerini takip etmek insan zihni için yorucu hale gelir.

Bu zorluklar, özellikle 1960'lı yıllardan itibaren yazılım sistemlerinin karmaşıklığının artmasıyla daha belirgin hale geldi. Bu sorunlara çözüm arayışları, **nesne yönelimli programlama** fikrinin doğmasına yol açtı. Simula programlama dili, ilk nesne yönelimli dil olarak kabul edilirken, Smalltalk bu paradigmayı popülerleştiren ilk dillerden biri olmuştur. Daha sonra C++, Java, C# ve modern JavaScript/TypeScript gibi dillerde OOP güçlü bir şekilde yer bulmuştur.

#### 1.3.2 OOP'nin Avantajları

OOP'nin prosedürel yaklaşıma göre sunduğu temel avantajlar şunlardır:

* **Yeniden Kullanılabilirlik (Reusability):** Sınıflar birer şablon olduğu için, bir kez yazılan bir sınıf, farklı yerlerde birçok nesne oluşturmak için tekrar tekrar kullanılabilir. Miras alma sayesinde, mevcut sınıfları genişleterek yeni sınıflar oluşturulabilir, bu da kod tekrarını büyük ölçüde azaltır.
* **Bakım Kolaylığı (Maintainability):** **Kapsülleme** sayesinde, bir nesnenin iç durumu dışarıdan doğrudan erişime kapalıdır. Bu, bir nesnenin iç işleyişinde yapılan bir değişikliğin, dışarıdaki kodları bozma olasılığını azaltır. Hata tespiti ve düzeltilmesi daha kolay hale gelir.
* **Ölçeklenebilirlik (Scalability):** OOP, sistemleri küçük, bağımsız ve etkileşimli nesnelere ayırmaya teşvik eder. Bu modüler yapı, büyük ve karmaşık projelerin daha kolay geliştirilmesini ve zamanla genişletilmesini sağlar. Yeni özellikler, mevcut kodu çok fazla değiştirmeden eklenebilir.
* **Gerçek Dünya Modellemesi:** Nesne yönelimli yaklaşım, gerçek dünyayı modellediği için, geliştiricilerin problem alanını daha sezgisel bir şekilde anlamasına ve tasarlamasına yardımcı olur.
* **Esneklik (Flexibility) ve Genişletilebilirlik (Extensibility):** **Çok biçimlilik (Polymorphism)** sayesinde, farklı nesneler aynı arayüz üzerinden farklı davranışlar sergileyebilir. Bu, sisteme yeni davranışlar veya türler eklemeyi çok daha kolay hale getirir.
* **Kapsülleme (Encapsulation):** Veri ve metotları tek bir birimde bir araya getirerek, verinin dışarıdan direkt erişime kapatılmasını sağlar. Bu, veri bütünlüğünü korur ve nesnenin iç durumunun kontrolsüzce değiştirilmesini engeller.
* **Daha Az Karmaşıklık (Reduced Complexity):** Büyük sistemler, küçük, yönetilebilir parçalara (nesnelere) bölündüğü için, her bir parçanın anlaşılması ve üzerinde çalışılması daha kolaydır.
* **Daha İyi Güvenlik:** Kapsülleme sayesinde, hassas veriler dış erişime kapatılarak daha güvenli bir yapı sağlanır.

#### 1.3.3 OOP'nin Dezavantajları

Her güçlü araç gibi, OOP'nin de bazı potansiyel dezavantajları vardır:

* **Öğrenme Eğrisi:** OOP prensipleri ve kavramları (özellikle başlangıçta) prosedürel programlamaya göre daha soyut ve karmaşık gelebilir. Yeni başlayanlar için kavramsal olarak daha zorlayıcı olabilir.
* **Performans Overhead'i:** Bazı durumlarda, OOP soyutlamaları ve nesne oluşturma maliyetleri, basit prosedürel yaklaşımlara göre hafif bir performans ek yüküne neden olabilir. Ancak modern donanım ve derleyicilerle bu genellikle ihmal edilebilir düzeydedir.
* **Aşırı Tasarım (Over-engineering) Riski:** Her şeyi bir nesneye dönüştürme veya gereksiz yere karmaşık hiyerarşiler oluşturma eğilimi olabilir. Bu, basit problemler için gereksiz soyutlamalara ve gereksiz karmaşıklığa yol açabilir. "YAGNI (You Ain't Gonna Need It)" ve "KISS (Keep It Simple, Stupid)" gibi prensipler bu riski azaltmaya yardımcı olur.

Genel olarak, OOP'nin sağladığı avantajlar, özellikle orta ve büyük ölçekli yazılım projelerinde dezavantajlarını fazlasıyla geride bırakır.

***

#### 1.4 TypeScript'e Hızlı Bir Bakış

Bu kitapta OOP prensiplerini TypeScript ile inceleyeceğimiz için, TypeScript'in temel özelliklerine hızlıca bir göz atmakta fayda var.

#### 1.4.1 TypeScript Neden Var? (JavaScript'in Eksiklikleri)

JavaScript, web'in dili olmasının yanı sıra Node.js ile backend ve mobil uygulamalarla geniş bir kullanım alanına sahiptir. Ancak büyüyen ve karmaşıklaşan JavaScript projelerinde bazı zorluklar ortaya çıkar:

* **Dinamik Tipler (Dynamic Typing):** JavaScript'te değişkenlerin tipleri çalışma zamanında belirlenir. Bu esneklik sağlasa da, büyük projelerde beklenmedik tip hatalarına yol açabilir ve hataları tespit etmeyi zorlaştırır. Örneğin, bir fonksiyona sayı beklerken string göndermek, ancak hatanın kodun çok sonraki bir noktasında ortaya çıkması gibi.
* **Büyük Ölçekli Projeler İçin Zorluk:** Kod tabanı büyüdükçe, farklı modüller arasında veri akışını takip etmek, değişkenlerin hangi tipleri alabileceğini hatırlamak karmaşıklaşır.
* **Refactoring Zorluğu:** Kodda bir değişiklik yapıldığında, bu değişikliğin diğer kısımları nasıl etkileyeceğini görmek zordur, çünkü derleyici tip güvenliği sağlamaz.
* **Geliştirme Ortamı Desteğinin Eksikliği:** Dinamik tipler nedeniyle, IDE'ler (Entegre Geliştirme Ortamları) kod tamamlama (IntelliSense), hata tespiti ve gezinme gibi konularda tam destek sağlayamaz.

İşte bu noktada **TypeScript** devreye girer. Microsoft tarafından geliştirilen TypeScript, JavaScript'in bir **üst kümesidir (superset)**. Yani, geçerli her JavaScript kodu aynı zamanda geçerli bir TypeScript kodudur. TypeScript'in en temel özelliği, JavaScript'e **statik tip sistemi** eklemesidir.

#### 1.4.2 Temel TypeScript Tipleri

TypeScript, değişkenlerin, fonksiyon parametrelerinin ve geri dönüş değerlerinin tiplerini önceden belirlemenize olanak tanır. Bu, kod yazarken olası tip hatalarının daha derleme aşamasında yakalanmasını sağlar.

*   **`string`:** Metin verileri için kullanılır.

    ```typescript
    let name: string = "Alice"; // Bir metin değeri atama
    ```



*   **`number`:** Sayısal veriler için kullanılır (hem tam sayılar hem de ondalık sayılar).

    ```typescript
    let age: number = 30; // Tam sayı atama
    let price: number = 29.99; // Ondalık sayı atama
    ```



*   **`boolean`:** `true` veya `false` değerlerini tutar.

    ```typescript
    let isActive: boolean = true; // Mantıksal değer atama
    ```



*   **`array`:** Bir tipin koleksiyonunu tutar.

    ```typescript
    let numbers: number[] = [1, 2, 3]; // Sayılardan oluşan bir dizi
    let names: Array<string> = ["John", "Jane"]; // Metinlerden oluşan bir dizi (alternatif söz dizimi)
    ```



*   **`tuple`:** Belirli sayıda elemanı olan, elemanlarının tipleri ve sıralaması bilinen dizilerdir.

    ```typescript
    let coordinate: [number, number] = [10, 20]; // İki sayıdan oluşan bir tuple
    ```



*   **`enum`:** Bir dizi isimlendirilmiş sabiti tanımlamak için kullanılır.

    ```typescript
    enum Difficulty {
        Low,    // Düşük
        Medium, // Orta
        High    // Yüksek
    }
    let gameDifficulty: Difficulty = Difficulty.Medium; // Enum değerini atama
    ```



*   **`any`:** Herhangi bir tipe sahip olabileceğini belirtir. Tip güvenliğini devre dışı bırakır, bu nedenle dikkatli kullanılmalıdır.

    ```typescript
    let value: any = "hello"; // Başlangıçta string
    value = 123; // Sonra number olabilir
    ```



*   **`unknown`:** `any`'ye benzer ama daha tip güvenlidir. Değeri kullanmadan önce tip kontrolü yapılmasını zorunlu kılar.

    ```typescript
    let unknownValue: unknown = 4; // unknown tipinde bir değişken
    // let myName: string = unknownValue; // Hata: unknown tipi doğrudan başka bir tipe atanamaz
    if (typeof unknownValue === 'string') {
        let myName: string = unknownValue; // Geçerli: Tip kontrolü yapıldıktan sonra string olarak kullanılabilir
    }
    ```



*   **`void`:** Bir fonksiyonun değer döndürmediğini belirtir.

    ```typescript
    function logMessage(): void {
        console.log("A message was logged."); // Geri dönüş değeri yok
    }
    ```



*   **`null` ve `undefined`:** JavaScript'teki karşılıkları gibi, ancak tipler olarak da kullanılabilirler.

    ```typescript
    let data: string | null = null; // String veya null olabilir (Union Type)
    ```



*   **`object`:** İlkel olmayan her türlü tip için kullanılır (objeler, diziler, fonksiyonlar).

    ```typescript
    let student: object = { name: "David", age: 22 }; // Bir nesne atama
    ```

#### 1.4.3 TypeScript Derleyicisi (TSC) ve Çalışma Ortamı

TypeScript kodu, doğrudan web tarayıcıları veya Node.js gibi JavaScript çalışma ortamları tarafından anlaşılamaz. Bu yüzden, yazdığınız TypeScript kodunu **JavaScript'e derlemeniz** gerekir. Bu işlemi **TypeScript Derleyicisi (TSC)** yapar.

**Kurulum:**

Node.js ve npm (Node Package Manager) yüklü olduktan sonra, terminalden şu komutu çalıştırarak TypeScript'i global olarak kurabilirsiniz:

```bash
npm install -g typescript
```

**Derleme:**

Bir `.ts` uzantılı TypeScript dosyası oluşturun (örneğin `app.ts`).

TypeScript

```typescript
// app.ts
// İsim alan bir fonksiyon tanımla ve bir selam mesajı döndür
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Bir kullanıcı adı tanımla
let userName: string = "Alice";
// Fonksiyonu çağır ve sonucu konsola yazdır
console.log(greet(userName));
```

Terminalde, bu dosyayı derlemek için `tsc` komutunu kullanın:

```bash
tsc app.ts
```

Bu komut, `app.ts` dosyasını derleyerek aynı dizine `app.js` adında bir JavaScript dosyası oluşturacaktır:

```javascript
// app.js (derlenmiş çıktı)
// İsim alan bir fonksiyon tanımla ve bir selam mesajı döndür
function greet(name) {
    return "Hello, " + name + "!";
}
// Bir kullanıcı adı tanımla
var userName = "Alice";
// Fonksiyonu çağır ve sonucu konsola yazdır
console.log(greet(userName));
```

Artık `app.js` dosyasını Node.js ile çalıştırabilir veya bir web sayfasında kullanabilirsiniz:

```bash
node app.js
```

**Çıktı:**

```
Hello, Alice!
```

Modern geliştirme ortamlarında, bu derleme süreci genellikle bir yapılandırma dosyası (`tsconfig.json`) ve otomasyon araçları (Webpack, Rollup, Vite gibi) aracılığıyla otomatikleştirilir.

Bu hızlı bakış, TypeScript'in temel felsefesini ve çalışma şeklini anlamanıza yardımcı oldu. Şimdi, bu güçlü dilin OOP prensipleriyle nasıl birleştiğini detaylıca incelemeye hazırız!
