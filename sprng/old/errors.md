## 1. Ushbu code da qanday hatolik bor:

---

```java
package com.clean.code.spring_boot.service;

import com.clean.code.spring_boot.repository.EmployeeRepository;

public class EmployeeService {
    private final EmployeeRepository employeeRepository;
}

```
1. **Initsializatsiya** yo‘qligi: `employeeRepository` o'zgaruvchisi e'lon qilingan, lekin unga qiymat berilmagan. Agar siz `employeeRepository` ni ishlatmoqchi bo'lsangiz, uni **initsializatsiya** qilishingiz kerak.
2. `Constructor` yo‘qligi: Agar siz EmployeeRepository ob'ektini to'g'ridan-to'g'ri initsializatsiya qilmasangiz, **konstruktor** qo'shishingiz kerak.

```java
package com.clean.code.spring_boot.service;

import com.clean.code.spring_boot.repository.EmployeeRepository;

public class EmployeeService {
    private final EmployeeRepository employeeRepository;

    // Konstruktor orqali initsializatsiya
    public EmployeeService(EmployeeRepository employeeRepository) {
        this.employeeRepository = employeeRepository;
    }
}
```

**Initsializatsiya** — bu dasturda o'zgaruvchini biror qiymat yoki ob'ekt bilan belgilash jarayoni.

### Nima uchun initsializatsiya kerak?

1. **O'zgaruvchi qiymatini belgilash:** O'zgaruvchi e'lon qilinganda, unga qiymat berilmasa, dasturda xatolik yuzaga kelishi mumkin. Masalan, Java da bo'sh qiymatga ega o'zgaruvchini chaqirish xatolikka olib keladi.
2. **Ob'ektlar bilan ishlash:** Agar siz ob'ektni yaratishingiz va uni biror o'zgaruvchiga taqdim etishingiz kerak bo'lsa, initsializatsiya qilish zarur. Bu ob'ektning metodlarini chaqirishga imkon beradi

`Misol`:

```java
public class Example {
    private int number; // E'lon qilingan, ammo initsializatsiya qilinmagan

    public void printNumber() {
        System.out.println(number); // Bu yerda "number" 0 ga teng bo'ladi
    }
}
```

### Konstruktor orqali initsializatsiya

Agar siz ob'ektni konstruktor orqali uzatmoqchi bo'lsangiz, quyidagicha ko'rinadi:

```java
public class EmployeeService {
    private final EmployeeRepository employeeRepository;

    // Konstruktor orqali initsializatsiya
    public EmployeeService(EmployeeRepository employeeRepository) {
        this.employeeRepository = employeeRepository; // Bu yerda initsializatsiya qilinmoqda
    }
}
```
Bu yerda `EmployeeRepository` obyekti `konstruktor` orqali `employeeRepository` o'zgaruvchisiga beriladi, va shu bilan u initsializatsiya qilinadi. Endi siz employeeRepository orqali metodlarni chaqirishingiz mumkin.




































































































































































































