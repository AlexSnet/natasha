# Natasha

![](http://i.imgur.com/jQtaSTV.png)

Наташа извлекает именованные сущности из текста на русском языке, включая (но не ограничиваясь):

**Физ. лица**: `Иванов Иван Иванович`, `Иван Иванов`, `Иван Петрович`, `Ваня`  
**Организации**: `ПАО «Газпром»`, `ИП Иванов Иван Иванович`, `агентство Bloomberg`  
**События**: `фестиваль «Ковчег спасения»`, `шоу «Пятая империя»`  
**Гео-объекты**: `Москва`, `Ленинградская область`, `Российская Федерация`, `Северо-Кавказский ФО`  
**Объекты времени**: `21 мая 1996 года`, `21.05.1996`, `21 мая`, `сегодня`, `в конце года`  
**Денежные единицы**: `200 рублей`, `1 млрд. долларов`,  `семьдесят пять тысяч рублей`  

Алгоритм работы (выделение сущностей по заданным правилам, используя морфологический разбор) похож на [Томита-парсер от Яндекса](https://tech.yandex.ru/tomita/).

# Зачем?

При всех хороших качествах существующих решений для выделения сущностей, большинство из них трудно использовать в python-проектах, многие имеют закрытый исходный код, неудобную лицензию, бедную документацию и словари.  
`Natasha` призвана решить эту проблему - код написан исключительно на python, все используемые библиотеки имеют открытый исходный код, а морфологический анализатор - свободный словарь [OpenCorpora](http://opencorpora.org/).  
Если вдаваться в технические детали, `natasha` - всего лишь набор частоиспользуемых грамматик для [shift-reduce парсера](https://github.com/bureaucratic-labs/yargy). Это позволяет избавить разработчика, желающего научить своё приложение понимать натуральный язык, от необходимости писать лингвистические правила, ведь это скучно.

# Где используется?

Например, в [Rent-a-room](https://rent-a-room.ru) - агрегаторе объявлений о сдаче в аренду жилой недвижимости:  
![Imgur](http://i.imgur.com/iTsBtCS.png)

# Установка

*Важно:* для работы требуется python версии  **3.5** и выше.

```bash
$ pip install natasha
```

# Использование

Для первого знакомства можно использовать [онлайн версию](https://bureaucratic-labs.github.io/natasha/).

```python
from natasha import Combinator, DEFAULT_GRAMMARS
from natasha.grammars import Geo, Date

# DEFAULT_GRAMMARS содержит стандартный набор правил:
# [
#    <enum 'Money'>,
#    <enum 'Person'>,
#    <enum 'Geo'>,
#    <enum 'Date'>,
#    <enum 'Brand'>,
#    <enum 'Event'>
# ]

# Можно использовать их частично или использовать свои правила
MY_GRAMMARS_LIST = [
    Geo,
    Date,
]

text = "23 августа в Нижнем Новгороде пройдет очередной день"

combinator = Combinator(MY_GRAMMARS_LIST)
for (grammar, rule, tokens) in combinator.extract(text):
    print("Тип:", grammar)
    print("Правило:", rule)
    print("Токены:", tokens)
```

# Лицензия

Исходный код распространяется под лицензией MIT.
