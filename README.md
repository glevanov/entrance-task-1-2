# Задание 1 — найди ошибки

В этом репозитории находятся материалы тестового задания "Найди ошибки" для [14-й Школы разработки интерфейсов](https://academy.yandex.ru/events/frontend/shri_msk-2018-2) (осень 2018, Москва, Санкт-Петербург, Симферополь).

Для работы тестового приложения нужен Node.JS v9. В проекте используются [Yandex Maps API](https://tech.yandex.ru/maps/doc/jsapi/2.1/quick-start/index-docpage/) и [ChartJS](http://www.chartjs.org).

## Задание

Код содержит ошибки разной степени критичности. Некоторые из них — стилистические, а другие — даже не позволят вам запустить приложение. Вам нужно найти все ошибки и исправить их.

Пункты для самопроверки:

1. Приложение должно успешно запускаться.
1. По адресу http://localhost:9000 должна открываться карта с метками.
1. Должна правильно работать вся функциональность, перечисленная в условиях задания.
1. Не должно быть лишнего кода.
1. Все должно быть в едином codestyle.

## Запуск

```
npm i
npm start
```

При каждом запуске тестовые данные генерируются заново случайным образом.

## Ход решения
1. Запустить и посмотреть что получится. А получится синтаксическая ошибка в консоли ноды - **ошибка при экспорте в модуле map**, а именно не хватает default.
1. Глаз зацепился за **var** в том же модуле, а это неконсистентно общему стилю (ES6). Меняем на const в map и filter.
1. Проверяем на ошибки. В ноде нет, в браузере в консоли нет.
1. Приложение сообщило о запуске (init), но карта не отрисовывается. Идем проверять, а правильно ли изначально встроена карта. Для этого открываем  «быстрый старт» и видим, что прокол уже на втором пункте - **контейнер карты нулевого размера**. Через style явно задаем размеры контейнера в 100% вьюпорта (vh/vw). Лучше использовать относительные еденицы чтобы размер карты менялся вместе с размером окна.
1. Идеи закончились, приходится одним глазом читать код, другим - документацию. Находим первую ошибку, которая пока ни на что не влияет - в map в objectManager.clusters.options.set **вместо объекта передаются две строки**, исправляем на объект с ключом-значением.
1. В процессе чтения модуля mappers обнаружил **двойные кавычки вместо одинарных**, что тоже неконсистентно общему стилю. Исправляем здесь в index на одинарные.
1. В том же mappers обнаружил, что в coordinates **перепутаны местами lat и lon** - поправил
1. Не смотря на все фиксы на карте до сих пор не отрисовываются точки. Это повод посмотреть, а как их вообще надо отрисовывать. Из примера в «быстром старте» мы видим, что в этом проекте **пропущено добавление точек на карту**. Поэтому в модуле map нужно добавить myMap.geoObjects.add(), куда положить в качестве аргумента наши точки наш objectManager.
1. Следующая проблема - почему-то на карте нет кластеров с красными точками. Путем поиска в документации по green (такой запрос придумал потому, что метки зелёные) обнаружил, что **проблема в пресете greenClusterIcons**. Если его удалить, кластеры становятся разноцветными.
1. Далее необходимо разобраться с открытием Balloon по клику на пин. Он открывается, но не возле метки. Изучая в коде всё что связано с Balloon доходим до строчки с **geoObjectBalloonContentLayout**. Если её убрать, Balloon отрисовывается возле пина.