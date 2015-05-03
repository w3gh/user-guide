Пошаговая настройка Ghost++ для linux (также для Ubuntu сервера.) компьютера.
Для данного проекта будет использоваться свежоустановленный Ubuntu 8.10 сервер, другие версии Linux также применимы,
правда некоторые вещи поменяются такие как `bot_war3path` в конфиг файле.

###Подготовка машины
Первое что вам нужно сделать это собрать `build-essentials`.
Этот пакет позволит вам компилировать различные исходные файлы, это нам нужно сделать чтобы скомпилировать Ghost++.
Большинство серверов уже имеют этот пакет, но если нет, то это необходимо.
Также нам нужно чтобы был установлен GMP чтобы скомпилировать bncsutil библиотеку, и нам нужно M4 чтобы скомпилировать GMP библиотеку.
Но перво наперво нам нужно `zlib` и `libbz2` для StormLib.
Итак давайте начнём с сборки `build-essentials`, `m4`, `zlib` и `libbz2`.
Для установки просто используем apt.

    sudo apt-get install build-essential m4 zlib1g-dev libbz2-dev libgmp3-dev

Далее нужно установить GMP.
Берём последний tarball с сайта ftp://ftp.gnu.org/gnu/gmp/

    wget ftp://ftp.gnu.org/gnu/gmp/gmp-x.x.x.tar.bz2

(замените на нужную версию)
Для извлечения tarball, вводим

    tar -jxvf gmp-x.x.x.tar.bz2

Переходим в gmp папку, настраиваем, собираем, и устанавливаем.

    cd gmp-x.x.x
    sudo ./configure
    sudo make
    sudo make install

GMP должна спросить вас, проверить сборку. Соглашаемся.
Это займёт секунду  и это избавит вас от гемороя если вы что то сделает нетак.
Просто вводим `make check`.

###Компиляция Boost
Новые версии бота требуют установленную boost библиотеку, последние версии можно всегда скачать с http://boost.org

Скачать Boost 1.38.0

    wget http://downloads.sourceforge.net/project/boost/boost/1.38.0/boost_1_38_0.tar.gz

Распаковать

    tar -zxf boost_1_38_0.tar.gz
    cd boost_1_38_0

Выполнить

    ./configure --prefix=/usr --with-libraries=date_time,thread,system,filesystem,regex

Отредактировать __Makefile__
_BJAM_CONFIG=_
(2 строка)
заменить на:
_BJAM_CONFIG= --layout=system_

после чего

    make
    sudo make install

###Компиляция StormLib и BNC
Теперь мы можем перейти к работе с ghost++.
Для начала возмём исходники ghost++ c http://code.google.com/p/ghostplusplus/
Запуск ghost++ будем производить из home папки `/home/admin/` или просто `~/`.
Вы можете положить его куда вам угодно, но для данного случая будем использовать эту папку.

    cd ~/
    wget http://ghostplusplus.googlecode.com/files/ghostplusplus_xx.xx.zip
    unzip ghostplusplus_xx.xx.zip

> Если компиляция на последней стабильной версии выводит ошибки, попробуйте взять последнюю версию из репозитория
> `svn checkout http://ghostplusplus.googlecode.com/svn/trunk/ ghostplusplus`

Хорошо, теперь мы готовы к сборке компонентов нужных для запуска ghost++ :
battle.net клиентская библиотека bncsutil и StormLib.
Давайте начнём с bncsutil.

    cd ~/ghost/bncsutil/src/bncsutil/
    sudo make
    sudo make install

Оно должно скомпилицо без ошибок. Если нет, дважды проверяем установлен ли правильно build-essentials, GMP, и m4.
Далее нам нужно скомпилировать наш StormLib.

    cd ~/ghost/StormLib/stormlib
    sudo make
    sudo make install

Опять же, убедитесь что он скомпилировался без ощибок, в противном слушчае проверяем и смотрим установлены ли правильно build-essentials, zlib, и libbz2.

###Компиляция GHost++
Мы почти готовы к компиляции ghost++, но сначала нам нужно немного изменить его чтобы он смог работать под linux.
Используем vim для добавления, можно использовать nano или любой другой консольный редактор

_Для старых версий_

>
> Вставим
>    `#include <string.h>`
>
> непосредственно после `#include` в `game.cpp` файле.
>
>    vim ~/ghost/ghost/game.cpp
>
> Спускаемся под строку, далее вставляем текст. Вводим то что нужно, жмём 'esc' и вводим ZZ tдля выхода (в правильном регистре).


Отлично! Теперь мы готовы для компиляции ghost++.
Это займёт несколько сек, и мы сможем его запустить через несколько минут.

    cd ~/ghost/ghost
    sudo make

Давайте перейдём в root папку проверим имеет ли доступ бот к конфигу.

    cp ~/ghost/ghost/ghost++ ~/ghost/ghost++

### Настройка GHost++

Теперь давайте протестируем правдо ли он запущен. Он ещё не готов но мы в одном шаге от этого

    ./ghost++

При запуске должна появицо линия, `[GHOST] GHost++ Version xx.xx`. Вот что мы ищем. Жмём ctrl+c чтобы убить сервер. Запомните у нас нет установленного wc3 на linux, так что нам нужно предоставить ghost++ несколько файлов из стандартного warcraft 3. Давайте начнём, идём в вашу Warcraft 3 . Копируем файлы game.dll, Storm.dll, и war3.exe в отдельную папку. Убедитесь что вы переименовали ваши папки и файлы в нижний регистр ( пример. Storm.dll --> storm.dll ). Перекидываем содержимое папки в ubuntu box в /usr/lib/.
Если вы используете выделенный сервер как мой, конечно мы не можем указать пальцом чтобы скопировать туда файлы. Но зато можем использовать программу pscp для создания scp трансфера на ваш сервер. Загружаем pscp тут [URL='http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html']http://www.chiark.greenend.org.uk/~sgta ... nload.html[/URL] , и ложим exe файл в папку с Windows ( C:\Windows  ). Открываем командную строку в windows box, переходим к папке и пишем.

    pscp * root@YOUR_SERVERS_IP:/usr/lib

Теперь ghost должен иметь доступ к своим файлам.
Переходим к конфиг файлу. Основное различие заключается в том, что пути должны быть адаптированы к Linux.

    bot_war3path = /usr/lib/
    bot_mapcfgpath = mapcfgs/
    bot_savegamepath = savegames/
    bot_mappath = maps/
    bot_replaypath = replays/

После этого, заполняем нужные поля в конфиге (b.net акк, пароль, cd keys, и т.д.) теперь давайте запустим ghost, переходим к нему в папку (cd ~/ghost/ если вы уже тут) и пишем

    ./ghost++

Готово! Если всё настроено правилно он должен зайти на bnet. Вау!

### Разное
Также если вы заходите через ssh, но хотелось бы оставить бота запущенным на сервер, можно использовать `screen`.

    sudo apt-get install screen

Если он у вас уже есть. Вводим

    screen
    ./ghost++

Далее нажимаем комбинацию клавиш
CTRL+A D

Это отключит вас от `screen` и оставит его запущенным в фоновом режиме.
Для возврата пишем

    screen -r