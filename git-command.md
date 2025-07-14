## 1. `local` yaratilgan loyihani `git` ga `push` qilish.

```bash
   # 1. Barcha o'zgarishlarni indeksga qo'shish
   git add .
   
   # 2. Commit yaratish
   git commit -m "Initial commit"
   
   # 3. Agar sizda 'main' branchi bo'lmasa (yoki boshqa nomda branch bo'lsa)
   git branch -M main
   
   # 4. Remote repositoryni qo'shish (mana to'g'ri usul)
   git remote add origin https://github.com/spring-boot-microservice-new/user-service.git
    
   # 5. Kodni remote repositoryga yuborish
   git push -u origin main
   
   # 6. Repository bo'sh emas degan xatolik chiqsa:
   git pull origin main --allow-unrelated-histories
   # Yoki yangi versiya uchun:
   git push --force-with-lease origin main
   
```

### Agar remote allaqachon mavjud bo'lsa:

```bash
   # Remote ni yangilash
   git remote set-url origin https://github.com/spring-boot-microservice-new/user-service.git
   
   # Keyin push qilish
   git push -u origin main
```