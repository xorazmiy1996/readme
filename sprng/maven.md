## 1. Agar siz Maven ni IntelliJ IDEA orqali o'rnatgan bo'lsangiz, quyidagi ma'lumotlar sizga foydali bo'ladi:

1. `IntelliJ IDEA`-da `Maven` joylashuvini aniqlash

    - `IntelliJ` ni oching
    - `File` > `Settings` (`Windows`/`Linux`) yoki `IntelliJ IDEA `> `Preferences` (`Mac`) 
    - `Build, Execution, Deployment` > `Build Tool`s > `Maven` bo'limiga o'ting

    Bu yerda siz 3 muhim yo'lni ko'rishingiz mumkin:

    - `Maven home directory` - Bu sizning `MAVEN_HOME` manzilingiz
    - `User settings file` - Foydalanuvchi `settings.xml` fayli joylashuvi
    - `Local repository` - Lokal repository manzili
   
2. `MAVEN_HOME` ni `IntelliJ` orqali topish

   `IntelliJ` odatda 2 xil usulda `Maven` bilan ishlaydi:

   - Bundled Maven (IntelliJ bilan kelgan):
     - **Joylashuvi:** IntelliJ ning o'z papkalari ichida
     - `MAVEN_HOME`: `IntelliJ/plugins/maven/lib/maven3`
   - Tashqi `Maven` (siz o'rnatgan):
     - Joylashuvi: Siz ko'rsatgan papkada
     - Uni ko'rish uchun: `Settings` > `Build Tools` > `Maven` > **"Maven home path"**

3. `settings.xml` faylini topish va tahrirlash

   IntelliJ IDEA orqali `settings.xml` ni tahrirlash:

    - `Maven settings` oynasiga o'ting (yuqorida ko'rsatilgan)
    - `User settings file` maydonidagi manzilni ko'ring
    - Agar fayl mavjud bo'lmasa:
      - `Override` tugmasini bosing
      - `Create settings.xml` tugmasini bosing

4. `IntelliJ`-da `Maven` terminalidan foydalanish

   `IntelliJ` ichidagi terminalda `MAVEN_HOME` ni tekshirish:

    ```bash
    # Windows
    echo %MAVEN_HOME%
    ```

5. `Maven` loyihalarini `IntelliJ`-da boshqarish

   - `Maven` panelini ochish:
     - Odatda o'ng tomonda `Maven` paneli bor
     - Yo'q bo'lsa: `View` > `Tool Windows` > `Maven`
   
   - Maven buyruqlarini bajarish:
     - Maven panelida `lifecycleda` istalgan buyruqni 2 marta bosish (clean, install, etc.)
     - Yoki terminaldan: mvn clean install

6. Maslahatlar

    - Bundled Maven:
      - `IntelliJ` bilan kelgan `Maven` ishlashi uchun `MAVEN_HOME` o'zgaruvchisi talab qilinmaydi
      - Lekin tashqi Maven ishlatmoqchi bo'lsangiz, uni `PATH` ga qo'shishingiz kerak
      - Lekin tashqi `Maven` ishlatmoqchi bo'lsangiz, uni `PATH` ga qo'shishingiz kerak
    - `settings.xml` ni o'zgartirish:
       ```xml
       <!-- Nexus server qo'shish misoli -->
       <mirrors>
         <mirror>
           <id>nexus</id>
           <url>http://localhost:8081/repository/maven-public/</url>
           <mirrorOf>*</mirrorOf>
         </mirror>
       </mirrors>
       ```
    - Muammolarga yechimlar:
      - Agar `Maven` buyruqlari ishlamasa: `File` > `Invalidate Caches`
      - Versiyalar mos kelmasa: `Project Structure` > `Project Settings` > `Project` > `SDK` va `Maven` versiyasini tekshiring

   `IntelliJ IDEA` `Maven` bilan ishlashda juda qulay muhit taqdim etadi, `MAVEN_HOME` ni qo'lda sozlash shart emas - `IDE` o'zi kerakli manzillarni aniqlay oladi.























































































