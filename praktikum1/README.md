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

## 6. Buatlah query yang menampilkan nama departemen dan jumlah pegawai pada masing-masing departemen yang mulai menjabat di tahun 2000.
```mysql
SELECT b.dept_name, count(a.emp_no) as 'jumlah pegawai' 
FROM dept_emp as a, departments as b 
WHERE year(a.from_date) = 2000 and a.dept_no = b.dept_no GROUP BY b.dept_no;
```
```mysql
+--------------------+----------------+
| dept_name          | jumlah pegawai |
+--------------------+----------------+
| Marketing          |            103 |
| Finance            |             54 |
| Human Resources    |             37 |
| Production         |            250 |
| Development        |            160 |
| Quality Management |            108 |
| Sales              |            104 |
| Research           |            111 |
| Customer Service   |            184 |
+--------------------+----------------+
9 rows in set (0.23 sec)
```

## 7. Buatlah query untuk mencari departemen-departemen yang rata-rata gaji manajernya di atas 50.000. Tampilkan nama departemen dan rata-rata gaji manajer pada departemen tersebut!
```mysql
SELECT d.dept_name, avg(b.salary) as 'rata-rata gaji manager' 
FROM employees as a, salaries as b, dept_manager as c, departments as d 
WHERE a.emp_no = b.emp_no and a.emp_no = c.emp_no and c.dept_no = d.dept_no 
GROUP BY d.dept_no 
HAVING avg(b.salary) > 50000;
```
```mysql
+--------------------+------------------------+
| dept_name          | rata-rata gaji manager |
+--------------------+------------------------+
| Marketing          |             87570.5882 |
| Finance            |             72822.7778 |
| Human Resources    |             62990.2222 |
| Production         |             57052.3333 |
| Development        |             59658.1176 |
| Quality Management |             69934.6875 |
| Sales              |             85738.7647 |
| Customer Service   |             55565.5600 |
+--------------------+------------------------+
8 rows in set (0.01 sec)
```

## 8. Buatlah sebuah query yang menampilkan 10 baris pertama dari nomor karyawan, nama pertama, tanggal ulang tahun, gaji terbaru, dan diurutkan menurun berdasarkan gaji saat ini, kemudian menaik berdasarkan tanggal lahir!
```mysql
SELECT a.emp_no, a.first_name, a.birth_date, b.salary 
FROM employees as a, salaries as b 
WHERE a.emp_no = b.emp_no and CURDATE() between b.from_date and b.to_date 
ORDER BY b.salary desc, a.birth_date asc limit 10;
```
```mysql
+--------+------------+------------+--------+
| emp_no | first_name | birth_date | salary |
+--------+------------+------------+--------+
|  80823 | Willard    | 1963-01-21 | 154459 |
| 238117 | Mitsuyuki  | 1959-06-21 | 152220 |
|  46439 | Ibibia     | 1953-01-31 | 150345 |
|  66793 | Lansing    | 1964-05-15 | 150052 |
|  36219 | Vivian     | 1964-10-09 | 148820 |
| 232753 | Tadanori   | 1953-11-09 | 147480 |
|  91935 | Oldrich    | 1961-05-28 | 147469 |
|  44465 | Brigham    | 1955-06-04 | 146719 |
| 210661 | Boalin     | 1964-06-01 | 145902 |
|  18997 | Basim      | 1958-11-30 | 145215 |
+--------+------------+------------+--------+
10 rows in set (1.15 sec)
```

## 9. Buatlah query untuk menampilkan nama belakang pegawai non manajer yang memiliki nama belakang yang sama dengan manajer yang memiliki gaji di atas 90.000. CATATAN: Gunakan set operation.

## 10. Buatlah query untuk menampilkan 10 baris pertama dari semua data karyawan yang saat ini memiliki posisi sebagai engineer dan memiliki gaji kurang dari 50000.
```mysql
SELECT * 
FROM employees 
WHERE emp_no in (
SELECT emp_no 
FROM titles
WHERE title like '%Engineer' and emp_no in (
SELECT emp_no 
FROM salaries 
WHERE salary < 50000)) limit 10;
```
```mysql
+--------+------------+------------+------------+--------+------------+
| emp_no | birth_date | first_name | last_name  | gender | hire_date  |
+--------+------------+------------+------------+--------+------------+
|  10003 | 1959-12-03 | Parto      | Bamford    | M      | 1986-08-28 |
|  10027 | 1962-07-10 | Divier     | Reistad    | F      | 1989-07-07 |
|  10031 | 1959-01-27 | Karsten    | Joslin     | M      | 1991-09-01 |
|  10035 | 1953-02-08 | Alain      | Chappelet  | M      | 1988-09-05 |
|  10037 | 1963-07-22 | Pradeep    | Makrucki   | M      | 1990-12-05 |
|  10043 | 1960-09-19 | Yishay     | Tzvieli    | M      | 1990-10-20 |
|  10045 | 1957-08-14 | Moss       | Shanbhogue | M      | 1989-09-02 |
|  10051 | 1953-07-28 | Hidefumi   | Caine      | M      | 1992-10-15 |
|  10057 | 1954-05-30 | Ebbe       | Callaway   | F      | 1992-01-15 |
|  10063 | 1952-08-06 | Gino       | Leonhardt  | F      | 1989-04-08 |
+--------+------------+------------+------------+--------+------------+
10 rows in set (0.00 sec)
```

## 11. Buatlah query untuk menampilkan semua employee dengan title manager yang membawahi departemen dengan nomor d009.
```mysql
SELECT a.emp_no, a.birth_date, a.first_name, a.last_name, a.gender, a.hire_date 
FROM employees as a, dept_manager as b 
WHERE b.dept_no = 'd009' and a.emp_no=b.emp_no;
```
```mysql
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
| 111877 | 1962-10-18 | Xiaobin    | Spinelli  | F      | 1991-08-17 |
| 111939 | 1960-03-25 | Yuchang    | Weedman   | M      | 1989-07-10 |
+--------+------------+------------+-----------+--------+------------+
2 rows in set (0.00 sec)
```

## 12. Buatlah query untuk memasukkan data baru ke tabel departemen dengan nama departemen Engineer dan nomor departemen d010
```mysql
INSERT INTO departments VALUES ('d010','Engineer');
```
```mysql
Query OK, 1 row affected (0.01 sec)s
```









