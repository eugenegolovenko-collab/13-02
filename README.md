# Домашнее задание к занятию "`Защита хоста`" - `Евгений Головенко`
------

### Задание 1

1. Установите **eCryptfs**.
2. Добавьте пользователя cryptouser.
3. Зашифруйте домашний каталог пользователя с помощью eCryptfs.


*В качестве ответа  пришлите снимки экрана домашнего каталога пользователя с исходными и зашифрованными данными.*  

### Решение 1

1.1. Установка **eCryptfs**

```cmd
sudo apt update
sudo apt install -y ecryptfs-utils
which ecryptfs-migrate-home
```

- обновляем индексы;
- устанавливаем eCryptfs;
- проверяем установку.

Результат:

<img width="756" height="685" alt="eCryptfs_1" src="https://github.com/user-attachments/assets/c89f3dd6-b5ec-410f-81b4-9c25054e05c3" />

1.2. Создание пользователя `cryptouser`

```cmd
sudo adduser cryptouser
```

Результат:

<img width="773" height="359" alt="eCryptfs_2" src="https://github.com/user-attachments/assets/a9f47376-9491-46b8-b53e-d031c70851fa" />

1.3. Шифрование домашнего каталога пользователя `cryptouser`

- Проверка каталога до шифрования:
  
```cmd
sudo ls -la /home/cryptouser
```

<img width="699" height="139" alt="eCryptfs_3" src="https://github.com/user-attachments/assets/55ab727f-3c5c-48d8-a168-01152470cfff" />

Создаем рандомные данные:

```cmd
sudo -u cryptouser bash
cd ~
mkdir secret
echo "Тут секретов нет!" > secret/secret.txt
echo "Пароль от рута = cjhjrnsczxj,tpmzyd;jgeceyekb,fyfy" > secret/passwords.txt
exit
```
После создания файлов проверяем, что каталог с неким содержимым создан. Проверяем, что пользователь `cryptouser` нигде не залогинен во избежании потери его данных.

<img width="831" height="310" alt="eCryptfs_4" src="https://github.com/user-attachments/assets/0ec6c53e-9882-41c1-9a21-3a7765f7db2a" />

Запускаем шифрование домашнего каталога `cryptouser`:

```cmd
sudo ecryptfs-migrate-home -u cryptouser
```
<img width="928" height="954" alt="eCryptfs_5" src="https://github.com/user-attachments/assets/62bf44ff-33a2-4c3b-821c-d6c7b41bf232" />

Проверяем:

<img width="707" height="160" alt="eCryptfs_6" src="https://github.com/user-attachments/assets/c3ec2b28-eea3-4f5e-be57-30801847760b" />

Бэкап создан. Он создаётся eCryptfs как резервная копия незашифрованных данных. Он не зашифрован и доступен администратору.
После проверки корректности шифрования данный каталог должен быть удалён вручную, иначе конфиденциальность данных не обеспечивается.

Проверяем зашифрованный каталог пользователя. 

<img width="749" height="192" alt="eCryptfs_8" src="https://github.com/user-attachments/assets/ea07171b-d335-41ec-9719-5a1f7cf575cc" />

Если все нормально, удаляем бэкап.

<img width="761" height="173" alt="eCryptfs_7" src="https://github.com/user-attachments/assets/517e0127-c2f9-4335-851f-ef4649d59b97" />

Это важно, потому что:
- если забыть удалить backup, то при краже диска данные в открытом виде так же будут утрачены;
- эти данные также доступны под правами администратора, то есть имеет место инсайдерская угроза.

---

### Задание 2

1. Установите поддержку **LUKS**.
2. Создайте небольшой раздел, например, 100 Мб.
3. Зашифруйте созданный раздел с помощью LUKS.

*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*

### Решение 2

2.1. Установка поддержки **LUKS**

<img width="868" height="616" alt="Luks_1" src="https://github.com/user-attachments/assets/48cb5950-4fa4-4c81-a9bf-4a351f2a3157" />

2.2. - 2.3. Создание небольшого раздела в 100 Мб и его шифрование.

В VirtualBox был создан диск на 100 Мб. Проверяем, как его видит система и если все нормально, то готовим раздел (luksFormat):

<img width="933" height="633" alt="Luks_2" src="https://github.com/user-attachments/assets/9b82fb8e-bf4d-4f15-8f50-968a8e3f959d" />

Монтируем раздел:

<img width="631" height="105" alt="Luks_3" src="https://github.com/user-attachments/assets/c8af65a4-1c0b-4bae-a5fe-002b84abf7b3" />

Форматируем раздел:

<img width="816" height="257" alt="Luks_4" src="https://github.com/user-attachments/assets/abc61889-fcd7-4daa-bcde-37599563b5bf" />

Монтируем раздел:

<img width="850" height="481" alt="Luks_5" src="https://github.com/user-attachments/assets/a819056b-e016-4476-8957-287866bd5917" />

Проверяем работоспособность полученного. Смотрим, что на диске, создаем файл и читаем его:

<img width="951" height="141" alt="Luks_6" src="https://github.com/user-attachments/assets/491455d9-72e6-4359-a4d5-41dd0b0e4ff4" />

Прибираемся за собой:

<img width="919" height="547" alt="Luks_7" src="https://github.com/user-attachments/assets/f216224e-7c09-40da-bd01-9ed656e91ba9" />

---

### Задание 3 *

1. Установите **apparmor**.
2. Повторите эксперимент, указанный в лекции.
3. Отключите (удалите) apparmor.

*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*

### Решение 3*

3.1.* Проверяем, жив ли AppArmor:

<img width="968" height="260" alt="aa_1" src="https://github.com/user-attachments/assets/d1694cf5-5ecb-4912-9832-6a434e596a73" />

Жив. Устанавливать не нужно. В эксперименте на лекции жертвой был `man`. У меня он `enforced`.

Ну ок, пробуем. План такой:

- cоздаём временную копию оригинала `sudo cp /usr/bin/man /usr/bin/man.bak`;
- закрываем все процессы `man` - `sudo pkill -9 man`;
- подменяем бинарник `sudo cp /bin/ping /usr/bin/man`;
- проверяем `man 127.0.0.1`.

<img width="1282" height="225" alt="aa_3" src="https://github.com/user-attachments/assets/f8952fc5-60e1-4a64-9827-c5f16f22c91e" />

Чтож, разбираем лог... 

- apparmor="DENIED" -> операция запрещена
- operation="create" -> процесс пытался создать объект
- class="net" -> контролируется сетевой стек ядра, а не файловая система
- profile="/usr/bin/man" -> ограничение применяется к пути бинарника, а не к содержимому. Если `man` подменён на `ping`, профиль остаётся `/usr/bin/man`
- pid=6730 -> ID процесса, который вызвал операцию
- comm="man" -> имя процесса
- family="inet" -> процесс пытался создать сокет для IPv4
- sock_type="dgram" -> тип сокета: datagram (UDP)
- protocol=1 -> протокол сетевого сокета, 1 это ICMP (для IPv4). Т.е. процесс пытался послать ICMP-пакет через UDP-подобный сокет, поведение `ping`.
- requested="create", denied="create" -> процесс запросил создание сокета, AppArmor отказал на уровне MAC.

Ок, делаем `man` `unenforced`...

<img width="682" height="141" alt="aa_4" src="https://github.com/user-attachments/assets/4d859f47-9209-4f29-824b-8d208a9d84fd" />

Не помогло... Вдумчивое чтение документации привело к следующему выводу: 

Ошибка теперь от ядра Linux и capabilities, а не AppArmor и именно поэтому сообщение теперь “Operation not permitted”, а не “Permission denied” от AppArmor.

Raw sockets требуют `cap_net_raw+p`... Чтож, ок. Пробуем `sudo setcap cap_net_raw+p /usr/bin/man`.

<img width="661" height="191" alt="aa_5" src="https://github.com/user-attachments/assets/99798dd1-58fc-4570-80a7-e100138f37b1" />

Снова возвращаем `man` `enforced`

<img width="838" height="276" alt="aa_6" src="https://github.com/user-attachments/assets/972511ef-38ba-41cc-889c-f70060b4c6f1" />

И видим “Permission denied” от AppArmor.

Подчищаем за собой:

<img width="731" height="90" alt="aa_7" src="https://github.com/user-attachments/assets/451179cf-3692-40f1-a177-714c87643d7a" />

---
