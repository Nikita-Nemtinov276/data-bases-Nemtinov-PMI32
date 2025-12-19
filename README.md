<h1 name="content" align="center"><a href=""> </a> MSSQL</h1>
<p align="center"> 
  <a href="#-lab1"><img alt="lab1" src="https://img.shields.io/badge/Lab1-blue"></a>
  <a href="#-lab2"><img alt="lab2" src="https://img.shields.io/badge/Lab2-blue"></a>
  <a href="#-lab3"><img alt="lab3" src="https://img.shields.io/badge/Lab3-blue"></a>
  <a href="#-lab4"><img alt="lab4" src="https://img.shields.io/badge/Lab4-blue"></a>
  <a href="#-lab6"><img alt="lab6" src="https://img.shields.io/badge/Lab6-blue"></a>
  <a href="#-lab7"><img alt="lab7" src="https://img.shields.io/badge/Lab7-blue"></a>
</p>
<h3 align="center"> <a href="#client"></a> 
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

<h4>№1. ER-модель</h4>
![image](/SUBO/EEER.png)

 <h4>№1.1. Реляционная модель</h4>
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

# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab3
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
</h3>

<div>
  <h4>Часть 1</h4>
  <p><b>Цель:</b> изучить конструкции языка SQL для манипулирования данными в СУБД MSSQL.</p>
  <p><b>Задания и краткое описание работы:</b></p>
  <ol>
    <li>Выборка из одной таблицы.
      <ol type="1">
        <li>Выбрать из произвольной таблицы данные и отсортировать их по двум произвольным имеющимся в таблице признакам (разные направления сортировки).</li>

```
select *
from Сотрудник
order by фио ASC;

select *
from Сотрудник
order by дата_рождения DESC; 
```
<li>Выбрать из произвольной таблицы те записи, которые удовлетворяют условию отбора (where). Привести 2-3 запроса.</li>

```
select *
from Сотрудник
where дата_рождения > '1990-01-01';

select *
from Занятие
where занятие_проведено = 1;
```

<li>Привести примеры 2-3 запросов с использованием агрегатных функций (count, max, sum и др.) с группировкой и без группировки.</li>

```
select count(*) as всего_занятий, min(дата) as самое_ранее, max(дата) as самое_позднее
from Занятие;

select k.название_кафедры, count(sk.сотрудник_id) as количество_сотрудников
from Кафедра k
left join Сотрудник_Кафедра sk on sk.кафедра_id = k.id
group by k.id;
```

<li>Привести примеры подведения подытога с использованием GROUP BY [ALL] [CUBE | ROLLUP] (2-3 запроса). В ROLLUP и CUBE использовать не менее 2-х столбцов.</li>

```
select k.название_кафедры, s.фио, count(*) as количество_должностей
from Сотрудник s
join Сотрудник_Кафедра sk on sk.сотрудник_id = s.id
join Кафедра k on k.id = sk.кафедра_id
join Сотрудник_Должность sd on s.id = sd.сотрудник_id
group by cube(k.название_кафедры, s.фио);

select k.название_кафедры, s.фио, count(*) as количество_должностей
from Сотрудник s
join Сотрудник_Кафедра sk on sk.сотрудник_id = s.id
join Кафедра k on k.id = sk.кафедра_id
join Сотрудник_Должность sd on s.id = sd.сотрудник_id
group by rollup(k.название_кафедры, s.фио);
```

<li>Выбрать из таблиц информацию об объектах, в названиях которых нет заданной последовательности букв (LIKE).</li>

```
select id, фио
from Сотрудник 
where фио not like '%ов';

select id, название_кафедры
from Кафедра
where название_кафедры not like '%общей%';
```
</ol>
</li>
    <li>Выборка из нескольких таблиц.
      <ol type="1">
        <li>Вывести информацию подчиненной (дочерней) таблицы, заменяя коды (значения внешних ключей) соответствующими символьными значениями из родительских таблиц. Привести 2-3 запроса с использованием классического подхода соединения таблиц (where).</li>

```
select r.id, r.аудитория, r.время, s.фио
from Сотрудник s, Расписание r
where r.сотрудник_id = s.id;

select s.id, s.фио, d.название
from Сотрудник s, Должность d, Сотрудник_Должность sd
where s.id = sd.сотрудник_id and d.id = sd.должность_id;
```
<li>Реализовать запросы пункта 2.1 через внутреннее соединение inner join.</li>

```
select r.id, r.аудитория, r.время, s.фио
from Сотрудник s
join Расписание r on r.сотрудник_id = s.id;

select s.id, s.фио, d.название
from Сотрудник s
join Сотрудник_Должность sd on s.id = sd.сотрудник_id
join Должность d on d.id = sd.должность_id;
```
<li>Левое внешнее соединение left join. Привести 2-3 запроса.</li>

```
SELECT c.фио, r.название_занятия, r.время
FROM Сотрудник c
LEFT JOIN Расписание r ON c.id = r.сотрудник_id;

select r.название_занятия, z.дата, z.занятие_проведено
from Расписание r
left join Занятие z on r.id = z.расписание_id;
```
<li>Правое внешнее соединение right join. Привести 2-3 запроса.</li>

```
SELECT c.фио, r.название_занятия, r.время
FROM Расписание r
LEFT JOIN Сотрудник c ON c.id = r.сотрудник_id;

select r.название_занятия, z.дата, z.занятие_проведено
from Занятие z
right join Расписание r on r.id = z.расписание_id;
```
<li>Привести примеры 2-3 запросов с использованием агрегатных функций и группировки.</li>

```
select сотрудник_id, sum(процентная_ставка) as сумма_выплат
from Сотрудник_Должность 
group by сотрудник_id
order by сумма_выплат DESC;

select r.сотрудник_id, count(z.занятие_проведено) as количество_проведенных
from Расписание r
left join Занятие z on z.расписание_id = r.id
where занятие_проведено = 1
group by r.сотрудник_id;
```
<li>Привести примеры 2-3 запросов с использованием группировки и условия отбора групп (Having).</li>

```
select id, название_занятия
from Расписание
group by id
having аудитория between '300' and '399';

select k.название_кафедры, count(sk.сотрудник_id) as количество_сотрудников
from Кафедра k
join Сотрудник_Кафедра sk on k.id = sk.кафедра_id
group by k.название_кафедры
having count(sk.сотрудник_id) > 1;
```
<li>Привести примеры 3-4 вложенных (соотнесенных, с использованием IN, EXISTS) запросов.</li>

```
select фио, телефон
from Сотрудник s
where exists(
select 1
from Расписание r
where r.сотрудник_id = s.id
);

select k.название_кафедры, k.телефон
from Кафедра k
where not exists(
select 1
from Сотрудник_Кафедра sk
where sk.кафедра_id = k.id
);

select s.фио, r.название_занятия
from Расписание r
join Сотрудник s on r.сотрудник_id = s.id
where s.id in (
select sd.сотрудник_id
from Сотрудник_Должность sd
where sd.процентная_ставка > 300.00
);
```
</ol>
    </li>
    <li>Представления
      <ol type="1">
        <li>На основе любых запросов из п. 2 создать два представления (VIEW).</li>

```
create view занятия_на_3 as
select id, название_занятия
from Расписание
group by id
having аудитория between '300' and '399';

create view ФИО_Должность as 
select s.id, s.фио, d.название
from Сотрудник s, Должность d, Сотрудник_Должность sd
where s.id = sd.сотрудник_id and d.id = sd.должность_id;
```
<li>Привести примеры использования общетабличных выражений (CTE) (2-3 запроса).</li>

```
with
СотрудникСДолжностями as (
select s.фио, k.название_кафедры, d.название as должность, sd.процентная_ставка
from Сотрудник s
join Сотрудник_Кафедра sk on sk.сотрудник_id = s.id
join Кафедра k on k.id = sk.кафедра_id
join Сотрудник_Должность sd on s.id = sd.сотрудник_id
join Должность d on d.id = sd.должность_id
),

СтавкиПоКафедрам as (
select название_кафедры, count(distinct фио) as сотрудников, sum(процентная_ставка) as выплаты
from СотрудникСДолжностями
group by название_кафедры
)

select название_кафедры, сотрудников, выплаты
from СтавкиПоКафедрам
order by выплаты DESC;

with
РасписаниеСотрудников as (
select r.id, r.аудитория, r.время, r.название_занятия, s.фио as преподаватель
from Расписание r
join Сотрудник s on r.сотрудник_id = s.id
),

СтатистикаЗанятий as (
select rs.преподаватель, count(z.id) as всего_занятий, sum(z.занятие_проведено) as проведено_занятий
from РасписаниеСотрудников rs
left join Занятие z on z.расписание_id = rs.id
group by rs.преподаватель
)

select * from СтатистикаЗанятий;
```
  </ol>
    </li>
    <li>Функции ранжирования
      <ol type="1">
        <li>Привести примеры 3-4 запросов с использованием ROW_NUMBER, RANK, DENSE_RANK (с PARTITION BY и без).</li>

```
select фио, дата_рождения,
row_number() over (order by дата_рождения) as row_number,
rank() over (order by дата_рождения) as rank,
dense_rank() over (order by дата_рождения) as dense_rank
from Сотрудник
where дата_рождения is not null;

select s.фио, d.название, sd.процентная_ставка,
row_number() over (partition by d.id order by sd.процентная_ставка desc) as row_number,
dense_rank() over (partition by d.id order by sd.процентная_ставка desc) as dense_rank
from Сотрудник_Должность sd
join Сотрудник s on s.id = sd.сотрудник_id
join Должность d on d.id = sd.должность_id;

select название_кафедры, аудитория, телефон,
row_number() over (order by аудитория) as row_number,
rank() over (order by аудитория) as rank,
dense_rank() over (order by аудитория) as dense_rank
from Кафедра;
```
  </ol>
    </li>
    <li>Объединение, пересечение, разность
      <ol type="1">
        <li>Привести примеры 3-4 запросов с использованием UNION / UNION ALL, EXCEPT, INTERSECT. Данные в одном из запросов отсортируйте по произвольному признаку.</li>

```
select аудитория
from Расписание

except

select аудитория
from Кафедра
order by аудитория;

select аудитория
from Расписание

intersect

select аудитория
from Кафедра;

select аудитория
from Расписание

union all

select аудитория
from Кафедра
order by аудитория;
```
 </ol>
    </li>
    <li>Использование CASE, PIVOT и UNPIVOT
      <ol type="1">
        <li>Привести примеры получения сводных (итоговых) таблиц с использованием CASE.</li>

```
select count(*) as всего_занятий,
sum(case when занятие_проведено = 1 then 1 else 0 end) as проведено,
sum(case when занятие_проведено = 0 then 1 else 0 end) as не_проведено
from Занятие;

select count(*) as всего_аудиторий,
sum(case when аудитория like '1%' then 1 else 0 end) as первый_этаж,
sum(case when аудитория like '2%' then 1 else 0 end) as второй_этаж,
sum(case when аудитория like '3%' then 1 else 0 end) as третий_этаж,
sum(case when аудитория like '4%' then 1 else 0 end) as четвертый_этаж,
sum(case when аудитория like '5%' then 1 else 0 end) as пятый_этаж
from Расписание;
```
  <li>Привести примеры получения сводных (итоговых) таблиц с использованием PIVOT и UNPIVOT.</li>

```
select *
from crosstab(
'select k.название_кафедры, d.название as должность, sum(sd.процентная_ставка) as сумма_ставок
from Сотрудник_Должность sd
join Должность d on d.id = sd.должность_id
join Сотрудник s on s.id = sd.сотрудник_id
join Сотрудник_Кафедра sk on sk.сотрудник_id = s.id
join Кафедра k on k.id = sk.кафедра_id
group by k.название_кафедры, d.название
order by 1, 2'
) as ct (
кафедра varchar(100),
профессор numeric,
доцент numeric,
ассистент numeric
);
```
  </ol>
    </li>
  </ol>

  <p><b>Обязательными к выполнению</b> являются запросы, приведенные ниже (смотри свой вариант).</p>
  <p>Отчет по лабораторной работе предоставляется в виде документа <i>(Фамилия_Группа.docx)</i>.  
  В этом документе по каждому заданию необходимо представить: условие запроса, текст SQL-запроса, скрин-копию результата выполнения запроса.</p>

  <h4>Часть 2</h4>
  <p><b>Составить следующие запросы:</b></p>
  <ol type="a">
    <li>Найти сотрудников, работающих более чем на 1 ставке.</li>

```
select s.id, count(distinct должность_id)
from Сотрудник s
join Сотрудник_Должность sd on sd.сотрудник_id = s.id
group  by s.id
having count(distinct должность_id) > 1;
```
<li>Вывести список отделов, где количество сотрудников пенсионного возраста превышает 20%.</li>

```
select k.id, count(*) as всего_сотрудников, count(case when age(s. дата_рождения ) >= interval '60 years' then 1 end) as пенсионеров
from Кафедра k
join Сотрудник_Кафедра  sk on k.id = sk.кафедра_id 
join Сотрудник  s on sk.сотрудник_id = s.id
group by k.id, k.название_кафедры
having count(case when age(s.дата_рождения) >= interval '60 years' then 1 end) * 100.0 / count(*) < 20
order by k.id;
```
<li>Вывести список сотрудников, чья зарплата превышает среднюю по вузу.</li>

```
select s.фио, sum(sd.процентная_ставка) as зарплата
from Сотрудник s
join Сотрудник_Должность sd on s.id = sd.сотрудник_id
group by s.id, s.фио 
having sum(sd.процентная_ставка) > (
select AVG(зарплата) 
from (
select SUM( процентная_ставка ) as зарплата
from  Сотрудник_Должность 
group  by  сотрудник_id 
) as зарплаты
)
order by зарплата DESC;
```
<li>Вывести список должностей с количеством ставок, на которые в каких-либо отлелах/кафедрах есть свободные вакансии.</li>

```
select d.название
from Должность d
left join Сотрудник_Должность sd on d.id = sd.должность_id
where sd.должность_id is null
group by d.id, d.название;
```
<li>Вывести список сотрудников, работающих более чем в 1-м отделе/кафедре с указанием списка отделов для каждого сотрудника.</li>

```
select s.id, max(s.фио) as фио, string_agg(d.название, ', ') as все_должности
from Сотрудник s
join Сотрудник_Должность sd on sd.сотрудник_id = s.id
join Должность d on d.id = должность_id
group by s.id
having count(distinct sd.должность_id) > 1;
```
  </ol>
</div>
<a href="https://github.com/Nikita-Nemtinov276/data-bases-Nemtinov-PMI32/blob/main/SUBO/Немтинов_ПМИ32_лаба3.docx" target="_blank">Лабораторная №3</a>

# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab4
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
</h3>

<div>
  <h4>Создать  4 различных хранимых процедуры:</h4>
  <ol type="a">
    <li><b>Процедура без параметров, формирующая список сотрудников, работающих более чем на одной кафедре(отделе) в виде: ФИО, название кафедры(отдела).</li>
      
```
CREATE OR REPLACE PROCEDURE employees_multiple_departments()
LANGUAGE plpgsql
AS $$
DECLARE
    rec RECORD;
BEGIN
    DROP TABLE IF EXISTS temp_multiple_dept_employees;
    CREATE TEMP TABLE temp_multiple_dept_employees AS
    SELECT 
        s."фио" AS full_name,
        k."название_кафедры" AS department_name
    FROM "Сотрудник" s
    JOIN "Сотрудник_Кафедра" sk ON s.id = sk."сотрудник_id"
    JOIN "Кафедра" k ON sk."кафедра_id" = k.id
    WHERE s.id IN (
        SELECT "сотрудник_id" 
        FROM "Сотрудник_Кафедра" 
        GROUP BY "сотрудник_id" 
        HAVING COUNT(DISTINCT "кафедра_id") > 1
    )
    ORDER BY s."фио", k."название_кафедры";
    
    RAISE NOTICE 'Сотрудники, работающие на нескольких кафедрах:';
    FOR rec IN SELECT * FROM temp_multiple_dept_employees ORDER BY full_name, department_name
    LOOP
        RAISE NOTICE 'ФИО: %, Кафедра: %', rec.full_name, rec.department_name;
    END LOOP;
END;
$$;
```
![image](/SUBO/4.1.a.png)

<li><b> Процедура, на входе получающая название должности и формирующая список кафедр, на которых есть ровно 1 свободная ставка по этой должности.</li>
  
```
CREATE OR REPLACE PROCEDURE public.departments_with_one_vacancy()
LANGUAGE 'sql'
AS $BODY$
CREATE OR REPLACE PROCEDURE departments_with_one_vacancy(
    IN position_name VARCHAR(1000)
)
LANGUAGE plpgsql
AS $$
BEGIN
    DROP TABLE IF EXISTS temp_departments_vacancy;
    CREATE TEMP TABLE temp_departments_vacancy AS
    SELECT 
        k."название_кафедры" AS department_name,
        COUNT(DISTINCT sd."сотрудник_id") AS current_employees
    FROM "Кафедра" k
    CROSS JOIN "Должность" d
    LEFT JOIN "Сотрудник_Кафедра" sk ON k.id = sk."кафедра_id"
    LEFT JOIN "Сотрудник_Должность" sd ON sk."сотрудник_id" = sd."сотрудник_id" AND d.id = sd."должность_id"
    WHERE d."название" = position_name
    GROUP BY k.id, k."название_кафедры", d.id
    HAVING COUNT(DISTINCT sd."сотрудник_id") = 1;
    
    IF EXISTS (SELECT 1 FROM temp_departments_vacancy) THEN
        RAISE NOTICE 'Кафедры с ровно одной свободной ставкой по должности "%":', position_name;
        FOR rec IN (SELECT * FROM temp_departments_vacancy ORDER BY department_name) 
        LOOP
            RAISE NOTICE 'Кафедра: %, Текущие сотрудники: %', rec.department_name, rec.current_employees;
        END LOOP;
    ELSE
        RAISE NOTICE 'Нет кафедр с одной свободной ставкой по должности "%"', position_name;
    END IF;
END;
$$;
$BODY$;
```
![image](/SUBO/4.1.b.png)

<li><b> Процедура, на входе получающая ФИО сотрудника, на выходе выдающая общее количество ставок, на которых он работает (с учетом возможной работы на нескольких кафедрах).</li>

```
CREATE OR REPLACE PROCEDURE public.get_employee_total_positions(
	)
LANGUAGE 'sql'
AS $BODY$
CREATE OR REPLACE PROCEDURE get_employee_total_positions(
    IN employee_fio VARCHAR(100),
    OUT total_positions NUMERIC(10,2)
)
LANGUAGE plpgsql
AS $$
BEGIN
    SELECT COALESCE(SUM(sd."процентная_ставка"), 0) INTO total_positions
    FROM "Сотрудник_Должность" sd
    JOIN "Сотрудник" s ON sd."сотрудник_id" = s.id
    WHERE s."фио" = employee_fio;
    
    RAISE NOTICE 'Сотрудник "%" работает на % ставках', employee_fio, total_positions;
END;
$$;
$BODY$;
```
![image](/SUBO/4.1.c.png)

<li><b> Процедура, вызывающая вложенную процедуру, которая находит средний оклад сотрудников вуза. Вызывающая процедура находит сотрудников, имеющих меньший оклад и назначает им единовременное пособие, при этом формирует данные в виде: ФИО сотрудника, размер пособия в рублях (равный разности суммарного оклада сотрудника и среднего оклада по вузу).</li>

```
CREATE OR REPLACE PROCEDURE calculate_average_salary(
    OUT average_salary NUMERIC(10,2)
)
LANGUAGE plpgsql
AS $$
BEGIN
    -- Средняя процентная ставка по всем сотрудникам
    SELECT AVG(sd."процентная_ставка") INTO average_salary
    FROM "Сотрудник_Должность" sd;
    
    RAISE NOTICE 'Средняя ставка (оклад) по вузу: %%', average_salary;
END;
$$;
```

```
CREATE OR REPLACE PROCEDURE public.assign_subsidy_below_average()
LANGUAGE 'plpgsql'
AS $BODY$
DECLARE
    avg_salary NUMERIC(10,2);
    emp_record RECORD;
BEGIN
    CALL calculate_average_salary(avg_salary);
    
    DROP TABLE IF EXISTS employee_subsidies;
    CREATE TEMP TABLE employee_subsidies (
        full_name VARCHAR(100),
        total_salary NUMERIC(10,2),
        subsidy_amount NUMERIC(10,2)
    );
    
    INSERT INTO employee_subsidies
    SELECT 
        s."фио" AS full_name,
        SUM(sd."процентная_ставка") AS total_salary,
        (avg_salary - SUM(sd."процентная_ставка")) AS subsidy_amount
    FROM "Сотрудник" s
    JOIN "Сотрудник_Должность" sd ON s.id = sd."сотрудник_id"
    GROUP BY s.id, s."фио"
    HAVING SUM(sd."процентная_ставка") < avg_salary;
    
    RAISE NOTICE 'Сотрудники со ставкой ниже средней (%):', avg_salary;
    FOR emp_record IN SELECT * FROM employee_subsidies ORDER BY subsidy_amount DESC
    LOOP
        RAISE NOTICE 'ФИО: %, Общая ставка: %, Пособие: %', 
            emp_record.full_name, 
            emp_record.total_salary, 
            emp_record.subsidy_amount;
    END LOOP;
END;
$BODY$;
```
![image](/SUBO/4.1.d.png)

</ol>

<h4>3 пользовательские функции:</h4>
<ol type="a">
<li><b>Скалярная функция, возвращающая количество работающих пенсионеров вуза.</li>
		
```
CREATE OR REPLACE FUNCTION public.count_retired_employees()
RETURNS integer
LANGUAGE 'plpgsql'
COST 100
VOLATILE PARALLEL UNSAFE
AS $BODY$
DECLARE
    retired_count INTEGER;
BEGIN
    SELECT COUNT(*) INTO retired_count
    FROM "Сотрудник" s
    JOIN "Сотрудник_Должность" sd ON s.id = sd."сотрудник_id"
    WHERE EXTRACT(YEAR FROM AGE(s."дата_рождения")) >= 60;
    
    RETURN retired_count;
END;
$BODY$;
```
![image](/SUBO/4.2.a.png)

<li><b>Inline-функция, получающая на входе название должности и возвращающая список сотрудников по отделам(кафедрам), работающих в этой должности.</li>

```
CREATE OR REPLACE FUNCTION public.get_employees_by_position(
	position_name character varying)
    RETURNS TABLE(department_name character varying, employee_fio character varying, position_stake numeric) 
    LANGUAGE 'sql'
    COST 100
    VOLATILE PARALLEL UNSAFE
    ROWS 1000

AS $BODY$
    SELECT 
        k."название_кафедры" AS department_name,
        s."фио" AS employee_fio,
        sd."процентная_ставка" AS position_stake
    FROM "Сотрудник" s
    JOIN "Сотрудник_Должность" sd ON s.id = sd."сотрудник_id"
    JOIN "Должность" d ON sd."должность_id" = d.id
    JOIN "Сотрудник_Кафедра" sk ON s.id = sk."сотрудник_id"
    JOIN "Кафедра" k ON sk."кафедра_id" = k.id
    WHERE d."название" = position_name
    ORDER BY k."название_кафедры", s."фио";
$BODY$;
```
![image](/SUBO/4.2.b.png)

<li><b>Multi-statement-функция, выдающая список кафедр, где сотрудники работают только на этой кафедре и больше нигде.</li>

```
CREATE OR REPLACE FUNCTION public.get_exclusive_departments(
	)
    RETURNS TABLE(department_name character varying, employee_count integer, total_stake numeric) 
    LANGUAGE 'plpgsql'
    COST 100
    VOLATILE PARALLEL UNSAFE
    ROWS 1000

AS $BODY$
BEGIN
    RETURN QUERY
    WITH exclusive_employees AS (
        SELECT "сотрудник_id"
        FROM "Сотрудник_Кафедра"
        GROUP BY "сотрудник_id"
        HAVING COUNT(DISTINCT "кафедра_id") = 1
    ),
    department_stats AS (
        SELECT 
            k.id AS department_id,
            k."название_кафедры" AS department_name,
            COUNT(DISTINCT s.id)::INTEGER AS employee_count,
            COALESCE(SUM(sd."процентная_ставка"), 0) AS total_stake
        FROM "Кафедра" k
        JOIN "Сотрудник_Кафедра" sk ON k.id = sk."кафедра_id"
        JOIN "Сотрудник" s ON sk."сотрудник_id" = s.id
        JOIN exclusive_employees ee ON s.id = ee."сотрудник_id"
        LEFT JOIN "Сотрудник_Должность" sd ON s.id = sd."сотрудник_id"
        GROUP BY k.id, k."название_кафедры"
    )
    SELECT 
        ds.department_name,
        ds.employee_count,
        ds.total_stake
    FROM department_stats ds
    WHERE ds.employee_count > 0
    ORDER BY ds.employee_count DESC, ds.department_name;
END;
$BODY$;
```
![image](/SUBO/4.2.c.png)

</ol>

<h4>Создать  3 триггера:</h4>
<ol type="a">
<li><b>Триггер любого типа на добавление сотрудника на кафедру – если сотрудник новый (не занят ни на каких других кафедрах), то его можно добавить только при условии, что по его должности на данной кафедре имеется не менее 0.5 свободных ставки.</li>

```
CREATE OR REPLACE FUNCTION public.check_employee_addition()
    RETURNS trigger
    LANGUAGE 'plpgsql'
    COST 100
    VOLATILE NOT LEAKPROOF
AS $BODY$
DECLARE
    employee_department_count INTEGER;
    total_salary NUMERIC(10,2);
    department_budget NUMERIC(10,2);
    used_budget NUMERIC(10,2);
    free_budget NUMERIC(10,2);
    min_salary NUMERIC(10,2) := 0.5;
BEGIN
    SELECT COUNT(*) INTO employee_department_count
    FROM "Сотрудник_Кафедра"
    WHERE "сотрудник_id" = NEW."сотрудник_id";
    
    IF employee_department_count = 0 THEN
        SELECT COALESCE(SUM("процентная_ставка"), 0) INTO total_salary
        FROM "Сотрудник_Должность"
        WHERE "сотрудник_id" = NEW."сотрудник_id";
        
        -- Предполагаем, что бюджет кафедры = 100000
        department_budget := 100000;
        
        SELECT COALESCE(SUM(sd."процентная_ставка"), 0) INTO used_budget
        FROM "Сотрудник_Должность" sd
        JOIN "Сотрудник_Кафедра" sk ON sd."сотрудник_id" = sk."сотрудник_id"
        WHERE sk."кафедра_id" = NEW."кафедра_id"
        AND sd."сотрудник_id" != NEW."сотрудник_id";
        
        free_budget := department_budget - used_budget;
        
        IF total_salary > free_budget THEN
            RAISE EXCEPTION 'Недостаточно бюджета на кафедре. Свободно: %, требуется: %', free_budget, total_salary;
        END IF;
        
        IF total_salary < min_salary THEN
            RAISE EXCEPTION 'Оклад сотрудника слишком низкий. Текущий: %, минимальный: %', total_salary, min_salary;
        END IF;
    END IF;
    
    RETURN NEW;
END;
$BODY$;
```
![image](/SUBO/4.3.a.png)

<li><b>Последующий триггер на изменение штатного расписания – при уменьшении количества ставок по какой-то должности для кафедры разрешить уменьшать не более чем на количество незанятых ставок.</li>

```
CREATE OR REPLACE FUNCTION public.check_staffing_changes()
RETURNS trigger
LANGUAGE 'plpgsql'
COST 100
VOLATILE NOT LEAKPROOF
AS $BODY$
DECLARE
    total_occupied_stake NUMERIC(10,2);
    max_reduction NUMERIC(10,2);
    reduction_amount NUMERIC(10,2);
BEGIN
    IF NEW."процентная_ставка" < OLD."процентная_ставка" THEN
        SELECT COALESCE(SUM("процентная_ставка"), 0) INTO total_occupied_stake
        FROM "Сотрудник_Должность"
        WHERE "должность_id" = OLD."должность_id";
        
        SELECT COALESCE(SUM("процентная_ставка"), 0) INTO total_occupied_stake
        FROM "Сотрудник_Должность"
        WHERE "должность_id" = OLD."должность_id"
        AND "сотрудник_id" != OLD."сотрудник_id";
        
		max_reduction := OLD."процентная_ставка" - total_occupied_stake;
        
        IF max_reduction < 0 THEN
            max_reduction := 0;
        END IF;
        
        reduction_amount := OLD."процентная_ставка" - NEW."процентная_ставка";
        
        IF reduction_amount > max_reduction THEN
            RAISE EXCEPTION 'Уменьшение оклада превышает допустимое. Текущий оклад: %, запрашиваемый: %, максимальное уменьшение: %', 
                OLD."процентная_ставка", NEW."процентная_ставка", max_reduction;
        END IF;
    END IF;
    
    RETURN NEW;
END;
$BODY$;
```
![image](/SUBO/4.3.b.png)

<li><b>Замещающий триггер на операцию увольнения сотрудника с кафедры – если сотрудник работает только в одном отделе (кафедре),  то его уволить оттуда нельзя</li>


```
CREATE OR REPLACE FUNCTION public.check_department_dismissal()
    RETURNS trigger
    LANGUAGE 'plpgsql'
    COST 100
    VOLATILE NOT LEAKPROOF
AS $BODY$
DECLARE
    department_count INTEGER;
BEGIN
    SELECT COUNT(*) INTO department_count
    FROM "Сотрудник_Кафедра"
    WHERE "сотрудник_id" = OLD."сотрудник_id";
    
    IF department_count = 1 THEN
        RAISE EXCEPTION 'Сотрудник ID % работает только на одной кафедре. Увольнение запрещено.', OLD."сотрудник_id";
    END IF;
    
    RETURN OLD;
END;
$BODY$;
```
![image](/SUBO/4.3.c.png)
</div>


# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab6
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
</h3>
<div>
<h4>Продумайте и создайте графовые таблицы по реляционной БД, заполните графовые таблицы используя данные в реляционных таблицах.</h4>
	
![image](/SUBO/laba6.png)
	
	```
	-- Включение графовой функциональности
	CREATE EXTENSION IF NOT EXISTS graph;
	
	-- Таблицы графа
	CREATE TABLE IF NOT EXISTS graph_nodes (
	    id SERIAL PRIMARY KEY,
	    node_label TEXT NOT NULL,
	    properties JSONB
	);
	
	CREATE TABLE IF NOT EXISTS graph_edges (
	    id SERIAL PRIMARY KEY,
	    edge_label TEXT NOT NULL,
	    from_node INT REFERENCES graph_nodes(id),
	    to_node INT REFERENCES graph_nodes(id),
	    properties JSONB
	);

	--Подготовка: удаляем старый граф и создаём новый
	SELECT drop_graph('университет_граф', true);
	SELECT create_graph('университет_граф');
	
	--Узлы: Кафедры
	SELECT * FROM cypher('университет_граф', $$
	CREATE
	  (k1:Кафедра {id:1, название:'Кафедра информатики и вычислительной техники', телефон:'79524816317', аудитория:'310', веб_сайт:'kafedra-informatiki.ru'}),
	  (k2:Кафедра {id:2, название:'Кафедра высшей математики', телефон:'79157034268', аудитория:'121', веб_сайт:'kafedra-matematiki.ru'}),
	  (k3:Кафедра {id:3, название:'Кафедра общей физики', телефон:'79963148502', аудитория:'502', веб_сайт:'kafedra-fiziki.ru'}),
	  (k4:Кафедра {id:4, название:'Кафедра органической химии', телефон:'79206583741', аудитория:'202', веб_сайт:'kafedra-himii.ru'}),
	  (k5:Кафедра {id:5, название:'Кафедра экономической теории', телефон:'79015482396', аудитория:'211', веб_сайт:'kafedra-ekonomiki.ru'}),
	  (k6:Кафедра {id:6, название:'Кафедра отечественной истории', телефон:'79843621507', аудитория:'422', веб_сайт:'kafedra-istorii.ru'}),
	  (k7:Кафедра {id:7, название:'Кафедра русской литературы', телефон:'79371825460', аудитория:'302', веб_сайт:'kafedra-literatury.ru'}),
	  (k8:Кафедра {id:8, название:'Кафедра иностранных языков', телефон:'79408256319', аудитория:'320', веб_сайт:'kafedra-inyaz.ru'}),
	  (k9:Кафедра {id:9, название:'Кафедра физического воспитания', телефон:'79632587014', аудитория:'512', веб_сайт:'kafedra-fizra.ru'}),
	  (k10:Кафедра {id:10, название:'Кафедра общей психологии', телефон:'79714028635', аудитория:'232', веб_сайт:'kafedra-psihologii.ru'})
	$$) AS t(n agtype);
	
	--Узлы: Сотрудники
	SELECT * FROM cypher('университет_граф', $$
	CREATE
	  (s1:Сотрудник {id:1, фио:'Иванов Петр Сергеевич', паспорт:'4001123456', адрес:'г. Москва, ул. Ленина, д. 10, кв. 5', телефон:'79161234567'}),
	  (s2:Сотрудник {id:2, фио:'Петрова Мария Ивановна', паспорт:'4001123457', адрес:'г. Москва, ул. Пушкина, д. 25, кв. 12', телефон:'79161234568'}),
	  (s3:Сотрудник {id:3, фио:'Сидоров Андрей Владимирович', паспорт:'4001123458', адрес:'г. Москва, пр-т Мира, д. 15, кв. 8', телефон:'79161234569'}),
	  (s4:Сотрудник {id:4, фио:'Козлова Елена Дмитриевна', паспорт:'4001123459', адрес:'г. Москва, ул. Гагарина, д. 7, кв. 3', телефон:'79161234570'}),
	  (s5:Сотрудник {id:5, фио:'Николаев Дмитрий Петрович', паспорт:'4001123460', адрес:'г. Москва, ул. Садовая, д. 30, кв. 15', телефон:'79161234571'}),
	  (s6:Сотрудник {id:6, фио:'Федоров Сергей Александрович', паспорт:'4001123461', адрес:'г. Москва, ул. Тверская, д. 20, кв. 7', телефон:'79161234572'}),
	  (s7:Сотрудник {id:7, фио:'Орлова Анна Викторовна', паспорт:'4001123462', адрес:'г. Москва, ул. Кутузовская, д. 5, кв. 22', телефон:'79161234573'}),
	  (s8:Сотрудник {id:8, фио:'Васнецов Игорь Николаевич', паспорт:'4001123463', адрес:'г. Москва, ул. Арбат, д. 35, кв. 10', телефон:'79161234574'}),
	  (s9:Сотрудник {id:9, фио:'Семенова Ольга Павловна', паспорт:'4001123464', адрес:'г. Москва, ул. Новый Арбат, д. 15, кв. 18', телефон:'79161234575'}),
	  (s10:Сотрудник {id:10, фио:'Кузнецов Алексей Борисович', паспорт:'4001123465', адрес:'г. Москва, ул. Ломоносова, д. 12, кв. 9', телефон:'79161234576'}),
	  (s11:Сотрудник {id:11, фио:'Попова Татьяна Михайловна', паспорт:'4001123466', адрес:'г. Москва, ул. Дмитровская, д. 8, кв. 14', телефон:'79161234577'}),
	  (s12:Сотрудник {id:12, фио:'Лебедев Виктор Степанович', паспорт:'4001123467', адрес:'г. Москва, ул. Профсоюзная, д. 25, кв. 6', телефон:'79161234578'}),
	  (s13:Сотрудник {id:13, фио:'Новикова Ирина Олеговна', паспорт:'4001123468', адрес:'г. Москва, ул. Академическая, д. 30, кв. 11', телефон:'79161234579'}),
	  (s14:Сотрудник {id:14, фио:'Морозов Павел Андреевич', паспорт:'4001123469', адрес:'г. Москва, ул. Научная, д. 17, кв. 4', телефон:'79161234580'}),
	  (s15:Сотрудник {id:15, фио:'Волкова Екатерина Сергеевна', паспорт:'4001123470', адрес:'г. Москва, ул. Студенческая, д. 22, кв. 13', телефон:'79161234581'})
	$$) AS t(n agtype);
	
	--Узлы: Должности
	SELECT * FROM cypher('университет_граф', $$
	CREATE
	  (d1:Должность {id:1, название:'Заведующий кафедрой'}),
	  (d2:Должность {id:2, название:'Профессор'}),
	  (d3:Должность {id:3, название:'Доцент'}),
	  (d4:Должность {id:4, название:'Старший преподаватель'}),
	  (d5:Должность {id:5, название:'Ассистент'}),
	  (d6:Должность {id:6, название:'Научный сотрудник'}),
	  (d7:Должность {id:7, название:'Лаборант'}),
	  (d8:Должность {id:8, название:'Декан факультета'}),
	  (d9:Должность {id:9, название:'Проректор по учебной работе'}),
	  (d10:Должность {id:10, название:'Руководитель образовательной программы'})
	$$) AS t(n agtype);
	
	--Узлы: Расписание (20 записей)
	SELECT * FROM cypher('университет_граф', $$
	CREATE
	  (r1:Расписание {id:1, аудитория:'311', время:'2026-01-15 09:00', название_занятия:'Базы данных и СУБД', сотрудник_id:1}),
	  (r2:Расписание {id:2, аудитория:'312', время:'2026-01-15 09:00', название_занятия:'Программирование на Python', сотрудник_id:11}),
	  (r3:Расписание {id:3, аудитория:'122', время:'2026-01-15 09:00', название_занятия:'Математический анализ', сотрудник_id:2}),
	  (r4:Расписание {id:4, аудитория:'123', время:'2026-01-15 10:30', название_занятия:'Линейная алгебра', сотрудник_id:12}),
	  (r5:Расписание {id:5, аудитория:'501', время:'2026-01-15 09:00', название_занятия:'Физика твердого тела', сотрудник_id:3}),
	  (r6:Расписание {id:6, аудитория:'503', время:'2026-01-15 14:00', название_занятия:'Квантовая механика', сотрудник_id:13}),
	  (r7:Расписание {id:7, аудитория:'203', время:'2026-01-15 09:00', название_занятия:'Органическая химия', сотрудник_id:4}),
	  (r8:Расписание {id:8, аудитория:'204', время:'2026-01-16 09:00', название_занятия:'Биохимия', сотрудник_id:4}),
	  (r9:Расписание {id:9, аудитория:'212', время:'2026-01-15 10:30', название_занятия:'Микроэкономика', сотрудник_id:5}),
	  (r10:Расписание {id:10, аудитория:'213', время:'2026-01-16 09:00', название_занятия:'Макроэкономика', сотрудник_id:5}),
	  (r11:Расписание {id:11, аудитория:'421', время:'2026-01-15 10:30', название_занятия:'История России XX века', сотрудник_id:6}),
	  (r12:Расписание {id:12, аудитория:'423', время:'2026-01-16 09:00', название_занятия:'История древнего мира', сотрудник_id:6}),
	  (r13:Расписание {id:13, аудитория:'301', время:'2026-01-15 10:30', название_занятия:'Русская литература XIX века', сотрудник_id:7}),
	  (r14:Расписание {id:14, аудитория:'303', время:'2026-01-16 10:30', название_занятия:'Зарубежная литература', сотрудник_id:7}),
	  (r15:Расписание {id:15, аудитория:'321', время:'2026-01-15 12:00', название_занятия:'Английский язык для начинающих', сотрудник_id:8}),
	  (r16:Расписание {id:16, аудитория:'322', время:'2026-01-16 10:30', название_занятия:'Немецкий язык', сотрудник_id:8}),
	  (r17:Расписание {id:17, аудитория:'511', время:'2026-01-15 12:00', название_занятия:'Спортивные игры', сотрудник_id:9}),
	  (r18:Расписание {id:18, аудитория:'513', время:'2026-01-16 10:30', название_занятия:'Легкая атлетика', сотрудник_id:9}),
	  (r19:Расписание {id:19, аудитория:'231', время:'2026-01-15 12:00', название_занятия:'Общая психология', сотрудник_id:10}),
	  (r20:Расписание {id:20, аудитория:'233', время:'2026-01-17 12:00', название_занятия:'Клиническая психология', сотрудник_id:10})
	$$) AS t(n agtype);
	
	--Узлы: Занятия (20 записей)
	SELECT * FROM cypher('университет_граф', $$
	CREATE
	  (z1:Занятие {id:1, дата:'2026-01-15', занятие_проведено:true, расписание_id:1}),
	  (z2:Занятие {id:2, дата:'2026-01-15', занятие_проведено:true, расписание_id:2}),
	  (z3:Занятие {id:3, дата:'2026-01-15', занятие_проведено:true, расписание_id:3}),
	  (z4:Занятие {id:4, дата:'2026-01-15', занятие_проведено:false, расписание_id:4}),
	  (z5:Занятие {id:5, дата:'2026-01-15', занятие_проведено:true, расписание_id:5}),
	  (z6:Занятие {id:6, дата:'2026-01-15', занятие_проведено:true, расписание_id:6}),
	  (z7:Занятие {id:7, дата:'2026-01-15', занятие_проведено:true, расписание_id:7}),
	  (z8:Занятие {id:8, дата:'2026-01-16', занятие_проведено:true, расписание_id:8}),
	  (z9:Занятие {id:9, дата:'2026-01-15', занятие_проведено:true, расписание_id:9}),
	  (z10:Занятие {id:10, дата:'2026-01-16', занятие_проведено:false, расписание_id:10}),
	  (z11:Занятие {id:11, дата:'2026-01-15', занятие_проведено:true, расписание_id:11}),
	  (z12:Занятие {id:12, дата:'2026-01-16', занятие_проведено:true, расписание_id:12}),
	  (z13:Занятие {id:13, дата:'2026-01-15', занятие_проведено:true, расписание_id:13}),
	  (z14:Занятие {id:14, дата:'2026-01-16', занятие_проведено:true, расписание_id:14}),
	  (z15:Занятие {id:15, дата:'2026-01-15', занятие_проведено:true, расписание_id:15}),
	  (z16:Занятие {id:16, дата:'2026-01-16', занятие_проведено:true, расписание_id:16}),
	  (z17:Занятие {id:17, дата:'2026-01-15', занятие_проведено:true, расписание_id:17}),
	  (z18:Занятие {id:18, дата:'2026-01-16', занятие_проведено:false, расписание_id:18}),
	  (z19:Занятие {id:19, дата:'2026-01-15', занятие_проведено:true, расписание_id:19}),
	  (z20:Занятие {id:20, дата:'2026-01-17', занятие_проведено:true, расписание_id:20})
	$$) AS t(n agtype);
	
	-- ==========================
	-- Сотрудник -> Кафедра (Работает_на)
	-- ==========================
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:1}),(k:Кафедра {id:1}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:2}),(k:Кафедра {id:2}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:3}),(k:Кафедра {id:3}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:4}),(k:Кафедра {id:4}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:5}),(k:Кафедра {id:5}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:6}),(k:Кафедра {id:6}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:7}),(k:Кафедра {id:7}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:8}),(k:Кафедра {id:8}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:9}),(k:Кафедра {id:9}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:10}),(k:Кафедра {id:10}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:11}),(k:Кафедра {id:1}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:12}),(k:Кафедра {id:2}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:13}),(k:Кафедра {id:3}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:14}),(k:Кафедра {id:4}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:15}),(k:Кафедра {id:5}) CREATE (s)-[:Работает_на]->(k)$$) AS t(n agtype);
	
	-- ==========================
	--Сотрудник -> Должность (Имеет_должность)
	-- ==========================
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:1}),(d:Должность {id:1}) CREATE (s)-[:Имеет_должность {ставка:450.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:2}),(d:Должность {id:1}) CREATE (s)-[:Имеет_должность {ставка:420.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:3}),(d:Должность {id:1}) CREATE (s)-[:Имеет_должность {ставка:400.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:4}),(d:Должность {id:2}) CREATE (s)-[:Имеет_должность {ставка:350.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:5}),(d:Должность {id:2}) CREATE (s)-[:Имеет_должность {ставка:380.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:6}),(d:Должность {id:2}) CREATE (s)-[:Имеет_должность {ставка:320.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:7}),(d:Должность {id:3}) CREATE (s)-[:Имеет_должность {ставка:280.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:8}),(d:Должность {id:3}) CREATE (s)-[:Имеет_должность {ставка:250.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:9}),(d:Должность {id:3}) CREATE (s)-[:Имеет_должность {ставка:220.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:10}),(d:Должность {id:4}) CREATE (s)-[:Имеет_должность {ставка:200.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:11}),(d:Должность {id:4}) CREATE (s)-[:Имеет_должность {ставка:180.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:12}),(d:Должность {id:4}) CREATE (s)-[:Имеет_должность {ставка:170.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:13}),(d:Должность {id:5}) CREATE (s)-[:Имеет_должность {ставка:150.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:14}),(d:Должность {id:5}) CREATE (s)-[:Имеет_должность {ставка:140.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:15}),(d:Должность {id:5}) CREATE (s)-[:Имеет_должность {ставка:130.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:1}),(d:Должность {id:2}) CREATE (s)-[:Имеет_должность {ставка:200.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:2}),(d:Должность {id:3}) CREATE (s)-[:Имеет_должность {ставка:150.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:8}),(d:Должность {id:4}) CREATE (s)-[:Имеет_должность {ставка:120.00}]->(d)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:11}),(d:Должность {id:5}) CREATE (s)-[:Имеет_должность {ставка:100.00}]->(d)$$) AS t(n agtype);
	
	-- ==========================
	--Сотрудник -> Расписание (Преподает)
	-- ==========================
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:1}),(r:Расписание {id:1}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:11}),(r:Расписание {id:2}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:2}),(r:Расписание {id:3}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:12}),(r:Расписание {id:4}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:3}),(r:Расписание {id:5}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:13}),(r:Расписание {id:6}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:4}),(r:Расписание {id:7}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:4}),(r:Расписание {id:8}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:5}),(r:Расписание {id:9}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:5}),(r:Расписание {id:10}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:6}),(r:Расписание {id:11}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:6}),(r:Расписание {id:12}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:7}),(r:Расписание {id:13}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:7}),(r:Расписание {id:14}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:8}),(r:Расписание {id:15}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:8}),(r:Расписание {id:16}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:9}),(r:Расписание {id:17}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:10}),(r:Расписание {id:18}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:10}),(r:Расписание {id:19}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (s:Сотрудник {id:11}),(r:Расписание {id:20}) CREATE (s)-[:Преподает]->(r)$$) AS t(n agtype);
	
	-- ==========================
	--Расписание -> Занятие (Содержит_занятие)
	-- ==========================
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:1}),(z:Занятие {id:1}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:2}),(z:Занятие {id:2}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:3}),(z:Занятие {id:3}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:4}),(z:Занятие {id:4}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:5}),(z:Занятие {id:5}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:6}),(z:Занятие {id:6}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:7}),(z:Занятие {id:7}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:8}),(z:Занятие {id:8}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:9}),(z:Занятие {id:9}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:10}),(z:Занятие {id:10}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:11}),(z:Занятие {id:11}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:12}),(z:Занятие {id:12}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:13}),(z:Занятие {id:13}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:14}),(z:Занятие {id:14}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:15}),(z:Занятие {id:15}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:16}),(z:Занятие {id:16}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:17}),(z:Занятие {id:17}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:18}),(z:Занятие {id:18}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:19}),(z:Занятие {id:19}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	SELECT * FROM cypher('университет_граф', $$MATCH (r:Расписание {id:20}),(z:Занятие {id:20}) CREATE (r)-[:Содержит_занятие]->(z)$$) AS t(n agtype);
	```
<h4>Часть 2</h4>
<p><b>Составить следующие запросы:</b></p>
<ol type="a">
<li>Найти сотрудников, работающих более чем на 1 ставке.</li>

```
SELECT * FROM cypher('университет_граф', $$
MATCH (s:Сотрудник)-[:Имеет_должность]->(d:Должность)
WITH s, COUNT(DISTINCT d.id) AS количество_должностей
WHERE количество_должностей > 1
RETURN s.id, s.фио, количество_должностей
ORDER BY количество_должностей DESC
$$) AS t(id agtype, фио agtype, количество_должностей agtype);
```
![image](/SUBO/6.1.png)
<li>Вывести список отделов, где количество сотрудников пенсионного возраста превышает 20%.</li>

```
SELECT * FROM cypher('университет_граф', $$
MATCH (k:Кафедра)<-[:Работает_на]-(s:Сотрудник)
WHERE s.дата_рождения IS NOT NULL
WITH k, s, split(s.дата_рождения, '-')[0] AS год_рождения
WHERE год_рождения IS NOT NULL
WITH k, s, toInteger(год_рождения) AS год_рождения_int
WITH k, s, 2025 - год_рождения_int AS возраст
WHERE возраст IS NOT NULL
WITH k, 
     COUNT(s) AS всего_сотрудников,
     SUM(CASE WHEN возраст >= 60 THEN 1 ELSE 0 END) AS пенсионеров
WHERE пенсионеров * 100.0 / всего_сотрудников < 20
RETURN k.id AS id_кафедры,
       k.название AS название_кафедры,
       всего_сотрудников,
       пенсионеров
ORDER BY k.id
$$) AS t(id_кафедры agtype, название_кафедры agtype, всего_сотрудников agtype, пенсионеров agtype);
```
![image](/SUBO/6.2.png)
<li>Вывести список сотрудников, чья зарплата превышает среднюю по вузу.</li>

```
SELECT * FROM cypher('университет_граф', $$
MATCH (s:Сотрудник)-[rel:Имеет_должность]->(:Должность)
WITH AVG(rel.ставка) * COUNT(DISTINCT rel) / COUNT(DISTINCT s) AS средняя_зарплата
MATCH (s:Сотрудник)-[rel:Имеет_должность]->(:Должность)
WITH s, SUM(rel.ставка) AS зарплата, средняя_зарплата
WHERE зарплата > средняя_зарплата
RETURN s.фио, зарплата
ORDER BY зарплата DESC
$$) AS t(фио agtype, зарплата agtype);
```
![image](/SUBO/6.3.png)
<li>Вывести список должностей с количеством ставок, на которые в каких-либо отлелах/кафедрах есть свободные вакансии.</li>

```
SELECT * FROM cypher('университет_граф', $$
MATCH (d:Должность)
WHERE NOT EXISTS {
	MATCH (d)<-[:Имеет_должность]-(:Сотрудник)
}
RETURN d.название
ORDER BY d.название
$$) AS t(название agtype);
```
![image](/SUBO/6.4.png)
<li>Вывести список сотрудников, работающих более чем в 1-м отделе/кафедре с указанием списка отделов для каждого сотрудника.</li>

```
SELECT * FROM cypher('университет_граф', $$
MATCH (s:Сотрудник)-[:Имеет_должность]->(d:Должность)
WITH s, COLLECT(DISTINCT d.название) AS должности
WHERE SIZE(должности) > 1
RETURN s.id, s.фио, должности
ORDER BY s.id
$$) AS t(id agtype, фио agtype, должности agtype);
```
![image](/SUBO/6.5.png)
</div>


# <img src="https://github.com/user-attachments/assets/e080adec-6af7-4bd2-b232-d43cb37024ac" width="20" height="20"/> Lab7
[Назад](#content)
<h3 align="center">
  <a href="#client"></a>
</h3>
<div>
	<a href="https://github.com/Nikita-Nemtinov276/data-bases-Nemtinov-PMI32/blob/main/SUBO/НемтиновЛаб7-1.docx" target="_blank">Лабораторная №7-1</a>

Код первого задания:


```
INSERT INTO public."Кафедра" ("телефон","аудитория","название_кафедры")
VALUES ('99999999999','999','кафедра')
INSERT INTO "Кафедра" ("телефон","аудитория","название_кафедры")
VALUES ('99999999998','998','кафедра 2')

SELECT * FROM "Кафедра";

BEGIN;
-- допустим, хотим удалить кафедру с id = 15
DELETE FROM "Кафедра" WHERE id = 15;
SELECT * FROM "Кафедра";

-- Отмена транзакции — откат
ROLLBACK;
SELECT * FROM "Кафедра";

-- Повторим: удалим, применим savepoint, откатим часть и затем зафиксируем
BEGIN;
DELETE FROM "Кафедра" WHERE id = 15;
-- видим отсутствие
SELECT * FROM "Кафедра" WHERE id = 15;

-- создаём savepoint
SAVEPOINT sp1;

-- допустим удалили ещё одну кафедру
DELETE FROM "Кафедра" WHERE id = 17;
SELECT * FROM "Кафедра";

-- откат к savepoint — восстановит id=17 (до sp1), но не восстановит удаление id=15
ROLLBACK TO SAVEPOINT sp1;
SELECT * FROM "Кафедра";

-- теперь снова добавим (восстановим) запись id = 15 и зафиксируем
ROLLBACK;
SELECT * FROM "Кафедра";

COMMIT;
```

<a href="https://github.com/Nikita-Nemtinov276/data-bases-Nemtinov-PMI32/blob/main/SUBO/НемтиновЛаб7-1.docx" target="_blank">Лабораторная №7-2</a>

Код второго задания:

```
-- В обеих сессиях:
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

-- Сценарий 1
-- A
BEGIN;
SELECT "процентная_ставка" FROM "Сотрудник_Должность"
WHERE "сотрудник_id" = 1 AND "должность_id" = 1;
-- имитируем вычисление: -10000
UPDATE "Сотрудник_Должность"
SET "процентная_ставка" = "процентная_ставка" - 10000
WHERE "сотрудник_id" = 1 AND "должность_id" = 1;
-- НЕ COMMIT пока

-- B
BEGIN;
SELECT "процентная_ставка" FROM "Сотрудник_Должность"
WHERE "сотрудник_id" = 1 AND "должность_id" = 1;
UPDATE "Сотрудник_Должность"
SET "процентная_ставка" = "процентная_ставка" - 20000
WHERE "сотрудник_id" = 1 AND "должность_id" = 1;
COMMIT;  -- B завершил изменения

-- A
COMMIT;
SELECT "процентная_ставка" FROM "Сотрудник_Должность"
WHERE "сотрудник_id" = 1 AND "должность_id" = 1;


-- Сценарий 2
-- A
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

BEGIN;
UPDATE "Сотрудник"
SET фио = 'Незакоммиченные данные'
WHERE id = 1;
-- НЕ делаем COMMIT

-- B
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT фио FROM "Сотрудник" WHERE id = 1;

-- A
ROLLBACK;


-- Сценарий 4
-- A
BEGIN;
SELECT "адрес" FROM "Сотрудник" WHERE id = 1;
-- не COMMIT

-- B
BEGIN;
UPDATE "Сотрудник" SET "адрес" = 'ул. Новый' WHERE id = 1;
COMMIT;

-- A
SELECT "адрес" FROM "Сотрудник" WHERE id = 1; -- при READ COMMITTED увидит 'ул. Новый'
COMMIT;


-- Сценарий 5
-- A
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN;
SELECT "адрес" FROM "Сотрудник" WHERE id = 1;
-- не COMMIT

-- B
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN;
UPDATE "Сотрудник" SET "адрес" = 'ул. Новый2' WHERE id = 1;
COMMIT;

-- A
SELECT "адрес" FROM "Сотрудник" WHERE id = 1; -- при REPEATABLE READ вернёт прежнее значение
COMMIT;


-- Сценарий 6
-- A
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN;
SELECT * FROM "Сотрудник" WHERE фио LIKE 'Фантом%';

-- B
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN;
INSERT INTO "Сотрудник"(паспорт, фио)
VALUES ('8888888888', 'Фантомный пользователь');
COMMIT;

-- A
SELECT * FROM "Сотрудник" WHERE фио LIKE 'Фантом%';
COMMIT;


-- Сценарий 7
-- A
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN;
SELECT * FROM "Сотрудник" WHERE фио LIKE 'СерФантом%';

-- B
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN;
INSERT INTO "Сотрудник"(паспорт, фио)
VALUES ('7777777777', 'СерФантом');
COMMIT;

-- A
SELECT * FROM "Сотрудник" WHERE фио LIKE 'СерФантом%';
COMMIT;

```
</div>
