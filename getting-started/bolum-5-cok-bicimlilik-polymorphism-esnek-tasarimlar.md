# Bölüm 5: Çok Biçimlilik (Polymorphism) - Esnek Tasarımlar

Harika! Miras alma ile sınıflarımız arasında güçlü bir hiyerarşi kurmayı öğrendik. Şimdi bu hiyerarşinin bize sağladığı en esnek özelliklerden birine, yani **Çok Biçimlilik (Polymorphism)**'e geçebiliriz.

***

### Bölüm 5: Çok Biçimlilik (Polymorphism) - Esnek Tasarımlar

Nesne Yönelimli Programlama'nın (OOP) dört ana prensibinden üçüncüsü **çok biçimlilik (polymorphism)**'tir. Kelime anlamı olarak "çok şekilli olma" veya "çok formlu olma" demektir. Yazılımda, farklı nesnelerin aynı arayüz veya temel sınıf tipi üzerinden çağrıldığında farklı davranışlar sergilemesi yeteneğini ifade eder.

Gerçek dünyadan bir örnekle açıklayacak olursak, "telefon etme" eylemini düşünün. Bir akıllı telefonla telefon edersiniz, bir sabit hatlı telefonla da telefon edersiniz, hatta bir uydu telefonla da. Her biri "telefon etme" eylemini gerçekleştirir, ancak her cihaz bu eylemi kendi iç mekanizmalarına göre farklı şekillerde yapar. Çok biçimlilik, tam da bu tür bir senaryoyu yazılımda modellememizi sağlar: farklı türdeki nesneleri aynı genel komutla işleyebilme yeteneği.

***

#### 5.1 Çok Biçimlilik Nedir? Neden Önemlidir?

**Çok biçimlilik**, bir temel sınıf referansının, o temel sınıftan türetilmiş farklı alt sınıfların nesnelerini işaret edebilmesi ve bu referans üzerinden çağrılan metotların, nesnenin gerçek tipine göre farklılık göstermesidir.

TypeScript'te çok biçimlilik, genellikle **metot ezme (method overriding)** ve **arayüzler (interfaces)** aracılığıyla sağlanır.

**Temel Konseptler:**

1. **Tek Arayüz, Çoklu Implementasyon:** Farklı sınıflar, aynı metot imzasına sahip olabilir veya aynı arayüzü uygulayabilir. Bu, aynı isme sahip bir metodun, farklı nesnelerde farklı davranışlar sergileyebileceği anlamına gelir.
2. **Temel Tip Referansı:** Ortak bir temel sınıf veya arayüz tipinde bir değişken tanımlayabilir ve bu değişkene, temel tipin herhangi bir türetilmiş sınıfından oluşturulmuş bir nesneyi atayabiliriz.
3. **Çalışma Zamanı Bağlama (Runtime Binding):** Temel tip referansı üzerinden ezilmiş bir metot çağrıldığında, hangi metodun (temel sınıftaki mi yoksa türetilmiş sınıftaki mi) çalışacağına programın derleme zamanında değil, **çalışma zamanında** (runtime) karar verilir. Bu, nesnenin bellekteki gerçek tipine göre doğru metodun çağrılmasını sağlar.

**Örnek: Hayvan Sesleri**

Önceki `Animal` ve `Dog` örneğimize bir `Cat` sınıfı ekleyerek çok biçimliliği görelim.

```typescript
// Temel Sınıf
class Animal {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    // Her hayvanın bir ses çıkarma yeteneği olsun
    // Bu metot alt sınıflar tarafından ezilmelidir
    makeSound(): void {
        console.log(`${this.name} makes a generic sound.`);
    }
}

// Türetilmiş Sınıf
class Dog extends Animal {
    constructor(name: string) {
        super(name);
    }

    // makeSound metodunu ezme
    makeSound(): void {
        console.log(`${this.name} barks! Woof!`);
    }
}

// Başka bir Türetilmiş Sınıf
class Cat extends Animal {
    constructor(name: string) {
        super(name);
    }

    // makeSound metodunu ezme
    makeSound(): void {
        console.log(`${this.name} meows! Meow!`);
    }
}

// Farklı türde hayvan nesneleri oluşturalım
const genericAnimal = new Animal("Generic");
const myDog = new Dog("Buddy");
const myCat = new Cat("Whiskers");

// Her bir nesnenin kendi makeSound metodunu çağıralım
genericAnimal.makeSound(); // Çıktı: Generic makes a generic sound.
myDog.makeSound();        // Çıktı: Buddy barks! Woof!
myCat.makeSound();        // Çıktı: Whiskers meows! Meow!

console.log("--- Using Polymorphism ---");

// Bir Animal dizisi oluşturalım ve içine farklı Animal türlerini ekleyelim
const animals: Animal[] = [];
animals.push(new Dog("Max"));
animals.push(new Cat("Luna"));
animals.push(new Animal("Wild Creature")); // Temel sınıfın kendisi de olabilir
animals.push(new Dog("Daisy"));

// Dizideki her hayvan için makeSound metodunu çağıralım
// Burada önemli olan, 'animal' değişkeninin tipi 'Animal' olmasına rağmen,
// çalışma zamanında nesnenin gerçek tipine (Dog, Cat vb.) göre doğru makeSound metodu çağrılır.
animals.forEach(animal => {
    animal.makeSound();
});
// Çıktı:
// Max barks! Woof!
// Luna meows! Meow!
// Wild Creature makes a generic sound.
// Daisy barks! Woof!
```

Yukarıdaki örnekte:

* `Animal` sınıfı bir `makeSound()` metoduna sahiptir.
* `Dog` ve `Cat` sınıfları `Animal`'dan miras alır ve `makeSound()` metodunu kendi türlerine özgü sesleri çıkaracak şekilde ezerler.
* `animals` dizisi `Animal[]` tipindedir, yani `Animal` tipindeki veya ondan türetilmiş herhangi bir nesneyi tutabilir.
* `forEach` döngüsünde, `animal` değişkeni her iterasyonda bir `Animal` nesnesini temsil eder. Ancak `animal.makeSound()` çağrıldığında, JavaScript motoru o anki `animal` nesnesinin gerçek tipine bakar (`Dog` mu, `Cat` mi, yoksa `Animal`'ın kendisi mi?) ve o tipin `makeSound()` metodunu çalıştırır. İşte bu **polimorfizm**'dir!

**Çok Biçimliliğin Faydaları (Neden Önemlidir?):**

1. **Esneklik ve Genişletilebilirlik:** Sisteme yeni bir hayvan türü (örneğin `Cow`) eklemek istediğinizde, sadece `Animal`'dan türeyen yeni bir sınıf oluşturur ve `makeSound()` metodunu ezersiniz. Mevcut `animals` dizisini işleyen `forEach` döngüsünü veya diğer kod parçacıklarını değiştirmeye gerek kalmaz. Bu, kodu daha az kırılgın ve daha kolay genişletilebilir yapar.
2. **Kod Tekrarını Azaltma:** Ortak arayüz sayesinde, farklı türler üzerinde aynı işlemi (örneğin tüm hayvanların ses çıkarmasını sağlama) tek bir döngü veya fonksiyonla gerçekleştirebilirsiniz. Her hayvan türü için ayrı ayrı kod yazmak zorunda kalmazsınız.
3. **Daha Okunabilir ve Temiz Kod:** Genel türler ve arayüzler üzerinden çalışmak, kodun daha soyut ve üst düzey düşünülmesini sağlar. Bu, karmaşıklığı azaltır ve niyetinizi daha net ifade etmenizi sağlar.
4. **Bakım Kolaylığı:** Yeni bir davranış eklemek veya mevcut bir davranışı değiştirmek, sadece ilgili alt sınıfta yapılabilir ve bu, sistemin diğer kısımlarını etkilemez.

Çok biçimlilik, OOP'de büyük ve karmaşık sistemleri yönetmek için olmazsa olmaz bir prensiptir. Esneklik, genişletilebilirlik ve kodun genel kalitesi açısından sunduğu faydalar, modern yazılım geliştirmede vazgeçilmezdir.

***

#### 5.2 Metot Ezme (Method Overriding) ile Çok Biçimlilik

Metot ezme (Method Overriding), çok biçimliliğin en yaygın uygulama şekillerinden biridir. Bir temel sınıfta tanımlanan bir metodu, türetilmiş sınıflarda aynı imzayla yeniden tanımlayarak her türe özgü bir davranış sergilemesini sağlarız. Çalışma zamanında, nesnenin gerçek tipine göre hangi metodun çağrılacağına karar verilir.

**Çalışma Zamanı Bağlama (Runtime Binding / Dynamic Dispatch):**

Bu konsept, çok biçimliliğin temelini oluşturur. Derleyici, bir temel sınıf tipindeki değişkene bakarak hangi metodun çağrılacağını tam olarak bilemez. Örneğin:

```typescript
let myAnimal: Animal;
myAnimal = new Dog("Buddy");
myAnimal.makeSound(); // Hangi makeSound çağrılacak?
```

Burada `myAnimal`'ın tipi `Animal`'dır. Ancak çalışma zamanında, `myAnimal`'ın aslında bir `Dog` nesnesini işaret ettiği anlaşılır ve `Dog` sınıfının ezilmiş `makeSound()` metodu çağrılır. Bu dinamik karar verme süreci **çalışma zamanı bağlama** olarak adlandırılır.

**Örnek: Çalışan Maaş Hesaplaması**

Farklı çalışan tiplerinin (Normal Çalışan, Yönetici, Serbest Çalışan) farklı maaş hesaplama mantıkları olduğunu varsayalım. Ortak bir `Employee` sınıfı ve `calculateSalary` metodu tanımlayarak bunu modelleyebiliriz.

```typescript
// Temel Sınıf
class Employee {
    name: string;
    hourlyRate: number; // Saatlik ücret

    constructor(name: string, hourlyRate: number) {
        this.name = name;
        this.hourlyRate = hourlyRate;
    }

    // Her çalışanın maaşını hesaplayan temel metot
    calculateSalary(hours: number): number {
        return this.hourlyRate * hours;
    }

    displayDetails(): void {
        console.log(`Name: ${this.name}, Hourly Rate: $${this.hourlyRate}`);
    }
}

// Türetilmiş Sınıf: Manager (Yönetici)
// Maaşına ek olarak prim (bonus) alır
class Manager extends Employee {
    bonus: number;

    constructor(name: string, hourlyRate: number, bonus: number) {
        super(name, hourlyRate);
        this.bonus = bonus;
    }

    // calculateSalary metodunu ezme
    calculateSalary(hours: number): number {
        // Üst sınıfın hesaplamasını kullan ve prim ekle
        const baseSalary = super.calculateSalary(hours);
        return baseSalary + this.bonus;
    }

    displayDetails(): void {
        super.displayDetails(); // Üst sınıfın detaylarını göster
        console.log(`Role: Manager, Bonus: $${this.bonus}`);
    }
}

// Türetilmiş Sınıf: Freelancer (Serbest Çalışan)
// Proje bazlı ücret alır, saatlik ücret ve proje katsayısı vardır
class Freelancer extends Employee {
    projectMultiplier: number;

    constructor(name: string, hourlyRate: number, projectMultiplier: number) {
        super(name, hourlyRate);
        this.projectMultiplier = projectMultiplier;
    }

    // calculateSalary metodunu ezme
    calculateSalary(hours: number): number {
        // Kendi özel hesaplama mantığı
        return (this.hourlyRate * hours) * this.projectMultiplier;
    }

    displayDetails(): void {
        super.displayDetails(); // Üst sınıfın detaylarını göster
        console.log(`Role: Freelancer, Project Multiplier: ${this.projectMultiplier}`);
    }
}

// Farklı çalışan türlerinden nesneler oluşturalım
const emp1 = new Employee("Jane Doe", 20);
const manager1 = new Manager("John Smith", 30, 500);
const freelancer1 = new Freelancer("Alice Brown", 25, 1.2);

// Tüm çalışanları tutabilecek bir dizi (Polimorfik dizi)
const employees: Employee[] = [];
employees.push(emp1);
employees.push(manager1);
employees.push(freelancer1);

console.log("--- Employee Salaries ---");
employees.forEach(employee => {
    employee.displayDetails();
    // Burada 'employee' değişkeninin tipi 'Employee' olmasına rağmen,
    // çalışma zamanında her nesnenin kendi calculateSalary metodu çağrılır.
    const salary = employee.calculateSalary(160); // Her biri 160 saat çalıştı
    console.log(`Calculated Salary for ${employee.name}: $${salary.toFixed(2)}\n`);
});

// Çıktı:
// --- Employee Salaries ---
// Name: Jane Doe, Hourly Rate: $20
// Calculated Salary for Jane Doe: $3200.00
//
// Name: John Smith, Hourly Rate: $30
// Role: Manager, Bonus: $500
// Calculated Salary for John Smith: $5300.00
//
// Name: Alice Brown, Hourly Rate: $25
// Role: Freelancer, Project Multiplier: 1.2
// Calculated Salary for Alice Brown: $4800.00
```

Bu örnek, metot ezme ve çalışma zamanı bağlamanın çok biçimliliği nasıl sağladığını mükemmel bir şekilde gösteriyor:

* `Employee` sınıfı, `calculateSalary()` için genel bir implementasyon sağlar.
* `Manager` ve `Freelancer` sınıfları, `calculateSalary()` metodunu kendi özel maaş hesaplama mantıklarıyla ezerler.
* `employees` dizisi, `Employee` tipinde olmasına rağmen, içinde `Employee`, `Manager` ve `Freelancer` nesnelerini tutabilir.
* `employees.forEach(employee => { ... employee.calculateSalary(160); ... });` döngüsünde, her `employee` nesnesi için `calculateSalary()` metodu çağrıldığında, TypeScript/JavaScript motoru otomatik olarak o anki nesnenin gerçek tipine (runtime type) bakarak doğru ezilmiş metodu çalıştırır.

Bu, kodu son derece esnek ve genişletilebilir hale getirir. Yeni bir çalışan türü (örneğin `Intern` - stajyer) eklemek istediğinizde, sadece `Employee`'dan miras alıp `calculateSalary()`'yi ezmeniz yeterlidir; mevcut `employees` dizisini işleyen döngüyü değiştirmeye gerek kalmaz. İşte çok biçimliliğin gücü budur!

***

#### 5.3 Arayüzler (Interfaces) ile Çok Biçimlilik

Çok biçimlilik, sadece sınıf miras alma yoluyla değil, aynı zamanda **arayüzler (interfaces)** aracılığıyla da sağlanabilir. Arayüzler, bir sınıfın (veya objenin) sahip olması gereken metotları ve/veya özellikleri tanımlayan bir "sözleşme"dir. Arayüzler, bir sınıfın "ne yapabileceğini" tanımlar, "nasıl yapacağını" değil.

TypeScript, güçlü bir tip sistemi olduğundan, arayüzler çok biçimliliği sağlamada kilit bir rol oynar. Bir sınıf bir arayüzü uyguladığında (`implements` anahtar kelimesiyle), o arayüzde tanımlanan tüm üyeleri (metotlar ve özellikler) içermek zorundadır.

**Neden Arayüzler Kullanılır?**

* **Sözleşme Tanımlama:** Farklı sınıfların ortak bir davranış setine uymasını zorunlu kılan bir sözleşme sağlar.
* **Tam Kapsülleme:** Arayüzler implementasyon detayları içermez, sadece tanımlamalar içerir. Bu, tam bir soyutlama sağlar.
* **Çoklu Tip İlişkisi:** Bir sınıf tek bir sınıftan miras alabilirken, birden fazla arayüzü uygulayabilir. Bu, "bir X bir Y'dir" ilişkisinin yanı sıra "bir X şunları yapabilir" ilişkisini de modellememizi sağlar.
* **Gevşek Bağlılık (Loose Coupling):** Kodunuzu somut sınıflara bağımlı hale getirmek yerine, arayüzlere bağımlı hale getirirsiniz. Bu, modüller arasındaki bağımlılığı azaltır ve sistemin daha esnek olmasını sağlar.

**Örnek: Yazdırılabilir Nesneler**

Farklı türdeki nesnelerin (raporlar, faturalar, belgeler) yazdırılabilir olması gerektiğini varsayalım. Bir `IPrintable` arayüzü tanımlayarak bunu modelleyebiliriz.

```typescript
// Arayüz Tanımı
// Bir nesnenin yazdırılabilir olması için sahip olması gereken davranışı tanımlar.
interface IPrintable {
    print(): void; // print adında bir metodu olmalı
    getPrintContent(): string; // getPrintContent adında bir metodu olmalı ve string döndürmeli
}

// Sınıf 1: Rapor
// IPrintable arayüzünü uyguluyor
class Report implements IPrintable {
    title: string;
    content: string;

    constructor(title: string, content: string) {
        this.title = title;
        this.content = content;
    }

    print(): void {
        console.log(`Printing Report: ${this.title}`);
        console.log(`Content:\n${this.content}`);
    }

    getPrintContent(): string {
        return `Report Title: ${this.title}\nReport Body:\n${this.content}`;
    }
}

// Sınıf 2: Fatura
// IPrintable arayüzünü uyguluyor
class Invoice implements IPrintable {
    invoiceNumber: string;
    amount: number;
    customerName: string;

    constructor(invoiceNumber: string, amount: number, customerName: string) {
        this.invoiceNumber = invoiceNumber;
        this.amount = amount;
        this.customerName = customerName;
    }

    print(): void {
        console.log(`Printing Invoice #${this.invoiceNumber}`);
        console.log(`Customer: ${this.customerName}, Amount: $${this.amount.toFixed(2)}`);
    }

    getPrintContent(): string {
        return `Invoice No: ${this.invoiceNumber}\nCustomer: ${this.customerName}\nAmount: $${this.amount.toFixed(2)}`;
    }
}

// Sınıf 3: Resim (Image)
// IPrintable arayüzünü uyguluyor
class Image implements IPrintable {
    filename: string;
    dimensions: string;

    constructor(filename: string, dimensions: string) {
        this.filename = filename;
        this.dimensions = dimensions;
    }

    print(): void {
        console.log(`Printing Image: ${this.filename}`);
        console.log(`Dimensions: ${this.dimensions}`);
    }

    getPrintContent(): string {
        return `Image File: ${this.filename}\nDimensions: ${this.dimensions}`;
    }
}

// Ortak arayüz tipini kullanarak polimorfik bir dizi
const printQueue: IPrintable[] = [];
printQueue.push(new Report("Monthly Sales", "Sales figures show a 10% increase..."));
printQueue.push(new Invoice("INV-2023-001", 150.75, "Acme Corp."));
printQueue.push(new Image("logo.png", "100x100px"));
printQueue.push(new Report("Annual Budget", "Budget allocation for next year..."));

console.log("--- Processing Print Queue ---");
printQueue.forEach(item => {
    // item'ın somut tipini bilmeden, sadece IPrintable arayüzüne güvenerek print() metodunu çağırıyoruz
    item.print(); // Çalışma zamanında doğru sınıfın print() metodu çalışır
    console.log("---");
});

// Ayrıca getPrintContent() de çağrılabilir
console.log("\n--- Getting Print Contents ---");
printQueue.forEach(item => {
    console.log(item.getPrintContent());
    console.log("---");
});
```

Bu örnekte:

* `IPrintable` arayüzü, `print()` ve `getPrintContent()` metotlarına sahip olma sözleşmesini tanımlar.
* `Report`, `Invoice` ve `Image` sınıfları, bu arayüzü `implements` anahtar kelimesiyle uygulayarak, arayüzde belirtilen tüm metotları kendi özel implementasyonlarıyla sağlamak zorundadır.
* `printQueue` dizisi `IPrintable[]` tipindedir. Bu, dizinin `IPrintable` arayüzünü uygulayan herhangi bir nesneyi tutabileceği anlamına gelir.
* `forEach` döngüsünde, `item` değişkeninin tipi `IPrintable`'dır. `item.print()` çağrıldığında, TypeScript çalışma zamanında `item`'ın aslında bir `Report`, `Invoice` veya `Image` nesnesi olduğunu anlar ve o nesnenin kendi `print()` metodunu çalıştırır.

**Arayüzlerle Çok Biçimliliğin Faydaları:**

1. **Daha Gevşek Bağlılık:** Kodunuzu somut sınıflara (`Report`, `Invoice`) bağımlı hale getirmek yerine, soyut bir arayüze (`IPrintable`) bağımlı hale getirirsiniz. Bu, sistemin farklı bileşenleri arasındaki bağımlılığı azaltır. Eğer bir gün `Invoice` sınıfının adı veya iç yapısı değişirse, `printQueue`'yu işleyen kodun etkilenmesi riski azalır, çünkü o sadece `IPrintable` sözleşmesini bekler.
2. **Esneklik ve Genişletilebilirlik:** Yeni bir yazdırılabilir öğe (örneğin `Document`) eklemek istediğinizde, tek yapmanız gereken `IPrintable` arayüzünü uygulayan yeni bir sınıf oluşturmaktır. `printQueue`'yu işleyen mevcut kodda hiçbir değişiklik yapmaya gerek kalmaz.
3. **Çoklu Miras Benzeri Yetenek:** TypeScript'te bir sınıf birden fazla arayüzü uygulayabilir. Bu, bir sınıfın birden fazla "türden" davranış setine uyabileceği anlamına gelir (örneğin, hem `IPrintable` hem de `ISaveable` olabilir). Bu, bazı dillerdeki çoklu mirasın yol açtığı sorunlardan kaçınarak esneklik sağlar.
4. **Test Edilebilirlik:** Arayüzler, birim testi yazmayı kolaylaştırır. Bir metodun belirli bir arayüze uyan bir nesne beklediği durumlarda, gerçek sınıf yerine o arayüzü uygulayan bir "mock" veya "stub" nesnesi oluşturarak bağımlılıkları kolayca simüle edebilirsiniz.

Arayüzler, özellikle büyük ve karmaşık uygulamalarda, farklı sınıfların ortak davranışlar sergilemesini gerektiren senaryolarda çok biçimliliği sağlamak için güçlü ve temiz bir yoldur.

***

#### 5.4 Çok Biçimliliğin Uygulama Alanları ve Avantajları

Çok biçimlilik, modern yazılım geliştirmede vazgeçilmez bir prensiptir. Uygulama alanları oldukça geniştir ve sağladığı avantajlar, daha kaliteli, esnek ve bakımı kolay sistemler inşa etmemize olanak tanır.

**Başlıca Uygulama Alanları:**

1. **Koleksiyonlar ve Jenerik İşlemler:**
   * Örneklerde gördüğümüz gibi, aynı temel tipteki (veya aynı arayüzü uygulayan) nesneleri bir dizide veya başka bir koleksiyonda toplamak ve her birine aynı komutu (örneğin `makeSound()`, `print()`, `calculateSalary()`) vermek.
   * Bu, kodu daha az tekrarlı ve daha okunabilir hale getirir. Yeni türler eklendiğinde mevcut işleme kodunu değiştirmeye gerek kalmaz.
2. **Fabrika Metotları ve Abstract Fabrikalar:**
   * Belirli bir türden nesne oluşturmak için kullanılır, ancak hangi somut türün oluşturulacağına çalışma zamanında karar verilir.
   * Örneğin, bir `VehicleFactory` (Araç Fabrikası) metodunuz olabilir. Bu metot, kendisine verilen bir `string` değere (örneğin "car", "truck") göre bir `Vehicle` (Araç) arayüzünü uygulayan farklı somut sınıfların (örneğin `Car`, `Truck`) nesnelerini döndürebilir. Kullanıcı, fabrikanın ne döndürdüğünü bilmek zorunda kalmaz, sadece `Vehicle` arayüzünü kullanır.
3. **Strateji Deseni (Strategy Pattern):**
   * Bir algoritma ailesini tanımlamak ve her algoritmayı ayrı bir sınıfta kapsüllemek için kullanılır. Bu sınıflar ortak bir arayüzü uygular.
   * Örneğin, farklı ödeme yöntemleri (`CreditCardPayment`, `PayPalPayment`) ortak bir `IPaymentStrategy` arayüzünü uygulayabilir. Bir sipariş işleme modülü, hangi ödeme stratejisinin kullanıldığını bilmeden `processPayment()` metodunu çağırabilir.
4. **Şablon Metot Deseni (Template Method Pattern):**
   * Bir algoritmanın iskeletini bir temel sınıfta tanımlarken, belirli adımların implementasyonunu alt sınıflara bırakmak için kullanılır.
   * Örneğin, bir `ReportGenerator` sınıfı, rapor oluşturma sürecinin genel adımlarını (veri alma, işleme, formatlama, çıktı alma) tanımlar. Ancak "veri alma" adımı `DatabaseReportGenerator` için veritabanından, `CsvReportGenerator` için CSV dosyasından farklı olabilir.
5. **Olay İşleme Sistemleri:**
   * Farklı türdeki olaylar (mouse click, keyboard press, network response) ortak bir `IEvent` arayüzünü uygulayabilir. Bir olay işleyici, belirli bir olayı almak ve polimorfik olarak işlemek için yazılabilir.

**Çok Biçimliliğin Sağladığı Temel Avantajlar:**

1. **Yeniden Kullanılabilirlik (Reusability):** Ortak bir arayüz veya temel sınıf üzerinden çalışarak, farklı tiplerde aynı kodu tekrar yazmaktan kaçınırsınız. Tek bir fonksiyona veya algoritmayı, çeşitli nesne türleriyle çalışacak şekilde yazabilirsiniz.
   * Örnek: Tek bir `processPayment(payment: IPaymentStrategy)` fonksiyonu, farklı ödeme yöntemlerini işleyebilir.
2. **Esneklik ve Genişletilebilirlik (Flexibility & Extensibility):** Mevcut kodu değiştirmeden sisteme yeni işlevsellik eklemenizi sağlar. Yeni bir alt sınıf veya arayüz implementasyonu eklediğinizde, bu yeni tür otomatik olarak mevcut polimorfik kod tarafından işlenebilir. Bu, "açık/kapalı prensibi" (Open/Closed Principle - SOLID prensiplerinden biri) ile uyumludur: yazılım varlıkları genişlemeye açık, ancak değişiklik yapmaya kapalı olmalıdır.
   * Örnek: Yeni bir `CryptoPayment` stratejisi eklediğinizde, `processPayment` fonksiyonunu güncellemeniz gerekmez.
3. **Gevşek Bağlılık (Loose Coupling):** Bileşenler arasındaki bağımlılığı azaltır. Bir modül, somut sınıflar yerine arayüzlere bağımlı olduğunda, o somut sınıflarda yapılan değişiklikler modülü daha az etkiler. Bu, kodun daha esnek ve bakımı kolay olmasını sağlar.
   * Örnek: `Report` sınıfının iç implementasyonu değişse bile, `printQueue`'yu işleyen kod etkilenmez çünkü sadece `IPrintable` arayüzüne bağımlıdır.
4. **Daha Temiz ve Okunabilir Kod:** Soyutlama seviyesini artırır. Detayları gizler ve kodun daha genel, niyetini daha net ifade eden bir şekilde yazılmasını sağlar. Karmaşıklığı yönetmeye yardımcı olur.
5. **Test Edilebilirlik:** Modüller arasındaki bağımlılıklar arayüzler aracılığıyla azaldığında, birim testleri yazmak çok daha kolay hale gelir. Bağımlı olunan gerçek bileşenler yerine, arayüzün mock (sahte) implementasyonlarını kullanarak bir bileşeni izole bir şekilde test edebilirsiniz.

Çok biçimlilik, dinamik, adapte edilebilir ve uzun ömürlü yazılım sistemleri inşa etmek için kritik bir araçtır. OOP'nin gücünü tam olarak ortaya çıkaran bir prensiptir.

***

Bölüm 5'i de tamamladık. Çok biçimliliğin ne olduğunu, metot ezme ve arayüzler aracılığıyla nasıl uygulandığını ve yazılım tasarımına ne gibi esneklikler getirdiğini detaylıca ele aldık.

Sıradaki ve son ana prensibimiz **Soyutlama (Abstraction)** olacak. Bu bölümde, kullanıcıdan gereksiz detayları gizleyerek sistemin karmaşıklığını nasıl yöneteceğimizi öğreneceğiz.
