# Find_distanse_python_postgres
Вычисление расстояния между любыми пунктами планеты. Используются географические координаты широты и долготы.

Используется подключение из Python к Postgres, далее используется формула для вычисления расстояний с помощью синусов и косинусов. Напрямую из Python сделать так нет возможности, т.к. не обладаю данными, есть ли такие формулы непосредственно для Python.

```sql
import psycopg2
connect = psycopg2.connect(database="postgres",
                           user="postgres",
                           password="postgres")

coordinates_1 = input('Точка 1: ')
coordinates_2 = input('Точка 2: ')

longitude_1 = coordinates_1.split(",")[1]
latitude_1 = coordinates_1.split(", ")[0]
longitude_2 = coordinates_2.split(",")[1]
latitude_2 = coordinates_2.split(", ")[0]


def distanse(conn):
    with connect as conn:
        with conn.cursor() as cur:
            cur.execute("""select round((acos(sind(%s::numeric)*sind(%s::numeric) + cosd(%s::numeric)*cosd(%s::numeric)
            *cosd(%s::numeric - %s::numeric)) * 6371)::numeric, 1) as distanse""",
            (latitude_1, latitude_2, latitude_1, latitude_2, longitude_1, longitude_2))

            print(cur.fetchone())

distanse(connect)
```
