## 1. Spring boot loyihasini `PostgreSql` ma'lumotlar bazasi  bilan ulash uchun minimum sozlamar qanday?

---

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/spring
    username: postgres
    password: 1996
  jpa:
    hibernate:
      ddl-auto: update

```

1. `spring.datasource`

   Bu bo‘limda ma’lumotlar bazasiga ulanish parametrlarini ko‘rsatamiz.

   - `url`: jdbc:postgresql://localhost:5432/spring
     - `jdbc:postgresql://` → PostgreSQL drayveridan foydalanayotganingni bildiradi.
     - `localhost` → Baza shu kompyuteringda ishga tushgan (agar masofadagi server bo‘lsa IP yoki domen bo‘ladi).
     - `5432` → `PostgreSQL`-ning standart porti.
     - `spring` → Ulanayotgan bazang nomi (`PostgreSQL`-da bu ma’lumotlar bazasi nomi).
        > Demak: `Spring Boot` shu `URL` bo‘yicha bazaga ulanadi. Agar spring nomli `DB` yo‘q bo‘lsa yoki `port` bloklangan bo‘lsa, xatolik bo‘ladi.
       
   - `username: postgres`:
      
     **Bazaga kirish login** — ko‘pincha `postgres` — bu `PostgreSQL` o‘rnatilganda yaratiladigan `superuser` hisobidir.
   - `password: 1996`  
     - Bazaga kirish paroli.
     - **Eslatma**: ishlab chiqarish (`production`) muhitida parolni yashirish (masalan, `environment variables` orqali) tavsiya qilinadi.

2. `spring.jpa` bo‘limi

- `hibernate.ddl-auto: update`
  -  Bu parametr `Hibernate Entity` klasslari asosida **bazada** `table` yaratish yoki o‘zgartirish jarayonini boshqaradi.
  - **Asosiy qiymatlar**:
    - `none` → Bazaga hech narsa qilmaydi, faqat ulanishni tekshiradi.
    - `validate` → `Entity` va bazadagi `table` tuzilmasini solishtiradi, farq bo‘lsa xatolik beradi, lekin hech narsa o‘zgartirmaydi.
    - `update` → `Entity` klasslarga qarab `table` qo‘shadi yoki ustun qo‘shadi, lekin mavjud ustunlarni o‘chirib tashlamaydi.
    - `create` → Har safar ilova ishga tushganda tablelarni yangidan yaratadi (eskilarini o‘chiradi).
    - `create-drop` → Ilova yopilganda tablelarni o‘chiradi (testlar uchun).

> Bizda `update` qo‘yilgan — bu yaxshi, chunki `entity` qo‘shilganda `Hibernate` bazada yangi `table` yaratishi kerak.
> Agar table yaratilmayotgan bo‘lsa:
> - `Entity-laringda @Entity annotatsiyasi yo‘q yoki noto‘g‘ri paktda.`
> - `Spring Boot scanning` qilmayapti (asosiy klassdagi `@SpringBootApplication` paketdan yuqorida turishi kerak).

> Agar `entity` bo‘lmasa yoki `scanning` ishlamasa — `table` yaratilmaydi.


## 2. Bu ikkita anotatsiya nima uchun kerak:` @EnableJpaRepositories(basePackages = "uz.test.userservice")` va  `@EntityScan("uz.test.userservice.entity")`

---

1. `@EnableJpaRepositories(basePackages = "...")`

   - Belgilangan paket (yoki paketlar) ichida joylashgan **Spring Data JPA repository interfeyslarini** (masalan, `UserRepository extends JpaRepository<User, Long>)` topadi va ularni avtomatik ro‘yxatdan o‘tkazadi (`Spring Bean` sifatida).

   - Agar bu annotatsiyani yozmasang va `repositor` lar `@SpringBootApplication` klassi bilan bir paketda yoki uning ichki paketlarida bo‘lmasa, `Spring` ularni topa olmaydi, natijada` No qualifying bean of type ...` xatosi chiqishi mumkin. 

   `Misol`:

    ```java
    @EnableJpaRepositories(basePackages = "uz.test.userservice.repository")
    
    ```
   Bu orqali Spring `uz.test.userservice.repository` papkasidagi repository interfeyslarini izlaydi.

2. `@EntityScan("...")`

   - Belgilangan `paket` (yoki paketlar) ichidagi `@Entity` annotatsiyali klasslarni qidiradi va `Hibernate` ga ro‘yxatdan o‘tkazadi.
   - Agar `entity` klasslaring asosiy `@SpringBootApplication` **paketingda** bo‘lmasa, `Hibernate` ularni avtomatik topmaydi — shu sababli `@EntityScan` **orqali qaysi paketdan `entity` qidirishni ko‘rsatib qo‘yamiz.**

   `Misol`:
    ```java
    @EntityScan("uz.test.userservice.entity")
    ```
   Bu orqali Spring `uz.test.userservice.entity` paketidagi barcha `@Entity` klasslarni `Hibernate` ga skan qiladi va jadval sifatida ro‘yxatdan o‘tkazadi.

### Qisqa qilib:

- `@EnableJpaRepositories(...)` → `repository` larni qayerdan qidirish kerakligini ko‘rsatadi.
- `@EntityScan(...)` → `entity` larni qayerdan qidirish kerakligini ko‘rsatadi.

> Agar `entity` lar ham, `repository` lar ham `@SpringBootApplication` klassi bilan bitta paketda bo‘lsa — bu annotatsiyalarni yozishga hojat yo‘q.


1. Hech narsa yozmasang ham ishlaydigan holat
    ```java
    @SpringBootApplication
    public class UserServiceApplication {
        public static void main(String[] args) {
            SpringApplication.run(UserServiceApplication.class, args);
        }
    }
    ```
   Agar:
    - `@SpringBootApplication` classi `uz.test.userservice` paketida bo‘lsa,
    - `entity` lar `uz.test.userservice.entity` paketida bo‘lsa,
    - `repository` lar `uz.test.userservice.repository` paketida bo‘lsa,

   `Spring Boot` barchasini o‘zi topadi (chunki `@SpringBootApplication` ichida `@ComponentScan` bor va u o‘z paketini va ichki paketlarni skan qiladi). 
    > Annotatsiyalar kerak emas.

2. Annotatsiyalar kerak bo‘ladigan holat

Agar paketlar asosiy `application` klassidan boshqa joyda bo‘lsa:

`Misol`:

```java
@SpringBootApplication
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```
`Lekin`:

- `Entity` lar `uz.models.entity` paketida bo‘lsa,
- `Repository` lar uz.data.repo paketida bo‘lsa.

Spring Boot bularni avtomatik topolmaydi, chunki `default` skan qilish faqat `@SpringBootApplication` joylashgan paket va ichki paketlarda ishlaydi.

### Shunda yozish kerak:

```java
@EntityScan("uz.models.entity")
@EnableJpaRepositories(basePackages = "uz.data.repo")
```
Xulosa:

- **Bitta paket ichida** bo‘lsa — annotatsiyalar shart emas.
- **Turli paket/modullarda** bo‘lsa — `@EntityScan` va `@EnableJpaRepositories` bilan yo‘l ko‘rsatib berish kerak.


## 3. `Postgresql` bilan bog'lanish uchun `spring boot` ga qanday kutubxonalr zarur.

---

```yaml
<dependencies>
    <!-- PostgreSQL driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>

    <!-- Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Spring Web (REST API uchun) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Validation (optional) -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
</dependencies>
```

## 4.  `spring-boot-starter-data-jpa` nima

`spring-boot-starter-data-jpa` - bu `Spring Boot` ning ma'lumotlar bazasi bilan ishlash uchun tayyor paketi. U bir nechta kutubxonalarni birlashtirib, `JPA (Java Persistence API)` bilan ishlashni juda osonlashtiradi.

### Bu starter paketi quyidagi asosiy kutubxonalarni o'z ichiga oladi:

1. `Spring Data JPA` - Asosiy kutubxona

    ```yaml
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-jpa</artifactId>
    </dependency>
    ```
2. `Hibernate` - JPA Implementation

    ```yaml
    <dependency>
        <groupId>org.hibernate.orm</groupId>
        <artifactId>hibernate-core</artifactId>
    </dependency>
    ```
3. `Spring ORM` - Object-Relational Mapping  

    ```yaml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-orm</artifactId>
    </dependency>
    ```
4. `Spring Transaction` - Transaction Management

    ```yaml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
    </dependency>
    ```
5. `JDBC` - Java Database Connectivity

    ```yaml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
    </dependency>
    ```
### `spring-boot-starter-data-jpa` nima qiladi?

1. Avtomatik Konfiguratsiya
    ```java
       // Sizning o'rningizda Spring Boot shularni qiladi:
       @Configuration
       @EnableJpaRepositories  // ✅ Avtomatik qo'shiladi
       @EntityScan            // ✅ Avtomatik qo'shiladi
       @EnableTransactionManagement // ✅ Avtomatik qo'shiladi
       public class AutoJpaConfig {
           // Hech narsa yozish shart emas!
       }
    ```

2. Repository Magic

    ```java
    // Faqat interfeys yozing, implementation Spring tomonidan yaratiladi
    public interface UserRepository extends JpaRepository<User, Long> {
        // Avtomatik methodlar
        List<User> findByName(String name);
        List<User> findByEmailContaining(String keyword);
    }
   ```

3. Entity Management

    ```java
    // Entity yaratish juda oson
    @Entity
    public class User {
        @Id
        @GeneratedValue
        private Long id;
        private String name;
        // ...
    }
    ```









































