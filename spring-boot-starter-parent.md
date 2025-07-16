## 1. `Spring Boot Starter Parent` nima?

---

`spring-boot-starter-parent` - bu maxsus `POM` (`Project Object Model`) fayli bo'lib, `Spring Boot` ilovalari uchun asosiy konfiguratsiyalarni taqdim etadi. U quyidagilarni o'z ichiga oladi:

1. Java versiyasi
2. UTF-8 kodlash
3. Standart plugin konfiguratsiyalari
4. `Dependency management` (versiyalarni avtomatik boshqarish)

Qo'llash usuli

`pom.xml `faylida quyidagicha ishlatiladi:

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version> <!-- Oxirgi versiyani ishlating -->
</parent>
```

Afzalliklari

1. **Versiya boshqaruvi:** Barcha `Spring Boot` bog'liqliklari uchun mos versiyalarni taqdim etadi.
    ```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!-- Versiya ko'rsatilmaydi, parent tomonidan boshqariladi -->
        </dependency>
    </dependencies>
    ```
2. **Standart konfiguratsiyalar:**

    - Kompilyatsiya uchun Java versiyasi
    - `UTF-8` kodlash
    - Resource filtratsiya


3. Plugin konfiguratsiyalari:

   - `spring-boot-maven-plugin` uchun optimal sozlamalar
   - `maven-surefire-plugin` va `maven-failsafe-plugin` uchun test sozlamalari


## 2. Microservislarda `Parent POM` bilan bog'lanish tushuntirishi

---

Microservis arxitekturasida pom.xml fayllarini parent POM bilan bog'lash bir necha usulda amalga oshirilishi mumkin. Quyida tushunarli tarzda tushuntiraman:

1. `Parent POM` yaratish va modullarni bog'lash

   Microservislar uchun umumiy `parent POM` yaratish eng yaxshi amaliyot hisoblanadi:

    ```xml
    <!-- Root/parent pom.xml -->
    <groupId>com.example</groupId>
    <artifactId>microservices-parent</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>
    
    <modules>
        <module>user-service</module>
        <module>order-service</module>
        <module>payment-service</module>
    </modules>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
    </parent>
    ```

    ### Har bir microservis modulida (masalan, `user-service/pom.xml`):

    ````xml
    <parent>
        <groupId>com.example</groupId>
        <artifactId>microservices-parent</artifactId>
        <version>1.0.0</version>
    </parent>
    
    <artifactId>user-service</artifactId>
    ````
2. Bog'liqliklarni markaziy boshqarish

---

`Parent POM` da `dependency management` qo'llash orqali barcha microservislarda bir xil versiyalardan foydalanish mumkin:

```xml
<!-- Parent POMda -->
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
```

3. Modullarda `dependency` ishlatish

   Har bir microservisda faqat `dependency` nomini ko'rsatish kifoya, versiya `parent` tomonidan boshqariladi:
    ```xml
    <!-- user-service/pom.xml -->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    ```

4. Ikkita darajali `parent` bog'lanish
   - **Birinchi daraja:** `spring-boot-starter-parent` (`Spring Boot` tomonidan taqdim etiladi)
   - **Ikkinchi daraja:** Sizning loyihangizning parent POMi

    Bu ierarxiya sizga:

   - `Spring Boot` ning standart sozlamalaridan foydalanish 
   - Loyihaga xos sozlamalarni qo'shish imkoniyatini berad

5. Bog'lanishni tekshirish

    Bog'lanish to'g'ri ishlayotganligini tekshirish uchun:

   - Har bir modulda `mvn clean install` buyrug'ini ishga tushiring
   - Modullar orasidagi versiyalar bir xil ekanligini tekshiring
   - `Dependency tree` ni ko'rish uchun mvn `dependency:tree` buyrug'idan foydalaning

Muhim nuqtalar:

- **Modullar mustaqil ishlashi kerak:** Har bir microservis mustaqil deploy qilinishi mumkin bo'lishi kerak
- **Versiya konflikti:** `Parent POM` yordamida versiya konfliktlarining oldini olish mumkin
- **Plugin boshqaruvi:** `Build` pluginlarini ham `parent POM` da boshqarish mumkin

Bu usul yordamida siz barcha microservislaringizdagi `pom.xml` fayllarini markaziy ravishda boshqarishingiz va ularni bir-biri bilan bog'lashingiz mumkin.

## 3. Ikkita Darajali Parent Bog'lanish Usuli (Multi-level Parent POM)

--- 

Microservis loyihalarida ikkita darajali parent bog'lanish - bu kuchli strukturaviy yechim bo'lib, `Spring Boot` loyihalarida versiya boshqaruvi va standart sozlamalarni saqlash uchun ishlatiladi.


1. Tushuncha diagrammasi

    ```text
    spring-boot-starter-parent (1-daraja)
           ↑
    your-company-parent-pom (2-daraja)
           ↑
    microservice-module-pom
    ```

2. To'liq misol

    ` Birinchi daraja: Spring Boot Parent`

    ```xml
    <!-- Birinchi daraja parent: spring-boot-starter-parent -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
        <relativePath/> <!-- Maven markaziy repodan yuklab oladi -->
    </parent>
    ```
    **Ikkinchi daraja:** Kompaniya `Parent POM`

    ```xml
    <!-- company-parent/pom.xml -->
    <project>
        <modelVersion>4.0.0</modelVersion>
    
        <!-- Spring Boot parentga bog'lanish -->
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>3.2.0</version>
        </parent>
    
        <groupId>com.yourcompany</groupId>
        <artifactId>company-parent</artifactId>
        <version>1.0.0</version>
        <packaging>pom</packaging>
    
        <properties>
            <java.version>17</java.version>
            <spring-cloud.version>2023.0.0</spring-cloud.version>
            <company.common-lib.version>2.5.0</company.common-lib.version>
        </properties>
    
        <dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-dependencies</artifactId>
                    <version>${spring-cloud.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>
    
                <!-- Kompaniya ichidagi umumiy kutubxonalar -->
                <dependency>
                    <groupId>com.yourcompany.common</groupId>
                    <artifactId>logging-starter</artifactId>
                    <version>${company.common-lib.version}</version>
                </dependency>
            </dependencies>
        </dependencyManagement>
    
        <build>
            <pluginManagement>
                <plugins>
                    <plugin>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-maven-plugin</artifactId>
                    </plugin>
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-compiler-plugin</artifactId>
                        <configuration>
                            <source>${java.version}</source>
                            <target>${java.version}</target>
                        </configuration>
                    </plugin>
                </plugins>
            </pluginManagement>
        </build>
    </project>
    ```

**Microservis Moduli**

```xml
<!-- user-service/pom.xml -->
<project>
    <modelVersion>4.0.0</modelVersion>
    
    <!-- Kompaniya parentga bog'lanish -->
    <parent>
        <groupId>com.yourcompany</groupId>
        <artifactId>company-parent</artifactId>
        <version>1.0.0</version>
        <relativePath>../company-parent/pom.xml</relativePath>
    </parent>
    
    <artifactId>user-service</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    
    <dependencies>
        <!-- Versiyalarni ko'rsatish shart emas -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        
        <dependency>
            <groupId>com.yourcompany.common</groupId>
            <artifactId>logging-starter</artifactId>
        </dependency>
    </dependencies>
</project>
```    

3. Afzalliklari

   - Versiya izchilligi:
     - Barcha loyihalarda bir xil `Spring Boot` va `dependency` versiyalari
     - Kompaniya standartlarini markazlashtirish



`Modullar tuzilishi`:

```text
project-root/
├── company-parent/
│   └── pom.xml
├── user-service/
│   └── pom.xml
├── order-service/
│   └── pom.xml
└── pom.xml (aggregator)
```
















































































































































































