# Лабораторная работа 1
## Начало работы с MySQL. MySQL Workbench
### Задание 1

#### Раздел «Instance» («Экземпляр БД»)

Этот раздел предназначен для управления конкретным экземпляром сервера MySQL. Здесь выполняются действия, связанные с его запуском, остановкой и настройкой.

#### Startup / Shutdown

В данном подразделе можно запустить сервер MySQL, остановить его или перезапустить. Также отображается текущее состояние службы (запущена или остановлена). В некоторых случаях можно настроить автоматический запуск сервера при включении операционной системы.

#### Options File

Подраздел предназначен для работы с конфигурационным файлом сервера (my.ini или my.cnf). Все параметры сгруппированы по разделам — память, сеть, кэширование, безопасность и другие. Через графический интерфейс можно изменять настройки без ручного редактирования файла. После изменения параметров они сохраняются в конфигурационный файл.

#### Environment

Здесь отображается информация о среде работы сервера: версия MySQL, используемая операционная система, пути к системным каталогам и файлам базы данных. Этот раздел помогает проверить текущую конфигурацию и расположение файлов.

#### Раздел «Performance» («Производительность»)

Этот раздел предназначен для анализа работы сервера и оценки его производительности. Он позволяет отслеживать нагрузку и выявлять возможные проблемы.

#### Dashboard

Отображает основные показатели работы сервера в виде графиков. Можно увидеть загрузку процессора, использование оперативной памяти, количество подключений и выполняемых запросов, а также нагрузку на диск. Информация обновляется в реальном времени.

#### Performance Reports

Содержит отчёты о работе сервера. Здесь можно посмотреть статистику по медленным запросам, блокировкам таблиц, использованию индексов, активным соединениям и операциям ввода-вывода. Эти данные помогают определить узкие места в работе базы данных.

#### Performance Schema Setup

Предназначен для настройки сбора статистики. В этом подразделе можно включать или отключать определённые параметры мониторинга и управлять глубиной анализа производительности.

### Задание 3
```
CREATE TABLE `simpledb`.`users` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `email` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `email_UNIQUE` (`email` ASC) VISIBLE);
```
### Задание 4
Создание пользователей:
```
INSERT INTO `simpledb`.`users` (`name`, `email`) VALUES ('Emin', 'armenia@mail.ru');
INSERT INTO `simpledb`.`users` (`name`, `email`) VALUES ('Gosha', 'vesk228@mail.ru');
INSERT INTO `simpledb`.`users` (`name`, `email`) VALUES ('Dima', 'klazax@mail.ru');
```
Изменение пользователей:
```
UPDATE `simpledb`.`users` SET `email` = 'kazax@mail.ru' WHERE (`id` = '3');
```
### Задание 5
```
ALTER TABLE `simpledb`.`users` 
ADD COLUMN `gender` ENUM('M', 'F') NULL AFTER `email`,
ADD COLUMN `bday` DATE NULL AFTER `gender`,
ADD COLUMN `postal_code` VARCHAR(10) NULL AFTER `bday`,
ADD COLUMN `raiting` FLOAT NULL AFTER `postal_code`,
ADD COLUMN `created` TIMESTAMP NULL AFTER `raiting`,
CHANGE COLUMN `name` `name` VARCHAR(50) NOT NULL ,
CHANGE COLUMN `email` `email` VARCHAR(50) NOT NULL ;
```
Что означает тип поля created TIMESTAMP DEFAULT CURRENT_TIMESTAMP()?

Это означает, что поле имеет тип TIMESTAMP (дата и время), и если при добавлении записи значение для него не указано, то автоматически будет подставлена текущая дата и текущее время сервера в момент вставки строки.
Полями со значениями NULL могут быть все, за исключением name, id, email. Так как они нужны для идентификации пользователей.


### Задание 6
```
UPDATE `simpledb`.`users` SET `gender` = 'M', `bday` = '2000-02-11', `postal_code` = '15467', `rating` = '4' WHERE (`id` = '1');
UPDATE `simpledb`.`users` SET `gender` = 'M', `bday` = '2000-02-11', `postal_code` = '54375', `rating` = '2' WHERE (`id` = '2');
UPDATE `simpledb`.`users` SET `gender` = 'M', `bday` = '1998-08-12', `postal_code` = '456789', `rating` = '5' WHERE (`id` = '3');
```
### Задание 7
```
id,name,email,gender,bday,postal_code,rating,created
1,Emin,armenia@mail.ru,M,2000-02-11,15467,4,"2020-03-24 00:00:00"
2,Gosha,vesk228@mail.ru,M,2000-02-11,54375,2,"2020-03-24 00:00:00"
3,Dima,kazax@mail.ru,M,1998-08-12,456789,5,"2020-03-24 00:00:00"
4,Ekaterina,ekaterina.petrova@outlook.com,F,2000-02-11,145789,1.123,"2020-03-24 00:00:00"
5,Paul,paul@superpochta.ru,M,1998-08-12,123789,1,"2020-03-24 00:00:00"
```
### Задание 8
```
CREATE TABLE `simpledb`.`resume` (
  `resumeid` INT NOT NULL AUTO_INCREMENT,
  `userid` INT NOT NULL,
  `title` VARCHAR(100) NOT NULL,
  `skills` TEXT NULL,
  `created` TIMESTAMP NULL,
  PRIMARY KEY (`resumeid`),
  INDEX `userid_idx` (`userid` ASC) VISIBLE,
  CONSTRAINT `userid`
    FOREIGN KEY (`userid`)
    REFERENCES `simpledb`.`users` (`id`)
    ON DELETE CASCADE
    ON UPDATE CASCADE);
```
При удалении связанных пользователей из таблицы users пользовательудалится из обоих таблиц.

### Задание 9
```
resumeid,userid,title,skills,created
1,1,qe,programming,"2020-03-24 00:00:00"
2,2,bfd,skating,"2020-03-24 00:00:00"
3,3,zsasd,basket,"2020-03-24 00:00:00"
```
Добавление пользователя с несуществующим id:
```
INSERT INTO `simpledb`.`resume` (`userid`) VALUES ('132');
 ```
<img width="974" height="741" alt="image" src="https://github.com/user-attachments/assets/b9f469fe-fed9-49eb-951c-4ab667ee74c0" />

Добавить пользователя с несуществующим id невозиожно, так как поле userid является внешним ключом, ссылающимся на поле id в таблице users. Поскольку в таблице users отсутствует запись с гнсуществующим идентификатором , ограничение ссылочной целостности блокирует выполнение операции.



### Задание 10:

Удаление пользователя:
```
DELETE FROM `simpledb`.`users` WHERE (`id` = '3');
```
Из-за того что таблицы связаны в CASCADE, то при удалении или изменении в 1 из таблиц, изменяемые данные удалятся или изменятся в обоих таблицах.
