# เอกสารประกอบการสอน

**รายวิชา: การพัฒนาแอปพลิเคชันด้วย Dart และ Flutter**  
**หัวข้อ: การเชื่อมต่อกับฐานข้อมูล MySQL**

## 1. การเชื่อมต่อกับฐานข้อมูล MySQL

### 1.1 ภาพรวมสถาปัตยกรรม

* Flutter ไม่เชื่อมต่อ MySQL โดยตรง (เหตุผล: Security, Performance, Connection Pooling)
* ใช้ **API (REST API)** เป็นตัวกลาง เช่น PHP + MySQL
* โครงสร้าง: Flutter App ↔ API (PHP) ↔ MySQL Database

### 1.2 การสร้างฐานข้อมูล MySQL

ตัวอย่าง (phpMyAdmin หรือ CLI)

```sql
CREATE DATABASE flutterdb;
USE flutterdb;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  age INT
);
```

### 1.3 การสร้าง API (PHP Example)

ไฟล์ `config.php`

```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "flutterdb";

$conn = new mysqli($host, $user, $pass, $db);
if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}
?>
```

ไฟล์ `get_users.php`

```php
<?php
include 'config.php';

$sql = "SELECT * FROM users";
$result = $conn->query($sql);

$rows = array();
while($row = $result->fetch_assoc()) {
  $rows[] = $row;
}
echo json_encode($rows);
?>
```

## 2. วิธีการดึงข้อมูลมาแสดง

### 2.1 ติดตั้ง Package http

```yaml
dependencies:
  http: ^1.0.0
```

### 2.2 Flutter ดึงข้อมูลผ่าน API

```dart
import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  Future<List> fetchUsers() async {
    final response = await http.get(Uri.parse("http://localhost/flutter/get_users.php"));
    return json.decode(response.body);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("แสดงข้อมูลจาก MySQL")),
        body: FutureBuilder<List>(
          future: fetchUsers(),
          builder: (context, snapshot) {
            if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
            return ListView.builder(
              itemCount: snapshot.data!.length,
              itemBuilder: (context, index) {
                var user = snapshot.data![index];
                return ListTile(
                  title: Text(user['name']),
                  subtitle: Text(user['email']),
                );
              },
            );
          },
        ),
      ),
    );
  }
}
```

## 3. วิธีการค้นหาข้อมูล

### 3.1 API ค้นหาข้อมูล (PHP)

ไฟล์ `search_user.php`

```php
<?php
include 'config.php';
$keyword = $_GET['keyword'];

$sql = "SELECT * FROM users WHERE name LIKE '%$keyword%'";
$result = $conn->query($sql);

$rows = array();
while($row = $result->fetch_assoc()) {
  $rows[] = $row;
}
echo json_encode($rows);
?>
```

### 3.2 Flutter เรียกใช้ API Search

```dart
Future<List> searchUsers(String keyword) async {
  final response = await http.get(Uri.parse("http://localhost/flutter/search_user.php?keyword=$keyword"));
  return json.decode(response.body);
}
```

ใช้ร่วมกับ `TextField` และ `FutureBuilder`

```dart
TextEditingController _controller = TextEditingController();
List _results = [];

void _doSearch() async {
  var data = await searchUsers(_controller.text);
  setState(() {
    _results = data;
  });
}
```

## 4. แบบฝึกหัด

1. สร้างฐานข้อมูล MySQL ชื่อ `flutterdb` และตาราง `products` (name, price, category)
2. เขียน API PHP ดึงข้อมูลสินค้าทั้งหมด
3. แสดงข้อมูลใน Flutter ListView
4. เพิ่ม TextField สำหรับค้นหาสินค้าตามชื่อ

## 5. สรุป

* Flutter เชื่อมต่อ MySQL ผ่าน API (ไม่ต่อโดยตรง)
* ใช้ `http` package ดึงข้อมูลจาก API
* การค้นหาข้อมูลทำได้โดยส่ง parameter ไปยัง API แล้วนำผลลัพธ์มาแสดง