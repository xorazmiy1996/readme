## 1. Ushbu code ni sintaksis tushuntirib ber:

```yaml
spring:
    gateway:
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/api/users/**
        - id: product-service
          uri: lb://product-service
          predicates:
            - Path=/api/products/**
        - id: order-service
          uri: lb://order-service
          predicates:
            - Path=/api/orders/**
```

---

Bu konfiguratsiya `Spring Cloud Gateway` uchun `routing` (yo'naltirish) qoidalarini belgilaydi. Keling, har bir qismini alohida tushuntiramiz:

1. `User Service Route`

    ```yaml
    - id: user-service
      uri: lb://user-service
      predicates:
        - Path=/api/users/**
    ```

    **Tushuntirish:**

   - `id`: `Route` uchun unikal identifikator (`user-service`)
   - `uri`:
     - `lb://` - Load balancing (yuk balanslash) ishlatilishini bildiradi
     - `user-service` - `Service Discovery` (**Eureka/Nacos**) da ro'yxatdan o'tgan service nomi
   - `predicates`:
     - `Path=/api/users/**` - **"/api/users/"** bilan boshlanadigan barcha so'rovlar ushbu servicega yo'naltiriladi

### Asosiy tushunchalar:

1. `URI` formatlari:
   - `lb://SERVICE-NAME` - Service Discovery orqali (Eureka/Nacos bilan)
   - `http://host:port`- To'g'ridan-to'g'ri URL orqali
   
2. **Predicate:**
    - So'rovni qaysi servicega yo'naltirishni belgilovchi shartlar
    - **Path** predicate - URL yo'lini tekshiradi
3. **Yulduzcha belgisi `(**)`:** 

   - `/api/users/**` - **"/api/users/"** dan keyin istalgan path qo'shilishi mumkinligini bildiradi
   - Masalan: `/api/users/1`, `/api/users/profile`,`/api/users/1/orders` hammasi ushbu routega tushadi


























































































