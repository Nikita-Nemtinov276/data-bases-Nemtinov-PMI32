<h1 name="content" align="center"><a href="">
</a> MSSQL</h1>

<p align="center">
  <a href="#-lab1"><img alt="lab1" src="https://img.shields.io/badge/Lab1-blue"></a> 
</p>
<h3 align="center">
  <a href="#client"></a>
  Вариант 25. Отдел кадров вуза.
  
Информация о сотрудниках: ФИО, паспорт, телефон, адрес, отдел/кафедра(может работать на нескольких кафедрах), должность, % ставки по должности (м.б. несколько должностей),...
Штатное расписание: для каждого отдела/кафедры список должностей с количеством ставок и окладом, кол-во занятых ставок для каждой должности (например, для кафедры мат.анализа положено: 1 ставка заведующего с окладом 30 000р., 2 ставки доцента с окладом 25 000р., 5 ставок старшего преподавателя, 2 ставки ассистента, 1 ставка лаборанта; для кафедры общей математики – 1 ставка заведующего, 3 ставки старшего преподавателя, 0.5 ставки документоведа, ... )

Реализовать:
- Вывод следующей ниформации:
о вакансиях (кафедра, должность, кол-во свободных ставок); 
о сотрудниках, работающих более чем в одном отделе/кафедре;
о пенсионерах; 
о ветеранах (работающих в институте не менее тридцати лет); 
о сотрудниках, работающих более чем на одной ставке.

</h3>

# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab1
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
  Разработать ER-модель данной предметной области: выделить сущности, их атрибуты, связи между сущностями. 
Для каждой сущности указать ее имя, атрибут (или набор атрибутов), являющийся первичным ключом, список остальных атрибутов.
Для каждого атрибута указать его тип, ограничения, может ли он быть пустым, является ли он первичным ключом.
Для каждой связи между сущностями указать: 
- тип связи (1:1, 1:M, M:N)
- обязательность

ER-модель д.б. представлена в виде ER-диаграммы (картинка)

По имеющейся ER-модели создать реляционную модель данных и отобразить ее в виде списка сущностей с их атрибутами и типами атрибутов,  для атрибутов указать, явл. ли он первичным или внешним ключом 
</h3>

#### №1. ER-модель
![image](/SUBO/EEER.png)

#### №1.1. Реляционная модель
![image](/SUBO/RRRREL.png)

# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab2
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
  В соответствии с реляционной моделью данных, разработанной в Лаб.№1, создать реляционную БД на учебном сервере БД:
  
- создать таблицы, определить первичные ключи и иные ограничения
  
- определить связи между таблицами
  
- создать диаграмму

- заполнить все таблицы адекватной информацией (не меньше 10 записей в таблицах, наличие примеров для связей типа 1:M )

</h3>

## №2. Создание таблиц
```
CREATE TABLE IF NOT EXISTS public."Кафедра"
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "телефон" character varying(11) COLLATE pg_catalog."default" NOT NULL,
    "аудитория" character varying(4) COLLATE pg_catalog."default" NOT NULL,
    "веб_сайт" character varying(100) COLLATE pg_catalog."default",
    "название_кафедры" character varying(100) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT "Кафедра_pkey" PRIMARY KEY (id),
    CONSTRAINT "Кафедра_Название_кафедры_key" UNIQUE ("название_кафедры"),
    CONSTRAINT "Кафедра_Телефон_key" UNIQUE ("телефон")
);

CREATE TABLE IF NOT EXISTS public."Сотрудник"
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "паспорт" character varying(10) COLLATE pg_catalog."default" NOT NULL,
    "фио" character varying(100) COLLATE pg_catalog."default" NOT NULL,
    "адрес" character varying(1000) COLLATE pg_catalog."default",
    "телефон" character varying(11) COLLATE pg_catalog."default",
    "дата_рождения" date,
    CONSTRAINT "Сотрудник_pkey" PRIMARY KEY (id),
    CONSTRAINT "Сотрудник_Паспорт_key" UNIQUE ("паспорт"),
    CONSTRAINT "Сотрудник_Телефон_key" UNIQUE ("телефон")
);

CREATE TABLE IF NOT EXISTS public."Должность"
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "название" character varying(1000) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT "Должность_pkey" PRIMARY KEY (id),
    CONSTRAINT "Должность_Название_key" UNIQUE ("название")
);

CREATE TABLE IF NOT EXISTS public."Расписание"
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "аудитория" character varying(4) COLLATE pg_catalog."default" NOT NULL,
    "время" timestamp without time zone,
    "название_занятия" character varying(1000) COLLATE pg_catalog."default",
    "сотрудник_id" integer NOT NULL,
    CONSTRAINT "Расписание_pkey" PRIMARY KEY (id),
    CONSTRAINT "Расписание_id_сотрудника_fkey" FOREIGN KEY ("сотрудник_id")
        REFERENCES public."Сотрудник" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
);

CREATE TABLE IF NOT EXISTS public."Занятие"
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "дата" date,
    "занятие_проведено" integer,
    "расписание_id" integer NOT NULL,
    CONSTRAINT "Занятие_pkey" PRIMARY KEY (id),
    CONSTRAINT "Занятие_id_расписание_fkey" FOREIGN KEY ("расписание_id")
        REFERENCES public."Расписание" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
);

CREATE TABLE IF NOT EXISTS public."Сотрудник_Кафедра"
(
    "сотрудник_id" integer NOT NULL,
    "кафедра_id" integer NOT NULL,
    CONSTRAINT "Сотрудник_Кафедра_pkey" PRIMARY KEY ("сотрудник_id", "кафедра_id"),
    CONSTRAINT "Сотрудник_Кафедра_id_кафедры_fkey" FOREIGN KEY ("кафедра_id")
        REFERENCES public."Кафедра" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT "Сотрудник_Кафедра_id_сотрудника_fkey" FOREIGN KEY ("сотрудник_id")
        REFERENCES public."Сотрудник" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
);

CREATE TABLE IF NOT EXISTS public."Сотрудник_Должность"
(
    "сотрудник_id" integer NOT NULL,
    "должность_id" integer NOT NULL,
    "процентная_ставка" numeric(10,2) NOT NULL,
    CONSTRAINT "Сотрудник_Должность_pkey" PRIMARY KEY ("сотрудник_id", "должность_id"),
    CONSTRAINT "Сотрудник_Должнос_id_сотрудника_fkey" FOREIGN KEY ("сотрудник_id")
        REFERENCES public."Сотрудник" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT "Сотрудник_Должност_id_должности_fkey" FOREIGN KEY ("должность_id")
        REFERENCES public."Должность" (id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
);
```

## 2.1 Диаграмма
![image](/SUBO/ERD.png)

## 2.2 Данные в таблицах
### Данные кафедр
  
![image](/SUBO/Kafdata.png)

### Данные сотрудников

![image](/SUBO/Sotdata.png)

### Данные должностей

![image](/SUBO/Doldata.png)

### Данные расписаний

![image](/SUBO/Rasdata.png)

### Данные занятий

![image](/SUBO/Zandata.png)

### Данные сотрудник-кафедра

![image](/SUBO/SKdata.png)

### Данные сотрудник-должность

![image](/SUBO/SDdata.png)
