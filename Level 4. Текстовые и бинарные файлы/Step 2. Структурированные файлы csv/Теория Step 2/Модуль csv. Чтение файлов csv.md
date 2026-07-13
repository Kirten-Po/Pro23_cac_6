Модуль csv содержит функции для чтения и записи данных в формате csv.

Прочитать данные можно в объект, похожий на список, — reader. Каждая строка данных разбивается по разделителю и записывается во вложенный список в том же порядке. Все данные имеют тип — строка.

При создании объекта reader ему нужно передать обязательные аргументы — имя файла и разделитель, а также необязательные — кодировку и другие.

```python
import csv

with open('example.csv') as file:
    reader = csv.reader(file, delimiter=';')
    print(*reader, sep='\n')
```

Пусть в файле example.csv содержатся данные:

id;field1;field2;field3
123;data1;data2;321
124;data3;data4;421
125;data5;data6;521
126;data7;data8;621

Тогда код, представленный выше, выведет:

['id', 'field1', 'field2', 'field3']
['123', 'data1', 'data2', '321']
['124', 'data3', 'data4', '421']
['125', 'data5', 'data6', '521']
['126', 'data7', 'data8', '621']

Метод возвращает итерируемый объект, поэтому строку заголовков можно убрать с помощью next().

```python
import csv

with open('example.csv') as file:
    reader = csv.reader(file, delimiter=';')
    next(reader)
    print(*reader, sep='\n')
```

## Пример решения задачи (reader)

**Задача «Зоопарк»**

В файле zoo.csv записана информация о некоторых животных зоопарка в формате (разделители — точка с запятой):

номер, кличка, вид животного, возраст, вольер

_no, name, mode, age, aviary_

Нужно написать программу, которая прочитает данные из файла и запишет информацию о животных в список.

Данные в файле для проверки решения:

```python
no;name;mode;age;aviary
11111;Jake;wombat;3;A
11112;Chip;capybara;0;A
11113;Jerry;wombat;5;A
11114;Elliot;wombat;3;B
11115;Hazel;marsupial wolf;1;A
11116;Calder;kangaroo;0;C
11121;Lark;quokka;3;B
11131;Neal;platypus;2;A
11122;Oskar;quokka;1;A
```

_Решение:_

```python
import csv

with open('zoo.csv') as in_file:
    reader = csv.reader(in_file, delimiter=';')
    next(reader)
    data = [[int(x[0]), x[1], x[2], int(x[3]), x[4]] for x in reader]
    print(*data, sep='\n')
```

Результатом будет такой вывод:

```python
[11111, 'Jake', 'wombat', 3, 'A']
[11112, 'Chip', 'capybara', 0, 'A']
[11113, 'Jerry', 'wombat', 5, 'A']
[11114, 'Elliot', 'wombat', 3, 'B']
[11115, 'Hazel', 'marsupial wolf', 1, 'A']
[11116, 'Calder', 'kangaroo', 0, 'C']
[11121, 'Lark', 'quokka', 3, 'B']
[11131, 'Neal', 'platypus', 2, 'A']
[11122, 'Oskar', 'quokka', 1, 'A']
```

Прочитать данные также можно с помощью класса DictReader, тогда каждая строка — запись — будет представлена в виде словаря с ключами, определяемыми заголовками, и значениями — свойствами этой записи по соответствующим полям. Сами словари записываются в подобие списка словарей, этот объект также является итератором. В таком варианте первая строка интерпретируется как заголовки, и словари будут составлены автоматически. Согласитесь, удобнее оперировать данными по названию, чем по расположению в строке высчитывать индекс нужного поля.

```python
import csv

with open('example.csv') as file:
    data = csv.DictReader(file, delimiter=';')
    print(*data, sep='\n')
```

Будет выведено:

{'id': '123', 'field1': 'data1', 'field2': 'data2', 'field3': '321'}
{'id': '124', 'field1': 'data3', 'field2': 'data4', 'field3': '421'}
{'id': '125', 'field1': 'data5', 'field2': 'data6', 'field3': '521'}
{'id': '126', 'field1': 'data7', 'field2': 'data8', 'field3': '621'}

Теперь строка заголовков нам не мешала, а даже была полезна, поэтому мы её не удаляли.

## Пример решения задачи (DictReader)

Решим задачу с файлом zoo.csv из прошлого примера, в которой нужно создать словарь со списками животных по каждому виду. Пусть значением будет количество животных данного вида.

Решение:

```python
import csv

with open('zoo.csv') as in_file:
    reader = csv.DictReader(in_file, delimiter=';')
    dic = {}
    for item in reader:
        dic[item['mode']] = dic.get(item['mode'], 0) + 1
    print(dic)
```

Получим такой словарь:

```python
{'wombat': 3,
 'capybara': 1,
 'marsupial wolf': 1,
 'kangaroo': 1,
 'quokka': 2,
 'platypus': 1}
```