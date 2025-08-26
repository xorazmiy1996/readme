## 1. `Spring Boot` da `ResponseEntity` nima uchun ishlatilinadi.

`Spring Boot` da `ResponseEntity` klassi `HTTP` responselarni boshqarish va sozlash uchun foydalaniladi. Bu `Spring MVC`
va `Spring WebFlux` da mavjud bo'lib, `HTTP status` kodi, `respons body` (javob tanasi), va headerlarni qaytarish
imkonini beradi.

1. `HTTP Status` Kodini Boshqarish

`ResponseEntity` yordamida `HTTP status` kodini aniq belgilash mumkin. Masalan, siz `200 (OK)`, `404 (Not Found)`, yoki
`500 (Internal Server Error)` kabi status kodlarini qaytarishingiz mumkin.

```java

@GetMapping("/example")
public ResponseEntity<String> example() {
    return new ResponseEntity<>("Hello, World!", HttpStatus.OK);
}
```

`Yuqoridagi misolda`:

- Body: "Hello, World!"
- Status: `200 OK`

2. Headerlarni Qoʻshish

Agar siz javobga maxsus **headerlarni** qoʻshmoqchi boʻlsangiz, `ResponseEntity` bilan buni osongina qilishingiz mumkin.

```java

@GetMapping("/custom-header")
public ResponseEntity<String> customHeader() {
    HttpHeaders headers = new HttpHeaders();
    headers.add("Custom-Header", "CustomHeaderValue");
    return new ResponseEntity<>("Response with custom header", headers, HttpStatus.OK);
}
```

Bu kodda:

- Custom-Header nomli header va uning qiymati javobga qoʻshiladi.

3. Moslashuvchanlik

`ResponseEntity` `respons body` va `status` kodni boshqarish bo'yicha yuqori darajadagi moslashuvchanlikni ta'minlaydi.
Masalan, quyidagi kodda ma'lumot mavjud yoki mavjud emasligini tekshirib, mos `status` kodni qaytarishingiz mumkin:

```java

@GetMapping("/user/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    Optional<User> user = userService.findById(id);
    if (user.isPresent()) {
        return new ResponseEntity<>(user.get(), HttpStatus.OK);
    } else {
        return new ResponseEntity<>(null, HttpStatus.NOT_FOUND);
    }
}
```

Yuqorida:

- Agar foydalanuvchi topilsa, `200 OK` va foydalanuvchi ma'lumoti qaytariladi.
- Aks holda, `404 Not Found` `status` kodi bilan bo'sh body qaytariladi.

4. Exception Handling:
   `ResponseEntity` maxsus xatoliklar uchun ham foydali. Masalan, `@ExceptionHandler` bilan birgalikda ishlatish mumkin:

```java

@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
    return new ResponseEntity<>(ex.getMessage(), HttpStatus.NOT_FOUND);
}
```

Bu yerda xatolik yuz berganda 404 Not Found status kodi va xatolik xabari qaytariladi.

5. `JSON` yoki `XML` Javoblar:

`ResponseEntity` `JSON` yoki `XML` formatdagi ma'lumotlarni qaytarish uchun ham ishlatiladi. `Spring` avtomatik ravishda
`body` ni mos formatga seriallashtiradi:

```java

@GetMapping("/user")
public ResponseEntity<User> getUser() {
    User user = new User("John", "Doe");
    return new ResponseEntity<>(user, HttpStatus.OK);
}
```

Yuqoridagi kodda:

- `User` obyekti `JSON` yoki `XML` formatda qaytariladi (klient so‘roviga qarab).

### Qachon `ResponseEntity` Kerak Bo'ladi?

- `HTTP status` kodlarini boshqarish kerak bo'lganda.
- `Header` ma'lumotlarini qo'shish yoki o'zgartirish zarur bo'lganda.
- `Body` ma'lumotlarini dinamik boshqarish kerak bo'lganda.
- Xatoliklarni boshqarish va mos `status` kodlarini qaytarish talab qilinganda.

## 2. ResponseEntity

> `ResponseEntity` bu class. U `Spring Framework` tomonidan taqdim etilgan va `org.springframework.http` paketida
> joylashgan class bo‘lib, `HTTP` so‘rovlariga javobni (`response`) qaytarishda ishlatiladi.

Lekin, siz `ResponseEntity` ni foydalanganda, odatda, uning `obyekti` yaratiladi. Ushbu obyekt orqali `HTTP status`
kodi, **headerlar** va `respons` ma'lumotlarini qaytarasiz.

`ResponseEntity` obyekti sifatida

`ResponseEntity` klassidan `obyekt` yaratib, **responsni** boshqarishingiz mumkin.

### Obyekt yaratish usullari

- Konstruktordan foydalanish:
    ```java
     ResponseEntity<String> response = new ResponseEntity<>("Hello, World!", HttpStatus.OK);
    ```
  Bu yerda:

    - `Hello, World!` — respons body.
    - `HttpStatus.OK` — status kodi (200 OK).

- Statik metodlardan foydalanish (`ok()`, `notFound()` va boshqalar):

   ```java
     ResponseEntity<String> response = ResponseEntity.ok("Hello, World!");
   ```
  Bu usul orqali qisqaroq va o‘qilishi qulayroq kod yozish mumkin.

- `Misol`: `ResponseEntity` klass va obyekt sifatida:

  **Klass sifatida ishlatilishi:**

  `ResponseEntity Spring Framework` tomonidan `HTTP` javoblarni boshqarish uchun ishlatiladigan klass ekanini
  ko‘rsatadi.

  ```java
  public ResponseEntity<String> getResponse() {
      // ResponseEntity klassidan obyekt yaratish
      ResponseEntity<String> response = new ResponseEntity<>("Success", HttpStatus.OK);
      return response;
  }
  ```         

  **Obyekt sifatida ishlatilishi:**

  `ResponseEntity` ning obyekti orqali `HTTP` javobni qaytarishingiz mumkin.

  ```java
  @GetMapping("/example")
  public ResponseEntity<String> example() {
  return ResponseEntity.ok("This is a response");
  }
  ```
  Bu yerda:
    - `ResponseEntity.ok()` — statik metod bo‘lib, yangi `ResponseEntity` obyektini yaratadi.
    - Javob tanasi (body) – `This is a response`.
    - Status kodi – `200 OK`.

- `ResponseEntity` bilan ishlash usullari
  **Statik metodlar bilan obyekt yaratish:**

| Statik metod         | 	Tavsifi                                           | 	Misol                                              |
|----------------------|----------------------------------------------------|-----------------------------------------------------|
| `ok()`               | 00 OK status kodi bilan javob qaytaradi.           | `ResponseEntity.ok("Success")`                      |
| `notFound()`         | 404 Not Found status kodi bilan javob qaytaradi.   | `ResponseEntity.notFound().build()`                 |
| `badRequest()`       | 400 Bad Request status kodi bilan javob qaytaradi. | `ResponseEntity.badRequest().body("Invalid data")`  |
| `status(HttpStatus)` | Istalgan status kodi bilan javob qaytaradi.        | `ResponseEntity.status(HttpStatus.CREATED).build()` |


**Responsni moslash**:

**Headerlar** qo‘shish, `body` va **statusni** sozlash uchun ishlatiladi:

```java
@GetMapping("/custom-response")
public ResponseEntity<String> customResponse() {
    HttpHeaders headers = new HttpHeaders();
    headers.add("Custom-Header", "HeaderValue");

    return ResponseEntity
            .status(HttpStatus.CREATED)
            .headers(headers)
            .body("Resource created successfully");
}
```

**Natija**:
- Status: `201 Created`
- Header: `Custom-Header: HeaderValue`
- Body: `Resource created successfully`

`Xulosa`:

- `ResponseEntity` klassdir, chunki u `Spring Framework` da `HTTP` javoblarini boshqarish uchun mo‘ljallangan `Java` klassi.
`Obyekt` sifatida ishlatiladi, chunki `HTTP` javobni qaytarishda `ResponseEntity` obyektini yaratib, unda `status` kodi, **headerlar** va `body` ma'lumotlarini belgilaysiz.
Bu klass `RESTful` `API` lar yaratishda ko‘p ishlatiladi va `HTTP` javoblarni moslashtirish uchun juda qulay vosita hisoblanadi.



















































