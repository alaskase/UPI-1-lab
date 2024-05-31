# UPI-1-lab
# Работу выполнил Росляков Владислав Александрович 
# Группа ББМО-01-23
# Вариант 24( 9, 2 ) 

##	Задание №1. Написать функцию, которая объединяет два одномерных массива в один, состоящих только из элементов входящих в первый массив
## Использую язык Python: 
 ```python
def merge_arrays():
    array1 = input("Введите элементы первого массива через пробел: ").split()
    array2 = input("Введите элементы второго массива через пробел: ").split()

    # Преобразование ввода в целочисленные значения
    array1 = [int(elem) for elem in array1]
    array2 = [int(elem) for elem in array2]

    # Оставляем только элементы, входящие в первый массив
    merged_array = [elem for elem in array2 if elem in set(array1)]

    return merged_array

result = merge_arrays()
print("Результирующий массив, содержащий элементы из второго массива, входящие в первый:", result)
```
### В данной функции пользователь вводит элементы двух массивов через пробел, после чего программа объединяет эти массивы, оставляя только элементы, которые встречаются в первом массиве

## Задание №2.	Вывод виде таблицы ФИО всех преподавателей, в зачетке
```python
import sqlite3
from tabulate import tabulate

# Функция для получения данных из консоли
def read_data_from_console():
    teachers_data = []
    print("Введите ФИО преподавателя (для завершения нажмите Enter без ввода данных):")
    while True:
        first_name = input("Имя: ")
        if not first_name:
            break
        last_name = input("фамилия: ")
        teachers_data.append({"first_name": first_name, "last_name": last_name})
    return teachers_data

# Функция для чтения данных из файла
def read_data_from_file(file_path):
    with open(file_path, "r") as file:
        teachers_data = [{"first_name": line.split()[0], "last_name": line.split()[1]} for line in file.readlines()]
    return teachers_data

# Функция для чтения данных из базы данных SQLite
def read_data_from_sqlite(db_path):
    conn = sqlite3.connect(db_path)
    cursor = conn.cursor()
    cursor.execute("SELECT first_name, last_name FROM teachers")
    teachers_data = [{"first_name": row[0], "last_name": row[1]} for row in cursor.fetchall()]
    conn.close()
    return teachers_data

# Выбор источника данных
data_source = input("Выберите источник данных (1 - консоль, 2 - файл, 3 - БД SQLite): ")

if data_source == "1":
    teachers = read_data_from_console()
elif data_source == "2":
    file_path = input("Укажите путь к файлу: ")
    teachers = read_data_from_file(file_path)
elif data_source == "3":
    db_path = input("Укажите путь к БД SQLite: ")
    teachers = read_data_from_sqlite(db_path)
else:
    print("Некорректный выбор источника данных!")

# Вывод таблицы
table_headers = ["Имя", "Фамилия"]
fio_data = [[teacher["first_name"], teacher["last_name"]] for teacher in teachers]
print(tabulate(fio_data, headers=table_headers, tablefmt="grid"))
```

