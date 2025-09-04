## 1. Entitiy class nima uchun kerak?

---

> `Entity` - bu ma'lumotlar bazasidagi jadvalni ifodalovchi Java klassi. Har bir Entity obyekti ma'lumotlar bazasidagi
> qatorga (row) to'g'ri keladi.

**Entity Class Nega Kerak?**

1. ORM (Object-Relational Mapping)
   > Entity classlari: Java obyektlari va ma'lumotlar bazasi jadvallari o'rtasida ko'prik vazifasini bajaradi:

    ```java
    @Entity
    @Table(name = "users") // DB'dagi "users" jadvali
    public class User {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;          // DB'dagi id column
        
        @Column(name = "full_name")
        private String fullName;  // DB'dagi full_name column
        
        private String email;     // DB'dagi email column
    }
    ```
2. Ma'lumotlar Strukturasi

   Ma'lumotlarning qanday saqlanishini belgilaydi:

    ```java
    @Entity
    public class Product {
        @Id
        private Long id;
        
        @NotBlank(message = "Name is required")
        private String name;
        
        @Min(value = 0, message = "Price cannot be negative")
        private BigDecimal price;
        
        @Enumerated(EnumType.STRING)
        private ProductStatus status;
    }
    ```
3. Relationships (Bog'lanishlar)

   Jadvallar o'rtasidagi bog'lanishlarni aniqlaydi:

    ```java
    @Entity
    public class Order {
        @Id
        private Long id;
        
        // Bir order bir user ga tegishli
        @ManyToOne
        @JoinColumn(name = "user_id")
        private User user;
        
        // Bir orderda ko'p products bo'lishi mumkin
        @OneToMany(mappedBy = "order")
        private List<OrderItem> items;
    }
    ```
4. Validation

   Ma'lumotlarning to'g'riligini tekshiradi:

    ```java
    @Entity
    public class Employee {
        @Id
        private Long id;
        
        @Email(message = "Email should be valid")
        private String email;
        
        @Size(min = 2, max = 50, message = "Name must be between 2-50 characters")
        private String name;
        
        @Past(message = "Birth date must be in the past")
        private LocalDate birthDate;
    }
    ```

## 2. `Entitiy class` da eng ko'p ishlatiladigan anotatsiyalar

---

1. `@Entity` - Asosiy Anotatsiya

    ```java
    @Entity
    public class User {
        // Bu klass ma'lumotlar bazasida jadvalga aylanadi
    }
    ```

2. `@Table` - Jadval Nomini Belgilash

    ```java
    @Entity
    @Table(name = "users") // DB'dagi jadval nomi
    public class User {
        // Agar @Table ishlatilmasa, class nomi bo'yicha jadval yaratiladi
    }
    ```
3. `@Id `- Primary Key

    ```java
    @Entity
    public class User {
        @Id
        private Long id;
    }
    ```

4. `@GeneratedValue` - Auto Increment

    ```java
    @Entity
    public class User {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id; // Avtomatik raqamlanadi
    }
    ```

5. `@Column` - Column Properties

    ```java
    @Entity
    public class User {
        @Column(
            name = "full_name",          // DB column nomi
            length = 100,                // maksimal uzunlik
            nullable = false,            // null bo'lmasligi
            unique = true                // takrorlanmasligi
        )
        private String fullName;
    }
    ```

6. `@Enumerated` - Enumlarni Saqlash

    ```java
    @Entity
    public class User {
        @Enumerated(EnumType.STRING) // "ACTIVE" ko'rinishida saqlanadi
        private UserStatus status;
    }
    
    public enum UserStatus {
        ACTIVE, INACTIVE, PENDING
    }
    ```
7. `@Temporal` - Date/Time Saqlash

    ```java
    @Entity
    public class Event {
        @Temporal(TemporalType.DATE)     // Faqat sana: 2023-12-20
        private Date eventDate;
        
        @Temporal(TemporalType.TIMESTAMP) // Sana va vaqt: 2023-12-20 10:30:45
        private Date createdAt;
    }
    ```
8. `@CreationTimestamp` va `@UpdateTimestamp`

    ```java
    @Entity
    public class User {
        @CreationTimestamp
        private LocalDateTime createdAt; // Yaratilgan vaqt avtomatik
        
        @UpdateTimestamp
        private LocalDateTime updatedAt; // Yangilangan vaqt avtomatik
    }
    ```

### `Relationships` (Bog'lanishlar)

- `@OneToMany` - Birga Ko'p

    ```java
    @Entity
    public class Department {
        @OneToMany(mappedBy = "department")
        private List<Employee> employees;
    }
    ```
- `@ManyToOne` - Ko'pga Bir

    ```java
    @Entity
    public class Employee {
        @ManyToOne
        @JoinColumn(name = "department_id")
        private Department department;
    }
    ```

- `@ManyToMany` - Ko'pga Ko'p

    ```java
    @Entity
    public class Student {
        @ManyToMany
        @JoinTable(
            name = "student_course",
            joinColumns = @JoinColumn(name = "student_id"),
            inverseJoinColumns = @JoinColumn(name = "course_id")
        )
        private List<Course> courses;
    }
    ```

10. Validation Anotatsiyalari

    `@NotNull` - `Null` Bo'lmasligi

    ```java
    @Entity
    public class User {
        @NotNull(message = "Email cannot be null")
        private String email;
    }
    ```

    `@Size` - Uzunlik Cheklovi

    ```java
    @Entity
    public class Product {
        @Size(min = 2, max = 100, message = "Name must be 2-100 characters")
        private String name;
    }
    ```

    `@Email` - Email Format

    ```java
    @Entity
    public class User {
        @Email(message = "Email should be valid")
        private String email;
    }
    ```

11. `@Transient` - `DB`ga Saqlanmasin

    ```java
    @Entity
    public class User {
        private String password;
        
        @Transient // DBga saqlanmaydi, faqat objectda
        private String passwordConfirm;
    }
    ```
12. `@Lob` - Katta Ma'lumotlar

    ```java
    @Entity
    public class Document {
        @Lob // Katta textlar yoki filelar uchun
        private String content;
    }
    ```

### To'liq `Entity Class` misoli

```java

@Entity
@Table(name = "employees")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 50)
    @Column(name = "full_name", nullable = false, length = 50)
    private String fullName;

    @Email(message = "Email should be valid")
    @Column(unique = true, nullable = false)
    private String email;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private EmployeeStatus status;

    @CreationTimestamp
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;

    @UpdateTimestamp
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;

    // Constructors, getters, setters
    public Employee() {
    }

    // ... getters and setters
}

public enum EmployeeStatus {
    ACTIVE, ON_LEAVE, TERMINATED
}
```

## 3. `@NotNull` bilan `nullable = false` o'rtasidagi farqni tushuntiring

---

| Xususiyat        | `nullable = false`            | `@NotNull`                    |
|------------------|-------------------------------|-------------------------------|
| Qayerda ishlaydi | 	Database darajasida          | 	Application darajasida       |
| Vaqti            | 	Ma'lumot DBga saqlanganda    | 	Object yaratilganda          |
| Xato turi        | 	SQLException                 | 	ConstraintViolationException |
| Xabar            | 	DB xato messaji	             | Custom validation messaji     |
| Performance      | 	DBda tekshiradi              | 	Applicationda tekshiradi     |
| Required	        | Jakarta Validation kerak emas | 	Bean Validation kerak        |

## 4. Ushbu code dagi  ```@OneToMany(mappedBy = "department")```  `mappedy` nima uchun kerak?

---

> `mappedBy` - bu "bog'lanishning asl manbai qayerda ekanligini" ko'rsatish uchun ishlatiladi. U @OneToMany va
> @ManyToMany bog'lanishlarda ishlatiladi.


`Misoll`:

`Department` va `Employee` larni olsak:

```java
// Department - Bo'lim
@Entity
public class Department {
    @Id
    private Long id;
    private String name;

    @OneToMany(mappedBy = "department") // ← Bu qism
    private List<Employee> employees;
}

// Employee - Xodim  
@Entity
public class Employee {
    @Id
    private Long id;
    private String name;

    @ManyToOne
    @JoinColumn(name = "department_id") // ← Foreign key
    private Department department; // ← mappedBy shu fieldga ishora qiladi
}
```

mappedBy = "department" degani:

- > "Mening employees ro'yxatim Employee classidagi department fieldi orqali bog'langan"

📊 Database Jadval Strukturasi

Employees jadvali:

| id | name    | department_id  |
|----|---------|----------------|
| id | 	name   | 	department_id |
| 1  | 	Ali	   | 1              |
| 2  | 	Vali	  | 1              
| 3  | 	Hasan	 | 2              |

Departments jadvali:

| id | name |
|----|------|
| 1	 | IT   |
| 2  | 	HR  |

⚠️ E'tibor bering: Departments jadvalida employees haqida ma'lumot YO'Q

💡 mappedBy Nima Qiladi?

1. `Bog'lanish qayerda saqlanishini` ko'rsatadi
2. `Qaysi field orqali bog'lanishni` ko'rsatadi
3. `Database da qo'shimcha column yaratilishining oldini oladi`

❌ `mappedBy` BO'LMASA Nima Bo'lardi?

### Agar `mappedBy` bo'lmasa:

```java

@Entity
public class Department {
    @OneToMany // ❌ mappedBy yo'q
    private List<Employee> employees;
}
```

### Database:

- Departments jadvalida `employees_ids` columni yaratiladi
- Bu yomon design hisoblanadi

## Oddiy Qoida:

```java
// To'g'ri:
@OneToMany(mappedBy = "boshqaClassdagiFieldNomi")

// Misol:
@OneToMany(mappedBy = "department") // Employee classidagi field nomi
```

### Bog'lanish Yo'nalishi:

- `@ManyToOne` tomoni har doim egasi (owner) bo'ladi
- `@OneToMany` tomoni ko'rsatuvchi (inverse) bo'ladi

## Kod Misoli:

```java
// Employee.java - EGASI (OWNER)
@Entity
public class Employee {
    @ManyToOne
    @JoinColumn(name = "department_id") // ← Foreign key shu yerda
    private Department department;      // ← mappedBy shu fieldga qaraydi
}

// Department.java - KO'RSATUVCHI (INVERSE)  
@Entity
public class Department {
    @OneToMany(mappedBy = "department") // ← Employee.department fieldiga ishora
    private List<Employee> employees;
}
```

🏆 QOIDALAR:

- `mappedBy` faqat `@OneToMany` da ishlatiladi

- `mappedBy` qiymati boshqa classdagi field nomi bo'lishi kerak

- Ikkala tomonlama bog'lanishda faqat bitta tomonda `@JoinColumn` bo'ladi

- `mappedBy` qaysi tomon `inverse` ekanligini ko'rsatadi

## 5. `Relational mapping` (bog'lanishlar) haqida.

---

> `Relational Mapping` - bu ma'lumotlar bazasidagi jadvallar orasidagi bog'lanishlarni **Java objectlari** orasidagi
> bog'lanishlar sifatida ifodalash.

1. `One-to-One` (Bitta - Bittaga)

    ```java
    @Entity
    public class User {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        
        private String username;
        
        @OneToOne(mappedBy = "user", cascade = CascadeType.ALL)
        private UserProfile profile;
    }
    
    @Entity
    public class UserProfile {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        
        private String fullName;
        private String address;
        
        @OneToOne
        @JoinColumn(name = "user_id")
        private User user;
    }
    ```

   ### `@JoinColumn(name = "user_id")` - `UserProfile` tomonda
   Bu qism: `Foreign key` qaysi `column` da saqlanishini ko'rsatadi.

    ```java
    @Entity
    public class UserProfile {
        @Id
        private Long id;
        
        @OneToOne
        @JoinColumn(name = "user_id")  // ← BU QISMI
        private User user;
    }
    ```
   ### Nima Uchun Kerak?

    1. `Database Structure` - Database da `foreign key` columnini yaratadi
    2. `Relationship Owner` - Bog'lanishning "egasi" (`owner`) bo'ladi
    3. `Foreign Key` - `user_profiles` jadvalida `user_id` columni bo'ladi

   ### Database Jadvali:
   `user_profiles table`:

   | id | full_name  | address   | user_id |
                            |----|------------|-----------|---------|
   | 1  | John Doe   | Tashkent  | 1       |
   | 2  | Jane Smith | Samarkand | 2       |

   ⚠️ `E'tibor`: `user_id` columni `user_profiles` jadvalida bo'ladi

   ### 🎯 mappedBy = "user" - User Tomonda

   **Bu qism**: "Bog'lanish boshqa tomonda boshqariladi" degani.

    ```java
    @Entity
    public class User {
        @Id
        private Long id;
        
        @OneToOne(mappedBy = "user")  // ← BU QISMI
        private UserProfile profile;
    }
    ```
   `Bog'lanish boshqa tomonda boshqariladi` degani - `database` dagi `foreign key` (chet el kaliti) boshqa entityda
   joylashgan degani.

   Real Hayotdan Misol

   Keling, `User` va `UserProfile` misoliga qaytaylik:

    ```java
    @Entity
    public class User {
        @Id
        private Long id;
        private String username;
        
        @OneToOne(mappedBy = "user")  // ← "Bog'lanish UserProfile tomonda boshqariladi"
        private UserProfile profile;
    }
    
    @Entity
    public class UserProfile {
        @Id
        private Long id;
        private String fullName;
        
        @OneToOne
        @JoinColumn(name = "user_id")  // ← Foreign key SHU YERDA
        private User user;
    }
    ```

   🔍 "Boshqariladi" Nima Demak?

    1. Foreign Key Qayerda?
        - ✅ UserProfile jadvalida: user_id columni bor
        - ❌ User jadvalida: profile_id columni yo'q

    2. Qaysi Tomon "Egası" (Owner)?
        - ✅ UserProfile - Bog'lanishning EGASI
        - ❌ User - Bog'lanishning KO'RSATUVCHISI
    3. Qaysi Tomon O'zgartiradi?

       ```java
       // ✅ TO'G'RI: Foreign key egasi tomonda o'zgartirish
       UserProfile profile = new UserProfile();
       profile.setUser(user);  // ← user_id ni o'zgartiradi
       
       // ❌ NOTO'G'RI: Inverse tomonda o'zgartirish
       User user = new User();
       user.setProfile(profile); // ← Hech narsa o'zgarmaydi
       ```

   💻 Kod Misoli:

    ```java
    // 1. User yaratish
    User user = new User();
    user.setUsername("ali123");
    
    // 2. UserProfile yaratish
    UserProfile profile = new UserProfile();
    profile.setFullName("Ali Valiyev");
    
    // 3. ✅ BOG'LASH - TO'G'RI USUL
    profile.setUser(user);  // ← FOREIGN KEY NI O'ZGARTIRISH
    
    // 4. ❌ BOG'LASH - NOTO'G'RI USUL
    user.setProfile(profile); // ← BU HECH NARSA QILMAYDI
    
    // 5. Saqlash
    userRepository.save(user);
    profileRepository.save(profile); // ← FOREIGN KEY SAQLANADI
    ```
   > Oddiy Qoida:
   > - `mappedBy` - "Bog'lanish SHU YERDA EMAS, boshqa yerda"
   > - @JoinColumn - "Bog'lanish AYNAN SHU YERDA"

### Muhim Qoida:

1. `Bitta boshqaruvchi tomondan` bo'lsin
2. `Boshqaruvchi tomondan` ma'lumotlarni saqlang
3. `mappedBy` qo'llanilgan tomondan saqlash shart emas
4. `Transaction` ichida ishlating

   > Agar ikkala tomondan ham boshqaruvchi qilsangiz, `Hibernate` qaysi `foreign` keyni qayerda saqlashni bilmaydi va
   `null pointer` exceptionlar kelishi mumkin.

`Xulosa`:

`mappedBy` faqat:

- 🔧 Ma'lumotlar bazasida foreign key qayerda bo'lishini
- ✍️ Qaysi tomondan yangilanishlar saqlanishini

> Faqatgina `owner` tomonida (boshqaruvchi tomonda) yangilanishlar saqlanadi. Bu `JPA` ning muhim qoidasidir.

2. `One-to-Many` (Bitta - Ko'pga)
    ```java
    @Entity
    public class Department {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        
        private String name;
        
        @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
        private List<Employee> employees = new ArrayList<>();
    }
    
    @Entity
    public class Employee {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        
        private String name;
        
        @ManyToOne
        @JoinColumn(name = "department_id")
        private Department department;
    }
    ```

   ### Asosiy qoida:
   `mappedBy` har doim `ko'p` tomondagi `(Many)` maydon nomiga ishora qiladi

    ```java
    @Entity
    public class Department {
        @Id
        private Long id;
        
        @OneToMany(mappedBy = "department") // Employee'dagi "department" maydoni
        private List<Employee> employees;
    }
    
    @Entity
    public class Employee {
        @Id
        private Long id;
        
        @ManyToOne
        @JoinColumn(name = "dept_id")
        private Department department; // mappedBy shu maydonni ko'rsatadi
    }
    ```


3. `Many-to-Many` (Ko'p - Ko'pga)

    ```java
    @Entity
    public class Student {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        
        private String name;
        
        @ManyToMany
        @JoinTable(
            name = "student_course",
            joinColumns = @JoinColumn(name = "student_id"),
            inverseJoinColumns = @JoinColumn(name = "course_id")
        )
        private List<Course> courses = new ArrayList<>();
    }
    
    @Entity
    public class Course {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        
        private String title;
        
        @ManyToMany(mappedBy = "courses")
        private List<Student> students = new ArrayList<>();
    }
    ```
   `@ManyToMany` munosabatlarida `mappedBy` faqat bitta entityda ishlatiladi, ikkinchi `entity` esa `@JoinTable` bilan
   bog'lanishni boshqaradi.

   📝 Sintaksis

    ```java
    @Entity
    public class Student {
        @Id
        private Long id;
        
        @ManyToMany
        @JoinTable(
            name = "student_course",
            joinColumns = @JoinColumn(name = "student_id"),
            inverseJoinColumns = @JoinColumn(name = "course_id")
        )
        private List<Course> courses; // BOSHQARUVCHI TOMON
    }
    
    @Entity
    public class Course {
        @Id
        private Long id;
        
        @ManyToMany(mappedBy = "courses") // Ko'rsatuvchi tomon
        private List<Student> students;
    }
    ```    
   `@JoinTable` - Bu `JPA` annotatsiyasi bo'lib, `Many-to-Many` munosabatlarida ishlatiladigan bog'lanish jadvalini
   `(link table)` sozlash uchun ishlatiladi.
   `Link Table` - Bu ma'lumotlar bazasida ikki jadval o'rtasidagi ko'pdan-ko'p `(many-to-many)` munosabatlarini saqlash
   uchun ishlatiladigan maxsus jadval.

### `@JoinTable` dan boshqa bir nechta usullar mavjud. Keling ularni ko'rib chiqaylik:

1. 📊 Alohida Entity yaratish (Eng kuchli usul). Link jadvalini alohida entity qilish:
   ```java
   @Entity
   public class StudentCourse {
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
       
       @ManyToOne
       @JoinColumn(name = "student_id")
       private Student student;
       
       @ManyToOne
       @JoinColumn(name = "course_id")
       private Course course;
       
       private LocalDate enrollmentDate;
       private Integer grade;
       // qo'shimcha maydonlar...
   }
   
   @Entity
   public class Student {
       @Id
       private Long id;
       
       @OneToMany(mappedBy = "student")
       private List<StudentCourse> studentCourses;
   }
   
   @Entity
   public class Course {
       @Id
       private Long id;
       
       @OneToMany(mappedBy = "course")
       private List<StudentCourse> courseStudents;
   }
   ```
   💡 Afzalliklari:
    - ✅ Qo'shimcha maydonlar qo'shish mumkin (`grade`, `date`, `status`)
    - ✅ Har bir bog'lanishga individual control
    - ✅ Murakkab sorovlar yozish oson
    - ✅ Validation qo'shish mumkin

2. 🔄 `Bidirectional` @OneToMany (Composite Key bilan). Composite primary key yaratish:
3. 🗄️ Ma'lumotlar Bazasi View dan foydalanish
4. 📋 @ElementCollection (Simple Cases uchun)

## 6. `Cascade` - nima?

> `Cascade` - bu bitta `entity` ustida bajarilgan operatsiyalar avtomatik ravishda bog'liq entitylarga ham qo'
> llanilishini belgilaydi.

> `Cascade relation mapping` bilan chambarchas bog'liq. Entitylar o'rtasida bog'lanish (`OneToMany`, `ManyToOne`,
`ManyToMany`) bo'lganda, `cascade` qanday operatsiyalar avtomatik tarqalishini belgilaydi.

### `Cascade Types` (Kaskad Turlari)

```java
public enum CascadeType {
    ALL,        // Barcha operatsiyalar
    PERSIST,    // Faqat saqlash
    MERGE,      // Faqat birlashtirish
    REMOVE,     // Faqat o'chirish
    REFRESH,    // Faqat yangilash
    DETACH      // Faqat ajratish
}
```

## 7. Nima uchun ushbu code da:

```java

@Entity
public class Order {
    @Id
    @GeneratedValue
    private Long id;

    private String orderNumber;

    // ❌ CASCADE YO'Q
    @OneToMany(mappedBy = "order")
    private List<OrderItem> items = new ArrayList<>();

    public void addItem(OrderItem item) {
        items.add(item);
        item.setOrder(this);
    }
}

@Entity
public class OrderItem {
    @Id
    @GeneratedValue
    private Long id;

    private String productName;
    private Integer quantity;

    @ManyToOne
    @JoinColumn(name = "order_id")
    private Order order;
}
```

`items = new ArrayList<>()`; ishlatilgan oddiygin `items` emas!

---

🎯 1. `NullPointerException` dan himoya qilish

```java

@Entity
public class Order {
    @OneToMany(mappedBy = "order")
    private List<OrderItem> items; // ❌ Null bo'lishi mumkin

    public void addItem(OrderItem item) {
        items.add(item); // ❌ Agar items = null bo'lsa, NullPointerException
        item.setOrder(this);
    }
}
```

✅ TO'G'RI USUL:

```java

@Entity
public class Order {
    @OneToMany(mappedBy = "order")
    private List<OrderItem> items = new ArrayList<>(); // ✅ Hech qachon null bo'lmaydi

    public void addItem(OrderItem item) {
        items.add(item); // ✅ Hech qanday xatolik bo'lmaydi
        item.setOrder(this);
    }
}
```

🎯 2. `Collection Operations` ni ishlatish imkoniyati

```java
Order order = new Order();

// ✅ ArrayList metodlarini ishlatish mumkin
order.

getItems().

size();     // 0 qaytaradi
order.

getItems().

isEmpty();  // true qaytaradi  
order.

getItems().

add(item);  // Ishlaydi

// ❌ Agar null bo'lsa:
order.

getItems().

size();     // NullPointerException!
order.

getItems().

isEmpty();  // NullPointerException!
order.

getItems().

add(item);  // NullPointerException!
```

🎯 3. Lazy Initialization muammosini hal qilish

`Hibernate va Lazy Loading`:

```java
// Order ni o'qib olish
Order order = orderRepository.findById(1L).orElseThrow();

// ❌ Agar items = null bo'lsa:
for(
OrderItem item :order.

getItems()){ // ❌ NullPointerException
        System.out.

println(item.getProductName());
        }

// ✅ Agar items = new ArrayList<>() bo'lsa:
        for(
OrderItem item :order.

getItems()){ // ✅ Bo'sh list, loop ishlamaydi
        System.out.

println(item.getProductName()); // Hech narsa chiqmaydi
        }
```

🎯 4. Database dan ma'lumot kelmasa ham

```java
// Yangi Order yaratish
Order newOrder = new Order();
newOrder.

setOrderNumber("ORD-001");

// ❌ Agar items = null bo'lsa:
newOrder.

getItems().

add(new OrderItem()); // ❌ NullPointerException

// ✅ Agar items = new ArrayList<>() bo'lsa:
        newOrder.

getItems().

add(new OrderItem()); // ✅ Ishlaydi
        newOrder.

getItems().

add(new OrderItem()); // ✅ Ishlaydi
```

🎯 5. Kodni soddalashtirish va xatolarni oldini olish

❌ XATO: Har safar null tekshirish

```java
public void addItem(OrderItem item) {
    if (items == null) { // �️ Har safar tekshirish kerak
        items = new ArrayList<>();
    }
    items.add(item);
    item.setOrder(this);
}

public int getItemCount() {
    if (items == null) { // ❌ Har safar tekshirish
        return 0;
    }
    return items.size();
}
```

✅ TO'G'RI: Hech qachon null emas

```java
public void addItem(OrderItem item) {
    items.add(item); // ✅ Hech qanday tekshirish kerak emas
    item.setOrder(this);
}

public int getItemCount() {
    return items.size(); // ✅ Hech qanday tekshirish kerak emas
}
```

🎯 6. Serialization uchun

```java
// JSON ga o'tkazish
ObjectMapper mapper = new ObjectMapper();
String json = mapper.writeValueAsString(order);

// ❌ Agar items = null bo'lsa:
// {"id":1, "orderNumber":"ORD-001", "items":null}

// ✅ Agar items = new ArrayList<>() bo'lsa:  
// {"id":1, "orderNumber":"ORD-001", "items":[]}
```

🎯 7. Best Practice - Defensive Programming

```java

@Entity
public class Order {
    // ✅ YAXSHI: Har doim initialized
    private List<OrderItem> items = new ArrayList<>();

    // ✅ YAXSHI: Null qaytarmaslik
    public List<OrderItem> getItems() {
        return Collections.unmodifiableList(items); // yoki
        // return new ArrayList<>(items); // defensive copy
    }

    // ✅ YAXSHI: Null qabul qilmaslik
    public void setItems(List<OrderItem> items) {
        this.items = items != null ? new ArrayList<>(items) : new ArrayList<>();
    }
}
```

📊 XULOSA:

| Scenario       | 	List<OrderItem> items; (null) | 	List<OrderItem> items = new ArrayList<>(); |
|----------------|--------------------------------|---------------------------------------------|
| Yangi object   | 	❌ NullPointerException        | 	✅ Ishlaydi                                 |
| DB dan o'qish  | 	❌ Lazy loading issues         | 	✅ Ishlaydi                                 |
| Collection ops | 	❌ Har doim null tekshirish    | 	✅ To'g'ridan-to'g'ri                       |
| Kod sifati     | 	❌ Murakkab va xatolar         | 	✅ Oddiy va ishonchli                       |
| Serialization  | 	❌ "items": null               | 	✅ "items": []                              |

**Xulosa:** `new ArrayList<>()` ishlatish - bu defensive programmingning eng yaxshi amaliyoti. Bu sizning kodingizni:

- ✅ Xavfsizroq qiladi (`NullPointerException` dan himoya)
- ✅ Soddaroq qiladi (doimiy null tekshirishlarsiz)
- ✅ Ishonchliroq qiladi (har qanday holatda ishlaydi)
- ✅ Tozalroq qiladi (kod ko'rinishi yaxshilanadi)

## 8. `Collectionlarni initialize`, `Lazy Initialization`, `Collection Operations` va

`Collection Operations` atamalari haqida ma'lumot ber

---

1. `Collection` larni `Initialize` Qilish (Boshlang'ich Qiymat Berish)

   Nima bu?

   **Collectionlarni initialize qilish** - bu `collection` o'zgaruvchisiga dastlabki (boshlang'ich) qiymat berish,
   odatda bo'sh `collection` yaratish.

   `Misollar`:

    ```java
    // ✅ TO'G'RI - Initialize qilingan
    List<String> names = new ArrayList<>(); // Bo'sh ro'yxat
    Set<Integer> numbers = new HashSet<>(); // Bo'sh to'plam
    Map<String, Integer> scores = new HashMap<>(); // Bo'sh map
    
    // ❌ XATO - Initialize qilinmagan
    List<String> names; // null
    Set<Integer> numbers; // null  
    Map<String, Integer> scores; // null
    ```

   `Nega Muhim?`

    ```java
    // ❌ XATO: NullPointerException keladi
    List<String> items;
    items.add("book"); // ❌ ERROR: items is null
    
    // ✅ TO'G'RI: Ishlaydi
    List<String> items = new ArrayList<>();
    items.add("book"); // ✅ OK: items is empty list
    ```

2. `Lazy Initialization`

   `Lazy Initialization` - bu `object` yoki `resource` kerak bo'lgandagina yaratish printsipi. Dastur ishga tushganda
   emas, balki birinchi marta talab qilinganda yaratiladi.

   `Collectionlarda Lazy Initialization`:

    ```java
    @Entity
    public class Order {
        private List<OrderItem> items; // ❌ Null bo'lishi mumkin
        
        // Lazy initialization - faqat kerak bo'lganda yaratish
        public List<OrderItem> getItems() {
            if (items == null) {
                items = new ArrayList<>(); // ✅ Birinchi marta chaqirilganda yaratiladi
            }
            return items;
        }
    }
    ```

   `Afzalliklari`:
    - `Memory tejash` - Kerak bo'lmaganda yaratilmaydi
    - `Performance` - Dastur tezroq ishga tushadi

   `Kamchiliklari`:
    - `Null tekshirish` - Har safar tekshirish kerak
    - `Thread safety` - Ko'p threadli muhitda muammoli


3. `Collection Operations` (Collection Amallari)

   Nima bu?

   `Collection Operations` - bu collectionlar ustida bajariladigan turli xil amallar va operatsiyalar.

   Asosiy Collection Amallari:

    1. Qo'shish/OLchirish (`Add/Remove`)

       ```java
       List<String> fruits = new ArrayList<>();
       fruits.add("apple");    // ✅ Qo'shish
       fruits.add("banana");
       fruits.remove("apple"); // ✅ O'chirish
       ```
    2. O'lcham/Bo'shligini tekshirish

       ```java
       List<Integer> numbers = new ArrayList<>();
       numbers.size();     // ✅ Elementlar soni
       numbers.isEmpty();  // ✅ Bo'shligini tekshirish
       numbers.contains(5);// ✅ Mavjudligini tekshirish
       ```

    3. Iteratsiya (Loop)

       ```java
       List<String> names = new ArrayList<>();
       // For-each loop
       for (String name : names) {
           System.out.println(name);
       }
       
       // Iterator
       Iterator<String> iterator = names.iterator();
       while (iterator.hasNext()) {
           System.out.println(iterator.next());
       }
       ```

    4. Sortlash va qidirish

        ```java
        List<Integer> numbers = new ArrayList<>();
        numbers.sort(Comparator.naturalOrder()); // ✅ Sortlash
        Collections.sort(numbers);               // ✅ Sortlash
        int index = numbers.indexOf(5);          // ✅ Qidirish
        ```

    5. Stream Operations

        ```java
        List<String> names = new ArrayList<>();
        // Filter
        List<String> longNames = names.stream()
            .filter(name -> name.length() > 5)
            .collect(Collectors.toList());
        
        // Map
        List<Integer> nameLengths = names.stream()
            .map(String::length)
            .collect(Collectors.toList());
        ```

📊 XULOSA:

| Atama                 | 	Nima Bu?                                | 	Nega Muhim?                            |
   |-----------------------|------------------------------------------|-----------------------------------------|
| Initialize Qilish     | 	Collectionga boshlang'ich qiymat berish | 	NullPointerException dan himoya qiladi |
| Lazy Initialization   | 	Kerak bo'lgandagina yaratish	           | Memory va performance optimallashtirish |
| Collection Operations | 	Collection ustidagi amallar             | 	Ma'lumotlarni boshqarish imkoniyati    |


- ✅ Xavfsizroq qiladi (null xatolaridan himoya)
- ✅ Soddaroq qiladi (doimiy tekshirishlarsiz)
- ✅ Tozalroq qiladi (kod ko'rinishi yaxshilanadi)
- ✅ Ishonchliroq qiladi (har doim ishlaydi)

```java
// ✅ PROFESSIONAL USUL
private List<String> items = new ArrayList<>();
private Set<Integer> numbers = new HashSet<>();
private Map<String, Object> data = new HashMap<>();
```




























































