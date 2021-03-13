# IF2240 Praktikum-1

## 1. Buatlah query yang menampilkan nama lengkap (full_name), tanggal lahir, jenis kelamin, dan nama departemen dari manajer perusahaan yang mulai bekerja pada tahun 1985!
```mysql
SELECT CONCAT(a.first_name, ' ', a.last_name) as full_name, a.birth_date, a.gender, c.dept_name 
FROM employees as a, dept_manager as b, departments as c 
WHERE a.emp_no = b.emp_no and b.dept_no = c.dept_no and year(a.hire_date) = 1985;
```
```mysql
+-----------------------+------------+--------+--------------------+
| full_name             | birth_date | gender | dept_name          |
+-----------------------+------------+--------+--------------------+
| DeForest Hagimont     | 1957-07-08 | M      | Development        |
| Ebru Alpin            | 1959-10-28 | M      | Finance            |
| Shirish Ossenbruggen  | 1953-06-24 | F      | Human Resources    |
| Krassimir Wegerle     | 1956-06-08 | F      | Production         |
| Peternela Onuegbe     | 1961-03-14 | F      | Quality Management |
| Przemyslawa Kaelbling | 1962-02-24 | M      | Sales              |
+-----------------------+------------+--------+--------------------+
6 rows in set (0.00 sec)
```

## 2. Buatlah query untuk menampilkan nama lengkap, title, dan tanggal bergabung pegawai yang bekerja di departemen Quality Management sejak bulan Juli 2002.
```mysql
SELECT a.first_name, a.last_name, b.title, a.hire_date 
FROM employees as a, titles as b, dept_emp as c, departments as d 
WHERE a.emp_no = b.emp_no and a.emp_no = c.emp_no and c.dept_no = d.dept_no and d.dept_name = 'Quality Management' and a.hire_date like'2002-07%';
```
```mysql
Empty set (0.04 sec)
```

## 3. Buatlah query untuk menampilkan 10 nama pegawai tertua dan tanggal lahirnya yang jenis kelaminnya tidak bernilai null.
```mysql
SELECT CONCAT(first_name, ' ', last_name) as name 
FROM employees WHERE gender is not null and birth_date is not null ORDER BY birth_date asc limit 10;
```
```mysql
+----------------------+
| name                 |
+----------------------+
| Moni Decaestecker    |
| Ronghao Schaad       |
| True Denny           |
| Toshimi Bruckman     |
| Palash Hempstead     |
| Shigehito Sommer     |
| Reinhold Savasere    |
| Xiaoqiu Denis        |
| Marek Macedo         |
| Theirry Schneeberger |
+----------------------+
10 rows in set (0.07 sec)
```

## 4. Buatlah query yang menampilkan gender dan rata-rata gaji untuk setiap gender
```mysql
SELECT a.gender, avg(b.salary) as 'rata-rata gaji'
FROM employees as a, salaries as b
WHERE a.emp_no=b.emp_no GROUP BY gender;
```
```mysql
+--------+----------------+
| gender | rata-rata gaji |
+--------+----------------+
| M      |     63870.0437 |
| F      |     63910.4318 |
+--------+----------------+
2 rows in set (2.20 sec)
```

## 5. Buatlah query untuk memperlihatkan rata-rata gaji saat ini dari pegawai di departemen d009 berdasarkan nama belakangnya! (Hint: gunakan CURDATE() untuk mendapatkan tanggal hari ini)
```mysql
SELECT a.last_name, avg(b.salary) as 'rata-rata gaji' 
FROM employees as a, salaries as b, dept_emp as c 
WHERE CURDATE() between b.from_date and b.to_date and a.emp_no = b.emp_no and a.emp_no = c.emp_no and dept_no = 'd009' GROUP BY a.last_name;
```
```mysql
+------------------+----------------+
| last_name        | rata-rata gaji |
+------------------+----------------+
| Hutton           |     64278.6667 |
| Uhrig            |     90161.0000 |
| Yoshimura        |     70007.0000 |
| Mahmud           |     58152.0000 |
| Moehrke          |     82430.0000 |
| Lamba            |     87481.0000 |
| Koprowski        |     68422.0000 |
| Jiafu            |     60814.0000 |
| Glinert          |     57229.0000 |
| Kieras           |     68042.0000 |
| Majewski         |     46679.0000 |
| Zockler          |     98950.0000 |
| Erni             |     50634.0000 |
+------------------+----------------+
1508 rows in set (0.15 sec)
```












