## 1. `Cannot resolve symbol 'SpringApplication'` Xatosini Tuzatish

![](images/dependency-error.png)

> Bu xato odatda `Spring Boot` **dependencylari** to'g'ri sozlanmaganligi yoki `IDE` **(IntelliJ IDEA**) `cache` muammolari tufayli yuzaga keladi.

1. Asosiy Tuzatish Usullari
   - `Maven` Dependencylarni Yangilash
     - `IntelliJ IDEA`-ning o'ng tomonidagi `Maven` panelini oching
     - `Reload` iconini (⟳) bosing yoki
       - `mvn clean install` (terminalda)
       - `mvn dependency:resolve` (dependencylarni qayta yuklash uchun)

   -  `IDE Cache` ni Tozalash
     - `File > Invalidate Caches...` ni tanlang
     - `Invalidate and Restart` tugmasini bosing
     - `IDE` qayta yuklanganidan so'ng loyihani qayta oching

   - `Java SDK` ni Tekshirish
     - `File > Project Structure` (Ctrl+Alt+Shift+S)
     - `Project Settings > Project` bo'limida:
       - `Project SDK`: `Java 21` tanlanganligiga ishonch hosil qiling
       - `Project language level`: 21 tanlang

2. Batafsil Qadamlar

   -  `pom.xml` faylini tekshirish

      Quyidagilar to'g'ri ko'rsatilganligiga ishonch hosil qiling:

      ```xml
      <parent>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-parent</artifactId>
          <version>3.2.0</version> <!-- Oxirgi versiya -->
          <relativePath/> <!-- lookup parent from repository -->
      </parent>
      
      <dependencies>
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter</artifactId>
          </dependency>
          <!-- Boshqa dependencylar... -->
      </dependencies>
      ```

   - Dependencylarni qo'lda yuklash

     Agar avvalgi usullar ishlamasa:

     - Terminalda loyiha ildizida:

       ```bash
       mvn clean
       mvn compile
       ```
   - `IntelliJ IDEA` Sozlamalari
     - `Settings > Build, Execution, Deployment > Build Tools > Maven`
       - `Always update snapshots` ni yoqing
       - `Import Maven projects automatically` ni yoqing


3. Alternativ Usullar

   - `.iml` faylni o'chirish
     - Loyiha ildizidagi `.idea` papkasida `.iml` faylni topib o'chiring
     - Loyihani qayta oching

   - `Dependency Tree` ni tekshirish

   Natijada `spring-boot-starter` ko'rinishi kerak.  

> Agar barcha usullar ishlamasa, loyihangizni yopib, `.idea` papkasini va `target` papkasini o'chirib, qayta ochish yaxshi yechim bo'lishi mumkin.















































































































































































































































































