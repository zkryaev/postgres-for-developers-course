1. Создать таблицу accounts(id integer, amount numeric); </br>
2. Добавить несколько записей и подключившись через 2 терминала добиться ситуации взаимоблокировки (deadlock). </br>
      `
        postgres=# TABLE accounts;
         id | amount 
        ----+--------
          1 | 200.00
          2 | 300.00
          3 | 400.00
        (3 rows)
     `
4. Посмотреть логи и убедиться, что информация о дедлоке туда попала. </br>
  `
  2025-03-20 16:11:51.969 MSK [1028] postgres@postgres STATEMENT:  UPDATE acoounts SET amount = 380 WHERE id = 2;
  2025-03-20 16:12:52.465 MSK [1120] postgres@postgres ERROR:  '**deadlock detected**'
  2025-03-20 16:12:52.465 MSK [1120] postgres@postgres DETAIL:  Process 1120 waits for ShareLock on transaction 1038; blocked by process 1028.
          Process 1028 waits for ShareLock on transaction 1037; blocked by process 1120.
          Process 1120: UPDATE accounts SET amount = 100 WHERE id = 1;
          Process 1028: UPDATE accounts SET amount = 380 WHERE id = 2;
  2025-03-20 16:12:52.465 MSK [1120] postgres@postgres HINT:  See server log for query details.
  2025-03-20 16:12:52.465 MSK [1120] postgres@postgres CONTEXT:  while updating tuple (0,1) in relation "accounts"
  2025-03-20 16:12:52.465 MSK [1120] postgres@postgres STATEMENT:  UPDATE accounts SET amount = 100 WHERE id = 1;
  `
