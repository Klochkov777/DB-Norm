# data base 1
<<<<<<< HEAD
=======
# Домашнее задание к занятию "`sdb-homeworks`" - `Klochkov Vladimir`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

[link to resolve]()
Синтаксически правильность написания команд не проверял и не запускал на бд. Возможны где то ошибки. Но на данном уровне в задание этого не требуется, я просто решил максимально приближенно к какой то практике слету сделать задачу.


Решил разделить в таблице так:
у департамента (department) есть отделы (division) и это отношение один ко многим,
аналогично у отдела есть группы (groups).


create table groups (
group_id bigserial primary key,
name varchar(100),
division_id biginteger,
address_id biginteger,
constrant fk_division
	foreign key (division_id)
	references (divsions),
constrant fk_address
	foreign key (address_id)
	references (addresses)
);

create table divisions (
division_id bigserial primary key,
name varchar(100),
department_id biginteger,
address_id biginteger,
constrant fk_department
	foreign key (department_id)
	references (departments),
constrant fk_address
	foreign key (address_id)
	references (addresses)
);

create table departments (
department_id bigserial primary key,
name varchar(100),
department_id biginteger,
address_id biginteger,
constrant fk_department
	foreign key (department_id)
	references (departments),
constrant fk_address
	foreign key (address_id)
	references (addresses)
);

Далее решил что не имеет смысла отделять от сотрудника его зарплату (если бы была в этом плане более сложная структура например тариф, различные виды надбавок, то это бы уже имело смысл) и его дату найма. Также здесь можно видеть отношение многие к одному это к должности (job_title), также каждый сотрудник должен принадлежать какому то департаменту, но он может не принадлежать отделу поэтому будет null (это если он руководитель департамента), аналогично с группами. Здесь я думаю это оправдано, потому что руководителей не так много и это нашу базу данных не загрузит, все аналогично и с группами.

create table employees (
employee_id bigserial primary key,
name varchar(100),
date_employ timestamp,
salary numeric,
job_title_id biginteger,
department_id biginteger,
division_id biginteger nullable,
group_id biginteger nullable,
constrant fk_job_title
	foreign key (job_title_id)
	references (job_titles),
constrant fk_department
	foreign key (department_id)
	references (departments),
constrant fk_division
	foreign key (division_id)
	references (divisions),
constrant fk_group
	foreign key (group_id)
	references (groups)
);

Здесь видим перечисление должностей и отношение один ко многим. Фактически это enumeration. Я сделал их уникальными

create table job_titles (
job_title_id bigserial primary key,
name varchar(100) unique,
);

Здесь поделил адрес на несколько таблиц (хотя возможно это излишне). В программировании работал где адрес так не делился. При этом для связи с департаментами, группами и отделами сделал дополнительную таблицу addresses.

create table regions (
region_id bigserial primary key,
name varchar(100),
);





create table cities (
city_id bigserial primary key,
name varchar(100),
region_id biginteger,
constrant fk_region
	foreign key (region_id)
	references (regions)
);

create table streets (
street_id bigserial primary key,
name varchar(100),
city_id biginteger,
constrant fk_city
	foreign key (city_id)
	references (cities)
);

create table houses (
house_id bigserial primary key,
number varchar(100),
street_id biginteger,
constrant fk_street
	foreign key (street_id)
	references (streets)
);

create table addresses (
address_id bigserial primary key,
region_id biginteger,
city_id biginteger,
street_id biginteger,
house_id biginteger,
constrant fk_region
	foreign key (region_id)
	references (regions),
constrant fk_region
	foreign key (city_id)
	references (cities),
constrant fk_region
	foreign key (street_id)
	references (streets),
constrant fk_region
	foreign key (house_id)
	references (houses)
);

далее перечисление проектов

create table projects (
project_id  bigserial primary key,
name varchar(255)
);

таблица для соединения многие ко многим, тк один человек может работать над несколькими проектами, а также несколько человек может работать над одним проектом.

create table progects_emploees (
project_mployee_id bigserial  primary key,
project_id biginteger,
employee_id biginteger,
constrant fk_projects
	foreign key (project_id)
	references (projects),
constrant fk_employees
	foreign key (employee_id)
	references (employees),
);

