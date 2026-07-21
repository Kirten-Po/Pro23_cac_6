Чтобы прочитать из файла json сохранённый в нём объект, нужно использовать метод json.load(). Обязательным аргументом будет переменная с файловым объектом:

```python
import json

with open('example.json') as in_file:
    dic = json.load(in_file)
    print(dic)
# {'key': 'value', 'key1': 'value1'}
```

Но если json-объект был сохранён не в файл, а в строку, преобразовать его в объект _Python_ можно с помощью метода json.loads():

```python
s = '{"key": "value", "key1": "value1"}'
dic = json.loads(s)
print(type(dic))  # <class 'dict'>
```

При чтении данных проблем с кириллическими символами не возникает:

```python
s = '{"age": 13, "name": "Василий", "surname": "Иванов"}'
dic = json.loads(s)
print(dic)
# {'age': 13, 'name': 'Василий', 'surname': 'Иванов'}
```