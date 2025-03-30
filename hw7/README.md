**Запрос** 
```
explain analyze
WITH all_place AS (
    SELECT count(s.id) as all_place, s.fkbus as fkbus
    FROM book.seat s
    group by s.fkbus
),
order_place AS (
    SELECT count(t.id) as order_place, t.fkride
    FROM book.tickets t
    group by t.fkride
)
SELECT r.id, r.startdate as depart_date, bs.city || ', ' || bs.name as busstation,  
      t.order_place, st.all_place
FROM book.ride r
JOIN book.schedule as s
      on r.fkschedule = s.id
JOIN book.busroute br
      on s.fkroute = br.id
JOIN book.busstation bs
      on br.fkbusstationfrom = bs.id
JOIN order_place t
      on t.fkride = r.id
JOIN all_place st
      on r.fkbus = st.fkbus
GROUP BY r.id, r.startdate, bs.city || ', ' || bs.name, t.order_place,st.all_place
ORDER BY r.startdate
limit 20;
```
## До индексов
```
Planning Time: 1.811 ms
 JIT:
   Functions: 79
   Options: Inlining false, Optimization false, Expressions true, Deforming true
   Timing: Generation 7.332 ms (Deform 1.174 ms), Inlining 0.000 ms, Optimization 2.002 ms, Emission 42.143 ms, Total 51.478 ms
 Execution Time: 1427.284 ms
(57 rows)
```
## Индекс на fkschedule
`CREATE INDEX idx_ride_fkschedule ON book.ride (fkschedule);`
```
Planning Time: 0.931 ms
 JIT:
   Functions: 79
   Options: Inlining false, Optimization false, Expressions true, Deforming true
   Timing: Generation 3.332 ms (Deform 1.223 ms), Inlining 0.000 ms, Optimization 2.136 ms, Emission 49.448 ms, Total 54.917 ms
 Execution Time: 1520.882 ms
(57 rows)
```
## + Индекс на fkroute
`CREATE INDEX idx_schedule_fkroute ON book.schedule (fkroute);`
```
Planning Time: 1.157 ms
 JIT:
   Functions: 79
   Options: Inlining false, Optimization false, Expressions true, Deforming true
   Timing: Generation 3.819 ms (Deform 1.443 ms), Inlining 0.000 ms, Optimization 2.034 ms, Emission 39.841 ms, Total 45.694 ms
 Execution Time: 1466.781 ms
(57 rows)
```
## + Индекс на fkbusstationfrom
`CREATE INDEX idx_busroute_fkbusstationfrom ON book.busroute (fkbusstationfrom);`
```
Planning Time: 1.023 ms
 JIT:
   Functions: 79
   Options: Inlining false, Optimization false, Expressions true, Deforming true
   Timing: Generation 3.339 ms (Deform 1.197 ms), Inlining 0.000 ms, Optimization 6.778 ms, Emission 47.076 ms, Total 57.194 ms
 Execution Time: 1478.531 ms
(57 rows)
```
## + Индекс на fkride
`CREATE INDEX idx_tickets_fkride ON book.tickets (fkride);`
```
Planning Time: 0.890 ms
 JIT:
   Functions: 79
   Options: Inlining false, Optimization false, Expressions true, Deforming true
   Timing: Generation 4.533 ms (Deform 1.223 ms), Inlining 0.000 ms, Optimization 6.073 ms, Emission 48.203 ms, Total 58.809 ms
 Execution Time: 1442.567 ms
(57 rows)
```
## + Индекс на fkbus
`CREATE INDEX idx_ride_fkbus ON book.ride (fkbus);`
```
Planning Time: 0.979 ms
 JIT:
   Functions: 79
   Options: Inlining false, Optimization false, Expressions true, Deforming true
   Timing: Generation 3.107 ms (Deform 1.123 ms), Inlining 0.000 ms, Optimization 1.813 ms, Emission 35.739 ms, Total 40.659 ms
 Execution Time: 1419.324 ms
(57 rows)
```
## + Индекс на seat fkbus
`CREATE INDEX idx_seat_fkbus ON book.seat (fkbus);`
```
Planning Time: 0.973 ms
 JIT:
   Functions: 79
   Options: Inlining false, Optimization false, Expressions true, Deforming true
   Timing: Generation 3.846 ms (Deform 1.192 ms), Inlining 0.000 ms, Optimization 2.009 ms, Emission 43.141 ms, Total 48.997 ms
 Execution Time: 1391.766 ms
(57 rows)
```

Добавление индексов немного ускорило выполнение запроса (с ~1427 мс до ~1391 мс), но прирост производительности оказался незначительным.
