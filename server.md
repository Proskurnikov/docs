# Настройка сервера

## Выполнение html как php

В разделе рассматриваются способы настройки сервера для выполнения файлов с расширением `.html` как `php` скриптов.
По умолчанию, сервер выполняет `php` скрипты файлов с расширением `.php`.

Согласно документации <http://httpd.apache.org/docs/2.2/howto/htaccess.html>,  следует полностью избегать использования файла `.htaccess` если есть доступ к основному конфигурационному файлу `httpd`. Использование файла `.htaccess` замедляет `http` сервер Apache. Все директивы, которые можно включить в `.htaccess` файл лучше указать в блоке `Directory` - это будет иметь тот же эффект с большей производительностью.

Основной конфигурационный файл сервера для разработки `xampp` <https://www.apachefriends.org/index.html> это `httpd-xampp.conf` который расположен в папке [C:\xampp\apache\conf\extra](file://C:/xampp/apache/conf/extra).

Модуль `php` для `apache` регистрируется как обработчик для `mime`-типа `application/x-httpd-php`. Файл `C:\xampp\apache\conf\extra\httpd-xampp.conf` содержит строки

```properties
<FilesMatch "\.php$">
    SetHandler application/x-httpd-php
</FilesMatch>
```

которые сообщают серверу `apache`, что файлы с расширением `.php` должны быть переданы обработчику `application/x-httpd-php`. Если Вы на самом деле хотите, что бы `.html` файлы обрабатывались как `php` Вы должны добавить похожие строки для для файлов с расширением `html`. Есть другие способы сказать apache какое расширение соответствует mime-типу или обработчику, но [FilesMatch](http://httpd.apache.org/docs/2.2/mod/core.html#filesmatch) / [SetHandler](http://httpd.apache.org/docs/2.2/mod/core.html#sethandler) - хороший способ. Если Вы хотите сделать такую обработку для конкретной директории, Вы можете использовать файл .htaccess.
[Источник](https://stackoverflow.com/a/2198740/11596781)

В файл `C:\xampp\apache\conf\extra\httpd-xampp.conf` добавляем

```properties
<FilesMatch "\.html$">
    SetHandler application/x-httpd-php
</FilesMatch>
```

Т.к. к серверу разработки есть доступ, но нет доступа к конфигурационному файлу сервера, на котором будет размещен сайт, то настройку будем производить в файле `.htaccess`.

Справка хостинга <https://jino.ru/help/faq/php/> (раздел "Как заставить html-страницы обрабатывать PHP код?") подсказывает:

>По умолчанию PHP-скриптами считаются лишь файлы с расширением .php и .phtml. Чтобы включить обработку PHP-кода в файлах с расширением .html или .htm, нужно добавить в файл .htaccess следующую директиву:
>
> ```htaccess
> AddType application/x-httpd-php .html .htm
> ```
>
>Если в нужной папке нет файла .htaccess, создайте его. Действие директив этого файла распространяется и на все вложенные папки.

После внесения изменений сервер для разработки следует перезагрузить.
