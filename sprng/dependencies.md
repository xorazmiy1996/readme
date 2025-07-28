## 0. **Spring boot modullari** va **Spring cloud modullari** nima degani, ularning farqi

---

- `Spring Boot Modullari:`

  > `Spring Boot modullari` - bu yakka (`standalone`) ilovalarni tez ishlab chiqish uchun mo'ljallangan tayyor
  komponentlar.

  `Asosiy xususiyatlar`:

    - **Starter paketlar** - kerakli dependencylarni avtomatik sozlaydi

        - **Embedded server** (`Tomcat`, `Jetty`, `Undertow`)

        - **Avtomatik konfiguratsiya** - minimal sozlash bilan ishlaydi

        - **Actuator** - ilova monitoringi

        - Test qo'llab-quvvatlash

  **Mashhur Spring Boot starterlar**:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId> <!-- Web ilovalar uchun -->
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId> <!-- Ma'lumotlar bazasi -->
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId> <!-- Testlash -->
    </dependency>
    ```

- **Spring Cloud Modullari:**

  > **Spring Cloud modullari** - bu mikroservis arxitekturasida ishlash uchun maxsus yechimlar.

  Asosiy xususiyatlar:

  Mikroservislararo muloqot

    - Service discovery (servislarni topish)

    - Markaziy konfiguratsiya

    - API Gateway

    - Circuit breakers (xatolarga bardosh)

    ```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId> <!-- Service discovery -->
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-client</artifactId> <!-- Markaziy konfiguratsiya -->
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId> <!-- API Gateway -->
    </dependency>
    ```
- **Asosiy Farqlar**

  | Xususiyat      | Spring Boot                 | Spring Cloud                             |
                      |----------------|-----------------------------|------------------------------------------|
  | Maqsad         | Yakka ilovalar              | Mikroservis tizimlarMikroservis tizimlar |
  | Arxitektura    | Monolit                     | Distribut tizim                          |
  | Asosiy vazifa  | Ilovani tez ishga tushirish | Servislararo muloqotni osonlashtirish    |
  | Konfiguratsiya | application.properties/yml  | Markaziy config server                   |
  | Servis topish  | Yo'q                        | Eureka, Consul, Zookeeper                |
  | HTTP muloqot   | RestController              | FeignClient, RestTemplate                |
  | Xavfsizlik     | Basic Auth, JWT             | OAuth2, JWT, Keycloak integratsiya       |

### Qanday birga ishlaydi?

`Spring Cloud` har doim `Spring Boot` ustida qurilgan:

- Avval `Spring Boot` ilovasi yaratasiz
- Keyin unga `Spring Cloud` modullarini qo'shasiz

## 0. Spring boot komponentlari va Spring cloud komponetlarin haqida

---

`Spring Boot` va `Spring Cloud` Komponentlari: to'liq taqqoslash

Quyida `Spring Boot` (`monolit` ilovalar uchun) va `Spring Cloud` (**mikroservis** arxitekturasidagi ilovalar uchun)
komponentlarining turlari va ularning farqlari keltirilgan.

1. `Spring Boot` komponentlari(Monolit/Microservice Asoslari)

   `Spring Boot` — yakka (`standalone`) ilovalarni tez ishlab chiqish uchun ishlatiladi.
   Asosiy komponentlar:

   | Komponent                    | 	Vazifasi                                           | 	Misol Dependency                                |
                            |------------------------------|-----------------------------------------------------|--------------------------------------------------|
   | spring-boot-starter-web      | 	REST API, MVC ilovalar uchun (Tomcat server bilan) | 	Web, Spring MVC, Tomcat                         |
   | spring-boot-starter-data-jpa | 	Ma'lumotlar bazasi bilan ishlash (Hibernate, JPA)  | 	Hibernate, JPA, Spring Data                     |
   | spring-boot-starter-security | 	Autentifikatsiya va avtorizatsiya (JWT, OAuth2)    | 	Spring Security, JWT                            |
   | spring-boot-starter-test     | 	Testlash (Unit, Integration testlar)               | 	JUnit, Mockito, Testcontainers                  |
   | spring-boot-starter-actuator | 	Ilova monitoringi (Health, Metrics, Logs)          | 	Prometheus, Health Checks                       |
   | spring-boot-starter-cache    | 	Caching (Redis, Ehcache, Caffeine)                 | 	Redis, Caffeine                                 |
   | spring-boot-starter-batch    | 	Batch (partiyaviy)                                 | jarayonlarni boshqarish	Job Scheduling, Tasklets |

2. `Spring Cloud` komponentlari (Mikroservis aloqalari)

   > Spring Cloud — mikroservislar orasidagi muloqot, konfiguratsiya va monitoring uchun ishlatiladi.

   Asosiy komponentlar:

   | Komponent                           | 	Vazifasi                                                   | 	Misol Dependency                    |
                         |-------------------------------------|-------------------------------------------------------------|--------------------------------------|
   | spring-cloud-starter-netflix-eureka | 	Service Discovery (servislarni avtomatik topish)           | 	Eureka Server, Eureka Client        |
   | spring-cloud-config-server          | 	Markaziy konfiguratsiya boshqaruvi (Git, Vault bilan)      | 	Config Server, Git Backend          |
   | spring-cloud-starter-gateway        | 	API Gateway (Routing, Load Balancing, Auth)                | 	Spring Cloud Gateway, Rate Limiting |
   | spring-cloud-starter-openfeign      | 	Mikroservislararo HTTP muloqot (Declarative REST Client)   | 	Feign Client, Load Balancing        |
   | spring-cloud-starter-circuitbreaker | 	Xatolarga chidamlilik (Circuit Breaker pattern)            | 	Resilience4j, Hystrix               |
   | spring-cloud-starter-sleuth         | 	Distributed Tracing (Zipkin, Jaeger bilan)                 | 	Sleuth, Zipkin Integration          |
   | spring-cloud-starter-bus            | 	Konfiguratsiya o'zgarishlarini tarqatish (RabbitMQ, Kafka) | 	Spring Cloud Bus, AMQP/Kafka        |


3. `Spring Boot` vs `Spring Cloud`: Qachon qaysi komponent kerak?

   | Vazifa                    | 	Spring Boot Komponenti      | 	Spring Cloud Komponenti             |
   |---------------------------|------------------------------|--------------------------------------|
   | REST API yaratish         | 	spring-boot-starter-web     | 	—                                   |
   | Ma'lumotlar bazasi        | 	spring-boot-starter-data-jpa | 	—                                   |
   | Security (JWT/OAuth2)     | 	spring-boot-starter-security | 	—                                   |
   | Monitoring	               | spring-boot-starter-actuator | —                                   |
   | Servislarni topish        | 	—                           | 	spring-cloud-starter-netflix-eureka |
   | API Gateway               | 	—	                          | spring-cloud-starter-gateway         |
   | Mikroservislararo muloqot | 	—                           | 	spring-cloud-starter-openfeign      |
   | Distributed Tracing       | 	—                           | 	spring-cloud-starter-sleuth         |

4. Qanday Birgalikda Ishlatiladi?

   ### Mikroservis loyihasida ikkalasi ham ishlatiladi:
   1. `Spring Boot` — har bir mikroservisning asosiy frameworki
   2. `Spring Cloud` — mikroservislar orasidagi aloqalarni boshqarish

   `Misol pom.xml (Spring Boot + Cloud)`:

    ```xml
    <!-- 1. Spring Boot Parent -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
    </parent>
    
    <!-- 2. Spring Cloud Dependencies -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>2023.0.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <!-- 3. Dependencylar -->
    <dependencies>
        <!-- Spring Boot -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <!-- Spring Cloud -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
    </dependencies>
    ```
5. Xulosa

    - `Spring Boot` = Yakka ilovalar uchun (`REST API`, `DB`, `Security`)
    - `Spring Cloud` = Mikroservislararo muloqot (`Discovery`, `Config`, `Gateway`)
    - Agar **mikroservis** yaratsangiz, ikkalasini ham ishlatishingiz kerak!
    > "`Spring Boot` — ilova iskeleti, `Spring Cloud` — mikroservislar orasidagi aloqa vositasi" deb eslang.




## 0. `Spring boot` va `Spring Cloud` dependenies larini qanday farqlayman

---

`Spring Boot` va `Spring Cloud` dependencylarini farqlash uchun quyidagi oddiy qoidalarga amal qiling:

- `GroupId` orqali farqlash
  - `Spring Boot` dependencylari har doim:
     ```xml
     <groupId>org.springframework.boot</groupId>
     ```
    
- `ArtifactId` nomlari orqali farqlash
  - `Spring Boot` starterlari:
     ```xml
     spring-boot-starter-*
     ```
    Misollar:
    
      ```xml
      <artifactId>spring-boot-starter-web</artifactId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
      ```

    - `Spring Cloud` starterlari:
       ```xml
       spring-cloud-starter-*
       ```
       Misollar:
       ```xml
       <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       <artifactId>spring-cloud-starter-config</artifactId>
       ```



- Vazifasi orqali farqlash
   - `Spring Boot` dependencylari:   
     - Yakka ilovalar uchun asosiy funksionallik
     - `Web`, `Data`, `Security` kabi asosiy modullar

   - `Spring Cloud` dependencylari:
     - Mikroservislararo aloqa
     - `Cloud-native` funksionallik



## 0. `spring-boot-dependencies` va `spring-cloud-dependencies` haqida

---

> `spring-boot-dependencies` — bu `Spring Boot` loyihalari uchun dependency versiyalarini boshqaruvchi `POM` fayli.
> `spring-cloud-dependencies` — bu Spring Cloud modullari uchun versiyalarni boshqaruvchi `POM` fayli

Mikroservis loyihalarida ikkalasi ham birga ishlatiladi:


```xml
<!-- 1. Spring Boot versiyalari -->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>3.2.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        
        <!-- 2. Spring Cloud versiyalari -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>2023.0.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```


## 1. Asosiy spring boot kutubxonalari:

---

- `spring-boot-starter-web`
    - `Spring MVC` (`REST API` yaratish), `Tomcat` (embedded server), `JSON` ma'lumotlarini qayta ishlash (`Jackson`)
      kabi imkoniyatlarni taqdim etadi.
    - **Vazifasi:** Web ilovalarini ishlab chiqish uchun asosiy kutubxona.

- `spring-boot-starter-actuator`
    - Ilova monitoringi, `health-check`, metrics yig‘ish (`/actuator` endpointlari) uchun.
    - **Vazifasi:** Mikroservis monitoringi va boshqaruv imkoniyatlari.
- `spring-boot-starter-test`
    - `JUnit`, `Mockito`, `Spring Test` kabi testlash vositalarini o‘z ichiga oladi.
    - **Vazifasi:** `Unit` va `integration` testlar yozish.

## 2. `Spring Cloud / Microservices` kutubxonalari:

---

- `spring-cloud-starter-netflix-eureka-client`:
    - Mikroservislarni **Eureka Server**ga ro‘yxatdan o‘tkazish va boshqa servislarni topish (service discovery) uchun.
    - **Vazifasi:** Servislar orasida avtomatik kommunikatsiya.


- `spring-cloud-dependencies` (BOM)
    - **<dependencyManagement>** da ishlatilgan. `Spring Cloud` komponentlarining mos versiyalarini boshqaradi.
    - **Vazifasi:** `spring-cloud-*` kutubxonalari versiyalarini avtomatik sinxronlash.

## 3. Dokumentatsiya va API boshqaruvi

--- 

- `springdoc-openapi-starter-webmvc-ui`

    - `Swagger UI` va `OpenAPI 3` asosida avtomatik `API` dokumentatsiya yaratadi.
    - **Vazifasi:** http://localhost:8080/swagger-ui.html manzilida interfeysni taqdim etadi.

## 4. Yordamchi kutubxonalar

---

- `lombok`
    - `Getter/Setter`, `@Data`, `@Builder` kabi annotatsiyalar orqali boilerplate kodni kamaytiradi.
    - **Vazifasi:** `Entity` va `DTO` klasslarini soddalashtirish.
    - **Eslatma:** `<optional>true</optional>` – boshqa modullarga avtomatik tarqalmasligi uchun.

- `commons-lang3`
    - `StringUtils`, `RandomStringUtils` kabi yordamchi klasslar.
    - **Vazifasi:** Oddiy amallarni tezlashtirish (masalan, `null-safe` string operations).

## 5. Build va pluginlar

---

- `maven-compiler-plugin`
    - `Java 21` versiyasida kompilyatsiya qilish uchun sozlanmagan.
    - `Lombok` uchun annotation processor qo‘shilgan (obyektlarni `@Data` bilan avtomatik yaratish).
- `spring-boot-maven-plugin`
    - Ilovani `executable JAR` qilib yig‘ish va `java -jar` bilan ishga tushirish uchun.
    - `Lombok`-ni `JAR` ichiga kiritmaslik uchun `<exclude>` qilingan.

### Qo‘shimcha tushuntirishlar

- Agar bu modul boshqa modullarga bog‘liq bo‘lsa, ular **<dependencyManagement>** da ko‘rsatilardi.

### Diagramma: Kutubxonalar o‘rni

```text
Spring Boot (Web, Actuator, Test)
│
├── Spring Cloud (Eureka Client)
│
├── SpringDoc (OpenAPI/Swagger)
│
├── Lombok (Code Generation)
│
└── Apache Commons Lang3 (Utilities)
```

## 6. `commons-lang3` Kutubxonasidan foydalanishga namunalar. `StringUtils` va `RandomStringUtils` haqida:

1. `String` (Matn) bilan ishlash

    ```java
    import org.apache.commons.lang3.StringUtils;
    
    public class StringExamples {
        public static void main(String[] args) {
            // 1. Bo'sh yoki null matnni tekshirish
            String text = null;
            System.out.println(StringUtils.isEmpty(text));       // true
            System.out.println(StringUtils.isBlank("   "));      // true
            
            // 2. Matnni bosh harf bilan yozish
            System.out.println(StringUtils.capitalize("hello")); // "Hello"
            
            // 3. Matnlarni qo'shish
            System.out.println(StringUtils.join("A", "B", "C")); // "A B C"
            
            // 4. Matnni takrorlash
            System.out.println(StringUtils.repeat("Ab", 3));     // "AbAbAb"
        }
    }
    ```
2. **Obyekt** lar bilan ishlash

    ```java
    import org.apache.commons.lang3.RandomStringUtils;
    
    public class RandomExamples {
        public static void main(String[] args) {
            // 1. Tasodifiy raqamlar
            System.out.println(RandomStringUtils.randomNumeric(6)); // "529874"
            
            // 2. Tasodifiy harflar
            System.out.println(RandomStringUtils.randomAlphabetic(5)); // "KdFjR"
            
            // 3. Harf va raqam aralash
            System.out.println(RandomStringUtils.randomAlphanumeric(8)); // "fG3h9T2m"
        }
    }
    ```
3. Massivlar bilan ishlash

    ```java
    import org.apache.commons.lang3.ArrayUtils;
    
    public class ArrayExamples {
        public static void main(String[] args) {
            String[] colors = {"Red", "Green", "Blue"};
            
            // 1. Element borligini tekshirish
            System.out.println(ArrayUtils.contains(colors, "Green")); // true
            
            // 2. Yangi element qo'shish
            colors = ArrayUtils.add(colors, "Yellow");
            
            // 3. Massivni chop etish
            System.out.println(ArrayUtils.toString(colors)); 
            // "{Red,Green,Blue,Yellow}"
        }
    }
    ```

4. Vaqtni hisoblash

    ```java
    import org.apache.commons.lang3.time.StopWatch;
    
    public class TimeExamples {
        public static void main(String[] args) throws InterruptedException {
            StopWatch stopWatch = StopWatch.createStarted();
            
            // Biror amal bajaramiz
            Thread.sleep(1250); // 1.25 soniya
            
            stopWatch.stop();
            System.out.println("Sarflangan vaqt: " + stopWatch.getTime() + " ms"); 
            // "Sarflangan vaqt: 1250 ms"
        }
    }
    ```

5. `Spring Boot Controller` ida qo'llash

    ```java
    import org.apache.commons.lang3.StringUtils;
    import org.springframework.web.bind.annotation.*;
    
    @RestController
    @RequestMapping("/api/utils")
    public class UtilsController {
    
        @GetMapping("/reverse")
        public String reverseString(@RequestParam(required = false) String input) {
            if (StringUtils.isBlank(input)) {
                return "Iltimos, matn kiriting!";
            }
            return StringUtils.reverse(input);
        }
        
        @GetMapping("/random")
        public String generateRandom(@RequestParam(defaultValue = "10") int length) {
            return RandomStringUtils.randomAlphanumeric(length);
        }
    }
    ```

> Bu misollar `commons-lang3` kutubxonasining eng ko'p ishlatiladigan funksiyalarini ko'rsatadi. Kutubxonada bundan ham
> ko'proq imkoniyatlar mavjud.

## 7. `spring-boot-starter-parent` ning asosiy maqsadi va ahamiyati:

`spring-boot-starter-parent` - bu `Spring Boot` loyihangiz uchun tayyor shablon bo'lib, quyidagilarni avtomatik hal
qiladi:

1. "Qayta ixtiro qilishning oldini olish"

   Har bir yangi loyihada:

    - Qanday `Java` versiyasidan foydalanish kerak?
    - Qanday pluginlar kerak?
    - **Kodirovka** qanday bo'lishi kerak?
    - **Testlar** qanday ishlashi kerak?
    - > Bularning barchasini `spring-boot-starter-parent` avtomatik hal qiladi.

2. Haqiqiy hayot misoli

   Tasavvur qiling, siz restoran ochmoqchisiz. Sizga 2 variant:

    1. **O'zingiz hamma narsani tanlash:**
        - Stol stullarni tanlash
        - Idish-tovoqlarni tanlash
        - Oshxonaga jihozlar tanlash
        - Xodimlarni tanlash
    2. "Turnkey solution" tanlash:
        - Barchasi allaqachon tanlangan va moslashtirilgan tayyor restoran paketini olish
   > spring-boot-starter-parent - aynan ikkinchi variant!

3. Qanday ishlaydi?

   Oddiy loyiha (parentsiz):

    ```xml
    <project>
        <!-- Java versiyasini qo'lda sozlash -->
        <properties>
            <maven.compiler.source>17</maven.compiler.source>
            <maven.compiler.target>17</maven.compiler.target>
        </properties>
    
        <!-- Har bir kutubxona uchun versiya ko'rsatish -->
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>3.5.3</version> <!-- Versiyani qo'lda kiritish -->
            </dependency>
        </dependencies>
    
        <!-- Pluginlarni qo'lda sozlash -->
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.11.0</version>
                    <configuration>
                        <source>17</source>
                        <target>17</target>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </project>
    ```
   `Spring Boot` loyihasi (parent bilan):

    ```xml
    <project>
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>3.5.3</version>
        </parent>
    
        <!-- Faqat loyiha ma'lumotlari -->
        <groupId>com.example</groupId>
        <artifactId>my-app</artifactId>
        <version>1.0.0</version>
    
        <!-- Versiyasiz bog'liqliklar -->
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
        </dependencies>
    </project>
    ```

4. Qachon kerak emas?

   Agar siz:

    - `Legacy` loyihada ishlayotgan bo'lsangiz
    - Boshqa parent `POM` dan foydalanayotgan bo'lsangiz
    - Har bir detalni qo'lda boshqarmoqchi bo'lsangiz

> Lekin 95% hollarda - bu eng optimal yechim.Bu - Spring Bootning **sehrli formulasi** bo'lib, sizga faqat biznes
> mantiqiga e'tibor qaratish imkonini beradi!

## 8. `spring-boot-dependencies` haqida batafsil

---

1. Asosiy Tushuncha

   `spring-boot-dependencies` - bu Maven `BOM` (Bill of Materials) fayli bo'lib, `Spring Boot` bilan ishlaydigan barcha
   kutubxonalar uchun mos keladigan versiyalarni belgilaydi. Bu modul:

    - `300+` kutubxona uchun versiyalarni boshqaradi
    - Versiyalar to'qnashuvlarining oldini oladi
    - `Spring Boot` ekotizimidagi barcha komponentlarning bir-biri bilan mosligini ta'minlaydi

2. Qanday Ishlatiladi?

   a) To'g'ridan-to'g'ri import qilish

    ```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>3.5.3</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    ```
   Bu usul `spring-boot-starter-parent` dan foydalana olmagan hollarda (masalan, boshqa parent POM ishlatayotgan
   bo'lsangiz) qo'llaniladi.

   b) `Starter Parent` orqali

   `spring-boot-starter-parent` o'zida `spring-boot-dependencies` ni o'z ichiga oladi:

    ```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.3</version>
    </parent>
    ```
3. Asosiy Afzalliklari

    1. Versiya Boshqaruvi:
        - Barcha `Spring Boot` bog'liqliklari uchun mos versiyalarni avtomatik tanlash
        - Misol: `spring-boot-starter-web` qo'shganda, `Spring MVC`, `Tomcat` va `Jackson` kutubxonalari mos
          versiyalarda olinadi
    2. Moslik Kafolati:
        - `Spring Boot` versiyasini yangilaganingizda, barcha bog'liq kutubxonalar versiyalari avtomatik yangilanadi
    3. Ishni Soddalashtirish:
        - Har bir kutubxona uchun versiya ko'rsatish shart emas
          ```xml
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-data-jpa</artifactId>
              <!-- Versiya kerak emas -->
          </dependency>
          ```

4. Tarkibi va Versiyalar

   `spring-boot-dependencies` quyidagilarni o'z ichiga oladi

   Asosiy kutubxonalar:
    - Spring Framework (6.1.9)
    - Hibernate (6.6.18.Final)
    - Jackson (2.19.1)

   Ma'lumotlar bazasi:
    - HikariCP (6.3.0)
    - H2 (2.3.232)
    - MySQL (8.3.0)

   Test kutubxonalari:
    - JUnit Jupiter (5.12.2)
    - Mockito (5.11.0)

   Pluginlar:
    - Maven Compiler Plugin (3.14)
    - Spring Boot Maven Plugin (3.5.3)

5. Versiyalarni O'zgartirish

   Agar ma'lum bir kutubxona versiyasini o'zgartirmoqchi bo'lsangiz:

    ```xml
    <properties>
        <hibernate.version>6.6.0.Final</hibernate.version>
    </properties>
    ```
   Yoki to'g'ridan-to'g'ri versiyani ko'rsatish:

    ```xml
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>6.6.0.Final</version>
    </dependency>
    ```

6. Starter Dependencies bilan Bog'liqlik

   `spring-boot-dependencies` `Spring Boot` starterlarining asosi hisoblanadi. Har bir starter (masalan,
   `spring-boot-starter-web`) o'ziga tegishli kutubxonalarni `spring-boot-dependencies` dagi versiyalar asosida oladi

7. Qanday Tanlash Kerak?

    - `spring-boot-starter-parent` - yangi loyihalar uchun eng yaxshi tanlov
    - `spring-boot-dependencies` - agar boshqa parent `POM` ishlatayotgan bo'lsangiz

8. Muhim Nuqtalar

    - Bu faqat versiyalarni boshqaradi, haqiqiy bog'liqliklarni qo'shish uchun `<dependencies>` dan foydalaning
    - `Spring Boot` yangilanganda, `spring-boot-dependencies` versiyasini ham yangilang
    - Versiyalarni qo'lda o'zgartirish tavsiya etilmaydi, chunki bu moslik muammolariga olib kelishi mumkin

> `spring-boot-dependencies` - bu Spring Bootning **"sehrli formulasi"** bo'lib, kutubxonalar versiyalarini boshqarish
> stressidan xalos qiladi va ishni sezilarli darajada soddalashtiradi

## 8. `spring-boot-starter-parent`

---

> **Oddiy tushuntirish:** Bu - `Spring Boot` loyihalari uchun standart sozlamalar to'plami. U sizning loyihangiz uchun
> kerakli barcha narsalarning versiyalarini avtomatik boshqaradi.

`Batafsil`:

- **Versiya boshqaruvi:** Barcha Spring Boot modullari uchun mos versiyalarni o'rnatadi

- **Standart konfiguratsiyalar:** Kompilyatsiya parametrlari, kodlash standartlari

- **Pluginlar sozlamalari:** Maven pluginlari uchun standart konfiguratsiyalar

- **Dependency management:** Boshqa kutubxonalar uchun mos versiyalarni ta'minlaydi

## 9. `spring-boot-dependencies` - Dependency boshqaruvi

---

`Oddiy tushuntirish`: Bu - `spring-boot-starter-parent` ning dependency boshqaruv qismi, lekin ota POM sifatida emas.

`Batafsil`:

Faqat dependency versiyalarini boshqaradi

Loyiha strukturasi yoki pluginlarni sozlamaydi

Boshqa POM-lardan meros olish kerak bo'lganda ishlatiladi

## 10. `spring-cloud-dependencies`

---

> `spring-cloud-dependencies` - bu `Spring Cloud` loyihalarida ishlatiladigan maxsus `POM` (Project Object Model) fayli
> bo'lib, Spring Cloud komponentlarining versiyalarini markaziy ravishda boshqarish uchun ishlatiladi.

Asosiy tushuncha

Bu nima qiladi?

- Barcha Spring Cloud modullari uchun mos keladigan versiyalarni ta'minlaydi
- Dependency versiyalari o'rtasidagi konfliktlarning oldini oladi
- Spring Boot versiyasi bilan mosligini avtomatik ravishda ta'minlaydi













































































































































































































