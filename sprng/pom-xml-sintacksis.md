## 1. Quyidagi pom.xml file ichidagi narsalrni batafsil tushuntir:

---

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>


    <groupId>uz.micro_service</groupId>
    <artifactId>parent-starter</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>parent-starter</name>
    <description>parent-starter</description>


    <url/>
    <licenses>
        <license/>
    </licenses>
    <developers>
        <developer/>
    </developers>

    <scm>
        <connection/>
        <developerConnection/>
        <tag/>
        <url/>
    </scm>

    <properties>
        <java.version>21</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

1. `<?xml version="1.0" encoding="UTF-8"?>`:
 - Bu qator XML faylining `prolog` (kirish qismi) deyiladi va quyidagi vazifalarni bajaradi:
   - Bu qator faylning `XML` formatida ekanligini bildiradi
 - `version="1.0"`:
   - `XML 1.0` versiyasidan foydalanilayotganini bildiradi
 - `encoding="UTF-8"`
   - `UTF-8` - faylda ishlatiladigan belgilar kodlash turi
   - `UTF-8` xalqaro belgilar (shu jumladan kirill, arab, xitoy va boshqa belgilar) bilan ishlash imkonini beradi
   - Agar `encoding` ko'rsatilmasa, dastur standart encodingdan foydalanadi (bu muammolarga olib kelishi mumkin)

2. `<modelVersion>4.0.0</modelVersion>`
   - Maven POM model versiyasi (4.0.0 - eng yangi va barqaror versiya)
3. Parent Konfiguratsiyasi:

    ```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    ```
   - `parent`: Loyiha `Spring Boot parent POM` dan meros oladi
     - `groupId`, `artifactId`, `version`: Parent POM identifikatorlari
     - `relativePath`: Parentni avval lokal papkalardan qidirish (bo'sh bo'lsa, to'g'ridan-to'g'ri repositorydan oladi)

4.  Loyiha Identifikatorlari

    ```xml
    <groupId>uz.micro_service</groupId>
    <artifactId>parent-starter</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    ```
    - `groupId`: Loyihaning guruh nomi (odatda tashkilot nomi)
    - `artifactId`: Loyiha nomi (jar fayl nomi bo'ladi)
    - `version`: Loyiha versiyasi (SNAPSHOT - ishlanayotgan versiya)

5. Loyiha Ma'lumotlari:

    ```xml
    <name>parent-starter</name>
    <description>parent-starter</description>
    ```
    - Loyiha nomi va tavsifi (inspektorga mo'ljallangan)


























































