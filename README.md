# Subd
# 1. 
     * Стешиц Дарья Николаевна;
     * 153502;
     * интернет магазин
# 2. Минимальные функциональные требования:
    * aвторизация пользователя.
    * Управление пользователями (CRUD).
    * Система ролей.
    * Журналирование действий пользователя.
# 3. Основные Сущности
1. ***Пользователь (Users)*** - представляет собой основную сущность, хранящую информацию о зарегистрированных пользователях.
    - user_id(PK): Уникальный идентификатор пользователя.
    - user_fio: Информация о имени, фамилии пользователя.
    - user_login: Уникальный логин пользователя, используется для аутентификации.
    - user_password: Хешированный пароль пользователя для безопасной аутентификации.
    - user_email: Уникальный адрес электронной почты пользователя.
    - adress_id: хранит информацию о стране и городе проживания пользователя
    - role_id(FK): Многие пользователи могут иметь множество ролей через сущность "Роль пользователя". 
    - card_id(FK): Пользователь может привязать банковскую карту для оплаты покупок онлайн. 
    
       
2. ***Роль (Roles)***
    - role_id(PK): Уникальный идентификатор роли.
    - role_name: Название роли, например, "администратор", "пользователь", "Поставщик".
       
3. ***Категория продукта (Category)*** - Эта сущность хранит информацию о категории продукта для облегченного поиска покупателей
    - cat_id(PK): уникальный id категории
    - cat_name: имя категории
      
4. ***Склад(Stock)*** - хранит информацию о товарах и их количестве, хранящихся на складе
   - stock_id(PK) - уникальный идентификатор склада
   - product_id(FK) - идентификатор продукта
   - product_count - количество товара
      
5. ***Продукт (Product)*** - хранит информацию о товарах, продаваемых в интернет магазине.
    - product_id(PK): Уникальный идентификатор продукта.
    - product_name: имя продукта
    - amount - количество продукта на складе 
    - product_cost - Цена 1 шт продукта
    - discription - описание продукта
    - category_id(FK): категория продукта
          
6. ***OrderProduct*** - между таблицами продукт и заказ связь многие ко многим, которая корректно реализовывается с помощью 3 таблиц. 2 таблицы - источники данных и 1 соединительная(OrderProduct)
    - order_id (PK) - составной PK
    - product_id (PK)- составной PK
    - count - количество одинаковых продуктов в заказе (not null)
              
7. ***Заказ (Order)*** - Эта сущность хранит информацию о заказе пользователя (промежуточная стадия, включающая статус заказа(оплачен/не оплачен), дату и время оформления)
    - order_id(PK): уникальный номер заказа
    - user_id(FK) - пользователь, сделавший заказ
    - order_datetime - дата и время оформления заказа
    - order_payment - статус оплаты заказа(оплачен/не оплачен)
    - order_cost - суммарная стоимость заказа на момент его оформления
    - deliv_id(FK) - идентификатор доставки заказа
    - point_id(FK) - пользователь может выбрать один из пунктов доставки из "Пункты выдачи".
        
8. ***Карты (Cards)*** - Эта сущность хранит информацию о карте, привязанной к пользователю.
    - card_id(PK): Уникальный идентификатор карты
    - card_num - уникальный номер карты
    - card_date - время действия карты (до какого числа)
      
9. ***Пункт выдачи (PickUp_point)*** - хранит информацию о пункте выдачи
    - point_id(PK): уникальный идентификатор пункта выдачи
    - point_adress: адрес пункта выдачи
    - point_worktime: время работы пункта выдачи

10. ***Доставка товара (Delivery)*** - хранятся данные о процессе доставки товара 
    - deliv_id(PK): Уникальный идентификатор
    - deliv_status: Статус заказа (оформлен/собран/в пути/доставлен)
      
11. ***Производитель продукта (Producer)*** - хранит информацию о производителе товара.
    - producer_id(PK): уникальный id производителя
    - prod_name: имя производителя
    - prod_discription: описание компании производителя
    - prod_contacts: контакты производителя
    - product_id(FK): производимые товары

# 4 Ограничения
1. **Table Users**:
   - *user_id* INT PRIMARY KEY AUTO_INCREMENT
   - *user_fio* VARCHAR(60) NOT NULL
   - *user_login* VARCHAR(30) NOT NULL UNIQUE
   - *user_password* VARCHAR(10) NOT NULL UNIQUE
   - *user_email* VARCHAR(30) NOT NULL UNIQUE
   - *role_id* INT FOREIGN KEY REFERENCES Roles (role_id)
   - *card_id* INT FOREIGN KEY REFERENCES Cards(card_id)
     
2. **Table Roles**:
   - *role_id* INT PRIMARY KEY AUTO_INCREMENT
   - *role_name* VARCHAR(30) NOT NULL
     
3. **Table Category**:
    - *cat_id* INT PRIMARY KEY AUTO_INCREMENT
    - *cat_name* VARCHAR(30) NOT NULL
      
4. **Stock**:
   - *stock_id* INT PRIMARY KEY AUTO_INCREMENT
   - *product_id* INT FOREIGN KEY Product(product_id)
   - *product_count* INT 
      
5. **Table Product**:
   - *product_id* INT PRIMARY KEY AUTO_INCREMENT
   - *product_name* VARCHAR(30) NOT NULL
   - *amount* INT  
   - *product_cost* DOUBLE NOT NULL
   - *discription* TEXT NOT NULL
   - *category_id* INT FOREIGN KEY Category (cat_id)
     
6. **Table OrderProduct**:
   - *order_id* INT AUTO_INCREMENT
   - *product_id* INT AUTO_INCREMENT
   - *count* INT NOT NULL
     - PRIMARY KEY(order_id,product_id)
     
7. **Table Order**:
   - *order_id* INT PRIMARY KEY AUTO_INCREMENT
   - *user_id* INT FOREIGN KEY Users (user_id)
   - *order_datetime* DATETAME 
   - *order_payment* BOOLEAN
   - *order_cost* DOUBLE NOT NULL
   - *point_id* INT FOREIGN KEY PickUp_point(point_id)
   - *deliv_id* INT FOREIGN KEY Delivery(deliv_id)
     
8. **Table Cards**:
   - *card_id* INT PRIMARY KEY AUTO_INCREMENT
   - *card_num* VARCHAR(16) NOT NULL UNIQUE
   - *card_date* DATE
     
9. **Table Delivery**:
   - *deliv_id* INT PRIMARY KEY AUTO_INCREMENT
   - *deliv_status* VARCHAR(40)
  
10. **PickUp_point**:
    - *point_id* INT PRIMARY KEY AUTO_INCREMENT
    - *point_adress* VARCHAR(50)
    - *point_worktime* TIME
     
11. **Table Producer**:
   - *producer_id* INT PRIMARY KEY AUTO_INCREMENT
   - *prod_name* VARCHAR(30)
   - *prod_discription* TEXT
   - *prod_contacts* VARCHAR(30)
   - *product_id* INT FOREIGN KEY Product(product_id)
   
       
