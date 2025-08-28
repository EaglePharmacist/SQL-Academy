<a href="https://sql-academy.org/ru/trainer">
    <img src="https://sql-academy.org/static/logo.svg" title="SQL-Academy" alt="SQL-Academy" width="100" height="100" alt="Кнопка">
</a>

### Задание 1. Вывести имена всех людей, которые есть в базе данных авиакомпаний
```sql
SELECT name
FROM Passenger;
```
### Задание 2. Вывести названия всеx авиакомпаний
```sql
SELECT name
FROM Company;
```
### Задание 3. Вывести все рейсы, совершенные из Москвы
```sql
SELECT *
FROM Trip
WHERE town_from = "Moscow";
```
### Задание 4. Вывести имена людей, которые заканчиваются на "man"
```sql
SELECT name
FROM Passenger
WHERE name LIKE "%man";
```
### Задание 5. Вывести количество рейсов, совершенных на TU-134
```sql
SELECT COUNT(*) AS COUNT
FROM Trip
WHERE plane = "TU-134";
```
### Задание 6. Какие компании совершали перелеты на Boeing
```sql
SELECT DISTINCT Company.name
FROM Company
	JOIN Trip ON Company.id = Trip.company
WHERE Trip.plane = "Boeing";
```
### Задание 7. Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
```sql
SELECT DISTINCT plane
FROM Trip
WHERE town_to = "Moscow"
```
### Задание 8. В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
```sql
SELECT town_to,
	TIMEDIFF(time_in, time_out) AS flight_time
FROM Trip
WHERE town_from = "Paris";
```
### Задание 9. Какие компании организуют перелеты из Владивостока (Vladivostok)?
```sql
SELECT DISTINCT Company.name
FROM Company
	JOIN Trip ON Company.id = Trip.company
WHERE town_from = "Vladivostok";
```
### Задание 10. Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.
```sql
SELECT *
FROM Trip
WHERE time_out >= "1900-01-01T10%"
	AND time_out <= "1900-01-01T14%";
```
### Задание 11. Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени
```sql
SELECT name
FROM Passenger
ORDER BY LENGTH(name) DESC
LIMIT 2;
```
### Задание 12. Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0"
```sql
SELECT Trip.id,
	COUNT(passenger) AS count
FROM Pass_in_trip
	RIGHT JOIN Trip ON Pass_in_trip.trip = Trip.id
GROUP BY Trip.id;
```
### Задание 13. Вывести имена людей, у которых есть полный тёзка среди пассажиров
```sql
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(*) > 1;
```
### Задание 14. В какие города летал Bruce Willis
```sql
SELECT town_to
FROM Trip
	JOIN Pass_in_trip ON Trip.id = Pass_in_trip.trip
	JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = "Bruce Willis";
```
### Задание 15. Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London)
```sql
SELECT Passenger.id,
	Trip.time_in
FROM Trip
	JOIN Pass_in_trip ON Trip.id = Pass_in_trip.trip
	JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = "Steve Martin"
	AND Trip.town_to = "London";
```
### Задание 16. Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет
```sql
SELECT name,
	COUNT(passenger) AS count
FROM Passenger
	JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
GROUP BY passenger
HAVING COUNT(trip) >= 1
ORDER BY count DESC,
	name;
```
### Задание 17. Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили
```sql
SELECT member_name,
	status,
	SUM(unit_price * amount) AS costs
FROM FamilyMembers AS f
	JOIN Payments AS p ON f.member_id = p.family_member
WHERE date LIKE "2005%"
GROUP BY member_name,
	status;
```
### Задание 18 . Выведите имя самого старшего человека. Если таких несколько, то выведите их всех
```sql
SELECT member_name
FROM FamilyMembers
WHERE birthday = (
		SELECT MIN(birthday)
		FROM FamilyMembers
	);
```
### Задание 19. Определить, кто из членов семьи покупал картошку (potato)
```sql
SELECT DISTINCT status
FROM FamilyMembers
	JOIN Payments ON FamilyMembers.member_id = Payments.family_member
	JOIN Goods ON Payments.good = Goods.good_id
WHERE Goods.good_name = "potato";
```
### Задание 20. Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму
```sql
SELECT status,
	member_name,
	SUM (unit_price * amount) AS costs
FROM FamilyMembers
	JOIN Payments ON FamilyMembers.member_id = Payments.family_member
	JOIN Goods ON Payments.good = Goods.good_id
	JOIN GoodTypes ON Goods.type = GoodTypes.good_type_id
WHERE good_type_name = "entertainment"
GROUP BY status,
	member_name;
```
### Задание 21. Определить товары, которые покупали более 1 раза
```sql
SELECT good_name
FROM Goods AS g
	JOIN Payments AS p ON g.good_id = p.good
GROUP BY good
HAVING COUNT(good) > 1;
```
### Задание 22. Найти имена всех матерей (mother)
```sql
SELECT member_name
FROM FamilyMembers
WHERE status = "mother";
```
### Задание 23. Найдите самый дорогой деликатес (delicacies) и выведите его цену
```sql
SELECT good_name,
	unit_price
FROM Goods
	JOIN GoodTypes ON Goods.type = GoodTypes.good_type_id
	JOIN Payments ON Goods.good_id = Payments.good
WHERE good_type_name = "delicacies"
ORDER BY unit_price DESC
LIMIT 1;
```
### Задание 24. Определить кто и сколько потратил в июне 2005
```sql
SELECT member_name,
	SUM(unit_price * amount) AS costs
FROM FamilyMembers AS f
	JOIN Payments AS p ON f.member_id = p.family_member
WHERE date LIKE "2005-06%"
GROUP BY member_name;
```
### Задание 25. Определить, какие товары не покупались в 2005 году
```sql
SELECT good_name
FROM Goods
WHERE good_id NOT IN(
		SELECT good
		FROM Payments
		WHERE YEAR(date) = 2005
	);
```
### Задание 26. Определить группы товаров, которые не приобретались в 2005 году
```sql
SELECT good_type_name
FROM GoodTypes
WHERE good_type_id NOT IN (
		SELECT type
		FROM Goods
			JOIN Payments ON Goods.good_id = Payments.good
		WHERE YEAR(date) = 2005
	);
```
### Задание 27. Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её
```sql
SELECT good_type_name,
	SUM(unit_price * amount) AS costs
FROM Payments
	JOIN Goods ON Payments.good = Goods.good_id
	JOIN GoodTypes ON Goods.type = GoodTypes.good_type_id
WHERE YEAR(date) = 2005
GROUP BY good_type_name;
```
### Задание 28. Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow)?
```sql
SELECT COUNT(*) AS count
FROM Trip
WHERE town_from = "Rostov"
	AND town_to = "Moscow";
```
### Задание 29. Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов
```sql
SELECT DISTINCT name
FROM Passenger
	JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
	JOIN Trip ON Pass_in_trip.trip = Trip.id
WHERE town_to = "Moscow"
	AND plane = "TU-134";
```
### Задание 30. Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности
```sql
SELECT trip,
	COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```
### Задание 31. Вывести всех членов семьи с фамилией Quincey
```sql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE "%Quincey";
```
### Задание 32. Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону
```sql
SELECT FLOOR(AVG(TIMESTAMPDIFF(YEAR, birthday, CURDATE()))) AS age
FROM FamilyMembers;
```
### Задание 33. Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры
```sql
SELECT AVG(unit_price) AS cost
FROM Payments
WHERE good IN(
		SELECT good_id
		FROM Goods
		WHERE good_name LIKE "%caviar"
	);
```
### Задание 34. Сколько всего 10-ых классов
```sql
SELECT COUNT(*) AS count
FROM Class
WHERE name LIKE "10%";
```
### Задание 35. Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?
```sql
SELECT COUNT(DISTINCT classroom) AS count
FROM Schedule
WHERE date LIKE "2019-09-02%";
```
### Задание 36. Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?
```sql
SELECT *
FROM Student
WHERE address LIKE "ul. Pushkina%";
```
### Задание 37. Сколько лет самому молодому обучающемуся ?
```sql
SELECT TIMESTAMPDIFF(YEAR, birthday, CURDATE()) AS year
FROM Student
ORDER BY birthday DESC
LIMIT 1;
```
### Задание 38. Сколько учениц с именем Анна (Anna) учится в школе?
```sql
SELECT COUNT(*) AS count
FROM Student
WHERE Student.first_name = "Anna";
```
### Задание 39. Сколько обучающихся в 10 B классе ?
```sql
SELECT COUNT(*) AS count
FROM Student_in_class
	JOIN CLass ON Student_in_class.class = Class.id
WHERE Class.name = "10 B";
```
### Задание 40. Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такой фамилией.
```sql
SELECT name AS subjects
FROM Subject
	JOIN Schedule ON Subject.id = Schedule.subject
	JOIN Teacher ON Schedule.teacher = Teacher.id
WHERE last_name = "Romashkin"
	AND first_name LIKE "P%"
	AND middle_name LIKE "P%";
```
### Задание 41. Выясните, во сколько по расписанию начинается четвёртое занятие.
```sql
SELECT DISTINCT start_pair
FROM Timepair
	JOIN Schedule ON Timepair.id = Schedule.number_pair
WHERE Schedule.number_pair = 4;
```
### Задание 42. Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?
```sql
SELECT DISTINCT TIMEDIFF(
		(
			SELECT end_pair
			FROM Timepair
			WHERE id = 4
		),
		(
			SELECT start_pair
			FROM Timepair
			WHERE id = 2
		)
	) AS time
FROM Timepair;
```
### Задание 43. Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке.
```sql
SELECT last_name
FROM Teacher
	JOIN Schedule ON Teacher.id = Schedule.teacher
	JOIN Subject ON Schedule.subject = Subject.id
WHERE Subject.name = "Physical Culture"
GROUP BY Teacher.last_name
ORDER BY Teacher.last_name ASC;
```
### Задание 44. Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW().
```sql
SELECT MAX(TIMESTAMPDIFF(YEAR, birthday, NOW())) AS max_year
FROM Student
	JOIN Student_in_class ON Student.id = Student_in_class.student
	JOIN Class ON Student_in_class.class = Class.id
WHERE name LIKE "10%";
```
