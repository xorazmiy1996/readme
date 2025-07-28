## 1. **Mikroservislar** (`Microservices`) bilan ishlash

> Mikroservislar arxitekturasi - bu katta ilovalarni mustaqil ishlaydigan, kichik va o'zaro aloqador servislarga bo'lish usuli. `Spring Boot` bilan mikroservislar yaratish juda qulay.

### **Mikroservislarni** o'rganish uchun asosiy qadamlar:

1. **Mikroservis asoslari**
   - **`Monolit` va mikroservis farqlari:** Har bir **mikroservis** alohida jarayon sifatida ishlaydi va yengil protokollar (`HTTP/REST`) orqali kommunikatsiya qiladi
   - **Avtonomlik:** Har bir **servis** mustaqil ishlashi va o'z ma'lumotlar bazasiga ega bo'lishi kerak
   - **Domain-Driven Design (DDD):** Mikroservislarni biznes funksiyalari bo'yicha ajratish


2. **`Spring Boot` bilan mikroservis yaratish**

    ```java
    @SpringBootApplication
    public class UserServiceApplication {
        public static void main(String[] args) {
            SpringApplication.run(UserServiceApplication.class, args);
        }
    }
    
    @RestController
    @RequestMapping("/users")
    public class UserController {
        
        @GetMapping("/{id}")
        public ResponseEntity<User> getUser(@PathVariable Long id) {
            // User ni qidirish logikasi
            return ResponseEntity.ok(user);
        }
    }
    ```
3. **Mikroservislar uchun kerak bo'ladigan asosiy texnologiyalar:**

    - **1 `Service Discovery` (Eureka, Consul)**
      - Servislarni bir-birini topishi uchun
    - **2 `API Gateway` (Spring Cloud Gateway)**
      - Barcha so'rovlarni boshqarish va routing qilish
    - **3 `Config Server` (Spring Cloud Config)**
      - Markaziy konfiguratsiya boshqaruvi
    - **4 `Distributed Tracing` (Sleuth + Zipkin)**
      - So'rovlarni tizim bo'ylab kuzatish
    - **5 `Resilience Patterns` (Hystrix, Resilience4j)**
      - Circuit breaker, retry, fallback mexanizmlari

4. **Mikroservislar orasida kommunikatsiya usullari:**

    - `REST API` (Eng ko'p ishlatiladi)
    - `Message Brokers` (RabbitMQ, Kafka)
    - `gRPC` (Tezroq binary protokol)

### O'rganish uchun amaliy loyiha taklifi:

1. **Oddiy 3 ta mikroservis yarating:**
   - `User Service` (Foydalanuvchilar bilan ishlash)
   - `Product Service` (Mahsulotlar bilan ishlash)
   - `Order Service` (Buyurtmalar bilan ishlash)

2. **Ularni quyidagilar bilan jihozlang**

   - `Eureka Server` (Service discovery)
   - `API Gateway` (Barcha so'rovlar kirish nuqtasi)
   - `Config Server` (Markaziy sozlashlar)

3. **Servislar orasida `REST API` va `RabbitMQ` orqali kommunikatsiya qiling**


## 2. Mikroservis Loyihasini Boshlash: Batafsil Qo'llanma

> Quyida `Spring Boot` bilan mikroservis arxitekturasini boshlash uchun to'liq qadamlar keltirilgan:


1. **Loyiha tuzilmasini tayyorlash**

   Avvalo, asosiy modullarni yaratamiz:

    ```text
    microservices-project/
    ├── discovery-server/           # Eureka Server
    ├── api-gateway/                 # API Gateway
    ├── config-server/               # Config Server
    ├── user-service/                # Foydalanuvchilar servisi
    ├── product-service/             # Mahsulotlar servisi
    └── order-service/               # Buyurtmalar servisi
    ```

2. **`Discovery Server` (`Eureka`) yaratish**

    - **1-qadam:** `Spring Initializr` dan yangi loyiha yarating:
      - Dependencylar: `Eureka Server`
    
    - **2-qadam:** Asosiy klassni sozlash:

      ```java
      @SpringBootApplication
      @EnableEurekaServer
      public class DiscoveryServerApplication {
          public static void main(String[] args) {
              SpringApplication.run(DiscoveryServerApplication.class, args);
          }
      }
      ```
    - **3-qadam:** `application.yml` faylini sozlash:

      ```yaml
      server:
        port: 8761
      
      eureka:
        client:
          register-with-eureka: false
          fetch-registry: false
      ```

3. **`API Gateway` yaratish**

   - **1-qadam:** Yangi loyiha yarating:
     - **Dependencylar:** `Gateway`, `Eureka Discovery Client`
   
   - **2-qadam:** Asosiy klass:
       ```java
       @SpringBootApplication
       @EnableDiscoveryClient
       public class ApiGatewayApplication {
           public static void main(String[] args) {
               SpringApplication.run(ApiGatewayApplication.class, args);
           }
       }
       ```
   - **3-qadam:** `application.yml`:

       ```yaml
       server:
         port: 8888
       
       spring:
         application:
           name: config-server
         cloud:
           config:
             server:
               git:
                 uri: https://github.com/sizning-config-repo
                 clone-on-start: true
       ```
4. **`Config Server` yaratish**

   - **1-qadam: Yangi loyiha yarating:**
     - **Dependencylar:** `Config Server`
    
   - 2-qadam: Asosiy klass:
       ```java
       @SpringBootApplication
       @EnableConfigServer
       public class ConfigServerApplication {
           public static void main(String[] args) {
               SpringApplication.run(ConfigServerApplication.class, args);
           }
       }
       ```
   - **3-qadam: application.yml:**
       ```yaml
       server:
         port: 8888
       
       spring:
         application:
           name: config-server
         cloud:
           config:
             server:
               git:
                 uri: https://github.com/sizning-config-repo
                 clone-on-start: true
       ```

5. **Birinchi Mikroservis (`User Service`) yaratish**

    - **1-qadam**: Yangi loyiha yarating:
      - Dependencylar: `Web`, `Eureka Discovery Client`, `Lombok`

    - **2-qadam: Asosiy klass:**
        ```yaml
        @SpringBootApplication
        @EnableDiscoveryClient
        public class UserServiceApplication {
            public static void main(String[] args) {
                SpringApplication.run(UserServiceApplication.class, args);
            }
        }
        ```
    - **3-qadam: `bootstrap.yml` (`Config Server` bilan ishlash uchun):**
        ```yaml
        spring:
          application:
            name: user-service
          cloud:
            config:
              uri: http://localhost:8888
        ```
    - **4-qadam: Oddiy `REST controller`:**

        ```java
        @RestController
        @RequestMapping("/users")
        public class UserController {
        
            @GetMapping("/{id}")
            public ResponseEntity<User> getUser(@PathVariable Long id) {
                User user = new User(id, "Foydalanuvchi " + id, "user" + id + "@example.com");
                return ResponseEntity.ok(user);
            }
        }
        
        @Data
        @NoArgsConstructor
        @AllArgsConstructor
        class User {
            private Long id;
            private String name;
            private String email;
        }
        ```
6. **Loyihalarni ishga tushurish tartibi**

    - **1** Birinchi bo'lib `Discovery Server` ni ishga tushiring (8761 port) 
    - **2** Keyin `Config Server` ni ishga tushiring (8888 port)*
    - **3** So'ng `API Gateway` ni ishga tushiring (8080 port) 
    - **4** Nihoyat, `User Service` ni ishga tushiring 
 
7. **Test qilish**

    - **1** `Eureka` dashboardini tekshiring: http://localhost:8761
    - **2** `API Gateway` orqali user servisga so'rov yuboring: http://localhost:8080/api/users/1

> Keyingi qadam sifatida Product va Order servislarini ham shu tartibda yaratishingiz mumkin. Har bir yangi servis Eureka Serverga registratsiya qilinadi va API Gateway orqali erishish mumkin bo'ladi.

## 3. `application.yml` va `application.properties` Faylari haqida:

> `application.yml` va `application.properties` fayllari bir xil maqsadda ishlatiladi, lekin ular farq qiluvchi formatlardir.

### Asosiy Farqlar:

1. **Format:**
   - `.properties` - an'anaviy format (key=value)
      ```properties
      server.port=8080
      spring.datasource.url=jdbc:mysql://localhost:3306/mydb
      ```
   - `.yml` (YAML format) - ierarxik tuzilma

      ```yaml
      server:
        port: 8080
      spring:
        datasource:
          url: jdbc:mysql://localhost:3306/mydb
      ```

2. Qo'llab-quvvatlash:

   - Ikkala `format` ham `Spring` tomonidan to'liq qo'llab-quvvatlanadi
   - `.yml` fayllarni o'qish osonroq, ayniqsa murakkab konfiguratsiyalar uchun

### Muhim Nuqtalar:

1. Agar loyihada ikkala fayl ham bo'lsa, `.properties` fayl ustunlik qiladi
2. `bootstrap.yml/bootstrap.properties` - Spring Cloud config uchun (ilova ishga tushishidan oldin o'qiladi)
3. Faqat bitta formatni tanlash tavsiya etiladi (aralash ishlatmaslik yaxshi)

### Konvertatsiya Misoli:

- `.properties:`

    ```properties
    spring.datasource.url=jdbc:mysql://localhost:3306/mydb
    spring.datasource.username=root
    spring.datasource.password=secret
    ```

- `.yml`:

    ```yaml
    spring:
      datasource:
        url: jdbc:mysql://localhost:3306/mydb
        username: root
        password: secret
    ```

> Agar siz yangi `Spring Boot` loyihasini boshlayotgan bo'lsangiz, men `.yml` formatini tavsiya qilaman, chunki u mikroservis konfiguratsiyalari uchun ancha qulay va tushunarli.


## 4. Yuqoridagi har-bir service haqida ma'lumot.

`Service Discovery`:

> Service Discovery - Servisni topish mexanizmi. Bu mikroservis arxitekturasida turli servislar bir-birlarini avtomatik ravishda topishini ta'minlovchi tizim. Bu mexanizm bo'lmasa, har bir servis boshqa servislarning joylashgan manzillarini qo'lda sozlashga majbur bo'ladi.












































































































































































































































