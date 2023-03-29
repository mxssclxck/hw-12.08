# 12.8. «Резервное копирование баз данных» НиконоровДА - FOPS-6

## Задание 1. Резервное копирование

Кейс
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.

Необходимо описать, какие варианты резервного копирования подходят в случаях:

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.

1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.

1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.

Приведите ответ в свободной форме.

```
1.1. Для резервного копирования данных, которые необходимо восстановить в полном объёме за предыдущий день, можно использовать ежедневное полное копирование, когда каждый день создается полная копия всех данных. Также можно использовать инкрементальное или дифференциальное копирование, которые сохранят только измененные данные за предыдущий день. 

1.2. Для восстановления данных за час до предполагаемой поломки необходимо использовать частые резервные копии, когда новые копии создаются каждый час. Для этого можно использовать одну из следующих стратегий:
 - Ежечасное полное копирование
 - Ежечасное инкрементальное или дифференциальное копирование
 - Использование резервного копирования в реальном времени.

1.3. Для случая, когда происходит моментальное переключение на работающую базу данных, можно использовать методы репликации или клонирования данных. Репликация создает точную копию базы данных на другом сервере, который может быть использован в случае поломки. Клонирование позволяет создать точную копию базы данных на том же сервере и переключиться на нее в случае поломки основной базы данных.
```

## Задание 2. PostgreSQL

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

2.1.* Возможно ли автоматизировать этот процесс? Если да, то как?

Приведите ответ в свободной форме.

2.1. Пример команды для создания резервной копии базы данных PostgreSQL с помощью утилиты pg_dump:

```
pg_dump -U username -h hostname -p port -Fc dbname > backup.dump
```

Где:

- `username` - имя пользователя PostgreSQL
- `hostname` - имя хоста, на котором расположена база данных
- `port` - порт, который слушает сервер баз данных PostgreSQL
- `dbname` - имя базы данных, которую необходимо скопировать
- `backup.dump` - имя файла, в который будет сохранена резервная копия.

Пример команды для восстановления базы данных из резервной копии с помощью утилиты pg_restore:

```
pg_restore -U username -h hostname -p port -d dbname backup.dump
```

Где:

- `username` - имя пользователя PostgreSQL
- `hostname` - имя хоста, на котором расположена база данных
- `port` - порт, который слушает сервер баз данных PostgreSQL
- `dbname` - имя базы данных, для которой будет восстановлена копия
- `backup.dump` - имя файла, из которого будет восстановлена резервная копия.

2.1.* Да, процесс резервного копирования и восстановления базы данных PostgreSQL можно автоматизировать с помощью скриптов и планировщика задач. Например, можно написать скрипт, который будет выполнять команды pg_dump или pg_restore, а затем запланировать его выполнение в определенное время с помощью cron на Unix-системах или Task Scheduler на Windows. Также есть готовые инструменты для автоматизации резервного копирования и восстановления, такие как решения компании PostgreSQL или сторонние приложения.

## Задание 3. MySQL
3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL.

3.1.* В каких случаях использование реплики будет давать преимущество по сравнению с обычным резервным копированием?

Приведите ответ в свободной форме.

3.1. Пример команды для создания инкрементного резервного копирования базы данных MySQL с помощью утилиты mysqldump:

```
mysqldump --user=username --password=password --host=hostname --single-transaction --add-locks --flush-logs --master-data=2 --databases dbname | gzip > backup.sql.gz
```

Где:

- `username` и `password` - учетные данные пользователя MySQL
- `hostname` - имя хоста, на котором расположена база данных
- `dbname` - имя базы данных, которую необходимо скопировать
- `backup.sql.gz` - имя файла, в который будет сохранена резервная копия.

3.1.* Использование реплики в MySQL будет давать преимущество в случаях, когда необходимо обеспечить высокую доступность базы данных. Репликация позволяет точно скопировать данные со всеми изменениями на другой сервер, что позволяет быстро переключиться на копию в случае сбоя основной базы данных. Кроме того, репликация обеспечивает возможность распределения нагрузки на несколько серверов, что снижает риск отказа системы и повышает ее производительность. Однако репликация требует дополнительных ресурсов для поддержания копии базы данных и может потребоваться настройка и поддержка дополнительного оборудования для обеспечения ее работоспособности. Обычное резервное копирование, в свою очередь, является более простым и доступным вариантом для создания копии базы данных для восстановления в случае сбоя.
