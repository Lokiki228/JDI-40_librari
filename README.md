# JDI-40_librari
Легкая библиотека для модуля JDI-40, настраиваемый по UART, библиотека написана для легкой настройки модуля из программы(Вы можете и сами это делать без библиотеки, используя AT команды).
Я собрал все это в одну библиотеку. Для более легкой настройки используется класс, так что настройка будет очень легкой. Также добавил еще немного плюшек по типу симплекса и энергосбережения.
## Возможности
### Сейчас
- гибкая настройка модуля из программы
- передача любого Stream объекта
- какое никакое энерго сбережение(актуально только для симплексного режима работы)
- симплексный режим работы UART-моста(в разработке, но уже есть пара методов)
- отчет об успешности выполнения настройки(ТОЛЬКО в режиме настройки через АТ): отправили команду - получаем данные с UART - и понимаем получили или нет
- проверка наличия модуля на шине
### В будующем
- наследование класса print
- получение всех настроек с модуля
## Совместимость
С ней проблем не должно быть, т.к. используются ардуино функции.
Но для справки! Я тестировал это на esp8266 и lgt8f328. Рекомендуется использовать softSerial, но может и получиться с hardware.
## Скорость
При 9600 бод, setAllSettings выполняется около 400 мс, одиночные настройки выполняются 70 - 80 мс, сами AT команды выполняются быстрее, но у нас же все по-умному).
- Во-первых нужны небольшие зажержки чтобы модуль успел очухаться и понять что он в режиме настройки
- предусмотрел небольшие задержки перед отправкой и после
- быстрая синхронизация двух систем по проводу(например вемос в связке с jdi), я еще не придумал поэтому не могу описать.
Где небольшие зажержки - 15 мс.
## Методы
### Конструктор
```cpp
JDI40 jdi(Serial); //в конструктор передаем Stream объект, Serial или SoftSerial, рекомендую softSerial, но может и заработать и без него.
```
Проверено на собственном опыте! Не подключайте модуль к аппаратному Serial esp8266(available был просто пуст), у меня заработал только с программным.
### Проблемы
после setBaud, как минимум 2 настройки не воспринимаются, так что лучше делать setAllSettings или ВООБЩЕ НЕ ИСПОЛЬЗОВАТЬ setBaud. Я постараюсь решить данную проблему.
### Функции
```cpp
void configDevice(bool mode, uint8_t cs, uint8_t set); //Передаем режим(о нем ниже), передаем пин на который цепляем cs, передаем пин на который цепляем set

bool begin(); //проверяет наличие модуля, вернет 1 если все норм

void on(); //включить модуль
void off(); //выключить модуль
//не совсем нужные функции, используются если хотите вручную посылать данные в порт(для передачи) для симплекса, при этом сохранив низкое потребление, о нем ниже 

bool setAllSettings(uint8_t baud, uint16_t rfid, uint16_t dvid, uint8_t rfc, uint8_t power); //передать все настройки модуля: скорость порта,
//ID сети(пока что от 1000 до 9999), ID устройства(пока что от 1000 до 9999), канал связи, и мощность передатчика.
//о диапазонах параметров ниже
//если все хорошо вернет 1

//те же настройки но по отдельности
//вернут единичку если все прошло хорошо
bool setBaud(uint8_t baud); //от 1 до 6
/* скорости в бодах
1 - 1200
2 - 2400
3 - 4800
4 - 9600
5 - 14400
6 - 19200
*/ //по умолчанию 4

bool setID(uint16_t rfid, uint16_t dvid); //передаем ID сети(пока что от 1000 до 9999), ID устройства(пока что от 1000 до 9999). на деле можно передать 16-ти битные числа
//вернет 1 если все прошло хорошо

bool setChannel(uint8_t rfc); //установить канал связи от 0 до 127
//вернет 1 если все прошло хорошо

bool setPower(uint8_t power); //установить мощность передатчика от 0 до 9
//вернет 1 если все прошло хорошо
/*
0 - 25db
1 - 15db
2 - 5db
3 - 0db
4 - 3db
5 - 6db
6 - 9db
7 - 10db
8 - 10db
9 - 12db
*/ //и да 7 и 8 - это не ошибка, по умолчанию стоит 10dB




//Отправка в симплексе
void print(String str); //принимает строку
void print(const char* str); //принимает текст в кавычках типа "бла бла бла"
//тестовые функции, используются для симплекса, при энергосбережении
//тобишь включили чип - отправили данные - выключили чип

//coming soon... More, MORE!
```
Буду рад вашим дополнениям к библиотеке,














