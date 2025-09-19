# ชั่วโมงที่ 4: Interactive Widgets
**เวลา: 1 ชั่วโมง**

---

## ส่วนที่ 1: Input Widgets (30 นาที)

### 1.1 TextField และ TextFormField (12 นาที)

#### TextField พื้นฐาน
```dart
TextField(
  decoration: InputDecoration(
    labelText: 'ชื่อผู้ใช้',
    hintText: 'กรุณาใส่ชื่อผู้ใช้',
    border: OutlineInputBorder(),
  ),
  onChanged: (value) {
    print('ค่าที่พิมพ์: $value');
  },
)
```

#### TextFormField สำหรับการ validate
```dart
TextFormField(
  decoration: InputDecoration(
    labelText: 'อีเมล',
    prefixIcon: Icon(Icons.email),
    border: OutlineInputBorder(
      borderRadius: BorderRadius.circular(10),
    ),
  ),
  keyboardType: TextInputType.emailAddress,
  validator: (value) {
    if (value == null || value.isEmpty) {
      return 'กรุณาใส่อีเมล';
    }
    if (!value.contains('@')) {
      return 'รูปแบบอีเมลไม่ถูกต้อง';
    }
    return null;
  },
)
```

#### ตัวอย่างการใช้ Controller
```dart
class MyWidget extends StatefulWidget {
  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  final TextEditingController _nameController = TextEditingController();

  @override
  void dispose() {
    _nameController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return TextField(
      controller: _nameController,
      decoration: InputDecoration(labelText: 'ชื่อ'),
    );
  }
}
```

### 1.2 Button Varieties (10 นาที)

#### ElevatedButton
```dart
ElevatedButton(
  onPressed: () {
    print('กดปุ่ม Elevated!');
  },
  style: ElevatedButton.styleFrom(
    backgroundColor: Colors.blue,
    foregroundColor: Colors.white,
    padding: EdgeInsets.symmetric(horizontal: 20, vertical: 10),
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(8),
    ),
  ),
  child: Text('บันทึก'),
)
```

#### TextButton
```dart
TextButton(
  onPressed: () {
    print('กดปุ่ม Text!');
  },
  child: Text('ยกเลิก'),
)
```

#### IconButton
```dart
IconButton(
  onPressed: () {
    print('กดปุ่ม Icon!');
  },
  icon: Icon(Icons.favorite),
  color: Colors.red,
  iconSize: 30,
)
```

#### OutlinedButton
```dart
OutlinedButton(
  onPressed: () {
    print('กดปุ่ม Outlined!');
  },
  style: OutlinedButton.styleFrom(
    side: BorderSide(color: Colors.blue, width: 2),
  ),
  child: Text('แก้ไข'),
)
```

### 1.3 Switch, Checkbox, RadioButton (8 นาที)

#### Switch
```dart
class SwitchExample extends StatefulWidget {
  @override
  _SwitchExampleState createState() => _SwitchExampleState();
}

class _SwitchExampleState extends State<SwitchExample> {
  bool isNotificationOn = false;

  @override
  Widget build(BuildContext context) {
    return ListTile(
      title: Text('รับการแจ้งเตือน'),
      trailing: Switch(
        value: isNotificationOn,
        onChanged: (bool value) {
          setState(() {
            isNotificationOn = value;
          });
        },
      ),
    );
  }
}
```

#### Checkbox
```dart
class CheckboxExample extends StatefulWidget {
  @override
  _CheckboxExampleState createState() => _CheckboxExampleState();
}

class _CheckboxExampleState extends State<CheckboxExample> {
  bool isAccepted = false;

  @override
  Widget build(BuildContext context) {
    return CheckboxListTile(
      title: Text('ยอมรับข้อตกลงและเงื่อนไข'),
      value: isAccepted,
      onChanged: (bool? value) {
        setState(() {
          isAccepted = value ?? false;
        });
      },
      controlAffinity: ListTileControlAffinity.leading,
    );
  }
}
```

#### RadioButton
```dart
class RadioExample extends StatefulWidget {
  @override
  _RadioExampleState createState() => _RadioExampleState();
}

class _RadioExampleState extends State<RadioExample> {
  String gender = 'male';

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        RadioListTile<String>(
          title: Text('ชาย'),
          value: 'male',
          groupValue: gender,
          onChanged: (String? value) {
            setState(() {
              gender = value!;
            });
          },
        ),
        RadioListTile<String>(
          title: Text('หญิง'),
          value: 'female',
          groupValue: gender,
          onChanged: (String? value) {
            setState(() {
              gender = value!;
            });
          },
        ),
      ],
    );
  }
}
```

---

## ส่วนที่ 2: Event Handling (20 นาที)

### 2.1 Callbacks และ Event Handling (12 นาที)

#### onPressed Callback
```dart
class ButtonExample extends StatelessWidget {
  void _handleButtonPress() {
    print('ปุ่มถูกกด!');
    // ทำงานเมื่อกดปุ่ม
  }

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: _handleButtonPress,
      child: Text('กดฉัน'),
    );
  }
}
```

#### onChanged Callback
```dart
class TextFieldExample extends StatefulWidget {
  @override
  _TextFieldExampleState createState() => _TextFieldExampleState();
}

class _TextFieldExampleState extends State<TextFieldExample> {
  String inputText = '';

  void _handleTextChange(String value) {
    setState(() {
      inputText = value;
    });
    print('ข้อความที่พิมพ์: $value');
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        TextField(
          onChanged: _handleTextChange,
          decoration: InputDecoration(
            labelText: 'พิมพ์ข้อความ',
          ),
        ),
        SizedBox(height: 20),
        Text('ข้อความที่พิมพ์: $inputText'),
      ],
    );
  }
}
```

#### Passing Data ระหว่าง Widgets
```dart
// Parent Widget
class ParentWidget extends StatefulWidget {
  @override
  _ParentWidgetState createState() => _ParentWidgetState();
}

class _ParentWidgetState extends State<ParentWidget> {
  String messageFromChild = '';

  void _updateMessage(String message) {
    setState(() {
      messageFromChild = message;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('ข้อความจาก Child: $messageFromChild'),
        ChildWidget(onMessageChanged: _updateMessage),
      ],
    );
  }
}

// Child Widget
class ChildWidget extends StatelessWidget {
  final Function(String) onMessageChanged;

  ChildWidget({required this.onMessageChanged});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        onMessageChanged('สวัสดีจาก Child Widget!');
      },
      child: Text('ส่งข้อความ'),
    );
  }
}
```

### 2.2 Form Validation พื้นฐาน (8 นาที)

#### การใช้ GlobalKey และ Form
```dart
class FormValidationExample extends StatefulWidget {
  @override
  _FormValidationExampleState createState() => _FormValidationExampleState();
}

class _FormValidationExampleState extends State<FormValidationExample> {
  final _formKey = GlobalKey<FormState>();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();

  @override
  void dispose() {
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  String? _validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return 'กรุณาใส่อีเมล';
    }
    
    String pattern = r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$';
    RegExp regex = RegExp(pattern);
    
    if (!regex.hasMatch(value)) {
      return 'รูปแบบอีเมลไม่ถูกต้อง';
    }
    return null;
  }

  String? _validatePassword(String? value) {
    if (value == null || value.isEmpty) {
      return 'กรุณาใส่รหัสผ่าน';
    }
    if (value.length < 6) {
      return 'รหัสผ่านต้องมีอย่างน้อย 6 ตัวอักษร';
    }
    return null;
  }

  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      // ข้อมูลถูกต้อง สามารถดำเนินการต่อได้
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('ข้อมูลถูกต้อง!')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Form(
      key: _formKey,
      child: Column(
        children: [
          TextFormField(
            controller: _emailController,
            validator: _validateEmail,
            decoration: InputDecoration(
              labelText: 'อีเมล',
              prefixIcon: Icon(Icons.email),
            ),
          ),
          SizedBox(height: 16),
          TextFormField(
            controller: _passwordController,
            validator: _validatePassword,
            obscureText: true,
            decoration: InputDecoration(
              labelText: 'รหัสผ่าน',
              prefixIcon: Icon(Icons.lock),
            ),
          ),
          SizedBox(height: 20),
          ElevatedButton(
            onPressed: _submitForm,
            child: Text('เข้าสู่ระบบ'),
          ),
        ],
      ),
    );
  }
}
```

---

## ส่วนที่ 3: Styling และ Themes (10 นาที)

### 3.1 Colors และ MaterialColor (5 นาที)

#### การใช้สีพื้นฐาน
```dart
Container(
  color: Colors.blue,           // สีพื้นฐาน
  child: Text('สีน้ำเงิน'),
)

Container(
  color: Colors.blue[300],      // สีน้ำเงินอ่อน
  child: Text('สีน้ำเงินอ่อน'),
)

Container(
  color: Color(0xFF2196F3),     // สีจาก Hex code
  child: Text('สีจาก Hex'),
)

Container(
  color: Color.fromRGBO(33, 150, 243, 1.0), // สีจาก RGBA
  child: Text('สีจาก RGBA'),
)
```

#### การสร้าง Custom Colors
```dart
class MyColors {
  static const Color primaryBlue = Color(0xFF2196F3);
  static const Color secondaryBlue = Color(0xFF64B5F6);
  static const Color lightGray = Color(0xFFF5F5F5);
  
  static const MaterialColor customBlue = MaterialColor(
    0xFF2196F3,
    <int, Color>{
      50: Color(0xFFE3F2FD),
      100: Color(0xFFBBDEFB),
      200: Color(0xFF90CAF9),
      300: Color(0xFF64B5F6),
      400: Color(0xFF42A5F5),
      500: Color(0xFF2196F3),
      600: Color(0xFF1E88E5),
      700: Color(0xFF1976D2),
      800: Color(0xFF1565C0),
      900: Color(0xFF0D47A1),
    },
  );
}
```

### 3.2 ThemeData พื้นฐาน (5 นาที)

#### การกำหนด Theme ใน MaterialApp
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        // สีหลักของแอพ
        primarySwatch: Colors.blue,
        primaryColor: Colors.blue,
        
        // สีพื้นหลัง
        scaffoldBackgroundColor: Colors.white,
        
        // การกำหนดรูปแบบ AppBar
        appBarTheme: AppBarTheme(
          backgroundColor: Colors.blue,
          titleTextStyle: TextStyle(
            color: Colors.white,
            fontSize: 20,
            fontWeight: FontWeight.bold,
          ),
          iconTheme: IconThemeData(color: Colors.white),
        ),
        
        // การกำหนดรูปแบบ Button
        elevatedButtonTheme: ElevatedButtonThemeData(
          style: ElevatedButton.styleFrom(
            backgroundColor: Colors.blue,
            foregroundColor: Colors.white,
            padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        ),
        
        // การกำหนดรูปแบบ TextField
        inputDecorationTheme: InputDecorationTheme(
          border: OutlineInputBorder(
            borderRadius: BorderRadius.circular(8),
          ),
          focusedBorder: OutlineInputBorder(
            borderRadius: BorderRadius.circular(8),
            borderSide: BorderSide(color: Colors.blue, width: 2),
          ),
        ),
        
        // การกำหนดรูปแบบ Text
        textTheme: TextTheme(
          headlineLarge: TextStyle(
            fontSize: 24,
            fontWeight: FontWeight.bold,
            color: Colors.black87,
          ),
          bodyLarge: TextStyle(
            fontSize: 16,
            color: Colors.black87,
          ),
        ),
      ),
      home: MyHomePage(),
    );
  }
}
```

#### การใช้ Theme ใน Widget
```dart
class ThemedWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        // ใช้สีจาก Theme
        Container(
          color: Theme.of(context).primaryColor,
          child: Text(
            'ข้อความที่ใช้ Theme',
            style: Theme.of(context).textTheme.headlineLarge,
          ),
        ),
        
        // ปุ่มที่ใช้ Theme
        ElevatedButton(
          onPressed: () {},
          child: Text('ปุ่มที่ใช้ Theme'),
        ),
      ],
    );
  }
}
```

---

## กิจกรรมปฏิบัติ: สร้างฟอร์มลงทะเบียน (15 นาที)

### โจทย์:
สร้างฟอร์มลงทะเบียนที่ประกอบด้วย:
1. ช่องใส่ชื่อ (required)
2. ช่องใส่อีเมล (required + validation)
3. ช่องใส่รหัสผ่าน (required + อย่างน้อย 6 ตัวอักษร)
4. ช่องยืนยันรหัสผ่าน (ต้องตรงกับรหัสผ่าน)
5. เลือกเพศ (Radio buttons)
6. ยอมรับเงื่อนไข (Checkbox)
7. ปุ่มลงทะเบียน

### โค้ดตัวอย่างที่สมบูรณ์:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Registration Form',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        inputDecorationTheme: InputDecorationTheme(
          border: OutlineInputBorder(
            borderRadius: BorderRadius.circular(8),
          ),
        ),
      ),
      home: RegistrationForm(),
    );
  }
}

class RegistrationForm extends StatefulWidget {
  @override
  _RegistrationFormState createState() => _RegistrationFormState();
}

class _RegistrationFormState extends State<RegistrationForm> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  final _confirmPasswordController = TextEditingController();
  
  String _gender = 'male';
  bool _acceptTerms = false;

  @override
  void dispose() {
    _nameController.dispose();
    _emailController.dispose();
    _passwordController.dispose();
    _confirmPasswordController.dispose();
    super.dispose();
  }

  String? _validateName(String? value) {
    if (value == null || value.isEmpty) {
      return 'กรุณาใส่ชื่อ';
    }
    return null;
  }

  String? _validateEmail(String? value) {
    if (value == null || value.isEmpty) {
      return 'กรุณาใส่อีเมล';
    }
    String pattern = r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$';
    RegExp regex = RegExp(pattern);
    if (!regex.hasMatch(value)) {
      return 'รูปแบบอีเมลไม่ถูกต้อง';
    }
    return null;
  }

  String? _validatePassword(String? value) {
    if (value == null || value.isEmpty) {
      return 'กรุณาใส่รหัสผ่าน';
    }
    if (value.length < 6) {
      return 'รหัสผ่านต้องมีอย่างน้อย 6 ตัวอักษร';
    }
    return null;
  }

  String? _validateConfirmPassword(String? value) {
    if (value == null || value.isEmpty) {
      return 'กรุณายืนยันรหัสผ่าน';
    }
    if (value != _passwordController.text) {
      return 'รหัสผ่านไม่ตรงกัน';
    }
    return null;
  }

  void _submitForm() {
    if (_formKey.currentState!.validate()) {
      if (!_acceptTerms) {
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(
            content: Text('กรุณายอมรับข้อตกลงและเงื่อนไข'),
            backgroundColor: Colors.red,
          ),
        );
        return;
      }
      
      // ข้อมูลถูกต้องสมบูรณ์
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text('ลงทะเบียนสำเร็จ!'),
          backgroundColor: Colors.green,
        ),
      );
      
      // แสดงข้อมูลที่กรอก
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: Text('ข้อมูลการลงทะเบียน'),
          content: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text('ชื่อ: ${_nameController.text}'),
              Text('อีเมล: ${_emailController.text}'),
              Text('เพศ: ${_gender == 'male' ? 'ชาย' : 'หญิง'}'),
            ],
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(context),
              child: Text('ตกลง'),
            ),
          ],
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('ลงทะเบียน'),
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: SingleChildScrollView(
        padding: EdgeInsets.all(16),
        child: Form(
          key: _formKey,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              // ช่องชื่อ
              TextFormField(
                controller: _nameController,
                validator: _validateName,
                decoration: InputDecoration(
                  labelText: 'ชื่อ',
                  prefixIcon: Icon(Icons.person),
                ),
              ),
              SizedBox(height: 16),
              
              // ช่องอีเมล
              TextFormField(
                controller: _emailController,
                validator: _validateEmail,
                keyboardType: TextInputType.emailAddress,
                decoration: InputDecoration(
                  labelText: 'อีเมล',
                  prefixIcon: Icon(Icons.email),
                ),
              ),
              SizedBox(height: 16),
              
              // ช่องรหัสผ่าน
              TextFormField(
                controller: _passwordController,
                validator: _validatePassword,
                obscureText: true,
                decoration: InputDecoration(
                  labelText: 'รหัสผ่าน',
                  prefixIcon: Icon(Icons.lock),
                ),
              ),
              SizedBox(height: 16),
              
              // ช่องยืนยันรหัสผ่าน
              TextFormField(
                controller: _confirmPasswordController,
                validator: _validateConfirmPassword,
                obscureText: true,
                decoration: InputDecoration(
                  labelText: 'ยืนยันรหัสผ่าน',
                  prefixIcon: Icon(Icons.lock_outline),
                ),
              ),
              SizedBox(height: 20),
              
              // เลือกเพศ
              Text(
                'เพศ',
                style: TextStyle(
                  fontSize: 16,
                  fontWeight: FontWeight.bold,
                ),
              ),
              Row(
                children: [
                  Expanded(
                    child: RadioListTile<String>(
                      title: Text('ชาย'),
                      value: 'male',
                      groupValue: _gender,
                      onChanged: (String? value) {
                        setState(() {
                          _gender = value!;
                        });
                      },
                    ),
                  ),
                  Expanded(
                    child: RadioListTile<String>(
                      title: Text('หญิง'),
                      value: 'female',
                      groupValue: _gender,
                      onChanged: (String? value) {
                        setState(() {
                          _gender = value!;
                        });
                      },
                    ),
                  ),
                ],
              ),
              SizedBox(height: 16),
              
              // ยอมรับเงื่อนไข
              CheckboxListTile(
                title: Text('ยอมรับข้อตกลงและเงื่อนไข'),
                value: _acceptTerms,
                onChanged: (bool? value) {
                  setState(() {
                    _acceptTerms = value ?? false;
                  });
                },
                controlAffinity: ListTileControlAffinity.leading,
              ),
              SizedBox(height: 24),
              
              // ปุ่มลงทะเบียน
              ElevatedButton(
                onPressed: _submitForm,
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.blue,
                  foregroundColor: Colors.white,
                  padding: EdgeInsets.symmetric(vertical: 16),
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(8),
                  ),
                ),
                child: Text(
                  'ลงทะเบียน',
                  style: TextStyle(fontSize: 18),
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

### ผลลัพธ์ที่คาดหวัง:
- ฟอร์มที่สมบูรณ์พร้อม validation
- แสดง error message เมื่อข้อมูลไม่ถูกต้อง
- แสดง dialog ยืนยันเมื่อลงทะเบียนสำเร็จ
- UI ที่สวยงามและใช้งานง่าย

### Tips สำหรับผู้สอน:
1. **อธิบายทีละส่วน** - เริ่มจาก Widget ง่ายๆ ก่อน
2. **ให้ผู้เรียนลองพิมพ์เอง** - อย่าใจให้ copy-paste
3. **แสดงผลลัพธ์ให้เห็น** - รันโค้ดและดูการทำงาน
4. **อธิบาย Best Practices** - เช่นการใช้ dispose(), การตั้งชื่อตัวแปร
5. **เปิดโอกาสให้ถาม** - Interactive Widgets มีรายละเอียดเยอะ
