## 1. `Windows` da portni tozalash

`Windows uchun`:

1. Portni band qilgan jarayonni topish:

    ```text
    netstat -ano | findstr :8081
    tasklist | findstr <PID>
    ```
   Natijada ko'rsatilgan `PID` (`Process ID`) raqamini eslab qoling.

   `Misol natija`:

    ```text
    TCP    0.0.0.0:8081           0.0.0.0:0              LISTENING       1234
    ```

2. Jarayon haqida ma'lumot olish:

    ```text
    tasklist | findstr 1234
    ```
   Bu sizga qaysi dastur (masalan, `java.exe`) portni band qilayotganini ko'rsatadi.

3. Jarayonni to'xtatish:

    ```text
    taskkill /PID 1234 /F
    ```

    > `/F` flagi jarayonni majburiy to'xtatish uchun ishlatiladi.






























































































