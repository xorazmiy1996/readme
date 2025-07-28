## 1. Tomcat Server Nimaga Kerak? (Soddaroq Tushuntirish)

`Tomcat` — bu veb-server, ya'ni brauzer yoki mobil ilovalardan kelgan so'rovlarni qabul qilib, Java dasturlaringizga yetkazib beradi va javobni qaytaradi.

###  Nima uchun kerak?

1. Brauzer `↔` Java Dastur Oraliqchi
  - Siz brauzerga http://localhost:8080/users deb yozsangiz, `Tomcat` bu so'rovni olib, `Spring Boot` dasturingizdagi `@GetMapping("/users")` metodiga yo'naltiradi.
  - Natijani yana brauzerga qaytaradi.

2. Nega alohida server emas?
  - O'zi Bilgan Birga Keladi (`Embedded`- o'rnatilgan)
    - Avvalgi Java ilovalarini ishga tushirish uchun tashqi `Tomcat` serverini o'rnatish kerak edi.
    - `Spring Boot` esa o'z ichida `Tomcat'ni` olib yuradi — qo'shimcha server sozlash kerak emas! 

3. Qanday ishga tushadi?
    - Siz `main()` metodini ishga tushirsangiz, `Spring Boot` ichidagi `Tomcat` avtomatik ish boshlaydi.
    - **8080**-portda brauzer so'rovlarini kutadi.
    - Agar siz `@RestController` yozsangiz, `Tomcat` uni avtomatik taniydi.

4. Nega Boshqa Serverlar Kerak Emas?

   - Oddiy ilovalar uchun `Tomcat` yetarli (tez, oson).
   - Katta yuklar uchun `Jetty` yoki `Undertow` ishlatish mumkin (lekin ular ham xuddi shunday ishlaydi).
   - **Xulosa:** `Tomcat` — `Spring Boot` ilovangizni internetga ulaydigan "karobka". Siz faqat kod yozasiz, qolganini `Tomcat` o'zi qiladi!















































































































