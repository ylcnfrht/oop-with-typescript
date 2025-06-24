# Bölüm 9: Pratik Bir Proje Örneği

Bu bölümde, şimdiye kadar öğrendiğimiz tüm Nesne Yönelimli Programlama (OOP) prensiplerini (Kapsülleme, Miras Alma, Çok Biçimlilik, Soyutlama) ve SOLID prensiplerini (SRP, OCP, LSP, ISP, DIP) gerçekçi bir proje senaryosu üzerinde uygulamalı olarak göreceğiz. Bu, teorik bilgileri pratiğe dökmenize ve prensiplerin neden önemli olduğunu somut bir şekilde deneyimlemenize yardımcı olacaktır.

***

#### 9.1 Örnek Proje Girişi: Kütüphane Yönetim Sistemi

Basit bir **Kütüphane Yönetim Sistemi** tasarlayalım. Bu sistemin temel gereksinimleri şunlar olacaktır:

* **Kitaplar:** Sisteme kitaplar eklenebilmeli, kitapların bilgileri (başlık, yazar, ISBN, yayımlanma yılı, müsaitlik durumu) tutulabilmeli.
* **Kullanıcılar:** Kütüphane kullanıcıları sisteme kayıt olabilmeli, kullanıcı bilgileri (ID, ad, soyad, iletişim bilgileri) tutulabilmeli.
* **Ödünç Alma/Verme:** Kullanıcılar kitapları ödünç alabilmeli ve geri iade edebilmeli. Bir kitabın aynı anda birden fazla kullanıcı tarafından ödünç alınamayacağı kontrol edilmeli.
* **Kütüphane Yönetimi:** Kütüphaneci, kitapları ve kullanıcıları yönetebilmeli, ödünç alma/verme işlemlerini takip edebilmeli.

Bu projeyi tasarlarken özellikle şunlara dikkat edeceğiz:

* Her sınıfın tek bir sorumluluğu olması (SRP).
* Sisteme yeni öğeler (örneğin dergiler, DVD'ler) eklendiğinde mevcut kodun değiştirilmeyecek şekilde genişletilebilir olması (OCP).
* Nesnelerin beklenen şekilde yerine geçebilmesi (LSP).
* Amaca uygun küçük arayüzler kullanma (ISP).
* Modüller arası bağımlılıkları soyutlamalar üzerinden yönetme (DIP).

***

#### 9.2 OOP Prensiplerini Uygulama: Proje Tasarımı

Projemiz için ana sınıfları ve arayüzleri belirleyelim ve hangi prensipleri nasıl uygulayacağımızı düşünelim:

**1. Varlık Sınıfları (Entity Classes):**

* `Book`: Bir kitabın özelliklerini (başlık, yazar, ISBN, yayın yılı, mevcut mu?) tutar. Kapsülleme uygulanır.
* `User`: Bir kullanıcının özelliklerini (ID, ad, soyad, iletişim) tutar. Kapsülleme uygulanır.
* `Loan`: Bir ödünç alma işleminin detaylarını (kitap, kullanıcı, ödünç alma tarihi, iade tarihi) tutar.

**2. Servis Sınıfları (Service Classes) ve Arayüzler:**

* **Veri Depolama Katmanı için Soyutlama (DIP):**
  * `ILibraryRepository`: Kitap, kullanıcı ve ödünç alma verilerini depolama, okuma, güncelleme ve silme (CRUD) işlemlerini tanımlayan bir arayüz. Bu sayede `LibraryService` somut bir veritabanı implementasyonuna bağımlı olmaz.
  * `InMemoryLibraryRepository`: `ILibraryRepository` arayüzünü uygulayan, verileri hafızada tutan basit bir implementasyon (testler ve basit senaryolar için).
* **İş Mantığı Servisleri (SRP):**
  * `BookService`: Kitaplarla ilgili işlemleri (ekleme, silme, arama, müsaitlik kontrolü) yönetir. `ILibraryRepository`'ye bağımlıdır (DI).
  * `UserService`: Kullanıcılarla ilgili işlemleri (ekleme, silme, arama) yönetir. `ILibraryRepository`'ye bağımlıdır (DI).
  * `LoanService`: Ödünç alma ve iade işlemlerini yönetir. `ILibraryRepository`, `BookService` ve `UserService`'e bağımlıdır (DI).
* **Genişletilebilirlik İçin Soyutlama (OCP, LSP, ISP):**
  * `ILibraryItem`: Kütüphane içindeki tüm öğeler (Kitap, Dergi vb.) için ortak bir arayüz tanımlar (`getTitle()`, `isAvailable()`). Bu, `Book` gibi somut öğelerin bu arayüzü uygulamasını sağlar ve gelecekte `Magazine` gibi yeni öğeler eklemeyi kolaylaştırır.
  * `Book` sınıfı `ILibraryItem` arayüzünü uygulayacak.

**3. Uygulama Katmanı:**

* `LibraryApp`: Kullanıcı arayüzü ile etkileşim kuran ve servisleri kullanarak iş mantığını tetikleyen ana uygulama sınıfı. Tüm servisleri Bağımlılık Enjeksiyonu ile alır.

**Sınıf İlişkileri (Basitleştirilmiş UML):**

```plant-uml
+----------------+       +-------------------+       +-----------------+
| ILibraryItem   |<------| Book              |       | LibraryService  |
+----------------+       +-------------------+       +-----------------+
| +getTitle(): string    | -title: string    |       | +addBook()      |
| +isAvailable(): boolean| -author: string   |       | +removeBook()   |
+----------------+       | -isbn: string     |       | +findBook()     |
                         | -isAvailable: boolean|<-----+               |
                         | +constructor()    |       | +isBookAvailable()|
                         | +borrow()         |       +--------^--------+
                         | +return()         |                |
                         +--------^----------+                |
                                  |                           |
+-------------------+             |                           |
| User              |             |                           |
+-------------------+             |                           |
| -id: number       |             |                           |
| -name: string     |             |                           |
| -contact: string  |             |                           |
+-------------------+             |                           |
                                  |                           |
+-------------------+             |                           |
| Loan              |             |                           |
+-------------------+             |                           |
| -book: Book       |             |                           |
| -user: User       |             |                           |
| -loanDate: Date   |             |                           |
| -returnDate: Date?|             |                           |
+-------------------+             |                           |
                                  |                           |
+-------------------+             |                           |
| ILibraryRepository|<-------------+--------------------------+
+-------------------+                                        |
| +saveBook(Book)   |                                        |
| +findBook(isbn): Book?|                                        |
| +saveUser(User)   |                                        |
| +findUser(id): User?|                                        |
| +saveLoan(Loan)   |                                        |
| +findLoan(book, user): Loan?|                                        |
| +getAllBooks(): Book[]|                                        |
| +getAllUsers(): User[]|                                        |
| +getAllLoans(): Loan[]|                                        |
+-------------------+                                        |
          ^                                                    |
          | implements                                         |
+-----------------------+                                      |
| InMemoryLibraryRepository|<----------------------------------+
+-----------------------+                                      |
                                                               |
+-------------------+                                        |
| LibraryApp        |                                        |
+-------------------+                                        |
| -bookService: BookService      |<--------------------------+
| -userService: UserService      |<--------------------------+
| -loanService: LoanService      |<--------------------------+
| +run()                     |
+----------------------------+
```

***

#### 9.3 Adım Adım Geliştirme ve Açıklamalar

Şimdi bu tasarımı TypeScript koduna dökelim.

**1. Varlık Sınıfları ve Arayüz:**

`ILibraryItem` arayüzü ve `Book` sınıfı.

```typescript
// ILibraryItem.ts
// OCP & LSP için temel soyutlama
export interface ILibraryItem {
    title: string;
    isAvailable: boolean; // Item'ın ödünç alınabilir olup olmadığı

    getTitle(): string;
    borrow(): void;      // Ödünç alındığında durumu güncelle
    returnItem(): void;  // İade edildiğinde durumu güncelle
}

// Book.ts
// Kapsülleme (private özellikler) ve ILibraryItem implementasyonu
import { ILibraryItem } from './ILibraryItem';

export class Book implements ILibraryItem {
    private _title: string;
    private _author: string;
    private _isbn: string;
    private _publicationYear: number;
    private _isAvailable: boolean;

    constructor(title: string, author: string, isbn: string, publicationYear: number) {
        this._title = title;
        this._author = author;
        this._isbn = isbn;
        this._publicationYear = publicationYear;
        this._isAvailable = true; // Varsayılan olarak kitap müsait
    }

    // Getter'lar
    get title(): string { return this._title; }
    get author(): string { return this._author; }
    get isbn(): string { return this._isbn; }
    get publicationYear(): number { return this._publicationYear; }
    get isAvailable(): boolean { return this._isAvailable; }

    // ILibraryItem arayüz metotları implementasyonu
    getTitle(): string {
        return this._title;
    }

    borrow(): void {
        if (!this._isAvailable) {
            throw new Error(`Book "${this._title}" (ISBN: ${this._isbn}) is already borrowed.`);
        }
        this._isAvailable = false;
        console.log(`Book "${this._title}" borrowed.`);
    }

    returnItem(): void {
        if (this._isAvailable) {
            throw new Error(`Book "${this._title}" (ISBN: ${this._isbn}) is not currently borrowed.`);
        }
        this._isAvailable = true;
        console.log(`Book "${this._title}" returned.`);
    }

    displayBookInfo(): void {
        console.log(`Title: ${this._title}, Author: ${this._author}, ISBN: ${this._isbn}, Year: ${this._publicationYear}, Available: ${this._isAvailable ? 'Yes' : 'No'}`);
    }
}

// User.ts
// Kapsülleme
export class User {
    private _userId: number;
    private _firstName: string;
    private _lastName: string;
    private _contactInfo: string;

    constructor(userId: number, firstName: string, lastName: string, contactInfo: string) {
        this._userId = userId;
        this._firstName = firstName;
        this._lastName = lastName;
        this._contactInfo = contactInfo;
    }

    get userId(): number { return this._userId; }
    get firstName(): string { return this._firstName; }
    get lastName(): string { return this._lastName; }
    get contactInfo(): string { return this._contactInfo; }

    get fullName(): string {
        return `${this._firstName} ${this._lastName}`;
    }

    displayUserInfo(): void {
        console.log(`User ID: ${this._userId}, Name: ${this.fullName}, Contact: ${this._contactInfo}`);
    }
}

// Loan.ts
// Kapsülleme
import { Book } from './Book';
import { User } from './User';

export class Loan {
    private _book: Book;
    private _user: User;
    private _loanDate: Date;
    private _returnDate: Date | null;

    constructor(book: Book, user: User, loanDate: Date) {
        this._book = book;
        this._user = user;
        this._loanDate = loanDate;
        this._returnDate = null; // Başlangıçta iade edilmemiş
    }

    get book(): Book { return this._book; }
    get user(): User { return this._user; }
    get loanDate(): Date { return this._loanDate; }
    get returnDate(): Date | null { return this._returnDate; }

    setReturnDate(date: Date): void {
        this._returnDate = date;
    }

    displayLoanInfo(): void {
        console.log(`Book: "${this._book.title}" (ISBN: ${this._book.isbn})`);
        console.log(`User: ${this._user.fullName} (ID: ${this._user.userId})`);
        console.log(`Loan Date: ${this._loanDate.toLocaleDateString()}`);
        if (this._returnDate) {
            console.log(`Return Date: ${this._returnDate.toLocaleDateString()}`);
        } else {
            console.log(`Status: Currently borrowed`);
        }
    }
}
```

**2. Veri Depolama Katmanı (DIP):**

`ILibraryRepository` arayüzü ve `InMemoryLibraryRepository` implementasyonu.

```typescript
// ILibraryRepository.ts
// DIP için soyutlama
import { Book } from './Book';
import { User } from './User';
import { Loan } from './Loan';

export interface ILibraryRepository {
    // Kitap işlemleri
    saveBook(book: Book): void;
    findBookByISBN(isbn: string): Book | undefined;
    getAllBooks(): Book[];
    removeBook(isbn: string): void;

    // Kullanıcı işlemleri
    saveUser(user: User): void;
    findUserById(userId: number): User | undefined;
    getAllUsers(): User[];
    removeUser(userId: number): void;

    // Ödünç alma işlemleri
    saveLoan(loan: Loan): void;
    findActiveLoan(bookIsbn: string, userId: number): Loan | undefined;
    getAllLoans(): Loan[];
    // findLoanByBook(bookIsbn: string): Loan | undefined; // Geliştirilebilir
    // findLoansByUser(userId: number): Loan[] // Geliştirilebilir
}

// InMemoryLibraryRepository.ts
// ILibraryRepository implementasyonu
import { ILibraryRepository } from './ILibraryRepository';
import { Book } from './Book';
import { User } from './User';
import { Loan } from './Loan';

export class InMemoryLibraryRepository implements ILibraryRepository {
    private books: Map<string, Book> = new Map();
    private users: Map<number, User> = new Map();
    // Anahtar: `${book.isbn}-${user.userId}` (benzersiz ödünç alma için)
    private loans: Map<string, Loan> = new Map();

    saveBook(book: Book): void {
        this.books.set(book.isbn, book);
        console.log(`[Repo] Book "${book.title}" saved.`);
    }

    findBookByISBN(isbn: string): Book | undefined {
        return this.books.get(isbn);
    }

    getAllBooks(): Book[] {
        return Array.from(this.books.values());
    }

    removeBook(isbn: string): void {
        this.books.delete(isbn);
        console.log(`[Repo] Book with ISBN ${isbn} removed.`);
    }

    saveUser(user: User): void {
        this.users.set(user.userId, user);
        console.log(`[Repo] User "${user.fullName}" saved.`);
    }

    findUserById(userId: number): User | undefined {
        return this.users.get(userId);
    }

    getAllUsers(): User[] {
        return Array.from(this.users.values());
    }

    removeUser(userId: number): void {
        this.users.delete(userId);
        console.log(`[Repo] User with ID ${userId} removed.`);
    }

    saveLoan(loan: Loan): void {
        const key = `${loan.book.isbn}-${loan.user.userId}`;
        this.loans.set(key, loan);
        console.log(`[Repo] Loan for "${loan.book.title}" by "${loan.user.fullName}" saved.`);
    }

    findActiveLoan(bookIsbn: string, userId: number): Loan | undefined {
        const key = `${bookIsbn}-${userId}`;
        const loan = this.loans.get(key);
        // Sadece aktif (iade edilmemiş) ödünçleri döndür
        return loan && !loan.returnDate ? loan : undefined;
    }

    getAllLoans(): Loan[] {
        return Array.from(this.loans.values());
    }
}
```

**3. Servis Sınıfları (SRP, DIP):**

`BookService`, `UserService`, `LoanService`.

```typescript
// BookService.ts
// SRP: Kitaplarla ilgili iş mantığı
import { Book } from './Book';
import { ILibraryRepository } from './ILibraryRepository';

export class BookService {
    private repository: ILibraryRepository; // DIP: Arayüze bağımlı

    constructor(repository: ILibraryRepository) {
        this.repository = repository;
    }

    addBook(title: string, author: string, isbn: string, publicationYear: number): Book {
        if (this.repository.findBookByISBN(isbn)) {
            throw new Error(`Book with ISBN ${isbn} already exists.`);
        }
        const newBook = new Book(title, author, isbn, publicationYear);
        this.repository.saveBook(newBook);
        return newBook;
    }

    removeBook(isbn: string): void {
        const book = this.repository.findBookByISBN(isbn);
        if (!book) {
            throw new Error(`Book with ISBN ${isbn} not found.`);
        }
        if (!book.isAvailable) {
            throw new Error(`Book "${book.title}" cannot be removed as it is currently borrowed.`);
        }
        this.repository.removeBook(isbn);
        console.log(`Book "${book.title}" removed successfully.`);
    }

    findBook(isbn: string): Book | undefined {
        return this.repository.findBookByISBN(isbn);
    }

    getAllBooks(): Book[] {
        return this.repository.getAllBooks();
    }

    isBookAvailable(isbn: string): boolean {
        const book = this.repository.findBookByISBN(isbn);
        return book ? book.isAvailable : false;
    }
}

// UserService.ts
// SRP: Kullanıcılarla ilgili iş mantığı
import { User } from './User';
import { ILibraryRepository } from './ILibraryRepository';

export class UserService {
    private repository: ILibraryRepository; // DIP: Arayüze bağımlı

    constructor(repository: ILibraryRepository) {
        this.repository = repository;
    }

    addUser(userId: number, firstName: string, lastName: string, contactInfo: string): User {
        if (this.repository.findUserById(userId)) {
            throw new Error(`User with ID ${userId} already exists.`);
        }
        const newUser = new User(userId, firstName, lastName, contactInfo);
        this.repository.saveUser(newUser);
        return newUser;
    }

    removeUser(userId: number): void {
        const user = this.repository.findUserById(userId);
        if (!user) {
            throw new Error(`User with ID ${userId} not found.`);
        }
        // Daha karmaşık senaryoda: kullanıcının aktif ödünçleri olup olmadığını kontrol et
        this.repository.removeUser(userId);
        console.log(`User "${user.fullName}" removed successfully.`);
    }

    findUser(userId: number): User | undefined {
        return this.repository.findUserById(userId);
    }

    getAllUsers(): User[] {
        return this.repository.getAllUsers();
    }
}

// LoanService.ts
// SRP: Ödünç alma/iade iş mantığı
import { Book } from './Book';
import { User } from './User';
import { Loan } from './Loan';
import { ILibraryRepository } from './ILibraryRepository';
import { BookService } from './BookService'; // DIP: Servislere bağımlılık (ama bu biraz tartışmalı, daha iyisi olabilir)
import { UserService } from './UserService';

export class LoanService {
    private repository: ILibraryRepository;
    private bookService: BookService;
    private userService: UserService;

    // DIP: Bağımlılık Enjeksiyonu
    constructor(repository: ILibraryRepository, bookService: BookService, userService: UserService) {
        this.repository = repository;
        this.bookService = bookService;
        this.userService = userService;
    }

    borrowBook(bookIsbn: string, userId: number): Loan {
        const book = this.bookService.findBook(bookIsbn);
        if (!book) {
            throw new Error(`Book with ISBN ${bookIsbn} not found.`);
        }
        if (!book.isAvailable) {
            throw new Error(`Book "${book.title}" is currently not available.`);
        }

        const user = this.userService.findUser(userId);
        if (!user) {
            throw new Error(`User with ID ${userId} not found.`);
        }

        // Kullanıcının bu kitabı zaten ödünç alıp almadığını kontrol et (aktif ödünç)
        if (this.repository.findActiveLoan(bookIsbn, userId)) {
             throw new Error(`User ${user.fullName} already has "${book.title}" on loan.`);
        }
        
        book.borrow(); // Kitabın durumunu güncelle (Kapsülleme)
        const newLoan = new Loan(book, user, new Date());
        this.repository.saveLoan(newLoan);
        console.log(`Loan recorded: "${book.title}" by ${user.fullName}.`);
        return newLoan;
    }

    returnBook(bookIsbn: string, userId: number): void {
        const book = this.bookService.findBook(bookIsbn);
        if (!book) {
            throw new Error(`Book with ISBN ${bookIsbn} not found.`);
        }

        const user = this.userService.findUser(userId);
        if (!user) {
            throw new Error(`User with ID ${userId} not found.`);
        }

        const loan = this.repository.findActiveLoan(bookIsbn, userId);
        if (!loan) {
            throw new Error(`No active loan found for book "${book.title}" by user ${user.fullName}.`);
        }

        book.returnItem(); // Kitabın durumunu güncelle (Kapsülleme)
        loan.setReturnDate(new Date()); // Ödünç kaydını güncelle
        this.repository.saveLoan(loan); // Güncellenmiş ödünç kaydını kaydet
        console.log(`Book "${book.title}" returned by ${user.fullName}.`);
    }

    getAllLoans(): Loan[] {
        return this.repository.getAllLoans();
    }
}
```

**4. Uygulama Katmanı:**

`LibraryApp` sınıfı.

```typescript
// LibraryApp.ts
import { InMemoryLibraryRepository } from './InMemoryLibraryRepository';
import { BookService } from './BookService';
import { UserService } from './UserService';
import { LoanService } from './LoanService';
import { Book } from './Book';
import { User } from './User';
import { Loan } from './Loan';
import { ILibraryItem } from './ILibraryItem'; // Polimorfizm için

export class LibraryApp {
    private bookService: BookService;
    private userService: UserService;
    private loanService: LoanService;

    constructor() {
        // Bağımlılık Enjeksiyonu: Repository ve Servisleri oluşturup birbirlerine enjekte ediyoruz.
        // Gelecekte, farklı bir repository (örneğin bir veritabanı repo'su) kolayca enjekte edilebilir.
        const repository = new InMemoryLibraryRepository();
        this.bookService = new BookService(repository);
        this.userService = new UserService(repository);
        this.loanService = new LoanService(repository, this.bookService, this.userService);
    }

    async run(): Promise<void> {
        console.log("--- Kütüphane Yönetim Sistemi Başlatılıyor ---");

        // 1. Kitaplar Ekleme
        console.log("\n--- Kitap Ekleme ---");
        const book1 = this.bookService.addBook("Sefiller", "Victor Hugo", "978-0140449432", 1862);
        const book2 = this.bookService.addBook("Suç ve Ceza", "Fyodor Dostoevsky", "978-0486415871", 1866);
        const book3 = this.bookService.addBook("Beyaz Gemi", "Cengiz Aytmatov", "978-9750711929", 1970);
        
        try {
            this.bookService.addBook("Sefiller", "Victor Hugo", "978-0140449432", 1862); // Zaten var hatası
        } catch (error: any) {
            console.error(`Error adding book: ${error.message}`);
        }

        this.bookService.getAllBooks().forEach(book => book.displayBookInfo());

        // 2. Kullanıcılar Ekleme
        console.log("\n--- Kullanıcı Ekleme ---");
        const user1 = this.userService.addUser(1, "Can", "Yılmaz", "can@example.com");
        const user2 = this.userService.addUser(2, "Ela", "Demir", "ela@example.com");

        this.userService.getAllUsers().forEach(user => user.displayUserInfo());

        // 3. Kitap Ödünç Alma İşlemleri
        console.log("\n--- Kitap Ödünç Alma ---");
        try {
            this.loanService.borrowBook(book1.isbn, user1.userId);
            this.loanService.borrowBook(book2.isbn, user2.userId);
            // Aynı kitabı tekrar ödünç alma denemesi
            this.loanService.borrowBook(book1.isbn, user1.userId);
        } catch (error: any) {
            console.error(`Error borrowing book: ${error.message}`);
        }

        console.log("\n--- Güncel Kitap Durumları ---");
        this.bookService.getAllBooks().forEach(book => book.displayBookInfo());

        console.log("\n--- Tüm Ödünçler ---");
        this.loanService.getAllLoans().forEach(loan => loan.displayLoanInfo());

        // 4. Kitap İade Etme İşlemleri
        console.log("\n--- Kitap İade Etme ---");
        try {
            this.loanService.returnBook(book1.isbn, user1.userId);
            // İade edilmiş kitabı tekrar iade etme denemesi
            this.loanService.returnBook(book1.isbn, user1.userId);
        } catch (error: any) {
            console.error(`Error returning book: ${error.message}`);
        }

        console.log("\n--- Güncel Kitap Durumları ---");
        this.bookService.getAllBooks().forEach(book => book.displayBookInfo());

        console.log("\n--- Tüm Ödünçler (Güncellenmiş) ---");
        this.loanService.getAllLoans().forEach(loan => loan.displayLoanInfo());

        // 5. Polimorfizm Örneği (ILibraryItem kullanımı)
        console.log("\n--- Polimorfik Kütüphane Öğeleri ---");
        const libraryItems: ILibraryItem[] = [];
        libraryItems.push(book1);
        libraryItems.push(book2);
        libraryItems.push(book3);

        // Gelecekte eklenebilecek bir Magazine sınıfı da buraya gelebilir
        // libraryItems.push(new Magazine(...));

        libraryItems.forEach(item => {
            console.log(`Item Title: ${item.getTitle()}, Available: ${item.isAvailable ? 'Yes' : 'No'}`);
            // if (item instanceof Book) { item.displayBookInfo(); } // Özel metotlara erişim gerektiğinde Type Guard kullanılabilir
        });
        
        // 6. Kitap Silme İşlemi
        console.log("\n--- Kitap Silme ---");
        try {
            this.bookService.removeBook(book3.isbn);
            // Ödünç alınmış kitabı silme denemesi (şu anda book2 ödünçte)
            this.bookService.removeBook(book2.isbn);
        } catch (error: any) {
            console.error(`Error removing book: ${error.message}`);
        }
        
        console.log("\n--- Kalan Kitaplar ---");
        this.bookService.getAllBooks().forEach(book => book.displayBookInfo());
    }
}

// Uygulamayı çalıştırma
const app = new LibraryApp();
app.run();
```

**Uygulanan OOP ve SOLID Prensipleri:**

* **Kapsülleme:** `Book`, `User`, `Loan` sınıflarındaki `private` özellikler ve getter/setter'lar. Örneğin, bir kitabın müsaitlik durumu sadece `borrow()` ve `returnItem()` metotları aracılığıyla değiştirilebilir.
* **Miras Alma:** Bu örnekte doğrudan miras alma kullanılmamıştır (daha fazla varlık tipi olsaydı kullanılabilirdi, örn. `PhysicalBook` ve `EBook` temel bir `Book` sınıfından miras alabilirdi).
* **Çok Biçimlilik ve Soyutlama (Arayüzler):**
  * `ILibraryItem` arayüzü, `Book` gibi tüm kütüphane öğelerinin ortak bir `getTitle()`, `borrow()`, `returnItem()` metoduna sahip olmasını sağlar. Gelecekte `Magazine` gibi yeni öğeler eklendiğinde, `ILibraryItem` tipindeki bir koleksiyonda polimorfik olarak işlenebilirler (OCP, LSP).
  * `ILibraryRepository` arayüzü, veri depolama mantığını soyutlar.
* **SRP (Tek Sorumluluk Prensibi):**
  * `Book`, `User`, `Loan` sınıfları sadece veri varlıklarını temsil eder.
  * `BookService`, `UserService`, `LoanService` sınıflarının her birinin tek bir sorumluluğu vardır: sırasıyla kitap, kullanıcı ve ödünç alma/verme işlemlerinin iş mantığını yönetmek.
  * `InMemoryLibraryRepository` sadece verileri depolama/yönetme sorumluluğuna sahiptir.
* **OCP (Açık/Kapalı Prensibi):**
  * `ILibraryRepository` sayesinde, veri depolama teknolojisini (örneğin `PostgresLibraryRepository`) değiştirmek istediğimizde, `BookService`, `UserService`, `LoanService` sınıflarını değiştirmemize gerek kalmaz. Sadece yeni implementasyonu yazıp `LibraryApp`'te enjekte etmemiz yeterlidir.
  * `ILibraryItem` sayesinde, yeni kütüphane öğeleri (örneğin `Magazine`) eklediğimizde, mevcut servis sınıflarının ana mantığını değiştirmemiz gerekmez, sadece yeni öğeyi tanıyıp `ILibraryItem` olarak kullanmaya devam edebiliriz.
* **DIP (Bağımlılık Tersine Çevirme Prensibi):**
  * `BookService`, `UserService`, `LoanService` sınıfları, somut `InMemoryLibraryRepository` yerine `ILibraryRepository` arayüzüne bağımlıdır.
  * `LibraryApp` sınıfı, servis sınıflarını oluştururken bu bağımlılıkları enjekte eder (Dependency Injection). Bu, test edilebilirliği ve esnekliği artırır.

***

#### 9.4 Proje Sonucu ve Değerlendirme

Bu Kütüphane Yönetim Sistemi örneği, SOLID prensiplerinin ve OOP kavramlarının gerçek bir senaryoda nasıl bir araya geldiğini göstermektedir.

**Elde Edilen Avantajlar:**

* **Esneklik:** Veri depolama katmanını (örneğin in-memory'den bir veritabanına) değiştirmek kolaydır.
* **Genişletilebilirlik:** Yeni kütüphane öğesi türleri (dergi, DVD) eklemek, `ILibraryItem` arayüzünü uygulayarak mevcut kodu değiştirmeden sisteme dahil edilebilir.
* **Bakım Kolaylığı:** Her sınıfın tek bir sorumluluğu olduğu için, bir hatayı bulmak veya bir iş kuralını değiştirmek daha kolaydır.
* **Test Edilebilirlik:** Bağımlılık Enjeksiyonu ve arayüzler sayesinde, her bir servis sınıfı izole bir şekilde test edilebilir. Örneğin, `BookService`'i test ederken gerçek bir veritabanına ihtiyaç duymadan bir `MockLibraryRepository` enjekte edilebilir.
* **Kod Organizasyonu:** Kod, mantıksal olarak varlıklar, servisler ve repository'ler olarak ayrılmıştır, bu da projenin anlaşılmasını ve yönetilmesini kolaylaştırır.

**Daha Fazla Geliştirme Fikirleri:**

Bu basit örnek, daha karmaşık gereksinimler için bir başlangıç noktasıdır. Geliştirilebilecek bazı alanlar:

* **Hata Yönetimi:** Daha detaylı ve özel hata sınıfları oluşturma.
* **Kullanıcı Kimlik Doğrulama/Yetkilendirme:** Kullanıcı rolleri (kütüphaneci, üye) ve erişim kontrolleri.
* **Veritabanı Entegrasyonu:** `ILibraryRepository` için PostgreSQL, MongoDB veya başka bir veritabanı implementasyonu yazma.
* **UI Entegrasyonu:** Bir web arayüzü (React, Angular, Vue) veya CLI (komut satırı arayüzü) ekleme.
* **Raporlama:** Ödünç alma geçmişi, en popüler kitaplar gibi raporlar oluşturma.
* **Daha Fazla Kütüphane Öğesi:** `Magazine`, `DVD` gibi `ILibraryItem` arayüzünü uygulayan yeni sınıflar ekleme.
* **Tarih Kontrolleri:** Ödünç alma süreleri, gecikme ücretleri gibi mantıklar ekleme.

Bu proje örneği, OOP ve SOLID prensiplerinin sadece teorik kavramlar olmadığını, aynı zamanda gerçek dünya yazılımı geliştirirken karşılaşılan zorluklarla başa çıkmak için güçlü araçlar olduğunu göstermektedir.

***

### Ekler (Appendix)

Bu bölüm, TypeScript ile yazılım geliştirme yolculuğunuzda size yardımcı olacak ek kaynakları ve kurulum bilgilerini içermektedir.

***

#### A.1 TypeScript Geliştirme Ortamı Kurulumu

TypeScript ile kod yazmaya başlamadan önce, uygun bir geliştirme ortamına sahip olmanız gerekir. Aşağıdaki adımlar, temel bir TypeScript geliştirme ortamını kurmanıza yardımcı olacaktır.

**1. Node.js ve npm (Node Package Manager) Kurulumu:**

TypeScript derleyicisi ve birçok geliştirme aracı Node.js ortamında çalışır. npm ise Node.js paketlerini yönetmek için kullanılır.

* **Node.js İndirme:** Node.js resmi web sitesine gidin: [https://nodejs.org/](https://nodejs.org/)
* **Önerilen Sürüm:** Genellikle "LTS" (Long Term Support - Uzun Süreli Destek) etiketli sürümü indirmeniz önerilir, çünkü daha kararlıdır.
* **Kurulum:** İndirdiğiniz yükleyiciyi çalıştırın ve varsayılan seçeneklerle kurulumu tamamlayın. Bu işlem npm'i de otomatik olarak kuracaktır.
*   **Doğrulama:** Kurulumun başarılı olup olmadığını kontrol etmek için komut istemcinizi (Terminal veya Komut İstemi) açın ve aşağıdaki komutları çalıştırın:Bash

    ```
    node -v
    npm -v
    ```

    Bu komutlar Node.js ve npm'in yüklü sürümlerini göstermelidir.

**2. Visual Studio Code (VS Code) Kurulumu:**

VS Code, TypeScript geliştirme için en popüler ve güçlü kod düzenleyicilerinden biridir. TypeScript için mükemmel dahili desteğe sahiptir.

* **VS Code İndirme:** VS Code resmi web sitesine gidin: [https://code.visualstudio.com/](https://code.visualstudio.com/)
* **Platform Seçimi:** İşletim sisteminize (Windows, macOS, Linux) uygun sürümü indirin.
* **Kurulum:** İndirdiğiniz yükleyiciyi çalıştırın ve kurulumu tamamlayın.
* **Önemli VS Code Uzantıları (Extensions):**
  * **Prettier - Code formatter:** Kodunuzu otomatik olarak biçimlendirerek tutarlı bir kod stili sağlar.
  * **ESLint:** JavaScript/TypeScript kodunuzdaki hataları ve stil sorunlarını bulmanıza yardımcı olur.
  * **GitLens:** Git entegrasyonunu geliştirir, kod geçmişini daha görünür hale getirir.
  * **Path Intellisense:** Dosya yollarını tamamlarken yardımcı olur.

**3. TypeScript Derleyicisi Kurulumu:**

TypeScript kodunuzu JavaScript'e dönüştürmek için TypeScript derleyicisine (tsc) ihtiyacınız vardır. Bunu npm aracılığıyla global olarak yükleyebilirsiniz.

*   Komut istemcinizi açın ve aşağıdaki komutu çalıştırın:Bash

    ```
    npm install -g typescript
    ```
*   **Doğrulama:** Kurulumun başarılı olup olmadığını kontrol etmek için:Bash

    ```
    tsc -v
    ```

    Bu komut TypeScript derleyicisinin yüklü sürümünü göstermelidir.

**4. `tsconfig.json` Dosyası Oluşturma:**

Her TypeScript projesinde, derleyici seçeneklerini yapılandırmak için bir `tsconfig.json` dosyası bulunur.

* Projenizin kök dizininde (veya yeni bir proje klasörü oluşturun) komut istemcisini açın.
*   Aşağıdaki komutu çalıştırarak varsayılan bir `tsconfig.json` dosyası oluşturun:Bash

    ```
    tsc --init
    ```
* Bu komut, projenizin gereksinimlerine göre özelleştirebileceğiniz bir `tsconfig.json` dosyası oluşturacaktır. Özellikle `outDir` (derlenmiş JS dosyalarının nereye kaydedileceği), `rootDir` (kaynak TypeScript dosyalarının nerede olduğu), `target` (hangi ES sürümüne derleneceği) ve `module` (hangi modül sisteminin kullanılacağı) gibi ayarları kontrol etmek isteyebilirsiniz.
  * Dekoratörler kullanacaksanız, `experimentalDecorators: true` ve `emitDecoratorMetadata: true` ayarlarını açmayı unutmayın.

**Artık Hazırsınız!**

Basit bir `.ts` dosyası oluşturun (örneğin `app.ts`):

```typescript
// app.ts
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet(): string {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
console.log(greeter.greet());
```

Komut istemcinizde bu dosyayı derleyin:

Bash

```
tsc app.ts
```

Bu, `app.js` adında bir JavaScript dosyası oluşturacaktır. Oluşan `.js` dosyasını Node.js ile çalıştırabilirsiniz:

Bash

```
node app.js
```

Tebrikler! TypeScript geliştirme ortamınız artık hazır.

***

#### A.2 Temel Git Komutları

Git, projelerin versiyon kontrolü için vazgeçilmez bir araçtır. Özellikle işbirliği içinde çalışırken veya kodunuzun geçmişini takip etmek istediğinizde hayati öneme sahiptir. Burada, en temel ve sık kullanılan Git komutlarını bulacaksınız.

**1. Git Kurulumu:**

* Git'in resmi web sitesinden indirin ve kurun: [https://git-scm.com/downloads](https://git-scm.com/downloads)
* Kurulumu doğrulamak için terminalde: `git --version`

**2. Git Temelleri:**

*   **Global Kullanıcı Yapılandırması:** İlk kurulumdan sonra adınızı ve e-postanızı ayarlayın (commit'lerinizde görünecektir).

    Bash

    ```
    git config --global user.name "Adınız Soyadınız"
    git config --global user.email "email@example.com"
    ```
* **Depo (Repository) Oluşturma / Başlatma:**
  *   **Yeni bir depo başlatma:** Mevcut bir klasörü Git deposuna dönüştürür.Bash

      ```
      git init
      ```
  *   **Uzak bir depoyu klonlama:** GitHub, GitLab gibi bir yerden mevcut bir depoyu bilgisayarınıza kopyalar.Bash

      ```
      git clone <repo-url>
      ```
*   **Durum Kontrolü:** Çalışma dizininizin ve aşamalandırma alanınızın (staging area) durumunu gösterir.

    Bash

    ```
    git status
    ```
*   **Dosyaları Aşamalandırma (Staging Area'ya Ekleme):** Değişiklikleri bir sonraki commit'e dahil etmek için hazırlama.

    Bash

    ```
    git add <dosya_adı>        # Belirli bir dosyayı ekle
    git add .                 # Tüm değişiklikleri (yeni ve değiştirilmiş) ekle
    ```
*   **Değişiklikleri Kaydetme (Commit):** Aşamalandırılmış değişiklikleri depoya kalıcı olarak kaydeder.

    Bash

    ```
    git commit -m "Mesajınız buraya"   # Mesajla birlikte commit et
    ```
*   **Değişiklikleri Uzak Depoya Gönderme (Push):** Yerel commit'leri uzak depoya (örneğin GitHub) yükler.

    Bash

    ```
    git push origin <dal_adı>   # Örneğin: git push origin main
    ```
*   **Değişiklikleri Uzak Depodan Çekme (Pull):** Uzak depodaki değişiklikleri yerel deponuza indirir ve birleştirir.

    Bash

    ```
    git pull origin <dal_adı>   # Örneğin: git pull origin main
    ```

**3. Dallanma (Branching) ve Birleştirme (Merging):**

*   **Dalları Listeleme:**

    Bash

    ```
    git branch          # Yerel dalları listeler
    git branch -a       # Tüm (yerel ve uzak) dalları listeler
    ```
*   **Yeni Dal Oluşturma:**

    Bash

    ```
    git branch <yeni_dal_adı>
    ```
*   **Dala Geçme:**

    Bash

    ```
    git checkout <dal_adı>      # Mevcut çalışma dizinini o dala taşır
    git switch <dal_adı>        # Git 2.23+ ile önerilen daha yeni komut
    ```
*   **Yeni Dal Oluşturma ve Hemen Geçme:**

    Bash

    ```
    git checkout -b <yeni_dal_adı>
    git switch -c <yeni_dal_adı>
    ```
*   **Dalları Birleştirme (Merge):** Bir dalın değişikliklerini diğerine birleştirir.

    Bash

    ```
    # Önce ana dala geçin (örneğin main)
    git checkout main
    # Sonra diğer dalı (örneğin feature-x) main'e birleştirin
    git merge <birleştirilecek_dal_adı>
    ```
*   **Dal Silme:**

    Bash

    ```
    git branch -d <silinecek_dal_adı>   # Dal birleştirildiyse siler
    git branch -D <silinecek_dal_adı>   # Dal birleştirilmemiş olsa bile zorla siler
    ```

**4. Geçmişi İnceleme:**

*   **Commit Geçmişini Görme:**&#x42;ash

    ```
    git log                       # Tüm commit'leri gösterir
    git log --oneline             # Her commit'i tek satırda gösterir
    git log --graph --oneline     # Dallanma ve birleşmeleri grafikle gösterir
    ```

**5. Geri Alma İşlemleri (Temel):**

*   **Dosyadaki değişiklikleri geri alma (unstage):** Aşamalandırılmış dosyaları aşamalandırma alanından çıkarır.Bash

    ```
    git reset HEAD <dosya_adı>    # Veya sadece `git restore --staged <dosya_adı>`
    ```
*   **Çalışma dizinindeki değişiklikleri atma:** Kaydedilmemiş değişiklikleri geri alır.Bash

    ```
    git checkout -- <dosya_adı>   # Veya sadece `git restore <dosya_adı>`
    ```

Bu komutlar, Git'in sunduğu temel işlevlerdir. Git, çok daha kapsamlı bir araçtır, ancak bu komutlarla çoğu günlük versiyon kontrolü görevini rahatlıkla yerine getirebilirsiniz.

***

#### A.3 Önerilen Kaynaklar

TypeScript, Nesne Yönelimli Programlama ve genel yazılım mühendisliği becerilerinizi geliştirmek için faydalı olabilecek bazı kaynaklar aşağıda listelenmiştir.

**1. TypeScript Resmi Kaynaklar:**

* **TypeScript Belgeleri (Resmi Dokümantasyon):** Öğrenmek ve referans almak için en iyi kaynaktır.
  * [https://www.typescriptlang.org/docs/](https://www.typescriptlang.org/docs/)
* **TypeScript Playground:** Tarayıcınızda TypeScript kodu yazabilir, derlenmiş JavaScript çıktısını görebilir ve farklı derleyici seçeneklerini deneyebilirsiniz.
  * [https://www.typescriptlang.org/play](https://www.typescriptlang.org/play)

**2. Nesne Yönelimli Programlama (OOP) ve Tasarım Desenleri:**

* **Kitaplar:**
  * **"Design Patterns: Elements of Reusable Object-Oriented Software" (GoF - Gang of Four):** Tasarım desenlerinin klasikleridir. Başlangıç için ağır olabilir ama temel referanstır.
  * **"Head First Design Patterns":** Daha anlaşılır ve görsel bir yaklaşımla tasarım desenlerini öğreten harika bir kitaptır.
  * **"Clean Code: A Handbook of Agile Software Craftsmanship" by Robert C. Martin (Uncle Bob):** İyi kod yazma prensipleri ve SOLID dahil birçok konuyu kapsar.
  * **"Clean Architecture: A Craftsman's Guide to Software Structure and Design" by Robert C. Martin:** Daha yüksek seviye mimari prensipleri ve SOLID'in bağlamını açıklar.
* **Online Kaynaklar/Makaleler:**
  * **Refactoring.Guru:** Tasarım desenleri, SOLID prensipleri ve kod refactoring teknikleri için görsel ve anlaşılır açıklamalar sunar.
  * [https://refactoring.guru/design-patterns/typescript](https://refactoring.guru/design-patterns/typescript) (TypeScript özelinde tasarım desenleri)
  * [https://refactoring.guru/principles/solid-principles](https://www.google.com/search?q=https://refactoring.guru/principles/solid-principles) (SOLID prensipleri)
  * **Microsoft Learn / MDN Web Docs:** JavaScript ve genel web geliştirme prensipleri için güvenilir kaynaklar.

**3. Online Kurslar:**

* **Udemy, Coursera, Pluralsight, Frontend Masters:** Bu platformlarda, hem TypeScript hem de genel OOP/Tasarım Desenleri konularında birçok kaliteli kurs bulabilirsiniz. Kursları seçerken eğitmenin deneyimine ve yorumlara dikkat edin.

**4. Bloglar ve Topluluklar:**

* **Dev.to, Medium, Hashnode:** Geliştiricilerin deneyimlerini ve bilgilerini paylaştığı popüler blog platformları. "TypeScript OOP", "SOLID Principles TypeScript" gibi aramalarla birçok makale bulabilirsiniz.
* **Stack Overflow:** Karşılaştığınız teknik sorunlar için çözüm bulabileceğiniz en büyük geliştirici topluluklarından biridir.
* **GitHub:** Açık kaynaklı projeleri inceleyerek, diğer geliştiricilerin kodlarını nasıl yapılandırdığını ve prensipleri nasıl uyguladığını görebilirsiniz.

**5. Test Etme:**

* **Jest:** JavaScript/TypeScript projeleri için popüler bir test framework'ü.
  * [https://jestjs.io/](https://jestjs.io/)
* **Vitest:** Vite üzerine kurulu, hızlı ve Jest uyumlu bir test framework'ü.
  * [https://vitest.dev/](https://vitest.dev/)
* **Testing Library (React, Vue, Angular için):** Bileşenleri kullanıcı bakış açısından test etmeye odaklanan bir dizi yardımcı.
  * [https://testing-library.com/](https://testing-library.com/)

Unutmayın, öğrenme sürekli bir süreçtir. Bu kaynakları kullanarak bilginizi pekiştirin ve pratik yapmaya devam edin.

***

#### A.4 Terimler Sözlüğü (Glossary)

Bu bölümde, OOP ve TypeScript serisi boyunca karşılaştığınız anahtar terimlerin kısa tanımlarını bulacaksınız.

* **Abstraction (Soyutlama):** Bir sistemin karmaşık detaylarını gizleyerek, yalnızca temel ve ilgili bilgileri dış dünyaya sunma prensibi. Kullanıcıdan "nasıl çalıştığını" gizleyip "ne yaptığını" gösterme.
* **Abstract Class (Soyut Sınıf):** Doğrudan nesne oluşturulamayan ve genellikle alt sınıflar için bir şablon görevi gören sınıf. Soyut metotlar içerebilir.
* **Abstract Method (Soyut Metot):** Sadece bir imza (tanım) içeren, implementasyonu (gövdesi) olmayan metot. Soyut sınıflarda bulunur ve alt sınıflar tarafından implemente edilmek zorundadır.
* **Accessor (Erişimci):** Bir sınıf özelliğine kontrollü okuma (`get`) veya yazma (`set`) erişimi sağlayan metot. Bkz. Getter, Setter.
* **Anti-Pattern (Anti-Desen):** Yaygın bir probleme yanlış veya verimsiz bir çözüm. Kaçınılması gereken bir tasarım kalıbı.
* **Class (Sınıf):** Benzer özelliklere (veri) ve davranışlara (metotlar) sahip nesneler oluşturmak için kullanılan bir şablon veya blueprint.
* **Cohesion (Uyumluluk):** Bir modülün veya sınıfın içindeki öğelerin birbirleriyle ne kadar ilişkili olduğunu ve tek bir amaca ne kadar odaklandığını gösteren bir ölçü. Yüksek uyumluluk tercih edilir.
* **Composition (Kompozisyon):** Bir sınıfın, diğer sınıfların nesnelerini özellik olarak içermesi (has-a ilişkisi). Davranışları paylaşmak için miras almaya alternatif bir yöntem.
* **Concrete Class (Somut Sınıf):** Doğrudan nesnesi oluşturulabilen ve tüm metotları implemente edilmiş (soyut olmayan) sınıf.
* **Constructor (Kurucu):** Bir sınıfın yeni bir nesnesi oluşturulduğunda otomatik olarak çağrılan özel bir metot. Nesnenin başlangıç durumunu başlatır.
* **Coupling (Bağlılık):** İki veya daha fazla modül veya sınıfın birbirine ne kadar bağımlı olduğunun bir ölçüsü. Düşük bağlılık (loose coupling) tercih edilir.
* **Decorator (Dekoratör):** Sınıflara, metotlara, özelliklere veya parametrelere metadata ekleyen veya onların davranışlarını çalışma zamanında değiştiren özel bir tür bildirim (`@` sembolü ile kullanılır).
* **Dependency (Bağımlılık):** Bir nesnenin, işini yapabilmek için başka bir nesneye veya modüle ihtiyaç duyması durumu.
* **Dependency Injection (Bağımlılık Enjeksiyonu - DI):** Bir nesnenin bağımlılıklarının dışarıdan (kurucu, metot veya özellik aracılığıyla) sağlanması prensibi. DIP'yi uygulamanın yaygın bir yolu.
* **Dependency Inversion Principle (Bağımlılık Tersine Çevirme Prensibi - DIP):** SOLID prensiplerinden biri. Üst seviye modüllerin alt seviye modüllere değil, her ikisinin de soyutlamalara bağlı olması gerektiğini belirtir.
* **Derived Class (Türetilmiş Sınıf / Alt Sınıf / Child Class):** Başka bir sınıftan miras alan sınıf.
* **Encapsulation (Kapsülleme):** Veri (özellikler) ve bu veriler üzerinde işlem yapan kodun (metotlar) tek bir birim içinde bir araya getirilmesi ve nesnenin dahili durumunun dış dünyadan gizlenmesi prensibi.
* **ES Modules (ES Modülleri):** ECMAScript standardına uygun, JavaScript ve TypeScript'te kodun modüler bir şekilde organize edilmesi için kullanılan `import`/`export` ifadeleri.
* **`extends`:** TypeScript'te bir sınıfın başka bir sınıftan miras aldığını belirtmek için kullanılan anahtar kelime.
* **`get` (Getter):** Bir sınıfın `private` veya `protected` özelliğinin değerini okumak için kullanılan özel metot.
* **Inheritance (Miras Alma):** Bir sınıfın (türetilmiş sınıf) başka bir sınıfın (temel sınıf) özelliklerini ve metotlarını devralması prensibi.
* **`implements`:** TypeScript'te bir sınıfın bir arayüzü uyguladığını belirtmek için kullanılan anahtar kelime.
* **Information Hiding (Bilgi Gizleme):** Bir nesnenin dahili detaylarının dış dünyadan saklanması. Kapsülleme ve soyutlamanın temel bir yönü.
* **Instance (Örnek):** Bir sınıfın bellekteki gerçek bir kopyası, bir nesne.
* **`instanceof`:** Çalışma zamanında bir nesnenin belirli bir sınıfın örneği olup olmadığını kontrol eden TypeScript/JavaScript operatörü (bir Type Guard).
* **Interface (Arayüz):** Bir sınıfın (veya objenin) sahip olması gereken metotları ve/veya özellikleri tanımlayan bir sözleşme. Implementasyon detayları içermez.
* **Interface Segregation Principle (Arayüz Ayırma Prensibi - ISP):** SOLID prensiplerinden biri. Müşterilerin kullanmadıkları metotlara sahip arayüzlere bağımlı olmaya zorlanmaması gerektiğini belirtir.
* **KISS (Keep It Simple, Stupid):** Yazılım tasarımlarını ve implementasyonları mümkün olduğunca basit tutmayı öneren prensip.
* **Liskov Substitution Principle (Liskov Yerine Geçme Prensibi - LSP):** SOLID prensiplerinden biri. Temel türlerin örneklerinin, türetilmiş türlerin örnekleriyle programın doğruluğunu bozmadan değiştirilebilir olması gerektiğini belirtir.
* **Method (Metot):** Bir nesnenin davranışını tanımlayan bir fonksiyon.
* **Method Overriding (Metot Ezme):** Bir türetilmiş sınıfın, temel sınıftan miras aldığı bir metodu kendi ihtiyaçlarına göre yeniden tanımlaması.
* **Mixin (Karışım):** TypeScript'te çoklu miras sorununu çözmek ve sınıflara dinamik olarak ek yetenekler kazandırmak için kullanılan bir tasarım deseni.
* **Module (Modül):** TypeScript/JavaScript'te kodu ayrı dosyalar halinde organize etmek için kullanılan bir birim (`import`/`export` ile çalışır).
* **Namespace (Ad Alanı):** Kodunuzu mantıksal olarak gruplandırmanın ve global çakışmaları önlemenin eski bir TypeScript yolu.
* **Object (Nesne):** Bir sınıfın bir örneği; veriye (özellikler) ve bu veri üzerinde çalışan metotlara sahip somut bir varlık.
* **Open/Closed Principle (Açık/Kapalı Prensibi - OCP):** SOLID prensiplerinden biri. Yazılım varlıklarının genişlemeye açık, ancak değiştirmeye kapalı olması gerektiğini belirtir.
* **Parent Class (Temel Sınıf / Üst Sınıf / Base Class):** Başka bir sınıfın miras aldığı sınıf.
* **Polymorphism (Çok Biçimlilik):** Farklı nesnelerin aynı arayüz veya temel sınıf tipi üzerinden çağrıldığında farklı davranışlar sergilemesi yeteneği.
* **`private`:** Bir sınıfın özelliğinin veya metodunun sadece tanımlandığı sınıfın içinden erişilebileceğini belirten erişim belirleyici.
* **Property (Özellik):** Bir nesnenin durumunu veya verisini tanımlayan bir değişken.
* **`protected`:** Bir sınıfın özelliğinin veya metodunun tanımlandığı sınıfın içinden ve bu sınıfı miras alan alt sınıfların içinden erişilebileceğini belirten erişim belirleyici.
* **`public`:** Bir sınıfın özelliğinin veya metodunun her yerden erişilebileceğini belirten erişim belirleyici (varsayılan).
* **Refactoring (Yeniden Düzenleme):** Yazılımın dış davranışını değiştirmeden iç yapısını iyileştirme süreci.
* **Runtime Binding (Çalışma Zamanı Bağlama):** Hangi metodun çağrılacağına programın çalışma zamanında, nesnenin gerçek tipine göre karar verilmesi. Bkz. Polymorphism.
* **`set` (Setter):** Bir sınıfın `private` veya `protected` özelliğinin değerini ayarlamak (yazmak) için kullanılan özel metot.
* **Single Responsibility Principle (Tek Sorumluluk Prensibi - SRP):** SOLID prensiplerinden biri. Bir sınıfın veya modülün yalnızca bir değişim nedeni olması gerektiğini belirtir.
* **SOLID Principles (SOLID Prensipleri):** Robert C. Martin tarafından tanımlanan beş temel tasarım prensibi (SRP, OCP, LSP, ISP, DIP) için bir akronim.
* **`super()`:** Türetilmiş bir sınıfın kurucusu içinde, üst sınıfın kurucusunu çağırmak için kullanılan anahtar kelime.
* **`super.`:** Türetilmiş bir sınıfın metotları içinde, üst sınıfın metotlarına veya özelliklerine erişmek için kullanılan anahtar kelime.
* **Type Guard (Tip Koruyucu):** TypeScript'in derleme zamanında bir değişkenin veya parametrenin tipini daha spesifik bir tipe daraltmasına yardımcı olan çalışma zamanı kontrolü (`typeof`, `instanceof`, `in` veya kullanıcı tanımlı fonksiyonlar).
* **YAGNI (You Ain't Gonna Need It):** Gelecekteki olası ihtiyaçlar için gereksiz kod yazmaktan kaçınmayı öneren prensip.
