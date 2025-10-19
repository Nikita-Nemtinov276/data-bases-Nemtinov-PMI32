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
select s.id, s.фио, k.название_кафедры
from Сотрудник s
left join Сотрудник_Кафедра sk on s.id = sk.сотрудник_id
left join Кафедра k on k.id = sk.кафедра_id;

select r.название_занятия, z.дата, z.занятие_проведено
from Расписание r
left join Занятие z on r.id = z.расписание_id;
```
<li>Правое внешнее соединение right join. Привести 2-3 запроса.</li>

```
select s.id, s.фио, k.название_кафедры
from Кафедра k 
right join Сотрудник_Кафедра sk on k.id = sk.кафедра_id
right join Сотрудник s on s.id = sk.сотрудник_id;

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

select название_кафедры, аудитория, телефон
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
