Для записи данных в файл csv есть также два метода. Первый — это создать объект writer и передать в него:

- файловый объект, открытый для записи;
- разделитель.

Далее у этого объекта можно вызвать метод, записывающий одну строку, — writerow, тогда ему нужно передать подготовленный список свойств по всем полям. Или можно подготовить список списков с данными для всех строк и передать его методу writerows.

Важное замечание: при открытии файла на запись ему нужно передать дополнительный аргумент newline со значением — "" — пустая строка. Это нужно сделать, чтобы после каждой записанной в файл строки не было перевода курсора и не возникала лишняя пустая строка.

## Пример решения задачи (writer)

Пусть у нас есть список с данными: первый список содержит названия полей, остальные — значения по этим полям. Нужно написать программу, которая запишет эти данные в файл data.csv с разделителями — дефисами.

_Решение:_

Можно записывать данные построчно:

```python
import csv

data = [['id', 'field1', 'field2', 'field3'],
        ['123', 'data1', 'data2', '321'],
        ['124', 'data3', 'data4', '421'],
        ['125', 'data5', 'data6', '521'],
        ['126', 'data7', 'data8', '621']]
with open('data.csv', 'w', newline='') as out_file:
    writer = csv.writer(out_file, delimiter='-')
    for row in data:
        writer.writerow(row)
```

А можно сразу всё целиком:

```python
import csv

data = [['id', 'field1', 'field2', 'field3'],
        ['123', 'data1', 'data2', '321'],
        ['124', 'data3', 'data4', '421'],
        ['125', 'data5', 'data6', '521'],
        ['126', 'data7', 'data8', '621']]
with open('data.csv', 'w', newline='') as out_file:
    writer = csv.writer(out_file, delimiter='-')
    writer.writerows(data)
```

Для записи также есть объект, содержащий словари для каждой записи в данных. Это объект DictWriter. Ему, кроме файлового объекта и разделителя, нужно ещё передать заголовки файла (аргумент fieldnames), чтобы он мог на их основе создать ключи словарей.

Но и передавать ему нужно либо по одному словарю для каждой записи при использовании метода writerow(), либо список словарей, если использовать writerows().

Кроме того, нужно отдельной командой записать строку заголовков, автоматически она не запишется. Для этого есть метод writeheader().

## Пример решения задачи (DictWriter)

Для записи данных из списка в прошлом примере в файл data.csv с помощью DictWriter нужно немного подготовить данные. Во-первых, заведём отдельную переменную для заголовков (просто для удобства) и каждый вложенный список будем преобразовывать в словарь. Ниже показаны примеры построчной записи в файл и сразу всем списком словарей.

Вот так запишем данные по одной строке:

```python
import csv

data = [['id', 'field1', 'field2', 'field3'],
        ['123', 'data1', 'data2', '321'],
        ['124', 'data3', 'data4', '421'],
        ['125', 'data5', 'data6', '521'],
        ['126', 'data7', 'data8', '621']]
with open('data.csv', 'w', newline='') as out_file:
    headers = data[0]
    writer = csv.DictWriter(out_file, delimiter='-', fieldnames=headers)
    writer.writeheader()
    for item in data[1:]:
        temp_dic = {headers[i]: item[i] for i in range(len(headers))}
        writer.writerow(temp_dic)
```

А вот так сразу все строки:

```python
import csv

data = [['id', 'field1', 'field2', 'field3'],
        ['123', 'data1', 'data2', '321'],
        ['124', 'data3', 'data4', '421'],
        ['125', 'data5', 'data6', '521'],
        ['126', 'data7', 'data8', '621']]
headers = data[0]
temp_list = []
for item in data[1:]:
    temp_list.append({headers[i]: item[i] for i in range(len(headers))})
  
with open('data.csv', 'w', newline='') as out_file:
    writer = csv.DictWriter(out_file, delimiter='-', fieldnames=headers)
    writer.writeheader()
    writer.writerows(temp_list)
```