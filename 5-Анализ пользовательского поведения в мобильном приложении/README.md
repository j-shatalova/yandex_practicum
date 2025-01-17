# Анализ поведения пользователей в мобильном приложении и проведение А/Б-тестирования
## Описание проекта 
Вы работаете в стартапе, который продаёт продукты питания. Нужно разобраться, как ведут себя пользователи вашего мобильного приложения.  

## Цель проекта
Необходимо изучить воронку продаж и узнать, как пользователи доходят до покупки. Исследовать результаты A/A/B-эксперимента по замене шрифтов во всём приложении и выяснить, какой шрифт лучше.

## Задачи проекта
- предобработать данные
- изучить и проверить данные
- изучить воронку событий
- изучить результаты A/A/B-эксперимента по замене шрифтов во всём приложении

## Используемые библиотеки

*pandas*

*numpy*

*seaborn*

*matplotlib*

*scipy*

*plotly*

*math*

## Общий вывод

В ходе работы была проведена следующая предобработка данных:
- удалено 413 строк-дубликатов
- пропуски не выявлены
- переименованы названия колонок и приведены к "змеиному" регистру
- поменян формат столбца с датой и временем на datetime
- добавлен столбец с одной датой 'date'

Исследовательский анализ данных выявил следующие факты:

- в первоначальном варианте лога присутствовало событий: 243713, количество уникальных пользователей в логе: 7551
- среднее количество событий на одного уникального пользователя: 32
- период ведения лога: 13 дней - с 2019-07-25 по 2019-08-07
- определен период, за который мы имеем полные данные: с 01-08-2019 по 07-08-2019
- был отброшен период, за который данные были неполными. Отбросив их, мы потеряли 1,16% событий и 0,23% уникальных пользователей. Потери можно считать незначительными.

в отфильтрованном датафрейме есть пользователи всех трех групп тестирования:
- группа 246 - 2484 человека
- группа 247 - 2513 человек
- группа 248 - 2537 человек

в логах всего есть 5 событий: самое встречаемое - MainScreenAppear - появление главного экрана (его доля 48.7%), на втором месте экран предложения (доля 19.2%) OffersScreenAppear, на третьем месте появление экрана корзины CartScreenAppear (доля 17.5%), на 4 месте PaymentScreenSuccessful - экран успешной оплаты (доля 14.1%), наименее частое событие - Tutorial - руководство приложения, его вызывали всего 1005 раз (0,4%)

на главный экран заходят 98,5% пользователей, что вызывает вопросы, 7534 пользователя установили приложение, но главный экран увидели только 7419 пользователя, скорее всего при его загрузке возникли технические проблемы. На экран предложения заходят около 61% пользователей, до этапа корзины доходят 49%, и 47% пользователей проходят до экрана успешной оплаты.

выявлен порядок, в котором происходят события: MainScreenAppear появление главного экрана > Tutorial экран с руководством к приложению > OffersScreenAppear появление экрана с предложением товара > CartScreenAppear появление экрана с корзиной > PaymentScreenSuccessful экран успешной оплаты. Экран с руководством к приложению Tutorial не является обязательным этапом и не влияет на то, совершил пользователь покупку или нет, всего 11% пользователей проходят этот этап. При расчете воронки этап Tutorial был исключен из лога событий.

по воронке событий были сделаны следующие выводы:

- на экран предложения товара переходит около 62% пользователей (4593 чел.), из них на экран корзины переходят 81% пользователей (3734 чел.), далее на экран успешной оплаты попадают 95% от числа пользователей предыдущего шага (3539 чел.)
- больше всего пользователей теряется на втором шаге - при переходе с главного экрана (MainScreenAppear) на экран появления предложения (OffersScreenAppear), таким образом 38% пользователей не заходят дальше главного экрана
- от первого события (главный экран приложения) до успешной оплаты доходит 47,7% - практически половина пользователей.

Проведенное А/А и А/Б тестирование дало следующие результаты:

- А/А тестирование выявило отсутствие статистической разницы между контрольными группами 246 и 247. Разделение на группы работает верно.
- А/Б тестирование не выявило статистическую разницу между контрольными группами 246, 247, объединенной группой (246 и 247) и группой с измененным шрифтом 248.

Таким образом, проведенное изменение шрифтов никак не повлияло на поведение пользователей приложения.

Рекомендации:

- необходимо выяснить, почему только 98,5% из 100% пользователей заходят на главный экран приложения - нет ли технических проблем при его загрузке?
- так как до экрана успешной оплаты доходит только 47% пользователей необходимо разработать список гипотез вместе с маркетинговым отделом по увеличению конверсии
