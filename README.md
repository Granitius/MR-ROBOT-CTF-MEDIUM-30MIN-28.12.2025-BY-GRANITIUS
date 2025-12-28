# MR-ROBOT-CTF-MEDIUM-30MIN-28.12.2025-BY-GRANITIUS

<img width="671" height="413" alt="image" src="https://github.com/user-attachments/assets/5b5d8ab9-0089-4420-8f59-7cc7808b75e3" />

Подключаемся к VPN Туннелю, IP адрес 
`10.67.176.23`

1. Проведем **Nmap** сканирование с флагами `-sC -sV -Pn` для выявления открытых портов
 
   <img width="243" height="64" alt="image" src="https://github.com/user-attachments/assets/4e0a5ca6-a3e8-4abe-b1d7-2f59ae02d500" />
   
   Видим что 80 HTTP порт открыт, посмотрев сайт я не обнаружил ничего интересного,
попробуем провести атаку директорий через утилиту gobuster

Из интересных доменов видим
`robots.txt` `readme.txt` `wp-login license`

Исходный текст **robots.txt**
`User-agent: * `
`fsocity.dic`
`key-1-of-3.txt`

Вот и первый ключ

<img width="518" height="84" alt="image" src="https://github.com/user-attachments/assets/a016e8c1-2ccf-49a8-8123-a58ff0813b39" />

`073403c8a58a1f80d943455fb30724b9`

2. Открыв **fsocity.dic** предположим что это журнал с паролями для брутфорса,
скачаем его через curl для дальнейшего пользования

Откроем **license** и просмотрим код страницы

<img width="677" height="81" alt="image" src="https://github.com/user-attachments/assets/17d44cb7-3e27-4887-bd6b-0421d8bbb68e" />

Похоже на **base64**

<img width="673" height="395" alt="image" src="https://github.com/user-attachments/assets/214225f4-6d4f-41da-a62a-07d42f79a939" />    

`elliot:ER28-0652`
Теперь перейдем к **WordPress**
Используем найденные креды

<img width="426" height="457" alt="image" src="https://github.com/user-attachments/assets/99ad5585-df93-43df-a5a1-542a6e4859d9" />

Зайдем в кастомизацию тем и увидим там файл 404.php
Вместо него поставим реверс шел от **PentestMonkey**

<img width="634" height="606" alt="image" src="https://github.com/user-attachments/assets/ced9442e-2992-4912-b2f9-fbf507b7be5c" />

Запускаем слушатель **netcat**
`nc -lvnp 9002`
 в терминале и получим доступ к системе после активации реверс шелла

 <img width="679" height="121" alt="image" src="https://github.com/user-attachments/assets/bcd9e89c-92c8-49e2-bf79-a204abeef87c" />
 
пишем `python3 -c 'import pty;pty.spawn("/bin/bash")'` для получения более стабильной и функциональной TTY-оболочки

Проверяем базовые команды
`cd home`
`ls`
`cd robot`
`ls`

Находим эти два файла

<img width="494" height="43" alt="image" src="https://github.com/user-attachments/assets/4ae79dbf-ed3f-4cb6-b703-0bb5c5309c19" />

Как мы видим мы не можем получить сразу доступ ко второму ключу, но дан второй файл, который скорее всего является хешем с алгоритмом **md5**
`c3fcd3d76192e4007dfb496cca67e13b`
Чтобы не терять времени найдем его сразу через **crackstation.net** (датабаза с уже взломанными хешами)

<img width="698" height="62" alt="image" src="https://github.com/user-attachments/assets/e90d1d8c-1942-4e95-a895-37fa3133aea3" />

Входим в пользователя **robot**

<img width="364" height="134" alt="image" src="https://github.com/user-attachments/assets/079e3e99-d9f4-49d8-b0f7-0ef30cc438cc" />

Второй ключ

<img width="261" height="117" alt="image" src="https://github.com/user-attachments/assets/06bd8245-82de-46f6-a363-7eabf372f293" />

`822c73956184f694993bede3eb39f959`

3. Найдем и третий ключ,
пользователь robot не является суперюзером, поэтому я найду решение как узнать какие сервисы можно запустить с рут правами
`find / -type f -perm -4000 -ls 2>/dev/null`

<img width="682" height="203" alt="image" src="https://github.com/user-attachments/assets/bb7b222f-e75c-451c-b890-9c1f433f6ce1" />

Среди всего этого видим **/usr/local/bin/nmap**

Найдем уязвимость на **GTFOBins**

<img width="445" height="111" alt="image" src="https://github.com/user-attachments/assets/0a0b0ba2-028e-4926-8f05-a555f86f8313" />

Попробуем это

img width="453" height="111" alt="image" src="https://github.com/user-attachments/assets/20b614d8-ffb7-4950-8ab5-00fb39ebf03b" />

Попробуем зайти в /root директорию

<img width="385" height="201" alt="image" src="https://github.com/user-attachments/assets/f32b116b-3e0d-4072-abf2-0a26837cef40" />

`04787ddef27c3dee1ee161b21670b4e4`
Райтап сделан: https://github.com/Granitius























