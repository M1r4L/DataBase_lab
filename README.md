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

# Лабораторная работа 3

## Создание представлений для выходных документов

**1: Показываем информацию о подписчиках по изданиям**

![](Data_base/View1.png)

**2: вычисляет общую сумму оплаты за все подписки для каждого подписчика**

![](Data_base/View2.png)

## Создание процедур

**1: Показывает подписчиков определенного издания**

![](Data_base/Procedure1.png)

**2: Добавляет издание, если оно уже есть, то ничего не меняет**

Таблица до выполнения процедуры:

![](Data_base/table_Editions.png)

Вызов процедуры:

![](Data_base/Procedure2.png)

Таблица после вызова процедуры:

![](Data_base/Table_after_p2.png)

При повторном вызове процедуры выдает сообщение о том что это издание уже имеется

![](Data_base/p2_after_p2.png)

## Оптимизация запросов с использованием View

![](Data_base/optimize_v1.png)

![](Data_base/optimize_v2.png)

# Лабораторная работа 4

## Создание генераторов

**генератор для Addresses:**
![](Data_base/Gen_Addr1.png)

**Вызов генератора:**
![](Data_base/CGen_Addr1.png)

**генератор для Editions:**
![](Data_base/Gen_Edit1.png)

**Вызов генератора:**
![](Data_base/CGen_Edit1.png)

**генератор для Subscribers:**
![](Data_base/Gen_Subr1.png)

**Вызов генератора:**
![](Data_base/CGen_Subr1.png)

**генератор для Subs:**
![](Data_base/Gen_Subs1.png)

**Вызов генератора:**
![](Data_base/CGen_Subs1.png)

**После вызова генераторов все таблицы стали заполнены**

## Анализ планов выполнения запросов (EXPLAIN ANALYZE)

**Сделаем анализ для самого первого запроса с join, но выводим данные, по определенному периоду**

![](Data_base/Analyze.png)

Как видно на выполнения этого запроса уходит примерно 11 мс.
Попробуем оптимизировать нашу БД чезер индексы

**Создаем индекс**

![](Data_base/Creat_idx.png)

**Делаем анализ того же запроса после создания индекса**

![](Data_base/Analyze_after_Optimize.png)

Как мы можем увидеть теперь анализ занимает меньше времени, а конкретно 5 мс, вместо 11 мс.

# Лабораторная работа 5

## Создание триггера

**Создаем триггер удаляющий информацию о подписке**

![](Data_base/Creat_Trigger.png)

**Создаем триггер на таблиц Editions**

![](Data_base/Creat_Trigger2.png)

**Теперь проверим его работу**
Выведем информацию из таблицы Subs с конкертным editions_id, после чего удалим информацию
о нем из таблицы Editions

![](Data_base/Table_Trigger1.png)

![](Data_base/Table_Trigger2.png)

![](Data_base/Table_Trigger3.png)

Как мы видим если попытаться вновь вывести информацию из таблицы Subs, то никаких данных нет

## Создание таблица-журнал для отслеживания изменений в таблице Subscribers

![](Data_base/Magazine.png)

## Создание триггеров аудита изменений (INSERT, UPDATE, DELETE)
**INSERT**

![](Data_base/Insert.png)

![](Data_base/Insert2.png)

Как видно в журнале появилась информация об этой операции

**UPDATE**

![](Data_base/Update.png)

![](Data_base/Update2.png)

Информация так же отображается в журнале

**DELETE**

![](Data_base/Delete.png)

Вот информация которую мы будем удалать, она была получена из работы двух предыдущих триггеров insert и update

![](Data_base/Table_change.png)

Запускаем триггер

![](Data_base/Delete2.png)

Информация так же отображается в журнале

И для проверки можно вывести таблицу Subscribers.
Теперь в ней снова нет строки которую мы создали

![](Data_base/Table_change_del.png)

