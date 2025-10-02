# เอกสารประกอบการสอน: การเชื่อมต่อ Database ใน Flutter

**วิชา:** การพัฒนา Mobile Application ด้วย Dart และ Flutter  
**ระยะเวลา:** 4 ชั่วโมง  
**ระดับ:** ปริญญาตรี

---

## สารบัญ

1. [บทนำและการเตรียมพร้อม](#1-บทนำและการเตรียมพร้อม)
2. [การเชื่อมต่อกับ Database](#2-การเชื่อมต่อกับ-database)
3. [การดึงข้อมูลมาแสดง](#3-การดึงข้อมูลมาแสดง)
4. [การค้นหาข้อมูล](#4-การค้นหาข้อมูล)
5. [การเพิ่ม แก้ไข และลบข้อมูล](#5-การเพิ่ม-แก้ไข-และลบข้อมูล)
6. [Workshop และแบบฝึกหัด](#6-workshop-และแบบฝึกหัด)
7. [Best Practices](#7-best-practices)

---
<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/25b8291d-96f9-4255-8ecd-5c1bdbbd9715" />


## 1. บทนำและการเตรียมพร้อม

### วัตถุประสงค์การเรียนรู้
หลังจากเรียนจบบทเรียนนี้ นักศึกษาจะสามารถ:
- เชื่อมต่อ Flutter App กับ MySQL และ Firebase
- ดึงข้อมูลจาก Database มาแสดงใน App
- สร้างระบบค้นหาข้อมูล
- ดำเนินการ CRUD (Create, Read, Update, Delete) ข้อมูล

### ความรู้พื้นฐานที่ต้องมี
- Dart Programming พื้นฐาน
- Flutter Widgets และ State Management
- RESTful API (สำหรับ MySQL)
- JSON และ async/await

### การติดตั้ง Dependencies

เพิ่ม dependencies ใน `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # สำหรับ Firebase
  firebase_core: ^2.24.0
  cloud_firestore: ^4.13.0
  
  # สำหรับ MySQL (ผ่าน REST API)
  http: ^1.1.0
  
  # อื่นๆ
  provider: ^6.1.0  # State Management
```

จากนั้นรันคำสั่ง:
```bash
flutter pub get
```

---

## 2. การเชื่อมต่อกับ Database

### 2.1 การเชื่อมต่อกับ Firebase

#### ขั้นตอนที่ 1: Setup Firebase Project
1. ไปที่ [Firebase Console](https://console.firebase.google.com)
2. สร้าง Project ใหม่
3. เพิ่ม App (Android/iOS)
4. ดาวน์โหลดไฟล์ config

#### ขั้นตอนที่ 2: เชื่อมต่อ Firebase ใน Flutter

**ไฟล์: `main.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  
  // เริ่มต้น Firebase
  await Firebase.initializeApp();
  
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Database Demo',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: HomePage(),
    );
  }
}
```

#### ขั้นตอนที่ 3: สร้าง Firestore Database Service

**ไฟล์: `services/firestore_service.dart`**
```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class FirestoreService {
  // สร้าง instance ของ FirebaseFirestore
  final FirebaseFirestore _firestore = FirebaseFirestore.instance;
  
  // Reference ไปยัง collection
  CollectionReference get usersCollection => _firestore.collection('users');
  
  // เมธอดสำหรับทำงานกับข้อมูล (จะเพิ่มเติมในภายหลัง)
}
```

### 2.2 การเชื่อมต่อกับ MySQL (ผ่าน REST API)

เนื่องจาก Flutter ไม่สามารถเชื่อมต่อ MySQL โดยตรง เราจะใช้ REST API เป็นตัวกลาง

#### สร้าง PHP REST API สำหรับ MySQL

**ไฟล์: `api/config.php`**
```php
<?php
header("Access-Control-Allow-Origin: *");
header("Content-Type: application/json");

// การตั้งค่าการเชื่อมต่อ MySQL
define('DB_HOST', 'localhost');
define('DB_USER', 'root');
define('DB_PASS', '');
define('DB_NAME', 'flutter_db');

// เชื่อมต่อฐานข้อมูล
function getDBConnection() {
    $conn = new mysqli(DB_HOST, DB_USER, DB_PASS, DB_NAME);
    
    if ($conn->connect_error) {
        die(json_encode(['error' => 'Connection failed: ' . $conn->connect_error]));
    }
    
    return $conn;
}
?>
```

**ไฟล์: `api/get_users.php`**
```php
<?php
require_once 'config.php';

$conn = getDBConnection();
$sql = "SELECT * FROM users";
$result = $conn->query($sql);

$users = array();
if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        $users[] = $row;
    }
}

echo json_encode($users);
$conn->close();
?>
```

#### สร้าง API Service ใน Flutter

**ไฟล์: `services/api_service.dart`**
```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

class ApiService {
  // URL ของ API (แก้ไขตามที่ตั้งของคุณ)
  static const String baseUrl = 'http://localhost/flutter_api';
  
  // เมธอดสำหรับเรียก API (จะเพิ่มเติมในภายหลัง)
  
  // Helper method สำหรับ GET request
  Future<dynamic> getData(String endpoint) async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/$endpoint'),
      );
      
      if (response.statusCode == 200) {
        return json.decode(response.body);
      } else {
        throw Exception('Failed to load data');
      }
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
  
  // Helper method สำหรับ POST request
  Future<dynamic> postData(String endpoint, Map<String, dynamic> data) async {
    try {
      final response = await http.post(
        Uri.parse('$baseUrl/$endpoint'),
        headers: {'Content-Type': 'application/json'},
        body: json.encode(data),
      );
      
      if (response.statusCode == 200) {
        return json.decode(response.body);
      } else {
        throw Exception('Failed to post data');
      }
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
}
```

---

## 3. การดึงข้อมูลมาแสดง

### 3.1 การดึงข้อมูลจาก Firebase

#### สร้าง Model Class

**ไฟล์: `models/user_model.dart`**
```dart
class UserModel {
  final String id;
  final String name;
  final String email;
  final int age;
  
  UserModel({
    required this.id,
    required this.name,
    required this.email,
    required this.age,
  });
  
  // สร้าง UserModel จาก Firestore Document
  factory UserModel.fromFirestore(DocumentSnapshot doc) {
    Map<String, dynamic> data = doc.data() as Map<String, dynamic>;
    
    return UserModel(
      id: doc.id,
      name: data['name'] ?? '',
      email: data['email'] ?? '',
      age: data['age'] ?? 0,
    );
  }
  
  // แปลง UserModel เป็น Map
  Map<String, dynamic> toMap() {
    return {
      'name': name,
      'email': email,
      'age': age,
    };
  }
  
  // สร้าง UserModel จาก JSON (สำหรับ MySQL API)
  factory UserModel.fromJson(Map<String, dynamic> json) {
    return UserModel(
      id: json['id'].toString(),
      name: json['name'],
      email: json['email'],
      age: int.parse(json['age'].toString()),
    );
  }
}
```

#### อัพเดท Firestore Service

**ไฟล์: `services/firestore_service.dart`**
```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/user_model.dart';

class FirestoreService {
  final FirebaseFirestore _firestore = FirebaseFirestore.instance;
  
  CollectionReference get usersCollection => _firestore.collection('users');
  
  // ดึงข้อมูลทั้งหมด (แบบ Stream - Real-time)
  Stream<List<UserModel>> getAllUsers() {
    return usersCollection.snapshots().map((snapshot) {
      return snapshot.docs.map((doc) {
        return UserModel.fromFirestore(doc);
      }).toList();
    });
  }
  
  // ดึงข้อมูลทั้งหมด (แบบ Future - ครั้งเดียว)
  Future<List<UserModel>> getUsersOnce() async {
    QuerySnapshot snapshot = await usersCollection.get();
    return snapshot.docs.map((doc) {
      return UserModel.fromFirestore(doc);
    }).toList();
  }
  
  // ดึงข้อมูลเฉพาะ ID
  Future<UserModel?> getUserById(String id) async {
    DocumentSnapshot doc = await usersCollection.doc(id).get();
    
    if (doc.exists) {
      return UserModel.fromFirestore(doc);
    }
    return null;
  }
}
```

#### สร้าง UI สำหรับแสดงข้อมูล

**ไฟล์: `screens/firebase_users_screen.dart`**
```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';
import '../models/user_model.dart';

class FirebaseUsersScreen extends StatelessWidget {
  final FirestoreService _firestoreService = FirestoreService();
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Firebase Users'),
      ),
      body: StreamBuilder<List<UserModel>>(
        stream: _firestoreService.getAllUsers(),
        builder: (context, snapshot) {
          // กรณีกำลังโหลดข้อมูล
          if (snapshot.connectionState == ConnectionState.waiting) {
            return Center(child: CircularProgressIndicator());
          }
          
          // กรณีเกิด error
          if (snapshot.hasError) {
            return Center(child: Text('Error: ${snapshot.error}'));
          }
          
          // กรณีไม่มีข้อมูล
          if (!snapshot.hasData || snapshot.data!.isEmpty) {
            return Center(child: Text('ไม่มีข้อมูล'));
          }
          
          // แสดงข้อมูล
          List<UserModel> users = snapshot.data!;
          
          return ListView.builder(
            itemCount: users.length,
            itemBuilder: (context, index) {
              UserModel user = users[index];
              
              return Card(
                margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
                child: ListTile(
                  leading: CircleAvatar(
                    child: Text(user.name[0].toUpperCase()),
                  ),
                  title: Text(user.name),
                  subtitle: Text(user.email),
                  trailing: Text('${user.age} ปี'),
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

### 3.2 การดึงข้อมูลจาก MySQL (ผ่าน API)

#### อัพเดท API Service

**ไฟล์: `services/api_service.dart`**
```dart
import 'dart:convert';
import 'package:http/http.dart' as http;
import '../models/user_model.dart';

class ApiService {
  static const String baseUrl = 'http://localhost/flutter_api';
  
  // ดึงข้อมูล Users ทั้งหมด
  Future<List<UserModel>> getUsers() async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/get_users.php'),
      );
      
      if (response.statusCode == 200) {
        List<dynamic> jsonData = json.decode(response.body);
        return jsonData.map((json) => UserModel.fromJson(json)).toList();
      } else {
        throw Exception('Failed to load users');
      }
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
  
  // ดึงข้อมูล User เฉพาะ ID
  Future<UserModel?> getUserById(String id) async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/get_user.php?id=$id'),
      );
      
      if (response.statusCode == 200) {
        Map<String, dynamic> jsonData = json.decode(response.body);
        return UserModel.fromJson(jsonData);
      }
      return null;
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
}
```

#### สร้าง UI สำหรับแสดงข้อมูล

**ไฟล์: `screens/mysql_users_screen.dart`**
```dart
import 'package:flutter/material.dart';
import '../services/api_service.dart';
import '../models/user_model.dart';

class MySQLUsersScreen extends StatefulWidget {
  @override
  _MySQLUsersScreenState createState() => _MySQLUsersScreenState();
}

class _MySQLUsersScreenState extends State<MySQLUsersScreen> {
  final ApiService _apiService = ApiService();
  List<UserModel> _users = [];
  bool _isLoading = true;
  String? _error;
  
  @override
  void initState() {
    super.initState();
    _loadUsers();
  }
  
  Future<void> _loadUsers() async {
    setState(() {
      _isLoading = true;
      _error = null;
    });
    
    try {
      List<UserModel> users = await _apiService.getUsers();
      setState(() {
        _users = users;
        _isLoading = false;
      });
    } catch (e) {
      setState(() {
        _error = e.toString();
        _isLoading = false;
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('MySQL Users'),
        actions: [
          IconButton(
            icon: Icon(Icons.refresh),
            onPressed: _loadUsers,
          ),
        ],
      ),
      body: _buildBody(),
    );
  }
  
  Widget _buildBody() {
    if (_isLoading) {
      return Center(child: CircularProgressIndicator());
    }
    
    if (_error != null) {
      return Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.error, size: 64, color: Colors.red),
            SizedBox(height: 16),
            Text('Error: $_error'),
            SizedBox(height: 16),
            ElevatedButton(
              onPressed: _loadUsers,
              child: Text('ลองใหม่'),
            ),
          ],
        ),
      );
    }
    
    if (_users.isEmpty) {
      return Center(child: Text('ไม่มีข้อมูล'));
    }
    
    return RefreshIndicator(
      onRefresh: _loadUsers,
      child: ListView.builder(
        itemCount: _users.length,
        itemBuilder: (context, index) {
          UserModel user = _users[index];
          
          return Card(
            margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
            child: ListTile(
              leading: CircleAvatar(
                child: Text(user.name[0].toUpperCase()),
              ),
              title: Text(user.name),
              subtitle: Text(user.email),
              trailing: Text('${user.age} ปี'),
            ),
          );
        },
      ),
    );
  }
}
```

---

## 4. การค้นหาข้อมูล

### 4.1 การค้นหาข้อมูลใน Firebase

#### อัพเดท Firestore Service

**ไฟล์: `services/firestore_service.dart`**
```dart
class FirestoreService {
  // ... โค้ดเดิม ...
  
  // ค้นหาตามชื่อ (ต้องมี index ใน Firestore)
  Future<List<UserModel>> searchUsersByName(String name) async {
    QuerySnapshot snapshot = await usersCollection
        .where('name', isGreaterThanOrEqualTo: name)
        .where('name', isLessThanOrEqualTo: name + '\uf8ff')
        .get();
    
    return snapshot.docs.map((doc) {
      return UserModel.fromFirestore(doc);
    }).toList();
  }
  
  // ค้นหาตามอายุ (ช่วง)
  Future<List<UserModel>> searchUsersByAgeRange(int minAge, int maxAge) async {
    QuerySnapshot snapshot = await usersCollection
        .where('age', isGreaterThanOrEqualTo: minAge)
        .where('age', isLessThanOrEqualTo: maxAge)
        .get();
    
    return snapshot.docs.map((doc) {
      return UserModel.fromFirestore(doc);
    }).toList();
  }
  
  // ค้นหาแบบ Complex (client-side filtering)
  Stream<List<UserModel>> searchUsers(String query) {
    return getAllUsers().map((users) {
      if (query.isEmpty) return users;
      
      String lowerQuery = query.toLowerCase();
      return users.where((user) {
        return user.name.toLowerCase().contains(lowerQuery) ||
               user.email.toLowerCase().contains(lowerQuery);
      }).toList();
    });
  }
}
```

#### สร้าง UI สำหรับค้นหา

**ไฟล์: `screens/firebase_search_screen.dart`**
```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';
import '../models/user_model.dart';

class FirebaseSearchScreen extends StatefulWidget {
  @override
  _FirebaseSearchScreenState createState() => _FirebaseSearchScreenState();
}

class _FirebaseSearchScreenState extends State<FirebaseSearchScreen> {
  final FirestoreService _firestoreService = FirestoreService();
  final TextEditingController _searchController = TextEditingController();
  String _searchQuery = '';
  
  @override
  void dispose() {
    _searchController.dispose();
    super.dispose();
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('ค้นหา Users (Firebase)'),
      ),
      body: Column(
        children: [
          Padding(
            padding: EdgeInsets.all(16),
            child: TextField(
              controller: _searchController,
              decoration: InputDecoration(
                labelText: 'ค้นหา',
                hintText: 'ชื่อหรืออีเมล',
                prefixIcon: Icon(Icons.search),
                suffixIcon: _searchQuery.isNotEmpty
                    ? IconButton(
                        icon: Icon(Icons.clear),
                        onPressed: () {
                          _searchController.clear();
                          setState(() {
                            _searchQuery = '';
                          });
                        },
                      )
                    : null,
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(10),
                ),
              ),
              onChanged: (value) {
                setState(() {
                  _searchQuery = value;
                });
              },
            ),
          ),
          Expanded(
            child: StreamBuilder<List<UserModel>>(
              stream: _firestoreService.searchUsers(_searchQuery),
              builder: (context, snapshot) {
                if (snapshot.connectionState == ConnectionState.waiting) {
                  return Center(child: CircularProgressIndicator());
                }
                
                if (snapshot.hasError) {
                  return Center(child: Text('Error: ${snapshot.error}'));
                }
                
                if (!snapshot.hasData || snapshot.data!.isEmpty) {
                  return Center(
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Icon(Icons.search_off, size: 64, color: Colors.grey),
                        SizedBox(height: 16),
                        Text('ไม่พบข้อมูล'),
                      ],
                    ),
                  );
                }
                
                List<UserModel> users = snapshot.data!;
                
                return ListView.builder(
                  itemCount: users.length,
                  itemBuilder: (context, index) {
                    UserModel user = users[index];
                    
                    return Card(
                      margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
                      child: ListTile(
                        leading: CircleAvatar(
                          child: Text(user.name[0].toUpperCase()),
                        ),
                        title: Text(user.name),
                        subtitle: Text(user.email),
                        trailing: Chip(
                          label: Text('${user.age} ปี'),
                        ),
                      ),
                    );
                  },
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

### 4.2 การค้นหาข้อมูลใน MySQL

#### สร้าง PHP API สำหรับค้นหา

**ไฟล์: `api/search_users.php`**
```php
<?php
require_once 'config.php';

$conn = getDBConnection();

// รับค่าจาก query string
$searchTerm = isset($_GET['q']) ? $_GET['q'] : '';

// ป้องกัน SQL Injection
$searchTerm = $conn->real_escape_string($searchTerm);

// สร้าง SQL Query
$sql = "SELECT * FROM users WHERE 
        name LIKE '%$searchTerm%' OR 
        email LIKE '%$searchTerm%'";

$result = $conn->query($sql);

$users = array();
if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        $users[] = $row;
    }
}

echo json_encode($users);
$conn->close();
?>
```

#### อัพเดท API Service

**ไฟล์: `services/api_service.dart`**
```dart
class ApiService {
  // ... โค้ดเดิม ...
  
  // ค้นหา Users
  Future<List<UserModel>> searchUsers(String query) async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/search_users.php?q=${Uri.encodeComponent(query)}'),
      );
      
      if (response.statusCode == 200) {
        List<dynamic> jsonData = json.decode(response.body);
        return jsonData.map((json) => UserModel.fromJson(json)).toList();
      } else {
        throw Exception('Failed to search users');
      }
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
}
```

#### สร้าง UI สำหรับค้นหา

**ไฟล์: `screens/mysql_search_screen.dart`**
```dart
import 'package:flutter/material.dart';
import 'dart:async';
import '../services/api_service.dart';
import '../models/user_model.dart';

class MySQLSearchScreen extends StatefulWidget {
  @override
  _MySQLSearchScreenState createState() => _MySQLSearchScreenState();
}

class _MySQLSearchScreenState extends State<MySQLSearchScreen> {
  final ApiService _apiService = ApiService();
  final TextEditingController _searchController = TextEditingController();
  
  List<UserModel> _searchResults = [];
  bool _isSearching = false;
  Timer? _debounce;
  
  @override
  void dispose() {
    _searchController.dispose();
    _debounce?.cancel();
    super.dispose();
  }
  
  void _onSearchChanged(String query) {
    // ใช้ debounce เพื่อไม่ให้เรียก API บ่อยเกินไป
    if (_debounce?.isActive ?? false) _debounce!.cancel();
    
    _debounce = Timer(const Duration(milliseconds: 500), () {
      if (query.isNotEmpty) {
        _performSearch(query);
      } else {
        setState(() {
          _searchResults = [];
        });
      }
    });
  }
  
  Future<void> _performSearch(String query) async {
    setState(() {
      _isSearching = true;
    });
    
    try {
      List<UserModel> results = await _apiService.searchUsers(query);
      setState(() {
        _searchResults = results;
        _isSearching = false;
      });
    } catch (e) {
      setState(() {
        _isSearching = false;
      });
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('เกิดข้อผิดพลาด: $e')),
      );
    }
  }
  
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('ค้นหา Users (MySQL)'),
      ),
      body: Column(
        children: [
          Padding(
            padding: EdgeInsets.all(16),
            child: TextField(
              controller: _searchController,
              decoration: InputDecoration(
                labelText: 'ค้นหา',
                hintText: 'ชื่อหรืออีเมล',
                prefixIcon: Icon(Icons.search),
                suffixIcon: _searchController.text.isNotEmpty
                    ? IconButton(
                        icon: Icon(Icons.clear),
                        onPressed: () {
                          _searchController.clear();
                          setState(() {
                            _searchResults = [];
                          });
                        },
                      )
                    : null,
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(10),
                ),
              ),
              onChanged: _onSearchChanged,
            ),
          ),
          Expanded(
            child: _buildSearchResults(),
          ),
        ],
      ),
    );
  }
  
  Widget _buildSearchResults() {
    if (_isSearching) {
      return Center(child: CircularProgressIndicator());
    }
    
    if (_searchController.text.isEmpty) {
      return Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.search, size: 64, color: Colors.grey),
            SizedBox(height: 16),
            Text('พิมพ์เพื่อค้นหา'),
          ],
        ),
      );
    }
    
    if (_searchResults.isEmpty) {
      return Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(Icons.search_off, size: 64, color: Colors.grey),
            SizedBox(height: 16),
            Text('ไม่พบข้อมูล'),
          ],
        ),
      );
    }
    
    return ListView.builder(
      itemCount: _searchResults.length,
      itemBuilder: (context, index) {
        UserModel user = _searchResults[index];
        
        return Card(
          margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
          child: ListTile(
            leading: CircleAvatar(
              child: Text(user.name[0].toUpperCase()),
            ),
            title: Text(user.name),
            subtitle: Text(user.email),
            trailing: Chip(
              label: Text('${user.age} ปี'),
            ),
          ),
        );
      },
    );
  }
}
```

---

## 5. การเพิ่ม แก้ไข และลบข้อมูล

### 5.1 CRUD Operations ใน Firebase

#### อัพเดท Firestore Service

**ไฟล์: `services/firestore_service.dart`**
```dart
class FirestoreService {
  // ... โค้ดเดิม ...
  
  // เพิ่มข้อมูล (Create)
  Future<String> addUser(UserModel user) async {
    try {
      DocumentReference docRef = await usersCollection.add(user.toMap());
      return docRef.id;
    } catch (e) {
      throw Exception('เพิ่มข้อมูลไม่สำเร็จ: $e');
    }
  }
  
  // แก้ไขข้อมูล (Update)
  Future<void> updateUser(String id, UserModel user) async {
    try {
      await usersCollection.doc(id).update(user.toMap());
    } catch (e) {
      throw Exception('แก้ไขข้อมูลไม่สำเร็จ: $e');
    }
  }
  
  // ลบข้อมูล (Delete)
  Future<void> deleteUser(String id) async {
    try {
      await usersCollection.doc(id).delete();
    } catch (e) {
      throw Exception('ลบข้อมูลไม่สำเร็จ: $e');
    }
  }
  
  // อัพเดทเฉพาะ field
  Future<void> updateUserField(String id, String field, dynamic value) async {
    try {
      await usersCollection.doc(id).update({field: value});
    } catch (e) {
      throw Exception('อัพเดทข้อมูลไม่สำเร็จ: $e');
    }
  }
}
```

#### สร้าง Form สำหรับเพิ่ม/แก้ไขข้อมูล

**ไฟล์: `screens/firebase_user_form_screen.dart`**
```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';
import '../models/user_model.dart';

class FirebaseUserFormScreen extends StatefulWidget {
  final UserModel? user; // null = เพิ่มใหม่, ไม่ null = แก้ไข
  
  FirebaseUserFormScreen({this.user});
  
  @override
  _FirebaseUserFormScreenState createState() => _FirebaseUserFormScreenState();
}

class _FirebaseUserFormScreenState extends State<FirebaseUserFormScreen> {
  final _formKey = GlobalKey<FormState>();
  final FirestoreService _firestoreService = FirestoreService();
  
  late TextEditingController _nameController;
  late TextEditingController _emailController;
  late TextEditingController _ageController;
  
  bool _isLoading = false;
  
  @override
  void initState() {
    super.initState();
    _nameController = TextEditingController(text: widget.user?.name ?? '');
    _emailController = TextEditingController(text: widget.user?.email ?? '');
    _ageController = TextEditingController(
      text: widget.user?.age.toString() ?? '',
    );
  }
  
  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _ageController.dispose();
    super.dispose();
  }
  
  Future<void> _saveUser() async {
    if (!_formKey.currentState!.validate()) return;
    
    setState(() {
      _isLoading = true;
    });
    
    try {
      UserModel user = UserModel(
        id: widget.user?.id ?? '',
        name: _nameController.text.trim(),
        email: _emailController.text.trim(),
        age: int.parse(_ageController.text.trim()),
      );
      
      if (widget.user == null) {
        // เพิ่มใหม่
        await _firestoreService.addUser(user);
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('เพิ่มข้อมูลสำเร็จ')),
        );
      } else {
        // แก้ไข
        await _firestoreService.updateUser(widget.user!.id, user);
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('แก้ไขข้อมูลสำเร็จ')),
        );
      }
      
      Navigator.pop(context, true);
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('เกิดข้อผิดพลาด: $e')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }
  
  Future<void> _deleteUser() async {
    if (widget.user == null) return;
    
    bool? confirm = await showDialog<bool>(
      context: context,
      builder: (context) => AlertDialog(
        title: Text('ยืนยันการลบ'),
        content: Text('คุณต้องการลบข้อมูลนี้ใช่หรือไม่?'),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context, false),
            child: Text('ยกเลิก'),
          ),
          TextButton(
            onPressed: () => Navigator.pop(context, true),
            child: Text('ลบ', style: TextStyle(color: Colors.red)),
          ),
        ],
      ),
    );
    
    if (confirm != true) return;
    
    setState(() {
      _isLoading = true;
    });
    
    try {
      await _firestoreService.deleteUser(widget.user!.id);
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('ลบข้อมูลสำเร็จ')),
      );
      Navigator.pop(context, true);
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('เกิดข้อผิดพลาด: $e')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    bool isEdit = widget.user != null;
    
    return Scaffold(
      appBar: AppBar(
        title: Text(isEdit ? 'แก้ไขข้อมูล' : 'เพิ่มข้อมูล'),
        actions: isEdit
            ? [
                IconButton(
                  icon: Icon(Icons.delete),
                  onPressed: _isLoading ? null : _deleteUser,
                ),
              ]
            : null,
      ),
      body: _isLoading
          ? Center(child: CircularProgressIndicator())
          : SingleChildScrollView(
              padding: EdgeInsets.all(16),
              child: Form(
                key: _formKey,
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.stretch,
                  children: [
                    TextFormField(
                      controller: _nameController,
                      decoration: InputDecoration(
                        labelText: 'ชื่อ',
                        border: OutlineInputBorder(),
                      ),
                      validator: (value) {
                        if (value == null || value.trim().isEmpty) {
                          return 'กรุณากรอกชื่อ';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: 16),
                    TextFormField(
                      controller: _emailController,
                      decoration: InputDecoration(
                        labelText: 'อีเมล',
                        border: OutlineInputBorder(),
                      ),
                      keyboardType: TextInputType.emailAddress,
                      validator: (value) {
                        if (value == null || value.trim().isEmpty) {
                          return 'กรุณากรอกอีเมล';
                        }
                        if (!value.contains('@')) {
                          return 'รูปแบบอีเมลไม่ถูกต้อง';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: 16),
                    TextFormField(
                      controller: _ageController,
                      decoration: InputDecoration(
                        labelText: 'อายุ',
                        border: OutlineInputBorder(),
                      ),
                      keyboardType: TextInputType.number,
                      validator: (value) {
                        if (value == null || value.trim().isEmpty) {
                          return 'กรุณากรอกอายุ';
                        }
                        int? age = int.tryParse(value);
                        if (age == null || age < 0 || age > 150) {
                          return 'กรุณากรอกอายุที่ถูกต้อง';
                        }
                        return null;
                      },
                    ),
                    SizedBox(height: 24),
                    ElevatedButton(
                      onPressed: _saveUser,
                      style: ElevatedButton.styleFrom(
                        padding: EdgeInsets.symmetric(vertical: 16),
                      ),
                      child: Text(
                        isEdit ? 'บันทึกการแก้ไข' : 'เพิ่มข้อมูล',
                        style: TextStyle(fontSize: 16),
                      ),
                    ),
                  ],
                ),
              ),
            ),
    );
  }
}
```

### 5.2 CRUD Operations ใน MySQL

#### สร้าง PHP API สำหรับ CRUD

**ไฟล์: `api/create_user.php`**
```php
<?php
require_once 'config.php';

$conn = getDBConnection();

// รับข้อมูล JSON
$data = json_decode(file_get_contents('php://input'), true);

$name = $conn->real_escape_string($data['name']);
$email = $conn->real_escape_string($data['email']);
$age = intval($data['age']);

$sql = "INSERT INTO users (name, email, age) VALUES ('$name', '$email', $age)";

if ($conn->query($sql) === TRUE) {
    echo json_encode([
        'success' => true,
        'id' => $conn->insert_id,
        'message' => 'เพิ่มข้อมูลสำเร็จ'
    ]);
} else {
    echo json_encode([
        'success' => false,
        'message' => 'เกิดข้อผิดพลาด: ' . $conn->error
    ]);
}

$conn->close();
?>
```

**ไฟล์: `api/update_user.php`**
```php
<?php
require_once 'config.php';

$conn = getDBConnection();

$data = json_decode(file_get_contents('php://input'), true);

$id = intval($data['id']);
$name = $conn->real_escape_string($data['name']);
$email = $conn->real_escape_string($data['email']);
$age = intval($data['age']);

$sql = "UPDATE users SET name='$name', email='$email', age=$age WHERE id=$id";

if ($conn->query($sql) === TRUE) {
    echo json_encode([
        'success' => true,
        'message' => 'แก้ไขข้อมูลสำเร็จ'
    ]);
} else {
    echo json_encode([
        'success' => false,
        'message' => 'เกิดข้อผิดพลาด: ' . $conn->error
    ]);
}

$conn->close();
?>
```

**ไฟล์: `api/delete_user.php`**
```php
<?php
require_once 'config.php';

$conn = getDBConnection();

$id = isset($_GET['id']) ? intval($_GET['id']) : 0;

$sql = "DELETE FROM users WHERE id=$id";

if ($conn->query($sql) === TRUE) {
    echo json_encode([
        'success' => true,
        'message' => 'ลบข้อมูลสำเร็จ'
    ]);
} else {
    echo json_encode([
        'success' => false,
        'message' => 'เกิดข้อผิดพลาด: ' . $conn->error
    ]);
}

$conn->close();
?>
```

#### อัพเดท API Service

**ไฟล์: `services/api_service.dart`**
```dart
class ApiService {
  // ... โค้ดเดิม ...
  
  // เพิ่ม User
  Future<bool> createUser(UserModel user) async {
    try {
      final response = await http.post(
        Uri.parse('$baseUrl/create_user.php'),
        headers: {'Content-Type': 'application/json'},
        body: json.encode(user.toMap()),
      );
      
      if (response.statusCode == 200) {
        Map<String, dynamic> result = json.decode(response.body);
        return result['success'] == true;
      }
      return false;
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
  
  // แก้ไข User
  Future<bool> updateUser(UserModel user) async {
    try {
      final response = await http.post(
        Uri.parse('$baseUrl/update_user.php'),
        headers: {'Content-Type': 'application/json'},
        body: json.encode({
          'id': user.id,
          ...user.toMap(),
        }),
      );
      
      if (response.statusCode == 200) {
        Map<String, dynamic> result = json.decode(response.body);
        return result['success'] == true;
      }
      return false;
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
  
  // ลบ User
  Future<bool> deleteUser(String id) async {
    try {
      final response = await http.get(
        Uri.parse('$baseUrl/delete_user.php?id=$id'),
      );
      
      if (response.statusCode == 200) {
        Map<String, dynamic> result = json.decode(response.body);
        return result['success'] == true;
      }
      return false;
    } catch (e) {
      throw Exception('Error: $e');
    }
  }
}
```

---

## 6. Workshop และแบบฝึกหัด

### Workshop 1: สร้างระบบจัดการ Todo List

**โจทย์:** สร้าง Todo List App ที่เชื่อมต่อกับ Firebase

**ขั้นตอน:**
1. สร้าง Model สำหรับ Todo
2. สร้าง Service สำหรับ CRUD operations
3. สร้าง UI สำหรับแสดงรายการ Todo
4. เพิ่มฟีเจอร์ค้นหาและกรอง
5. เพิ่มฟีเจอร์เพิ่ม/แก้ไข/ลบ Todo

**โครงสร้าง Model:**
```dart
class TodoModel {
  final String id;
  final String title;
  final String description;
  final bool completed;
  final DateTime createdAt;
  
  // ... เมธอดต่างๆ
}
```

### Workshop 2: สร้างระบบจัดการคลังสินค้า

**โจทย์:** สร้าง Inventory Management System ที่เชื่อมต่อกับ MySQL

**ฟีเจอร์:**
- แสดงรายการสินค้า
- เพิ่ม/แก้ไข/ลบสินค้า
- ค้นหาสินค้า
- แสดงสินค้าที่ใกล้หมด
- สรุปยอดรวมคลังสินค้า

### แบบฝึกหัด

**แบบฝึกหัดที่ 1:** เพิ่มฟีเจอร์ Pagination ในการดึงข้อมูล

**แบบฝึกหัดที่ 2:** สร้างระบบ Filter ข้อมูลแบบหลายเงื่อนไข

**แบบฝึกหัดที่ 3:** เพิ่มฟีเจอร์ Sorting (เรียงลำดับข้อมูล)

**แบบฝึกหัดที่ 4:** สร้างระบบ Offline-first ด้วย SQLite

**แบบฝึกหัดที่ 5:** เพิ่มการ Upload รูปภาพไปยัง Firebase Storage

---

## 7. Best Practices

### 7.1 การจัดการ Error

```dart
// ใช้ try-catch เสมอ
try {
  await _firestoreService.addUser(user);
} catch (e) {
  print('Error: $e');
  // แสดงข้อความแจ้งเตือนผู้ใช้
}

// ตรวจสอบ response status
if (response.statusCode == 200) {
  // สำเร็จ
} else {
  // ล้มเหลว
}
```

### 7.2 การจัดการ State

```dart
// ใช้ State Management (Provider, Riverpod, Bloc)
// แทนการใช้ setState() ทั่วทั้ง App

class UserProvider extends ChangeNotifier {
  List<UserModel> _users = [];
  
  List<UserModel> get users => _users;
  
  Future<void> loadUsers() async {
    _users = await apiService.getUsers();
    notifyListeners();
  }
}
```

### 7.3 Security

```dart
// Firebase Security Rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null 
                   && request.auth.uid == userId;
    }
  }
}

// สำหรับ MySQL API - ใช้ Prepared Statements
$stmt = $conn->prepare("SELECT * FROM users WHERE id = ?");
$stmt->bind_param("i", $id);
$stmt->execute();
```

### 7.4 Performance Optimization

**Firebase:**
- ใช้ `limit()` เพื่อจำกัดจำนวนข้อมูล
- ใช้ Index สำหรับ query ที่ซับซ้อน
- ใช้ `pagination` สำหรับข้อมูลจำนวนมาก

```dart
QuerySnapshot snapshot = await usersCollection
    .orderBy('name')
    .limit(20)
    .get();
```

**MySQL API:**
- ใช้ `LIMIT` และ `OFFSET` ใน SQL
- สร้าง Index สำหรับคอลัมน์ที่ใช้ค้นหาบ่อย
- ใช้ Connection Pooling

### 7.5 Code Organization

```
lib/
├── main.dart
├── models/
│   ├── user_model.dart
│   └── todo_model.dart
├── services/
│   ├── firestore_service.dart
│   └── api_service.dart
├── screens/
│   ├── home_screen.dart
│   ├── user_list_screen.dart
│   └── user_form_screen.dart
├── widgets/
│   ├── user_card.dart
│   └── loading_indicator.dart
└── utils/
    ├── constants.dart
    └── validators.dart
```

### 7.6 การทดสอบ

```dart
// Unit Test
test('UserModel should be created from JSON', () {
  final json = {'id': '1', 'name': 'John', 'email': 'john@example.com', 'age': 25};
  final user = UserModel.fromJson(json);
  
  expect(user.name, 'John');
  expect(user.email, 'john@example.com');
});

// Widget Test
testWidgets('UserCard displays user information', (WidgetTester tester) async {
  final user = UserModel(id: '1', name: 'John', email: 'john@example.com', age: 25);
  
  await tester.pumpWidget(MaterialApp(
    home: UserCard(user: user),
  ));
  
  expect(find.text('John'), findsOneWidget);
  expect(find.text('john@example.com'), findsOneWidget);
});
```

---

## สรุป

การเชื่อมต่อ Database ใน Flutter สามารถทำได้หลายวิธี:

1. **Firebase Firestore** - เหมาะสำหรับ Realtime Database และ Cloud Storage
2. **MySQL ผ่าน REST API** - เหมาะสำหรับระบบที่มี Backend อยู่แล้ว

**สิ่งที่ควรจำ:**
- ใช้ async/await สำหรับการทำงานแบบ asynchronous
- จัดการ Error ให้ดี
- คำนึงถึง Security เสมอ
- เขียนโค้ดให้สะอาดและอ่านง่าย
- ทดสอบโค้ดอย่างสม่ำเสมอ

---

## แหล่งข้อมูลเพิ่มเติม

**เอกสารอย่างเป็นทางการ:**
- [Flutter Documentation](https://flutter.dev/docs)
- [Firebase for Flutter](https://firebase.flutter.dev/)
- [Dart Documentation](https://dart.dev/guides)

**Tutorial และ Course:**
- [Flutter & Firebase Tutorial](https://www.youtube.com/c/FlutterExplained)
- [REST API with Flutter](https://www.udemy.com/topic/flutter/)

**Community:**
- [Flutter Community](https://flutter.dev/community)
- [Stack Overflow - Flutter Tag](https://stackoverflow.com/questions/tagged/flutter)

---

**จัดทำโดย:** [ชื่อผู้สอน]  
**วันที่:** [วันที่จัดทำ]  
**เวอร์ชัน:** 1.0


