## 1. `Spring`, `Spring MVC` va `Spring Boot` haqida tushuncha.

---

### 1. `Spring Framework` - Asosiy Framework
Spring - bu framework (dasturiy ramka), ya'ni dasturlashni osonlashtirish uchun ishlab chiqilgan vositalar to'plami. Bu Java dasturlash tilida murakkab yoki korporativ darajadagi ilovalarni yaratishni soddalashtiradi.

### `Spring Framework` ning asosiy xususiyatlari:

- `Dependency Injection (DI)`: Spring asosiy maqsadi - komponentlar orasidagi bog'liqlikni boshqarish.
- `Aspect-Oriented Programming (AOP)`: Dasturga qo'shimcha funksiyalarni (masalan, loglash, xavfsizlik) kodni o'zgartirmasdan qo'shish imkonini beradi.
- `Transaction Management`: Ma’lumotlar bazasi tranzaktsiyalarini boshqarish imkonini beradi.
- `Spring Modules`:
  - `Spring Core` (`DI` va `AOP` uchun).
  - `Spring JDBC` (ma'lumotlar bazasi bilan ishlashni soddalashtiradi).
  - `Spring ORM` (`Hibernate` va `JPA` bilan integratsiya).
  - `Spring MVC` (Web ilovalar uchun mo'ljallangan modul).
  - `Spring Security` (Xavfsizlikni boshqarish).

> **Xulosa**: `Spring Framework` - bu butun boshli ekotizim yoki platforma. Spring MVC va Spring Boot bu ekotizimning bir qismidir.

## 2. `Spring MVC` - `Spring Framework` ning Web Texnologiyasi

---

`Spring MVC` - bu `Spring Framework` ning bir moduli bo'lib, veb-ilovalarni yaratish uchun `Model-View-Controller (MVC)` arxitekturasidan foydalanadi.

### `Spring MVC` texnologiyasi nima uchun ishlatiladi?
- Veb-so'rovlarni boshqarish: `HTTP` so'rovlarini qabul qilish va ularga javob qaytarish.
- `MVC arxitekturasi`: Ilovani quyidagi qismlarga ajratadi:
  - `Model`: Ma'lumotlar va biznes mantiqni ifodalaydi.
  - `View`: Foydalanuvchi interfeysini yaratadi (`HTML`, `JSP`, `Thymeleaf` kabi).
  - `Controller`: Foydalanuvchi so'rovlarini qabul qiladi va javoblarni tayyorlaydi.
- `Spring DispatcherServlet`: `MVC` jarayonini boshqaruvchi asosiy komponent.


> `Xulosa`: `Spring MVC` - bu `Spring Framework` ning web texnologiyasi bo'lib, veb-ilovalar yaratish uchun ishlatiladi.

## 3. `Spring Boot` - `x x` ning Tezlashtirilgan Versiyasi

---

### `Spring Boot` - bu `Spring Framework` ustiga qurilgan texnologiya bo'lib, `Spring` komponentlarini avtomatlashtirish va tezlashtirish uchun mo'ljallangan.

- `Auto Configuration`: Spring konfiguratsiyasini avtomatlashtiradi (masalan, `Tomcat` serverni avtomatik sozlash).
- `Starter Dependencies`: Loyihaga kerakli kutubxonalarni oson qo'shish imkoniyatini beradi (masalan, `spring-boot-starter-web`).
- `Embedded Servers`: Ichki `Tomcat`, `Jetty` yoki `Undertow` serverlari bilan birga keladi.
- `Spring Boot Actuator`: **Monitoring** va **diagnostikani** osonlashtiradi.
- `Spring Initializr`: Yangi `Spring Boot` loyihasini yaratishni osonlashtiradigan vosita.

> **Xulosa**: `Spring Boot` - bu `Spring Framework` ning tezkor rivojlanish va avtomatik konfiguratsiya texnologiyasi.

## 4. `Spring Framework`, `Spring MVC` va `Spring Boot` o'rtasidagi bog'liqlik

---

- `Spring Framework`: Asosiy platforma yoki framework (barcha komponentlar uchun asos).
- `Spring MVC`: `Spring Framework` ning web texnologiyasi. Bu `Spring Framework` ichidagi bir modul.
- `Spring Boot`: `Spring Framework` ustiga qurilgan texnologiya bo'lib, `Spring MVC` va boshqa komponentlarni avtomatlashtirish va tezlashtirish uchun ishlatiladi.

### Misol orqali tushuntirish
1. `Spring Framework`: Siz katta korporativ ilova qurmoqchisiz. Siz `Spring Framework` dan foydalanib, komponentlarni qo'lda sozlaysiz (masalan, `DI`, `AOP`, `Transaction Management`).
   - Bu juda ko'p konfiguratsiya va sozlashni talab qiladi.

2. `Spring MVC`: Siz `Spring Framework` yordamida web ilova yaratmoqchisiz. So'rovlarni boshqarish, View yaratish va Model bilan ishlash uchun `Spring MVC` dan foydalanasiz.
   - Masalan: `Veb-sayt` yoki foydalanuvchi interfeysiga ega bo'lgan ilova.

3. `Spring Boot`: Siz `Spring Framework` asosida tezroq va kamroq konfiguratsiya bilan ilova yaratmoqchisiz. `Spring Boot` avtomatik konfiguratsiya va kutubxonalarni boshqarishni o'z ichiga oladi.
   - Masalan: `RESTful API` yaratish yoki mikroservis arxitekturasini qurish.


## 5. `Spring Boot` va `Spring MVC` birgalikda ishlaydimi?

---

Ha, ishlaydi. `Spring Boot` aslida `Spring MVC` ni ichida ishlatadi. Agar `Spring Boot` yordamida veb-ilova yaratmoqchi bo'lsangiz, aslida siz `Spring MVC` modulidan foydalanasiz.

`Misol`:

```java
@SpringBootApplication
@RestController
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, World!";
    }
}
```
- Bu `Spring Boot` loyihasi, lekin unda `Spring MVC` ishlatilgan (`@RestController` va `@GetMapping` `Spring MVC` annotatsiyalari).

**Shunday qilib**: 
- `Spring Framework` - bu asosiy framework (platforma).
- `Spring MVC` - bu `Spring Framework` ichidagi web texnologiyasi.
- `Spring Boot` - bu `Spring Framework` ustiga qurilgan avtomatlashtiruvchi texnologiya.

> **Xulosa**: Shunday qilib, Spring - bu framework, lekin `Spring MVC` va `Spring Boot` - texnologiyalar yoki vositalar bo'lib, ular `Spring Framework` asosida ishlaydi.

### 6. Dependency Injection (DI) nima?

`Dependency Injection (DI)` - bu dizayn naqshlari **(design pattern)** dan biri bo'lib, bir obyektning boshqa obyektlarga bo'lgan bog'liqliklarini **(dependencies)** qo'lda yaratish o'rniga avtomatik ravishda ta'minlash metodidir. Bu`Spring Framework` kabi dasturiy platformalarda keng qo'llaniladi.

> `DI` ning asosiy maqsadi - komponentlarning o'zaro bog'liqligini boshqarish va kodni moslashuvchan va test qilish oson bo'lishini ta'minlash.

Dasturda bir obyekt boshqa obyektga bog'liq bo'lishi mumkin. 

`Misol`:

```java
public class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine(); // Car obyekt Engine obyektiga bog'liq
    }
}
```
**Yuqorida:**

- `Car` obyektining ishlashi uchun `Engine` obyektiga ehtiyoj bor. Bu yerda `Car` obyektining `Engine` obyektiga bog'liqligi `(dependency)` mavjud.
  
### Muammo nima?
- `Car` klassi ichida `Engine` obyektini o'zi yaratadi. Bu esa kodni kam moslashuvchan qiladi.
- Agar biz `Engine` ni almashtirishni xohlasak (masalan, boshqa turdagi dvigatel qo'shish), `Car` klassini o'zgartirishimiz kerak bo'ladi.
- Kodni testlash ham qiyinlashadi, chunki biz `Car` ni test qilish uchun haqiqiy `Engine` obyektini yaratishimiz kerak bo'ladi.


### `DI` bilan bu muammoni qanday hal qilish mumkin?

`Dependency Injection` da obyektlar o'z bog'liqliklarini o'zlari yaratmaydi, balki tashqaridan ularga taqdim etiladi. 

`Misol`:

```java
public class Car {
    private Engine engine;

    public Car(Engine engine) { // Bog'liqlik tashqaridan kiritiladi
        this.engine = engine;
    }
}
```

- Endi `Car` obyektini yaratishda, biz unga `Engine` obyektini tashqaridan uzatamiz:

```java
Engine engine = new Engine();
Car car = new Car(engine);
```
Bu `Dependency Injection` deb ataladi. Endi `Car` klassi `Engine` obyektini qanday yaratishni bilmaydi va faqat foydalanadi.


### `Spring Framework` da `Dependency Injection` qanday ishlaydi?

`Spring Framework` `DI` ni avtomatlashtirish uchun `IoC konteyner` dan foydalanadi. `IoC konteyner` quyidagilarni amalga oshiradi:

- Obyektlarni yaratadi.
- Ularning bog'liqliklarini o'rnatadi.
- Ularni boshqaradi va hayotiy siklini nazorat qiladi.

### `DI Spring` da quyidagicha amalga oshiriladi:

1. **Annotatsiyalar** yordamida:
   - `@Component`: Klassni Spring konteyneriga kiritadi.
   - `@Autowired`: Bog'liqlikni avtomatik sozlaydi.

    `Misol`:
    ```java
    @Component
    public class Engine {
        // Engine klassi
    }
    
    @Component
    public class Car {
        @Autowired
        private Engine engine; // DI avtomatik
    }
    ```
2. `XML` konfiguratsiya yordamida:

- `Spring konteyner` bog'liqliklarni `XML` faylda belgilangan konfiguratsiyalar asosida amalga oshiradi.

  `Misol`:
    ```xml
    <bean id="engine" class="com.example.Engine" />
    <bean id="car" class="com.example.Car">
    <property name="engine" ref="engine" />
    </bean>
    ```
3. `Java` konfiguratsiya yordamida:
   - Spring konteynerni Java klasslar orqali sozlash.
   
   `Misol`:

    ```java
    @Configuration
    public class AppConfig {
        @Bean
        public Engine engine() {
            return new Engine();
        }
    
        @Bean
        public Car car() {
            return new Car(engine());
        }
    }
    ```

### `DI` va `IoC` o'rtasidagi farq

- `Inversion of Control (IoC)` - bu printsip. `IoC` da obyektlar o'zlarini boshqarish o'rniga tashqi boshqaruvni qabul qiladi.
- `Dependency Injection (DI)` - `IoC` ni amalga oshirishning bir usuli. `DI` obyektlar orasidagi bog'liqlikni avtomatik taqdim etadi.

> `Xulosa`:Dependency Injection (DI) - bu obyektlar orasidagi bog'liqlikni boshqarish usuli bo'lib, obyektlar o'z bog'liqliklarini yaratish o'rniga, ularga bog'liqlikni tashqi manba `(Spring IoC konteyner)` taqdim etadi. Bu kodni moslashuvchan, qayta ishlatiladigan va testlashga yaroqli qiladi.

## 7. Spring IoC konteyneri nima?

`Spring IoC` konteyneri (`Inversion of Control Container`) - bu Spring Frameworkning asosiy qismi bo'lib, u ilova komponentlarini boshqarish va ularning bog'liqliklarini avtomatik sozlash uchun ishlatiladi. `IoC` konteyneri:

1. Obyektlarni yaratadi,
2. Bog'liqliklarni ta'minlaydi,
3. Obyektlarning hayot siklini boshqaradi.

`IoC` konteyneri `Inversion of Control (IoC)` printsipi asosida ishlaydi, ya'ni obyektlar o'z bog'liqliklarini o'zlari yaratish o'rniga, ularni konteyner orqali oladi.

### `IoC` konteynerining asosiy vazifalari

1. **Obyektlarni boshqarish (Bean Management)**:
   - `IoC` konteyner beanlarni yaratadi va ularni boshqaradi.
   - `Bean` - bu `Spring IoC` konteyneri tomonidan boshqariladigan har qanday obyekt.

2. `Dependency Injection (DI)`:
   - `IoC konteyner` obyektlarning bog'liqliklarini avtomatik sozlaydi (`constructor`, `setter` yoki `field injection` orqali).

3. **Hayot siklini boshqarish**:
   - `IoC konteyner` obyektlarni yaratish, ishga tushirish va yo'q qilish jarayonlarini boshqaradi.

4. **Konfiguratsiyani boshqarish**:
   - Konteyner obyektlarni yaratish uchun kerakli konfiguratsiyalarni o'qiydi (masalan, `XML`, `Java` yoki **annotatsiyalar** orqali).

### `Spring IoC` konteyneridan foydalanish usullari

1. `XML` konfiguratsiya yordamida

IoC konteynerni yaratish uchun XML fayl ishlatiladi.

`Misol`:

`XML konfiguratsiya`:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!-- Beanlarni aniqlash -->
    <bean id="engine" class="com.example.Engine" />
    <bean id="car" class="com.example.Car">
        <property name="engine" ref="engine" />
    </bean>
</beans>
```

**Java kod:**

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

Car car = context.getBean("car", Car.class);
car.drive();

```
2. Annotatsiyalar yordamida konfiguratsiya

Annotatsiyalar yordamida `IoC` konteynerni sozlash sodda va zamonaviy usuldir.

`Misol`:

```java
@Component
public class Engine {
    public void start() {
        System.out.println("Engine started");
    }
}

@Component
public class Car {
    @Autowired
    private Engine engine;

    public void drive() {
        engine.start();
        System.out.println("Car is driving");
    }
}

@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(MyApplication.class, args);
        Car car = context.getBean(Car.class);
        car.drive();
    }
}
```
**Annotatsiyalar:**

`@Component`: Bean sifatida belgilaydi.
`@Autowired`: Bog'liqlikni avtomatik sozlaydi.

3. Java konfiguratsiya yordamida

`Java` konfiguratsiya `XML` o'rniga `Java` kod yordamida beanlarni aniqlashga imkon beradi.

`Misol`:

```java
@Configuration
public class AppConfig {
    @Bean
    public Engine engine() {
        return new Engine();
    }

    @Bean
    public Car car() {
        return new Car(engine());
    }
}

public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        Car car = context.getBean(Car.class);
        car.drive();
    }
}
```

`Xulosa`:
- `IoC` konteyneri - bu Spring Frameworkning yuragi bo'lib, obyektlarni yaratish, bog'liqliklarni boshqarish va hayot siklini nazorat qilish uchun ishlatiladi.
- Siz `IoC` konteyner bilan `XML`, `Java` konfiguratsiya, yoki annotatsiyalar orqali ishlashingiz mumkin.
- `ApplicationContext` `IoC` konteynerining kengaytirilgan versiyasi bo'lib, ko'p hollarda ishlatiladi.

## 8. Aspect-Oriented Programming (AOP)

---

`Aspect-Oriented Programming (AOP)` — bu dasturlash paradigmasi bo'lib, u dasturiy ta'minotning turli qismlariga qo'shimcha funksiyalarni (masalan, loglash, xavfsizlik, tranzaksiyalar) kodni o'zgartirmasdan qo'shish imkonini beradi. Bu, asosan, "aspekt" deb ataladigan modullar orqali amalga oshiriladi.

### Asosiy tushunchalar:

1. `Aspect`: Qo'shimcha funksiyalarni ifodalovchi modul. Masalan, loglash yoki xavfsizlik.
2. `Join Point`: Dastur bajarilishidagi aniq nuqtalar, masalan, metod chaqiruvlari yoki ob'ekt yaratish.
3. `Advice`: Aspekt tomonidan bajariladigan kod. Bu kod join pointlarda bajariladi. Masalan, metoddan oldin yoki keyin bajarilishi mumkin.
4. `Pointcut`: Qaysi join pointlar uchun advice qo'llanilishini aniqlovchi ifoda.
5. `Weaving`: Aspektlarning asosiy kodga qo'shilishi jarayoni. Bu yig'ilish vaqtida yoki ish vaqtida amalga oshirilishi mumkin.

### AOP ning afzalliklari:
- **Modularlik**: Qo'shimcha funksiyalarni alohida joyda saqlash va boshqarish imkonini beradi.
- **Kodning o'qilishi**: Asosiy biznes mantiqdan qo'shimcha funksiyalarni ajratish orqali kodni o'qishni osonlashtiradi.
- **Qayta ishlash**: Bir nechta joyda bir xilda ishlatiladigan funksiyalarni bir marta yozish va bir joyda boshqarish imkonini beradi.

## 9. `Transaction Management`

---

**Tranzaksiya** (transaksiya) deb pul, mol-mulk yoki maʼlumotlar bir shaxs yoki tashkilotdan boshqasiga oʻtish jarayoniga aytiladi. Bankda pul oʻtkazish, internetda toʻlov qilish yoki kriptovalyuta almashish kabi amaliyotlar tranzaksiya misollari boʻlishi mumkin. 


### `ACID` xususiyatlari:

1. `Atomicity`: Tranzaksiya yoki to'liq bajariladi, yoki to'liq bekor qilinadi.
2. `Consistency`: Tranzaksiya bajarilganda ma'lumotlar bazasining holati doimo to'g'ri bo'lishi kerak.
3. `Isolation`: Bir tranzaksiya boshqa tranzaksiyalar ta'sir qilmasdan bajariladi.
4. `Durability`: Tranzaksiya muvaffaqiyatli bajarilgandan so'ng, uning natijalari saqlanadi, hatto tizim muammolari yuzaga kelsa ham.

## 10. Spring JDBC nima?

---

`Spring JDBC` — bu `Spring` ramkasining bir qismi bo'lib, u `Java` dasturlarida `Relatsion Ma'lumotlar Bazalari (RDBMS)`bilan ishlashni soddalashtiradi. Bu modul **JDBC (Java Database Connectivity) API** bilan ishlashni qulaylashtiradi va ma'lumotlar bazasi operatsiyalari uchun ko'plab foydali funksiyalarni taqdim etadi.

## 11. `Spring Boot` da ilova yaratish ketma-ketligi:

---

1. `Spring Initializr` orqali loyiha yaratish.
   - `Spring Initializr` saytiga kiring: [start.spring.io](https://start.spring.io/)
   - Loyihani sozlang:
     - Project: `Maven` yoki `Gradle`
     - Language: `Java`
     - `Spring Boot` versiyasi: Eng so'nggi barqaror versiyani tanlang.
     - Group: com.example (o'zingizning guruhingiz)
     - Artifact: demo (o'zingizning loyiha nomingiz)
   - Dependencies qo'shing:
     - Spring Web
     - Spring Data JPA
     - H2 Database (yoki sizga kerakli boshqa ma'lumotlar bazasi)
   - `Generate` tugmasini bosing va loyiha `zip` faylini yuklab oling.
2. Loyihani ochish
   - `IDE` oching: `IntelliJ` IDEA yoki `Eclipse`.
   - Yuklangan faylni oching: `Zip` faylni oching va lohiyangizni `IDE` ga import qiling.
3. Asosiy faylni yaratish
```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

4. REST Kontrollerini qo'shish

    Kontroller sinfini yarating:

    ```java
    package com.example.demo.controller;
    
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;
    
    @RestController
    public class HelloController {
        @GetMapping("/hello")
        public String hello() {
            return "Salom, Spring Boot!";
        }
    }
   ```

5. Ilovani ishga tushirish
   - Loyihani ishga tushirish: `IDE` dan `DemoApplication` sinfini ishga tushiring yoki terminalda quyidagi buyruqni bajaring:
   
     ```bash 
     ./mvnw spring-boot:run
     ```
6. Brauzerda tekshirish
   - Brauzerni oching: `http://localhost:8080/hello` manziliga o'ting. **"Salom, Spring Boot!"** matnini ko'rishingiz kerak.

**Qo'shimcha ma'lumotlar**
- **Ma'lumotlar bazasi** va `JPA` bilan ishlash uchun `entity`, `repository` va `service` qatlamlarini qo'shishingiz mumkin.
- `Spring Boot` dasturini rivojlantirishda qo'shimcha modullar va konfiguratsiyalarni o'rganing.

## 12. `Maven` va `Gradle` nima uchun kerak.

---

`Maven` va `Gradle` — bu Java loyihalarini boshqarish uchun ishlatiladigan qurilish vositalari.

`Maven`:

1. Qurilish jarayoni:
   - `Maven` loyihangizni qurish, testdan o'tkazish, va tarqatish jarayonlarini avtomatlashtiradi.
   - `pom.xml` faylida kerakli `Dependency` va loyiha ma'lumotlarini ta'riflaysiz.
2. `Dependency Management`:
   - `Maven` yordamida loyihangizga kerakli kutubxonalarni `(Dependency)` qo'shishingiz mumkin. Bu `Dependency` lar  avtomatik ravishda yuklanadi va loyihangizda ishlatiladi.

3. Konventsiya asosida sozlash:
   - `Maven` konventsiya asosida ishlaydi, ya'ni ma'lum bir tuzilmani va jarayonlarni qo'llab-quvvatlaydi. Bu yangi foydalanuvchilar uchun osonroq.
   
    
`Gradle`:

1. Moslashuvchanlik:
   - Gradle ko'proq moslashuvchanlikni taklif etadi. Bu, ko'p tillar va loyihalar bilan ishlash imkonini beradi.
   - `build.gradle` faylida loyihangizni o'zingiz xohlagan tarzda sozlashingiz mumkin.

2. Performans:
   - `Gradle` juda tez ishlaydi, chunki u kesh mexanizmini qo'llaydi. Bu avval bajarilgan vazifalarni qayta bajarishdan qochish imkonini beradi.
3. Ko'p loyihalar uchun qo'llab-quvvatlash:
   - Agar sizda ko'p loyihalar bo'lsa, `Gradle` ularni bir joyda boshqarish imkoniyatini beradi.

### Nima uchun kerak?
- **Loyihani boshqarish**: Katta loyihalarda ko'plab kutubxonalar va modullar mavjud bo'lishi mumkin. `Maven` yoki `Gradle` yordamida bularni boshqarish juda oson.
- **Avtomatlashtirish**: Qurilish jarayonlarini, testlarni va tarqatishni avtomatlashtirish, dasturchilarga vaqt tejash va xatolarni kamaytirish imkonini beradi. 
- **`Dependency` larni yuklash**: Kutubxonalarni qo'shish va ularni yangilash jarayonini soddalashtiradi. Bu foydalanuvchilarga o'z loyihalarini tezroq rivojlantirish imkonini beradi.


## 13. Group: `com.example` nima uchun kerak.

`Group` — bu loyihangizni `unikal` tarzda **identifikatsiya** qilish uchun ishlatiladigan nom. Bu nom odatda sizning kompaniyangiz yoki tashkilotingizning domen nomidan kelib chiqadi. Keling, buni batafsil tushuntiray.

**Nima uchun kerak?**

Identifikatsiya - bu biror narsa yoki shaxsni aniqlash jarayoni

1. **Unikal identifikatsiya**:
    - Har bir `Java` loyihasi o'ziga xos guruh nomiga ega bo'lishi kerak. Bu, boshqa loyihalar bilan to'qnashuvlardan qochish uchun muhimdir.
    - Misol uchun, agar siz `com.example` ni tanlasangiz, boshqa dasturchilar ham o'z loyihalarida `com.example` dan foydalanishlari mumkin, lekin bu to'g'ri bo'lmaydi.
2. `Domen` nomidan kelib chiqishi:
    - Odatda, guruh nomi sizning domen nomingizga mos keladi. Masalan, agar sizda **mycompany.com** domeni bo'lsa, siz **com.mycompany** guruh nomini tanlashingiz mumkin.

3. **Organizatsiya**:
   - Loyihani tashkil qilishda yordam beradi. Guruh nomi orqali siz loyihangizni boshqa modullar va `dependency` bilan bog'lashingiz mumkin.


**Misollar:**
- **Individual dasturchilar**: Agar siz shaxsiy loyiha yaratayotgan bo'lsangiz, siz `com.yourname` (masalan, `com.johnsmith`) tanlashingiz mumkin.
- **Kompaniyalar**: Agar siz kompaniya uchun loyiha yaratayotgan bo'lsangiz, kompaniya domeningizga asoslangan nom tanlang (masalan, `org.companyname`).


Artefakt `(Artifact)` - bu inson qo'li bilan yaratilgan yoki o'zgartirilgan va madaniy, tarixiy yoki arxeologik ahamiyatga ega bo'lgan ob'ekt yoki buyum

`Artifact` — bu loyihangizning nomini belgilovchi parametrdir. U dasturiy ta'minot paketini yoki jar faylini yaratishda ishlatiladi. Keling, buni batafsil ko'rib chiqamiz.

1. Identifikatsiya:
   - Artifact nomi loyihangizni boshqa loyihalardan ajratib turadi. Bu, ayniqsa, bir nechta loyiha ustida ishlayotganingizda muhimdir.
2. Tashkil etish:
   - Artifact nomi jar fayli yoki boshqa chiqish fayllarini yaratishda ishlatiladi. Masalan, Maven yoki Gradle loyihangizni qurganingizda, chiqish fayli odatda artifact-name-version.jar (masalan, demo-1.0.0.jar) shaklida bo'ladi.
    

### Group va Artifact o'rtasidagi farq:

- `Group` nomi ko'pincha katalog tuzilmasida aks etadi. Misol uchun, `com.example` bo'lsa, sizning fayllaringiz src/main/java/com/example katalogida joylashadi.
- `Artifact` nomi yordamida siz bir xil group ostida bir nechta loyihalarni boshqarishingiz mumkin. Misol uchun, com.example.demo va com.example.api.



## 15. `Spring Data JPA` nima?

`Spring Data JPA` — bu `Spring Framework` ning bir qismi bo'lib, `Java Persistence API (JPA)` bilan ishlashni osonlashtiradi. U ma'lumotlar bazasi bilan bog'lanishni soddalashtirish, ma'lumotlarni saqlash va olish jarayonini avtomatlashtirish uchun mo'ljallangan.

### ORM Nima?

1. Ob'ekt va Ma'lumotlar Bazasi o'rtasidagi bog'lanish:
   - `ORM` yordamida dasturchilar ma'lumotlar bazasidagi jadval va ustunlarni Java ob'ektlari va ularning xususiyatlariga bog'laydilar. Bu, ma'lumotlar bilan ishlashni osonlashtiradi.

2. `SQL` yozmasdan ishlash:
   - `ORM` yordamida dasturchilar `SQL` so'rovlarini to'g'ridan-to'g'ri yozmasdan, ob'ektlar bilan ishlashadi. `ORM` kutubxonalari avtomatik ravishda `SQL` so'rovlarini yaratadi.

### Qanday ishlaydi?


1. `Entity` sinflarini yaratish:
   - Ma'lumotlar bazasidagi jadvalga mos keladigan Java sinflarini yaratish
2. Bog'lanmalarni aniqlash:
   - Ob'ektlar o'rtasidagi bog'lanishlar (masalan, bir-to'plam, ko'p-to'plam) aniqlanadi.
3. `CRUD` operatsiyalarini bajarish:
   - `ORM` yordamida ob'ektlar ustida `CRUD` (yaratish, o'qish, yangilash, o'chirish) operatsiyalarini bajarish.




































































































































































































































































































































































































































































































