## 1. `Entity` ichida  `@Table` annotatsiyasini ishlatmasangiz:

```java

@Entity // Faqat @Entity bilan
public class User {
    @Id
    private Long id;
    private String name;
}
```

- Jadval nomi **avtomatik ravishda** class nomidan olinadi (`User` → `user`)
- Bu muammoli bo'lishi mumkin, chunki `user` maxsus kalit so'z

### Maxsus kalit so'zlardan himoya qilish

```java
@Entity
@Table(name = "\"user\"") // PostgreSQL uchun
// @Table(name = "`user`")   // MySQL uchun
public class User {
    // "user" kabi maxsus so'zlardan himoya qiladi
}
```


### `@Table` parametrlari:

| Parametr          | 	Tavsif            | 	Misal                                    |
|-------------------|--------------------|-------------------------------------------|
| name              | 	Jadval nomi       | 	name = "foydalanuvchilar"                |
| schema	           | Schema nomi	       | schema = "public"                         |
| catalog           | 	Katalog nomi      | 	catalog = "mydb"                         |
| uniqueConstraints | 	Unique cheklovlar | 	@UniqueConstraint(columnNames = "email") |
| indexes           | 	Indexlar ro'yxati | 	@Index(columnList = "email")             |

`Xulosa`:

✅ Jadval nomini aniq belgilash imkoniyati

✅ Ma'lumotlar bazasi tuzilmasini boshqarish

✅ Unique cheklovlar va indexlarni qo'shish

✅ Maxsus kalit soʻzlardan himoya qilish

✅ Kodni aniqroq va o'qilishi oson qilish

