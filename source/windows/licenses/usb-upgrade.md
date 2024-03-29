# Обновление USB-ключа

Процедура обновления электронного usb-ключа может потребоваться в следующих случаях:

* Изменение состава компонентов продукта
* Изменение количества пользователей компонентов продукта
* Продление времени действия временных ограничений

## Обновление памяти электронного ключа

Самый удобный вариант обновления памяти электронного ключа – это дистанционное программирование памяти ключа при помощи программы GrdTRU.exe.

> **Важно**: Перед началом выполнения обновления памяти ключа необходимо закрыть все модули программы Loginom.

Для дистанционного программирования памяти ключа при помощи программы GrdTRU.exe, пользователю Loginom необходимо выполнить следующие шаги:

**1.** На том компьютере, на котором установлен usb-ключ Loginom, запустить программу GrdTRU.exe.

> **Примечание**: Электронный ключ во время работы программы GrdTRU.exe должен быть подсоединен к компьютеру.

**2.** После запуска программы на первом шаге мастера нужно выбрать пункт "Начать новую операцию обновления ключа" и нажать на кнопку "Далее".

![](../../images/guardant-usb-upgrade-1.png)

**3.** На втором шаге мастера пользователю будет предоставлен "число-вопрос", которое необходимо будет переслать в службу техподдержки компании Loginom Company на адрес [support@loginom.ru](mailto:support@loginom.ru).

![](../../images/guardant-usb-upgrade-2.png)

Полученное "число-вопрос" можно сразу отправить по электронной почте воспользовавшись нижерасположенной кнопкой "По почте". Или сохранить для последующей отправки в буфере обмена или в текстовом файле.

Примечание. Сообщите записанное "число-вопрос" компании Loginom Company по электронной почте [support@loginom.ru](mailto:support@loginom.ru) с названием темы "Обновление Loginom: число-вопрос" и указанием оснований причин модификации состава лицензий.

**4.** После отправки "числа-вопроса" можно завершить работу программы нажав кнопку Завершить.

**5.** После того как вы получите число-ответ (в письме будет содержаться "файл-обновления ключа"), снова запустите программу GrdTRU.exe. При запуске программы нужно выбрать пункт "Продолжить операцию обновления ключа, инициализированную во время предыдущего сеанса" и нажать на кнопку "Далее".

![](../../images/guardant-usb-upgrade-3.png)

> **Примечание**: Программу GrdTRU.exe не следует повторно запускать до получения "файла-ответа", иначе всю процедуру будет необходимо повторить заново.

**6.** На следующем шаге вводится полученное "число-ответ". Для его ввода можно воспользоваться кнопками вставки "Из буфера" или "Из файла".

![](../../images/guardant-usb-upgrade-4.png)

**7.** В случае правильного следования предшествующих инструкций вы должны увидеть следующее окно – программирование ключа прошло успешно. Внизу окна отображается код подтверждения операции, который необходимо переслать поставщику. Для завершения программы нажмите кнопку "Готово".

![](../../images/guardant-usb-upgrade-5.png)

> **Примечание**:  Код подтверждения операции необходимо обязательно сообщить компании Loginom Company по электронной почте [support@loginom.ru](mailto:support@loginom.ru) с названием темы "Обновление Loginom: код подтверждения".

Если при перепрограммировании ключа были открыты модули Loginom, то перед запуском нового проекта их необходимо закрыть и заново открыть иначе может возникнуть ошибка запуска проекта.

> **Важно**:  После обновления сетевого ключа, в обязательном порядке необходимо перезапустить службу сервера лицензий "Guardant Dongle License Service".
