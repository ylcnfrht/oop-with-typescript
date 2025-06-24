# Bölüm 2: Sınıflar (Classes) - Nesnelerin Temeli

Nesne Yönelimli Programlama'nın kalbi, **sınıflar** ve bu sınıflardan türetilen **nesnelerdir**. Sınıflar, nesnelerin nasıl görüneceğini ve nasıl davranacağını tanımlayan soyut şablonlardır. Gerçek dünyada bir ev inşa etmeden önce bir mimari plan çizmeye benzer; sınıf bu plandır, nesne ise o plana göre inşa edilmiş somut evdir.

Bu bölümde, TypeScript'te sınıfların nasıl tanımlandığını, özelliklerinin ve metotlarının nasıl belirlendiğini, nesnelerin nasıl oluşturulduğunu ve bu yapı taşlarını kullanarak sağlam bir temel atmayı öğreneceğiz.

***

#### 2.1 Sınıf Tanımlama ve Söz Dizimi

TypeScript'te bir sınıf tanımlamak için `class` anahtar kelimesini kullanırız. Sınıf adı genellikle **PascalCase** (her kelimenin ilk harfi büyük) ile yazılır ve çoğul değil, tekil olmalıdır (örneğin `Book` yerine `Books` değil).

**Temel Sınıf Yapısı:**

Bir sınıfın temel yapısı, `class` anahtar kelimesiyle başlar, ardından sınıf adı gelir ve süslü parantezler `{}` içinde sınıfın üyeleri (özellikler ve metotlar) yer alır.

```typescript
// Sınıf tanımlaması
class Person {
    // Sınıf üyeleri (özellikler ve metotlar) buraya gelir
}
```

Bu örnekte, `Person` adında boş bir sınıf tanımladık. Bu haliyle bile, bu sınıftan nesneler oluşturabiliriz, ancak bu nesnelerin henüz herhangi bir özelliği veya davranışı olmayacaktır.

**Örnekler: Basit bir `Person` veya `Product` sınıfı tanımı**

Şimdi daha somut örneklerle sınıfları inceleyelim.

**Örnek 1: `Person` Sınıfı**

Bir kişinin adını ve yaşını tutan basit bir `Person` sınıfı tanımlayalım.

```typescript
// Person sınıfının tanımı
class Person {
    // Özellikler (properties): Her Person nesnesinin sahip olacağı veri parçacıkları
    name: string;
    age: number;

    // Metotlar (methods): Her Person nesnesinin yapabileceği davranışlar
    greet(): string {
        return `Hello, my name is ${this.name} and I am ${this.age} years old.`;
    }
}

// Person sınıfından bir nesne (instance) oluşturma
const person1 = new Person();

// Nesnenin özelliklerine değer atama
person1.name = "Alice";
person1.age = 30;

// Nesnenin metodunu çağırma
console.log(person1.greet()); // Çıktı: Hello, my name is Alice and I am 30 years old.

// Başka bir Person nesnesi oluşturma
const person2 = new Person();
person2.name = "Bob";
person2.age = 25;
console.log(person2.greet()); // Çıktı: Hello, my name is Bob and I am 25 years old.
```

Yukarıdaki örnekte:

* `class Person { ... }`: Bir `Person` sınıfı tanımladık.
* `name: string;`: Her `Person` nesnesinin bir `name` (ad) özelliğine sahip olacağını ve bunun bir `string` tipinde olacağını belirttik.
* `age: number;`: Her `Person` nesnesinin bir `age` (yaş) özelliğine sahip olacağını ve bunun bir `number` tipinde olacağını belirttik.
* `greet(): string { ... }`: Her `Person` nesnesinin `greet` adında bir metoda sahip olacağını ve bu metodun bir `string` döndüreceğini tanımladık. `this.name` ve `this.age` ifadeleri, metodun çağrıldığı **o anki nesnenin** `name` ve `age` özelliklerine erişmek için kullanılır.
* `const person1 = new Person();`: `Person` sınıfından `person1` adında **yeni bir nesne** oluşturduk. `new` anahtar kelimesi, bir sınıfın örneğini (instance) yaratmak için kullanılır.
* `person1.name = "Alice";`: `person1` nesnesinin `name` özelliğine "Alice" değerini atadık.
* `console.log(person1.greet());`: `person1` nesnesinin `greet` metodunu çağırdık.

**Örnek 2: `Product` Sınıfı**

Bir ürünün adını, fiyatını ve stok miktarını tutan bir `Product` sınıfı tanımlayalım.

```typescript
// Product sınıfının tanımı
class Product {
    // Özellikler
    name: string;
    price: number;
    stock: number;

    // Metotlar
    displayProductInfo(): void {
        console.log(`Product: ${this.name}`);
        console.log(`Price: $${this.price.toFixed(2)}`); // Fiyatı iki ondalık basamakla gösterme
        console.log(`Stock: ${this.stock} units`);
    }

    sell(quantity: number): void {
        // Yeterli stok olup olmadığını kontrol etme
        if (this.stock >= quantity) {
            this.stock -= quantity; // Stoktan düşme
            console.log(`${quantity} units of ${this.name} sold.`);
            console.log(`Remaining stock: ${this.stock}`);
        } else {
            console.log(`Not enough ${this.name} in stock to sell ${quantity} units.`);
        }
    }
}

// Product sınıfından nesneler oluşturma
const laptop = new Product();
laptop.name = "Laptop";
laptop.price = 1200.50;
laptop.stock = 50;

console.log("--- Laptop Info ---");
laptop.displayProductInfo(); // Çıktı: Ürün bilgileri

laptop.sell(5); // Çıktı: 5 units of Laptop sold. Remaining stock: 45
laptop.sell(100); // Çıktı: Not enough Laptop in stock to sell 100 units.
```

Bu örnekte, `Product` sınıfının nasıl tanımlandığını ve `displayProductInfo()` ile `sell()` gibi metotların nasıl kullanıldığını görüyoruz. `sell()` metodu, stok kontrolü gibi basit bir iş mantığını içerir. Bu, verinin (stok) ve bu veri üzerindeki operasyonun (satış) aynı **nesne** içinde birleştirilerek daha modüler ve yönetilebilir bir yapı oluşturulduğunu gösterir.

***

#### 2.2 Özellikler (Properties) ve Üyeler (Members)

Bir sınıfın **özellikleri (properties)**, o sınıfın nesnelerinin sahip olacağı veri parçacıklarıdır. Bunlar, bir nesnenin durumunu veya niteliklerini tanımlar. Sınıf içerisinde değişkenler gibi bildirilirler.

**Özellik Bildirimi ve Tip Tanımlamaları:**

TypeScript, değişkenlerde olduğu gibi sınıf özelliklerinde de tip tanımlamasını zorunlu kılar veya çıkarım yapar. Bu, kodun okunabilirliğini artırır ve hataları derleme zamanında yakalamaya yardımcı olur.

```typescript
class Car {
    brand: string; // Marka özelliği, string tipinde
    model: string; // Model özelliği, string tipinde
    year: number;  // Yıl özelliği, number tipinde
    color: string; // Renk özelliği, string tipinde
}
```

Yukarıdaki `Car` sınıfında `brand`, `model`, `year` ve `color` adında dört özellik tanımladık. Her birine uygun TypeScript tipi (`string`, `number`) atandı.

**Varsayılan Değerler, Opsiyonel Özellikler (`?`) ve Salt Okunur Özellikler (`readonly`)**

Özellikleri tanımlarken, onlara varsayılan değerler atayabilir veya opsiyonel olduklarını belirtebiliriz. Ayrıca, bir özelliğin yalnızca oluşturulduğunda ayarlanabilmesini ve sonradan değiştirilememesini isteyebiliriz.

*   **Varsayılan Değerler:** Bir özelliğe tanımlandığı anda bir başlangıç değeri atayabiliriz. Bu, eğer kurucu metot (constructor) bu özelliğe bir değer atamazsa, özelliğin bu varsayılan değere sahip olmasını sağlar.

    TypeScript

    ```typescript
    class Settings {
        theme: string = "dark"; // Varsayılan tema "dark"
        fontSize: number = 16;  // Varsayılan font boyutu 16
    }

    const userSettings = new Settings();
    console.log(userSettings.theme);    // Çıktı: dark
    console.log(userSettings.fontSize); // Çıktı: 16
    ```
*   **Opsiyonel Özellikler (`?`):** Bir özelliğin bir nesne oluşturulduğunda mutlaka var olması gerekmediğini belirtmek için adının sonuna `?` (soru işareti) ekleriz. Bu, özelliğin `undefined` olabileceği anlamına gelir.



    ```typescript
    class User {
        id: number;
        username: string;
        email?: string; // email özelliği opsiyonel

        constructor(id: number, username: string, email?: string) {
            this.id = id;
            this.username = username;
            if (email) {
                this.email = email;
            }
        }
    }

    const user1 = new User(1, "john.doe");
    const user2 = new User(2, "jane.doe", "jane@example.com");

    console.log(user1.username); // Çıktı: john.doe
    console.log(user1.email);    // Çıktı: undefined

    console.log(user2.username); // Çıktı: jane.doe
    console.log(user2.email);    // Çıktı: jane@example.com
    ```
*   **Salt Okunur Özellikler (`readonly`):** Bir özelliğin yalnızca tanımlandığı yerde veya kurucu metot içinde bir kez atanabilmesini ve sonradan asla değiştirilememesini istiyorsak, önüne `readonly` anahtar kelimesini ekleriz. Bu, değişmez (immutable) veri oluşturmak için kullanışlıdır.



    ```typescript
    class BankTransaction {
        readonly transactionId: string; // İşlem ID'si sadece oluşturulduğunda atanabilir
        amount: number;
        date: Date;

        constructor(id: string, amount: number, date: Date) {
            this.transactionId = id; // Kurucu içinde atama yapabiliriz
            this.amount = amount;
            this.date = date;
        }

        // changeTransactionId(newId: string): void {
        //     this.transactionId = newId; // Hata: 'transactionId' salt okunur bir özellik
        // }
    }

    const transaction = new BankTransaction("TXN12345", 150.75, new Date());
    console.log(transaction.transactionId); // Çıktı: TXN12345
    // transaction.transactionId = "NEW_ID"; // Hata: 'transactionId' salt okunur olduğu için değiştirilemez
    transaction.amount = 200.00; // amount özelliği değiştirilebilir
    console.log(transaction.amount); // Çıktı: 200
    ```

**Örnekler: Bir `Book` sınıfının `title`, `author`, `pageCount` özelliklerinin tanımlanması ve kullanımı.**

Şimdi, bir kitap için özelliklerin nasıl tanımlanacağını ve kullanılacağını gösteren daha kapsamlı bir örnek oluşturalım.

```typescript
class Book {
    readonly isbn: string; // Kitabın ISBN numarası, değişmez olmalı
    title: string;          // Kitabın başlığı
    author: string;         // Kitabın yazarı
    pageCount: number;      // Kitaptaki sayfa sayısı
    isAvailable: boolean = true; // Kitabın kütüphanede olup olmadığını gösterir, varsayılan değeri true

    constructor(isbn: string, title: string, author: string, pageCount: number) {
        this.isbn = isbn;
        this.title = title;
        this.author = author;
        this.pageCount = pageCount;
        // isAvailable burada atanmadı, varsayılan değerini alacak
    }

    displayBookDetails(): void {
        console.log(`--- Book Details ---`);
        console.log(`ISBN: ${this.isbn}`);
        console.log(`Title: ${this.title}`);
        console.log(`Author: ${this.author}`);
        console.log(`Pages: ${this.pageCount}`);
        console.log(`Available: ${this.isAvailable ? "Yes" : "No"}`);
    }

    borrowBook(): void {
        if (this.isAvailable) {
            this.isAvailable = false;
            console.log(`'${this.title}' has been borrowed.`);
        } else {
            console.log(`'${this.title}' is currently not available.`);
        }
    }

    returnBook(): void {
        if (!this.isAvailable) {
            this.isAvailable = true;
            console.log(`'${this.title}' has been returned.`);
        } else {
            console.log(`'${this.title}' is already available.`);
        }
    }
}

const theGreatGatsby = new Book("978-0743273565", "The Great Gatsby", "F. Scott Fitzgerald", 180);
theGreatGatsby.displayBookDetails();
// Çıktı:
// --- Book Details ---
// ISBN: 978-0743273565
// Title: The Great Gatsby
// Author: F. Scott Fitzgerald
// Pages: 180
// Available: Yes

theGreatGatsby.borrowBook(); // Çıktı: 'The Great Gatsby' has been borrowed.
theGreatGatsby.displayBookDetails();
// Çıktı: Available: No

theGreatGatsby.borrowBook(); // Çıktı: 'The Great Gatsby' is currently not available.
theGreatGatsby.returnBook(); // Çıktı: 'The Great Gatsby' has been returned.
theGreatGatsby.displayBookDetails();
// Çıktı: Available: Yes

// theGreatGatsby.isbn = "some-new-isbn"; // Hata: 'isbn' salt okunur olduğu için değiştirilemez.
```

Bu örnekte, `Book` sınıfının `isbn` özelliği `readonly` olarak tanımlanmıştır, bu da onun bir kez kurucu içinde ayarlandıktan sonra değiştirilemez olmasını sağlar. `isAvailable` özelliği bir varsayılan değere sahiptir ve kitap ödünç alındığında veya iade edildiğinde değişir. Bu, özelliklerin bir nesnenin durumunu nasıl temsil ettiğini ve metotların bu durumu nasıl değiştirdiğini açıkça göstermektedir.

***

#### 2.3 Kurucu Metotlar (Constructors)

Bir sınıfın **kurucu metodu (constructor)**, o sınıftan yeni bir nesne (instance) oluşturulduğunda otomatik olarak çalışan özel bir metottur. Temel amacı, yeni oluşturulan nesnenin özelliklerini başlatmak ve nesnenin geçerli bir başlangıç durumuna sahip olmasını sağlamaktır.

**`constructor` Anahtar Kelimesi ve Nesne Oluşturma Süreci:**

TypeScript'te (ve JavaScript'in modern versiyonlarında) kurucu metot, `constructor` anahtar kelimesi kullanılarak tanımlanır. Bir sınıfın sadece bir kurucu metodu olabilir.

Bir nesne `new` anahtar kelimesi ile oluşturulduğunda, şu adımlar gerçekleşir:

1. Bellekte yeni, boş bir nesne oluşturulur.
2. Nesnenin prototipi (prototype) sınıfın prototipine bağlanır.
3. Sınıfın kurucu metodu çağrılır ve kendisine geçirilen argümanlar (`new ClassName(arg1, arg2...)`) bu kurucuya parametre olarak aktarılır.
4. Kurucu içinde `this` anahtar kelimesi, yeni oluşturulan boş nesneyi işaret eder. Bu sayede kurucu içinde `this.property = value;` şeklinde özelliklere başlangıç değerleri atanabilir.
5. Kurucu metot tamamlandıktan sonra, başlatılmış nesne `new` ifadesinin sonucu olarak geri döndürülür.

```typescript
class Dog {
    name: string;
    breed: string;

    // Kurucu metot
    constructor(name: string, breed: string) {
        this.name = name;   // Gelen name parametresini nesnenin name özelliğine ata
        this.breed = breed; // Gelen breed parametresini nesnenin breed özelliğine ata
        console.log(`${this.name} the ${this.breed} has been created!`);
    }

    bark(): void {
        console.log(`${this.name} says Woof!`);
    }
}

// Nesne oluşturma ve kurucu metodun otomatik çağrılması
const myDog = new Dog("Buddy", "Golden Retriever"); // Çıktı: Buddy the Golden Retriever has been created!
myDog.bark(); // Çıktı: Buddy says Woof!

const anotherDog = new Dog("Lucy", "Labrador"); // Çıktı: Lucy the Labrador has been created!
anotherDog.bark(); // Çıktı: Lucy says Woof!
```

Yukarıdaki örnekte, `Dog` sınıfının bir kurucusu vardır. `new Dog("Buddy", "Golden Retriever")` dediğimizde, `Buddy` ve `Golden Retriever` değerleri kurucuya parametre olarak geçer ve bu değerlerle `myDog` nesnesinin `name` ve `breed` özellikleri başlatılır.

**Parametreli Kurucular ve Parametre Özellikleri (Parameter Properties)**

Kurucu metotlara parametreler tanımlamak, nesne oluşturulurken gerekli verilerin dışarıdan sağlanmasını sağlar. TypeScript, bu konuda oldukça kullanışlı bir kısayol sunar: **Parametre Özellikleri (Parameter Properties)**.

Parametre özelliklerini kullanarak, kurucu parametrelerini doğrudan sınıf özellikleri olarak bildirebilir ve atamalarını otomatik olarak yapabilirsiniz. Bu, kod tekrarını azaltır ve sınıfın daha kısa olmasını sağlar.

Bir kurucu parametresinin önüne bir erişim belirleyici (`public`, `private`, `protected`) veya `readonly` eklediğinizde, TypeScript otomatik olarak şu işlemleri yapar:

1. Bu isimde bir sınıf özelliği oluşturur.
2. Bu özelliğe, kurucu parametresi olarak gelen değeri atar.

```typescript
class Bike {
    // name ve color otomatik olarak sınıf özelliği olarak tanımlanacak ve değer atanacak
    constructor(public name: string, private color: string, public readonly maxSpeed: number) {
        // Bu kurucu içinde manuel olarak this.name = name; gibi atama yapmaya gerek yok.
        // TypeScript bunu otomatik halleder.
        console.log(`${this.name} (${this.color}) with max speed ${this.maxSpeed} created.`);
    }

    displayColor(): void {
        console.log(`This bike is ${this.color}.`);
    }

    // getColor(): string {
    //     return this.color; // 'color' private olduğu için doğrudan erişilemez, ancak metot üzerinden erişilebilir
    // }
}

const mountainBike = new Bike("Mountain Bike", "red", 60);
// Çıktı: Mountain Bike (red) with max speed 60 created.

console.log(mountainBike.name);     // Çıktı: Mountain Bike (public olduğu için erişilebilir)
// console.log(mountainBike.color); // Hata: 'color' private olduğu için dışarıdan erişilemez

mountainBike.displayColor();        // Çıktı: This bike is red. (Metot sınıf içinde olduğu için private üyeye erişebilir)

console.log(mountainBike.maxSpeed); // Çıktı: 60
// mountainBike.maxSpeed = 70;      // Hata: 'maxSpeed' readonly olduğu için değiştirilemez
```

Bu örnekte, `name`, `color` ve `maxSpeed` doğrudan kurucu içinde tanımlanmıştır. `public`, `private` ve `readonly` belirleyicileri sayesinde hem özellik olarak tanımlanmış hem de gelen değerler atanmıştır. Bu yöntem, özellikle kurucu parametreleri ve sınıf özellikleri arasında birebir eşleşme olduğunda kodunuzu daha okunabilir ve kısa hale getirir.

**Örnekler: `Student` sınıfının `name`, `surname`, `studentId` ile başlatılması.**

Bir `Student` sınıfı oluşturalım ve öğrencileri ad, soyad ve öğrenci numarası ile başlatalım.

```typescript
class Student {
    // Özelliklerin kurucu metotta parametre özellikleri olarak tanımlanması
    constructor(
        public name: string,
        public surname: string,
        public readonly studentId: string
    ) {
        console.log(`Student ${this.name} ${this.surname} (ID: ${this.studentId}) registered.`);
    }

    get fullName(): string {
        return `${this.name} ${this.surname}`;
    }

    study(subject: string): void {
        console.log(`${this.fullName} is studying ${subject}.`);
    }
}

// Yeni öğrenci nesneleri oluşturma
const student1 = new Student("Ayşe", "Yılmaz", "S1001");
// Çıktı: Student Ayşe Yılmaz (ID: S1001) registered.

const student2 = new Student("Can", "Demir", "S1002");
// Çıktı: Student Can Demir (ID: S1002) registered.

console.log(student1.fullName); // Çıktı: Ayşe Yılmaz
student2.study("Mathematics"); // Çıktı: Can Demir is studying Mathematics.

// student1.studentId = "S9999"; // Hata: studentId readonly olduğu için değiştirilemez
```

Bu örnek, parametre özelliklerinin ne kadar kullanışlı olduğunu net bir şekilde gösteriyor. `name`, `surname` ve `studentId` doğrudan kurucuya parametre olarak geçirilerek hem sınıf özellikleri olarak tanımlanmış hem de değerleri atanmıştır. `studentId`'nin `readonly` olması, öğrenci numarasının nesne oluşturulduktan sonra değişmemesini garanti eder.

***

#### 2.4 Metotlar (Methods)

Metotlar, bir sınıfın nesnelerinin gerçekleştirebileceği eylemleri veya davranışları tanımlayan fonksiyonlardır. Bir nesnenin özelliklerini değiştirebilir, başka nesnelerle etkileşime girebilir veya bir hesaplama yapıp sonuç döndürebilirler.

**Metot Tanımlama, Çağırma ve `this` Anahtar Kelimesi:**

Metotlar, sınıfın içinde normal fonksiyonlar gibi tanımlanır, ancak `function` anahtar kelimesi kullanılmaz. Metodun adı, parametreleri (varsa) ve geri dönüş tipi (varsa) belirtilir.

```typescript
class Calculator {
    add(a: number, b: number): number {
        return a + b;
    }

    subtract(a: number, b: number): number {
        return a - b;
    }
}

const calc = new Calculator();
console.log(calc.add(5, 3));      // Çıktı: 8
console.log(calc.subtract(10, 4)); // Çıktı: 6
```

Metotlar genellikle nesnenin kendi özelliklerine erişmek veya değiştirmek için **`this`** anahtar kelimesini kullanır. `this`, metodun çağrıldığı **o anki nesneyi** temsil eder.

```typescript
class Timer {
    seconds: number = 0;

    start(): void {
        console.log("Timer started.");
        // Her saniye 'seconds' özelliğini artıracak bir interval başlatma
        // Arrow fonksiyonu kullanmak 'this' bağlamını korur.
        setInterval(() => {
            this.seconds++;
            console.log(`Seconds passed: ${this.seconds}`);
        }, 1000);
    }

    reset(): void {
        this.seconds = 0;
        console.log("Timer reset.");
    }
}

const myTimer = new Timer();
myTimer.start(); // Başlangıçta 0'dan başlayıp her saniye artar
// Birkaç saniye sonra myTimer.reset(); çağrılabilir.
```

Yukarıdaki `Timer` sınıfında, `start()` metodu `this.seconds`'a erişerek nesnenin kendi `seconds` özelliğini günceller. `setInterval` içindeki ok (arrow) fonksiyonu kullanımı önemlidir, çünkü normal bir fonksiyon kullanılsaydı `this` bağlamı kaybedilirdi. Ok fonksiyonları, `this` bağlamını tanımlandıkları yerdeki üst kapsamdan alır.

**Metot Parametreleri ve Geri Dönüş Tipleri:**

Metotlar, dışarıdan bilgi almak için parametreler kabul edebilirler. Bu parametrelerin tipleri ve metodun döndürdüğü değerin tipi de açıkça belirtilmelidir. Eğer metot bir değer döndürmüyorsa, geri dönüş tipi `void` olarak belirtilir.

TypeScript

```typescript
class Greeter {
    greeting: string;

    constructor(message: string) {
        this.greeting = message;
    }

    // Parametre olarak isim alan ve string döndüren metot
    greet(name: string): string {
        return `${this.greeting}, ${name}!`;
    }

    // Parametre almayan ve void döndüren metot
    sayHello(): void {
        console.log("Hello there!");
    }
}

const myGreeter = new Greeter("Merhaba");
console.log(myGreeter.greet("Dünya")); // Çıktı: Merhaba, Dünya!
myGreeter.sayHello(); // Çıktı: Hello there!
```

**Örnekler: Bir `Account` sınıfının `deposit()`, `withdraw()`, `getBalance()` metotları.**

Şimdi, bir banka hesabını temsil eden bir `Account` sınıfı oluşturalım ve para yatırma, çekme ve bakiye sorgulama gibi işlemleri gerçekleştiren metotlar ekleyelim.

```typescript
class Account {
    private _balance: number; // Kapsülleme için private özellik

    constructor(initialBalance: number) {
        if (initialBalance < 0) {
            throw new Error("Initial balance cannot be negative.");
        }
        this._balance = initialBalance;
    }

    // Bakiye bilgisini döndüren getter metodu
    getBalance(): number {
        return this._balance;
    }

    // Hesaba para yatırma metodu
    deposit(amount: number): void {
        if (amount <= 0) {
            console.error("Deposit amount must be positive.");
            return;
        }
        this._balance += amount;
        console.log(`Deposited $${amount}. New balance: $${this._balance}`);
    }

    // Hesaptan para çekme metodu
    withdraw(amount: number): void {
        if (amount <= 0) {
            console.error("Withdrawal amount must be positive.");
            return;
        }
        if (this._balance < amount) {
            console.error(`Insufficient funds. Current balance: $${this._balance}`);
            return;
        }
        this._balance -= amount;
        console.log(`Withdrew $${amount}. New balance: $${this._balance}`);
    }
}

const checkingAccount = new Account(500);
console.log(`Current balance: $${checkingAccount.getBalance()}`); // Çıktı: Current balance: $500

checkingAccount.deposit(200);   // Çıktı: Deposited $200. New balance: $700
checkingAccount.withdraw(100);  // Çıktı: Withdrew $100. New balance: $600
checkingAccount.withdraw(1000); // Çıktı: Insufficient funds. Current balance: $600
checkingAccount.deposit(-50);   // Çıktı: Deposit amount must be positive.
```

Bu `Account` sınıfı örneğinde:

* `_balance` özelliği, doğrudan dışarıdan erişilememesi için `private` olarak tanımlanmıştır. Bu, **kapsülleme** prensibinin bir ön gösterimidir.
* `getBalance()` metodu, `_balance` değerini dış dünyaya güvenli bir şekilde sunar.
* `deposit()` ve `withdraw()` metotları, `_balance` üzerinde işlem yapmadan önce gelen `amount` değerini doğrular. Bu doğrulama, nesnenin iç durumunun (bakiyenin) tutarlı kalmasını sağlar ve yanlış kullanımları engeller.

Metotlar, bir nesnenin temel işlevselliğini tanımlayan ana bileşenlerdir. İyi tasarlanmış metotlar, sınıfın sorumluluklarını netleştirir ve kodun modülerliğini artırır.

***

#### 2.5 Erişim Belirleyiciler (Access Modifiers)

Erişim belirleyiciler (Access Modifiers), bir sınıfın üyelerinin (özellikler ve metotlar) sınıf dışından nasıl erişilebileceğini kontrol eden anahtar kelimelerdir. TypeScript'te üç ana erişim belirleyici bulunur: `public`, `private` ve `protected`. Bu belirleyiciler, **kapsülleme** prensibinin temelini oluşturur ve kodun güvenliğini, bakım kolaylığını ve anlaşılırlığını artırır.

**`public`:** Varsayılan Erişim.

* `public` olarak işaretlenen üyeler, sınıf içinden, sınıf dışından ve miras alan alt sınıflardan **her yerden** erişilebilir.
* Eğer bir üyeye herhangi bir erişim belirleyici atanmazsa, TypeScript varsayılan olarak onu `public` kabul eder. Ancak açıkça `public` yazmak, kodun niyetini daha net ifade eder.

```typescript
class Dog {
    public name: string; // Her yerden erişilebilir
    public breed: string; // Her yerden erişilebilir

    constructor(name: string, breed: string) {
        this.name = name;
        this.breed = breed;
    }

    public bark(): void { // Her yerden çağrılabilir
        console.log(`${this.name} barks!`);
    }
}

const myDog = new Dog("Rex", "German Shepherd");
console.log(myDog.name); // Çıktı: Rex (Public olduğu için erişim var)
myDog.bark();            // Çıktı: Rex barks! (Public olduğu için çağrı var)
```

**`private`:** Sadece sınıf içinden erişim. Kapsülleme prensibi.

* `private` olarak işaretlenen üyeler, sadece tanımlandıkları sınıfın içinden erişilebilir. Sınıf dışından veya miras alan alt sınıflardan bu üyelere **erişilemez**.
* Bu belirleyici, bir nesnenin iç çalışma detaylarını gizlemek ve verinin dışarıdan kontrolsüzce değiştirilmesini engellemek için kullanılır. Bu, **kapsülleme** prensibinin doğrudan uygulamasıdır.

```typescript
class Car {
    private speed: number = 0; // Sadece Car sınıfı içinden erişilebilir
    public model: string;

    constructor(model: string) {
        this.model = model;
    }

    private checkSpeedLimit(currentSpeed: number): boolean {
        // Bu metot sadece sınıfın kendi içinde yardımcı bir işlev görür
        return currentSpeed < 120;
    }

    accelerate(amount: number): void {
        this.speed += amount;
        if (!this.checkSpeedLimit(this.speed)) { // Private metodu sınıf içinden çağırma
            console.log("Warning: Speed limit exceeded!");
        }
        console.log(`${this.model} is accelerating. Current speed: ${this.speed} km/h`);
    }

    getCurrentSpeed(): number {
        return this.speed; // Private özelliğe sınıf içinden erişim ve dışarıya güvenli sunum
    }
}

const myCar = new Car("Tesla Model 3");
// console.log(myCar.speed); // Hata: 'speed' private olduğu için erişilemez
// myCar.checkSpeedLimit(100); // Hata: 'checkSpeedLimit' private olduğu için çağrılamaz

myCar.accelerate(50); // Çıktı: Tesla Model 3 is accelerating. Current speed: 50 km/h
myCar.accelerate(80); // Çıktı: Warning: Speed limit exceeded! Tesla Model 3 is accelerating. Current speed: 130 km/h
console.log(`Car's speed: ${myCar.getCurrentSpeed()} km/h`); // Çıktı: Car's speed: 130 km/h
```

**`protected`:** Sınıf içinden ve miras alan alt sınıflardan erişim.

* `protected` olarak işaretlenen üyeler, tanımlandıkları sınıfın içinden ve bu sınıfı **miras alan (türetilmiş) alt sınıfların** içinden erişilebilir. Sınıf dışından doğrudan erişilemezler.
* Bu, miras hiyerarşisi içindeki sınıfların birbirlerinin belirli dahili durumlarına kontrollü bir şekilde erişmesini sağlarken, dış dünyadan bu detayları gizlemeye devam eder.

```typescript
class Vehicle {
    protected speed: number; // Sadece Vehicle sınıfı ve alt sınıflarından erişilebilir

    constructor(initialSpeed: number = 0) {
        this.speed = initialSpeed;
    }

    protected getInfo(): string { // Alt sınıflar tarafından erişilebilir metot
        return `Current speed: ${this.speed} km/h`;
    }
}

class Motorcycle extends Vehicle {
    public brand: string;

    constructor(brand: string, initialSpeed: number) {
        super(initialSpeed); // Üst sınıfın kurucusunu çağırma
        this.brand = brand;
    }

    accelerate(amount: number): void {
        this.speed += amount; // Protected 'speed' özelliğine alt sınıftan erişim
        console.log(`${this.brand} motorcycle accelerates. ${this.getInfo()}`); // Protected metodu alt sınıftan çağırma
    }
}

const honda = new Motorcycle("Honda", 20);
// console.log(honda.speed); // Hata: 'speed' protected olduğu için erişilemez
// honda.getInfo(); // Hata: 'getInfo' protected olduğu için dışarıdan çağrılamaz

honda.accelerate(40); // Çıktı: Honda motorcycle accelerates. Current speed: 60 km/h
```

**Erişim belirleyicilerin OOP'deki rolü ve güvenlik açısından önemi.**

Erişim belirleyiciler, OOP'deki en önemli prensiplerden biri olan **kapsüllemenin (encapsulation)** doğrudan bir uygulamasıdır. Temel rolleri ve önemi şunlardır:

1. **Bilgi Gizleme (Information Hiding):** Bir nesnenin dahili uygulama detaylarını dış dünyadan gizlerler. Kullanıcıların sadece nesnenin ne yaptığını bilmesi yeterlidir, nasıl yaptığını bilmeleri gerekmez. Bu, API'nin (arayüzün) temiz kalmasını sağlar.
2. **Veri Bütünlüğü (Data Integrity):** Özellikle `private` belirleyici, bir nesnenin iç durumunun (yani özelliklerinin) dışarıdan rastgele veya hatalı bir şekilde değiştirilmesini engeller. Veriye sadece sınıfın kendi metotları aracılığıyla (getter/setter gibi) kontrollü bir şekilde erişilmesini ve değiştirilmesini sağlarız.
3. **Bakım Kolaylığı (Maintainability):** Dahili bir özelliğin veya metodun implementasyonunu değiştirdiğinizde, `private` veya `protected` ise, bu değişiklik dışarıdaki veya miras almayan sınıfları etkilemez. Bu, kodun bakımını ve refactoring'ini çok daha güvenli hale getirir.
4. **Güvenlik:** Hassas verilerin veya kritik işlemlerin yanlışlıkla veya kötü niyetle dışarıdan erişilmesini ve kullanılmasını önler.
5. **Kodun Anlaşılırlığı ve Okunabilirliği:** Bir sınıfın API'sini (yani public üyelerini) inceleyen bir geliştirici, sınıfın ne işe yaradığını ve nasıl kullanılacağını kolayca anlayabilir. Gizlenen private ve protected üyeler, sınıfın dahili çalışma prensipleri olarak kalır ve dışarıdaki karmaşıklığı azaltır.

Erişim belirleyicilerin doğru kullanımı, daha robust (sağlam), daha güvenli ve daha kolay yönetilebilir yazılım sistemleri oluşturmanın anahtarlarından biridir.

***

#### 2.6 Getters ve Setters (Accessor Properties)

**Getters (alıcılar)** ve **Setters (atayıcılar)**, sınıf özelliklerine erişimi ve bu özellikleri değiştirmeyi kontrol etmek için kullanılan özel metotlardır. Bunlar, TypeScript'te (ve ES6+ JavaScript'te) **accessor properties** olarak bilinir ve özelliklere doğrudan erişmek yerine dolaylı bir arayüz sağlarlar. Özellikle bir özelliğin değerini okurken veya yazarken belirli bir mantık (doğrulama, formatlama, dönüştürme vb.) uygulamak istediğimizde çok kullanışlıdırlar.

**Özelliklere Erişimi Kontrol Etme ve Veri Doğrulama:**

Getters ve setters, **kapsülleme** prensibini desteklemenin önemli bir yoludur. Özel (private) bir özelliğe doğrudan erişimi engellerken, bu özelliğin değerinin okunması ve değiştirilmesi üzerinde tam kontrol sahibi olmamızı sağlarlar.

* **Getter (`get` anahtar kelimesi):** Bir özelliğin değerini okumak (almak) için kullanılır.
  * Bir getter metodu tanımladığınızda, o özelliğe normal bir özellik gibi erişebilirsiniz (parantez kullanmadan).
  * Getter'lar genellikle herhangi bir parametre almaz ve bir değer döndürür.
* **Setter (`set` anahtar kelimesi):** Bir özelliğin değerini yazmak (atamak) için kullanılır.
  * Bir setter metodu tanımladığınızda, o özelliğe normal bir özellik gibi değer atayabilirsiniz.
  * Setter'lar genellikle bir parametre alır (atanacak değer) ve bir değer döndürmez (void).

**Örnekler: Bir `Temperature` sınıfının Celsius ve Fahrenheit dönüşümleri için getter/setter kullanımı.**

Bir `Temperature` sınıfı düşünelim. Dahili olarak sıcaklığı Celsius cinsinden tutmak isteyebiliriz, ancak kullanıcının Fahrenheit cinsinden sıcaklığı okuyup ayarlamasını da sağlamak isteyebiliriz.

```typescript
class Temperature {
    private _celsius: number; // Dahili olarak Celsius değerini tutuyoruz

    constructor(celsius: number) {
        this._celsius = celsius;
    }

    // Celsius değerini okumak için getter
    get celsius(): number {
        return this._celsius;
    }

    // Celsius değerini ayarlamak için setter (isteğe bağlı, bu örnekte doğrudan kurucu ile ayarlanıyor)
    set celsius(newCelsius: number) {
        if (newCelsius < -273.15) { // Mutlak sıfır kontrolü
            throw new Error("Temperature cannot be below absolute zero (-273.15 °C).");
        }
        this._celsius = newCelsius;
    }

    // Fahrenheit değerini hesaplayıp döndüren getter
    get fahrenheit(): number {
        return (this._celsius * 9 / 5) + 32;
    }

    // Fahrenheit değerini alıp Celsius'a dönüştürüp ayarlayan setter
    set fahrenheit(newFahrenheit: number) {
        const newCelsius = (newFahrenheit - 32) * 5 / 9;
        if (newCelsius < -273.15) { // Mutlak sıfır kontrolü
            throw new Error("Temperature cannot be below absolute zero (-273.15 °C).");
        }
        this._celsius = newCelsius;
        console.log(`Fahrenheit set to ${newFahrenheit} °F. Celsius equivalent: ${this._celsius.toFixed(2)} °C`);
    }
}

const temp = new Temperature(25); // 25°C ile başlat

console.log(`Initial Celsius: ${temp.celsius} °C`);     // Getter çağrısı: Initial Celsius: 25 °C
console.log(`Initial Fahrenheit: ${temp.fahrenheit} °F`); // Getter çağrısı: Initial Fahrenheit: 77 °F

temp.fahrenheit = 98.6; // Setter çağrısı: Fahrenheit set to 98.6 °F. Celsius equivalent: 37.00 °C
console.log(`Updated Celsius: ${temp.celsius} °C`);       // Updated Celsius: 37 °C

try {
    temp.celsius = -300; // Setter ile hata fırlatma örneği
} catch (error: any) {
    console.error(error.message); // Çıktı: Temperature cannot be below absolute zero (-273.15 °C).
}
```

Bu örnekte:

* `_celsius` özel (private) bir özelliktir ve doğrudan dışarıdan erişilemez.
* `get celsius()` bir getter'dır; `temp.celsius` dediğimizde bu metot çalışır ve `_celsius` değerini döndürür.
* `set celsius()` bir setter'dır; `temp.celsius = ...` dediğimizde bu metot çalışır ve mutlak sıfır kontrolü yaparak `_celsius` değerini günceller.
* `get fahrenheit()` bir getter'dır; `temp.fahrenheit` dediğimizde Celsius değerini Fahrenheit'a dönüştürerek döndürür.
* `set fahrenheit()` bir setter'dır; `temp.fahrenheit = ...` dediğimizde gelen Fahrenheit değerini Celsius'a dönüştürür ve mutlak sıfır kontrolü yaparak `_celsius` değerini ayarlar.

**Getters ve Setters Neden Kullanılır?**

1. **Veri Doğrulama (Data Validation):** Bir özelliğe değer atanmadan önce koşulları kontrol etmenizi sağlar (örneğin, yaşın negatif olmaması, sıcaklığın mutlak sıfırın altına düşmemesi).
2. **Hesaplamalı Özellikler:** Bir özelliğin değeri, başka bir veya daha fazla özelliğin değerine bağlı olarak hesaplandığında kullanışlıdır (örneğin, tam adın ad ve soyaddan türetilmesi, kârın gelir ve maliyetten türetilmesi).
3. **Yan Etkiler (Side Effects):** Bir özelliğin değeri değiştiğinde ek işlemler yapmak istediğinizde (örneğin, bir veritabanını güncellemek, bir UI bileşenini yeniden çizmek, loglama yapmak).
4. **Kapsülleme (Encapsulation):** Nesnenin iç temsilini dış arayüzden soyutlamaya yardımcı olurlar. Dış dünya, bir özelliğe doğrudan eriştiğini düşünürken, aslında getter/setter metotları aracılığıyla kontrollü bir şekilde etkileşimde bulunur. Bu, sınıfın dahili yapısını değiştirdiğinizde (örneğin, Celsius yerine Kelvin cinsinden depolamaya geçtiğinizde) dışarıdaki kodu etkilemeden bunu yapmanızı sağlar.
5. **Debugging Kolaylığı:** Bir özelliğin ne zaman ve nasıl değiştiğini izlemek için setter'lara breakpoint'ler koyabilirsiniz.

Getters ve setters, kodunuzu daha esnek, güvenli ve bakımı kolay hale getiren güçlü araçlardır.

***

#### 2.7 Statik Üyeler (Static Members)

Sınıfların bir diğer önemli üye türü **statik üyelerdir**. Normal özellikler ve metotlar bir sınıfın belirli bir nesnesine (instance) aitken, statik üyeler sınıfın kendisine aittir. Bu, bir statik üyeye erişmek için sınıfın bir nesnesini oluşturmanıza gerek olmadığı anlamına gelir. Doğrudan sınıf adı üzerinden erişilirler.

**Statik Özellikler ve Statik Metotlar:**

* **Statik Özellikler:** Bir sınıfın tüm nesneleri arasında paylaşılan, ortak bir veri tutmak istediğinizde kullanışlıdır. Genellikle `const` gibi sabit değerler veya bir nesne sayacı gibi durum bilgisi için kullanılırlar.
* **Statik Metotlar:** Sınıfın herhangi bir nesnesinin durumuna (non-static özelliklerine) bağlı olmayan işlevsellik sağlamak için kullanılırlar. Yardımcı fonksiyonlar, fabrika metotları veya genel hesaplamalar için idealdirler.

Statik üyeler tanımlamak için `static` anahtar kelimesi kullanılır.

```typescript
class MyClass {
    // Statik özellik: Sınıfın tüm örnekleri tarafından paylaşılır
    static instanceCount: number = 0;

    // Normal (non-static) özellik: Her örnek kendi değerine sahiptir
    id: number;

    constructor() {
        this.id = ++MyClass.instanceCount; // Statik özelliğe sınıf adı üzerinden erişim
    }

    // Normal (non-static) metot: Bir örnek üzerinden çağrılır
    displayId(): void {
        console.log(`Instance ID: ${this.id}`);
    }

    // Statik metot: Sınıf adı üzerinden çağrılır, 'this' bir örneğe referans vermez
    static getNextId(): number {
        return MyClass.instanceCount + 1; // Başka bir statik özelliğe erişim
    }
}

console.log(`Next ID will be: ${MyClass.getNextId()}`); // Statik metodu sınıf adı üzerinden çağırma
// Çıktı: Next ID will be: 1

const obj1 = new MyClass(); // constructor çağrılır, instanceCount 1 olur
obj1.displayId();           // Çıktı: Instance ID: 1

const obj2 = new MyClass(); // constructor çağrılır, instanceCount 2 olur
obj2.displayId();           // Çıktı: Instance ID: 2

console.log(`Total instances created: ${MyClass.instanceCount}`); // Statik özelliğe sınıf adı üzerinden erişim
// Çıktı: Total instances created: 2

// console.log(obj1.instanceCount); // Hata: Statik özellik bir örnek üzerinden doğrudan erişilemez
// obj1.getNextId(); // Hata: Statik metot bir örnek üzerinden doğrudan çağrılamaz
```

Bu örnekte:

* `instanceCount` bir **statik özellik**tir. Her yeni `MyClass` nesnesi oluşturulduğunda bu değeri artırırız, böylece kaç nesne oluşturulduğunu takip edebiliriz. Bu değer, tüm `MyClass` nesneleri arasında paylaşılır.
* `getNextId()` bir **statik metot**tur. Henüz bir nesne oluşturulmamış olsa bile sınıf adı `MyClass.getNextId()` üzerinden çağrılabilir. Nesneye özel bir duruma (non-static özelliklere) ihtiyacı olmadığı için statik olması mantıklıdır.

**Yardımcı Metotlar ve Sabit Değerler için Kullanımı:**

Statik üyeler, özellikle yardımcı (utility) fonksiyonlar veya uygulama genelinde sabit kalacak değerler (sabitler) için çok yaygın olarak kullanılır.

**Örnekler: `Math` sınıfına benzer bir `UtilityFunctions` sınıfı, sayaç uygulamaları.**

**Örnek 1: `Math` Sınıfı Benzeri Yardımcı Fonksiyonlar**

JavaScript'teki `Math` objesi gibi, statik metotlar içeren bir `Calculator` veya `StringHelper` sınıfı oluşturabiliriz. Bu metotlar, herhangi bir `Calculator` veya `StringHelper` nesnesi oluşturmaya gerek kalmadan doğrudan sınıf üzerinden kullanılabilir.

```typescript
class Calculator {
    // PI sayısı, tüm hesaplamalarda ortak bir sabit olarak kullanılabilir
    static readonly PI: number = 3.14159;

    // İki sayıyı toplayan statik metot
    static add(a: number, b: number): number {
        return a + b;
    }

    // Bir sayının karesini alan statik metot
    static square(num: number): number {
        return num * num;
    }

    // Bir dairenin alanını hesaplayan statik metot
    static calculateCircleArea(radius: number): number {
        return Calculator.PI * Calculator.square(radius); // Statik özelliği ve metodu kullanma
    }
}

// Sınıfın bir nesnesini oluşturmaya gerek kalmadan statik metotları ve özellikleri kullanma
console.log(Calculator.add(10, 5));             // Çıktı: 15
console.log(Calculator.square(7));              // Çıktı: 49
console.log(Calculator.PI);                     // Çıktı: 3.14159
console.log(Calculator.calculateCircleArea(5)); // Çıktı: 78.53975
```

Bu `Calculator` sınıfı, matematiksel işlemler için bir "araç kutusu" görevi görür. Fonksiyonları, herhangi bir `Calculator` nesnesi oluşturmadan doğrudan kullanılabilir, çünkü bu işlemlerin bir `Calculator` nesnesinin belirli bir durumuyla (state) bir ilgisi yoktur.

**Örnek 2: ID Sayaç Uygulaması (Tekil ID Üretimi)**

Bir sistemde benzersiz ID'ler atamak için statik bir sayaç kullanmak yaygın bir senaryodur.

```typescript
class UniqueIdGenerator {
    private static _nextId: number = 1; // Statik ve özel bir sayaç

    // Statik metot: Yeni ve benzersiz bir ID üretir
    static generateId(): string {
        const newId = `ID-${UniqueIdGenerator._nextId.toString().padStart(5, '0')}`;
        UniqueIdGenerator._nextId++; // Sayacı bir sonraki ID için artır
        return newId;
    }

    // Statik getter (isteğe bağlı): Mevcut sayaç değerini gösterir (yalnızca bilgilendirme amaçlı)
    static get currentId(): number {
        return UniqueIdGenerator._nextId - 1;
    }
}

// ID'ler oluşturma
console.log(UniqueIdGenerator.generateId()); // Çıktı: ID-00001
console.log(UniqueIdGenerator.generateId()); // Çıktı: ID-00002
console.log(UniqueIdGenerator.generateId()); // Çıktı: ID-00003

console.log(`Currently generated IDs count: ${UniqueIdGenerator.currentId}`); // Çıktı: Currently generated IDs count: 3

// Başka bir yerden ID oluşturma
const anotherId = UniqueIdGenerator.generateId();
console.log(anotherId); // Çıktı: ID-00004
```

Bu örnekte, `UniqueIdGenerator` sınıfı hiçbir zaman örneklendirilmez. `generateId()` statik metodu, her çağrıldığında artan ve benzersiz bir ID üretir. `_nextId` özelliği `private static` olduğu için, dışarıdan doğrudan değiştirilemez, bu da ID üretme mekanizmasının güvenliğini sağlar.

**Statik Üyelerin Kullanım Alanları:**

* **Utility (Yardımcı) Sınıflar:** `Math` gibi, belirli bir alandaki genel işlevleri gruplamak için.
* **Sabitler:** Uygulama genelinde sabit kalacak değerleri tutmak için (örneğin, API URL'leri, konfigürasyon değerleri).
* **Fabrika Metotları:** Bir sınıfın nesnelerini belirli koşullara göre oluşturan metotlar (örneğin, `Connection.createDatabaseConnection()`).
* **Tekil Sınıflar (Singleton Pattern):** Bir sınıftan sadece bir tane nesne oluşmasını garanti etmek istediğinizde (ilerleyen konularda ele alınabilir).
* **Sayaçlar veya Paylaşılan Durum:** Tüm nesneler arasında ortak bir sayacı veya durumu izlemek için.

Statik üyeler, kodunuzu daha organize, işlevsel olarak gruplanmış ve bazen daha performanslı hale getirebilir, çünkü nesne oluşturma maliyetinden kaçınılmış olur. Ancak dikkatli kullanılmalı ve nesneye özel bir duruma ihtiyaç duyan işlevsellik için tercih edilmemelidirler.
