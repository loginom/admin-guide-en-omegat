# Установка Loginom Desktop для Linux

Loginom Desktop для Linux поставляется в виде архива:

* loginom-personal-7.x.x.tar.gz — для редакции Personal;
* loginom-community-7.x.x.tar.gz — для редакции Community.

где 7.x.x – цифры, обозначающие версию и релиз программы.

Для начала работы необходимо распаковать архив через любой доступный архиватор или выполнить команду:

* `tar -xf loginom-personal-7.x.x.tar.gz` — для редакции Personal;
* `tar -xf loginom-community-7.x.x.tar.gz` — для редакции Community.

Распакованный каталог `loginom-personal`/`loginom-community` будет являться директорией с программными файлами Loginom. Запустить Loginom Desktop можно просто выполнив из этого каталога исполняемый файл `loginom`.

В каталоге `loginom-personal`/`loginom-community` находится скрипт, позволяющий создать в меню ярлык для запуска Loginom Desktop:

`./install-shortcut.sh`

>**Примечание:**
>
>1. В архиве находится файл README.txt, в котором даны инструкции по установке, а также другая полезная информация.
>
>2. При использовании сторонних библиотек может потребоваться их самостоятельная установка.
