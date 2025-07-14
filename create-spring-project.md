## 1. `spring-boot-starter-parent` va `spring-boot-starter` lardan foydalangan holda loyiha yaratish ketma ket qadamlar

---

1. Loyihani Boshlash:

  - `IDE orqali (IntelliJ IDEA)`:

    ![](images/1-step-spring-begin.png)
    ![](images/1-step-spring.png)
    ![](images/2-step-spring.png)
    

2. Asosiy Projekt Strukturasi

   Loyiha yaratilgandan so'ng, quyidagi struktura paydo bo'ladi:

   Misol uchun `GroupId=uz.test` va `ArtifactId=my-app` deb tanlasangiz, loyiha strukturasi quyidagicha bo'ladi:

    ![](images/4-step-spring.png)

   ![](images/3-step-spring.png)


3. Loyihani ochish va tahlil qilish

    - `pom.xml` fayliga quyidagilar qo'shilganligini tekshiring:

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
        <groupId>uz.test</groupId>
        <artifactId>my-app</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <name>Test-uchun</name>
        <description>Test-uchun</description>
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
                <artifactId>spring-boot-starter-data-jpa</artifactId>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
    
            <dependency>
                <groupId>org.postgresql</groupId>
                <artifactId>postgresql</artifactId>
                <scope>runtime</scope>
            </dependency>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <optional>true</optional>
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
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <annotationProcessorPaths>
                            <path>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                            </path>
                        </annotationProcessorPaths>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <configuration>
                        <excludes>
                            <exclude>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                            </exclude>
                        </excludes>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    
    </project>
    
    ```


4.  Asosiy aplikatsiya klassini tekshirish

    `src/main/java/uz/test/myapp/TestUchunApplication.java:`

    ```java
    package uz.test.myapp;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    public class TestUchunApplication {
        public static void main(String[] args) {
            SpringApplication.run(TestUchunApplication.class, args);
        }
    }
    ```
5. `Application.properties` sozlamalari:

   `src/main/resources/application.properties` fayliga quyidagilarni qo'shing:

     ```properties
     # PostgreSQL konfiguratsiyasi
     spring.datasource.url=jdbc:postgresql://localhost:5432/testdb
     spring.datasource.username=postgres
     spring.datasource.password=password
     spring.datasource.driver-class-name=org.postgresql.Driver
     
     # JPA/Hibernate sozlamalari
     spring.jpa.hibernate.ddl-auto=update
     spring.jpa.show-sql=true
     spring.jpa.properties.hibernate.format_sql=true
     
     # Server porti
     server.port=8080
     
     # Logging
     logging.level.uz.test.myapp=DEBUG
     ```
6.  `Entity`, `Repository`, `Service` va `Controller` yaratish
    -  ### 1. `Entity` Klassini Yaratish
       `src/main/java/uz/test/myapp/model/Product.java`:
        ```java
        package uz.test.myapp.model;
        
        import jakarta.persistence.*;
        import lombok.*;
        
        @Entity
        @Table(name = "products")
        @Data
        @NoArgsConstructor
        @AllArgsConstructor
        @Builder
        public class Product {
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
            
            @Column(nullable = false)
            private String name;
            
            @Column(nullable = false)
            private Double price;
            
            private String description;
        }
        ```
    -  ### 2. `Repository` interfeysini yaratish

       `src/main/java/uz/test/myapp/repository/ProductRepository.java`:

        ```java
        package uz.test.myapp.repository;
        
        import uz.test.myapp.model.Product;
        import org.springframework.data.jpa.repository.JpaRepository;
        
        public interface ProductRepository extends JpaRepository<Product, Long> {
        }
        ```
    - ### 3. `Service Layer`

      `src/main/java/uz/test/myapp/service/ProductService.java`:

      ```java
      package uz.test.myapp.service;
      
      import uz.test.myapp.model.Product;
      import java.util.List;
      
      public interface ProductService {
          Product saveProduct(Product product);
          List<Product> getAllProducts();
          Product getProductById(Long id);
          Product updateProduct(Product product);
          void deleteProduct(Long id);
      }
      ```
      
      `src/main/java/uz/test/myapp/service/impl/ProductServiceImpl.java`:

       ```java
       package uz.test.myapp.service.impl;
       
       import lombok.RequiredArgsConstructor;
       import org.springframework.stereotype.Service;
       import uz.test.myapp.model.Product;
       import uz.test.myapp.repository.ProductRepository;
       import uz.test.myapp.service.ProductService;
       import java.util.List;
       
       @Service
       @RequiredArgsConstructor
       public class ProductServiceImpl implements ProductService {
           
           private final ProductRepository productRepository;
           
           @Override
           public Product saveProduct(Product product) {
               return productRepository.save(product);
           }
           
           @Override
           public List<Product> getAllProducts() {
               return productRepository.findAll();
           }
           
           @Override
           public Product getProductById(Long id) {
               return productRepository.findById(id)
                       .orElseThrow(() -> new RuntimeException("Product topilmadi"));
           }
           
           @Override
           public Product updateProduct(Product product) {
               Product existingProduct = getProductById(product.getId());
               existingProduct.setName(product.getName());
               existingProduct.setPrice(product.getPrice());
               existingProduct.setDescription(product.getDescription());
               return productRepository.save(existingProduct);
           }
           
           @Override
           public void deleteProduct(Long id) {
               productRepository.deleteById(id);
           }
       }
       ```
    - ### 4. `REST Controller` yaratish

      `src/main/java/uz/test/myapp/controller/ProductController.java`:

       ```java
       package uz.test.myapp.controller;
       
       import lombok.RequiredArgsConstructor;
       import org.springframework.http.HttpStatus;
       import org.springframework.web.bind.annotation.*;
       import uz.test.myapp.model.Product;
       import uz.test.myapp.service.ProductService;
       import java.util.List;
       
       @RestController
       @RequestMapping("/api/products")
       @RequiredArgsConstructor
       public class ProductController {
           
           private final ProductService productService;
           
           @PostMapping
           @ResponseStatus(HttpStatus.CREATED)
           public Product saveProduct(@RequestBody Product product) {
               return productService.saveProduct(product);
           }
           
           @GetMapping
           public List<Product> getAllProducts() {
               return productService.getAllProducts();
           }
           
           @GetMapping("/{id}")
           public Product getProductById(@PathVariable Long id) {
               return productService.getProductById(id);
           }
           
           @PutMapping
           public Product updateProduct(@RequestBody Product product) {
               return productService.updateProduct(product);
           }
           
           @DeleteMapping("/{id}")
           public void deleteProduct(@PathVariable Long id) {
               productService.deleteProduct(id);
           }
       }
       ```
7. Loyihani ishga tushirish va test qilish

    - ### 1. `PostgreSQL` serverini ishga tushiring:
       ```text
       docker run --name test-postgres -e POSTGRES_PASSWORD=password -p 5432:5432 -d postgres
       ```
   - ### 2. Ma'lumotlar bazasini yarating:
     ```sql
     CREATE DATABASE testdb;
     ```

   - ### 3. `IntelliJ IDEA`-da loyihani ishga tushiring:
     - `TestUchunApplication` klassini oching
     - Bosh menyudan `Run` > `Run 'MyAppApplication'` ni tanlang yoki `Shift+F10` tugmalarini bosing

   - ### 4. `Postman` yoki `cURL` orqali test qiling:

     ```text
     # Yangi product qo'shish
     curl -X POST http://localhost:8080/api/products \
     -H "Content-Type: application/json" \
     -d '{"name": "Test Product", "price": 99.99, "description": "Test description"}'
     
     # Barcha productlarni olish
     curl http://localhost:8080/api/products
     ```
8. Qo'shimcha Sozlamalar (Ixtiyoriy)

   - `Lombok` pluginini yoqish:
     - `File` > `Settings` > `Plugins`
     - `Lombok` pluginini qidiring va yoqing
     - `Apply` va `OK` tugmalarini bosing
     - `IntelliJ` ni qayta ishga tushiring

   - `Validation` qo'shish:
     - `pom.xml` ga qo'shing:
        ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        ```
     - `Entity` va `Controller` da validatsiya qo'llashingiz mumkin  

## 2. `Spring Boot` Starter Nima?

---

> `Spring Boot Starter` - bu `Spring Boot` frameworkining asosiy qismlaridan biri bo'lib, turli xil funksionallikni osongina loyihangizga qo'shish imkonini beradi.

### Mashhur `Spring Boot` Starterlar:

- `spring-boot-starter-web` - Web ilovalar uchun
- `spring-boot-starter-data-jpa` - Ma'lumotlar bazasi bilan ishlash uchun
- `spring-boot-starter-security` - Autentifikatsiya va avtorizatsiya uchun
- `spring-boot-starter-test` - Testlash uchun

## 3. Yangi `dependencey` larni `IntelliJ` orqali qanday qo'shsam bo'ladi

---

- `pom.xml` faylini oching
- `<dependencies>` ichiga kursorni qo'ying
- `Alt+Insert` Yordamida (Windows)

    ![](images/add-starter.png)
    ![](images/2-step-spring.png)





















































































































































































