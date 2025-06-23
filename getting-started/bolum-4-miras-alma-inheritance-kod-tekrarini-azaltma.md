# Bölüm 4: Miras Alma (Inheritance) - Kod Tekrarını Azaltma

Nesne Yönelimli Programlama'nın (OOP) dört temel prensibinden ikincisi **miras alma (inheritance)**'dır. Miras alma, mevcut bir sınıfın (üst sınıf, ana sınıf veya süper sınıf) özelliklerini ve metotlarını yeni bir sınıfa (alt sınıf, türetilmiş sınıf veya alt sınıf) devralmasını sağlayan bir mekanizmadır. Bu prensip, kod tekrarını azaltır, kodun yeniden kullanılabilirliğini artırır ve sınıflar arasında hiyerarşik bir ilişki kurarak yazılım tasarımını daha düzenli hale getirir.

Gerçek dünyadan bir örnek vermek gerekirse, "Motorlu Araç" genel bir kavramdır. "Araba", "Kamyon" ve "Motosiklet" ise motorlu araçların özel türleridir. Tüm motorlu araçların ortak özellikleri (motor hacmi, tekerlek sayısı, hızlanma yeteneği) ve davranışları (çalıştırma, durdurma, hızlanma) vardır. Araba, kamyon ve motosiklet bu ortak özellikleri ve davranışları devralır, ancak kendilerine özgü ek özelliklere (örneğin, arabanın yolcu kapasitesi, kamyonun taşıma kapasitesi) ve davranışlara da sahip olabilirler. Miras alma, bu tür "bir X bir Y'dir" ("a Car IS A Vehicle") ilişkilerini yazılımda modellememizi sağlar.

***

#### 4.1 Miras Alma Nedir? Neden Kullanılır?

**Miras alma**, bir sınıfın (türetilmiş sınıf) başka bir sınıfın (temel sınıf) işlevselliğini, yani özelliklerini ve metotlarını **genişletmesine** veya **yeniden kullanmasına** olanak tanıyan bir OOP mekanizmasıdır. Temel sınıf, daha genel bir kavramı temsil ederken, türetilmiş sınıf bu genel kavramın daha spesifik bir versiyonunu temsil eder.

TypeScript'te miras alma, `extends` anahtar kelimesi kullanılarak gerçekleştirilir.

```typescript
// Temel Sınıf (Base Class / Parent Class)
class Animal {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    move(distance: number = 0): void {
        console.log(`${this.name} moved ${distance}m.`);
    }
}

// Türetilmiş Sınıf (Derived Class / Child Class) - Animal sınıfından miras alıyor
class Dog extends Animal {
    breed: string;

    constructor(name: string, breed: string) {
        super(name); // Üst sınıfın constructor'ını çağırma
        this.breed = breed;
    }

    bark(): void {
        console.log(`${this.name} (${this.breed}) says Woof!`);
    }

    // Üst sınıftaki metodu ezme (override)
    move(distance: number = 5): void {
        console.log(`${this.name} (${this.breed}) is running ${distance}m.`);
    }
}

const myAnimal = new Animal("Generic Animal");
myAnimal.move(10); // Çıktı: Generic Animal moved 10m.

const myDog = new Dog("Buddy", "Golden Retriever");
myDog.bark();       // Çıktı: Buddy (Golden Retriever) says Woof!
myDog.move();       // Çıktı: Buddy (Golden Retriever) is running 5m. (ezilmiş metot)
myDog.move(20);     // Çıktı: Buddy (Golden Retriever) is running 20m. (ezilmiş metot)
console.log(myDog.name); // Türetilmiş sınıf üst sınıftan 'name' özelliğini miras aldı
```

Yukarıdaki örnekte:

* `Animal` sınıfı genel bir hayvanı temsil eder. `name` özelliğine ve `move` metoduna sahiptir.
* `Dog` sınıfı, `Animal` sınıfından `extends` anahtar kelimesiyle miras alır. Bu, `Dog` sınıfının `Animal` sınıfının tüm `public` ve `protected` özelliklerini ve metotlarını devraldığı anlamına gelir.
* `Dog` sınıfı ayrıca kendi özel özelliği olan `breed` ve kendi özel metodu olan `bark()`'a sahiptir.
* `super(name);` çağrısı, türetilmiş sınıfın kurucusu içinde üst sınıfın kurucusunu çağırmak için kullanılır. Bu, üst sınıfın özelliklerinin doğru şekilde başlatılmasını sağlar. Türetilmiş bir sınıfın kurucusu varsa, `super()` çağrısını yapmak zorunludur ve ilk ifade olmak zorundadır.
* `Dog` sınıfı, `move()` metodunu kendine özgü bir şekilde yeniden tanımlamıştır (ezmiştir). Bu sayede, bir `Dog` nesnesinin `move()` metodu çağrıldığında `Animal` sınıfının `move()` metodu yerine `Dog` sınıfının `move()` metodu çalışır.

**Miras Almanın Avantajları (Neden Kullanılır?):**

1. **Kod Tekrarını Azaltma (Code Reusability):** En büyük faydası budur. Ortak özellikler ve metotlar bir temel sınıfta tanımlanır ve birden fazla alt sınıf tarafından yeniden kullanılır. Bu, aynı kodu farklı yerlerde tekrar tekrar yazma ihtiyacını ortadan kaldırır.
2. **Modülerlik ve Genişletilebilirlik:** Mevcut işlevselliği değiştirmeden yeni özellikler veya davranışlar eklemeyi kolaylaştırır. Yeni bir alt sınıf eklemek, mevcut sınıfları bozmadan sistemi genişletmenin temiz bir yoludur.
3. **Hiyerarşik Yapı ve Sınıflandırma:** Gerçek dünyadaki hiyerarşik ilişkileri (örneğin, araçlar, hayvanlar, şekiller) yazılımda modellememizi sağlar. Bu, kodun daha düzenli ve anlaşılır olmasını sağlar.
4. **Polimorfizm Desteği:** Miras alma, OOP'nin bir diğer ana prensibi olan polimorfizm (çok biçimlilik) için temel oluşturur. Farklı türdeki nesneleri aynı arayüz üzerinden işleyebilmemizi sağlar (ilerleyen bölümlerde daha detaylı).
5. **Bakım Kolaylığı:** Ortak işlevsellik temel sınıfta toplandığı için, bu ortak işlevsellikte yapılan bir değişiklik sadece tek bir yerde (temel sınıfta) yapılır ve bu değişikliği devralan tüm alt sınıflara yansır.

**Miras Almanın Dezavantajları (Ne Zaman Dikkatli Olunmalı?):**

* **Sıkı Bağlılık (Tight Coupling):** Temel sınıf ve türetilmiş sınıflar arasında sıkı bir bağımlılık (coupling) yaratır. Temel sınıfta yapılan radikal değişiklikler, tüm alt sınıfları etkileyebilir ve "kırılmasına" neden olabilir.
* **"Elmas Problemi" (Diamond Problem):** Bazı dillerde (C++ gibi) çoklu miras (bir sınıfın birden fazla sınıftan miras alması) mümkün olduğunda ortaya çıkan bir karmaşıklıktır. TypeScript (ve Java, C#) bunu arayüzler (interfaces) aracılığıyla çözer. TypeScript'te bir sınıf sadece tek bir sınıftan miras alabilir.
* **Hiyerarşi Karmaşıklığı:** Çok derin veya çok geniş miras hiyerarşileri, sistemin anlaşılmasını ve bakımını zorlaştırabilir. "Kompozisyon miras almaya tercih edilir" ("Prefer composition over inheritance") prensibi, bu karmaşıklığı azaltmak için sıkça önerilir.

Miras alma, özellikle "bir X bir Y'dir" ilişkilerini modellemek için güçlü bir araçtır. Doğru kullanıldığında kod tekrarını önemli ölçüde azaltır ve sistemin modülerliğini artırır.

***

#### 4.2 Temel Sınıf ve Türetilmiş Sınıf Oluşturma

Miras alma ilişkisini kurmak için, bir **temel sınıf (base class)** ve bu sınıftan türeyecek **türetilmiş sınıf (derived class)** tanımlamamız gerekir. TypeScript'te bu ilişki `extends` anahtar kelimesiyle kurulur.

**Söz Dizimi:**

```typescript
class BaseClass {
    // Özellikler ve metotlar
}

class DerivedClass extends BaseClass {
    // Ek özellikler ve metotlar
    // Üst sınıftan gelenleri ezebilir (override)
}
```

**Örnek: Şekiller Hiyerarşisi**

Geometrik şekiller üzerine bir örnekle miras almayı daha iyi anlayalım. Genel bir `Shape` (Şekil) sınıfından `Circle` (Daire) ve `Rectangle` (Dikdörtgen) sınıflarını türetelim.

```typescript
// Temel Sınıf: Shape
// Tüm şekillerin ortak özelliklerini ve davranışlarını içerir
class Shape {
    protected name: string; // Alt sınıflardan erişilebilir

    constructor(name: string) {
        this.name = name;
    }

    // Her şeklin alanını hesaplaması beklenir, ancak genel bir uygulama verilemez
    // Bu metot alt sınıflar tarafından ezilmelidir.
    getArea(): number {
        throw new Error("getArea() method must be implemented by derived classes.");
    }

    displayInfo(): void {
        console.log(`This is a ${this.name}.`);
    }
}

// Türetilmiş Sınıf: Circle (Daire)
// Shape sınıfından miras alır
class Circle extends Shape {
    radius: number;

    constructor(radius: number) {
        super("Circle"); // Üst sınıfın constructor'ını çağırma ve 'name'i ayarlama
        this.radius = radius;
    }

    // getArea metodunu ezme (override) - Dairenin alanını hesaplar
    getArea(): number {
        return Math.PI * this.radius * this.radius;
    }

    // Circle sınıfına özgü metot
    getCircumference(): number {
        return 2 * Math.PI * this.radius;
    }
}

// Türetilmiş Sınıf: Rectangle (Dikdörtgen)
// Shape sınıfından miras alır
class Rectangle extends Shape {
    width: number;
    height: number;

    constructor(width: number, height: number) {
        super("Rectangle"); // Üst sınıfın constructor'ını çağırma ve 'name'i ayarlama
        this.width = width;
        this.height = height;
    }

    // getArea metodunu ezme (override) - Dikdörtgenin alanını hesaplar
    getArea(): number {
        return this.width * this.height;
    }
}

// Nesneler oluşturma ve kullanma
const myCircle = new Circle(10);
myCircle.displayInfo();         // Çıktı: This is a Circle. (Miras alınan metot)
console.log(`Circle Area: ${myCircle.getArea().toFixed(2)}`); // Çıktı: Circle Area: 314.16
console.log(`Circle Circumference: ${myCircle.getCircumference().toFixed(2)}`); // Çıktı: Circle Circumference: 62.83

const myRectangle = new Rectangle(5, 8);
myRectangle.displayInfo();         // Çıktı: This is a Rectangle. (Miras alınan metot)
console.log(`Rectangle Area: ${myRectangle.getArea()}`); // Çıktı: Rectangle Area: 40

// const genericShape = new Shape("Unknown");
// console.log(genericShape.getArea()); // Hata: getArea() method must be implemented by derived classes.
```

Bu örnekte:

* `Shape` sınıfı, tüm şekiller için ortak olan `name` özelliğini ve `displayInfo()` metodunu barındırır.
* `getArea()` metodu `Shape` içinde tanımlanmıştır ancak bir hata fırlatır. Bunun nedeni, her şeklin alanını hesaplama mantığının farklı olması ve temel sınıfta genel bir uygulama yapılamamasıdır. Bu metot, alt sınıflar tarafından mutlaka ezilmesi (override edilmesi) gereken bir "sözleşme" görevi görür. (Bu, soyut sınıflara bir geçiş noktasıdır, ilerleyen bölümde ele alınacak.)
* `Circle` ve `Rectangle` sınıfları, `Shape` sınıfından miras alır. Bu, her iki sınıfın da `name` özelliğine ve `displayInfo()` metoduna sahip olacağı anlamına gelir.
* Her türetilmiş sınıf, kendi özel özelliklerini (`radius`, `width`, `height`) ve kendi alan hesaplama mantıklarını (`getArea()` metotlarını ezerek) uygular.
* `super("Circle")` ve `super("Rectangle")` çağrıları, `Shape` sınıfının kurucusunu çağırarak `name` özelliğini doğru şekilde başlatır.

**`super()` Anahtar Kelimesi:**

* **`super()` (Fonksiyon Çağrısı):** Türetilmiş bir sınıfın kurucusu içinde, üst sınıfın kurucusunu çağırmak için kullanılır. Eğer türetilmiş bir sınıfın kendi `constructor`'ı varsa, bu kurucu içinde `super()` çağrısını yapmak zorunludur ve bu çağrı kurucunun ilk ifadesi olmalıdır. Bu sayede, miras alınan özellikler doğru bir şekilde başlatılır.
* **`super.` (Nesne Referansı):** Türetilmiş bir sınıfın metotları içinde, üst sınıfın metotlarına veya özelliklerine erişmek için kullanılır. Bu, üst sınıftaki bir metodu ezseniz bile, o metodun orijinal implementasyonunu çağırmak istediğinizde kullanışlıdır.

```typescript
class Parent {
    protected value: number;

    constructor(val: number) {
        this.value = val;
    }

    greet(): string {
        return "Hello from Parent!";
    }
}

class Child extends Parent {
    constructor(val: number) {
        super(val); // Parent constructor'ı çağrıldı
    }

    greet(): string {
        // Parent'taki greet metodunu çağır ve kendi mesajını ekle
        return `${super.greet()} And hello from Child! Value: ${this.value}`;
    }
}

const child = new Child(10);
console.log(child.greet()); // Çıktı: Hello from Parent! And hello from Child! Value: 10
```

`super()` ve `super.` kullanımı, miras alma hiyerarşisi içindeki sınıfların birbirleriyle nasıl etkileşim kurduğunu anlamak için temeldir.

***

#### 4.3 Metot Ezme (Method Overriding) ve `super` Anahtar Kelimesi

Metot ezme (Method Overriding), bir türetilmiş sınıfın, temel sınıftan miras aldığı bir metodu kendi ihtiyaçlarına göre yeniden tanımlamasıdır. Bu, aynı metodun farklı sınıflarda farklı davranışlar sergilemesine olanak tanır ve **polimorfizm** (çok biçimlilik) için temel bir yapı taşıdır.

**Metot Ezme Nasıl Yapılır?**

Bir metodu ezmek için, türetilmiş sınıfta, temel sınıftaki metotla aynı imzaya (aynı ad, aynı parametre sayısı ve tipleri, aynı geri dönüş tipi) sahip bir metot tanımlarsınız.

```typescript
class Vehicle {
    protected speed: number;

    constructor(initialSpeed: number = 0) {
        this.speed = initialSpeed;
    }

    accelerate(amount: number): void {
        this.speed += amount;
        console.log(`Vehicle speed increased to ${this.speed} km/h.`);
    }

    brake(amount: number): void {
        this.speed -= amount;
        if (this.speed < 0) this.speed = 0;
        console.log(`Vehicle speed decreased to ${this.speed} km/h.`);
    }
}

class Car extends Vehicle {
    private gear: number = 1;

    constructor(initialSpeed: number) {
        super(initialSpeed);
    }

    // accelerate metodunu ezme
    accelerate(amount: number): void {
        // Üst sınıftaki accelerate metodunu çağır
        super.accelerate(amount);
        // Araba sınıfına özgü ek mantık
        this.changeGear(); // Hız arttıkça vites değiştirme
        console.log(`Car is in gear ${this.gear}.`);
    }

    // brake metodunu ezme
    brake(amount: number): void {
        super.brake(amount); // Üst sınıftaki brake metodunu çağır
        this.changeGearDown(); // Hız azaldıkça vites düşürme
        console.log(`Car is in gear ${this.gear}.`);
    }

    private changeGear(): void {
        if (this.speed > 60 && this.gear < 5) {
            this.gear = 5;
        } else if (this.speed > 40 && this.gear < 4) {
            this.gear = 4;
        } else if (this.speed > 20 && this.gear < 3) {
            this.gear = 3;
        } else if (this.speed > 0 && this.gear < 2) {
            this.gear = 2;
        }
    }

    private changeGearDown(): void {
        if (this.speed <= 0) {
            this.gear = 1;
        } else if (this.speed < 20 && this.gear > 2) {
            this.gear = 2;
        } else if (this.speed < 40 && this.gear > 3) {
            this.gear = 3;
        } else if (this.speed < 60 && this.gear > 4) {
            this.gear = 4;
        }
    }
}

const myCar = new Car(0);
myCar.accelerate(30);
// Çıktı:
// Vehicle speed increased to 30 km/h.
// Car is in gear 3.

myCar.accelerate(35);
// Çıktı:
// Vehicle speed increased to 65 km/h.
// Car is in gear 5.

myCar.brake(20);
// Çıktı:
// Vehicle speed decreased to 45 km/h.
// Car is in gear 4.
```

Bu örnekte:

* `Vehicle` sınıfı genel bir aracın hızlanma ve frenleme davranışlarını tanımlar.
* `Car` sınıfı `Vehicle` sınıfından miras alır ve `accelerate()` ile `brake()` metotlarını ezer.
* Ezilen `accelerate()` metodunun içinde, `super.accelerate(amount);` ifadesi ile `Vehicle` sınıfındaki orijinal `accelerate` metodu çağrılır. Bu, üst sınıftaki temel hızlanma mantığının hala çalışmasını sağlarken, `Car` sınıfına özgü ek bir davranış (`changeGear()`) eklememize olanak tanır.
* Aynı mantık `brake()` metodu için de geçerlidir. `super.brake(amount);` çağrılır ve ardından araba sınıfına özgü vites düşürme mantığı uygulanır.

**`super` Anahtar Kelimesinin Kullanımı (`super.method()` ve `super.property`):**

* **`super.methodName(...)`:** Türetilmiş sınıfta ezdiğiniz bir metodun temel sınıftaki orijinal implementasyonunu çağırmak için kullanılır. Bu, temel sınıfın davranışını tamamen değiştirmek yerine, ona eklemeler yapmak veya onu genişletmek istediğinizde çok kullanışlıdır.
* **`super.propertyName`:** Temel sınıftaki `protected` bir özelliğe erişmek için kullanılır. `private` özelliklere `super` aracılığıyla bile erişilemez.

**Ezmenin Amacı ve Önemi:**

1. **Özelleştirme:** Temel sınıfın genel davranışını, türetilmiş sınıfın özel gereksinimlerine uyacak şekilde özelleştirmeyi sağlar.
2. **Davranış Farklılaştırması:** Aynı metodun, farklı tipteki nesnelerde (temel sınıf ve türetilmiş sınıfların nesneleri) farklı davranışlar sergilemesine olanak tanır.
3. **Kodu Daha Anlamlı Hale Getirme:** `Vehicle`'ın hızlanması ile `Car`'ın hızlanması arasındaki nüansları kodda net bir şekilde ifade etmenizi sağlar.
4. **Polimorfizm Temeli:** Ezme, polimorfizmin çalışması için kritik bir prensiptir. Bir temel sınıf tipindeki değişkene bir türetilmiş sınıf nesnesi atadığınızda ve ezilmiş bir metot çağırdığınızda, çalışma zamanında türetilmiş sınıfın versiyonu çalışır (bkz. Bölüm 5: Çok Biçimlilik).

Metot ezme, miras almanın gücünü ve esnekliğini ortaya koyan temel bir özelliktir. Kodun modülerliğini artırır ve daha anlamlı, anlaşılır sınıf hiyerarşileri oluşturmaya yardımcı olur.

***

#### 4.4 Miras Alma ve Kurucular (`super()` Kullanımı)

Türetilmiş bir sınıf (child class) tanımlandığında, temel sınıfın (parent class) özelliklerini ve davranışlarını devralır. Bu özelliklerin doğru bir şekilde başlatılması için, türetilmiş sınıfın kurucusu içinde temel sınıfın kurucusunu çağırmak genellikle zorunludur. Bu işlem `super()` anahtar kelimesi ile yapılır.

**Neden `super()` Kullanmak Zorunludur?**

* **Temel Sınıfın Başlatılması:** Bir türetilmiş sınıf nesnesi oluşturulduğunda, öncelikle temel sınıfın parçası olan özelliklerin başlatılması gerekir. Temel sınıfın kurucusu, bu başlatma işlemini yapar. `super()` çağrısı, bu sorumluluğu üstlenir.
* **`this` Öncesi Çağrı:** TypeScript'te (ve JavaScript'te) türetilmiş bir sınıfın kurucusunda `this` anahtar kelimesini kullanmadan önce `super()` çağrısının yapılması zorunludur. Bunun nedeni, `this` referansının temel sınıf kurucusu tamamlanana kadar oluşturulmamasıdır. Temel sınıf kurucusu, miras alınan özellikleri ve metotları başlatır ve `this`'i uygun bir duruma getirir.

**Söz Dizimi:**

```typescript
class ParentClass {
    parentProperty: string;

    constructor(parentVal: string) {
        this.parentProperty = parentVal;
        console.log(`ParentClass constructor called with: ${parentVal}`);
    }
}

class ChildClass extends ParentClass {
    childProperty: number;

    constructor(parentVal: string, childVal: number) {
        // super() çağrısı, this kullanılmadan önce yapılmalıdır
        super(parentVal); // Temel sınıfın constructor'ını çağırıyoruz

        this.childProperty = childVal; // Artık 'this' güvenle kullanılabilir
        console.log(`ChildClass constructor called with: ${childVal}`);
    }

    displayInfo(): void {
        console.log(`Parent Property: ${this.parentProperty}`);
        console.log(`Child Property: ${this.childProperty}`);
    }
}

const childInstance = new ChildClass("Hello Parent", 123);
// Çıktı:
// ParentClass constructor called with: Hello Parent
// ChildClass constructor called with: 123

childInstance.displayInfo();
// Çıktı:
// Parent Property: Hello Parent
// Child Property: 123
```

Bu örnekte:

* `ParentClass`'ın bir `parentProperty` özelliği vardır ve kurucusunda bu başlatılır.
* `ChildClass` `ParentClass`'tan miras alır ve kendi `childProperty` özelliğine sahiptir.
* `ChildClass`'ın kurucusu içinde, `this.childProperty = childVal;` satırından önce `super(parentVal);` çağrısı yapılmıştır. Bu çağrı, `parentVal`'ı `ParentClass`'ın kurucusuna ileterek `parentProperty`'nin başlatılmasını sağlar.

**Senaryolar ve `super()` Kullanımı:**

1.  **Türetilmiş Sınıfın Kurucusu Yoksa:** Eğer türetilmiş sınıfın kendi bir `constructor`'ı yoksa, TypeScript otomatik olarak varsayılan bir kurucu oluşturur ve içinde `super()` çağrısı yapar.

    ```typescript
    class Base {
        message: string;
        constructor(message: string) {
            this.message = message;
        }
    }

    class Derived extends Base {
        // TypeScript otomatik olarak şunu ekler:
        // constructor(...args: any[]) {
        //     super(...args);
        // }
    }

    const d = new Derived("Hello from auto-constructor!");
    console.log(d.message); // Çıktı: Hello from auto-constructor!
    ```
2.  **Türetilmiş Sınıfın Kendi Kurucusu Varsa:** Yukarıdaki ana örnekte gösterildiği gibi, `super()` çağrısı ilk ifade olmalıdır.

    TypeScript

    ```typescript
    class Animal {
        name: string;
        constructor(name: string) { this.name = name; }
    }

    class Cat extends Animal {
        private _hasWhiskers: boolean;

        constructor(name: string, hasWhiskers: boolean) {
            // this._hasWhiskers = hasWhiskers; // Hata: 'this' property accessors are only available when referencing a supertype constructor.
            super(name); // Önce super() çağrılmalı
            this._hasWhiskers = hasWhiskers;
        }

        meow(): void {
            console.log(`${this.name} ${this._hasWhiskers ? "with whiskers" : "without whiskers"} says Meow!`);
        }
    }

    const fluffy = new Cat("Fluffy", true);
    fluffy.meow(); // Çıktı: Fluffy with whiskers says Meow!
    ```

**Parametrelerin Aktarılması:**

`super()` çağrısına, temel sınıfın kurucusunun beklediği parametreleri iletmeniz gerekir. Eğer temel sınıfın birden fazla kurucusu varmış gibi görünse de (method overloading), TypeScript'te sadece bir kurucu tanımı olduğu için, o tek tanıma uygun parametreleri göndermeniz yeterlidir.

Miras alma ve kurucuların birlikte nasıl çalıştığını anlamak, sağlam ve doğru sınıf hiyerarşileri oluşturmak için kritik öneme sahiptir. `super()` anahtar kelimesi, temel sınıfın başlatılmasını güvence altına alır ve türetilmiş sınıfların kendi özel başlatma mantıklarını eklemesine olanak tanır.

***

#### 4.5 Miras Alma ve Erişim Belirleyiciler (`protected` Kullanımı)

Önceki bölümlerde `public`, `private` ve `protected` erişim belirleyicilerini görmüştük. Miras alma bağlamında, `protected` belirleyicinin önemi daha da artar.

**`protected` Anahtar Kelimesinin Rolü:**

* **`public`:** Her yerden erişilebilir. Miras alan sınıflar da dahil.
* **`private`:** Sadece tanımlandığı sınıfın içinden erişilebilir. Miras alan alt sınıflardan **erişilemez**.
* **`protected`:** Tanımlandığı sınıfın içinden **ve bu sınıfı miras alan (türetilmiş) alt sınıfların içinden** erişilebilir. Sınıf dışından (örneğin, doğrudan bir nesne üzerinden) yine erişilemez.

`protected`, bir miras hiyerarşisi içindeki sınıfların birbirlerinin belirli dahili durumlarına veya yardımcı metotlarına kontrollü bir şekilde erişmesini sağlarken, dış dünyadan bu detayları gizlemeye devam etmek istediğimizde idealdir.

**Örnek: Çalışan ve Yöneticiler Hiyerarşisi**

Bir `Employee` (Çalışan) temel sınıfı ve ondan türeyen bir `Manager` (Yönetici) sınıfı oluşturalım. `Employee` sınıfında maaş ve çalışma saatleri gibi bazı detayları `protected` yaparak, bu bilgilere sadece türetilmiş sınıfların (yöneticiler gibi) erişmesine izin verelim.

```typescript
class Employee {
    public name: string;
    protected salary: number; // Sadece Employee ve alt sınıflarından erişilebilir
    protected hoursWorked: number; // Sadece Employee ve alt sınıflarından erişilebilir

    constructor(name: string, salary: number, hoursWorked: number) {
        this.name = name;
        this.salary = salary;
        this.hoursWorked = hoursWorked;
    }

    public getDailyWage(): number {
        // Korunmuş 'salary' özelliğini sınıf içinden kullanma
        return this.salary / 22; // Aylık maaşın yaklaşık günlük karşılığı (22 iş günü)
    }

    protected getEmployeeDetails(): string {
        // Korunmuş özelliklere sınıf içinden erişim
        return `${this.name} - Salary: $${this.salary}, Hours Worked: ${this.hoursWorked}`;
    }
}

class Manager extends Employee {
    private teamSize: number;

    constructor(name: string, salary: number, hoursWorked: number, teamSize: number) {
        super(name, salary, hoursWorked); // Üst sınıfın constructor'ını çağırma
        this.teamSize = teamSize;
    }

    public manageTeam(): void {
        console.log(`${this.name} is managing a team of ${this.teamSize} people.`);
    }

    public getManagerOverview(): void {
        // Protected metodu alt sınıftan çağırma
        const employeeDetails = super.getEmployeeDetails();
        console.log(`--- Manager Overview ---`);
        console.log(employeeDetails); // Protected method call
        console.log(`Team Size: ${this.teamSize}`);

        // Protected 'salary' özelliğine alt sınıftan doğrudan erişim
        if (this.salary > 8000) {
            console.log(`${this.name} is a high-earning manager.`);
        }
    }
}

const emp = new Employee("Ali Veli", 5000, 160);
console.log(`Employee Name: ${emp.name}`);           // Public erişim
console.log(`Employee Daily Wage: $${emp.getDailyWage().toFixed(2)}`); // Public metot çağrısı

// console.log(emp.salary);         // Hata: Property 'salary' is protected and only accessible within class 'Employee' and its subclasses.
// console.log(emp.hoursWorked);    // Hata: Property 'hoursWorked' is protected...
// emp.getEmployeeDetails();        // Hata: Property 'getEmployeeDetails' is protected...

const manager = new Manager("Ayşe Can", 9000, 180, 10);
manager.manageTeam();      // Public metot çağrısı
manager.getManagerOverview(); // Public metot çağrısı, içinde protected'a erişiliyor
// Çıktı:
// --- Manager Overview ---
// Ayşe Can - Salary: $9000, Hours Worked: 180
// Team Size: 10
// Ayşe Can is a high-earning manager.
```

Bu örnekte:

* `Employee` sınıfındaki `salary`, `hoursWorked` özellikleri ve `getEmployeeDetails()` metodu `protected` olarak tanımlanmıştır.
* Bu sayede, `Employee` sınıfının dışından (`emp.salary` gibi) bu üyelere doğrudan erişim mümkün değildir.
* Ancak `Manager` sınıfı, `Employee` sınıfından miras aldığı için, kendi metotları içinde (`getManagerOverview()`) `this.salary`, `this.hoursWorked` gibi özelliklere ve `super.getEmployeeDetails()` gibi metotlara erişebilir.

**`protected` Kullanım Senaryoları:**

* **Dahili Yardımcı Üyeler:** Sadece bir sınıf hiyerarşisi içindeki sınıfların kullanması gereken yardımcı özellikler veya metotlar için. Bu, dış API'yi temiz tutar ve sınıflar arasındaki dahili işbirliğini destekler.
* **Özelliklerin Başlatılması veya İşlenmesi:** Temel sınıfta başlatılan veya işlenen, ancak alt sınıfların da erişmesi ve belki de genişletmesi gereken özellikler için.
* **Şablon Metot Tasarımı:** Temel sınıfın bir metodu, alt sınıfların belirli adımları kendi uygulamalarıyla doldurmasını gerektiren bir "şablon" sağlayabilir. Bu adımlar `protected` metotlar olabilir.

`protected` anahtar kelimesi, miras alma hiyerarşisi içindeki veri ve davranışın kontrolünü sağlamak için kritik bir araçtır. `private` ile dış dünyaya tamamen kapalı, `public` ile tamamen açık arasında kontrollü bir erişim seviyesi sunar. Bu, daha esnek ancak yine de kapsüllenmiş bir sınıf tasarımı yapmamızı sağlar.

***

Bölüm 4'ü de tamamlamış bulunuyoruz. Miras alma prensibini, temel ve türetilmiş sınıfları, `super` kullanımını ve erişim belirleyicilerin (özellikle `protected`'ın) miras alma bağlamındaki rolünü detaylıca inceledik.

Bir sonraki bölümde, OOP'nin bir diğer güçlü prensibi olan **Çok Biçimlilik (Polymorphism)** konusunu ele alacağız. Bu prensip, farklı nesnelerin aynı arayüz üzerinden farklı davranışlar sergilemesini nasıl sağladığımızı gösterecek ve daha esnek kod yazmamıza olanak tanıyacak.
