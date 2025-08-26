## 1. `Spring boot annotation` lar nima uchun kerak?

---

`Spring Boot` annotatsiyalari dastur tuzishni osonlashtirish va tezlashtirish uchun mo'ljallangan. Ular quyidagi maqsadlarni bajaradi:

1. **Konfiguratsiyani soddalashtirish:** Annotatsiyalar orqali konfiguratsiya fayllarini kamaytirish mumkin. Masalan, `@SpringBootApplication` annotatsiyasi dasturga kerakli konfiguratsiyalarni avtomatik ravishda qo'shadi.
2. **Qisqa kod:** Annotatsiyalar yordamida ko'p kod yozmasdan, kerakli funksiyalarni qo'shish mumkin. Masalan, `@RestController` va `@RequestMapping` annotatsiyalari yordamida `RESTful` xizmatlarni tezda yaratish mumkin.
3. **Avtomatik bog'lanish (`Dependency Injection`):** `@Autowired` annotatsiyasi yordamida kerakli komponentlarni avtomatik ravishda bog'lash mumkin, bu esa kodni toza va tushunarli qiladi.
4. **Ma'lumotlar bazasiga bog'lanish:** `@Entity`, `@Table`, va `@Repository` annotatsiyalari yordamida ma'lumotlar bazasi bilan ishlashni soddalashtiradi.
5. **Xatoliklarni boshqarish:** `@ControllerAdvice` va `@ExceptionHandler` annotatsiyalari yordamida xatoliklarni global va joylarda boshqarish imkonini beradi.

> Umuman olganda, `Spring Boot` annotatsiyalari dasturchilarga tez, samarali va oson loyihalarni yaratishga yordam beradi.

## 2. `Spring Boot` da qanday **annotatsiyalar** (`annotation`) lar bor?

---

1. `@SpringBootApplication`: Dastur uchun asosiy annotatsiya. U avtomatik konfiguratsiya, komponentlarni skanerlash va `Spring` konteynerini ishga tushirishni ta'minlaydi.
2. `@RestController`: `RESTful` web xizmatlarini yaratish uchun ishlatiladi. Bu annotatsiya `@Controller` va `@ResponseBody` ni birlashtiradi.
3. `@RequestMapping`: `HTTP` so'rovlarini ma'lum metodlarga yo'naltirish uchun ishlatiladi. `URL` yo'llarini va `HTTP` metodlarini belgilash imkonini beradi.
4. `@Autowired`: Bog'lanishni avtomatik ravishda amalga oshiradi. Bu annotatsiya yordamida kerakli bean'lar avtomatik ravishda inject qilinadi.
5. `@Service`: Biznes mantiqini o'z ichiga oladigan xizmatlar uchun annotatsiya. Bu annotatsiya yordamida servis qatlamini belgilash mumkin.
6. `@Repository`: Ma'lumotlar bazasi bilan ishlovchi komponentlar uchun annotatsiya. U Spring Data bilan birga ishlashda foydalidir.
7. `@Entity`: Ma'lumotlar bazasidagi jadvalga mos keluvchi sinf uchun annotatsiya. U `JPA` (Java Persistence API) bilan ishlashda qo'llaniladi.
8. `@Table`: `@Entity` annotatsiyasi bilan birga ishlatiladi va ma'lumotlar bazasidagi jadval nomini belgilash uchun ishlatiladi.
9. `@ControllerAdvice`: Global xatoliklarni boshqarish va umumiy xatoliklarni qayta ishlash uchun ishlatiladi.
10. `@Value`: Tashqi konfiguratsiya fayllaridan **(masalan, application.properties)** qiymatlarni olish uchun ishlatiladi.

> `RESTful` — bu **veb-servislar** yaratish uchun ishlatiladigan arxitektura uslubi bo'lib, "Representational State Transfer" (REST) prinsiplariga asoslanadi. `RESTful` xizmatlar quyidagi asosiy xususiyatlarga ega:

### Asosiy Xususiyatlar:

1. **Resurslar**: `RESTful` xizmatlar resurslarga asoslangan. Har bir resurs (masalan, foydalanuvchilar, mahsulotlar) `URL` orqali aniqlanadi.
2. `HTTP` Metodlari: `RESTful API` lar asosan `HTTP` metodlaridan foydalanadi:
   - `GET`: Resursni olish.
   - `POST`: Yangi resurs yaratish.
   - `PUT`: Mavjud resursni yangilash.
   - `DELETE`: Resursni o'chirish.
3. Ko'p formatlar: `RESTful API` lar ma'lumotlarni turli formatlarda (masalan, `JSON`, `XML`) uzatishi mumkin, ammo JSON eng keng tarqalgan formatdir.


## 3.`@RestController` `annotation` nima vazifani bajaradi

---

- `@RestController` yordamida siz RESTful web xizmatlarini osonlik bilan yaratishingiz mumkin. Bu annotatsiya HTTP so'rovlarini qabul qilish va ularga javob berish uchun zarur bo'lgan metodlarni belgilaydi.
- `@RestController` avtomatik ravishda metodlardan qaytgan ma'lumotlarni `JSON` yoki `XML` formatiga aylantiradi. Bu, asosan, `@ResponseBody` funksionalligini ta'minlaydi.

> Agar `@RestController` o'rnida boshqa annotatsiyalarni ishlatmoqchi bo'lsangiz, quyidagi variantlar mavjud:

### `@Controller` va `@ResponseBody`

`@RestController` o'rniga `@Controller` va `@ResponseBody` ni alohida ishlatishingiz mumkin. Bu holatda siz har bir metodda `@ResponseBody` annotatsiyasini qo'shishingiz kerak.

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MyController {

    @GetMapping("/hello")
    @ResponseBody
    public String sayHello() {
        return "Salom, Dunyo!";
    }
}
```
- Agar siz `RESTful` bo'lmagan, lekin ma'lumotlarni `JSON` yoki `XML` formatida qaytarishingiz kerak bo'lsa, @ResponseBody annotatsiyasini alohida ishlatishingiz mumkin.

`RESTful` bo'lmagan xizmatga oddiy bir misol sifatida quyidagi holatni keltirish mumkin:

`Misol`:

### `RESTful` xizmat:


- `GET /users` - Barcha foydalanuvchilar ro'yxatini olish.
- `POST /users` - Yangi foydalanuvchini qo'shish.
- `GET /users/{id}` - Ma'lum bir foydalanuvchi ma'lumotlarini olish.


### `ESTful` bo'lmagan xizmat:

- `GET /getUserData` - Foydalanuvchi ma'lumotlarini olish, lekin foydalanuvchi `ID` si so'rovning ichida **(masalan, `query` string yoki `body`)** beriladi va serverda foydalanuvchi holatini saqlaydi.

> Bu yerdagi asosiy farq shundaki, `ESTful` bo'lmagan xizmatda resursga murojaat qilish uchun `URI` orqali emas, balki boshqa usullar **(masalan, argumentlar orqali)** ishlatiladi va server holatni saqlaydi. Bu esa `REST` tamoyillariga zid keladi.


- `@Controller `annotatsiyasi sinfni web-qabul qiluvchi sifatida belgilaydi. Bu sinf `HTTP` so'rovlarini qabul qilish va ularga javob berish uchun ishlatiladi.
  - Ushbu annotatsiya yordamida Spring, URL so'rovlarini mos metodlarga yo'naltiradi. Bu, URL va metodlar o'rtasidagi bog'lanishni osonlashtiradi.

- `@ResponseBody` annotatsiyasi metoddan qaytgan qiymatni to'g'ridan-to'g'ri `HTTP` javobi sifatida qaytaradi. Bu, natijani avtomatik ravishda `JSON`, `XML` yoki boshqa formatlarda yuborishga imkon beradi.
    ```java
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.ResponseBody;
    import org.springframework.web.bind.annotation.RestController;
    
    
    @RestController
    public class UserController {
    
        @GetMapping("/users")
        @ResponseBody
        public List<User> getUsers() {
            // Foydalanuvchilar ro'yxatini qaytarish
            return userService.getAllUsers(); // JSON formatida qaytadi
        }
    }
    ```
### `HTTP` so'rovlarini boshqarish:

`@RestController` yordamida siz turli xil `HTTP` metodlari (`GET`, `POST`, `PUT`, `DELETE`) uchun mos keluvchi metodlarni yaratishingiz mumkin. Misollar:

- `@GetMapping`: GET so'rovlarini boshqaradi.
- `@PostMapping`: POST so'rovlarini boshqaradi.
- `@PutMapping`: PUT so'rovlarini boshqaradi.
- `@DeleteMapping`: DELETE so'rovlarini boshqaradi.


## 4. `@ResponseBody` `annotation` ni tushuntiring:

---

`@ResponseBody` annotatsiyasi metoddan qaytgan qiymatni to'g'ridan-to'g'ri `HTTP` javobi sifatida qaytaradi. Bu, natijani avtomatik ravishda `JSON`, `XML` yoki boshqa formatlarda yuborishga imkon beradi.

## 5. `@RequestBody` `annotation` ni tushuntiring

---

`@RequestBody` annotatsiyasi Spring framework da `HTTP` so'rovining tanasidan (`body`) ma'lumotlarni qabul qilish uchun ishlatiladi. Bu annotatsiya yordamida siz `JSON`, `XML` yoki boshqa formatlarda uzatilgan ma'`lumotlarni` Java ob'ektlariga avtomatik ravishda aylantirishingiz mumkin.

### Ma'lumotlarni qabul qilish:
   - `@RequestBody` annotatsiyasi yordamida foydalanuvchi so'rovining tanasida yuborilgan ma'lumotlarni o'qishingiz mumkin.
   - `Spring` avtomatik ravishda `JSON` yoki `XML` formatidagi ma'lumotlarni belgilangan `Java` ob'ektlariga aylantiradi. Bu, masalan, `Jackson` yoki `Gson` kutubxonalaridan foydalanish orqali amalga oshiriladi.
   - Odatda `@RequestBody` `POST` yoki `PUT` so'rovlarida ishlatiladi, chunki bu metodlar ko'pincha ma'lumotlarni serverga yuborish uchun mo'ljallangan. 

`Misol`:

Quyida `@RequestBody` annotatsiyasidan foydalanish misoli keltirilgan:

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class UserController {

    @PostMapping("/users")
    public String createUser(@RequestBody User user) {
        // User ob'ektini olish
        return "Foydalanuvchi " + user.getName() + " yaratildi.";
    }

    public static class User {
        private String name;
        private String email;

        // Getter va Setter metodlari
        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public String getEmail() {
            return email;
        }

        public void setEmail(String email) {
            this.email = email;
        }
    }
}
```

`Tavsif`:
- `@PostMapping`: `/users` `URL` manziliga `POST` so'rovini qabul qiladi.
- `@RequestBody`: Foydalanuvchi tomonidan yuborilgan ma'lumotlarni `User` ob'ektiga aylantiradi.

## 6. `@PathVariable` annotation nima uchun kerak?

---

`@PathVariable` annotatsiyasi `Spring framework` da `URL` yo'llaridan dinamik ma'lumotlarni olish uchun ishlatiladi. Bu annotatsiya yordamida `URL` ichida joylashgan o'zgaruvchilarni method argumentlari sifatida qabul qilish imkonini beradi.

### Dinamik `URL` parametrlarini olish:
   - `URL` ichida belgilangan o'zgaruvchilarni olish uchun ishlatiladi. Masalan, foydalanuvchi `ID` sini yoki boshqa parametrlarni olishda foydalidir.
   - `RESTful API` lar yaratishda juda qulaydir, chunki ko'pincha resurslar `URL` orqali ko'rsatiladi.

`Misol`:

Quyidagi misolda `@PathVariable` annotatsiyasidan foydalanish ko'rsatilgan:

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class UserController {

    @GetMapping("/users/{id}")
    public String getUserById(@PathVariable String id) {
        // ID orqali foydalanuvchini olish
        return "Foydalanuvchi ID: " + id;
    }
}
```

`Tavsif`:
- `@GetMapping`: `/users/{id}` `URL` manziliga `GET` so'rovini qabul qiladi.
- `{id}`: URL ichidagi dinamik parametr.
- `@PathVariable`: `{id}` parametrini id metod argumentiga o'zgartiradi.

`Qanday ishlaydi`:

1. Foydalanuvchi `GET` so'rovini yuboradi, masalan, `/api/users/1`.
2. Spring `@PathVariable` yordamida `1` raqamini `id` argumentiga joylaydi.
3. Metod ichida `id` argumentidan foydalanib, kerakli operatsiyalarni amalga oshirish mumkin.

> `@PathVariable` annotatsiyasi `URL` yo'llaridan dinamik ma'lumotlarni olish uchun qulay va samarali usuldir, ayniqsa `RESTful API` lar yaratishda.

## 7. `@PathVariable`, `@RequestBody` va `@RequestParam` `annotation` larni bir biridan qanday farqi bor?

> `@PathVariable`, `@RequestBody`, va `@PathParam` annotatsiyalari `Java Spring` va `Java EE (JAX-RS)` frameworklarida ishlatiladi. Ularning har biri `HTTP` so'rovlarida ma'lumotlarni olish uchun ishlatiladi, lekin ularning maqsadi va ishlatilishi farq qiladi.

1. `@PathVariable`
   - **Maqsad:** `URL` yo'lidagi o'zgaruvchilarni olish.
   - **Qanday ishlatiladi:** `URL` ichida o'zgaruvchilarni belgilash va ularni metod parametrlariga uzatish uchun.
   - `Misol`:
       ```java
        @GetMapping("/users/{id}")
        public User getUser(@PathVariable String id) {
        // id parametrini olish
        }
       ```

2. `@RequestBody`
   - **Maqsad:** `HTTP` so'rovining tanasidan `(body)` ma'lumotlarni olish.
   - **Qanday ishlatiladi:** `JSON` yoki `XML` formatida yuborilgan ma'lumotlarni `Java` ob'ektiga aylantirish uchun.
   - `Misol`:
      ```java
       @PostMapping("/users")
       public User createUser(@RequestBody User user) {
        // user ob'ektini olish
        }
      ```
3. `@RequestParam`
   - **Maqsad:** `URL` `query` parametrlarini olish.
   - **Qanday ishlatiladi:** `URL` da so'rov parametrlarini olish uchun, masalan, `?key=value` formatida.
   - `Misol`:
      ```java
      @GetMapping("/users")
      public List<User> getUsers(@RequestParam String role) {
           // role parametrini olish
      }
      ```
   ### Ma'lumot manbai:
    - `@PathVariable`: `URL` yo'lidan o'zgaruvchilarni oladi  **(masalan, `/users/123`)**.
    - `@RequestBody`: `HTTP` so'rovining tanasidan ma'lumotlarni oladi **(masalan, `/users?role=admin`)**.
    - `@RequestParam`: `URL` `query` parametrlaridan ma'lumotlarni oladi.


## 8. `@Entity` `annotation` nima uchun kerak?

`@Entity` annotatsiyasi `Java Persistence API` (`JPA`) va `Hibernate` kabi `ORM` (`Object-Relational Mapping`) frameworklarida ishlatiladi. U asosan quyidagi maqsadlar uchun kerak:
1.  Ma'lumotlar Bazasi Jadvaliga Mos Kelish
   - `@Entity` annotatsiyasi yordamida `Java` sinfini ma'lumotlar bazasidagi jadvalga mos keladigan ob'ekt sifatida belgilaysiz. Har bir @Entity ob'ekti jadvalda bir qatorni ifodalaydi.
2. Ob'ektlar va Jadval O'rtasidagi Moslashuv
   - Bu annotatsiya yordamida siz `Java` ob'ektlari va ma'lumotlar bazasidagi jadval o'rtasidagi moslashuvni ta'minlaysiz. Bu orqali siz `Java` kodida ma'lumotlarni oson boshqarishingiz mumkin.

3. `CRUD` Operatsiyalarini Osonlashtirish
- `@Entity` annotatsiyasi bilan belgilangan sinflar `JPA` yordamida `CRUD` (`Create`, `Get`, `Update`, `Delete`) operatsiyalarini bajarish uchun ishlatiladi. Bu, ma'lumotlar bilan ishlash jarayonini soddalashtiradi.

4. **Annotatsiyalar** Bilan qo'shimcha konfiguratsiyalar

    Siz `@Entity` bilan birga boshqa annotatsiyalarni ham qo'llashingiz mumkin, masalan:
    - `@Table`: Jadval nomini belgilash uchun.
    - `@Id`: Birlamchi kalit maydonini belgilash uchun.
    - `@Column`: Jadvaldagi ustunlarni moslashtirish uchun.
   
    `Misol`:
    ```java
    import javax.persistence.Entity;
    import javax.persistence.Id;
    import javax.persistence.Table;
    
    @Entity
    @Table(name = "users") // Jadval nomi "users"
    public class User {
        @Id
        private Long id; // Birlamchi kalit
    
        private String name; // Foydalanuvchi ismi
    
        // Getters va Setters
    }
    ```

`Xulosa`:

`@Entity` annotatsiyasi ma'lumotlar bazasi bilan ishlashni soddalashtirish va `Java` ob'ektlarini ma'lumotlar bazasidagi jadvalga moslashtirish uchun zarur. U `JPA` va `ORM` frameworklarida muhim rol o'ynaydi.


## 9. @GeneratedValue(strategy = GenerationType.IDENTITY) annotation nima uchun kerak.

`@GeneratedValue(strategy = GenerationType.IDENTITY)` annotatsiyasi `JPA` (`Java Persistence API`) va `Hibernate` kabi `ORM` frameworklarida ishlatiladi. U ma'lumotlar bazasida birlamchi kalit (`ID`) maydonining qiymatini avtomatik ravishda yaratish uchun kerak. Keling, uning asosiy jihatlarini ko'rib chiqaylik:


1. Avtomatik `ID` Yaratish
    - `@GeneratedValue` annotatsiyasi, `ID` maydonining qiymatining avtomatik ravishda generatsiya qilinishini ta'minlaydi. Bu, foydalanuvchi yoki dasturchi tomonidan qo'lda `ID` qiymatini belgilashni talab qilmaydi.

2. `GenerationType.IDENTITY` nima qiladi?
   - `GenerationType.IDENTITY` — bu `primary key` qiymatini ma'lumotlar bazasi orqali avtomatik generatsiya qilishni bildiradi.
     - Bu strategiyada `auto-increment` qatori ma'lumotlar bazasida ishlatiladi.
     - `Primary key` qiymati `JPA` tomonidan emas, balki ma'lumotlar bazasi tomonidan belgilaydi.
     - Ma'lumotlar bazasi keyin `primary key` qiymatini generatsiya qiladi va `JPA` bu qiymatni qaytarib oladi. `JPA` bu qiymatni bilib olish uchun ma'lumotlar bazasi bilan aloqada bo‘ladi.
     - `JPA primary key` qiymatini boshqarishni ma'lumotlar bazasiga topshiradi.









































































































































































































































































































































































