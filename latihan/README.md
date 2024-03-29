# LATIHAN PRAKTIKUM IF2240

## Database Schema
![Database Schema](https://github.com/mgstabrani/praktikum-basdat/blob/main/latihan/Screenshot%20from%202021-03-14%2011-33-30.png)

## 1. Buatlah query untuk menampilkan seluruh data kontinen.
```mysql
SELECT * 
FROM continents;
```
```mysql
+--------------+---------------+
| continent_id | name          |
+--------------+---------------+
|            1 | North America |
|            2 | Asia          |
|            3 | Africa        |
|            4 | Europe        |
|            5 | South America |
|            6 | Oceania       |
|            7 | Antarctica    |
+--------------+---------------+
7 rows in set (0.00 sec)
```

## 2. Buatlah query untuk menampilkan nama negara dan luasnya dengan luas lebih dari 10000000 satuan.
```mysql
SELECT name, area
FROM countries
WHERE area > 10000000;
```
```mysql
+--------------------+-------------+
| name               | area        |
+--------------------+-------------+
| Antarctica         | 13120000.00 |
| Russian Federation | 17075400.00 |
+--------------------+-------------+
2 rows in set (0.00 sec)
```

## 3. Buatlah query untuk menampilkan nama-nama negara yang menggunakan bahasa Malay.
```mysql
SELECT countries.name 
FROM countries, country_languages, languages 
WHERE countries.country_id=country_languages.country_id and country_languages.language_id=languages.language_id and language='Malay';
```
```mysql
+-------------------------+
| name                    |
+-------------------------+
| Brunei                  |
| Cocos (Keeling) Islands |
| Indonesia               |
| Malaysia                |
| Singapore               |
| Thailand                |
+-------------------------+
6 rows in set (0.01 sec)
```
