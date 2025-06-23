# Bölüm 8: TypeScript'te İleri OOP Konuları ve En İyi Pratikler

TypeScript'te doğrudan çoklu miras (bir sınıfın birden fazla sınıftan miras alması) desteklenmez. Bu, bazı durumlarda (örneğin, birden fazla sınıftan davranışları birleştirmek istediğinizde) bir sınırlama gibi görünebilir. Ancak **Mixins (karışımlar)**, bu çoklu miras sorununu çözmek ve sınıflara dinamik olarak ek yetenekler kazandırmak için güçlü bir desendir.

Bir Mixin, bir sınıfın işlevselliğini (metotlarını ve özelliklerini) başka bir sınıfa "karıştırmanıza" olanak tanıyan bir yaklaşımdır. Genellikle bir fonksiyonel yaklaşım veya sınıf implementasyonlarını kopyala-yapıştır yapmadan yeniden kullanmanın bir yoludur.

**Neden Mixin Kullanılır?**

* **Çoklu Miras Sorununu Çözme:** Bir sınıfın birden fazla kaynaktan davranış devralması gerektiğinde, mixinler alternatif bir çözüm sunar.
* **Kod Tekrarını Azaltma:** Farklı sınıfların paylaştığı ortak davranışları tek bir yerde tanımlayıp, gerektiğinde bu sınıflara "karıştırabilirsiniz".
* **Dinamik Yetenek Ekleme:** Sınıflara çalışma zamanında bile ek yetenekler kazandırabilir.

**TypeScript'te Mixin Implementasyonu**

TypeScript'te mixinler genellikle birleştirme (composition) ve tip birleştirme (type merging) teknikleri kullanılarak uygulanır. En yaygın desen, bir fonksiyon kullanarak bir sınıfı başka bir sınıfın özellikleriyle genişletmektir.

```typescript
// Mixin 1: Timestamp yeteneği ekleyen bir fonksiyon
// T'nin bir sınıf (new (...args: any[]) => {}) olduğunu belirtiyoruz.
function Timestamped<T extends { new (...args: any[]): {} }>(Base: T) {
    return class extends Base {
        createdAt: Date = new Date();
        updatedAt: Date = new Date();

        updateTimestamp(): void {
            this.updatedAt = new Date();
            console.log(`Timestamp updated for ${this.constructor.name}.`);
        }
    };
}

// Mixin 2: Loglama yeteneği ekleyen bir fonksiyon
function Loggable<T extends { new (...args: any[]): {} }>(Base: T) {
    return class extends Base {
        logMessage(message: string): void {
            console.log(`[${new Date().toLocaleTimeString()}] ${this.constructor.name}: ${message}`);
        }
    };
}

// Temel Sınıf
class User {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    sayHello(): void {
        console.log(`Hello, my name is ${this.name}`);
    }
}

// User sınıfına Timestamped ve Loggable mixinlerini uygulayalım
// TypeScript'in bu mixinleri tip olarak nasıl birleştireceğini anlatmak için 'as' kullanıyoruz.
const AdvancedUser = Timestamped(Loggable(User));

// AdvancedUser artık User, Timestamped ve Loggable özelliklerine/metotlarına sahip
const user = new AdvancedUser("Alice");
user.sayHello();         // User sınıfından
user.logMessage("User created!"); // Loggable mixin'den
console.log(`Created At: ${user.createdAt}`); // Timestamped mixin'den
user.updateTimestamp();  // Timestamped mixin'den
console.log(`Updated At: ${user.updatedAt}`); // Timestamped mixin'den
```

Bu örnekte:

* `Timestamped` ve `Loggable`, bir temel sınıfı alıp ona ek özellikler (`createdAt`, `updatedAt`, `logMessage`) ve metotlar ekleyerek yeni bir sınıf döndüren fonksiyonlardır.
* `AdvancedUser` sınıfı, `Loggable` mixin'ini `User` sınıfına, ardından `Timestamped` mixin'ini bu yeni sınıfa uygulayarak oluşturulur.
* Sonuç olarak `AdvancedUser`, hem `User` sınıfının orijinal metotlarına hem de her iki mixin'in eklediği metotlara ve özelliklere sahip olur.

**Mixinleri Uygularken Dikkat Edilmesi Gerekenler:**

* **Tip Güvenliği:** TypeScript'in mixinlerle doğru tip çıkarımı yapması bazen karmaşık olabilir. Yukarıdaki örnekte görüldüğü gibi, `const AdvancedUser = Timestamped(Loggable(User));` satırı, TypeScript'in doğru tiplendirme yapabilmesi için önemlidir.
* **Çakışmalar:** Eğer iki mixin veya bir mixin ile temel sınıf aynı isimde bir özellik veya metoda sahipse çakışmalar meydana gelebilir. Bu durumda, mixinlerin uygulanma sırası veya manuel birleştirme stratejileri önem kazanır.
* **`super` Çağrıları:** Mixinler içinde `super` çağrıları karmaşıklaşabilir, çünkü `super` dinamik olarak hangi temel sınıfın metodunu çağıracağını belirler.

Mixins, miras almanın sınırlamalarını aşmak ve kod tekrarını azaltmak için esnek bir yol sunar. Ancak, dikkatli kullanılmadığında kodun okunabilirliğini ve tip güvenliğini zorlaştırabilirler.

***

#### 8.2 Dekoratörler (Decorators)

**Dekoratörler (Decorators)**, sınıflara, metotlara, erişimcilerine (getters/setters), özelliklere veya parametrelere metadata (üst veri) eklemenize veya onların davranışlarını çalışma zamanında değiştirmenize olanak tanıyan özel bir tür bildirimdir. TypeScript'te deneysel bir özelliktir ve genellikle `@` sembolüyle kullanılırlar.

Dekoratörler, Angular gibi modern web framework'lerinde ve TypeORM gibi kütüphanelerde yaygın olarak kullanılır.

**Dekoratörlerin Çalışma Prensibi:**

Bir dekoratör, uygulandığı hedef (sınıf, metot vb.) hakkında bilgi alan ve bu hedefi değiştiren veya yeni bir hedef döndüren bir fonksiyondur.

**Neden Dekoratör Kullanılır?**

* **Ayırma Kaygısı (Separation of Concerns):** Bir sınıfa veya metoda ek bir işlevsellik (loglama, yetkilendirme, validasyon) eklemek istediğinizde, bu işlevselliği ana iş mantığından ayırarak kodu daha temiz hale getirir.
* **Tekrarlayan Kodu Azaltma (Boilerplate Reduction):** Tekrarlayan yapılandırma veya davranışları otomatik olarak eklemek için kullanılabilirler.
* **Metadata Ekleme:** Bir sınıfa veya özelliğe ek bilgiler (örneğin, bir veritabanı tablosu adı) eklemek için kullanılır.

**Dekoratör Türleri ve Örnekler:**

Dekoratörler, uygulandıkları yere göre farklı imzalara sahiptir:

1. **Sınıf Dekoratörleri:** Sınıfın kendisini hedefler.
2. **Metot Dekoratörleri:** Bir sınıfın metodunu hedefler.
3. **Erişimci (Accessor) Dekoratörleri:** `get` veya `set` metotlarını hedefler.
4. **Özellik (Property) Dekoratörleri:** Bir sınıfın özelliğini hedefler.
5. **Parametre Dekoratörleri:** Bir metodun parametresini hedefler.

**Dekoratörleri Etkinleştirme:**

`tsconfig.json` dosyasında `experimentalDecorators` ve `emitDecoratorMetadata` (metadata yansıtma için) ayarlarını `true` yapmanız gerekir:

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "CommonJS",
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

**Örnek: Loglama Metot Dekoratörü**

Bir metot her çağrıldığında, çağrıldığını loglayan bir dekoratör oluşturalım.

```typescript
// Metot Dekoratörü Tanımı
function LogMethod(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    // target: Metodun ait olduğu sınıfın prototipi (static metotlar için sınıfın kendisi)
    // propertyKey: Metodun adı (string)
    // descriptor: Metodun PropertyDescriptor'ı (value, writable, enumerable, configurable)

    const originalMethod = descriptor.value; // Metodun orijinal implementasyonunu sakla

    descriptor.value = function (...args: any[]) {
        console.log(`LOG: Method "${propertyKey}" called with arguments: ${JSON.stringify(args)}`);
        const result = originalMethod.apply(this, args); // Orijinal metodu çağır
        console.log(`LOG: Method "${propertyKey}" returned: ${JSON.stringify(result)}`);
        return result;
    };

    return descriptor; // Değiştirilmiş metot descriptor'ını döndür
}

class Calculator {
    @LogMethod // calculate metoduna dekoratörü uygula
    add(a: number, b: number): number {
        return a + b;
    }

    @LogMethod // subtract metoduna dekoratörü uygula
    subtract(a: number, b: number): number {
        return a - b;
    }

    multiply(a: number, b: number): number {
        return a * b; // Bu metoda dekoratör uygulanmadı
    }
}

const calc = new Calculator();
calc.add(5, 3);
// Çıktı:
// LOG: Method "add" called with arguments: [5,3]
// LOG: Method "add" returned: 8

calc.subtract(10, 4);
// Çıktı:
// LOG: Method "subtract" called with arguments: [10,4]
// LOG: Method "subtract" returned: 6

calc.multiply(2, 6); // Herhangi bir loglama çıktısı yok
```

Bu örnekte:

* `@LogMethod` dekoratörü, `Calculator` sınıfındaki `add` ve `subtract` metotlarına uygulanmıştır.
* Dekoratör, orijinal metodu sarmalayarak, metodun çağrılmasından önce ve sonra konsola loglar yazdırır.
* Bu, metodun ana işlevselliğine dokunmadan ek bir davranış (loglama) eklememizi sağlar.

**Diğer Dekoratör Türleri (Kısaca):**

* **Sınıf Dekoratörü:** `@sealed` gibi bir dekoratör, bir sınıfın genişletilmesini veya başka şekillerde değiştirilmesini engelleyebilir.
* **Özellik Dekoratörü:** `@Required` gibi bir dekoratör, bir özelliğin boş bırakılamayacağını belirtmek için kullanılabilir (validasyon kütüphanelerinde yaygın).
* **Parametre Dekoratörü:** Bir metot parametresine özel bilgi eklemek için kullanılır (örneğin, bir API isteğinde bir parametrenin kimliğini doğrulamak).

Dekoratörler, kodda belirli kalıpları veya çapraz kesen kaygıları (cross-cutting concerns) uygulamak için güçlü bir mekanizma sağlar. Ancak deneysel bir özellik olmaları ve karmaşıklaşabilmeleri nedeniyle dikkatli kullanılmaları önerilir.

***

#### 8.3 Namespace'ler ve Modüller

TypeScript, kod organizasyonu için hem **Namespace'ler (ad alanları)** hem de **Modüller** kavramlarını sunar. Ancak modern TypeScript ve JavaScript geliştirme dünyasında, modüller namespace'lere göre çok daha yaygın ve tercih edilen yöntemdir.

**Namespace'ler (Ad Alanları)**

Namespace'ler, kodunuzu mantıksal olarak gruplandırmanın ve global çakışmaları önlemenin bir yoludur. Genellikle küçük ölçekli uygulamalarda veya tek bir dosya içinde çok sayıda global değişken ve fonksiyon olduğunda kullanılırlar.

* `namespace` anahtar kelimesiyle tanımlanır.
* İçindeki üyeler dışarıdan erişilebilir olması için `export` edilmelidir.
* Global kapsamı kirletmezler, kendi kapsamlarını oluştururlar.

```typescript
// Namespace Tanımı
namespace MathOperations {
    export function add(x: number, y: number): number {
        return x + y;
    }

    export function subtract(x: number, y: number): number {
        return x - y;
    }

    // Alt namespace'ler de olabilir
    export namespace Advanced {
        export function multiply(x: number, y: number): number {
            return x * y;
        }
    }
}

// Kullanım
console.log(MathOperations.add(10, 5));        // Çıktı: 15
console.log(MathOperations.Advanced.multiply(4, 3)); // Çıktı: 12
```

**Modüller (Modules)**

Modüller, ayrı JavaScript dosyaları aracılığıyla kodun organize edilmesidir. Her dosya kendi modülüdür ve içindeki değişkenler, fonksiyonlar, sınıflar varsayılan olarak yereldir. Dış dünyaya açmak istediğiniz her şeyi `export` etmeniz, kullanmak istediğiniz her şeyi ise `import` etmeniz gerekir.

* Her `.ts` veya `.js` dosyası varsayılan olarak bir modüldür (eğer `import` veya `export` ifadesi içeriyorsa).
* `export` ve `import` anahtar kelimeleriyle tanımlanır.
* Modern web geliştirme ve Node.js ortamlarında standart olan **ES Modülleri (ES Modules)** konseptine dayanır.
* Bağımlılıkları açıkça belirtir ve çözümler.

```typescript
// math.ts (Ayrı bir dosya)
export function add(x: number, y: number): number {
    return x + y;
}

export function subtract(x: number, y: number): number {
    return x - y;
}

export class Calculator {
    static multiply(x: number, y: number): number {
        return x * y;
    }
}

// app.ts (Başka bir dosya)
import { add, subtract, Calculator } from './math'; // './math' dosyasından import ediyoruz

console.log(add(20, 10)); // Çıktı: 30
console.log(subtract(50, 15)); // Çıktı: 35
console.log(Calculator.multiply(5, 5)); // Çıktı: 25
```

**Namespace'ler ve Modüller Arasındaki Temel Farklar ve Tercih Edilen Yaklaşım:**

| Özellik          | Namespace'ler                                       | Modüller (ES Modules)                                         |
| ---------------- | --------------------------------------------------- | ------------------------------------------------------------- |
| **Kapsam**       | Global kapsamda iç içe geçmiş nesnolar oluşturur.   | Her dosya kendi özel kapsamını oluşturur.                     |
| **Bağımlılık**   | Bağımlılıklar dolaylıdır (global referanslar).      | Bağımlılıklar açıkça `import`/`export` ile belirtilir.        |
| **Derleme**      | Tek bir JavaScript dosyasına derlenebilir.          | Her modül ayrı bir JS dosyasıdır (bundling ile birleşebilir). |
| **Modernlik**    | Eski projelerde ve bazı özel durumlarda kullanılır. | Modern web ve Node.js geliştirmenin standart yoludur.         |
| **Araç Desteği** | Tooling desteği daha zayıf olabilir.                | Otomatik tamamlama, static analiz vb. için güçlü destek.      |

**Modern TypeScript projelerinde, kesinlikle modüller tercih edilmelidir.** Modüller, daha net bağımlılık grafikleri, daha iyi kod izolasyonu, daha kolay test edilebilirlik ve modern geliştirme araçlarıyla (bundlers, tree-shaking) daha iyi entegrasyon sağlar. Namespace'ler, özellikle eski JavaScript projelerinden geçiş yaparken veya küçük, kendiliğinden çalışan kütüphaneler yazarken hala kullanılabilir, ancak yeni projelerde tercih edilmezler.

***

#### 8.4 Type Guards ve `instanceof` Operatörü

Çok biçimlilik konusunda, farklı nesnelerin aynı arayüz üzerinden işlenebildiğini görmüştük. Ancak bazı durumlarda, çalışma zamanında bir nesnenin **gerçek tipini** bilmek ve o tipe özgü bir işlem yapmak isteyebiliriz. İşte bu noktada **Type Guards (Tip Koruyucuları)** devreye girer.

**Type Guards Nedir?**

Type Guard, TypeScript'in derleme zamanında (compile-time) bir değişkenin veya parametrenin tipini daraltmasına yardımcı olan çalışma zamanı kontrolleridir. Bir koşul ifadesiyle birlikte kullanıldığında, TypeScript'e belirli bir kod bloğu içinde bir değişkenin daha spesifik bir tipte olduğunu söyler.

**`instanceof` Operatörü:**

`instanceof` operatörü, bir nesnenin belirli bir sınıfın örneği olup olmadığını kontrol etmek için kullanılan temel bir Type Guard'dır. Özellikle sınıf hiyerarşilerinde çok biçimli nesneleri işlerken kullanışlıdır.

```typescript
class AnimalLSP {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    eat(): void {
        console.log(`${this.name} is eating.`);
    }
}

class DogLSP extends AnimalLSP {
    bark(): void {
        console.log(`${this.name} barks!`);
    }
}

class CatLSP extends AnimalLSP {
    meow(): void {
        console.log(`${this.name} meows!`);
    }
}

function communicate(animal: AnimalLSP): void {
    animal.eat(); // Tüm Animal'lar yemek yiyebilir

    // `instanceof` kullanarak tip kontrolü ve özel metot çağrısı
    if (animal instanceof DogLSP) {
        // Bu blok içinde TypeScript, 'animal'ın bir 'DogLSP' olduğunu bilir
        animal.bark();
    } else if (animal instanceof CatLSP) {
        // Bu blok içinde TypeScript, 'animal'ın bir 'CatLSP' olduğunu bilir
        animal.meow();
    } else {
        console.log(`${animal.name} made a generic animal sound.`);
    }
}

const dog = new DogLSP("Buddy");
const cat = new CatLSP("Whiskers");
const genericAnimal = new AnimalLSP("Wild Creature");

communicate(dog);
// Çıktı:
// Buddy is eating.
// Buddy barks!

communicate(cat);
// Çıktı:
// Whiskers is eating.
// Whiskers meows!

communicate(genericAnimal);
// Çıktı:
// Wild Creature is eating.
// Wild Creature made a generic animal sound.
```

Yukarıdaki `communicate` fonksiyonunda:

* `animal` parametresinin başlangıç tipi `AnimalLSP`'dir.
* `if (animal instanceof DogLSP)` koşulu doğru olduğunda, TypeScript derleyici otomatik olarak `animal`'ın tipini `DogLSP`'ye daraltır. Bu sayede `animal.bark()` metodu güvenle çağrılabilir.
* Benzer şekilde `else if (animal instanceof CatLSP)` bloğunda, `animal`'ın tipi `CatLSP` olarak daraltılır ve `animal.meow()` çağrılabilir.

**Diğer Type Guard Türleri:**

*   **`typeof` operatörü:** `string`, `number`, `boolean`, `symbol`, `bigint`, `undefined`, `object`, `function` gibi temel JavaScript tiplerini kontrol etmek için kullanılır.

    ```typescript
    function printId(id: number | string): void {
        if (typeof id === 'string') {
            console.log(id.toUpperCase()); // 'id' burada string olarak bilinir
        } else {
            console.log(id.toFixed(2)); // 'id' burada number olarak bilinir
        }
    }
    printId("abc");
    printId(123.456);
    ```
*   **`in` operatörü:** Bir nesnenin belirli bir özelliğe sahip olup olmadığını kontrol etmek için kullanılır.

    ```typescript
    interface Car {
        drive(): void;
    }

    interface Plane {
        fly(): void;
    }

    function moveVehicle(vehicle: Car | Plane): void {
        if ('drive' in vehicle) {
            vehicle.drive(); // 'vehicle' burada Car olarak bilinir
        } else if ('fly' in vehicle) {
            vehicle.fly(); // 'vehicle' burada Plane olarak bilinir
        }
    }
    ```
*   **Kullanıcı Tanımlı Type Guard Fonksiyonları:** `is` anahtar kelimesiyle kendi özel tip koruyucu fonksiyonlarınızı tanımlayabilirsiniz.

    TypeScript

    ```
    interface Fish {
        swim(): void;
    }

    interface Bird {
        fly(): void;
    }

    function isFish(pet: Fish | Bird): pet is Fish {
        return (pet as Fish).swim !== undefined; // veya başka bir mantık
    }

    function move(pet: Fish | Bird): void {
        if (isFish(pet)) {
            pet.swim();
        } else {
            pet.fly();
        }
    }
    ```

**Type Guards Ne Zaman Kullanılır?**

* Polimorfik bir koleksiyonu (örneğin `Animal[]`) iterasyondan geçirirken ve her elemanın özel tipine göre farklı bir işlem yapmak istediğinizde.
* Farklı tiplerdeki verilerin geldiği bir API cevabını işlerken.
* Opsiyonel özelliklere sahip nesnelerle çalışırken (örneğin, bir özelliğin varlığını kontrol etmek).

**OOP ile İlişkisi:**

Type Guards, polimorfizm ile birlikte çalışarak, çalışma zamanında nesnelerin gerçek tiplerine göre daha spesifik davranışlar sergilemesini sağlar. Ancak, çok sayıda `instanceof` veya `typeof` kontrolü içeren kod blokları bazen OCP veya DIP ihlaline işaret edebilir. Mümkün olduğunca, polimorfizmin gücünü kullanarak (yani, her sınıfın kendi metodunu ezerek) bu tür kontrollerden kaçınmak daha temiz bir tasarım yaklaşımı olabilir. Ancak, bazı durumlarda bu kontroller kaçınılmaz veya en pratik çözüm olabilir.

***

#### 8.5 OOP ile Test Edilebilirlik

İyi bir OOP tasarımı, aynı zamanda iyi bir **test edilebilirliği** de beraberinde getirir. SOLID prensiplerine uygun tasarlanmış sınıflar ve modüller, otomatik testler (özellikle birim testleri) yazmayı çok daha kolay hale getirir.

**Test Edilebilirlik Neden Önemlidir?**

* **Hata Yakalama:** Hataları geliştirme sürecinin erken aşamalarında yakalamaya yardımcı olur.
* **Güvenilirlik:** Kodunuzun beklendiği gibi çalıştığından emin olmanızı sağlar.
* **Refactoring Güveni:** Kodda değişiklik (refactoring) yaptığınızda, mevcut testlerin geçip geçmediğini kontrol ederek, değişikliklerin başka yerleri bozmadığından emin olmanızı sağlar.
* **Tasarım Doğrulaması:** Test yazma süreci, kodunuzun tasarımındaki zayıflıkları (örneğin sıkı bağımlılıklar) ortaya çıkarabilir. Test edilemeyen kod genellikle kötü tasarlanmış koddur.

**Test Türleri:**

1. **Birim Testleri (Unit Tests):**
   * Yazılımın en küçük, izole edilebilir birimlerini (genellikle tek bir sınıf veya metot) test eder.
   * Amaç, her bir birimin doğru çalıştığından emin olmaktır.
   * **OOP Bağlamında:** SRP'ye uygun, tek sorumluluklu sınıfların birim testlerini yazmak çok daha kolaydır. DIP'yi kullanarak bağımlılıkları enjekte ettiğinizde, bir sınıfı test ederken bağımlı olduğu diğer sınıfları "mock" veya "stub" nesnelerle değiştirebilirsiniz. Bu sayede sadece test ettiğiniz birimin davranışına odaklanabilirsiniz.
2. **Entegrasyon Testleri (Integration Tests):**
   * Birden fazla birimin veya modülün bir araya gelerek doğru bir şekilde etkileşim kurduğunu test eder.
   * Örneğin, bir `UserService`'in bir `Database` sınıfıyla doğru bir şekilde etkileşime girdiğini test etmek.
3. **Uçtan Uca Testler (End-to-End / E2E Tests):**
   * Bir uygulamanın tamamını, kullanıcı bakış açısından test eder.
   * Örneğin, bir web uygulamasında bir kullanıcının kayıt olma, giriş yapma, ürün sepete ekleme gibi tüm akışı test etmek.

**Bağımlılık Enjeksiyonu (Dependency Injection - DI) ve Testlere Etkisi:**

DIP, yani Bağımlılık Tersine Çevirme Prensibi, DI ile sıkı bir şekilde ilişkilidir. DI, bir nesnenin bağımlılıklarının (ihtiyaç duyduğu diğer nesneler) dışarıdan sağlanması (enjekte edilmesi) anlamına gelir. Bu genellikle kurucu metot (constructor injection), özellik (property injection) veya metot (method injection) aracılığıyla yapılır.

```typescript
// IDatabase arayüzü ve MySQLDatabaseDIP sınıfı önceki DIP örneğinden
// interface IDatabase { ... }
// class MySQLDatabaseDIP implements IDatabase { ... }

// Bağımlılık Enjeksiyonu ile test edilebilir ProductService
class ProductServiceTestable {
    private db: IDatabase; // Soyutlamaya bağımlılık

    // Bağımlılık Constructor aracılığıyla enjekte edildi
    constructor(database: IDatabase) {
        this.db = database;
    }

    addProduct(productName: string): void {
        this.db.save(`Product: ${productName}`);
    }

    getProducts(): string {
        return this.db.read();
    }
}

// Test için Mock Database
class MockDatabase implements IDatabase {
    public savedData: string[] = [];
    public readReturn: string = "";

    connect(): void { /* do nothing for mock */ }
    save(data: string): void {
        this.savedData.push(data);
    }
    read(): string {
        return this.readReturn;
    }
}

// Birim Testi Örneği (Jest veya Vitest gibi bir test framework'ü ile)
// describe('ProductService', () => {
//     let mockDb: MockDatabase;
//     let productService: ProductServiceTestable;

//     beforeEach(() => {
//         mockDb = new MockDatabase();
//         productService = new ProductServiceTestable(mockDb); // Her test öncesi yeni mock ve servis
//     });

//     test('should add a product by calling save on the database', () => {
//         productService.addProduct('New Laptop');
//         expect(mockDb.savedData).toContain('Product: New Laptop');
//         expect(mockDb.savedData.length).toBe(1);
//     });

//     test('should retrieve products from the database', () => {
//         mockDb.readReturn = 'Product: TV, Product: Phone';
//         const products = productService.getProducts();
//         expect(products).toBe('Product: TV, Product: Phone');
//     });
// });
```

Yukarıdaki `ProductServiceTestable` sınıfı, `IDatabase` bağımlılığını kurucusundan alır. Bu sayede, testlerde gerçek bir veritabanı bağlantısı kurmaya gerek kalmadan, `MockDatabase` gibi basit bir sahte nesne enjekte ederek `ProductServiceTestable`'ın `addProduct` ve `getProducts` metotlarını izole bir şekilde test edebiliriz.

**En İyi Pratikler:**

* **Test-Driven Development (TDD):** Testleri önce yazın, sonra kodu implemente edin. Bu, test edilebilir bir tasarım yapmanızı zorlar.
* **Küçük Fonksiyonlar/Metotlar:** Küçük ve tek bir iş yapan metotlar yazın. Bunların test edilmesi daha kolaydır.
* **Bağımlılıkları Yönetin:** Bağımlılık enjeksiyonu ve arayüzler kullanarak gevşek bağlılık sağlayın.
* **Mocking/Stubbing:** Dış bağımlılıkları (veritabanı, API'ler, dosya sistemi) testlerde sahte objelerle değiştirin.
* **Test Kapsamı (Test Coverage):** Kodunuzun mümkün olduğunca yüksek bir yüzdesini testlerle kapsadığınızdan emin olun.

OOP prensipleri, test edilebilirliği tasarımın bir parçası haline getirerek daha güvenilir, bakımı kolay ve evrimleşmeye açık yazılımlar oluşturmanıza olanak tanır.

***

#### 8.6 En İyi Pratikler ve Anti-Pattern'ler

Yazılım geliştirme, sadece kod yazmakla ilgili değildir; aynı zamanda okunabilir, sürdürülebilir ve ölçeklenebilir kod yazmakla ilgilidir. Bu bölümde, TypeScript ve OOP bağlamında yaygın olarak kabul görmüş bazı **en iyi pratiklere** ve kaçınılması gereken **anti-pattern'lere** değineceğiz.

**En İyi Pratikler (Best Practices):**

1. **DRY (Don't Repeat Yourself) Prensibi:**
   * **Nedir:** Aynı bilgi sistemde birden fazla yerde tekrarlanmamalıdır. Bir bilginin tek, açık ve güvenilir bir kaynağı olmalıdır.
   * **OOP Bağlamı:** Miras alma (ortak kodun temel sınıfta toplanması), kompozisyon (farklı davranışların bir araya getirilmesi) ve mixinler (davranışların karıştırılması) gibi OOP mekanizmaları ile kod tekrarını azaltabilirsiniz. Ortak yardımcı fonksiyonları veya sınıfları oluşturun.
   * **Örnek:** Benzer validasyon mantığını farklı sınıflara kopyalamak yerine, bir `ValidatorService` sınıfı oluşturup onu kullanmak.
2. **KISS (Keep It Simple, Stupid) Prensibi:**
   * **Nedir:** Tasarımları ve implementasyonları mümkün olduğunca basit ve anlaşılır tutun. Gereksiz karmaşıklıktan kaçının.
   * **OOP Bağlamı:** Çok derin miras hiyerarşilerinden veya çok sayıda katmandan kaçının. Sınıfları ve metotları tek bir sorumlulukla sınırlayın (SRP). Mümkünse basit fonksiyonlar veya kompozisyon kullanın.
   * **Örnek:** Bir problemi çözmek için 10 farklı sınıf ve 5 arayüz tasarlamak yerine, 2-3 basit sınıf yeterliyse onu tercih edin.
3. **YAGNI (You Ain't Gonna Need It) Prensibi:**
   * **Nedir:** "Şimdi ihtiyacın olmayan bir özelliği implemente etme." Gelecekteki olası ihtiyaçlar için fazladan kod yazmaktan kaçının.
   * **OOP Bağlamı:** Aşırı genelleme yapmaktan veya henüz gerekmeyen soyutlamalar eklemekten kaçının. Sadece mevcut gereksinimleri karşılayan kodu yazın ve gerektiğinde genişletilebilir hale getirin (OCP'yi hatırlayın).
   * **Örnek:** Henüz hiç kullanılmayacak bir "plug-in mimarisi" tasarlamak yerine, mevcut ihtiyaçları karşılayan en basit çözümü uygulamak.
4. **Kompozisyon Miras Almaya Tercih Edilir (Composition over Inheritance):**
   * **Nedir:** Ortak davranışları paylaşmak için miras alma (is-a ilişkisi) yerine, kompozisyon (has-a ilişkisi) kullanmayı tercih edin.
   * **OOP Bağlamı:** Miras alma, sıkı bir bağlılık yaratabilir. Kompozisyon (bir sınıfın başka bir sınıfın örneğini içermesi), daha esnek tasarımlar sağlar. Bir nesnenin "bir şeyi var" ise, o şeyi içeren bir özelliğe sahip olmalı, ondan miras almamalıdır.
   * **Örnek:** `FlyingVehicle` diye bir sınıf oluşturup `Car` ve `Plane`'in ondan miras alması yerine, `Car` ve `Plane` sınıflarının `Engine` ve `Wing` gibi bileşenleri içermesi (kompozisyon).
5. **Kodu Anlaşılır Adlandırma ve Düzenleme:**
   * Değişken, fonksiyon, sınıf ve arayüz isimleri açık, özlü ve niyetini belli eden olmalıdır.
   * Kodunuzu mantıksal olarak düzenleyin (modüller, klasör yapıları).
6. **Immutable (Değişmez) Objeler Kullanma:**
   * Mümkün olduğunca, bir kere oluşturulduktan sonra durumu değişmeyen objeler oluşturmayı tercih edin. Bu, hataları azaltır ve kodun anlaşılmasını kolaylaştırır.

**Anti-Pattern'ler (Kaçınılması Gereken Tasarım Kötüleri):**

1. **God Object / God Class (Tanrı Nesne / Tanrı Sınıfı):**
   * **Nedir:** Tek bir sınıfın sistemin çok büyük bir kısmının sorumluluğunu üstlenmesi. Genellikle yüzlerce veya binlerce satır kod içerir ve birden fazla değişim nedeni vardır (SRP ihlali).
   * **Sonuç:** Bakımı ve anlaşılması çok zordur, test edilemez.
2. **Spaghetti Code (Spagetti Kod):**
   * **Nedir:** Mantıksal akışı takip etmenin zor olduğu, bağımlılıkları karışık ve yapılandırılmamış kod.
   * **Sonuç:** Hata ayıklama kabusu, yeni özellik eklemek neredeyse imkansız.
3. **Anemic Domain Model (Kansız Etki Alanı Modeli):**
   * **Nedir:** Sınıfların sadece veri tutması (getter/setter'lar) ve hiçbir iş mantığı içermemesi. İş mantığı başka, ayrı hizmet (service) sınıflarında bulunur.
   * **Sonuç:** OOP'nin temel amacı olan veri ve davranışın bir araya getirilmesini ihlal eder. Kapsülleme ve sorumlulukların dağılımı bozulur.
4. **Magic Numbers / Strings (Sihirli Sayılar / Dizeler):**
   * **Nedir:** Kodu içinde, anlamı net olmayan sabit sayıların veya dizelerin doğrudan kullanılması.
   * **Çözüm:** Bunları anlamlı, iyi isimlendirilmiş sabitler (const) veya enum'lar olarak tanımlayın.
5. **Tight Coupling (Sıkı Bağlılık):**
   * **Nedir:** Bir modülün veya sınıfın başka bir modülün veya sınıfın iç detaylarına aşırı derecede bağımlı olması. Birinde yapılan değişiklik diğerini doğrudan etkiler.
   * **Çözüm:** SOLID prensipleri (özellikle OCP, ISP, DIP), kompozisyon ve bağımlılık enjeksiyonu kullanarak gevşek bağlılığı teşvik edin.
6. **Reinventing the Wheel (Tekerleği Yeniden İcat Etmek):**
   * **Nedir:** Var olan, iyi test edilmiş ve popüler kütüphaneleri veya desenleri kullanmak yerine, benzer işlevselliği sıfırdan kendi başınıza yazmaya çalışmak.
   * **Çözüm:** Mevcut çözümleri araştırın ve projenize uygun olanları kullanın.

Bu en iyi pratikler ve anti-pattern'lerden kaçınma, kodunuzun kalitesini artırmak ve uzun vadede geliştirme sürecinizi çok daha verimli hale getirmek için vazgeçilmezdir.

***

Bölüm 8'i de tamamladık. Bu bölümde Mixinler, Dekoratörler, Modüller, Type Guard'lar, Test Edilebilirlik ve genel en iyi pratikler gibi ileri düzey konulara değindik.

Şimdi tüm bu öğrendiklerimizi bir araya getireceğimiz ve pratik bir senaryo üzerinde uygulayacağımız **Bölüm 9: Pratik Bir Proje Örneği**'ne geçebiliriz.

***

###
