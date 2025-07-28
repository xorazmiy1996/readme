## 1. `application.yml` va `bootstrap.yml` fayllarining ahamiyati.

> Microservislarda **ikkala fayl ham kerak**, lekin ular turli maqsadlarda ishlatiladi:

1. `bootstrap.yml` va `application.yml` farqlari

   | Parametr | 	`bootstrap.yml` | 	`application.yml`                                 |
   |----------|----------------|--------------------------------------------------|
   |Yuklash tartibi| Birinchi yuklanadi (ilova ishga tushishidan oldin)| Keyin yuklanadi                                  |
   |Asosiy maqsad |Tizim darajasidagi sozlamalar (Config Server, Eureka)| Ilova darajasidagi sozlamalar (DB, port)         |
   |Majburiylik| Faqat Spring Cloud loyihalarida kerak| Barcha Spring Boot loyihalarida kerak            |

2. Har bir **microservice** uchun ikkala fayl ham kerak

   **`Config Server` uchun**:

    - `bootstrap.yml`: Config Serverning o'zi uchun sozlamalar (`Git/SVN` ulanishi)
    - `application.yml`: `Port`, `security`, `management` endpointlari

   **`Service Discovery` (Eureka) uchun**:

    - `bootstrap.yml`: spring.cloud.config.enabled=false (o'zi config kerak emas)
    - `application.yml`: Eureka server sozlamalari, port

   **API Gateway uchun:**

    - `bootstrap.yml`: Config Serverga ulanish
    - `application.yml`: Route sozlamalari, filterlar

   **User/Order Service uchun**:

    - `ootstrap.yml`: Config Server va Eureka ulanishi
    - `application.yml`: DB ulanishi, business logic sozlamalari


3. Qo'shimcha fayllar tizimi

   Har bir service uchun tavsiya etilgan fayl tuzilishi:
    
    ```text
    src/main/resources/
    ├── bootstrap.yml          # Majburiy (Spring Cloud uchun)
    ├── bootstrap-dev.yml      # Ixtiyoriy (profilga xos)
    ├── application.yml        # Majburiy
    ├── application-dev.yml    # Ixtiyoriy
    └── application-prod.yml   # Ixtiyoriy
    ```

4. Misol: `User Service` uchun to'liq konfiguratsiya

    `bootstrap.yml`:

    ```yaml
    spring:
      application:
        name: user-service
      cloud:
        config:
          uri: http://config-server:8888
          name: user-service,common
          profile: ${SPRING_PROFILES_ACTIVE:dev}
    ```

   `application.yml`:

    ```yaml
    server:
      port: 8081
    
    spring:
      datasource:
        url: jdbc:mysql://${DB_HOST:localhost}:3306/userdb
        username: ${DB_USER:root}
        password: ${DB_PASSWORD:}
    
    management:
      endpoints:
        web:
          exposure:
            include: health, metrics, info
    ```
5. Qachon faqat `application.yml` yetarli?

   Agar loyihangizda:

   - Spring Cloud ishlatmasangiz (Config Server, Eureka, etc.)
   - Barcha sozlamalarni lokal faylda saqlasangiz
   - Markaziy konfiguratsiya kerak bo'lmasa
   faqat `application.yml` ishlatishingiz mumkin.


6. Muhim maslahatlar

   - **Xavfsizlik**: Parollarni hech qachon to'g'ridan-to'g'ri faylga yozmang, environment variablesdan foydalaning
   - **Profillar**: Har bir muhit (`dev`, `test`, `prod`) uchun alohida konfiguratsiyalar yarating
   - Override qilish: `bootstrap.yml` dagi sozlamalarni `application.yml` da override qilishingiz mumkin

> Ikkala fayl ham microservislarning to'g'ri ishlashi uchun muhim ahamiyatga ega. `bootstrap.yml` - tizim konfiguratsiyasi, `application.yml` - ilova konfiguratsiyasi uchun.
















































































































































