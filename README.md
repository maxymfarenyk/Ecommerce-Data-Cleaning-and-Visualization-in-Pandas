# Ecommerce-Data-Cleaning-and-EDA-in-Pandas
У цьому файлі представлено покроковий опис виконання проєкту з дата клінінгу та EDA у Pandas
## 1. Завантаження [датасету із Kaggle](https://www.kaggle.com/datasets/nynkeugerard/dirty-ecommerce-data-eda-r)
В цьому датасеті багато даних, які навмисно були згенеровані з великою кількістю помилок для того, щоб попрактикуватись в очищенні даних
## 2. Імпорт необхідних бібліотек та датасету

![image](https://github.com/user-attachments/assets/e6c25343-6edf-4c31-956d-d7565ec172e5)

Після імпорту всього необхідного бачимо таблицю

![image](https://github.com/user-attachments/assets/12b6fad6-e6c4-4c07-8563-c7f3b5b8e3d9)

Також одразу можна переглянути коротку інформацію про цю таблицю

![image](https://github.com/user-attachments/assets/fec5e598-0c1c-4726-948a-c34187b13431)

Одразу кидається в очі, що є стовпці, які містять тип даних float, хоча мали б бути типу int, оскільки є цілими числами. Це виправимо у наступних кроках
## 3. Очищення даних
### 1) Видалення дублікатів
Видаляємо усі ідентичні рядки

![image](https://github.com/user-attachments/assets/457dd32d-ef69-44cc-bfb7-902345025dbf)

Після видалення дублікатів бачимо, що кількість записів зменшилась на 500 

![image](https://github.com/user-attachments/assets/7c0de272-3f01-4042-b3c4-e9606a76f070)

Оскільки існували повністю ідентичні рядки, отже попадались і повтори серед OrderID, які мали би бути унікальними значеннями. 

### 2) Унікальність OrderID
Перевіримо кількість унікальних OrderID 

![image](https://github.com/user-attachments/assets/69a08dec-aac6-48f6-a85e-dfe8f66e88b5)

Всього є 9500 замовлень, а унікальних OrderID лише 6123, що порушує умову унікальності. Тому необхідно створити новий стовпець OrderID, де всі значення будуть унікальними

![image](https://github.com/user-attachments/assets/798996b1-d53a-46f8-818a-c24e08a1b4cc)

Було створено додатковий DataFrame df2 для того щоб випадково не втратити наявні дані. Після чого у ньому ж створено стовпець NewOrderID, який заповнюється унікальними значеннями починаючи від 100000

![image](https://github.com/user-attachments/assets/4fff20bd-2e97-459d-9126-6c9c428c5491)

Замінюємо значення OrderID на значення із стовпця NewOrderID, після чого видаляємо останній. При перевірці на унікальність бачимо, що всі ідентифікатори унікальні

### 3) Приведення даних до правильних типів

![image](https://github.com/user-attachments/assets/34bbd4d0-dd98-449d-bb83-0880d372b701)

Помітно, що багато стовпців містять дані типу object та float, але для багатьох стовпців ці типи даних не підходять, тому замінимо їх підходящими.

#### а) Datetime

Перетворимо дані, що пов'язані із часом у формат Datetime

![image](https://github.com/user-attachments/assets/afce5b7a-a8b3-4d23-8fe7-688b0a7b75ad)

#### б) Category

Переглянемо унікальні даних у тих стовпцях, де їх мало б бути мало. 

![image](https://github.com/user-attachments/assets/cd704c61-1b09-4419-a306-ca95f5efacca)
![image](https://github.com/user-attachments/assets/0f939d92-9b89-4833-98ea-2fd231cc7ada)

Ці стовпці варто віднести до типу Category, тому що цей тип підходить для даних, де мало унікальних значень.

![image](https://github.com/user-attachments/assets/1c60da27-7fd9-44a3-9fdd-6b507a89a382)

#### в) Integer

Перетворимо дані що представляють цілі числа у тип Int64

![image](https://github.com/user-attachments/assets/420d6f3f-56b8-4384-8c41-58d6c7483685)

#### г) String

Для поштового індексу краще мати рядковий тип, оскільки поштовий індекс може містити нулі на початку. Але у нашому випадку поштові індекси не є інформативними, оскільки в більшості випадків не відповідають місту яким позначені. Тому цей стовпець краще видалити.

![image](https://github.com/user-attachments/assets/54d006ba-2370-4ea9-b984-04969b1dc77f)

#### Правильні типи

Тепер усі дані приведені до правильних типів

![image](https://github.com/user-attachments/assets/3beae764-793b-4c98-9251-16fa280b9330)

Можна помітити, що є int64 та Int64 типи даних. Різниця між ними полягає у тому, що Int64 допускає відсутність даних, а int64 ні. Це важливо у нашому випадку, тому що primary key не може мати порожніх значень.

### 4) Локація

Подивимось на стовпці, що відповідають за локацію

![image](https://github.com/user-attachments/assets/1e552d95-987d-42f6-bce1-4f293c842bab)

Як бачимо, значення не співпадають між собою, тому потрібно змінити їх так, щоб усе було правильно.

#### а) Видалення зайвого
Я помітив, що у стовпці регіон вказано лише сторони горизонту і ці дані не мають ніякого сенсу, оскільки незрозуміло до чого у них прив'язка. Тому цей стовпець також варто видалити.

![image](https://github.com/user-attachments/assets/19840453-86ac-4e74-bca0-7f3caf3e491f)

#### б) Зміна помилкових назв категорій
Спершу додамо до City категорію Unknown, замінимо всі входження InvalidCity на Unknown та видалимо зайві категорії

![image](https://github.com/user-attachments/assets/bf7e6865-60eb-4157-be9c-19e81821706e)

#### в) Заповнення порожніх значень
Тепер заповнимо усі порожні значення

![image](https://github.com/user-attachments/assets/7058a9c5-c5de-428a-af81-3a4e477e5ade)
![image](https://github.com/user-attachments/assets/39a07645-e8e5-4461-85a6-9315d0c2dd5b)

#### г) Налаштування відповідності

Для того, щоб усі дані щодо локації були правильні напишемо функцію, що виправлятиме неправильні значення.
Для початку зробимо словники із правильними відношеннями.

![image](https://github.com/user-attachments/assets/db2c3e00-28fc-45f4-9447-051e43379895)

Після цього потрібно написати власне функцію. Щоб перевіряти чи правильна локація спершу будемо дивитись на місто, якщо воно відоме, то за словниками заповнимо відповідну країну та штат. Якщо відомий лише штат, то зі словників дізнаємось лише країну. А якщо відома лише країна, то точнішої інформації дістати уже не вийде.

![image](https://github.com/user-attachments/assets/9a25c939-fa52-4467-b91c-6db23486aa5b)

Виправлені дані запишемо уже в новий датафрейм df3. 

#### Правильні дані локації
Тепер порівняємо

![image](https://github.com/user-attachments/assets/2784a727-8a17-4cbe-a663-c16b4250fe7d)
![image](https://github.com/user-attachments/assets/181e5cf2-251b-4441-9f6d-943dba05443f)

Як бачимо, тепер інформація про локацію відображається коректно.

### 5) Товари

![image](https://github.com/user-attachments/assets/15216c48-4b5d-4f08-bbaa-292ebe92e47f)

Спершу потрібно видалити помилкові назви категорій та заповнити пропуски.

#### а) Зміна помилкових назв категорій

![image](https://github.com/user-attachments/assets/3c909d25-7c77-40f8-b34f-5ff94beb5652)

![image](https://github.com/user-attachments/assets/b8695558-e34a-4f44-8a7b-c970cd5ca41b)

#### б) Заповнення порожніх значень

Додаємо до кожної множини категорій категорію Unknown

![image](https://github.com/user-attachments/assets/5626d7c9-512b-4a2e-b8b6-d673d0c9b300)

Після цього заповнюємо всі пропуски цим значенням

![image](https://github.com/user-attachments/assets/81ebfa19-ca53-4717-9e22-776f19bad9b9)

Тепер подивимось що вийшло

![image](https://github.com/user-attachments/assets/d4ee5040-5171-4173-bb0a-eaea9baa89c8)

Бачимо, що товари не всюди відповідають категоріям чи підкатегоріям. Тому треба налаштувати правильну відповідність по аналогії із даними локації.

#### в) Налаштування відповідності

Спершу зробимо словники відповідностей.

![image](https://github.com/user-attachments/assets/02da9999-7a94-45c2-8c08-a8c8520466e0)

Після цього напишемо функцію аналогічну до попередньої

![image](https://github.com/user-attachments/assets/298eff13-abd3-4256-a653-27dd8c0323ff)

А зараз запишемо виправлені дані у новий датафрейм та порівняємо їх

![image](https://github.com/user-attachments/assets/efcdb80a-5742-4590-9329-45cca1214a3e)

![image](https://github.com/user-attachments/assets/3ef047f5-a23f-480f-829e-f84aad94bdd1)

Тепер бачимо, що всі відповідності між категоріями правильні. 

#### г) Ідентифікатор

Ідентифікатором товару у нас є ProductID. Ідентифікатор повинен бути унікальним для кожного окремого товару, тому для початку подивимось на кількість унікальних айді

![image](https://github.com/user-attachments/assets/35e16222-5372-46ac-b1d3-fb92c7da80f8)

У нас є 1000 унікальних айді, а унікальних товарів 5 (із явно вказаних), а також є товари із невідомою назвою. 
Подивимось скільки їх

![image](https://github.com/user-attachments/assets/f1894119-e3e4-4dd7-897e-844c4e31b0f2)

Отже у нас 1600 значень, кожне з яких мало б мати унікальний айді, яких всього 1000. 
Переглянемо для прикладу товари з айді 3225.

![image](https://github.com/user-attachments/assets/3cb88a8f-4819-4367-ae05-da8379aa8c58)

Бачимо, що однаковий ідентифікатор присвоєно багатьом різним товарам, а отже цей ідентифікатор не є коректним і стовпець можна видалити.

![image](https://github.com/user-attachments/assets/e38c371e-4dfd-4cc4-8bca-db4c65f241f8)

### 6) Дати замовлення та доставки

Перевіримо дати замовлення та доставки. Доставка не може відбутись перед замовленням, тому подивимось чи є записи де дата доставки менша за замовлення

![image](https://github.com/user-attachments/assets/9519fbca-389f-4f42-ba61-ec41927a5e8c)

Бачимо, що багато помилкових даних. Щоб це виправити змінимо дату замовлення, щоб вона дорівнювала даті доставки.

![image](https://github.com/user-attachments/assets/1b015348-3338-4b49-a62c-9b07b1c0ce2d)

### 7) Персональні дані клієнтів

Подивимось на персональні дані клієнтів.

![image](https://github.com/user-attachments/assets/22df693a-7191-407d-858d-3b0f3c2fb5d0)

По-перше, бачимо що є порожні дані. По-друге, є імена що повторюються, але в них різні айді та сегменти.

#### а) Перевірка імен
Поглянемо для прикладу дані клієнта з іменем David White.

![image](https://github.com/user-attachments/assets/7fca83a7-b121-4a62-bc3d-e6a099bf0628)

Бачимо що дуже багато неспівпадінь, як із сегментом, так і з айді. Тому стовпець імені не є інформативним і його варто видалити.

![image](https://github.com/user-attachments/assets/25eabfad-992a-4a96-89a9-5f2aa9b30f61)

#### б) Заповнення порожніх значень

Спершу подивимось скільки порожніх значень серед айді.

![image](https://github.com/user-attachments/assets/eb7defa7-6b07-4bd1-a4e6-b89b039600a5)

Заповнимо їх наступними значеннями після максимального.

![image](https://github.com/user-attachments/assets/024f1def-c7da-4f21-abc7-344c039c8c18)

Тепер перейдемо до Segment.

![image](https://github.com/user-attachments/assets/c2a4820a-58bd-480f-bf3e-3d92e8bdf6a5)

Подивимось унікальні значення.

![image](https://github.com/user-attachments/assets/e054d4e4-487b-4e36-a7b5-e460ce5b1d31)

Замінимо пропуски значенням Undefined.

![image](https://github.com/user-attachments/assets/16054a54-d6ba-4270-a874-3535a60d9d39)

### 8) Числові дані та оплата

Подивимось на деякі значення із цих стовпців.

![image](https://github.com/user-attachments/assets/7146cab2-6fa3-42b0-a3e4-7d8578b3d690)

#### а) Sales і Quantity
Спершу подивимось чи є у стовпці Sales значення менші 0.

![image](https://github.com/user-attachments/assets/43a079f5-0151-4dcb-a787-393513b6594e)

Можливо від'ємні значення означають повернення товару, але для цього від'ємною має бути й кількість.
Подивимось скільки є рядків де від'ємні значення у двох колонках.

![image](https://github.com/user-attachments/assets/30ed243c-95cd-43fc-8787-f725321d6c7e)

Отже ці дані означають повернення, а всі інші (де лише значення одного з двох стовпців від'ємне) можна видалити.

![image](https://github.com/user-attachments/assets/00518716-150a-4f49-81f5-24cfd46d3192)

Також перевіримо чи є пропущені дані.

![image](https://github.com/user-attachments/assets/25acd9de-a953-40b3-afda-019482bc048e)

Отже, серед цих стовпців пропущених даних немає.

#### б) Discount
Подивимось які є унікальні значення. Знижка у відсотках може бути в діапазоні від 0 до 100.

![image](https://github.com/user-attachments/assets/2ffc4f83-2e68-47b9-aa84-45acd2939bc6)

Отже, тут є значення 110 і -10, які швидше за все значать просто 10%, але було допущено помилку при записі.
Замінимо ці дані на 10.

![image](https://github.com/user-attachments/assets/bcccde40-636a-443a-9376-25d675517dc1)

І заповнимо NA нулями.

![image](https://github.com/user-attachments/assets/7a49167f-a144-45d2-94db-cf53e5da259e)
![image](https://github.com/user-attachments/assets/bd37b546-f5cc-404d-abe9-54acc40aed1a)

#### в) Profit і ShippingCost
Також подивимось на унікальні значення.

![image](https://github.com/user-attachments/assets/5ebab924-0f3b-4661-b574-b5628403fd2d)

Також заповнимо Na нулями.

![image](https://github.com/user-attachments/assets/989f382d-c7ec-4e9e-8fa8-e7ee2d2d885b)
![image](https://github.com/user-attachments/assets/0021fb98-bf19-4a1a-9555-0919370bc265)

З ShippingCost робимо те саме

![image](https://github.com/user-attachments/assets/271a22fb-f039-44bc-b5ab-fbdcf672c02b)
![image](https://github.com/user-attachments/assets/c8ff1ca2-4109-4444-8593-e4ee272c2420)

#### г) CustomerRating
Подивимось які є рейтинги у покупців.
![image](https://github.com/user-attachments/assets/8a4aa19f-2e10-490f-ab02-4c3180fb00ef)

Важко визначити які межі цього оцінювання, тому що найбільшим значенням є 100, а найменшим -1. 
Щоб перевірити чи це одиничні помилки переглянемо скільки є рядків з різними рейтингами.

![image](https://github.com/user-attachments/assets/51d4f09a-e9d3-4ff9-8ac9-cc8a5b84ff4b)

Бачимо, що це не одиничні помилки, тому нам не зрозуміло яка тут шкала оцінювання і тому цей стовпець не є інформативним і його можна видалити.

![image](https://github.com/user-attachments/assets/4627b0b9-dc4c-4e49-890f-83434503e515)

#### ґ) PaymentMethod

![image](https://github.com/user-attachments/assets/ef6eb1f6-77c3-4675-948f-31aa3429efd6)

Заповнюємо Na значенням Undefined.

![image](https://github.com/user-attachments/assets/1f7f2f18-153d-4b5e-928f-6b25a58d2c55)

## 4. Аналіз даних

Після очищення даних збережемо цей датафрейм у csv файл

![image](https://github.com/user-attachments/assets/a2b1a71c-ec74-4975-b6dc-001002d1515a)

