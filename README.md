# Subd
# 1. 
     * Стешиц Дарья Николаевна;
     * 153502;
     * интернет магазин
# 2.
    Минимальные функциональные требования:
    * aвторизация пользователя.
    * Управление пользователями (CRUD).
    * Система ролей.
    * Журналирование действий пользователя.
# 3. 
1. Пользователь (Users) - представляет собой основную сущность, хранящую информацию о зарегистрированных пользователях.
     - user_id(PK): Уникальный идентификатор пользователя.
     - user_fio: Информация о имени, фамилии пользователя.
     - user_login: Уникальный логин пользователя, используется для аутентификации.
     - user_password: Хешированный пароль пользователя для безопасной аутентификации.
     - user_email: Уникальный адрес электронной почты пользователя.
     - adress_id: хранит информацию о стране и городе проживания пользователя
     - role_id(FK): Многие пользователи могут иметь множество ролей через сущность "Роль пользователя". 
     - card_id(FK): Пользователь может привязать банковскую карту для оплаты покупок онлайн. Данные о карте хранятся в сущности "Карта пользователя".
     - deliv_id(FK): Пользователь может выбрать один из пунктов доставки из "Пункты выдачи".
       
2. Роль (Roles)
     - role_id(PK): Уникальный идентификатор роли.
     - role_name: Название роли, например, "администратор", "пользователь", "Поставщик".
       
3. Категория продукта (Category) - Эта сущность хранит информацию о категории продукта для облегченного поиска покупателей
    - cat_id(PK): уникальный id категории
    - cat_name: имя категории
      
4. Продукт (Product) - хранит информацию о товарах, продаваемых в интернет магазине.
    - Поля:
        - product_id(PK): Уникальный идентификатор продукта.
        - product_name: имя продукта
        - amount - количество продукта на складе 
        - product_cost - Цена 1 шт продукта
        - discription - описание продукта
        - Категория(FK): категория продукта
          
5. OrderProduct - между таблицами продукт и заказ связь многие ко многим, которая корректно реализовывается с помощью 3 таблиц. 2 таблицы - источники данных и 1 соединительная(OrderProduct)
   - order_id (PK) - составной PK
   - product_id (PK)- составной PK
   - count - количество одинаковых продуктов в заказе (not null)
              
7. Заказ (Order) - Эта сущность хранит информацию о заказе пользователя (промежуточная стадия, включающая статус заказа(оплачен/не оплачен), дату и время оформления)
      - order_id(PK): уникальный номер заказа
      - user_id(FK) - пользователь, сделавший заказ
      - order_date - дата оформления заказа
      - order_time - время оформления заказа
      - order_payment - статус оплаты заказа(оплачен/не оплачен)
      - order_stat - статус заказа (собран / доставляется/на пункте выдачи )
      - order_cost - суммарная стоимость заказа на момент его оформления
        
6. OrderStatus - хранит информацию о статусах заказа
   - 
   - 
   -  
        
6. Карты (Cards) - Эта сущность хранит информацию о карте, привязанной к пользователю.
    - Поля:
      - ID(PK): Уникальный идентификатор карты
      - Card_num - уникальный номер карты
      - Card_date - время действия карты (до какого числа)
          
8. Покупка (Purchase) - Эта сущность хранит информацию об успешно завершенных заказах.
    - purchase_id(PK):
    - 
      
      
9. Пункт выдачи (Delivery) - хранятся данные о пунктах выдачи интернет магазина
    
    - deliv_id(PK):
    - deliv_adress:
      
11. Производитель продукта (Producer) - хранит информацию о производителе товара.
      - ID(PK): уникальный id производителя
      - prod_name: имя производителя
      - prod_discription: описание компании производителя
      - prod_contacts: контакты производителя
      - prod_id(FK): производимые товары

#4 Ограничения
1. **Table Users**:
   *user_id* INT PRIMARY KEY AUTO_INCREMENT
   *user_fio* VARCHAR(60) NOT NULL
   *user_login* VARCHAR(30) NOT NULL
   *user_password* VARCHAR(10) NOT NULL
   *user_email* VARCHAR(30) NOT NULL
   *adress_id*
   *role_id* INT FOREIGN KEY REFERENCES Roles (role_id)
   *card_id* INT FOREIGN KEY REFERENCES Cards(card_id)
   *deliv_id* INT FOREIGN KEY REFERENCES Delivery(deliv_id)
2. **Table Roles**:
   *role_id* INT PRIMARY KEY AUTO_INCREMENT
   *role_name* VARCHAR(30) NOT NULL
3.**Category**
    - *cat_id* INT PRIMARY KEY AUTO_INCREMENT
    - *cat_name* VARCHAR(30) NOT NULL
       
