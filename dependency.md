## 1. `dependency` lar  deb nimaga aytiladi?

> Dependency - sizning loyihangiz ishlashi uchun kerak bo'lgan boshqa loyihalar/kutubxonalar. Misol uchun
> Dependency  - bu dasturiy ta'minot loyihasi to'g'ri ishlashi uchun zarur bo'lgan tashqi kutubxonalar yoki modullar. Keling, buni tushunarli qilib tushuntirayman


### Qanday Ishga Tushadi?

1. `pom.xml` da dependency e'lon qilasiz
2. `Maven dependency` ni:
    - Avval local `.m2/repository` dan qidiradi
3. `Build` paytida `dependency` lar `classpath` ga qo'shiladi    - Topilmasa, `remote` repositorydan yuklaydi
4.  Final `jar` fayliga kiritiladi (scope ga qarab)

### Real Hayotdan Misol

Restoranda taom tayyorlayotganingizni tasavvur qiling:

- **Sizning loyihangiz** - siz tayyorlayotgan taom
- **Dependencylar** - tayyor ingredientlar (souslar, ziravorlar)
- **Local repository** - sizning muzlatgichingiz
- **Remote repository** - do'kon (agar muzlatgichda yo'q bo'lsa)

> Xulosa qilib aytganda, **dependency** lar - bu professional dasturchilarning ishini osonlashtiradigan va loyihalarni tezroq rivojlantirishga yordam beradigan tayyor komponentlar.

### Asosiy Dependency Strukturasi:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>3.2.0</version>
        <scope>compile</scope>
        <optional>false</optional>
        <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-api</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```
1. `<groupId>` - Guruh identifikatori
   - Dependencyni ishlab chiqargan tashkilot yoki jamoa
   - Domain nomiga teskari tartibda yoziladi
   - **Namuna:** org.springframework.boot, com.google.guava

2. `<artifactId>` - Artefakt identifikatori
   - Loyihaning o'ziga xos nomi
   - Odatiy ravishda kutubxona nomi bilan bir xil
   - **Namuna:** spring-boot-starter-web, lombok

3. `<optional>` - Ixtiyoriylik (ixtiyoriy)
   - `true` qilsangiz, bu dependencyni ishlatadigan loyihalar uni avtomatik olmaydi
   - Odatiy qiymati: `false`

4. `<exclusions>` - Istisnolar (ixtiyoriy)
   - `Transitive` dependencylarni (vositali bog'liqliklarni) chetlab o'tish uchun
   - **Namuna:** Tomcat o'rniga `Jetty` ishlatish uchun

    ```xml
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
    ```
    Versiyalarni `properties` bo'limida markazlashtirish yaxshi amaliyot hisoblanadi:
    
    ```xml
    <properties>
        <spring-boot.version>3.2.0</spring-boot.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>${spring-boot.version}</version>
        </dependency>
    </dependencies>
    ```

Xatolarga yo'l qo'ymaslik uchun:

1. Har doim `groupId`, `artifactId` va `version` ni ko'rsating
2. Versiyalarni `properties` bo'limida markazlashtiring
3. Dependency tree ni tekshirish uchun `mvn dependency:tree` dan foydalaning
4. Noto'g'ri dependencylar uchun `mvn dependency:analyze` buyrug'idan foydalaning





































































































