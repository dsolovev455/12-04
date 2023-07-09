# Домашнее задание к занятию «SQL. Часть 2» Соловьев Д.В

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

Ответ:

SELECT concat(staff.first_name  , ' ', staff.last_name) as Имя , city.city,  count(customer.customer_id) as Количество FROM staff
JOIN address ON staff.address_id = address.address_id
JOIN city ON address.city_id = city.city_id
JOIN store ON store.store_id = staff.store_id
JOIN customer ON store.store_id = customer.store_id
GROUP BY staff.first_name , staff.last_name , city.city
HAVING Количество > 300;

![alt text](https://github.com/dsolovev455/12-04/blob/main/img/1.png)


### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

Ответ:

SELECT  count(`length`) FROM film
WHERE `length` > (SELECT avg(`length`)FROM film );

![alt text](https://github.com/dsolovev455/12-04/blob/main/img/2.png)


### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

Ответ:

SELECT DATE_FORMAT(payment.payment_date, '%Y-%M') AS Дата , (sum(payment.amount )) AS Сумма , count((payment.rental_id )) AS Аренд FROM payment
GROUP BY Дата
ORDER BY Сумма DESC
LIMIT 1;

![alt text](https://github.com/dsolovev455/12-04/blob/main/img/3.png)


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

Ответ:

SELECT CONCAT(staff.first_name, ' ', staff.last_name) AS Продавец, COUNT(payment.amount) AS Продажи,
CASE
WHEN COUNT(payment.amount) > 8000 THEN 'YES'
WHEN COUNT(payment.amount) < 8000 THEN 'NO'
END AS Бонус, SUM(payment.amount) AS Доход FROM payment
JOIN staff ON staff.staff_id = payment.staff_id
WHERE payment.amount > 0
GROUP BY Продавец ORDER BY Продажи DESC;

![alt text](https://github.com/dsolovev455/12-04/blob/main/img/4.png)


### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

Ответ:

SELECT film.title, film.release_year, payment.amount, payment.payment_date, rental.return_date FROM payment
JOIN rental ON rental.rental_id = payment.rental_id
JOIN inventory ON inventory.inventory_id = rental.inventory_id
JOIN film ON film.film_id = inventory.film_id
WHERE payment.amount = 0 ORDER BY film.title;

![alt text](https://github.com/dsolovev455/12-04/blob/main/img/5.png)

