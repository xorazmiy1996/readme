## 1. `Spring Boot` loyihalari uchun Nexus Serverini ishga tushirish

Nexus Repository - bu Sonatype tomonidan ishlab chiqilgan artefaktlar (dependency) boshqaruv tizimi bo'lib, Spring Boot loyihalaringiz uchun shaxsiy (private) repository sifatida foydalanish mumkin. Quyida Nexus serverni ishga tushirish va Spring Boot loyihalaringiz bilan integratsiya qilish bo'yicha to'liq qo'llanma keltirilgan.

1. `Nexus Repository` ni o'rnatish

    Nexus ni o'rnatishning eng oson usuli - Docker orqali:
    ```bash
    docker run -d -p 8081:8081 -p 8083:8083 --name nexus -v nexus-data-new:/nexus-data sonatype/nexus3:3.76.0
    ```

2. Dastlabki sozlashlar

   - Brauzerdan http://localhost:8081 manzilini oching
   - Dastlabki admin parolini topish:
     ```bash
     docker exec -it nexus cat /nexus-data/admin.password
     ```
   - Parolni kiritib, yangi parol o'rnating

3. Repositorylar yaratish

    Spring Boot loyihalaringiz uchun quyidagi `repository` turlarini yaratishingiz kerak:

    - a) `Hosted Repository` (Shaxsiy repository)
      - **Maven (hosted):** O'zingizning shaxsiy artefaktlaringizni saqlash uchun
      - **Name:** maven-private
      - **Version policy:** Release yoki Snapshot (loyihangizga qarab)
    - b) Proxy Repository (Omborxona)
      - **Maven (proxy):** Maven Centralga proxy qilish uchun
      - **Name:** maven-central
      - **Remote storage:** https://repo1.maven.org/maven2/ 7
    - c) Group Repository (Guruh)
      - **Maven (group):** Bir nechta repositorylarni birlashtirish uchun
      - **Name:** maven-public
      - **Member repositories:** maven-private va maven-central 7
4. Maven sozlamalari

   `Spring Boot` loyihangizdan `Nexus` dan foydalanish uchun `settings.xml` fayliga quyidagilarni qo'shing:

    ```xml
    <servers>
      <server>
        <id>nexus</id>
        <username>admin</username>
        <password>sizning_parolingiz</password>
      </server>
    </servers>
    
    <profiles>
      <profile>
        <id>nexus</id>
        <repositories>
          <repository>
            <id>central</id>
            <url>http://localhost:8081/repository/maven-public/</url>
            <releases><enabled>true</enabled></releases>
            <snapshots><enabled>true</enabled></snapshots>
          </repository>
        </repositories>
      </profile>
    </profiles>
    
    <activeProfiles>
      <activeProfile>nexus</activeProfile>
    </activeProfiles>
    ```

5. `Spring Boot` loyihasini `Nexusga deploy` qilish

   Loyihangizning `pom.xml` fayliga quyidagilarni qo'shing:

    ```xml
    <distributionManagement>
      <repository>
        <id>nexus</id>
        <url>http://localhost:8081/repository/maven-private/</url>
      </repository>
      <snapshotRepository>
        <id>nexus</id>
        <url>http://localhost:8081/repository/maven-private/</url>
      </snapshotRepository>
    </distributionManagement>
    ```

   Keyin loyihangizni deploy qilish uchun: 

    ```bash
    mvn clean deploy
    ```
6. Nexusdan dependency olish

    Boshqa loyihalarda Nexusdan `dependency` olish uchun `pom.xml` ga oddiy `dependency` qo'shish kifoya:

    ```xml
    <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-spring-boot-app</artifactId>
      <version>1.0.0</version>
    </dependency>
    ```









































































































































































































