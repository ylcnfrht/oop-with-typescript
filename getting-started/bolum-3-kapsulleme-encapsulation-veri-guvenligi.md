# Bölüm 3: Kapsülleme (Encapsulation) - Veri Güvenliği

Nesne Yönelimli Programlama'nın (OOP) dört ana prensibinden ilki ve belki de en temel olanı **kapsülleme (encapsulation)**'dir. Kapsülleme, veri (özellikler) ve bu veriler üzerinde işlem yapan kodun (metotlar) tek bir birim içinde bir araya getirilmesi ve nesnenin dahili durumunun dış dünyadan gizlenmesidir. Bu prensip, veri bütünlüğünü korumayı, kodun daha güvenli ve bakımı daha kolay olmasını sağlamayı amaçlar.

Gerçek dünyadan bir örnekle açıklayacak olursak, bir televizyonu düşünün. Kumanda ile kanalları değiştirir, sesi açıp kapatırız. Televizyonun içinde karmaşık elektronik devreler ve işleyiş mekanizmaları vardır, ancak bizim bunlarla doğrudan etkileşime girmemize gerek kalmaz. Sadece "arayüz" (kumanda ve ekran) aracılığıyla televizyonu kullanırız. Televizyonun iç yapısı "kapsüllenmiştir". Kapsülleme, tam olarak bu mantığı yazılıma taşır.

***

#### 3.1 Kapsülleme Nedir? Neden Önemlidir?

**Kapsülleme**, bir nesnenin iç detaylarını (özelliklerini ve bazı metotlarını) dış dünyadan gizleyerek, sadece belirlenmiş bir arayüz (public metotlar) aracılığıyla etkileşime girmesine izin veren bir prensiptir. Bu, iki ana konsepti içerir:

1. **Verinin ve Davranışın Bir Arada Tutulması (Bundling):** Bir nesnenin, kendi verisini ve bu veri üzerinde çalışan metotlarını tek bir birimde (sınıf içinde) barındırmasıdır. Bu, verinin her yere dağılmış olmasını engeller ve ilgili her şeyi bir araya getirir.
2. **Bilgi Gizleme (Information Hiding):** Bir nesnenin dahili durumunun ve çalışma mekanizmalarının dışarıdan doğrudan erişime kapatılmasıdır. Dış dünya, nesnenin içinde ne olduğunu bilmek zorunda değildir; sadece nesnenin ne yapabildiğini (public arayüzünü) bilmesi yeterlidir.

**Kapsüllemenin Temel Amaçları ve Önemi:**

* **Veri Bütünlüğü ve Güvenliği:** En kritik faydası budur. Bir nesnenin hassas veya önemli özelliklerinin dışarıdan doğrudan değiştirilmesini engeller. Örneğin, bir banka hesabının bakiyesini doğrudan `myAccount.balance = -100;` şeklinde değiştirmek yerine, `myAccount.withdraw(100);` metoduyla kontrollü bir şekilde işlem yapılması sağlanır. Bu, bakiyenin negatif olmaması gibi iş kurallarının korunmasını garanti eder.
* **Bakım Kolaylığı:** Sınıfın dahili implementasyon detayları (bir özelliğin tipi, bir hesaplama algoritması vb.) değiştirildiğinde, bu değişiklik sınıfın public arayüzünü bozmadığı sürece, dışarıdaki kodu etkilemez. Bu, kodun güvenli bir şekilde yeniden düzenlenmesini (refactoring) ve geliştirilmesini sağlar.
* **Modülerlik ve Esneklik:** Her nesne kendi içinde bağımsız bir birim haline gelir. Bu modüler yapı, kodun farklı bölümlerinin birbirine olan bağımlılığını azaltır. Nesneler, "kara kutu" gibi davranır; içlerini bilmeye gerek kalmadan kullanılabilirler.
* **Kodun Anlaşılırlığı:** Sınıfın dışarıya açık arayüzü daha temiz ve net olur. Geliştiriciler, bir sınıfı kullanırken sadece public metotlarına odaklanır, dahili karmaşıklıkla uğraşmak zorunda kalmaz.
* **Hata Ayıklama Kolaylığı:** Bir sorun yaşandığında, kapsüllenmiş bir yapıda hatanın kaynağını tespit etmek daha kolaydır, çünkü veri değişimleri belirli metotlar üzerinden kontrollü bir şekilde gerçekleşir.

**Kapsülleme Nasıl Sağlanır?**

TypeScript'te (ve diğer OOP dillerinde) kapsülleme, özellikle **erişim belirleyiciler** (`public`, `private`, `protected`) ve **getter/setter metotları** kullanılarak sağlanır.

* `private` üyeler, sınıf dışından doğrudan erişimi tamamen engeller.
* `protected` üyeler, yalnızca sınıf ve miras alan alt sınıflar için erişimi kısıtlar.
* `public` üyeler ise dışarıdan erişilebilir arayüzü oluşturur.
* **Getter ve Setter'lar**, özel (private) bir özelliğe kontrollü bir şekilde okuma (`get`) ve yazma (`set`) erişimi sağlamak için kullanılır. Bu sayede, okuma ve yazma işlemleri sırasında doğrulama, formatlama veya yan etkiler gibi özel mantık uygulanabilir.

***

#### 3.2 Özel (Private) Özellikler ve Metotlar (`private`)

`private` erişim belirleyici, bir sınıfın bir özelliğine veya metoduna sadece tanımlandığı **sınıfın içinden** erişilebileceğini belirtir. Bu, kapsüllemenin en doğrudan ve güçlü biçimidir.

**Özel Üyelerin Tanımı ve Erişimi:**

Bir sınıf üyesini `private` yapmak için adının önüne `private` anahtar kelimesini eklemeniz yeterlidir.

```typescript
class BankAccount {
    private _accountNumber: string; // Hesap numarası dışarıdan doğrudan değiştirilemez
    private _balance: number;      // Bakiye dışarıdan doğrudan değiştirilemez

    constructor(accountNumber: string, initialBalance: number) {
        this._accountNumber = accountNumber;
        // Bakiye için başlangıçta bir doğrulama yapabiliriz
        if (initialBalance < 0) {
            throw new Error("Initial balance cannot be negative.");
        }
        this._balance = initialBalance;
    }

    // Public metot: Hesaba para yatırma
    public deposit(amount: number): void {
        if (amount <= 0) {
            console.error("Deposit amount must be positive.");
            return;
        }
        this._balance += amount; // Private özelliğe sınıf içinden erişim
        console.log(`Deposited $${amount}. New balance: $${this._balance}`);
    }

    // Public metot: Hesaptan para çekme
    public withdraw(amount: number): void {
        if (amount <= 0) {
            console.error("Withdrawal amount must be positive.");
            return;
        }
        if (this._balance < amount) {
            console.error(`Insufficient funds. Current balance: $${this._balance}`);
            return;
        }
        this._balance -= amount; // Private özelliğe sınıf içinden erişim
        console.log(`Withdrew $${amount}. New balance: $${this._balance}`);
    }

    // Public metot: Bakiyeyi güvenli bir şekilde okumak için
    public getBalance(): number {
        return this._balance; // Private özelliğe sınıf içinden erişim ve dışarıya sunum
    }

    // Public metot: Hesap numarasını güvenli bir şekilde okumak için
    public getAccountNumber(): string {
        // Hesap numarasının tamamını göstermeyip, son 4 hanesini gösterme gibi bir mantık eklenebilir.
        return `****${this._accountNumber.substring(this._accountNumber.length - 4)}`;
    }

    // Private metot: Sadece sınıfın dahili kullanımı için
    private logTransaction(type: string, amount: number): void {
        console.log(`${new Date().toLocaleString()}: ${type} of $${amount} processed.`);
    }
}

const myAccount = new BankAccount("ABC123456789", 1000);

// console.log(myAccount._balance); // Hata: Property '_balance' is private and only accessible within class 'BankAccount'.
// myAccount._balance = 5000;       // Hata: Property '_balance' is private and only accessible within class 'BankAccount'.

myAccount.deposit(200);   // Public metot aracılığıyla bakiye güncellenir
myAccount.withdraw(150);  // Public metot aracılığıyla bakiye güncellenir

console.log(`Current Balance: $${myAccount.getBalance()}`);         // Public metot aracılığıyla bakiye okunur
console.log(`Account Number: ${myAccount.getAccountNumber()}`); // Public metot aracılığıyla hesap numarası okunur
```

Yukarıdaki örnekte:

* `_accountNumber` ve `_balance` özellikleri `private` olarak tanımlanmıştır. Bu sayede, `myAccount._balance = 5000;` gibi doğrudan erişim ve atama denemeleri derleme hatası verir.
* Bakiye üzerindeki tüm işlemler (`deposit`, `withdraw`) ve bakiye sorgulama (`getBalance`), sınıfın public metotları aracılığıyla yapılır. Bu metotlar, iş kurallarını (örneğin negatif para yatırmayı engelleme, yetersiz bakiye kontrolü) uygulayabilir ve nesnenin dahili durumunun her zaman geçerli kalmasını sağlar.
* `logTransaction` metodu da `private`'tır. Bu, dışarıdan çağrılamayacağı, sadece `BankAccount` sınıfı içindeki diğer metotlar tarafından (örneğin `deposit` ve `withdraw` içinde) bir yardımcı fonksiyon olarak kullanılabileceği anlamına gelir. Bu, dahili yardımcı fonksiyonların dış API'yi kirletmesini engeller.

**JavaScript'te `#` ile Private Alanlar:**

TypeScript, derleme zamanında `private` anahtar kelimesiyle bu kısıtlamayı uygular. Ancak derlenen JavaScript kodunda, bu özellikler hala erişilebilir olabilir (sadece TypeScript derleyicisi sizi uyarır).

ES2020 standardı ile birlikte JavaScript'e tam anlamıyla private alanlar eklenmiştir. Bunlar, özelliğin önüne `#` (hash) işareti konularak tanımlanır. TypeScript, `target` ayarınız uygun olduğunda bunları da destekler ve gerçek çalışma zamanı gizliliği sağlar.

```javascript
class NewBankAccount {
    #accountNumber: string; // Gerçekten private alan (JS Runtime'da da gizli)
    #balance: number;

    constructor(accountNumber: string, initialBalance: number) {
        this.#accountNumber = accountNumber;
        if (initialBalance < 0) {
            throw new Error("Initial balance cannot be negative.");
        }
        this.#balance = initialBalance;
    }

    deposit(amount: number): void {
        this.#balance += amount; // # ile private alana sınıf içinden erişim
        console.log(`New balance: $${this.#balance}`);
    }
}

const newAccount = new NewBankAccount("XYZ987", 2000);
// console.log(newAccount.#balance); // SyntaxError: Private field '#balance' must be declared in an enclosing class
```

**Ne Zaman `private` Kullanmalıyız?**

* Bir özelliğin veya metodun, yalnızca sınıfın kendi iç işleyişi için gerekli olduğu durumlarda.
* Bir özelliğin değerinin dışarıdan kontrolsüzce değiştirilmesini engellemek istediğinizde.
* Bir özelliğin dahili temsilinin (örneğin, sıcaklığı Celsius yerine Kelvin olarak tutmak) dışarıdaki kodu etkilemeden değiştirilebilmesini sağlamak istediğinizde.
* Sınıfın API'sini (public arayüzünü) mümkün olduğunca küçük ve anlaşılır tutmak istediğinizde.

`private` kullanmak, nesnenin veri bütünlüğünü sağlamak ve daha güvenli, sürdürülebilir bir kod tabanı oluşturmak için çok önemlidir.

***

#### 3.3 Getter ve Setter Metotları ile Kontrollü Erişim

Önceki bölümde, getter ve setter metotlarını yüzeysel olarak incelemiştik. Şimdi kapsülleme bağlamında, özel (private) özelliklere kontrollü erişim sağlamak için ne kadar önemli olduklarını daha derinlemesine ele alacağız.

Hatırlayalım:

* **Getter (`get`):** Özel bir özelliğin değerini okumak için kullanılır.
* **Setter (`set`):** Özel bir özelliğin değerini ayarlamak için kullanılır.

Bu metotlar, bir özelliğe doğrudan erişimi engellerken, o özelliğin okunması veya değiştirilmesi sırasında ek mantık (doğrulama, formatlama, yan etkiler vb.) uygulanmasına olanak tanır.

**Neden Doğrudan Erişimi Kısıtlayıp Getter/Setter Kullanırız?**

Bir özelliğe doğrudan erişime izin vermek (`public` yapmak) yerine neden getter ve setter kullanırız?

```typescript
class Product {
    public price: number; // Kapsüllenmemiş
}

const p1 = new Product();
p1.price = -100; // Problem: Fiyat negatif olmamalıdır!
```

Yukarıdaki senaryoda, `price` özelliğine doğrudan erişim, geçersiz bir değerin atanmasına izin verir. Bu durum, veri bütünlüğünü bozar ve uygulamanın beklenmedik şekillerde davranmasına neden olabilir. İşte bu noktada getter ve setter'lar devreye girer.

**Örnek: `User` Sınıfında Yaş Doğrulaması ve Tam İsim Getter'ı**

Bir `User` sınıfı oluşturalım. Kullanıcının yaşının negatif olmamasını garanti etmek ve tam ismini dinamik olarak oluşturmak için getter/setter kullanalım.

```typescript
class User {
    private _firstName: string;
    private _lastName: string;
    private _age: number; // Yaş private olsun

    constructor(firstName: string, lastName: string, age: number) {
        this._firstName = firstName;
        this._lastName = lastName;
        // Kurucu içinde setter'ı çağırarak ilk yaş atamasında da doğrulamayı uygula
        this.age = age; 
    }

    // Tam ismi döndüren getter
    get fullName(): string {
        return `${this._firstName} ${this._lastName}`;
    }

    // Yaş değerini okumak için getter
    get age(): number {
        return this._age;
    }

    // Yaş değerini ayarlamak için setter (doğrulama içerir)
    set age(newAge: number) {
        if (newAge < 0 || newAge > 150) { // Yaşın geçerli aralıkta olup olmadığını kontrol et
            throw new Error("Invalid age. Age must be between 0 and 150.");
        }
        this._age = newAge;
    }

    // Yıl dönümünü kutlayan metot
    celebrateBirthday(): void {
        this.age++; // Yaşı setter üzerinden artırıyoruz, böylece doğrulama otomatik çalışır
        console.log(`${this.fullName} is now ${this.age} years old! Happy Birthday!`);
    }
}

const user = new User("Cem", "Yılmaz", 45);
console.log(`User Full Name: ${user.fullName}`); // Getter'ı kullanma: Cem Yılmaz
console.log(`User Age: ${user.age}`);           // Getter'ı kullanma: 45

user.celebrateBirthday(); // Çıktı: Cem Yılmaz is now 46 years old! Happy Birthday!

try {
    user.age = -5; // Setter'ı kullanma ve hata fırlatma
} catch (error: any) {
    console.error(`Error setting age: ${error.message}`); // Çıktı: Error setting age: Invalid age. Age must be between 0 and 150.
}

try {
    user.age = 200; // Setter'ı kullanma ve hata fırlatma
} catch (error: any) {
    console.error(`Error setting age: ${error.message}`); // Çıktı: Error setting age: Invalid age. Age must be between 0 and 150.
}

// user.age = 50; // Geçerli bir değer atama
// console.log(`User Age after direct assignment: ${user.age}`);
```

Bu örnekte:

* `_firstName`, `_lastName` ve `_age` özellikleri `private` olarak tanımlanmıştır.
* `fullName` bir **getter**'dır. Bu bir özellik gibi görünse de, her çağrıldığında `_firstName` ve `_lastName`'i birleştirerek dinamik bir `string` değeri üretir.
* `age` özelliği için hem bir **getter** (`get age()`) hem de bir **setter** (`set age(newAge: number)`) tanımlanmıştır.
* `set age()` metodu, `newAge` değerinin 0 ile 150 arasında olup olmadığını kontrol eder. Eğer geçerli değilse bir hata fırlatır. Bu, `_age` özelliğinin her zaman mantıklı bir değere sahip olmasını garanti eder.
* `user.age = -5;` gibi atamalar, `set age()` metodunu tetikler ve doğrulama hatası verir.
* `celebrateBirthday()` metodunda `this.age++` ifadesi kullandığımızda, aslında `this.age = this.age + 1;` yazmış oluruz. Buradaki atama işlemi de otomatik olarak `set age()` metodunu çağırır ve içerisindeki doğrulamayı uygular.

**Getters ve Setters'ın Avantajları (Kapsülleme Bağlamında):**

1. **Kontrollü Veri Erişimi:** Özelliklerin nasıl okunup yazılacağı üzerinde tam kontrol sağlar.
2. **Veri Doğrulama:** Özelliğe atanacak değerleri kontrol ederek nesnenin iç durumunun geçerli kalmasını sağlar.
3. **Dinamik Değerler:** Getter'lar, birden fazla özellikten türetilen veya hesaplanan değerleri sunabilir.
4. **Yan Etkiler:** Setter'lar, bir özellik güncellendiğinde ek işlemler (loglama, UI güncelleme vb.) yapmayı mümkün kılar.
5. **Soyutlama:** Dahili implementasyonu (veri depolama şeklini) değiştirmeden public arayüzü korumanızı sağlar. Örneğin, gelecekte `_age` yerine doğum tarihini depolayabilir ve yaş getter'ını doğum tarihinden hesaplayabiliriz; dışarıdaki kullanıcı kodunda hiçbir değişiklik yapmaya gerek kalmaz.

Getter ve setter'lar, kapsülleme prensibini güçlendirerek, daha sağlam, güvenli ve bakımı kolay nesne tasarımları oluşturmanıza yardımcı olur.

***

#### 3.4 Kapsüllemenin Faydaları ve Yazılım Tasarımına Etkisi

Kapsülleme prensibinin yazılım tasarımına ve genel geliştirme sürecine sağladığı faydaları daha yakından inceleyelim. Bu faydalar, OOP'yi neden tercih ettiğimizin temel nedenlerindendir.

**1. Veri Gizleme ve Güvenlik:**

* **Veri Bütünlüğü:** Kapsülleme, nesnenin dahili durumunun (verisinin) bozulmasını engeller. `private` özellikler ve setter metotları aracılığıyla veri üzerinde uygulanan doğrulamalar (validation), nesnenin her zaman geçerli ve tutarlı bir durumda olmasını sağlar. Örneğin, bir `Order` (Sipariş) nesnesinin `totalAmount` (toplam tutar) özelliğini doğrudan değiştirmek yerine, `addItem()` veya `removeItem()` gibi metotlar aracılığıyla güncelleyerek, tutarın her zaman doğru hesaplanmasını garanti edersiniz.
* **Yetkisiz Erişim Önleme:** `private` erişim belirleyici, hassas verilerin (örneğin, şifreler, API anahtarları, banka hesap numaraları) dışarıdan rastgele okunmasını veya değiştirilmesini önler. Bu veriler üzerinde işlem yapılması gerektiğinde, bu işlemler kontrollü metotlar aracılığıyla gerçekleştirilir.

**2. Kod Bakımı ve Esneklik:**

* **Değişikliklerin Etkisini Azaltma (Reduced Ripple Effect):** Bir sınıfın dahili implementasyon detayları (örneğin, bir algoritmanın veya bir özelliğin veri tipinin değişmesi) `private` olduğunda, bu değişiklikler sınıfın dışındaki kodları etkilemez. Public arayüz aynı kaldığı sürece, sınıfın içindeki her şeyi özgürce değiştirebilirsiniz. Bu, "ripple effect" adı verilen, bir değişikliğin dalga dalga tüm kod tabanına yayılması sorununu minimize eder.
* **Yeniden Düzenleme (Refactoring) Kolaylığı:** Kapsüllenmiş kod, daha kolay refactor edilebilir. Sınıfın içindeki bir özelliği yeniden adlandırmak veya veri yapısını değiştirmek istediğinizde, sadece sınıfın içindeki ilgili metotları güncellemeniz yeterli olur. Bu, büyük kod tabanlarında zaman ve efordan büyük tasarruf sağlar.
* **Esneklik:** Kapsülleme, bir nesnenin gelecekteki değişikliklere daha esnek yanıt vermesini sağlar. Eğer bir özelliğin depolama yöntemi değişirse (örneğin, bir sayı yerine bir enum kullanmaya başlanırsa), getter/setter'lar bu değişimi dış dünyaya soyutlayabilir.

**3. Modülerlik ve Anlaşılırlık:**

* **Modüler Tasarım:** Kapsülleme, sistemleri bağımsız, kendi kendine yeten modüllere (nesnelere) ayırmayı teşvik eder. Her nesne, belirli bir sorumluluğu üstlenir ve diğer nesnelerle sadece public arayüzü üzerinden etkileşime girer. Bu, kodun daha düzenli ve organize olmasını sağlar.
* **Arayüzün Netliği (Clearer Interface):** Bir sınıfın public metotları, o sınıfın dış dünyaya sunduğu "API"sini oluşturur. Gizlenen (private/protected) detaylar, bu arayüzü gereksiz karmaşıklıktan arındırır. Kullanıcılar, bir sınıfı nasıl kullanacaklarını anlamak için sadece public arayüzüne bakmak zorundadır, bu da öğrenme eğrisini düşürür.
* **Karmaşıklık Yönetimi:** Büyük ve karmaşık sistemleri daha küçük, yönetilebilir parçalara ayırarak genel sistemin karmaşıklığını azaltır. Her bir modül (sınıf) kendi içindeki karmaşıklığı yönetir, ancak bu karmaşıklığı dış dünyaya sızdırmaz.

**Özetle:**

Kapsülleme, sadece "private" anahtar kelimesiyle bir özelliği gizlemekten çok daha fazlasıdır. Veri ile o veriyi manipüle eden davranışları bir araya getirerek, nesnelerin kendi kendilerine yeten, güvenli ve tutarlı birimler olmasını sağlar. Bu, daha kaliteli, daha esnek ve bakımı daha kolay yazılım sistemlerinin inşası için vazgeçilmez bir prensiptir.

OOP'nin diğer prensipleri olan miras alma, çok biçimlilik ve soyutlama da kapsüllemeyle birleşerek daha güçlü ve etkili yazılım tasarımlarına olanak tanır.

***

Bölüm 3'ü de başarıyla tamamladık. Şimdi, OOP'nin diğer temel prensiplerine geçmeye hazırız.
