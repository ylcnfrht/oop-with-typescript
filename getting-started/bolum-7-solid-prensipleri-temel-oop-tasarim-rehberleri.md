# Bölüm 7: SOLID Prensipleri - Temel OOP Tasarım Rehberleri

***

Nesne Yönelimli Programlama'nın (OOP) dört ana prensibi (Kapsülleme, Miras Alma, Çok Biçimlilik, Soyutlama) bir yazılımın temel yapı taşlarını oluşturur. Ancak bu yapı taşlarını nasıl bir araya getireceğimiz, kodumuzun kalitesini, sürdürülebilirliğini ve esnekliğini doğrudan etkiler. İşte bu noktada **SOLID Prensipleri** devreye girer.

SOLID, yazılım mühendisi ve yazar Robert C. Martin (genellikle "Uncle Bob" olarak bilinir) tarafından ortaya konulan ve nesne yönelimli tasarımda önemli rol oynayan beş temel prensibin baş harflerinden oluşan bir akronimdir. Bu prensipler, yazılım bileşenlerini daha anlaşılır, esnek ve bakımı kolay hale getirmeyi amaçlar.

**SOLID Nedir?**

* **S** - **Single Responsibility Principle (Tek Sorumluluk Prensibi)**
* **O** - **Open/Closed Principle (Açık/Kapalı Prensibi)**
* **L** - **Liskov Substitution Principle (Liskov Yerine Geçme Prensibi)**
* **I** - **Interface Segregation Principle (Arayüz Ayırma Prensibi)**
* **D** - **Dependency Inversion Principle (Bağımlılık Tersine Çevirme Prensibi)**

Bu prensipler, bir yazılım projesinde sınıfların, modüllerin ve arayüzlerin nasıl tasarlanması gerektiği konusunda bize rehberlik eder.

***

#### 7.1 S - Single Responsibility Principle (Tek Sorumluluk Prensibi - SRP)

**Tek Sorumluluk Prensibi (SRP)**, bir sınıfın veya modülün yalnızca **bir (tek)** görevi veya **bir (tek) değişim nedeni** olması gerektiğini belirtir. Basitçe söylemek gerekirse, bir sınıfın birden fazla nedenle değişmeye eğilimi olmamalıdır. Eğer bir sınıfı değiştirmek için birden fazla sebebiniz varsa, o sınıfın birden fazla sorumluluğu var demektir ve bu sorumluluklar ayrılmalıdır.

**Neden Önemlidir?**

* **Daha Az Bağlılık (Loose Coupling):** Bir sınıfın tek bir sorumluluğu olduğunda, başka sınıflardaki değişikliklerden daha az etkilenir. Bu, sistemin genel bağlılığını azaltır.
* **Daha Kolay Bakım:** Bir hata ortaya çıktığında veya bir gereksinim değiştiğinde, hangi sınıfın etkileneceğini ve nerede değişiklik yapılması gerektiğini belirlemek daha kolay olur.
* **Daha Anlaşılır Kod:** Her sınıfın ne iş yaptığını anlamak daha kolaydır, çünkü belirli bir göreve odaklanmıştır.
* **Daha İyi Test Edilebilirlik:** Tek bir sorumluluğu olan sınıfları test etmek daha basittir, çünkü yalnızca o tek sorumluluğun davranışını doğrulamak gerekir.

**Örnek: SRP'ye Aykırı Tasarım**

Bir `User` (Kullanıcı) sınıfının hem kullanıcı bilgilerini yönettiğini hem de kullanıcılara e-posta gönderdiğini varsayalım.

```typescript
// SRP'ye Aykırı Tasarım
class User {
    id: number;
    name: string;
    email: string;

    constructor(id: number, name: string, email: string) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    // Sorumluluk 1: Kullanıcı bilgilerini yönetme
    updateName(newName: string): void {
        this.name = newName;
        console.log(`User ${this.id} name updated to ${newName}.`);
    }

    // Sorumluluk 2: E-posta gönderme
    sendEmail(subject: string, body: string): void {
        console.log(`Sending email to ${this.email} with subject: ${subject}`);
        console.log(`Body: ${body}`);
        // Gerçekte burada SMTP sunucusuyla iletişim kurma kodu olurdu
    }

    // Sorumluluk 3: Veritabanı işlemleri (gerçekte daha karmaşık olurdu)
    saveToDatabase(): void {
        console.log(`Saving user ${this.name} to database.`);
        // Gerçekte burada veritabanı CRUD işlemleri olurdu
    }
}

const user = new User(1, "Alice", "alice@example.com");
user.updateName("Alicia");
user.sendEmail("Welcome!", "Welcome to our platform!");
user.saveToDatabase();
```

Yukarıdaki `User` sınıfının birden fazla değişim nedeni vardır:

* Kullanıcı bilgilerinin yapısı değiştiğinde (`id`, `name`, `email`).
* E-posta gönderme mantığı değiştiğinde (örneğin, farklı bir e-posta servisi kullanıldığında).
* Veritabanı kayıt mantığı değiştiğinde (örneğin, farklı bir ORM veya veritabanı kullanıldığında).

Bu durum, `User` sınıfını gereksiz yere büyük ve karmaşık hale getirir, ayrıca bir değişiklik başka bir sorumluluğu etkileyebilir.

**SRP'ye Uygun Tasarım**

Her sorumluluğu ayrı bir sınıfa bölerek SRP'yi uygulayabiliriz.

```typescript
// SRP'ye Uygun Tasarım

// Sorumluluk 1: Kullanıcı bilgilerini yönetme
class UserData {
    id: number;
    name: string;
    email: string;

    constructor(id: number, name: string, email: string) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    updateName(newName: string): void {
        this.name = newName;
        console.log(`User ${this.id} name updated to ${newName}.`);
    }

    // Getter'lar
    getEmail(): string {
        return this.email;
    }
    getName(): string {
        return this.name;
    }
    getId(): number {
        return this.id;
    }
}

// Sorumluluk 2: E-posta gönderme
class EmailService {
    sendEmail(to: string, subject: string, body: string): void {
        console.log(`Sending email to ${to} with subject: "${subject}"`);
        console.log(`Body: ${body}`);
        console.log(`Email sent successfully.`);
    }
}

// Sorumluluk 3: Kullanıcı veritabanı işlemleri
class UserRepository {
    save(userData: UserData): void {
        console.log(`Saving user ${userData.getName()} (ID: ${userData.getId()}) to database.`);
        // Gerçek veritabanı kaydetme mantığı
    }

    load(id: number): UserData | null {
        console.log(`Loading user with ID: ${id} from database.`);
        // Gerçek veritabanından yükleme mantığı
        // Basitlik için varsayılan bir UserData döndürelim
        if (id === 1) {
            return new UserData(id, "Alice Loaded", "alice.loaded@example.com");
        }
        return null;
    }
}

const userProfile = new UserData(1, "Bob", "bob@example.com");
userProfile.updateName("Robert");

const emailSender = new EmailService();
emailSender.sendEmail(userProfile.getEmail(), "Account Created", "Welcome, " + userProfile.getName() + "!");

const userRepo = new UserRepository();
userRepo.save(userProfile);
const loadedUser = userRepo.load(1);
if (loadedUser) {
    console.log(`Loaded user: ${loadedUser.getName()}`);
}
```

Bu yeni tasarımda:

* `UserData` sınıfı yalnızca kullanıcı bilgilerini yönetmekten sorumludur.
* `EmailService` sınıfı yalnızca e-posta gönderme sorumluluğunu üstlenir.
* `UserRepository` sınıfı ise kullanıcı verilerini depolama ve yükleme sorumluluğuna sahiptir.

Her sınıfın tek bir değişim nedeni vardır. Örneğin, e-posta gönderme mantığı değiştiğinde, sadece `EmailService` sınıfını değiştiririz ve bu değişiklik `UserData` veya `UserRepository` sınıflarını etkilemez. Bu, kod tabanını çok daha sağlam ve yönetilebilir hale getirir.

SRP, temiz kod ve sağlam mimariler için atılan ilk ve en temel adımdır.

***

#### 7.2 O - Open/Closed Principle (Açık/Kapalı Prensibi - OCP)

**Açık/Kapalı Prensibi (OCP)**, yazılım varlıklarının (sınıflar, modüller, fonksiyonlar vb.) **genişletmeye açık, ancak değiştirmeye kapalı** olması gerektiğini belirtir.

Bu ne anlama geliyor?

* **Genişletmeye Açık (Open for Extension):** Yeni bir özellik veya davranış eklemek istediğinizde, mevcut kod tabanına yeni kod ekleyebilmelisiniz.
* **Değiştirmeye Kapalı (Closed for Modification):** Yeni bir özellik veya davranış eklerken, mevcut ve çalışan kodu değiştirmek zorunda kalmamalısınız.

**Neden Önemlidir?**

* **Kararlılık ve Güvenilirlik:** Mevcut çalışan kodu değiştirmek zorunda kalmadığınızda, yeni özelliklerin mevcut işlevselliği bozma riski azalır.
* **Daha Az Hata:** Azalan değişiklik, azalan hata demektir.
* **Daha Kolay Bakım:** Değişiklikler yeni kod eklenerek yapıldığından, bakım ve hata ayıklama süreçleri daha basittir.
* **Esneklik:** Sisteminizi daha esnek hale getirir, çünkü farklı davranışları mevcut kodu kırmadan kolayca ekleyebilirsiniz.

OCP genellikle **polimorfizm** (çok biçimlilik) ve **soyutlama** (arayüzler veya soyut sınıflar) kullanılarak uygulanır. Ortak bir arayüz veya soyut temel sınıf tanımlanır ve farklı implementasyonlar bu arayüzü/temel sınıfı genişleterek eklenir.

**Örnek: OCP'ye Aykırı Tasarım**

Farklı şekillerin alanını hesaplayan bir `AreaCalculator` sınıfı olduğunu ve yeni bir şekil eklendiğinde bu sınıfın sürekli değiştiğini varsayalım.

```typescript
// OCP'ye Aykırı Tasarım
class Rectangle {
    width: number;
    height: number;
    constructor(width: number, height: number) {
        this.width = width;
        this.height = height;
    }
}

class Circle {
    radius: number;
    constructor(radius: number) {
        this.radius = radius;
    }
}

class AreaCalculator {
    calculateArea(shape: any): number { // 'any' kullanımı da kötü bir işaret
        if (shape instanceof Rectangle) {
            return shape.width * shape.height;
        } else if (shape instanceof Circle) {
            return Math.PI * shape.radius * shape.radius;
        }
        // PROBLEM: Yeni bir şekil (örneğin Triangle) eklendiğinde,
        // buraya yeni bir 'else if' bloğu eklememiz gerekecek.
        // Bu, mevcut çalışan 'calculateArea' metodunu değiştirme demektir.
        throw new Error("Unsupported shape type.");
    }
}

const rect = new Rectangle(5, 10);
const circle = new Circle(7);
const calculator = new AreaCalculator();

console.log(`Rectangle Area: ${calculator.calculateArea(rect)}`);
console.log(`Circle Area: ${calculator.calculateArea(circle)}`);
```

Yukarıdaki `AreaCalculator` sınıfı, `calculateArea` metodunda OCP'ye aykırıdır. Yeni bir şekil (örneğin `Triangle`) eklediğimizde, `calculateArea` metodunun içine yeni bir `else if` bloğu eklememiz gerekir. Bu, mevcut kodu değiştirme (modify) anlamına gelir ve prensibi ihlal eder.

**OCP'ye Uygun Tasarım**

OCP'yi uygulamak için soyutlama ve polimorfizm kullanabiliriz. Ortak bir `Shape` arayüzü/soyut sınıfı tanımlayıp, her şeklin kendi alanını hesaplamasını sağlayabiliriz.

```typescript
// OCP'ye Uygun Tasarım

// Adım 1: Soyut bir arayüz veya soyut sınıf tanımla
interface IShape {
    calculateArea(): number;
}

// Adım 2: Her somut şekil bu arayüzü uygulasın
class OCPRectangle implements IShape {
    width: number;
    height: number;
    constructor(width: number, height: number) {
        this.width = width;
        this.height = height;
    }
    calculateArea(): number {
        return this.width * this.height;
    }
}

class OCPCircle implements IShape {
    radius: number;
    constructor(radius: number) {
        this.radius = radius;
    }
    calculateArea(): number {
        return Math.PI * this.radius * this.radius;
    }
}

// Adım 3: Yeni bir şekil eklemek istediğimizde, sadece bu arayüzü uygulayan yeni bir sınıf ekleriz.
// Mevcut 'AreaCalculator' sınıfını değiştirmemize gerek kalmaz.
class OCPTriangle implements IShape {
    base: number;
    height: number;
    constructor(base: number, height: number) {
        this.base = base;
        this.height = height;
    }
    calculateArea(): number {
        return 0.5 * this.base * this.height;
    }
}

// Adım 4: Alan hesaplama mantığı, artık her şeklin kendi içinde.
// AreaCalculator sadece IShape tipindeki nesneleri kabul eder ve calculateArea'yı çağırır.
class OCPAreaCalculator {
    // calculateArea metodu artık değiştirmeye kapalı
    // Sadece IShape tipindeki herhangi bir nesneyi alıp calculateArea() metodunu çağırır.
    calculateAreas(shapes: IShape[]): number[] {
        return shapes.map(shape => shape.calculateArea());
    }
}

const ocpRect = new OCPRectangle(5, 10);
const ocpCircle = new OCPCircle(7);
const ocpTriangle = new OCPTriangle(4, 6); // Yeni şekil

const ocpCalculator = new OCPAreaCalculator();

const shapes: IShape[] = [ocpRect, ocpCircle, ocpTriangle];
const areas = ocpCalculator.calculateAreas(shapes);

console.log(`Calculated Areas: ${areas}`); // Çıktı: Calculated Areas: 50,153.93804002589,12

```

Bu yeni tasarımda:

* `IShape` arayüzü, tüm şekiller için bir sözleşme tanımlar: hepsinin bir `calculateArea()` metodu olmalıdır.
* Her somut şekil sınıfı (`OCPRectangle`, `OCPCircle`, `OCPTriangle`), bu arayüzü uygular ve kendi `calculateArea()` mantığını sağlar.
* `OCPAreaCalculator` sınıfındaki `calculateAreas` metodu, `IShape` tipindeki bir diziyi kabul eder ve her eleman için `calculateArea()` metodunu çağırır.

Şimdi yeni bir şekil (`OCPSquare` gibi) eklemek istediğimizde, sadece `IShape` arayüzünü uygulayan yeni bir `OCPSquare` sınıfı oluştururuz. `OCPAreaCalculator` sınıfındaki mevcut `calculateAreas` metodunu **değiştirmemize gerek kalmaz**. Bu, prensibi doğru bir şekilde uygular. `OCPAreaCalculator` genişlemeye açıktır (yeni şekiller ekleyebiliriz), ancak değiştirmeye kapalıdır (mevcut kodunu dokunmamıza gerek kalmaz).

OCP, sisteminizi daha esnek, sürdürülebilir ve hataya daha az yatkın hale getirir.

***

#### 7.3 L - Liskov Substitution Principle (Liskov Yerine Geçme Prensibi - LSP)

**Liskov Yerine Geçme Prensibi (LSP)**, temel sınıfların (parent class) örneklerinin, türetilmiş sınıfların (child class) örnekleriyle değiştirilebilir olması gerektiğini belirtir. Yani, bir programda bir temel sınıf tipi beklenen her yerde, o temel sınıftan türemiş herhangi bir alt sınıfın nesnesi de sorunsuz bir şekilde kullanılabilmelidir, programın doğruluğunu bozmadan.

Bu prensip, miras alma (Inheritance) ile çok yakından ilişkilidir ve miras alma hiyerarşilerinin doğru bir şekilde tasarlandığından emin olmamızı sağlar.

**Neden Önemlidir?**

* **Doğru Hiyerarşi Tasarımı:** Sınıf hiyerarşilerinin mantıksal olarak doğru ve sağlam olmasını sağlar.
* **Kod Güvenilirliği:** Bir temel sınıf referansı üzerinden işlem yapıldığında, hangi alt sınıf nesnesinin kullanıldığına bakılmaksızın beklenen davranışın elde edilmesini garanti eder. Bu, beklenmedik hataları önler.
* **Esneklik ve Genişletilebilirlik:** Yeni alt sınıflar eklendiğinde, mevcut kodun bunları sorunsuz bir şekilde kullanmaya devam etmesini sağlar. Polimorfizmin tam olarak çalışabilmesi için LSP'ye uyulması gerekir.

**LSP İhlali Nasıl Olur?**

LSP ihlali genellikle, bir alt sınıfın temel sınıfının davranış sözleşmesini bozması, yani temel sınıfın yaptığı bir şeyi yapmaması veya beklenenden farklı, istisnai bir şekilde yapması durumunda ortaya çıkar.

**Örnek: LSP'ye Aykırı Tasarım**

Bir `Bird` (Kuş) sınıfımız ve ondan türeyen bir `Penguin` (Penguen) sınıfımız olduğunu varsayalım. Kuşların genellikle uçabildiği düşünülür, ancak penguenler uçamaz.

```typescript
// LSP'ye Aykırı Tasarım
class Bird {
    name: string;
    constructor(name: string) {
        this.name = name;
    }

    fly(): void {
        console.log(`${this.name} is flying.`);
    }

    eat(): void {
        console.log(`${this.name} is eating.`);
    }
}

class Penguin extends Bird {
    constructor(name: string) {
        super(name);
    }

    // PROBLEM: Penguenler uçamaz! Bu, temel sınıfın "fly" davranış sözleşmesini bozar.
    fly(): void {
        throw new Error("Penguins cannot fly!");
    }

    swim(): void {
        console.log(`${this.name} is swimming.`);
    }
}

function makeBirdFly(bird: Bird): void {
    bird.fly();
}

const sparrow = new Bird("Sparrow");
makeBirdFly(sparrow); // Çıktı: Sparrow is flying.

const pingu = new Penguin("Pingu");
try {
    makeBirdFly(pingu); // Hata: Penguins cannot fly!
} catch (error: any) {
    console.error(`Error: ${error.message}`); // Beklenmedik bir hata!
}
```

Yukarıdaki örnekte `makeBirdFly` fonksiyonu `Bird` tipinde bir argüman bekler. `Sparrow` (serçe) ile sorunsuz çalışırken, `Penguin` ile bir hata fırlatır. Bu, `Penguin`'in `Bird`'ün yerine geçemediği anlamına gelir; yani LSP ihlal edilmiştir. Temel sınıfın bir metodu çağrıldığında, alt sınıfın beklenmedik bir şekilde davranması (bu durumda hata fırlatması) LSP ihlalidir.

**LSP'ye Uygun Tasarım**

LSP'ye uymak için, miras alma hiyerarşisini yeniden düşünmeli ve ortak davranışları daha soyut bir seviyeye taşımalıyız. "Uçabilme" özelliği tüm kuşlara ait olmayabilir.

```typescript
// LSP'ye Uygun Tasarım

// Daha genel bir temel sınıf: Animal (Hayvan)
class AnimalLSP {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    eat(): void {
        console.log(`${this.name} is eating.`);
    }
}

// Uçabilen hayvanlar için bir arayüz veya soyut sınıf
// Bu arayüz sadece uçabilme yeteneği olanlar için bir sözleşmedir
interface IFlyable {
    fly(): void;
}

// Bird (Kuş) sınıfı, genel bir kuşu temsil eder.
// Artık fly() metodu Bird sınıfında zorunlu değil.
class BirdLSP extends AnimalLSP {
    constructor(name: string) {
        super(name);
    }
    // Kuşlara özgü diğer davranışlar buraya gelebilir.
}

// FlyingBird (Uçan Kuş) sınıfı, hem BirdLSP'den miras alır hem de IFlyable arayüzünü uygular
class FlyingBird extends BirdLSP implements IFlyable {
    constructor(name: string) {
        super(name);
    }
    fly(): void {
        console.log(`${this.name} is flying high!`);
    }
}

// Penguin (Penguen) sınıfı sadece BirdLSP'den miras alır, IFlyable'ı uygulamaz
class PenguinLSP extends BirdLSP {
    constructor(name: string) {
        super(name);
    }
    swim(): void {
        console.log(`${this.name} is swimming expertly.`);
    }
}

// Fonksiyon artık IFlyable bekliyor, böylece sadece uçabilen nesneleri alıyor
function makeItFly(flyer: IFlyable): void {
    flyer.fly();
}

const sparrowLSP = new FlyingBird("Sparrow");
makeItFly(sparrowLSP); // Çıktı: Sparrow is flying high!

const pinguLSP = new PenguinLSP("Pingu");
// makeItFly(pinguLSP); // Hata: Argument of type 'PenguinLSP' is not assignable to parameter of type 'IFlyable'.
pinguLSP.swim(); // Penguin'e özgü metot

const genericBird = new BirdLSP("Generic Bird");
// genericBird.fly(); // Hata: Property 'fly' does not exist on type 'BirdLSP'. (Doğru, çünkü tüm kuşlar uçmaz)
```

Bu yeni tasarımda:

* `AnimalLSP` daha genel bir temel sınıftır.
* `IFlyable` arayüzü, yalnızca uçabilen nesneler için bir sözleşme tanımlar.
* `BirdLSP` sınıfı, tüm kuşlar için ortak özellikleri taşır ancak `fly()` metodunu zorunlu kılmaz.
* `FlyingBird` sınıfı `BirdLSP`'den miras alır ve `IFlyable`'ı uygular, bu sayede `fly()` metodunu implemente eder.
* `PenguinLSP` sınıfı sadece `BirdLSP`'den miras alır, `IFlyable`'ı uygulamaz ve bu nedenle `fly()` metoduna sahip değildir. Bunun yerine kendi `swim()` metoduna sahiptir.
* `makeItFly` fonksiyonu artık `IFlyable` tipinde bir argüman bekler. Bu, derleme zamanında sadece `IFlyable` arayüzünü uygulayan nesnelerin bu fonksiyona geçirilebileceğini garanti eder.

Bu tasarım LSP'ye uygundur çünkü:

* `FlyingBird` bir `BirdLSP`'dir ve `IFlyable`'dır, dolayısıyla `makeItFly`'a geçirilebilir.
* `PenguinLSP` bir `BirdLSP`'dir, ancak bir `IFlyable` değildir. Bu nedenle `makeItFly`'a geçirilemez ve derleme zamanında hata verir, çalışma zamanında beklenmedik bir hata fırlatmaz.

LSP, miras alma ilişkilerinin sağlamlığını ve anlamlılığını güvence altına alır.

***

#### 7.4 I - Interface Segregation Principle (Arayüz Ayırma Prensibi - ISP)

**Arayüz Ayırma Prensibi (ISP)**, müşterilerin (client), kullanmadıkları metotlara sahip arayüzlere bağımlı olmaya zorlanmamaları gerektiğini belirtir. Başka bir deyişle, büyük ve kapsamlı arayüzler yerine, daha küçük, daha özel (amaca özel) ve istemciye özgü arayüzler tasarlanmalıdır.

**Neden Önemlidir?**

* **Gereksiz Bağımlılıkları Azaltma:** Bir sınıfın, aslında ihtiyacı olmayan metotları implemente etmek zorunda kalmasını önler. Bu, bir sınıfın sorumluluğunu gereksiz yere artırır ve SRP'yi ihlal edebilir.
* **Daha Az Bağlılık:** Modüller arasındaki bağımlılıkları azaltır. Bir arayüzdeki bir değişiklik, sadece o arayüzün ilgili metotlarını kullanan sınıfları etkiler. Büyük bir arayüzdeki değişiklik, o arayüzü uygulayan birçok sınıfı gereksiz yere etkileyebilir.
* **Daha Esnek Tasarım:** Farklı müşterilerin farklı ihtiyaçları olduğunda, onlara özel küçük arayüzler sunarak sistemi daha esnek hale getirir.
* **Daha Temiz Kod:** Sınıflar sadece gerçekten implemente etmeleri gereken metotları içerir, bu da kodun daha okunaklı ve yönetilebilir olmasını sağlar.

**Örnek: ISP'ye Aykırı Tasarım**

Bir çok işlevli bir yazıcı (çok fonksiyonlu cihaz) için tek bir devasa arayüz olduğunu varsayalım.

```typescript
// ISP'ye Aykırı Tasarım
interface IMultiFunctionPrinter {
    print(document: string): void;
    scan(document: string): void;
    fax(document: string): void;
    copy(document: string): void;
}

// Eski Model Yazıcı: Sadece yazdırabilir ve kopyalayabilir
class OldFashionedPrinter implements IMultiFunctionPrinter {
    print(document: string): void {
        console.log(`Old printer printing: ${document}`);
    }
    copy(document: string): void {
        console.log(`Old printer copying: ${document}`);
    }
    // PROBLEM: Bu yazıcı faks çekemez veya tarayamaz, ancak yine de bu metotları implemente etmek ZORUNDA KALIR.
    // Bu, ISP ihlalidir.
    scan(document: string): void {
        throw new Error("OldFashionedPrinter cannot scan.");
    }
    fax(document: string): void {
        throw new Error("OldFashionedPrinter cannot fax.");
    }
}

// Modern Çok Fonksiyonlu Yazıcı: Her şeyi yapabilir
class ModernMultiFunctionPrinter implements IMultiFunctionPrinter {
    print(document: string): void {
        console.log(`Modern printer printing: ${document}`);
    }
    scan(document: string): void {
        console.log(`Modern printer scanning: ${document}`);
    }
    fax(document: string): void {
        console.log(`Modern printer faxing: ${document}`);
    }
    copy(document: string): void {
        console.log(`Modern printer copying: ${document}`);
    }
}

const oldPrinter = new OldFashionedPrinter();
oldPrinter.print("MyDoc.pdf");
oldPrinter.copy("MyPhoto.jpg");
// oldPrinter.scan("MyID.png"); // Çalışma zamanında hata fırlatır! Bu istenmeyen bir durumdur.
```

`OldFashionedPrinter` sınıfı, `IMultiFunctionPrinter` arayüzünü implemente ettiğinde, aslında yapamadığı `scan()` ve `fax()` metotlarını da içermek zorunda kalır. Bu durum, prensibi ihlal eder ve kodda `throw new Error` gibi "yapılamaz" metotların oluşmasına neden olur.

**ISP'ye Uygun Tasarım**

Arayüzleri daha küçük, spesifik arayüzlere bölerek ISP'yi uygulayabiliriz.

```typescript
// ISP'ye Uygun Tasarım

interface IPrintable {
    print(document: string): void;
}

interface IScanable {
    scan(document: string): void;
}

interface IFaxable {
    fax(document: string): void;
}

interface ICopyable {
    copy(document: string): void;
}

// Eski Model Yazıcı: Sadece ihtiyacı olan arayüzleri uygular
class OldFashionedPrinterISP implements IPrintable, ICopyable {
    print(document: string): void {
        console.log(`Old printer printing: ${document}`);
    }
    copy(document: string): void {
        console.log(`Old printer copying: ${document}`);
    }
    // Artık scan() ve fax() metotlarını implemente etmek zorunda değiliz!
}

// Modern Çok Fonksiyonlu Yazıcı: Tüm arayüzleri uygulayabilir
class ModernMultiFunctionPrinterISP implements IPrintable, IScanable, IFaxable, ICopyable {
    print(document: string): void {
        console.log(`Modern printer printing: ${document}`);
    }
    scan(document: string): void {
        console.log(`Modern printer scanning: ${document}`);
    }
    fax(document: string): void {
        console.log(`Modern printer faxing: ${document}`);
    }
    copy(document: string): void {
        console.log(`Modern printer copying: ${document}`);
    }
}

// Müşteri kodları artık sadece ihtiyacı olan arayüz tipini bekler

function printDocument(printer: IPrintable, doc: string): void {
    printer.print(doc);
}

function scanDocument(scanner: IScanable, doc: string): void {
    scanner.scan(doc);
}

const oldPrinterISP = new OldFashionedPrinterISP();
printDocument(oldPrinterISP, "Report.pdf"); // Çalışır
// scanDocument(oldPrinterISP, "Photo.jpg"); // Hata: 'OldFashionedPrinterISP' tipi 'IScanable' olarak atanamaz. (Derleme zamanı hatası, çalışma zamanı değil!)

const modernPrinterISP = new ModernMultiFunctionPrinterISP();
printDocument(modernPrinterISP, "Resume.pdf");
scanDocument(modernPrinterISP, "Passport.png");
```

Bu yeni tasarımda:

* Büyük `IMultiFunctionPrinter` arayüzü, daha küçük ve amaca özel `IPrintable`, `IScanable`, `IFaxable`, `ICopyable` arayüzlerine ayrılmıştır.
* `OldFashionedPrinterISP` sınıfı, yalnızca gerçekten yapabildiği işlevleri (print ve copy) temsil eden arayüzleri uygular. Bu, gereksiz implementasyonlardan ve `Error` fırlatan metotlardan kurtulmasını sağlar.
* Müşteri kodları (örneğin `printDocument` fonksiyonu), artık sadece ihtiyacı olan küçük arayüze bağımlıdır.

ISP, sistemde gereksiz bağlılıkları azaltır, esnekliği artırır ve kodun daha temiz ve anlaşılır olmasını sağlar.

***

#### 7.5 D - Dependency Inversion Principle (Bağımlılık Tersine Çevirme Prensibi - DIP)

**Bağımlılık Tersine Çevirme Prensibi (DIP)**, SOLID prensiplerinin belki de en kafa karıştırıcı ancak en güçlü olanıdır. Bu prensip iki ana bölümden oluşur:

1. **Üst seviye modüller, alt seviye modüllere bağlı olmamalıdır. Her ikisi de soyutlamalara (arayüzlere veya soyut sınıflara) bağlı olmalıdır.**
2. **Soyutlamalar detaylara bağlı olmamalıdır. Detaylar soyutlamalara bağlı olmalıdır.**

Basitçe söylemek gerekirse: **Somut (concrete) implementasyonlara değil, soyutlamalara (arayüzlere) bağımlı olun.**

**Neden Önemlidir?**

* **Gevşek Bağlılık (Loose Coupling):** Sisteminizdeki bileşenler arasındaki doğrudan bağımlılıkları ortadan kaldırır. Üst seviye modüller, alt seviye implementasyonların detaylarını bilmek zorunda kalmaz.
* **Esneklik ve Genişletilebilirlik:** Alt seviye modülleri değiştirmek veya yeni implementasyonlar eklemek, üst seviye modülleri etkilemez. Bu, sistemin daha kolay genişletilmesini sağlar.
* **Test Edilebilirlik:** Modüller arasındaki bağımlılıklar soyutlamalar üzerinden kurulduğundan, bir modülü test ederken bağımlı olduğu somut implementasyonlar yerine kolayca "mock" veya "stub" nesneleri kullanabilirsiniz. Bu, birim testlerini çok daha basit hale getirir.
* **Yeniden Kullanılabilirlik:** Soyutlamalara dayalı kod, farklı ortamlarda veya farklı somut implementasyonlarla daha kolay yeniden kullanılabilir.

**"Bağımlılığın Tersine Çevrilmesi" Ne Anlama Gelir?**

Geleneksel olarak, üst seviye modüllerin (iş mantığı içeren) alt seviye modüllere (veri depolama, dış servisler vb.) doğrudan bağlı olması yaygındır. DIP, bu bağımlılığın yönünü tersine çevirir. Üst seviye modül doğrudan alt seviye somut sınıfı çağırmak yerine, her ikisi de ortak bir soyutlamaya (arayüz) bağlı hale gelir. Bu soyutlama, genellikle üst seviye modül tarafından tanımlanır, bu da bağımlılığın "tersine çevrilmesi"ne neden olur.

**Örnek: DIP'ye Aykırı Tasarım**

Bir `ProductService` (Ürün Servisi) sınıfının doğrudan somut bir `MySQLDatabase` sınıfına bağımlı olduğunu varsayalım.

```typescript
// DIP'ye Aykırı Tasarım
class MySQLDatabase {
    connect(): void {
        console.log("Connecting to MySQL Database...");
    }
    save(data: string): void {
        console.log(`Saving '${data}' to MySQL.`);
    }
    read(): string {
        console.log("Reading data from MySQL.");
        return "Data from MySQL";
    }
}

// Üst seviye modül (ProductService) doğrudan alt seviye somut MySQLDatabase'e bağımlı
class ProductService {
    private db: MySQLDatabase; // Somut bir implementasyona bağımlı

    constructor() {
        this.db = new MySQLDatabase(); // Bağımlılık burada yaratılıyor
        this.db.connect();
    }

    addProduct(productName: string): void {
        this.db.save(`Product: ${productName}`);
        console.log(`Product ${productName} added.`);
    }

    getProducts(): string {
        return this.db.read();
    }
}

const productService = new ProductService();
productService.addProduct("Laptop");
console.log(productService.getProducts());

// PROBLEM: Eğer veritabanını PostgreSQL veya MongoDB ile değiştirmek istersek,
// ProductService sınıfını değiştirmek zorunda kalırız.
// Ayrıca ProductService'i test etmek için her zaman bir MySQLDatabase nesnesine ihtiyaç duyarız.
```

Yukarıdaki `ProductService`, `MySQLDatabase` somut sınıfına sıkı sıkıya bağlıdır. Bu, `ProductService`'in `MySQLDatabase`'in detaylarını bilmesi gerektiği ve veritabanı değiştiğinde `ProductService`'in de değişmesi gerektiği anlamına gelir. Bu, DIP'ye aykırıdır.

**DIP'ye Uygun Tasarım**

DIP'yi uygulamak için bir soyutlama (arayüz) kullanırız. Üst seviye modül bu soyutlamaya bağımlı olur, alt seviye somut implementasyonlar da bu soyutlamayı uygular.

```typescript
// DIP'ye Uygun Tasarım

// Adım 1: Soyutlama (Arayüz) tanımla
// Bu arayüz, veri depolama katmanından beklenen davranışı tanımlar.
interface IDatabase {
    connect(): void;
    save(data: string): void;
    read(): string;
}

// Adım 2: Alt seviye somut modüller bu arayüzü uygular
class MySQLDatabaseDIP implements IDatabase {
    connect(): void {
        console.log("Connecting to MySQL Database (DIP)...");
    }
    save(data: string): void {
        console.log(`Saving '${data}' to MySQL (DIP).`);
    }
    read(): string {
        console.log("Reading data from MySQL (DIP).");
        return "Data from MySQL (DIP)";
    }
}

class MongoDBDatabaseDIP implements IDatabase {
    connect(): void {
        console.log("Connecting to MongoDB Database (DIP)...");
    }
    save(data: string): void {
        console.log(`Saving '${data}' to MongoDB (DIP).`);
    }
    read(): string {
        console.log("Reading data from MongoDB (DIP).");
        return "Data from MongoDB (DIP)";
    }
}

// Adım 3: Üst seviye modül soyutlamaya (arayüze) bağımlı olur
// Bağımlılık Enjeksiyonu (Dependency Injection) ile IDatabase nesnesi dışarıdan verilir.
class ProductServiceDIP {
    private db: IDatabase; // Soyut bir implementasyona bağımlı

    // Bağımlılık Enjeksiyonu: Constructor aracılığıyla bağımlılığı alıyoruz
    constructor(database: IDatabase) {
        this.db = database;
        this.db.connect();
    }

    addProduct(productName: string): void {
        this.db.save(`Product: ${productName}`);
        console.log(`Product ${productName} added.`);
    }

    getProducts(): string {
        return this.db.read();
    }
}

// Kullanım: Farklı veritabanı implementasyonları ile ProductService'i oluşturabiliriz
console.log("--- Using MySQL with ProductService ---");
const mysqlDb = new MySQLDatabaseDIP();
const productServiceWithMySQL = new ProductServiceDIP(mysqlDb); // Bağımlılık enjekte edildi
productServiceWithMySQL.addProduct("Keyboard");
console.log(productServiceWithMySQL.getProducts());

console.log("\n--- Using MongoDB with ProductService ---");
const mongoDb = new MongoDBDatabaseDIP();
const productServiceWithMongoDB = new ProductServiceDIP(mongoDb); // Başka bir bağımlılık enjekte edildi
productServiceWithMongoDB.addProduct("Monitor");
console.log(productServiceWithMongoDB.getProducts());

// Test Edilebilirlik Örneği (Mock Database)
class MockDatabase implements IDatabase {
    private _data: string[] = [];
    connect(): void { console.log("Mock DB Connected."); }
    save(data: string): void {
        this._data.push(data);
        console.log(`Mock DB saved: ${data}`);
    }
    read(): string {
        return this._data.join(", ");
    }
}

console.log("\n--- Testing ProductService with Mock Database ---");
const mockDb = new MockDatabase();
const testProductService = new ProductServiceDIP(mockDb); // Test için mock nesnesi enjekte edildi
testProductService.addProduct("Test Item 1");
testProductService.addProduct("Test Item 2");
console.log(testProductService.getProducts()); // Çıktı: Mock DB saved: Test Item 1, Mock DB saved: Test Item 2, Test Item 1, Test Item 2
```

Bu yeni tasarımda:

* `IDatabase` arayüzü, üst seviye `ProductServiceDIP` ve alt seviye `MySQLDatabaseDIP` / `MongoDBDatabaseDIP` arasında bir soyutlama sağlar.
* `ProductServiceDIP`, somut `MySQLDatabaseDIP` veya `MongoDBDatabaseDIP` sınıflarına doğrudan bağımlı değildir; sadece `IDatabase` arayüzüne bağımlıdır.
* Hangi veritabanı implementasyonunun kullanılacağı, `ProductServiceDIP`'in kurucusuna dışarıdan (`new ProductServiceDIP(mysqlDb)`) enjekte edilir (bu, **Bağımlılık Enjeksiyonu (Dependency Injection)** olarak bilinir ve DIP'yi uygulamanın ana yollarından biridir).
* Yeni bir veritabanı (örneğin `ElasticsearchDatabase`) eklendiğinde, sadece `IDatabase` arayüzünü uygulayan yeni bir sınıf oluştururuz. `ProductServiceDIP` sınıfını değiştirmemize gerek kalmaz.
* Test sırasında, gerçek veritabanı bağlantısı kurmaya gerek kalmadan `ProductServiceDIP`'i test etmek için `MockDatabase` gibi sahte bir implementasyon enjekte edebiliriz.

DIP, yazılım mimarinizi çok daha esnek, test edilebilir ve sürdürülebilir hale getirir. Bu, özellikle büyük ve karmaşık kurumsal uygulamalar için kritik öneme sahiptir.

***

#### 7.6 SOLID Prensiplerinin Faydaları ve Uygulama Örnekleri

SOLID prensipleri, yazılım geliştiricilerin daha yüksek kaliteli, sürdürülebilir ve esnek kodlar yazmasına yardımcı olan temel tasarım rehberleridir. Bu prensipleri uygulamak, başlangıçta biraz çaba gerektirse de, uzun vadede projenizin sağlığı ve yönetilebilirliği açısından büyük getiriler sağlar.

**SOLID Prensiplerinin Genel Faydaları:**

1. **Daha Esnek ve Genişletilebilir Sistemler:**
   * OCP ve DIP sayesinde, sisteminize yeni özellikler veya davranışlar eklemek çok daha kolay hale gelir. Mevcut kodu değiştirmek yerine, yeni sınıflar ekleyerek veya mevcut arayüzlerin yeni implementasyonlarını sağlayarak genişleme yapabilirsiniz.
   * Bu, "bir kere yaz, sonsuza kadar kullan" (write once, use forever) mantığından ziyade, "bir kere yaz, sonra kolayca genişlet" (write once, then easily extend) mantığını destekler.
2. **Daha Kolay Bakım ve Test Edilebilirlik:**
   * SRP ve ISP, sınıfların daha küçük, odaklanmış ve bağımsız olmasını sağlar. Bu, bir hata meydana geldiğinde hatanın nerede olduğunu bulmayı ve düzeltmeyi kolaylaştırır.
   * DIP, bağımlılıkları soyutlamalar üzerinden yönettiği için, bir bileşeni izole bir şekilde test etmek için "mock" veya "stub" nesneleri kullanmanıza olanak tanır. Gerçek bağımlılıklara ihtiyaç duymadan birim testleri yazabilirsiniz.
3. **Kod Kalitesini Artırma:**
   * Daha az kopya kod, daha iyi adlandırma, daha küçük ve odaklanmış sınıflar, kodun genel okunabilirliğini ve anlaşılırlığını artırır.
   * Geliştiriciler, belirli bir işlevi hangi sınıfın yerine getirdiğini daha kolay anlayabilir.
4. **Daha Az Bağlılık (Loose Coupling) ve Daha Yüksek Uyumluluk (High Cohesion):**
   * SOLID prensipleri, sistemdeki bileşenler arasındaki bağımlılığı azaltır (loose coupling). Bir bileşendeki değişiklik, sistemin diğer birçok bölümünü etkileme olasılığını düşürür.
   * Aynı zamanda, bir sınıfın kendi içindeki öğelerinin (özellikler ve metotlar) birbirleriyle ne kadar ilişkili olduğunu (high cohesion) artırır. SRP, bu konuda doğrudan etkilidir.
5. **Daha İyi Bir Mimari:**
   * Bu prensipler, yazılım mimarinizi daha sağlam ve ölçeklenebilir hale getirmenize yardımcı olur. Uzun ömürlü ve karmaşık sistemler geliştirirken kritik öneme sahiptirler.

**Ortak Hatalardan Kaçınma Yolları:**

* **God Object / Monolithic Class:** Tek bir sınıfın çok fazla sorumluluğu üstlenmesinden (SRP ihlali) kaçının. Sorumlulukları ayırın.
* **Arayüz Bloat'ı:** Çok büyük, her şeyi kapsayan arayüzler oluşturmaktan kaçının (ISP ihlali). Arayüzleri amaca özel, küçük parçalara bölün.
* **İmplementasyonlara Bağımlılık:** İş mantığınızın somut implementasyonlara (veritabanı sürücüleri, harici API istemcileri) doğrudan bağımlı olmasından kaçının (DIP ihlali). Soyutlamalar (arayüzler) kullanın ve bağımlılık enjeksiyonunu düşünün.
* **Kırılgan Temel Sınıflar:** Miras alma hiyerarşilerinde, alt sınıfların temel sınıfın sözleşmesini bozmamasına dikkat edin (LSP ihlali). Alt sınıflar her zaman temel sınıflarının yerine geçebilmelidir.
* **Her Değişiklikte Kodda Değişiklik:** Yeni bir özellik eklerken veya mevcut bir davranışı değiştirirken sürekli olarak var olan kodda değişiklik yapmaktan kaçının (OCP ihlali). Genişletilebilir bir tasarım kullanın (polimorfizm, soyutlama).

**SOLID Prensiplerini Uygulama Örnekleri (Kısa Özeti):**

* **SRP:** Bir `OrderProcessor` sınıfını `OrderCreator`, `OrderValidator`, `EmailNotifier` gibi daha küçük, tek sorumluluklu sınıflara ayırma.
* **OCP:** Bir `NotificationService`'in, yeni bildirim türleri (SMS, Push Notification) eklendiğinde mevcut kodunu değiştirmeden `INotification` arayüzünü uygulayan yeni sınıflar ekleyerek genişletilmesi.
* **LSP:** Bir `ShapeDrawer` fonksiyonunun, `IShape` arayüzünü uygulayan herhangi bir şekli (kare, daire, üçgen) sorunsuz bir şekilde çizebilmesi, çünkü her biri `draw()` metodunu beklenen şekilde implemente eder.
* **ISP:** Bir `Worker` arayüzünü `IWorkable` (çalışabilen) ve `IEatable` (yemek yiyebilen) gibi daha küçük arayüzlere ayırarak, `Robot` gibi sadece çalışabilen varlıkların gereksiz `eat()` metodunu implemente etmek zorunda kalmamasını sağlama.
* **DIP:** Bir `Logger` (loglayıcı) sınıfının doğrudan bir `ConsoleLogger`'a bağımlı olmak yerine, `ILogger` arayüzüne bağımlı olması. Böylece `FileLogger` veya `DatabaseLogger` gibi farklı loglama implementasyonları kolayca değiştirilebilir.

SOLID prensipleri, iyi bir yazılım mühendisliği pratiği için bir yol haritası sunar. Başlangıçta öğrenmesi ve uygulaması zor görünebilir,
