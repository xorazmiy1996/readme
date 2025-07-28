## 1. Mikroservis parent `POM.xml` strukturasi:

1. Asosiy Struktura
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    
        <!-- 1. Asosiy identifikatorlar -->
        <groupId>uz.micro_service</groupId>
        <artifactId>microservices-parent</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <packaging>pom</packaging> <!-- Parent POM uchun majburiy -->
    
        <!-- 2. Parent konfiguratsiyasi (agar mavjud bo'lsa) -->
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>3.5.3</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
    
        <!-- 3. Umumiy xususiyatlar -->
        <properties>
            <java.version>21</java.version>
            <spring-cloud.version>2025.0.0</spring-cloud.version>
            <spring-boot.version>3.5.3</spring-boot.version>
            <maven.compiler.source>${java.version}</maven.compiler.source>
            <maven.compiler.target>${java.version}</maven.compiler.target>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        </properties>
    
        <!-- 4. Modullar ro'yxati (agar multi-module bo'lsa) -->
        <modules>
            <module>service-discovery</module>
            <module>api-gateway</module>
            <module>user-service</module>
            <module>order-service</module>
        </modules>
    
        <!-- 5. Bog'liqliklar boshqaruvi -->
        <dependencyManagement>
            <dependencies>
                <!-- Spring Cloud Dependency Management -->
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-dependencies</artifactId>
                    <version>${spring-cloud.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>
    
                <!-- Umumiy mikroservis bog'liqliklari -->
                <dependency>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-web</artifactId>
                    <version>${spring-boot.version}</version>
                </dependency>
                
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
                    <version>${spring-cloud.version}</version>
                </dependency>
                
                <dependency>
                    <groupId>org.springdoc</groupId>
                    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
                    <version>2.1.0</version>
                </dependency>
            </dependencies>
        </dependencyManagement>
    
        <!-- 6. Umumiy bog'liqliklar (barcha mikroservislarda kerak bo'lganlar) -->
        <dependencies>
            <!-- Lombok (faqat kompilyatsiya vaqtida kerak) -->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.28</version>
                <scope>provided</scope>
                <optional>true</optional>
            </dependency>
    
            <!-- Test bog'liqliklari -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
        </dependencies>
    
        <!-- 7. Qurish konfiguratsiyasi -->
        <build>
            <pluginManagement>
                <plugins>
                    <!-- Java kompilyatsiya plugin -->
                    <plugin>
                        <groupId>org.apache.maven.plugins</groupId>
                        <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.11.0</version>
                        <configuration>
                            <source>${java.version}</source>
                            <target>${java.version}</target>
                            <annotationProcessorPaths>
                                <path>
                                    <groupId>org.projectlombok</groupId>
                                    <artifactId>lombok</artifactId>
                                    <version>1.18.28</version>
                                </path>
                            </annotationProcessorPaths>
                        </configuration>
                    </plugin>
    
                    <!-- Spring Boot plugin -->
                    <plugin>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-maven-plugin</artifactId>
                        <version>${spring-boot.version}</version>
                        <configuration>
                            <excludes>
                                <exclude>
                                    <groupId>org.projectlombok</groupId>
                                    <artifactId>lombok</artifactId>
                                </exclude>
                            </excludes>
                        </configuration>
                    </plugin>
    
                    <!-- Docker image yaratish plugin (agar kerak bo'lsa) -->
                    <plugin>
                        <groupId>com.google.cloud.tools</groupId>
                        <artifactId>jib-maven-plugin</artifactId>
                        <version>3.3.2</version>
                    </plugin>
                </plugins>
            </pluginManagement>
    
            <!-- Barcha mikroservislarga tegishli umumiy pluginlar -->
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>3.0.0</version>
                </plugin>
            </plugins>
        </build>
    
        <!-- 8. Profillar (agar kerak bo'lsa) -->
        <profiles>
            <profile>
                <id>dev</id>
                <properties>
                    <spring.profiles.active>dev</spring.profiles.active>
                </properties>
                <activation>
                    <activeByDefault>true</activeByDefault>
                </activation>
            </profile>
            
            <profile>
                <id>prod</id>
                <properties>
                    <spring.profiles.active>prod</spring.profiles.active>
                </properties>
            </profile>
        </profiles>
    </project>
    ```
2. Muhim nuqtalar va izohlar

   - `<packaging>pom</packaging>`
     - Parent POM fayllari uchun majburiy
     - Bu fayl hech qanday jar/war yaratmaydi, balki boshqa modullarni boshqarish uchun ishlatiladi
   - `<dependencyManagement>`
     - Barcha mikroservislarda ishlatiladigan kutubxonalarning versiyalarini markaziy boshqarish
     - Child loyihalarda versiyalarni takrorlash shart emas
   - `<pluginManagement>`
     - Barcha mikroservislarda bir xil plugin konfiguratsiyalarini ta'minlash
     - Build jarayonini standartlashtirish
   - **Umumiy Bog'liqliklar**
     - Faqat barcha mikroservislarda kerak bo'ladigan kutubxonalarni qo'shing
     - Maxsus bog'liqliklar har bir mikroservisning o'z POM faylida bo'lishi kerak

   - **Microservice Modullari**
     - `<modules>` bo'limida barcha mikroservislarni ro'yxatini keltirishingiz mumkin
     - Har bir modul alohida papkada joylashgan bo'lishi kerak

3. Child Microservice `POM.xml` Misoli

    Parent `POM` dan foydalanadigan mikroservis `POM` fayli quyidagicha bo'ladi:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    
        <parent>
            <groupId>uz.micro_service</groupId>
            <artifactId>microservices-parent</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </parent>
    
        <artifactId>user-service</artifactId>
        <packaging>jar</packaging>
    
        <dependencies>
            <!-- Parentdan meros olingan umumiy bog'liqliklar avtomatik qo'shiladi -->
            
            <!-- Maxsus bog'liqliklar -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-jpa</artifactId>
            </dependency>
            
            <dependency>
                <groupId>org.postgresql</groupId>
                <artifactId>postgresql</artifactId>
            </dependency>
        </dependencies>
    </project>
    ```

4. Yaxshi Amaliyotlar

   1. **Versiyalarni propertiesda saqlang** - barcha versiyalarni <properties> bo'limida boshqaring
   2. **Modullarni mantiqiy guruhlang** - bir xil funksionallikdagi mikroservislarni birga joylang
   3. **Dependency scopeni to'g'ri belgilang** - provided, runtime, test scopelardan to'g'ri foydalaning
   4. **Plugin konfiguratsiyasini optimallashtiring** - keraksiz pluginlarni olib tashlang
   5. **Profillardan foydalaning** - turli muhitlar (`dev`, `test`, `prod`) uchun alohida sozlashlar

> Bu struktura mikroservislaringizni yaxshi tashkil etishga, versiyalarni markaziy boshqarishga va kodni qayta ishlatish imkoniyatini beradi.

## 2. `<dependencyManagement>` va `<dependencies>` farqlari va birgalikda ishlatilishi:

1. `<dependencies>` - Haqiqiy bog'liqliklar

   Bu oddiy bog'liqliklarni qo'shish uchun ishlatiladi. Agar siz bu yerda versiya ko'rsatmasangiz, `Maven` xato beradi.

   `Misol`:
    ```xml
    <dependencies>
        <!-- Versiya bilan -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.3.20</version>
        </dependency>
        
        <!-- Versiyasiz (xato beradi) -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId> <!-- ERROR: Versiya kerak -->
        </dependency>
    </dependencies>
    ```

2. `<dependencyManagement>` - Versiyalarni boshqarish

   Bu esa versiyalarni "shablon" qilib belgilash uchun. Uning o'zi hech qanday bog'liqlikni loyihaga qo'shmaydi.

    ```xml
    <dependencyManagement>
        <dependencies>
            <!-- Bu faqat versiyalarni belgilaydi -->
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
                <version>5.3.20</version> <!-- Versiya shabloni -->
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <dependencies>
    <!-- Endi versiyasiz ishlatish mumkin -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId> <!-- Versiya avtomatik oladi -->
    </dependency>
    </dependencies>
    ```
3. Nima uchun ikkalasi kerak?

   Vazifa taqsimoti:

    - `<dependencyManagement>`: "Qanday versiyalardan foydalanish kerak"ni aytadi
    - `<dependencies>`: "Qaysi kutubxonalarni haqiqatan ham ishlatish kerak"ni aytadi

### Real hayot misoli:

`parent/pom.xml`:


```xml
<dependencyManagement>
    <dependencies>
        <!-- Butun kompaniya uchun standart versiyalar -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.28</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

`child-module/pom.xml`:

```xml
<dependencies>
    <!-- Versiyasiz, chunki parentda belgilangan -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
</dependencies>
```

















































































































