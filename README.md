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



---

### Задание 3 *

1. Установите **apparmor**.
2. Повторите эксперимент, указанный в лекции.
3. Отключите (удалите) apparmor.


*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*
