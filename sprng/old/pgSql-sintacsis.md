## 1. `DO $$ ... END $$` nima uchun kerak?

---

`DO $$ ... END $$` bloklari `PostgreSQL` ma'lumotlar bazasida `PL/pgSQL` tilida yozilgan kodni bajarish uchun
ishlatiladi.

1. `DO`
    - `DO`: Bu so'z yangi PL/pgSQL kod blokini boshlashni bildiradi. Bu blok ichida bir nechta buyruqlarni yozish
      mumkin.

2. `BEGIN`
    - `BEGIN`: Bu so'z kod blokining boshlanishini belgilaydi. `BEGIN` va `END` o'rtasida yozilgan barcha buyruqlar
      bajariladi

3. Kod Bloki
    - `BEGIN` va `END` o'rtasida siz `PL/pgSQL`tilida yozilgan har qanday kodni joylashtirishingiz mumkin. Bu kod odatda
      ma'lumotlar bazasidagi manipulyatsiyalar **(masalan, ma'lumot qo'shish, yangilash, o'chirish)** uchun ishlatiladi.

4. `END`
    - `END`: Bu so'z kod blokining tugashini bildiradi. `BEGIN` dan keyin yozilgan kodning oxirida turadi.

`Misol`:

```sql
DO $$
BEGIN
    -- Bu yerda kod yoziladi
    RAISE NOTICE 'Hello, World!';
END $$;

```

- **Yozilgan Buyruqlar**: `BEGIN` va `END` o'rtasida yozilgan barcha buyruqlar ketma-ket bajariladi.
- **Sikl va Shartli Buyruqlar**: `DO` bloklari ichida sikl (masalan, `FOR` yoki `WHILE`) va shartli buyruqlar (masalan,
  `IF`) ishlatishingiz mumkin.
- Bir nechta Buyruqlar: Bu blok yordamida bir nechta `SQL` so'rovlarini birga bajarishingiz mumkin.

## 2. `PostgreSQL` da `FOR` siklini ishlatish.

---

> `PostgreSQL`-da `FOR` siklining uchta asosiy turi mavjud:

1. Raqamli sikl (`numeric FOR loop`)
2. Qatorlarga asoslangan sikl (`record FOR loop`)
3. So‘rov natijalari bilan sikl (`query FOR loop`)


1. Raqamli Sikl (`Numeric FOR Loop`)
   Bu turdagi siklda boshlang‘ich va oxirgi qiymat beriladi, va sikl shu oraliqda ishlaydi.

Sintaksis:

```sql
FOR counter_variable IN [REVERSE] start_value..end_value LOOP
    -- Siklda bajariladigan amallar
END LOOP;
```

`Misol`:

```sql
DO $$
BEGIN 

	FOR i IN REVERSE 5..1 LOOP
		RAISE NOTICE 'SON: %',i;
	END LOOP;


END $$
```

`PostgreSQL` dagi `%` belgisi `RAISE NOTICE` operatorida o‘rinbosar (placeholder) sifatida ishlatiladi. Bu belgi ma'lum
bir qiymatni matn ichiga joylashtirish uchun ishlatiladi. Bu usul formatlash deb ataladi va boshqa dasturlash
tillaridagi `printf` yoki `format` funksiyasiga o‘xshash ishlaydi.

`%` belgisi quydagi vazifalarni bajaradi:

- `RAISE NOTICE` qatorida, `%` belgilari matn ichiga qiymatlarni joylashtirish uchun ishlatiladi.
- Matnning ichida `%` bo‘lgan joyga keyin berilgan qiymatlar ketma-ket (virgullar orqali) joylashtiriladi.

### Misolni tushuntirish:

```plpsql
RAISE NOTICE 'Son: %', i;
```

1. `'Son: %':` Bu yerda `%` belgisi o‘rinbosar sifatida ishlaydi.
2. `i`: Bu `%` o‘rniga qo‘yiladigan qiymat. Ya'ni, `i` o‘zgaruvchisining qiymati bu yerga kiritiladi.
3. Natijada, agar ining qiymati `5` bo‘lsa, quyidagi natija ekranga chiqariladi:

```sql
NOTICE:  Son: 5
```

Bir nechta `%` ishlatish:

Agar bir nechta o‘rinbosar `(%)` ishlatilsa, ular navbat bilan qiymatlarni qabul qiladi.

```plpgsql
RAISE NOTICE 'Xodim ID: %, Ismi: %', emp.id, emp.name;
```

### Agar `%` ishlatilmasa nima bo‘ladi?

Agar `%` belgisi bo‘lmasa, o‘zgaruvchilarni matnga qo‘shib bo‘lmaydi. Masalan:

```java
RAISE NOTICE 'Son: ',i; --XATO
```

To‘liq misol:
Quyidagi misolda bir nechta qiymatlarni formatlab ekranga chiqaramiz:

```sql
DO $$
DECLARE
    emp_id INTEGER := 101;
    emp_name TEXT := 'Aliyev';
    emp_salary NUMERIC := 2500.50;
BEGIN
    RAISE NOTICE 'Xodim ID: %, Ismi: %, Ish haqi: %', emp_id, emp_name, emp_salary;
END $$;
```

Natija:

```sql
NOTICE:  Xodim ID: 101, Ismi: Aliyev, Ish haqi: 2500.50
```

2. Qatorlarga Asoslangan Sikl (`Record FOR Loop`)

Bu turdagi sikl biror so‘rov (`query`) natijasida qaytgan har bir qator ustida ishlaydi.

`Sintaksis`:

```sql
FOR record_variable IN QUERY LOOP
    -- Siklda bajariladigan amallar
END LOOP;
```

`Misol`:

```sql
DO $$
DECLARE
    row RECORD;
BEGIN
    FOR row IN SELECT id, name FROM employees LOOP
        RAISE NOTICE 'Xodim ID: %, Ismi: %', row.id, row.name;
    END LOOP;
END $$;
```

`Izoh`:

- `row`: So‘rov natijasida qaytgan har bir qator uchun ishlatiladigan o‘zgaruvchi.
- `SELECT id, name FROM employees`: Bu so‘rov natijasidagi har bir qator siklda qayta ishlanadi.

3. So‘rov Natijalari Bilan Sikl (`Query FOR Loop`)

Bir nechta ustun (`column`) qaytaradigan so‘rovlar ustida ishlash uchun `alias` ishlatiladi.

`Sintaksis`:

```sql
FOR variable_alias IN SELECT ... LOOP
    -- Siklda bajariladigan amallar
END LOOP;
```

`Misol`:

```sql
DO $$
BEGIN
    FOR emp IN SELECT id, name FROM employees LOOP
        RAISE NOTICE 'Xodim ID: %, Ismi: %', emp.id, emp.name;
    END LOOP;
END $$;
```

`Izoh`:

- `emp`: Alias sifatida ishlatiladigan o‘zgaruvchi.
- `SELECT id, name FROM employees`: Sikl ushbu so‘rovni qayta ishlaydi.

### Qo‘shimcha Xususiyatlar:

1. `EXIT` va `CONTINUE` ishlatish
    - `EXIT`: Siklni to‘liq to‘xtatadi.
    - `CONTINUE`: Joriy iteratsiyani o‘tkazib yuboradi va navbatdagisiga o‘tadi.

`Misol`:

```plpgsql
DO $$
BEGIN
    FOR i IN 1..10 LOOP
        IF i = 5 THEN
            EXIT; -- Siklni to‘xtatadi
        ELSIF i = 3 THEN
            CONTINUE; -- 3 ni o‘tkazib yuboradi va keyingisiga o‘tadi
        END IF;
        RAISE NOTICE 'Son: %', i;
    END LOOP;
END $$;
```

4. `FOREACH` Sikli:

Massiv (`array`) ustida **iteratsiya** qilish uchun `FOREACH` ishlatiladi.

`Misol`:

```sql
DO $$
DECLARE
    num_array INTEGER[] := ARRAY[10, 20, 30, 40];
    val INTEGER;
BEGIN
    FOREACH val IN ARRAY num_array LOOP
        RAISE NOTICE 'Qiymat: %', val;
    END LOOP;
END $$;
```

`Xulosa`:

- **Raqamli Sikl**: Ma’lum bir oraliqda ishlash uchun.
- **Qator Sikli**: So‘rovdan qaytgan natijalarni qayta ishlash uchun.
- **FOREACH Sikli**: Massivlar ustida ishlash uchun.

## 3.`PostgreSQL`-da `IF` operatorining  sintaksisi.

---

PostgreSQL-da `IF` operatorining asosiy sintaksisi quyidagicha:

```sql
IF shart THEN
    -- Shart bajarilganda ishlatiladigan kod
END IF;
```

Agar qo‘shimcha shartlar yoki boshqa bloklar kerak bo‘lsa, quyidagicha kengaytirilgan sintaksis ishlatiladi:

```sql
IF shart THEN
    -- Shart bajarilganda ishlatiladigan kod
ELSIF boshqa_shart THEN
    -- Boshqa shart bajarilganda ishlatiladigan kod
ELSE
    -- Hech qaysi shart bajarilmasa ishlatiladigan kod
END IF;
```

### Oddiy Misol

Quyida biror sonning musbat yoki manfiy ekanligini tekshirish uchun `IF` ishlatilgan:

```sql
DO $$
DECLARE
    son INTEGER := -5;
BEGIN
    IF son > 0 THEN
        RAISE NOTICE 'Son musbat: %', son;
    ELSE
        RAISE NOTICE 'Son manfiy yoki nol: %', son;
    END IF;
END $$;
```

### Boshlang‘ich qiymat bermaslik

> Agar boshlang‘ich qiymat berilmasa, o‘zgaruvchi `NULL` qiymatiga ega bo‘ladi.

## 4. `DECLARE` `operator`  nima uchun ishlatilinadi?

---

`DECLARE` yordamida biror o‘zgaruvchi yaratib, unga ma'lum bir tur va boshlang‘ich qiymat beriladi.

`Misol`:

```sql
DO $$
DECLARE
    son INTEGER := 5; -- son o‘zgaruvchisi e'lon qilinadi va qiymati 5 ga o‘rnatiladi
BEGIN
    IF son > 0 THEN
        RAISE NOTICE 'Son musbat: %', son; -- `son` o‘zgaruvchisi ishlatilmoqda
    END IF;
END $$;
```

### Misolning ishlashi:

1. **`DECLARE` qismi**: Bu yerda son o‘zgaruvchisi `INTEGER` turida e'lon qilinadi va unga boshlang‘ich qiymat sifatida
   `5` beriladi.
2. **`BEGIN` qismi**: `DECLARE` orqali e'lon qilingan o‘zgaruvchi faqat `BEGIN ... END` blok ichida ishlaydi.
3. **`IF` sharti**: son qiymati shartda tekshiriladi (`son > 0`).

## 5. `pgSQL`  da o'zgaruvchilar turi:

---

> `DECLARE` orqali o‘zgaruvchining turini aniq ko‘rsatish kerak. Ko‘p ishlatiladigan turlar:

- `INTEGER` – butun sonlar uchun.
- `NUMERIC` – o‘nlik sonlar uchun.
- `TEXT` – matn saqlash uchun.
- `BOOLEAN` – mantiqiy qiymatlar (`TRUE` yoki `FALSE`) uchun.
- `DATE` – sana qiymatlari uchun.
- `RECORD` – so‘rov natijalari uchun.

`Misol`:

```sql
DO $$
DECLARE
    xodim_id INTEGER := 101; -- Butun son
    xodim_ismi TEXT := 'Aliyev'; -- Matn
    xodim_maoshi NUMERIC := 2500.50; -- O‘nlik son
BEGIN
    RAISE NOTICE 'Xodim ID: %, Ismi: %, Maoshi: %', xodim_id, xodim_ismi, xodim_maoshi;
END $$;
```

## 6.`RECORD` haqida ko'roq ma'lumot.

---

> `PostgreSQL` da `RECORD` maxsus o‘zgaruvchi turi bo‘lib, bir qator (`row`) ma'lumotni saqlash uchun ishlatiladi. Bu
> tur ko‘pincha dinamik so‘rov natijalarini saqlashda yoki `FOR loop` ichida har bir qatorni qayta ishlashda
> ishlatiladi.
`RECORD` ma'lumotlar tuzilmasini oldindan aniq bilmasangiz yoki har xil ustun tuzilishiga ega bo‘lgan so‘rovlar bilan
> ishlashda qulay hisoblanadi.

## 7. `PostgreSQL` da eng ko'p ishlatiladigan operatorlar:

- `SELECT` - ma'lumotlarni olish
- `WHERE` - filtrlash
- `JOIN` - jadvallarni birlashtirish
- `GROUP BY` - guruhlash
- `ORDER BY` - saralash
- `LIMIT/OFFSET` - sahifalash
- `INSERT` - qo'shish
- `UPDATE` - yangilash
- `DELETE` - o'chirish
- Aggregatsiya funktsiyalari
- `WITH` (CTE) - murakkab so'rovlar uchun
- `INDEX` yaratish (tezlik uchun)
- `EXPLAIN` - so'rov rejasini tahlil qilish

### `SELECT` turlari va misollar

---

1. Oddiy `SELECT` (barcha ustunlar)
    - `SELECT * FROM products;`

2. Aniq ustunlarni tanlash
    - `SELECT id, name, price FROM products;`

3. Ustunlarga taxallus (alias) berish

   ```sql
   SELECT 
       id AS product_id,
       name AS product_name,
       price AS product_price
   FROM products;
   ```

4. `DISTINCT` - takrorlanishlarni olib tashlash
    - `SELECT DISTINCT category FROM products;`


5. `WHERE` sharti bilan
    - `SELECT * FROM employees WHERE salary > 5000;`

6. `ORDER BY` bilan saralash
    - `SELECT * FROM products ORDER BY price DESC, name ASC;`

7. `GROUP BY` bilan guruhlash
   ```sql
   SELECT 
       department_id,
       COUNT(*) AS employee_count,
       AVG(salary) AS avg_salary
   FROM employees
   GROUP BY department_id;
   ```

8. `HAVING` bilan guruhlangan ma'lumotlarni filtrlash

   ```sql
   SELECT 
       category,
       AVG(price) AS avg_price
   FROM products
   GROUP BY category
   HAVING AVG(price) > 100;
   ```

9. `LIMIT/OFFSET` bilan sahifalash:

   ```sql
   SELECT * FROM orders ORDER BY order_date DESC LIMIT 10 OFFSET 20;
   ```

10. `JOIN` bilan jadvallarni birlashtirish
   ```sql
   SELECT 
       o.order_id,
       c.customer_name,
       p.product_name
   FROM orders o
   JOIN customers c ON o.customer_id = c.id
   JOIN products p ON o.product_id = p.id;
   ```
11. `Subquery` (ichki so'rov) bilan

```sql
SELECT name, price
FROM products
WHERE price > (SELECT AVG(price) FROM products);
```

12. `WITH` (`Common Table Expression`) bilan

```sql
WITH expensive_products AS (
    SELECT * FROM products WHERE price > 1000
)
SELECT * FROM expensive_products ORDER BY price DESC;
```

13. `CASE WHEN` shartli ifoda bilan

```sql
SELECT 
    name,
    price,
    CASE 
        WHEN price > 1000 THEN 'Premium'
        WHEN price > 500 THEN 'Standard'
        ELSE 'Budget'
    END AS price_category
FROM products;
```

14. `JSON` ma'lumotlar bilan ishlash

   ```sql
   SELECT 
       id,
       json_data->>'name' AS product_name,
       json_data->'details'->>'color' AS color
   FROM products_with_json;
   ```

### `WHERE` - filtrlash

1. Oddiy filtr
   ```sql
   SELECT * FROM talabalar 
   WHERE yoshi > 20;
   ```
2. Bir nechta shartlar
   ```sql
   SELECT ism, familiya FROM xodimlar
   WHERE bo'lim = 'IT' AND maosh > 5000;
   ```
3. `IN` operatori
   ```sql
   SELECT * FROM mahsulotlar
   WHERE kategoriya IN ('Elektronika', 'Maishiy texnika');
   ```
4. `LIKE` operatori
   ```sql
   SELECT * FROM mijozlar
   WHERE telefon LIKE '+99890%';  -- 90 kodli mobil raqamlar
   ```
5. Tarixlar bilan ishlash
   ```sql
   SELECT * FROM buyurtmalar
   WHERE buyurtma_sana BETWEEN '2023-01-01' AND '2023-01-31';
   ```

### `WHERE` va `GROUP BY` birgalikda

---

   ```sql
   SELECT bo'lim, AVG(maosh)::numeric(10,2)
   FROM xodimlar
   WHERE ishga_kirgan_sana > '2020-01-01'
   GROUP BY bo'lim;
   ```

Bu misolda avval 2020 yildan keyin ishga kirgan xodimlar filtrlanadi, keyin ular bo'limlar bo'yicha guruhlanadi.

`WHERE` va `HAVING` farqi:

- `WHERE` guruhlashdan oldin `individual` yozuvlarni filtrlaydi
- `HAVING` guruhlashdan keyin butun guruhlarni filtrlaydi

   ```sql
   SELECT 
       bo'lim, 
       COUNT(*) AS xodimlar_soni
   FROM xodimlar
   WHERE maosh > 3000  -- Avval 3000 dan yuqori maoshli xodimlarni tanlaydi
   GROUP BY bo'lim
   HAVING COUNT(*) > 5;  -- Keyin 5 dan ortiq xodimi bor bo'limlarni tanlaydi
   ```

### `JOIN` - jadvallarni birlashtirish

---

1. JOIN nima?

   > `JOIN` - bu bir nechta jadvallardagi ma'lumotlarni mantiqiy ravishda birlashtirish imkonini beruvchi `SQL` *
   *operatori**. Buni haqiqiy hayotdagi misollar bilan tushuntiramiz.

2. Asosiy jadvallar misoli:

   Talabalar jadvali (`students`):

   | student_id | 	student_name | 	faculty_id |
                                                                                                            |------------|---------------|-------------|
   | 1          | 	Ali	101      |
   | 2          | 	Vali	        | 102         |
   | 3          | 	Gani	        | 101         |
   | 4          | 	Lola	        | 103         |

   Fakultetlar jadvali (`faculties`):

   | faculty_id | 	faculty_name | 	dean     |
                                                                                                         |------------|---------------|-----------|
   | 101        | 	Informatika	 | Prof. X   |
   | 102	       | Matematika	 | Prof. Y   |
   | 103        | 	Fizika       | 	Prof. Z  |
   | 104        | 	Kimyo        | 	Prof. W  |


3. `JOIN` turlari va misollar

- `INNER JOIN` (Ichki birlashtirish)

   ```sql
   SELECT s.student_name, f.faculty_name
   FROM students s
   INNER JOIN faculties f ON s.faculty_id = f.faculty_id;
   ```

  `Natija`:

  | student_name | 	faculty_name     |
                                                                      |--------------|-------------------|
  | Ali          | 	Informatika      |
  | Vali         | 	Matematika       |
  | Gani         | 	Informatika      |
  | Lola         | 	Fizika           |

  > `Tushuntirish`: Faqat ikkala jadvalda ham mos keluvchi yozuvlar chiqadi (Kimyo fakulteti talabasi yo'q, shuning
  uchun ko'rinmaydi).


- `LEFT JOIN` (Chap birlashtirish)

    ```sql
    SELECT s.student_name, f.faculty_name
    FROM students s
    LEFT JOIN faculties f ON s.faculty_id = f.faculty_id;
    ```
  > `Natija`: Yuqoridagi bilan bir xil (chunki barcha talabalar fakultetga biriktirilgan).

- `RIGHT JOIN` (O'ng birlashtirish)

    ```sql
    SELECT s.student_name, f.faculty_name
    FROM students s
    RIGHT JOIN faculties f ON s.faculty_id = f.faculty_id;
    ```

  `Natija`:

  | student_name | 	faculty_name |
                                                                  |--------------|---------------|
  | Ali          | 	Informatika  |
  | Vali         | 	Matematika   |
  | Gani         | 	Informatika  |
  | Lola         | 	Fizika       |
  | NULL         | 	Kimyo       |
  > `Tushuntirish`: Kimyo fakulteti ko'rinadi (unga talaba biriktirilmagan).

- `FULL JOIN` (To'liq birlashtirish)

    ```sql
    SELECT s.student_name, f.faculty_name
    FROM students s
    FULL JOIN faculties f ON s.faculty_id = f.faculty_id;
    ```
  > Natija: Yuqoridagi natijalar birlashmasi (hamma talaba va hamma fakultet).

> `PostgreSQL` da agar siz faqat `JOIN` kalit so'zini ishlatsangiz (hech qanday turini ko'rsatmasdan), bu avtomatik
> ravishda `INNER JOIN` deb hisoblanadi.

```shell
   Oddiy JOIN = INNER JOIN
```

4. Ko'p jadvalli JOIN misoli

Keling, universitet tizimi uchun 4 ta bog'langan jadval orqali ko'p jadvalli `JOIN` ni tushuntiramiz:

- a) Talabalar jadvali (`students`)

  | student_id	 | student_name   | 	birth_date | 	gender |
                                                              |-------------|----------------|-------------|---------|
  | 1	          | Ali Valiyev	   | 2000-05-15	 | Erkak   |
  | 2	          | Lola Karimova	 | 2001-02-20  | 	Ayol   |
  | 3	          | Temur Usmonov	 | 1999-11-03  | 	Erkak  |
  | 4	          | Nilufar Yunus	 | 2000-07-28  | 	Ayol   |

- b) Fakultetlar jadvali (`faculties`)

  | faculty_id | 	faculty_name         | 	dean                 | 	building |
                                                            |------------|-----------------------|-----------------------|-----------|
  | 10         | 	Dasturiy injiniring	 | Prof. Xudoyberdiyev	3 |
  | 20	        | Iqtisodiyot           | 	Prof. Qodirova	1     |
  | 30         | 	Tibbiyot	            | Prof. Aliyev	5        |

- c) Yo'nalishlar jadvali (specialties)

  | spec_id	 | spec_name	            | faculty_id | 	duration |
                                                          |----------|-----------------------|------------|-----------|
  | 101	     | Dastur muhandisligi	  | 10         | 	4        |
  | 102	     | Ma'lumot xavfsizligi	 | 10         | 	4        |
  | 201      | 	Bank ishi	           | 20	        | 3         |
  | 202	     | Buxgalteriya	         | 20         | 	3        |


- d) Talaba-yo'nalish bog'lovchi jadval (student_specialties)

  | id	 | student_id	 | spec_id	 | enrollment_date | 	grant_amount |
                                                      |-----|-------------|----------|-----------------|---------------|
  | 1	  | 1	          | 101      | 	2020-09-01     | 	800000       |
  | 2	  | 2	          | 201      | 	2021-09-01	    | 750000        |
  | 3   | 	3          | 	101     | 	2020-09-01	    | 900000        |
  | 4   | 	4          | 	102	    | 2021-09-01	     | 850000        |

### Jadvalni birlashtirish so'rovi\

---

```sql
SELECT 
    s.student_name,
    s.gender,
    f.faculty_name,
    sp.spec_name,
    sp.duration AS years,
    ss.enrollment_date,
    ss.grant_amount
FROM 
    students s
JOIN 
    student_specialties ss ON s.student_id = ss.student_id
JOIN 
    specialties sp ON ss.spec_id = sp.spec_id
JOIN 
    faculties f ON sp.faculty_id = f.faculty_id
ORDER BY 
    f.faculty_name, s.student_name;
```

### So'rov natijasi

| student_name  | 	gender | 	faculty_name        | 	spec_name            | years | 	enrollment_date | 	grant_amount |
|---------------|---------|----------------------|-----------------------|-------|------------------|---------------|
| Ali Valiyev   | 	Erkak  | 	Dasturiy injiniring | 	Dastur muhandisligi  | 	4    | 	2020-09-01	     | 800000        |
| Temur Usmonov | 	Erkak  | 	Dasturiy injiniring | 	Dastur muhandisligi  | 	4    | 	2020-09-01      | 	900000       |
| Nilufar Yunus | 	Ayol   | 	Dasturiy injiniring | 	Ma'lumot xavfsizligi | 	4    | 	2021-09-01	     | 850000        
| Lola Karimova | 	Ayol   | 	Iqtisodiyot         | 	Bank ishi            | 	3    | 	2021-09-01      | 	750000       |

`Tushuntirish`:

1. Bog'lanish nuqtalari:
    - Talabalar `↔` Talaba-yo'nalish: `student_id`
    - Talaba-yo'nalish `↔` Yo'nalishlar: `spec_id`
    - Yo'nalishlar `↔` Fakultetlar: `faculty_id`

2. `JOIN` qismlari:
    ```sql
    FROM students s
    JOIN student_specialties ss ON s.student_id = ss.student_id
    JOIN specialties sp ON ss.spec_id = sp.spec_id
    JOIN faculties f ON sp.faculty_id = f.faculty_id
    ```

- Har bir `JOIN` yangi jadvalni bog'laydi
- `ON` sharti bog'lanish ustunlarini ko'rsatadi

3. **`SELECT` qismi**:
    - Har bir jadvaldan kerakli ustunlar tanlanadi
    - `s.`, `ss.`, `sp.`, `f.` prefikslari qaysi jadvaldan olinayotganini ko'rsatadi


4. **ORDER BY:**
    - Natijalar avval fakultet, keyin talaba ismi bo'yicha tartiblanadi


5. Bog'lanish diagrammasi

   ![](media-md/join-diagramma.png)

   `Tushuntirish`:
    1. `students` jadvali:
        - Asosiy kalit (primary key): `student_id`
        - Bog'lanish: `student_specialties.student_id` orqali

    2. `student_specialties` bog'lovchi jadval:
        - Asosiy kalit: id
        - Chet kalitlar (foreign keys):
            - `student_id` → `students.student_id`
            - `spec_id` → `specialties.spec_id`
    3. `specialties` jadvali:
        - Asosiy kalit: `spec_id`
        - Chet kalit: `faculty_id` `→` `faculties.faculty_id`
    4. `faculties` jadvali:
        - Asosiy kalit: `faculty_id`

`SQL JOIN` bog'lanishlari:

```sql
FROM students s
JOIN student_specialties ss ON s.student_id = ss.student_id  /* 1-bog'lanish */
JOIN specialties sp ON ss.spec_id = sp.spec_id               /* 2-bog'lanish */
JOIN faculties f ON sp.faculty_id = f.faculty_id             /* 3-bog'lanish */
```

### `GROUP BY` - guruhlash

`Misollar`:

1. Asosiy `Employees` Jadvali

   | id | 	name  | 	department | 	position    | 	salary |
                                                                      |----|--------|-------------|--------------|---------|
   | 1  | 	Ali   | 	IT         | 	Developer   | 	5000   |
   | 2  | 	Vali  | 	IT         | 	Team Lead   | 	6000   |
   | 3  | 	Gani  | 	HR         | 	Manager     | 	4500   |
   | 4  | 	Lola  | 	HR         | 	Recruiter   | 	4700   |
   | 5  | 	Timur | 	Sales      | 	Manager     | 	4000   |
   | 6  | 	Dina  | 	Sales      | 	Salesperson | 	4200   |
   | 7  | 	Erik  | 	Sales      | 	Salesperson | 	4100   |

2. `Misol 1`: Bo'limlar bo'yicha xodimlar soni

   `So'rov`:

    ```sql
    SELECT department, COUNT(*) AS employee_count
    FROM employees
    GROUP BY department;
    ```

   `Natija Jadvali`:

   | department | 	employee_count |
                                                                   |------------|-----------------|
   | IT	        | 2               |
   | HR         | 	2              |
   | Sales      | 	3              |

3. `Misol 2`: Bo'lim va lavozim bo'yicha xodimlar soni

   `So'rov`:
    ```sql
    SELECT department, position, COUNT(*) AS count
    FROM employees
    GROUP BY department, position;
    ```
   `Natija Jadvali`:

   | department | 	position	    | count |
                                                                |------------|---------------|-------|
   | IT         | 	Developer    | 	1    |
   | IT         | 	Team Lead    | 	1    |
   | HR	        | Manager       | 	1    |
   | HR	        | Recruiter	    | 1     |
   | Sales      | 	Manager      | 	1    |
   | Sales      | 	Salesperson	 | 2     |


4. `Misol 3`: Bo'limlar bo'yicha maosh statistikasi

   `So'rov`:
    ```sql
    SELECT 
        department,
        COUNT(*) AS employee_count,
        AVG(salary)::numeric(10,2) AS avg_salary,
        MAX(salary) AS max_salary,
        MIN(salary) AS min_salary
    FROM employees
    GROUP BY department;
    ```

   `Natija Jadvali`:

   | department | 	employee_count | 	avg_salary   | 	max_salary	 | min_salary |
                                                             |------------|-----------------|---------------|--------------|------------|
   | IT	        | 2               | 5500.00       | 	6000        | 	5000        |
   | HR	| 2               | 	4600.00        | 	4700	        | 4500         |
   | Sales      | 	3              | 	4100.00| 	4200        | 	4000        |


5. `Misol 4`: Orders Jadvali

   | order_id | 	product_id	 | customer_id | 	order_date  | 	amount |
                                                       |----------|------------|-------------|--------------|---------|
   | 1        | 	1         | 	101        | 	2023-01-15  | 	100    |
   | 2        | 	1         | 	102	       | 2023-01-16   | 	150    |
   | 3        | 	2         | 	101        | 	2023-01-16	 | 200     |
   | 4        | 1	 |               101	    |           2023-02-01 |	120|
   | 5        | 2	  |               103     |	2023-02-05 | 	180       |


6. `Misol 5`: Oylik mahsulot savdolari

   `So'rov`:

    ```sql
    SELECT 
        TO_CHAR(order_date, 'YYYY-MM') AS month,
        product_id,
        SUM(amount) AS total_sales,
        COUNT(*) AS order_count
    FROM orders
    GROUP BY TO_CHAR(order_date, 'YYYY-MM'), product_id
    ORDER BY month, product_id;
    ```

   `Natija Jadvali`:

   | month   | 	product_id | 	total_sales | 	order_count |
                                                 |---------|-------------|--------------|--------------|
   | 2023-01 | 	1          | 	250         | 	2           |
   | 2023-01 | 	2          | 	200         | 	1           |
   | 2023-02 | 	1          | 	120         | 	1           |
   | 2023-02 | 	2          | 	180         | 	1           |

7. `Misol 6`: `HAVING` bilan filtrlash

   `So'rov`:

    ```sql
    SELECT 
        department,
        AVG(salary)::numeric(10,2) AS avg_salary
    FROM employees
    GROUP BY department
    HAVING AVG(salary) > 5000;
    ```
   `Natija Jadvali`:

   | department | 	avg_salary |
                                              |------------|-------------|
   | IT	        | 5500.00     |

### `ORDER BY` - saralash

---

> `ORDER BY` - bu `SQL` so'rovlarida natijalarni ma'lum bir tartibda saralash uchun ishlatiladigan kalit so'z.

Asosiy Sintaksis

```sql
SELECT ustun1, ustun2, ...
FROM jadval_nomi
[WHERE shart]
[GROUP BY ustunlar]
[HAVING shart]
ORDER BY saralash_ustuni [ASC|DESC];
```

### `ORDER BY` qo'llanilishi

1. Oddiy saralash (o'sish tartibida)

    ```sql
    SELECT * FROM mahsulotlar ORDER BY narx;
    ```
   Yoki aniq ko'rsatish uchun:

    ```sql
    SELECT * FROM mahsulotlar ORDER BY narx ASC;  -- ASC (ascending) - o'sish tartibi
    ```

   `Misolda`:

   |id|	nomi| 	narx |
                                             |--|--|-------|
   | 3|	Qalam| 	5    |
   |1|	Daftar| 	10   |
   |2|	Ruchka| 	15   |

2. Teskari tartibda saralash
    ```sql
    SELECT * FROM mahsulotlar ORDER BY narx DESC;  -- DESC (descending) - kamayish tartibi
    ```
   `Natija`:

   | id | 	nomi	   | narx |
                                        |----|----------|------|
   | 2  | 	Ruchka  | 	15  |
   | 1  | 	Daftar	 | 10   |
   | 3  | 	Qalam   | 	5   |

3. Bir nechta ustunlar bo'yicha saralash

    ```sql
    SELECT * FROM talabalar ORDER BY familiya ASC, ism ASC;
    ```
   Bu birinchi familiya bo'yicha, keyin bir xil familiyali talabalar uchun ism bo'yicha saralaydi.


4. Ifodalar bo'yicha saralash

    ```sql
    SELECT *, (narx * miqdor) AS jami 
    FROM buyurtmalar 
    ORDER BY (narx * miqdor) DESC;
    ```
5. Ustun raqami bo'yicha saralash

    ```sql
    SELECT nomi, narx, miqdor FROM mahsulotlar ORDER BY 2;  -- 2-ustun (narx) bo'yicha saralash
    ```

### Maxsus saralash usullari

1. `NULL` qiymatlar bilan ishlash

    ```sql
    SELECT * FROM xodimlar ORDER BY maosh NULLS FIRST;  -- NULL qiymatlar birinchi
    SELECT * FROM xodimlar ORDER BY maosh NULLS LAST;   -- NULL qiymatlar oxirida
    ```
2. Belgilangan tartibda saralash (CASE WHEN)

    ```sql
    SELECT * FROM mahsulotlar
    ORDER BY 
      CASE kategoriya
        WHEN 'Elektronika' THEN 1
        WHEN 'Kiyim' THEN 2
        WHEN 'Oziq-ovqat' THEN 3
        ELSE 4
      END;
    ```

Amaliy Misollar

1. Talabalar ro'yxatini baho bo'yicha saralash:

    ```sql
    SELECT ism, familiya, baho 
    FROM talabalar 
    WHERE guruh = 'DIF-101'
    ORDER BY baho DESC;
    ```

2. Mahsulotlarni narx va sotuvlar bo'yicha saralash:

    ```sql
    SELECT p.nomi, p.narx, COUNT(s.id) AS sotuvlar_soni
    FROM mahsulotlar p
    LEFT JOIN sotuvlar s ON p.id = s.mahsulot_id
    GROUP BY p.id
    ORDER BY p.narx ASC, sotuvlar_soni DESC;
    ```
3. Xodimlarni ish staji bo'yicha saralash:

    ```sql
    SELECT ism, lavozim, ishga_kirgan_sana
    FROM xodimlar
    ORDER BY ishga_kirgan_sana ASC;  -- Eng ko'p stajli xodimlar oxirida
    ```

### `PERFORMANCE` Maslahatlari

1. Saralash uchun indekslardan foydalaning
2. Ko'p ma'lumotlar bo'lsa LIMIT bilan ishlating
3. Faqat kerakli ustunlarni tanlang
4. Murakkab saralashlarda subquery ishlating

> `ORDER BY` - bu ma'lumotlarni tahlil qilish va hisobotlar tayyorlashda juda muhim vosita bo'lib, natijalarni kerakli
> tartibda ko'rish imkonini beradi.

### `LIMIT/OFFSET` - sahifalash

> `LIMIT` va `OFFSET` - bu natijalar to'plamidan ma'lum miqdordagi yozuvlarni olish uchun ishlatiladigan `SQL`
> operatorlari.

### `LIMIT` - Natijalar sonini cheklash

> `LIMIT` operatori qaytariladigan qatorlar sonini cheklaydi.

`Sintaksis`:

```sql
SELECT ustun1, ustun2, ...
FROM jadval_nomi
LIMIT son;
```

`Misol`:

```sql
SELECT * FROM mahsulotlar LIMIT 5;
```

Bu so'rov mahsulotlar jadvalidan faqat birinchi 5 ta yozuvni qaytaradi.

### `OFFSET` - Natijalardan sakrash

`OFFSET` operatori berilgan miqdordagi qatorlarni o'tkazib, keyingilarini qaytaradi.

`Sintaksis`:

```sql
SELECT ustun1, ustun2, ...
FROM jadval_nomi
LIMIT son OFFSET sakrash_soni;
```

`Misol`:

```sql
SELECT * FROM mahsulotlar LIMIT 5 OFFSET 10;
```

Bu so'rov mahsulotlar jadvalidan birinchi 10 ta yozuvni o'tkazib, keyingi 5 tasini qaytaradi.

### `LIMIT` va `OFFSET` birgalikda ishlatilishi

Bu ikkala operator ko'pincha birga ishlatiladi, ayniqsa sahifalash (`pagination`) uchun.

### `Misol` (3-sahifani olish):

```sql
SELECT * FROM talabalar
ORDER BY familiya
LIMIT 10 OFFSET 20;  -- Har bir sahifada 10 talaba, 3-sahifa (21-30)
```

### `PERFORMANCE` Maslahatlari

1. Katta `OFFSET` qiymatlari sekin ishlashi mumkin
2. `OFFSET` o'rniga `WHERE` bilan filtrlash yaxshiroq:

    ```sql
    SELECT * FROM jadval WHERE id > 1000 LIMIT 100;
    ```
3. Ko'p ma'lumotlar bilan ishlaganda `indeks` lardan foydalaning
4. `EXPLAIN ANALYZE` yordamida so'rov tezligini tekshiring

### `INSERT` - ma'lumot qo'shish

> `INSERT` - bu PostgreSQLda jadvalga yangi yozuv(lar) qo'shish uchun ishlatiladigan SQL buyrug'i.

1. Asosiy `INSERT` sintaksisi

    ```sql
    INSERT INTO jadval_nomi (ustun1, ustun2, ...) 
    VALUES (qiymat1, qiymat2, ...);
    ```

   `Misol`:

    ```sql
    INSERT INTO talabalar (id, ism, familiya, yosh) 
    VALUES (1, 'Ali', 'Valiyev', 20);
    ```
2. Ko'p qator qo'shish

    ```sql
    INSERT INTO talabalar (ism, familiya, yosh) 
    VALUES 
      ('Gani', 'Tursunov', 21),
      ('Lola', 'Qodirova', 19),
      ('Timur', 'Usmonov', 22);
    ```
3. Ba'zi ustunlarni qo'shish

   > Agar ba'zi ustunlarga qiymat bermasangiz, ular `NULL` yoki default qiymatni oladi:

4. `DEFAULT` qiymatlardan foydalanish

    ```sql
    INSERT INTO mahsulotlar (nomi, narx) 
    VALUES ('Planshet', DEFAULT);  -- narx ustuni uchun default qiymat ishlatiladi
    ```
5. `SELECT` natijasini qo'shish

   Boshqa jadvaldan ma'lumotlarni tanlab, qo'shish mumkin:

    ```sql
    INSERT INTO bitiruvchilar (ism, familiya, yosh)
    SELECT ism, familiya, yosh 
    FROM talabalar 
    WHERE bitiruv = true;
    ```

### `PERFORMANCE` Maslahatlari

1. Ko'p ma'lumot qo'shishda bir nechta `VALUES` dan foydalaning
2. `1000+` qator qo'shishda `COPY` buyrug'i yaxshiroq
3. Indekslar `INSERT` ni sekinlashtirishi mumkin
4. Katta jadvallarda bir vaqtning o'zida juda ko'p qator qo'shmang

> `INSERT` - bu ma'lumotlar bazasiga yangi ma'lumotlar kiritishning asosiy usuli bo'lib, turli vaziyatlar uchun ko'p
> imkoniyatlar taqdim etadi.

### `UPDATE` - yangilash

> `UPDATE` - bu `PostgreSQL` da mavjud jadval yozuvlarini o'zgartirish uchun ishlatiladigan `SQL` buyrug'i.

Asosiy `UPDATE` sintaksisi

```sql
UPDATE jadval_nomi
SET ustun1 = yangi_qiymat1, 
    ustun2 = yangi_qiymat2, ...
[WHERE shart];
```

1. Oddiy yangilash misoli

   **Talabalar jadvali**:

   | id | ism           | familiya | yosh  | guruh |
                                  |----|---------------|----------|-------|-------|
   | 1  | Ali | Valiyev  | 20       | DIF-1 |
   | 2  | Lola | Karimova | 19       | DIF-2 |
   | 3  | Temur | Usmonov  | 21       | DIF-1 |

    ```sql
    UPDATE talabalar
    SET yosh = 22
    WHERE id = 3;
    ```
   `Natija`:
   Temurning yoshi 21 dan 22 ga o'zgaradi.

2. Bir nechta ustunlarni yangilash

    ```sql
    UPDATE talabalar
    SET yosh = 23, guruh = 'DIF-3'
    WHERE id = 1;
    ```

3. `WHERE` shartisiz yangilash

    ```sql
    UPDATE mahsulotlar
    SET narx = narx * 1.1;  -- Barcha mahsulotlar narxini 10% ga oshiradi
    ```
   > Ehtiyot bo'ling! `WHERE` shartisiz `UPDATE` barcha yozuvlarni yangilaydi.

4. Boshqa jadval asosida yangilash

   Ishchilar jadvali:

   | id	 | ism  | 	lavozim   | 	maosh |
                            |-----|------|------------|--------|
   | 1	  | Ali  | 	Dasturchi | 	500   |
   | 2	  | Vali | 	Tester    | 	450   |

   Lavozim_narxlari jadvali:

   | lavozim   | 	min_maosh |
                         |-----------|------------|
   | Dasturchi | 	600       |
   | Tester	   | 500        |

    ```sql
    UPDATE ishchilar
    SET maosh = lavozim_narxlari.min_maosh
    FROM lavozim_narxlari
    WHERE ishchilar.lavozim = lavozim_narxlari.lavozim
    AND ishchilar.maosh < lavozim_narxlari.min_maosh;
    ```

5. `CASE WHEN` bilan shartli yangilash

    ```sql
    UPDATE mahsulotlar
    SET narx = CASE 
                  WHEN narx < 100 THEN narx * 1.15  -- Arzon mahsulotlar 15% qimmatlashadi
                  WHEN narx >= 100 THEN narx * 1.10 -- Qimmat mahsulotlar 10% qimmatlashadi
               END;
    ```

6. Yangilash cheklovlari

    1. **NOT NULL cheklovi**: Ustun NULL qiymatga yangilanmaydi
    2. **Unique cheklov**: Takroriy qiymatga yangilanmaydi
    3. **Foreign key cheklovlari**: Bog'liq jadvallardagi qiymatlarga mos kelishi kerak

7. `PERFORMANCE` maslahatlari

    - Katta jadvallarda `WHERE` shartini indekslangan ustunlar bilan ishlating
    - Bir vaqtning o'zida juda ko'p yozuv yangilamaslik maqsadga muvofiq
    - Yangilashdan oldin `EXPLAIN ANALYZE` bilan rejani tekshiring
    - Faqat kerakli ustunlarni yangilang

### `DELETE` - o'chirish

`DELETE` - bu `PostgreSQL` da jadvaldan yozuv(lar)ni olib tashlash uchun ishlatiladigan `SQL` buyrug'i.

Asosiy `DELETE` sintaksisi

```sql
DELETE FROM jadval_nomi
[WHERE shart];
```

1. Oddiy o'chirish misoli

   Talabalar jadvali:

   | id | 	| ism	   | familiya    | 	yosh  | 	guruh |
                      |----|-|--------|-------------|--------|--------|
   | 1  | 	| Ali    | 	Valiyev	20 | 	DIF-1 |
   | 2  | 	| Lola	  | Karimova    | 	19    | 	DIF-2 |
   | 3  | 	| Temur	 | Usmonov	    | 21     | 	DIF-1 |

    ```sql
    DELETE FROM talabalar
    WHERE id = 3;
    ```
   `Natija`:
   Temur Usmonov jadvaldan o'chiriladi.

2. `WHERE` shartisiz o'chirish

    ```sql
    DELETE FROM vaqtinchalik_jadval;
    ```
   > DIQQAT! `WHERE` shartisiz `DELETE` barcha yozuvlarni o'chiradi. Oldindan yaxshi o'ylab ko'ring.

3. Bir nechta shartlar bilan o'chirish

    ```sql
    DELETE FROM talabalar
    WHERE yosh < 20 
    AND guruh = 'DIF-2';
    ```
4. Boshqa jadval asosida o'chirish

   O'quvchilar jadvali:

   | id | 	ism | 	holat      |
                   |----|------|-------------|
   | 1	 | Ali  | 	bitirgan   |
   | 2	 | Vali | 	o'qiyotgan |

    ```sql
    DELETE FROM oquvchilar
    WHERE id IN (
      SELECT id 
      FROM bitiruvchilar 
      WHERE bitiruv_yili < 2020
    );
    ```

5. `CASCADE` o'chirish (bog'liq jadvallardan)

   Agar jadvallar orasida foreign key bog'lanish bo'lsa va CASCADE o'rnatilgan bo'lsa:
    ```sql
    -- orders jadvalidan o'chirish, order_items ham o'chadi
    DELETE FROM orders
    WHERE order_id = 12345;
    ```
6. `TRUNCATE` bilan farqi

   `DELETE` dan tezroq alternativa:

    ```sql
    TRUNCATE TABLE jadval_nomi;  -- Barcha yozuvlarni tez o'chiradi
    ```

### Xavfsizlik masalalari

1. Har doim oldindan `SELECT` bilan tekshiring:

    ```sql
    SELECT * FROM jadval WHERE shart;  -- Oldin ko'rib olish
    DELETE FROM jadval WHERE shart;    -- Keyin o'chirish
    ```
2. Ma'lumotlar bazasini oldin zaxiralang
3. Katta jadvallarda bo'lib-bo'lib o'chiring
4. Transactionlardan foydalaning:
    ```sql
    BEGIN;
    DELETE FROM ...;
    -- Agar xato bo'lsa:
    ROLLBACK;
    -- Agar hamma yaxshi bo'lsa:
    COMMIT;
    ```

### `WITH` (`CTE`) - murakkab so'rovlar uchun

> `WITH` (`Common Table Expression yoki CTE`) - bu vaqtinchalik natijalar to'plamini yaratish va keyin uni asosiy so'
> rovda ishlatish imkonini beruvchi `SQL` xususiyati.

1. Eng Oddiy Misol

   Tasavvur qiling, sizda talabalar ro'yxati bor va siz o'rtacha bahodan yuqori olgan talabalarni topmoqchisiz:

   Talabalar jadvali:

   | id	 | ism      | 	familiya	 | guruh_id |
             |-----|----------|------------|----------|
   | 1   | 	Ali	    | Valiyev    | 	101     |
   | 2	  | Lola	    | Karimova	  | 102      |
   | 3	  | Temur	   | Usmonov    | 	101     |
   | 4	  | Dilfuza	 | Rahimova   | 	102     |

   Baholar jadvali:

   | id	 | talaba_id	 | fan        | 	baho |
             |-----|------------|------------|-------|
   | 1   | 	1	        | Matematika | 	5    |
   | 2	  | 1	         | Fizika     | 	4    |
   | 3	  | 2	         | Matematika | 	3    |
   | 4	  | 3	         | Fizika     | 	5    |
   | 5	  | 4	         | Matematika | 	4    |

   Misol: Har bir guruhda o'rtacha bahoni topish

    ```sql
    WITH guruh_baholari AS (
        SELECT 
            t.guruh_id,
            AVG(b.baho) AS o_rtacha_baho
        FROM talabalar t
        JOIN baholar b ON t.id = b.talaba_id
        GROUP BY t.guruh_id
    )
    SELECT 
        g.nomi AS guruh_nomi,
        gb.o_rtacha_baho
    FROM guruh_baholari gb
    JOIN guruhlar g ON gb.guruh_id = g.id
    ORDER BY gb.o_rtacha_baho DESC;
    ```
2. Haqiqiy Hayotdan Misol: Do'kon Savdolari

   Tasavvur qiling, sizda mahsulotlar va ularning oylik savdlari jadvallari bor. Siz har oy eng ko'p sotilgan 3 ta
   mahsulotni aniqlamoqchisiz:

   Mahsulotlar jadvali:

   | id	 | nomi	      | kategoriya    | 	narx |
       |-----|------------|---------------|-------|
   | 1	  | Notebook	  | Elektronika   | 	500  |
   | 2	  | Smartphone | 	Elektronika	 | 800   |
   | 3	  | Kitob      | 	Maishiy	     | 20    |
   | 4	  | Qalam      | 	Maishiy      | 	2    |

   Sotuvlar jadvali:

   | id	 | mahsulot_id	 | sana	 | miqdor |
       |-----|--------------|-------|--------|
   | 1	1 | 	2023-01-15	 | 2     |
   | 2	1 | 	2023-01-16  | 	1    |
   | 3	2 | 	2023-01-16  | 	3    |
   | 4	3 | 	2023-01-17  | 	5    |

   Har bir kategoriyada eng ko'p sotilgan mahsulot

    ```sql
    WITH kategoriya_sotuvlari AS (
        SELECT 
            m.kategoriya,
            m.nomi AS mahsulot_nomi,
            SUM(s.miqdor) AS jami_sotuv
        FROM mahsulotlar m
        JOIN sotuvlar s ON m.id = s.mahsulot_id
        GROUP BY m.kategoriya, m.nomi
    ),
    top_mahsulotlar AS (
        SELECT 
            kategoriya,
            mahsulot_nomi,
            jami_sotuv,
            RANK() OVER (PARTITION BY kategoriya ORDER BY jami_sotuv DESC) AS reyting
        FROM kategoriya_sotuvlari
    )
    SELECT 
        kategoriya,
        mahsulot_nomi,
        jami_sotuv
    FROM top_mahsulotlar
    WHERE reyting = 1;
    ```




## 9. `PostgreSQL` da `Column` Nomlarini Yozish Usullari:

1. To'liq Nomlash (`Fully Qualified Names`) - Jadval nomi bilan

   ```sql
   SELECT talabalar.ismi, baholar.fan, baholar.baho
   FROM talabalar
   INNER JOIN baholar ON talabalar.id = baholar.talaba_id;
   ```
   `Afzalliklari`:
    - Qaysi jadvaldan ustun olinayotgani aniq ko'rinadi
    - Bir nechta jadvalda bir xil nomli ustunlar bo'lsa, muammo bo'lmaydi

2. Soddalashtirilgan Nomlash - Faqat `column` nomlari

   ```sql
   SELECT ismi, fan, baho
   FROM talabalar
   INNER JOIN baholar ON talabalar.id = baholar.talaba_id;
   ```
   `Afzalliklari`:
    - Kod qisqa va o'qish oson
    - Agar ustun nomlari bir jadvalda takrorlanmasa ishlaydi

   `Kamchiliklari`:
    - Agar ikkala jadvalda ham **ismi** kabi ustun bo'lsa, xato kelib chiqadi
    - Qaysi jadvaldan ustun olinayotgani aniq emas

3. Qachon jadval nomi kerak?

   Jadval nomini qo'yish shart bo'lgan holatlar:

    - Ikkala jadvalda bir xil nomli ustunlar bo'lsa

      ```sql
      -- Ikkala jadvalda ham `nomi` ustuni bor
      SELECT talabalar.nomi, fanlar.nomi
      FROM talabalar
      JOIN fanlar ON talabalar.fan_id = fanlar.id;
      ```
      














































































































































































































































































































































































































































































































































































































































































































































































































