# Поведенческие шаблоны проектирования

В данной работе рассмотрены три классических поведенческих шаблона:

1. Команда (Command)
2. Стратегия (Strategy)
3. Шаблонный метод (Template Method)

Каждый из них будет представлен как «проблема — решение», включая:
- роль инкапсуляции
- применение принципа «разделяй и властвуй»
- концепт ортогональности стратегий
- влияние многопоточности

---

##  1. Шаблон «Команда» (Command)

###  Определение:
Команда инкапсулирует запрос как объект, позволяя параметризовать клиентов с различными запросами, ставить запросы в очередь или протоколировать их, а также поддерживать отмену операций.

---

###  Проблема:
- Необходимо отвязать отправителя запроса от получателя
- Поддержка логирования, откладывания, отмены действий
- Унификация API для различных действий

---

###  Решение:
- Создаём интерфейс `Command` с методом `execute()`
- Каждое действие инкапсулируется в отдельную реализацию команды
- Передаём объект команды вызывающему коду, который не знает о деталях

---

###  Примеры:

1. **GUI-кнопка**: назначаем команды `OpenFileCommand`, `SaveCommand`, не меняя кнопку.
2. **Удалённое управление**: команды `TurnOn`, `SetBrightness`, `Shutdown`.
3. **Очередь задач**: команда добавляется в очередь и исполняется worker-потоком.

---

###  Принципы:

- **Инкапсуляция**: действия инкапсулируются в объект
- **Разделяй и властвуй**: клиенту не нужно знать реализацию
- **Ортогональность**: легко заменять/отменять команды

---

###  Многопоточность:
Команды можно безопасно использовать в многопоточном планировщике — ставить в очередь, выполнять в пуле потоков, использовать lock-free структуры данных.

---

##  2. Шаблон «Стратегия» (Strategy)

###  Определение:
Стратегия определяет семейство алгоритмов, инкапсулирует каждый из них и делает их взаимозаменяемыми. Позволяет выбирать алгоритм во время выполнения.

---

###  Проблема:
- Поведение алгоритма может меняться
- Нельзя жёстко зашивать его в основной код
- Необходима гибкость и масштабируемость

---

###  Решение:
- Создаём интерфейс `IStrategy` с методом `execute()`
- Реализуем разные стратегии, например `QuickSort`, `MergeSort`
- Передаём нужную стратегию в объект, который её использует

---

###  Примеры:

1. **Алгоритмы сортировки**: пользователь может выбрать QuickSort или MergeSort.
2. **Поведение персонажа в игре**: агрессивная/оборонительная стратегия.
3. **Оплата заказа**: `PayByCard`, `PayByCash`, `PayByCrypto`.

---

###  Принципы:

- **Инкапсуляция**: стратегия реализует поведение вне основного класса
- **Разделяй и властвуй**: объект использует абстракцию
- **Ортогональность**: можно изменять поведение без изменения кода объекта

---

###  Многопоточность:
Возможность выбирать потокобезопасную или быструю (но небезопасную) стратегию во время выполнения. Также можно изолировать стратегии в разные потоки.

---

##  3. Шаблон «Шаблонный метод» (Template Method)

###  Определение:
Шаблонный метод определяет скелет алгоритма в операции, оставляя определённые шаги подклассам. Подклассы могут переопределить шаги алгоритма, не изменяя его структуру.

---

###  Проблема:
- Разные подклассы имеют схожую структуру алгоритма
- Но отдельные шаги должны различаться
- Не хочется дублировать весь алгоритм

---

###  Решение:
- Создаём абстрактный базовый класс с `runAlgorithm()`, вызывающим виртуальные методы
- Подклассы переопределяют только нужные шаги

---

###  Примеры:

1. **Процесс обработки документов**: `load -> parse -> save` (меняется только `parse`)
2. **Онлайн-курс**: `вступление -> урок -> тест -> сертификат`
3. **Создание отчёта**: `fetchData -> process -> export` (разные форматы)

---

###  Принципы:

- **Инкапсуляция**: общий алгоритм скрыт в базовом классе
- **Разделяй и властвуй**: шаги делегируются подклассам
- **Ортогональность**: изменения в одном шаге не затрагивают другие

---

###  Многопоточность:
Шаблонный метод полезен, если шаги выполняются в отдельных потоках или параллельно (например, через async). Позволяет разбить алгоритм на параллелизуемые блоки.

---

##  Что такое Архитектура ПО

###  Определение:
Архитектура программного обеспечения — это совокупность фундаментальных структур системы, а также дисциплина, которая занимается их созданием и развитием.

В неё входят:
- разбиение на модули
- шаблоны и соглашения
- связи между компонентами
- выбор технологий

---

##  Влияние многопоточности на архитектуру:

1. **Разделение ответственности**: выделение компонентов под каждый поток
2. **Безопасность данных**: синхронизация или использование lock-free структур
3. **Проектирование очередей и обработчиков**: архитектурные шаблоны типа Producer-Consumer
4. **Изоляция ошибок и отказоустойчивость**: каждый поток — как отдельный актор (актерная модель)

---

 **Вывод**:
Многопоточность влияет не только на реализацию, но и на архитектуру: приходится учитывать взаимодействие, синхронизацию и независимость компонентов уже на этапе проектирования.


