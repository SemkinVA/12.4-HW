# 12.4-HW
### Домашнее задание к занятию 12.4 - "SQL. Часть 2" - Семкин Вячеслав
***
### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

**Ответ:**
```
select s.store_id as №_магазина,
       concat(s2.last_name, ' ', s2.first_name) as ФИО_сотрудника,
       c.city as Город,
       count(c2.customer_id) as Количество_пользователей		
from store s 
join address a on a.address_id = s.address_id 
join staff s2 on s2.staff_id = s.store_id 
join city c on c.city_id = a.city_id 
join customer c2 on c2.store_id = s.store_id 
group by s.store_id, s2.last_name, s2.first_name, c.city_id, s2.staff_id 
having count(c2.customer_id) > 300;
```
***
### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

**Ответ:**
```
select count(length) as Долгие_фильмы
from film
where length > (select avg(length) from film);
```
***
### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

**Ответ:**
```
select month(p.payment_date) as Месяц, sum(p.amount) as Получено, count(p.rental_id) as Аренды  
from payment p
group by month(p.payment_date)
order by sum(p.amount) desc
limit 1;
```
***
### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

**Ответ:**
```
select concat(s.last_name, ' ', s.first_name) as Продавец,
       count(p.amount) as Продажи,
		case 
			when count(p.amount) > 8000 then 'Да'
			when count(p.amount) < 8000 then 'Нет'
		end as Премия,
		sum(p.amount) as Доход
from payment p
join staff s on s.staff_id = p.staff_id 
group by Продавец
order by Продажи desc;
```
***
### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

**Ответ:**
```
select f.title as Название_фильма,
       p.amount as Брали
from payment p 
join rental r on r.rental_id = p.rental_id 
join inventory i on i.inventory_id = r.inventory_id 
join film f on f.film_id = i.film_id  
where p.amount = 0
order by f.title; 
```
***
