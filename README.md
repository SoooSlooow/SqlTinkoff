# SQL запросы
Приняты следующие наименования таблиц:
* *Pilot* - пилоты; 
* *PLane* - самолеты; 
* *Flight* - рейсы. 

Задача 1. Напишите запрос, который выведет пилотов, которые в качестве второго пилота в
августе этого года трижды ездили в аэропорт Шереметьево. 

```mysql
SELECT p.id, p.name
    FROM Pilot p
    INNER JOIN Flight f
        ON p.pilot_id = f.second_pilot_id
    WHERE f.destination = 'Sheremetyevo'
        AND f.flight_dt BETWEEN '2022-08-01' AND '2022-08-31'
    GROUP BY p.id
    HAVING COUNT(p.id) = 3;
 ```
    
Задача 2. Выведите пилотов старше 45 лет, совершали полеты на самолетах с количеством
пассажиров больше 30.

```mysql
SELECT DISTINCT p.id, p.name
    FROM Pilot p
    INNER JOIN Flight f
        ON p.pilot_id = f.first_pilot_id
            OR p.pilot_id = f.second_pilot_id
    INNER JOIN Plane pl
        ON pl.plane_id = f.plane_id
    WHERE pl.cargo_flg = 0
        AND fl.quantity > 30
        AND p.age > 45;
```

Задача 3. Выведите ТОП 10 пилотов-капитанов (first_pilot_id), которые совершили наибольшее
число грузовых перелетов в этом году.

```mysql
SELECT p.id, p.name, COUNT(p.id) cnt
    FROM Pilot p
    INNER JOIN Flight f
        ON p.pilot_id = f.first_p	ilot_id
    INNER JOIN Plane p
        ON pl.plane_id = f.plane_id
    WHERE pl.cargo_flg = 1
    GROUP BY p.id
    ORDER BY cnt DESC
    LIMIT 10;
```
