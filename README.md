# Лабораторная работа 1

## Подписка. (вариант 15)
Имеются подписчики (ФИО, адрес) и издания (индекс, название, месячная стоимость). Подписчик может подписаться на несколько изданий, причем на каждое на определенное число месяцев.

**Выходные документы:**
- Выдать список подписчиков указанного издания, упорядоченный по фамилиям.
- Выдать список подписчиков и их изданий с подсчетом суммы подписки каждого подписчика, упорядоченный по фамилиям.

**Er-Диаграмма**

![](Er-Диаграмма.jpg)

# Лабораторная работа 2

**Логическая модель**

*Подписчик:*
  - Подписчик_id (Первичный ключ - PK)
  - Фамилия, Имя, Отчество
  - Дата рождения, Телефон
  - Адрес_id (Внешний ключ - FK к сущности "Адрес")
    
*Адрес:*
  - Адрес_id (PK)
  - Индекс, Город, Улица, Дом, Квартира
    
*Издание:*
  - Издание_id (PK)
  - Название
  - Категория
  - Месячная_стоимость
  - Год_основания
    
*Подписка (связующая таблица):*
  - Подписка_id (PK)
  - Подписчик_id (FK)
  - Издание_id (FK)
  - Количество_месяцев
  - Дата_оформления
  - Период_доставки

*Взаимосвязи*
  1. "Подписчик" и "Адрес": Связь один-ко-многим (один адрес может быть связан с несколькими подписчиками).
  2. "Подписчик" и "Подписка": Связь один-ко-многим (один подписчик может иметь много записей о подписке).
  3. "Издание" и "Подписка": Связь один-ко-многим (одно издание может фигурировать во многих записях о подписке).

**Физическая модель**
```sql
CREATE TABLE Addresses (
    address_id INT PRIMARY KEY,
    postcode VARCHAR(10),
    city VARCHAR(100),
    street VARCHAR(100),
    house VARCHAR(20),
    apartment VARCHAR(20)
);

CREATE TABLE Subscribers (
    subscriber_id SERIAL PRIMARY KEY,
    surname VARCHAR(100) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    patronymic VARCHAR(100),
    birthday_date DATE,
    phone VARCHAR(20),
    address_id INT,
    FOREIGN KEY (address_id) REFERENCES Addresses(address_id)
);

CREATE TABLE Editions (
    edition_id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    category VARCHAR(100),
    month_cost DECIMAL(10, 2) NOT NULL,
    foundation INT
);

CREATE TABLE Subs (
    sub_id INT PRIMARY KEY,
    subscriber_id INT NOT NULL,
    edition_id INT NOT NULL,
    month_numbers INT NOT NULL,
    date_register DATE NOT NULL,
    delivery_period VARCHAR(50),
    FOREIGN KEY (subscriber_id) REFERENCES Subscribers(subscriber_id),
    FOREIGN KEY (edition_id) FERENCES Editions(edition_id)
);
```
## Делаем запрос на создание таблиц

![](Data_base/Make_table1.png)
![](Data_base/Make_table2.png)

## Заполняем таблицы данными

**Таблица Addresses:**
![](Data_base/change_Addresses.png)
![](Data_base/table_Addresses.png)

**Таблица Subscribers:**
![](Data_base/change_Subscribers.png)
![](Data_base/table_Subscribers.png)

**Таблица Editions:**
![](Data_base/change_Editions.png)
![](Data_base/table_Editions.png)

**Таблица Subs:**
![](Data_base/change_Subs.png)
![](Data_base/table_Subs.png)

## Выполнение содержательных SELECT-запросов

**1 запрос:**

Выдать список подписчиков указанного издания, упорядоченный по фамилиям.

![](Data_base/Join_1.png)


**2 запрос:**

Выдать список подписчиков и их изданий с подсчетом суммы подписки каждого подписчика, упорядоченный по фамилиям.

![](Data_base/Join_2.1.png)
![](Data_base/Join_2.png)
