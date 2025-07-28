## 1. Telefon maskasi:

---

```javascript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
    name: 'phoneNumber',
    standalone: true
})
export class PhoneNumberPipe implements PipeTransform {

    transform(value: string, type?: string): string {
    if (!value) return '';
    if(type == 'full') {
    const formattedPhoneNumber = value;
    return formattedPhoneNumber.replace(/(\d{3})(\d{2})(\d{3})(\d{2})(\d{2})/, '$1 $2 $3 $4 $5');
} else if (type == 'hide'){
    return value.substring(0, 3) + ' ** *** ** ' + value.substring(10);
}else{
    const formattedPhoneNumber = value.substring(3);
    return formattedPhoneNumber.replace(/(\d{2})(\d{3})(\d{2})(\d{2})/, '$1 $2 $3 $4');
}

}
}
```
1. To'liq formatda:

    ```html
    {{ '+998901234567' | phoneNumber:'full' }}
    <!-- Natija: 998 90 123 45 67 -->
    ```
2. Oddiy formatda:

    ```html
    {{ '+998901234567' | phoneNumber }}
    <!-- Natija: 90 123 45 67 -->
    ```
3. Bo'sh qiymat bilan:

    ```html
    {{ '' | phoneNumber }}
    <!-- Natija: (bo'sh string) -->
    ```

4. Yashirin qiymatda:

   ```html
   {{ '+998901234567' | phoneNumber:'hide' }}
    <!-- Natija: 998 90 *** ** 67 -->
   ```


























