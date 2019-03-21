# Лабораторна робота № 4. Робота з даними на мобільній платформі

**Мета роботи:** ознайомитися з методами роботи з даними на мобільній платформі, реалізувати збереження даних у власному додатку

**Хід роботи:**
1) Ознайомитися з основними засобами зберігання даних на мобільній платформі.
2) Розробити засоби збереження даних для власної мобільної програми (обраної предметної області).
3) Реалізувати допоміжний клас для роботи з власною базою даних.
4) Написати звіт.

Приклад роботи з базою даних:
Android/AndroidLocalDatabase/app/src/main/java/ua/lpnu/testshop/LocalDatabase.java

## Трохи теорії

Для збереження даних мобільного додатку є низка можливостей, зокрема:

- файлова система мобільного пристрою;
- медіатекам обільного пристрою;
- реляційна база даних мобільного пристрою;
- база налаштувань мобільного додатку;
- локальні дані веб сайту (у випадку веб-додатку);
- віддаленесховищефайлів;
- віддалене сховище таблиць;
- віддалена реляційна базаданих;
- віддалений провайдер BaaS (Backend as a Service).

Використання **файлової системи** мобільного пристрою є найпростішим і найшвидшим методом збереження даних на мобільному пристрої. Таке сховище часто використовується для збереження тимчасових даних та мультимедіа для внутрішнього використання у додатку. Проте таке сховище має деякі особливості, які потрібно врахувати.

1. Операційна система не гарантує атомарність запису у файл. Блок даних, який записується, може бути частково записаний, наприклад, у випадку закінчення місця на пристрої або при аварійному вимкненні.
2. Такі файли можуть бути доступні лише з додатку, який їх створює. Для надання доступу до них з інших додатків необхідно реалізовувати додаткові сервіси.
3. Ці файли захищенні від зчитування лише на пристрої, який не був взламаний (рутований).
4. При оновленні додатку необхідно врахувати, що попередня версія додатку могла записувати файли в іншому форматі.

**Медіатека мобільного пристрою** дає змогу зберігати файли таким чином, щоб вони були доступні користувачу. Такі файли будуть доступні іншим додаткам, яким користувач також дозволив доступ до медіатеки. Файли у медіатеку додаються атомарно (вони або потрапляють туди у потрібному форматі, або не додаються).

Медіатека має такі особливості.
1. Формат файлів обмежений форматами, які підтримує операційна система мобільного пристрою. Це обмежує формати зображень, аудіо, відео, тощо;
2. Процес відкриття і збереження файлів регламентується операційною системою і дозволами, які користувач надав додатку.
Реляційна база даних мобільного додатку добре підходить для збереження структурованих даних мобільного додатку, для яких важливе збереження цілісності. Бібліотека бази даних гарантує атомарність транзакцій, але запис даних відбувається значно повільніше, ніж просто у файл.

**Реляційна база даних** мобільного додатку має такі особливості.
1. Використовується база даних SQLite;
2. Бібліотека SQLite гарантує атомарність запису даних і збереження цілісності
відношень бази даних;
3. Формування SQL команд бажано робити з використанням бібліотек, які
гарантують правильне форматування SQL запитів. Зокрема, правильне подання стрічок, які містять всередині символи одинарної або двійної лапки. Такі бібліотеки наявні як на Android (android.arch.persistence.room, android.database.sqlite) так і на iOS (CoreData);
4. Запис відбувається значно повільніше, ніж просто у файл.

**База налаштувань мобільного додатку** може містити набір невеликої кількості стрічок ключ-значення, для яких гарантується атомарність запису множини ключів. Таке сховище добре підходить для збереження налаштувань додатку і невеликих за обсягом даних. Для вразливих даних необхідно використовувати шифрування, оскільки на зламаному пристрої інші програми можуть отримати доступ до файлів налаштувань вашої програми.

База налаштувань мобільного додатку має такі особливості:
1. Зберігає лише стрічки невеликої довжини;
2. Система гарантує атомарність запису даних;
3. Кількість записів обмежена;
4. Не зручна для збереження масивів.

Викладені вище методи збереження даних стосувалися збереження даних на пристрої користувача. **Віддалене збереження** даних мобільного додатку може бути необхідне у наступних випадках.

1. Надання користувачу можливості відновити дані після втрати або поломки мобільного пристрою;
2. Надання доступу до даних з інших пристроїв;
3. Спільне використання даних різними користувачами;
4. Необхідність збирання даних користувачів для аналітики;
5. Необхідність працювати з великими об'ємами даних, які перевищують розмір
карти пам'яті пристрою;
6. Необхідність отримати доступ до великих обчислювальних потужностей. Віддалено зберігати дані можна у різному вигляді.

Найчастіше для цього використовуються хмарні сховища файлів, хмарні табличні сховища і реляційні бази даних. Також можна використовувати власний сервер для збереження даних, але таке рішення складно масштабувати при зростанні кількості користувачів.

Кожне з хмарних сховищ має свої особливості використання.

Зокрема **хмарне сховище файлів** найдешевше у використанні, дозволяє під'єднати сервіси CDN (Content delivery network) для швидкого доступу з різних куточків світу, дає змогу використовувати токени авторизації для надання доступу до файлів певним групам користувачів на певний час з обмеженням на розмір запису.

З іншого боку, файлове сховище не підтримує одночасне редагування файлу декількома користувачами і не має жодних можливостей перевірити файл на відповідність певному формату.

Таким чином, файлове сховище найдоцільніше використовувати для збереження великих бінарних даних, які один раз записуються, а потім часто використовуються (аудіо, фото, відео, тощо).

Для структурованих даних, які змінюються різними користувачами, варто використовувати  або **табличні сховища** або **реляційні бази даних**.

**Табличні сховища** дають змогу розміщати дані у вигляді великих таблиць ключ-значення, у яких ключ складається з двох частин: ключ розділу (PartitionKey) і ключ рядка (RowKey). При цьому значення, які мають однаковий ключ розділу гарантовано зберігаються на одному сервері. Табличне сховище гарантує атомарність операцій лише в межах одного розділу. Щоб гарантувати відсутність колізій при зміні декількох розділів, необхідно реалізовувати стратегію **компенсуючих транзакцій** (Compensating Transaction pattern), що вимагає реалізації складніших процедур керування сховищем.

Переваги табличного сховища.

1. Просте масштабування і висока швидкодія за умови правильного вибору ключа розділу
і відсутності значної кількості транзакцій, які одночасно намагаються змінити одні і ті самі рядки даних;
2. Можливість зберігати дані у JSON, XML, або будь-якому іншому форматі, що полегшує читання і запис;
3. Наявність часових міток записів, що спрощує процедуру синхронізації записів при одночасному записі даних різними користувачами;
4. Невелика ціна у провайдерів хмарного сервісу.

Недоліки табличного сховища:

1. Відсутність перевірки правильного формату даних;
2. Відсутність перевірки цілісності посилань між об'єктами;
3. Необхідність реалізації компенсуючих транзакцій при зміні даних у декількох
розділах;
4. Необхідність перевіряти при записі часові мітки записів, щоб гарантувати збереження
змін, внесених у записи іншими користувачами;
5. Відсутність тригерів.

Частково недоліки табличних сховищ вирішені провайдерами **BaaS сервісів** (Firebase, Azure Mobile Services та ін.). BaaS провайдери додають засоби перевірки прав запису і читання, засоби перевірки формату даних, тригери. Табличні сховища добре підходять для збереження даних, між якими немає значної кількості відношень, цілісність яких варто перевіряти (логи, чати, нотатки, тощо).

**Хмарні реляційні сховища даних** надають всі переваги реляційних баз даних, зокрема гарантують ізоляцію транзакцій, перевірку цілісності даних, можливість використання тригерів. При правильному проектуванні реляційної бази даних, всі проблеми з перевіркою даних на відповідність моделі виконуються сервером бази даних, що зменшує імовірність пошкодження даних через помилку у додатку.

Реляційні бази даних, попри свою зручність, значно повільніші за табличні сховища і погано підходять для розроблення систем, які обслуговують значну кількість запитів від користувачів (більше, ніж 500 запитів на секунду). Реляційні сховища варто використовувати для збереження інформації, яка не часто змінюється, але цілісність якої вкрай важлива (аккаунти користувачів, права доступу, тощо).

Часто на вибір сховища даних впливає необхідність підтримки **транзакцій**.
Під транзакцією розуміється неподільна послідовність операторів маніпулювання даними (читання, видалення, вставки, модифікації), що приводить до одного з двох можливих результатів: або послідовність виконується, якщо всі оператори правильні, або вся транзакція відкидається, якщо хоча б один оператор не може бути успішно виконаний. Підтримка транзакцій гарантує цілісність інформації в базі даних і узгодженість транзакцій між собою. Узгодженість означає, що оператори однієї транзакції бачать базу даних у такому стані, як вона була до виконання інших транзакцій, або після них. Таким чином, транзакція переводить базу даних з одного цілісного стану в інший, та інші паралельні транзакції бачать базу в цілісному стані.

Підтримка механізму транзакцій – показник рівня розвиненості СУБД. Коректна підтримка одночасних транзакцій є основою забезпечення цілісності БД. Транзакції також складають основу ізольованості в багатокористувацьких системах, де з одної БД паралельно можуть працювати кілька користувачів або прикладних програм. Одна з основних завдань СУБД – забезпечення ізольованості, тобто створення такого режиму функціонування, при якому кожному користувачеві здавалося б, що БД доступна тільки йому. Таке завдання СУБД прийнято називати паралелізмом транзакцій.


Більшість дій над базою даних виконуються як транзакції. За замовчуванням кожна команда виконується як самостійна транзакція. При необхідності користувач може явно вказати початок і кінець транзакції, щоб мати можливість включити в неї кілька команд.


При виконанні транзакції система управління базами даних повинна дотримуватися певних правил обробки набору команд, що входять в транзакцію. Зокрема, розроблено чотири правила, відомі як вимоги **ACID**, вони гарантують правильність і надійність роботи системи.
ACID-властивості транзакцій
Характеристики транзакцій описуються в термінах **ACID (Atomicity, Consistency, Isolation, Durability: неподільність, узгодженість, ізольованість, стійкість)**.

1. Транзакція неподільна в тому сенсі, що є єдиним цілим. Всі її компоненти або мають місце, або ні. Не буває часткової транзакції. Якщо може бути виконана лише частина транзакції, вона відхиляється.
2. Транзакція є узгодженою, бо не порушує бізнес-логіки і відношень між елементами даних. Ця властивість дуже важлива при розробленні клієнт-серверних систем, оскільки в сховище даних надходить велика кількість транзакцій від різних систем і об'єктів. Якщо хоча б одна з них порушить цілісність даних, то всі інші можуть видати невірні результати.
3. Транзакція завжди ізольована, оскільки її результати самодостатні. Вони залежать лише від попередніх, але не від наступних, чи частково виконаних транзакцій.
4. Стійкість транзакції означає, що після свого завершення вона зберігається в системі, яку ніщо не може повернути у вихідний (до початку транзакції) стан, тобто відбувається фіксація транзакції, що означає, що її дія постійна, навіть при збоях системи. При цьому мається на увазі якась форма зберігання інформації в постійній пам'яті як частина транзакції.

Зазначені вище правила виконує сервер. Програміст лише вибирає потрібний рівень ізоляції, піклується про дотримання логічної цілісності даних і бізнес-правил. На нього покладаються обов'язки щодо створення ефективних і логічно вірних алгоритмів обробки даних. Він вирішує, які команди повинні виконуватися як одна транзакція, а які можуть бути розбиті на декілька послідовно виконуваних транзакцій. Слід по можливості використовувати невеликі транзакції, які мають якомога менше команд і змінюють мінімум даних. Дотримання цієї вимоги дозволить найбільш ефективним чином забезпечити одночасну роботу з даними безлічі користувачів.

Принципи ACID легко реалізуються у СУБД, які виконуються на одному сервері. Проте при розробленні розподілених СУБД підтримка ACID транзакцій часто означає значну втрату швидкодії, яка нівелює виграш від розподіленості. Через це, в другій половині 2000-х років, протиставляючи себе ACID, з'явилась **BASE-архітектура**.

**BASE-архітектура (від англ. Basically Available, Soft-state, Eventually consistent – базова доступність, нестійкий стан, узгодженість в результаті)** – підхід до побудови розподілених систем, в яких вимоги цілісності і доступності виконані не в повній мірі. При цьому такий підхід прямо протиставляється ACID. Під базовою доступністю мається на увазі такий підхід до проектування додатків, щоб збій в деяких вузлах приводив до відмови в обслуговуванні тільки для незначної частини сесій при збереженні доступності в більшості випадків. Нестійкий стан має на увазі можливість жертвувати довгостроковим зберіганням стану сесій (таких як проміжні результати вибірок, інформація про навігацію, контекст), при цьому концентруючись на фіксації оновлень тільки критичних операцій.

ACID-архітектура прекрасно підходить для структур, в яких потрібне строге виконання всіх правил проведення транзакцій в сенсі ACID на реляційних СУБД, і відсутність підтримки хоча б однієї з властивостей є критичним. Застосування BASE- архітектури доцільно при обробці величезних обсягів даних, опрацювання яких вимагає значного розпаралелювання операцій, а невдалі операції можуть бути відмінені через механізм **компенсуючих транзакцій**.

**Компенсуюча транзакція** – це набір операцій, які відновлюють стан бази даних до такого стану, як він був до виконання невдалої транзакції. Наприклад, транзакція купівлі квитка може складатися з операцій “забронювати місце”, “зняти гроші з картки”, “створити електронний квиток”, “позначити заброньоване місце, як куплене”. В цьому випадку компенсуюча транзакція буде містити операції “позначити куплене місце, як заброньоване”, “видалити електронний квиток”, “повернути гроші на картку”, “зняти бронювання місця”.

Альтернативою до механізму компенсуючих транзакцій є механізм **розподілених транзакцій**, який використовується для забезпечення ACID принципів у розподілених СУБД. Проте, при значному зростанні навантаження на базу даних і при виконанні багатьох запитів, які вимагають опрацювання різними вузлами розподіленої СУБД, швидкодія розподілених транзакцій стає значно гіршою за механізм компенсуючих транзакцій.

**Розподілена транзакція** – це транзакція, яка об'єднує в собі одразу кілька транзакцій, використовуючи для управління різними транзакціями загальний керуючий процес, що називається диспетчером транзакцій. Ідентично, як і звичайна транзакція, розподілена транзакція гарантує перебування системи в узгодженому стані. Найбільш поширений алгоритм управління розподіленими транзакціями полягає в використанні двофазної фіксації. При цьому спочатку виконується фаза голосування, під час якої кожен учасник розподіленої транзакції повідомляє диспетчеру транзакцій про те, чи вважає він, що його локальна транзакція може починатися. Якщо диспетчер транзакцій отримає позитивну відповідь від усіх учасників, він дає їм команду на початок транзакцій і виконує їх фіксацію. Якщо хоча б один сервіс відмовляє, виконується відкат транзакції на всіх серверах.
