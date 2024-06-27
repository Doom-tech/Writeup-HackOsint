# **XLS Team Writeup**
Authored by: [@xsafter](t.me/xsafter), [@dontunique](t.me/dontunique), [@GorgonzolaCTF](t.me/GorgonzolaCTF)
# **WEB**

## 01. Отправная точка
**Описание**: «Здравствуйте! В сети орудует мошенник под именем **Александр Кот**, кидает людей на фейковую подписку на сервис. Город неизвестен, он явно живёт не там, где указал – по чём там рубероид он не знает.»
**Решение**:
Нас интересует мошенник с именем (псевдонимом) Александр Кот. Просто поищем в Яндексе скамера с данным именем:
![Screenshot 2024-06-26 at 18.10.11.png](https://hedgedoc.dev/api/private/media/5ba0a4578cf0a3c7a53dcbc0f56c5591.png)
![Screenshot 2024-06-25 at 19.21.33.png](https://hedgedoc.dev/api/private/media/2a69e93e598a7ca4e77256c79fe642a9.png)
Ссылка на его страницу: https://vk.com/hstmst

Последний его пост и является флагом к 1 заданию.
**Ответ**: `H0CTF{Th1S_1S_Th3_ST4R7_p01N7}`

## 02. Ого, вебсайт
**Описание**: Говорящее само за себя название, не правда ли?
**Решение**:
Также в его ВК есть пост, комментарий к которому намекает на то, что ссылку надо посмотреть в веб архиве:
![Screenshot 2024-06-25 at 19.27.47.png](https://hedgedoc.dev/api/private/media/e82bcd0497ac27125793c0783794b9ed.png)
Действительно, сама ссылка не открывается, но мы находим ее в вебархиве:
![Screenshot 2024-06-25 at 19.30.23.png](https://hedgedoc.dev/api/private/media/2426222e473701fc82bc968f9c484058.png)
https://web.archive.org/web/20240105050939/https://ibb.co/pr1NJcM

Загуглив имя пользователя и содержание этого отзыва, мы найдем сайт https://donthackme.ru/:
![Screenshot 2024-06-25 at 22.05.04.png](https://hedgedoc.dev/api/private/media/981b92ae18f889e496d2ca297e9cbfea.png)
На нем нас ждет флаг ко 2 заданию.

**Ответ**: `H0CTF{D0RKs_D0_R1gh7_Th1NGs}`

> После нахождения сайта и ВК мы пошли решать таски из Forensics, так что к ковырянию сайта мы вернулись не сразу.

## 03. Кто это написал?
**Описание**: Кто-то не на шутку разозлился...
**Решение**:
Изучив сорцы сайта, обнаруживаем файл `/js/comments.js` со ~~сломанными~~ комментариями. Бегло проглядев их, обнаруживаем обращение к API: `api/getcomments.php?per_page=5&page=${page}`. 
![Screenshot 2024-06-25 at 22.37.27.png](https://hedgedoc.dev/api/private/media/833518e1595316253fd151432f5afc2a.png)
~~Опять не обращаем внимание на кодировки~~. Хм, похоже все комментарии имеют параметр `"approved":true`.
![Screenshot 2024-06-26 at 16.25.43.png](https://hedgedoc.dev/api/private/media/2ac30a120d00720cb95cdf5fff977fa1.png)
Тогда попробуем поставить фильтрацию с аргументом `&approved=false` и видим: 
![Screenshot 2024-06-26 at 16.27.25.png](https://hedgedoc.dev/api/private/media/9fe7fab109fa650afe8c575de04a920f.png)
Перебирая комментарии обнаруживаем флаг:
![Screenshot 2024-06-26 at 16.29.59.png](https://hedgedoc.dev/api/private/media/81b13b5288b99209efe90b40378de110.png)
  **Ответ**: `H0CTF{4P1_M4y_b3_Uns4F3_T0O}`

## 04. Безобидный файлик
**Описание**: Очень злой человек опубликовал что-то не очень приятное для нашего скамера.
ВНИМАНИЕ! Здесь Вам необходимо указать название опубликованного файла.
**Решение**: 
Кроме того, в отзыве из предыдущего таска, мы можем прочитать что пользователь JVX_HACKER угрожал опубликовать wifi трафик Александроса 
![image.png](https://hedgedoc.dev/api/private/media/eb5ddf2c481aa754c5245b66164b40ce.png)
Вводим никнейм из отзыва в поиск по телеграмм-каналам и находим интересующий нас
![image.png](https://hedgedoc.dev/api/private/media/85f009999641bcc01e47f4ad81c0445d.png)
Название файла в канале и есть флаг
![image.png](https://hedgedoc.dev/api/private/media/6fd0f07f8a849a1a2bab327298008eec.png)
**Ответ**: sniffed.tar.gz

## 05. Сквозь тернии к звёздам
**Описание**: Видимо, фейсконтроль не проблема.
**Решение**: 
Открываем файл в wireshark и начинаем его анализировать. Обращаем внимание на комментарии с SSID:
![image.png](https://hedgedoc.dev/api/private/media/86de07a6f81ef025c22540eab27fb45d.png)
Есть связь с несколькими роутерами, но хочу получить их все. Заходим в "Беспроводная связь --> Трафик WLAN"

![image.png](https://hedgedoc.dev/api/private/media/30d5f4a42b7b64a9dfd981b18b547eae.png)

Видим все роутеры участвующие в передаче пакетов. Первая пришедшая идея - это расшифровать трафик. Судя по всему, в трафике есть рукопожатие от одного из роутеров. Заходим в aircrack и пытаемся побрутить с помощью rockyou
![image.png](https://hedgedoc.dev/api/private/media/d55d7f904680808dda4a919f27fac04e.png)
Видим рукопожатие на HostMaster, запускаем брут. Но по итогу пароля не нашлось в словаре, тогда переходим ко второму методу: брут по маске (как раз в это время появился соответствующий хинт).
Конвертируем файл .cap в .hccapx для hashcat и запускаем командой
```bash 
hashcat -m 2500 -a 3 sniffed.hccapx 1?d?d?d?d?d?d3
```
И получаем пароль: 11031943

Анализируя расшифрованный трафик в HTTP запросах находим страницу админа:
![image.png](https://hedgedoc.dev/api/private/media/0c258fe7d6009b70e533c94abb8e8192.png)
Заходим на страницу и видим, что нас не впускает с нашего IP адреса.
![image.png](https://hedgedoc.dev/api/private/media/0d468ccf3ee2d572924fa52804c55cba.png)

IP Александроса находим в том же расшифрованном трафике, просмотрев поток HTTP:
![image.png](https://hedgedoc.dev/api/private/media/7765cf587dd9ad0e6893526d0f81b1bc.png)
Применяя `X-Forwarded-For: 185.193.196.99`, попадаем на страницу, где нас встречает флаг:
![image.png](https://hedgedoc.dev/api/private/media/7f36371a8021bb2c36cf1198235d8e81.png)
**Ответ**: `H0CTF{X_F0Rw4Rd3d_FOR_Byp4sS}`

## 06. Не меняйте пароли

**Описание**:"Да кому ты нужен", - говорили они...
**Решение**: Проверяем robots.txt сайта Александра и обнаруживаем в нем несколько интересных записей. 
![image.png](https://hedgedoc.dev/api/private/media/5890ad90b047bf9a5ad3ef487fd09c93.png)

Один из самых интересных файлов: mail.conf

![image.png](https://hedgedoc.dev/api/private/media/bdbe959fb926a013e7dfcf0be5bd4504.png)

В файле мы видим почту для клиента, удалённый пароль и сайт, для проверки почты в базе утечек. Вспоминаем, что в трафике, расшифрованном нами, присутствовала почта gmail с паролем, но система сообщила, что эта почта найдена в базе утечек. Возможно пароль оттуда будет подходить, учитывая что мошенник любит использовать одинаковые пароли.

![Screenshot 2024-06-26 at 18.27.44.png](https://hedgedoc.dev/api/private/media/32639b52caf61b0c5818f9894dcdd150.png)

Почта: darkestpart@gmail.com
Проверяем нашу почту на том же агрегаторе утечек, который был любезно предоставлен в `mail.conf` и видим часть пароля и хэш:

![image.png](https://hedgedoc.dev/api/private/media/09ea5bb1e4a28d3ed502037a0eca241d.png)

Брутим хеш с помощью hashcat используя маску sourc?a?a?a?a?a и получаем пароль: sources00
Заходим на почту и видим письмо с флагом:
![image.png](https://hedgedoc.dev/api/private/media/bede7093e66768fdf801240aabbd6f6d.png)
**Ответ**: H0CTF{L34KeD_PWDs_Op3n_D00rS}

## 07. Где-то в дампах

**Описание**: Сложно искать черные символы в черном дампе... Особенно, когда их там нет...

**Решение**: 
Поизучаем почту. Среди многочисленного спама, находим ~~мемы от Фазана~~ какой-то файл с логами: 
![Screenshot 2024-06-26 at 16.39.13.png](https://hedgedoc.dev/api/private/media/1fa8abb88f3835c9ae2a9c0cce21c1b4.png)
Видим огромное количество записей вида: 
![Screenshot 2024-06-26 at 16.50.19.png](https://hedgedoc.dev/api/private/media/453c7022925719c1e225c85529e516ca.png)
Хм, ну раз есть "Can't start scheduled backup", значит наверное есть и Success. Поищем по этому слову и находим: 
![Screenshot 2024-06-26 at 16.52.21.png](https://hedgedoc.dev/api/private/media/e8df35c6d4b170501f52de762ba6a816.png)
Переходим по данному пути на сайте donthackme.ru и скачиваем SQL-дамп от phpMyAdmin: 
![Screenshot 2024-06-26 at 16.54.25.png](https://hedgedoc.dev/api/private/media/e3404aa3a4588a60dea0c69f942557c8.png)
Ого, да это же флаг.
**Ответ**: `H0CTF{D0_YoU_R34d_L0gs_HUH}`

## 08. Вапросав многа
**Описание**: Но я вижу, Вы нашли на них ответы.

**Решение**: 
Продолжим смотреть на дамп из предыдущего таска. Предыдущим инсертом в таблицу `admin` заносился пользователь с ником adminvpn и паролем, захэшированным с солью (salt). Существует много способов для такого хэширования, но судя по длинне хэша (32 байта) это, наиболее вероятно, MD5. Если запустить hashcat по rockyou с брутом популярными модами для хэширования с солью (10, 20), брут не дает результатов. Подбирая разные моды для такого типа хэширования (из https://hashcat.net/wiki/doku.php?id=example_hashes), находим, что с модом `3710` hashcat находит пароль - `monkey4life`.

Анализируя трафик в предыдущих заданиях мы нашли, что авторизация по email была отключена: 
![Screenshot 2024-06-26 at 17.09.47.png](https://hedgedoc.dev/api/private/media/59d12a6716b4938524d8b7eafe031037.png)
, поэтому попробуем залогиниться с помощью юзернейма `adminvpn`. 
Вернемся к админке из таска 05. Залогинимся с помощью этих кредов: 
![Screenshot 2024-06-26 at 17.13.34.png](https://hedgedoc.dev/api/private/media/af32042713a0fa75cdccb019ab14fd97.png)
Нас встречают вопросы "двухфакторной аутентификации": 
![Screenshot 2024-06-26 at 17.14.06.png](https://hedgedoc.dev/api/private/media/1621f0cb8b35f20e1c1abaffe92c50de.png)
Ответы на них мы знаем благодаря заданиям из цикла Forensics (Город, Фамилия лучшего друга, Любимая порода кошек, Любимая еда, Значение BIO Вашего рабочего TG, Страна, в которую хотите переехать). Введя ответы, мы попадаем на админку, где нас ждет флаг:
![Screenshot 2024-06-26 at 17.19.18.png](https://hedgedoc.dev/api/private/media/89cb630e0d2b125e5fe2122b9606b8b7.png)
**Ответ**: `H0CTF{T00_S7roNG_2FA_0r_NoT}`

## 09. Защита от защиты

**Описание**: А скамер-то, по всей видимости, страдает паранойей.

**Решение**: 
Попробуем зайти на страницу Upload в нашей админке (https://donthackme.ru/admin_f7Z0pjDe3LmeR1/upload.php):
![Screenshot 2024-06-26 at 17.37.06.png](https://hedgedoc.dev/api/private/media/fe4e22ef275f8f2a785ad688ef5aac9f.png)
Не тут-то было! Сначала нам требуется ввести какой-то пароль.
Вернемся к дампу из таска 07. У нас есть таблица `uploadpwd`, которая, по видимому, и хранит интересующий нас пароль для upload'а: 
![Screenshot 2024-06-26 at 17.35.22.png](https://hedgedoc.dev/api/private/media/8c644b6c70788d6e0a437887503eaca7.png)
Но никаких инсертов в эту таблицу нет.
Хорошенько подумав, мы вспоминаем про `/admin_f7Z0pjDe3LmeR1/comments.php`: 
![Screenshot 2024-06-26 at 17.44.46.png](https://hedgedoc.dev/api/private/media/2e478b0d15ff63eb45e848ed0411f047.png)
, который возвращает комментарии из (как мы увидили после получения дампа) SQL-таблицы `comments`: 
![Screenshot 2024-06-26 at 17.39.55.png](https://hedgedoc.dev/api/private/media/208a9fcdac49c786c48fb0ea7eb08112.png)
Возможно, мы можем совершить SQL-инъекцию, чтобы получить данные из таблицы `uploadpwd`. Методом проб и ошибок составляем UNION-based SQLI: `/admin_f7Z0pjDe3LmeR1/comments.php?per_page=1&page=1&approved=1%20UNION%20SELECT%20NULL,pwd,NULL,ekey%20FROM%20uploadpwd%20--` которая выдает нам pwd и ekey из интересующей нас таблицы. Выполняем ее: 
![Screenshot 2024-06-26 at 17.48.51.png](https://hedgedoc.dev/api/private/media/3304c5436ceccfe2728a7a5f80b184c7.png)
Мы получили Base64 строку, которая раскодируется как `WiiRemNGC5LMtIDEdVYj1tZc2l66gCf3iMm7gphKUwY=::iv1600XJk50QwbPa`. Слово IV (initialization vector) намекает нам на использование симметричного шифра AES-CBC. Расшифруем его в каком-нибудь онлайн-декодере предположив, что `iv1600XJk50QwbPa` это IV, `SZbunEGKNu29xx3C` (который лежит в поле дата) это ключ, а `WiiRemNGC5LMtIDEdVYj1tZc2l66gCf3iMm7gphKUwY=` это шифротекст. Получаем:
![Screenshot 2024-06-26 at 17.58.31.png](https://hedgedoc.dev/api/private/media/af0133f171932ff8a4b9d212f9085629.png)
Вводим этот пароль и видим флаг:
![Screenshot 2024-06-26 at 17.59.56.png](https://hedgedoc.dev/api/private/media/42636e505e2e126647d9184aaf96ba88.png)
**Ответ**: `H0CTF{SQL1_4nD_3NcRyP710N}`

## 10. Заметь слона

Заходим в админ панель с логином и паролем полученным ранее.
Переходим в uploads и вводим пароль полученный с помощью SQLi.

Перед нами загрузка файлов:
![image.png](https://hedgedoc.dev/api/private/media/88ea9f930e56502a17e8477cae0e0bf6.png)
Первое же, что приходит на ум, это загрузить какой-нибудь шелл для доступа к системе.
При успешной загрузке файлов нам выдаётся id залитого файла:
![image.png](https://hedgedoc.dev/api/private/media/f1b3a75736ef8d0131e8e9696734eaad.png)
Ранее мы находили `/js/file.js` где лежал путь к api поиска файлового пути
![image.png](https://hedgedoc.dev/api/private/media/a430b1248f64fa40eed3ca634503bff6.png)
Переходим по api с выданным id: ![image.png](https://hedgedoc.dev/api/private/media/f52059b159d5122d0eec92552ddebeaf.png)
И видим путь до загруженного файла, обращая внимание, что расширение обрезается, если в нём есть `.php`, что является одним из фильтров.

При попытке залить файл с php кодом, но применённым zero byte или другими способами обхода проверки на расширение, нам выдаёт ошибку, что в файле присутствует php код и загрузка не успешна.

Если поизучать страницу инфо, можно понять, что сервер загрузки файлов принимает файлы 5 MB, а учитывая, что бюджет у Кота не может быть большим для развёртывания кластера, то и его фильтры должны сломаться, при большом объёме информации.

![image.png](https://hedgedoc.dev/api/private/media/d189fda043f0ad50381c2f3fc15f240a.png)

Забиваем файл мусором и маскируем расширение: `file1.jpg.php5`:

![image.png](https://hedgedoc.dev/api/private/media/1ae1dc4a9e83b2ef0ffcd81c254dd7e4.png)

Открываем nc сервер через serveo.net и переходим по загруженному файлику. Происходит магия и мы уже внутри:

![image.png](https://hedgedoc.dev/api/private/media/e54c6022ed4f1febac4c45addb243c7e.png)

Переходим к директории файлов на веб сервере и находим флаг:

![image.png](https://hedgedoc.dev/api/private/media/a179ae0cafbc05024d34376771f2da90.png)

Читаем флаг:
![image.png](https://hedgedoc.dev/api/private/media/f764b5ca6664c9733d496de519ef0f03.png)

И прислушиваемся к комментарию автора, переименовываем наш шелл.

**Ответ**: `H0CTF{Upl04D_R3S7R1cti0Ns_BYp4SS}`

## 11. Мы дома... У кого?
**Описание**:
Мы, как бы, зашли в гости, но хозяина дома нет, да и дом думает, что мы и есть хозяева...

**Решение**: 
Осмотримся на сервере. На этом этапе не стали прокидывать шелл через bash потому что нам было лень). Проведем стандартную для PrivEsc операцию - поищем файлы с SUID-битами: 
![Screenshot 2024-06-26 at 19.23.44.png](https://hedgedoc.dev/api/private/media/11e0043d7e21825cd96390e763d90562.png)
Хм, нестандартный файл. В директории с ним лежает еще 2 файла:
![Screenshot 2024-06-26 at 19.24.29.png](https://hedgedoc.dev/api/private/media/34badcc9ae65a9c552b76ec0aad13643.png)
Прочитав их, мы понимаем, что это какая-то операция, необходимая для выполнения файла `state`:
![Screenshot 2024-06-26 at 19.25.09.png](https://hedgedoc.dev/api/private/media/b4466273259f7df833532d1132dc8ce9.png)
Очевидно, что файл, который исполняется под рутом (SUID-бит) и который мы можем изменять из-под текущего пользователя может привести к эскалации привилегий:
![Screenshot 2024-06-26 at 19.27.30.png](https://hedgedoc.dev/api/private/media/dd1a8527846aab2c2688a307b04efd05.png)
Заменим данную строчку на подходящую нам: 
![Screenshot 2024-06-26 at 19.31.04.png](https://hedgedoc.dev/api/private/media/ac2e9945d5620d0398364f7f89b70100.png)
Теперь, аналогичным образом, прочитаем флаг: 
![Screenshot 2024-06-26 at 19.34.52.png](https://hedgedoc.dev/api/private/media/6b56677ec54a6dd9af552cc74508dc50.png)
**Ответ**: `H0CTF{Lp3_t0_Us3R_sUCc3SSfuLy}`

## 12. Я есть рут!
**Описание**: Ну ты уже понял.

**Решение**:
Еще раз посмотрев содержание `/home/hostmaster` из предыдущего таска, мы находим `.bash_history`. Может быть там тоже будет что-то интересное? 
![Screenshot 2024-06-26 at 19.39.20.png](https://hedgedoc.dev/api/private/media/500da032c3025c11869fdd31eeeecbb2.png)
Действительно, там что-то очень интересное!
попробуем залогиниться на сервер под логином `hostmaster` и с данным паролем. Нам это вполне удается: 
![Screenshot 2024-06-26 at 19.40.46.png](https://hedgedoc.dev/api/private/media/2ee29b97f74bacdd99daa19273ddd993.png)
Посмотрим не менее стандартную для PrivEsc вещь - `sudo -l`:
![Screenshot 2024-06-26 at 19.41.58.png](https://hedgedoc.dev/api/private/media/cce731a3d07d8f9eadb258d5e16d170d.png)
Чудесным образом, на [GTFOBins](https://gtfobins.github.io/gtfobins/cowsay/) нас ждет способ повысить свои привелегии до root'а с помощью `cowsay`:
![Screenshot 2024-06-26 at 19.44.12.png](https://hedgedoc.dev/api/private/media/8f467c7fd126bc155c9e67fcb886d026.png)
Выполняем необходимые действия, читаем флаг из директории рута и радуемся!
**Ответ**: `H0CTF{R0oT_Pr1vS_G41n3D_m4N}`

## 13. Опять/Старт?
Проверяем другие файлы из директории рута. Находим непустой `.curl_history` и видим там ~~снова мемесы от Фазана~~ интересную ссылку на PasteBin:
![Screenshot 2024-06-26 at 19.46.34.png](https://hedgedoc.dev/api/private/media/ce111cf7f945382339283f4ce3359418.png)
Если убрать `raw` из этой ссылки, то можно понять, что файл называется `tupocurlbot.txt`:
![SCR-20240626-rgia.png](https://hedgedoc.dev/api/private/media/3ed9fc03af60c8bd5ad68d7588052034.png)
> Честно говоря вообще не понял что тут делает хинт "Признаюсь честно, я обожаю стейки. Правда, я не люблю СЫРЫЕ стейки". По-моему слово raw можно было обыграть и менее уцуцужно.

А есть ли в телеграме бот `@tupocurlbot`? Да, есть!
![Screenshot 2024-06-26 at 19.53.31.png](https://hedgedoc.dev/api/private/media/f806f112774e757813aa934395484640.png)
Если прописать старт, то он выдаст нам флаг.
**Ответ**: `H0CTF{TG_B0t_FoUNd_Bu7_Wh4T}`

## 14. Рабочий Стул
**Описание**: Интересно, как же на него попасть?
**Решение**:
Заходим в бота, ссылку которого мы получили и видим бота исполняющего функции команды curl. Отправляем ссылку на любой ресурс и получаем 2 файла - вывод с потока stdout и stderr.

![image.png](https://hedgedoc.dev/api/private/media/91c248b6b570bb8158ce5d675147bfe5.png)


![image.png](https://hedgedoc.dev/api/private/media/830e547b5341535f1ee88bcedd39fe35.png)

CURL это терминальная команда, поэтому логично бы предположить, что бот имеет в себе Command-Injection. Пошлем в него что-то типа: 
![Screenshot 2024-06-26 at 19.59.22.png](https://hedgedoc.dev/api/private/media/d239113e91ca47994020fd5b96582bc1.png)
![Screenshot 2024-06-26 at 20.02.25.png](https://hedgedoc.dev/api/private/media/1aac77079c6eedb0b895815f44db1395.png)
Похоже что-то не то. Возможно ссылка не оборачивается в кавычки?
Попробуем использовать команду "И"(&&), для получения директорий на сервере и о чудо обнаруживаем CMDi, но учитывая, что на bash команды мы получали ошибки, понимаем, что на сервере (точнее компе нашего кодера) стоит Windows:

![Screenshot 2024-06-26 at 20.05.45.png](https://hedgedoc.dev/api/private/media/c4bf8e76bdcdf240099c22d9d3cdc03f.png)

![Screenshot 2024-06-26 at 20.05.14.png](https://hedgedoc.dev/api/private/media/7151b17a87bbe9532c8158912a093100.png)

Узнаем примерное расположение директорий и пробуем прокопаться к рабочему столу, используя: `https://google.com/ && dir C:\Users\HostMaster\Desktop`
И получаем информацию, что на рабочем столе есть `FLAG.txt`

![image.png](https://hedgedoc.dev/api/private/media/581292327d80e51246220c6e0d67a926.png)
С помощью команды type читаем наш флаг: `https://google.com/ && type C:\Users\HostMaster\Desktop\FLAG.txt`
![image.png](https://hedgedoc.dev/api/private/media/46905d6514c3aacecc44bb9405f46971.png)

**Ответ**: `H0CTF{Rc3_1N_7G_B0t_1s_S7R0nG}`

# **Forensics**

## I. Чатек
**Описание**: Найдите рабочий чат "Кота" (линк/юзернейм чата, не название).
**Решение**: 
Помимо прочего, на странице в ВК Александра Кота можно заметить пост: 

![Screenshot 2024-06-25 at 23.43.43.png](https://hedgedoc.dev/api/private/media/ddf9c18c948e18d79b52903a16928e8a.png)

Поищем чат с таким названием в Телеграме:

![Screenshot 2024-06-25 at 23.45.20.png](https://hedgedoc.dev/api/private/media/569a67c3de5ae7acb9726433cc7b9dc6.png)

Действительно, такой чат (314SOS VPN) существует и ссылка является флагом.

**Ответ**: https://t.me/owicvnsdk



## I. Кошка
**Описание**: Какая любимая порода кошек у нашего "Кота"?
**Решение**: 
Изучим участников чата из предыдущего таска.

![Screenshot 2024-06-26 at 12.10.47.png](https://hedgedoc.dev/api/private/media/73620a5b34631102cb5b22904750082d.png)

Присмотримся к участнику “Сашка Котофей” (@AlexanderCatVPN). Судя по нику, это и есть “Александр Кот”. Забьём в яндекс поиск по картинкам его аватарку и получим результат:
![image.png](https://hedgedoc.dev/api/private/media/aeb10f442e4b2befa812fe8449c08ca8.png)

**Ответ**: Ликой.

<!---

## 

**Описание**:

**Решение**: 



**Ответ**: 

-->

## I. Фоточка

**Описание**: Найдите ссылку на фотографию "Кота", которую опубликовал его программист.

**Решение**: 

Для начала узнаем ID участника чата "Sad Prog" (например с помощью клиента Ayugram):

![image.png](https://hedgedoc.dev/api/private/media/ed42a7acfab2415e3efdb45f53c9ea3f.png)

Поищем сообщения от этого ID через fun stat bot (@Funstat_alive_bot):

![image.png](https://hedgedoc.dev/api/private/media/c96be6a55eb4c7f598074892ebdc490d.png)

**Ответ**: https://imgur.com/a/JJDXudt

## I. Песенка
**Описание**: Как называется песня, которую сочинил Кот для своего грустного программиста?
**Решение**: Аналогично с таском "Фоточка" проверяем участника @AlexanderCatVPN через fun stat bot:
![image.png](https://hedgedoc.dev/api/private/media/fc917b8d7f8da2265ac1d16ab8d8c367.png)
Поищем эту HEX-строку в Яндексе: 

![Screenshot 2024-06-26 at 13.37.23.png](https://hedgedoc.dev/api/private/media/098d05f76dbde64fa466308e7746e342.png)

Оказывается это ID видео на RuTube: 
![Screenshot 2024-06-26 at 13.38.38.png](https://hedgedoc.dev/api/private/media/9599a1b61fd9c5d499bb98635c75888d.png)

Название этого видео и является флагом. 

**Ответ**: Never Gonna Hit Those Notes
## I. Кто это нарисовал?
**Описание**: Найдите ID создателя VPN-стикеров.

**Решение**: 
Изучим стикерпак, за который кот отказался платить (первое сообщение в чате https://t.me/owicvnsdk)

![image.png](https://hedgedoc.dev/api/private/media/ed1d529193b3f19c8c9acb43b02f304d.png)

Поищем пользователя @CiaphasCain_bro:

![image.png](https://hedgedoc.dev/api/private/media/64259c1ab56fe3c3cedf5afba92e6b2c.png)

Попробуем поискать информацию о нём через unamer: 

![image.png](https://hedgedoc.dev/api/private/media/8c4a3b084e037bbbeee63095ecd7ecf5.png)

**Ответ**: `7039910552`

## I. Мультиакк
**Описание**: Найдите ID второго аккаунта создателя VPN-стикеров.
**Решение**: 
Поищем информацию о создателе стикеров через бота Telesint:

![image.png](https://hedgedoc.dev/api/private/media/87395fdd146b5c9afdd9f809b1a47204.png)

Протыкаем все чаты и найдем подозрительный последний чат:
![Screenshot 2024-06-26 at 18.38.43.png](https://hedgedoc.dev/api/private/media/02ba4fdd2939622e122ba56995ee0c15.png)
Находим там сообщения от удаленного аккаунта:

![image.png](https://hedgedoc.dev/api/private/media/3f95ecd6891ff5ccece19a2573ee86b8.png)

Через альтернативный клиент Telegram (например ayugram) узнаём его ID:

![image.png](https://hedgedoc.dev/api/private/media/3da1de36d9e5e2b174259e5b458b879a.png)

Ответ: `6811923097`

## I. Записки
**Описание**: Где хранятся записи Sad Prog о разработке VPN (ссылка)?
**Решение**: 
В чате @owicvnsdk есть файл с разрезанным qr кодом. Если собрать его в фотошопе (что удалось нам далеко не с 1 попытки) и отсканировать, открывается ссылка с картинкой: https://imgur.com/a/5yddTCc

Сверху можно заметить надпись: 
![image.png](https://hedgedoc.dev/api/private/media/817f97962d9dd1d278849771d6deab00.png)
Вбиваем указанный id в pastebin и получаем ссылку: https://pastebin.com/4sCxxVkj

**Ответ**: https://pastebin.com/4sCxxVkj

## I. Фотоаппарат
**Описание**: На какое устройство сделана фотография «мести» Sad Prog?
**Решение**: 
В метаданных фотографии QR-кода предыдущего таска, мы можем найти запись об авторе: 
![image.png](https://hedgedoc.dev/api/private/media/8b6f5107c6bf07f1b0453204ddd64d8a.png)
Поищем о нём информацию в поисковике:

![image.png](https://hedgedoc.dev/api/private/media/945e2007d4b8f2906c8df22893e1e695.png)

У этого пользователя есть только один репозиторий, в котором содержатся фотографии. Посмотрим на их метаданные. На самом деле у всех фотографий в метаданных фотоаппараты на которые они сделаны, поэтому в задании присутствует некоторая часть уцуцуги. Попробуем посдавать эти модели и у фотографии `999.jpg` в метаданных стоит нужная модель: 

![image.png](https://hedgedoc.dev/api/private/media/905a15a93fb394fcfd1be3ff22ac4aba.png)

**Ответ**: Panasonic DMC-S2

## I. ИНН
**Описание**: Какой ИНН у нашего скаммера?
**Решение**: Попробуем найти информацию через поисковик строго по ФИО скамера. ФИО мы нашли в записках (https://pastebin.com/4sCxxVkj таск “I. Записки”)


![Screenshot 2024-06-26 at 16.04.33.png](https://hedgedoc.dev/api/private/media/78bfec7d1cb433299001285f060bc339.png)

Гугл выдает нам сайт, на котором есть паспортные данные мошенника Александроса Ятовского
![image.png](https://hedgedoc.dev/api/private/media/9f11f3e4c50996bd1c0345ba1265be54.png)
В нем мы находим его ИНН
**Ответ**: `170105721377`

## I. Кличка
**Описание**: Узнайте, как "Кота" называли на прошлом месте работы.
**Решение**: 
Перейдем в корень сайта, где мы нашли ИНН в предыдущем таске. На нем есть ссылка на бота-HRa:
![Screenshot 2024-06-26 at 15.53.36.png](https://hedgedoc.dev/api/private/media/a259421190666a10452d90b26108890e.png)
Напишем Дибимбе и спросим его про кличку Кота: 
![Screenshot 2024-06-26 at 15.59.06.png](https://hedgedoc.dev/api/private/media/8d4826410c006d6c22f0219940bc9053.png)
Как видно, каждый раз он выдает неправильные ответы. Попробуем базовою промпт-инъекцию:
![Screenshot 2024-06-26 at 16.00.28.png](https://hedgedoc.dev/api/private/media/db6ba4adef3708c2ab68227d605e07e9.png)
И, удивительно, она сработала.
**Ответ**: Кот-обормот

> Кстати так как сайт размещен на Github Pages, нам не составило труда найти соответствующий репозиторий и посмотреть за забавной трансформацией лора)

## I. СоцСети
**Описание**: Найдите идентификационный номер реальной страницы "Кота" в социальной сети.
**Решение**: Поищем информацию о Ятовском по фамилии и имени через поисковик:
![Screenshot 2024-06-26 at 18.44.24.png](https://hedgedoc.dev/api/private/media/079c96b2fabc3dc65edf0692e4c43028.png)
Находим его страницу в VK

![image.png](https://hedgedoc.dev/api/private/media/0568702f036169742188e0bab8fc3a63.png)

Ответ: `859857312`
## I. С ДР!
**Описание**: Найдите дату рождения "Кота".
**Решение**: Изучим статьи со страницы, найденной в предыдущем таске. 
В ней указана дата рождения.

![image.png](https://hedgedoc.dev/api/private/media/96858008a7f3617e356c95f64e99912f.png)
Аналогичный ответ можно было найти через данные с сайта из таска I. ИНН
Ответ: 01.02.2003

## I. Лучший Друг
**Описание:** Найдите фамилию лучшего друга "Кота".
**Решение:** Видим нестандартное место работы: ХакИБ, попробуем найти всех оттуда:

![image.png](https://hedgedoc.dev/api/private/media/515665db659d166cb387708d0c963e3a.png)

Посмотрим всех людей из этой организации, у одного из них видим:

![image.png](https://hedgedoc.dev/api/private/media/b4c0ec7724ca1096cac0333ee704ea84.png)

Читаем комментарий и понимаем, что Никита Леманов - лучший друг Александроса.

**Ответ:** Леманов

## I. Кумир (нет, не информатика)
**Описание:** Какую известную компанию учредил кумир "Кота"?
**Решение:** Изучим посты Александроса в ВК.

![image.png](https://hedgedoc.dev/api/private/media/2459596e031e9a01473b3c095dd4ef32.png)

Тут явно сказано закрытии компании о его кумира-женщины (понятно из слов:”Она разработала уникальное”), относящейся к медицине (это понятно по словам “Я не врач не знаю”). Так же он назвал её новым Стивом Джобсом. Составим запрос в поисковик:
![image.png](https://hedgedoc.dev/api/private/media/103dbe756b3dd67fd21b3480775ba97e.png)

**Ответ:** Theranos

## I. Ликвидация
**Описание:** Кто подписал документ о ликвидации компании кумира "Кота"?
**Решение:** 
Информация о ликвидациях компаний США хранится в официальном реестре компаний штата. Компания Theranos базировалась в Калифорнии (информация из Википедии), поэтому находим реестр этого штата:
![image.png](https://hedgedoc.dev/api/private/media/9a9c13d7500af5b77b3045b21bb0a6bb.png)
Включаем американский vpn и ищем информацию о Theranos в реестре (вторая ссылка):
![image.png](https://hedgedoc.dev/api/private/media/3f627c07cde761cd7417b37144c902b8.png)
Изучаем документы и находим:
![image.png](https://hedgedoc.dev/api/private/media/69da72c5892f5a155e2d169dd9b1b5fb.png)
Смотрим историю документов:
![image.png](https://hedgedoc.dev/api/private/media/b32b40ef40e181741359be404acecd4a.png)
Открываем этот документ и видим:
![image.png](https://hedgedoc.dev/api/private/media/71ee0d57cf8cb505d1eaa2c3df43f6a6.png)
**Ответ:** Barry Kallander

## I. Рабочий TG
**Описание:** Какой ID у рабочего TG аккаунта "Кота"?
**Решение:** Попробуем поискать ник рабочей страницы в vk(https://vk.com/hstmst, таск 01) в телеграме:
![image.png](https://hedgedoc.dev/api/private/media/37ce300e920ef895c302e8026c407826.png)
Через глаз бога просматриваем его ID:
![image.png](https://hedgedoc.dev/api/private/media/40670a4fea7b17410aaa14d942789a2f.png)
**Ответ:** `6855083316`

## I. Хочу на юг...
**Описание:** В какую страну собирался переехать "Кот"?
**Решение:** Проверим ID рабочего аккаунта (6855083316, получен в предыдущем таске) через Telesint:
![image.png](https://hedgedoc.dev/api/private/media/d2e348ee3a5366e3bc11bffb9ff3cbb9.png)
Видим множество чатов про Абхазию. Посмотрим на его сообщения в этих чатах:
![image.png](https://hedgedoc.dev/api/private/media/086b9a13612e97a425743b5922158283.png)
**Ответ:** Абхазия

## II. MAC
**Описание:** Какой MAC-адрес у Wi-Fi сети "Кота"?
**Решение:** Возвращаемся к нашему скриншоту с MAC-адресами участвующими в передаче пакетов на трафике WIFI Кота:
  ![image.png](https://hedgedoc.dev/api/private/media/068a55aab384b2c7aa84a4329deb8fdd.png)
    На HostMaster самая большая часть переданных пакетов, видимо, это и есть MAC его сети. Выписываем MAC `C6:61:AB:1D:8F:8F`
	Ответ: `C6:61:AB:1D:8F:8F`

## II. Город
**Описание:** В каком городе жил/живет "Кот"? (Считаем, что он не пользуется анонимайзерами).
**Решение:** 
Расшифруем наш трафик паролем, полученным из предыдущего таска. Видим, что у нас появились HTTP-пакеты:

Если посмотреть HTTP-Stream, то мы увидим админ-панель сайта donthackme.ru. Немного помотав пакеты, находим, что dashboard.php отдает IP-адрес, с которого осуществлялся вход:

Воспользовавшись любым IP2Geo сервисом, узнаем, что он принадлежит городу Кызыл.
**Ответ:** Кызыл

## III. Кушанье
**Описание:** Как называется любимое блюдо "Кота"?
**Решение:** Попробуем поискать информацию о любимом блюде по Username участника @AlexanderCatVPN [чата (Таск "I. Чатек")](https://t.me/owicvnsdk) через поисковик:
![image.png](https://hedgedoc.dev/api/private/media/3b2396c36a8fc4597d46d235da9fa800.png)
Находится видео на сайте "epicube.su" с рецептом блюда "мамалыга" с комментарием от нашего Александроса:
![image.png](https://hedgedoc.dev/api/private/media/151e2dc86c314347ecb14459ee593ef1.png)

Также есть ещё одно решение, на почте находится письмо с отзывом о кафе, где Александр ел Мамалыгу
**Ответ:** Мамалыга

## III. Отдых
**Описание:** Найдите название отеля, в котором отдыхал "Кот".
**Решение:** На почте находим письмо: 

![image.png](https://hedgedoc.dev/api/private/media/9e76e18e888f7c178dbf3d2e8eb98164.png)

Выкачиваем картинку и запоминаем, что в этом заведении есть места для барбекю и патио, также присутствует зона для мангала, переходим в наш любимый поисковик и делаем точно такой запрос, что Александрос все таки переехал в Гагры.

![image.png](https://hedgedoc.dev/api/private/media/c2a4a038f735c99bcd38e74782765cf3.png)

Кликаем на все сайты, с первой страницы в поисках подходящей местности. Находим Grass Hotel
![image.png](https://hedgedoc.dev/api/private/media/a829c1b582700d704731ae364ddce0d0.png) 
Действительно, на картинках совпадает природа, расположение домиков друг относительно друга и цветовая температура освещения.

**Ответ:** Grass Hotel

## III. Переезд
**Описание:** Какую гостиницу присмотрел "Кот" для релокации?
**Решение:** Зайдём в почту нашего Кота и откроем вкладку "sent", заметим письмо:
![image.png](https://hedgedoc.dev/api/private/media/869739e647b209ca979f5f1009dd6b9f.png)
Через яндекс картинки поищем 2 фотографию, вот это место:

![image.png](https://hedgedoc.dev/api/private/media/f97039267f48cf5ff0fe1df9be2ba11d.png)

Это Гагра, Абхазия
В письме сказано, что отель находится в доме №33. Поищем отель в Гагре в 33-м доме через яндекс карты:

![image.png](https://hedgedoc.dev/api/private/media/af8e1a1d699fcea6325080d94aab22ed.png)

**Ответ:** Apsuana Rose

## III. Платежка
**Описание:** Получите фрагмент номера, на который "Кот" зарегистрировал платежный сервис.

На почте в категории отправленных находим интересное письмо:

![image.png](https://hedgedoc.dev/api/private/media/7295953a2bd8166a0f669018db7e9bb2.png)

Где есть ссылка с эл. адресом gmail. При проверке популярных платёжных сервисов, находим, что данная почта зарегистрирована на PayPal. Попробуем восстановить к ней пароль.

![image.png](https://hedgedoc.dev/api/private/media/db43020190adf76d127c7d2fa2c3f1fc.png)

Видим часть номера телефона с PayPal.

**Ответ**: ‪4475483‬
## IV. Перелет
**Описание:** Найдите название аэропорта, из которого вылетел "Кот".

**Решение:** 
В таске "II. Город" мы выяснили, что Александрос живёт в городе Кызыл. Откроем Яндекс карты и посмотрим какие в этом городе есть аэропорты:
![image.png](https://hedgedoc.dev/api/private/media/259880987f1146668e4e9ec741d62e00.png)

Во всём Кызыле есть только один аэропорт, который так и называется - Аэропорт Кызыл.

**Ответ:** Аэропорт Кызыл
## IV. Музыка
**Описание:** Узнайте любимую музыкальную группу "Кота".

**Решение:** С помощью тг бота найденного в 13 задании зайдем в папку музыки, команда: https://google.com/ && dir C:\Users\HostMaster\Music и получаем вывод
![image.png](https://hedgedoc.dev/api/private/media/a9695c1f502d5e6331f1663e5ef33897.png)

Большинство музыки составляет: треки под авторством Everyone Loves A Villain

**Ответ:** Everyone Loves A Villain