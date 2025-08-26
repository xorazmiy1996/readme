## 1. Biror bir `server (mikroservis)` ni `Config Server` bilan ulash uchun  `bootstrap.yml` da qanday sintaksis kerak:

---

### Asosiy minimal konfiguratsiya

```yaml
spring:
  application:
    name: your-service-name  # Microservisingizning unikal nomi
  cloud:
    config:
      uri: http://config-server-host:8888  # Config Server manzili
      fail-fast: true  # Config Server topilmasa xato berish
      profile: dev     # Ishlatiladigan profil (dev, test, prod)
```

### To'liq tavsiya etilgan konfiguratsiya

```yaml
spring:
  application:
    name: your-service-name  # Config repodagi fayl nomiga mos kelishi kerak
  profiles:
    active: @profile@  # Maven/Gradle orqali override qilish mumkin
  cloud:
    config:
      uri: ${CONFIG_SERVER_URI:http://localhost:8888}  # Environment variable orqali sozlash
      enabled: true
      fail-fast: true
      profile: ${SPRING_PROFILE_ACTIVE:dev}  # Environment orqali profilni o'zgartirish
      label: ${CONFIG_LABEL:main}  # Git branch nomi
      username: ${CONFIG_SERVER_USERNAME:config-user}  # Basic auth
      password: ${CONFIG_SERVER_PASSWORD:config-pass}
      retry:
        initial-interval: 1000
        max-interval: 3000
        multiplier: 1.5
        max-attempts: 5
```

## 2.. Ushbu code sintaksisni tushuntirib ber:

---

```yaml
spring:
  application:
    name: user-service
  cloud:
    config:
      uri: http://localhost:8888  # Config server manzili
      fail-fast: true  # Config server topilmasa xato berish
      profile: dev     # Ishlatiladigan profil (dev, prod, test)
      retry: # Qayta Urinish Mexanizmi
        initial-interval: 1000     # 1 soniya 
        max-interval: 2000     # Maksimal 2 soniya
        multiplier: 1.1       # Har bir urinishdan keyin interval 1.1 baravar oshadi
        max-attempts: 20      # Maksimal 20 marta urinish
```

### To'liq Ishlash Ketma-Ketligi

1. Dastur ishga tushadi
2. Config Serverga ulanishga harakat qiladi
3. Agar ulanish muvaffaqiyatsiz bo'lsa:
    - `fail-fast: true` bo'lsa - xato bilan to'xtaydi
    - `fail-fast: false` bo'lsa - qayta urinish mexanizmi ishga tushadi
4. Config Serverdan quyidagi fayllarni yuklaydi:
    - `user-service.yml` (asosiy konfiguratsiya)
    - `user-service-dev.yml` (profilga xos konfiguratsiya)

### Yaxshilash Takliflari

1. Environment Variables bilan ishlash:
    ```yaml
    profile: ${SPRING_PROFILE:dev}
    uri: ${CONFIG_SERVER_URI:http://localhost:8888}
    ```

2. Git Branch belgilash:

    ```yaml
    label: ${CONFIG_LABEL:main}
    ```

3. Xavfsizlik sozlamalari:

    ```yaml
    username: ${CONFIG_SERVER_USERNAME}
    password: ${CONFIG_SERVER_PASSWORD}
    ```
4. Qo'shimcha fayllar:

    ```yaml
    name: common-config, ${spring.application.name}
    ```