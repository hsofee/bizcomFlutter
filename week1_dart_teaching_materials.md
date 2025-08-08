# สัปดาห์ที่ 1: รู้จักกับ Dart และพื้นฐานการเขียนโปรแกรม

## Day 1: รู้จักภาษา Dart และการติดตั้ง

### 🎯 วัตถุประสงค์
- เข้าใจว่า Dart คืออะไรและทำไมสำคัญ
- ติดตั้ง Development Environment ให้เสร็จสมบูรณ์
- เขียนโปรแกรม Dart แรกได้สำเร็จ

### 📚 เนื้อหาบทเรียน

#### 1.1 ภาษา Dart คืออะไร? (30 นาที)

**Dart คือ** ภาษาโปรแกรมมิ่งที่พัฒนาโดย Google เพื่อสร้างแอปพลิเคชันสมัยใหม่

**จุดเด่นของ Dart:**
- **เรียนรู้ง่าย** - Syntax คล้าย Java, C++, JavaScript
- **ประสิทธิภาพสูง** - Compile เป็น native code ได้
- **Cross-platform** - รันได้ทั้ง Mobile, Web, Desktop
- **Strong Typing** - ป้องกัน error ตั้งแต่เขียนโค้ด

**ใช้ Dart ทำอะไรได้บ้าง?**
- 📱 Mobile Apps (ผ่าน Flutter)
- 🌐 Web Applications 
- 💻 Desktop Applications
- 🔧 Command Line Tools

#### 1.2 การติดตั้ง Dart SDK (45 นาที)

**ขั้นตอนการติดตั้ง:**

**สำหรับ Windows:**
1. ไปที่ https://dart.dev/get-dart
2. Download Dart SDK
3. Extract ไฟล์และเพิ่ม Path
4. เปิด Command Prompt และพิมพ์ `dart --version`

**สำหรับ macOS:**
```bash
brew tap dart-lang/dart
brew install dart
```

**สำหรับ Linux:**
```bash
sudo apt update
sudo apt install apt-transport-https
sudo sh -c 'wget -qO- https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -'
sudo sh -c 'wget -qO- https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > /etc/apt/sources.list.d/dart_stable.list'
sudo apt update
sudo apt install dart
```

#### 1.3 การติดตั้ง IDE (30 นาที)

**แนะนำ VS Code:**
1. Download VS Code จาก https://code.visualstudio.com/
2. ติดตั้ง Extension "Dart"
3. ติดตั้ง Extension "Code Runner"

**การตั้งค่า VS Code:**
- เปิด Settings (Ctrl+,)
- ค้นหา "dart"
- ตั้งค่า Dart SDK path

#### 1.4 โปรแกรม Dart แรก (15 นาที)

```dart
// ไฟล์: hello.dart
void main() {
  print('สวัสดี ชาว Dart!');
  print('ยินดีต้อนรับสู่โลกของการเขียนโปรแกรม');
}
```

**วิธีรันโปรแกรม:**
```bash
dart hello.dart
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 1

**กิจกรรม 1.1: ติดตั้งและทดสอบ**
- ทุกคนติดตั้ง Dart และ VS Code
- สร้างและรันโปรแกรม Hello World
- ทดสอบว่าทุกอย่างทำงานได้

**กิจกรรม 1.2: ปรับแต่งโปรแกรมแรก**
```dart
void main() {
  print('===================');
  print('ชื่อของฉัน: [ใส่ชื่อของคุณ]');
  print('อายุ: [ใส่อายุของคุณ]');
  print('งานอดิเรก: [ใส่งานอดิเรกของคุณ]');
  print('===================');
}
```

---

## Day 2: ตัวแปรและชนิดข้อมูล

### 🎯 วัตถุประสงค์
- เข้าใจแนวคิดของตัวแปรและชนิดข้อมูล
- ใช้ตัวแปรชนิดต่างๆ ได้อย่างถูกต้อง
- เข้าใจการประกาศตัวแปรแบบต่างๆ

### 📚 เนื้อหาบทเรียน

#### 2.1 ตัวแปร (Variables) คืออะไร? (20 นาที)

**ตัวแปร** คือ กล่องเก็บข้อมูลที่มีชื่อเรียก

```dart
// การประกาศตัวแปร
ชนิดข้อมูล ชื่อตัวแปร = ค่าเริ่มต้น;

// ตัวอย่าง
int age = 20;
String name = 'สมชาย';
```

#### 2.2 ชนิดข้อมูลพื้นฐาน (40 นาที)

**1. int - จำนวนเต็ม**
```dart
int studentCount = 30;
int temperature = -5;
int year = 2024;

print('จำนวนนักเรียน: $studentCount คน');
```

**2. double - จำนวนจริง (ทศนิยม)**
```dart
double price = 250.75;
double weight = 68.5;
double pi = 3.14159;

print('ราคา: $price บาท');
```

**3. String - ข้อความ**
```dart
String firstName = 'สมชาย';
String lastName = "ใจดี";
String fullName = '$firstName $lastName';

print('ชื่อเต็ม: $fullName');
```

**4. bool - ค่าความจริง (จริง/เท็จ)**
```dart
bool isStudent = true;
bool isWorking = false;

print('เป็นนักเรียน: $isStudent');
```

#### 2.3 การประกาศตัวแปรแบบต่างๆ (30 นาที)

**1. การระบุชนิดข้อมูลชัดเจน**
```dart
int age = 25;
double height = 175.5;
String name = 'อภิชาติ';
bool isMarried = false;
```

**2. การใช้ var (Auto Type Inference)**
```dart
var age = 25;        // Dart รู้ว่าเป็น int
var height = 175.5;  // Dart รู้ว่าเป็น double  
var name = 'อภิชาติ';  // Dart รู้ว่าเป็น String
```

**3. การใช้ dynamic**
```dart
dynamic data = 'Hello';
print(data);  // Hello

data = 123;
print(data);  // 123

data = true;
print(data);  // true
```

**4. การประกาศแบบ late**
```dart
late String userName;  // จะกำหนดค่าทีหลัง

void getUserName() {
  userName = 'admin';
}
```

#### 2.4 String Interpolation (15 นาที)

```dart
String name = 'สมหญิง';
int age = 22;
double gpa = 3.75;

// วิธีแทรกค่าในข้อความ
print('ชื่อ: $name');
print('อายุ: $age ปี');
print('เกรด: ${gpa.toStringAsFixed(2)}');
print('ปีเกิด: ${2024 - age}');
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 2

**กิจกรรม 2.1: สร้างโปรไฟล์ส่วนตัว**
```dart
void main() {
  // ข้อมูลส่วนตัว
  String name = 'ใส่ชื่อคุณ';
  int age = 0;  // ใส่อายุคุณ
  double height = 0.0;  // ใส่ส่วนสูงคุณ (cm)
  bool isStudent = true;  // เป็นนักเรียนหรือไม่
  
  // แสดงผล
  print('=== โปรไฟล์ส่วนตัว ===');
  print('ชื่อ: $name');
  print('อายุ: $age ปี');
  print('ส่วนสูง: $height ซม.');
  print('สถานะ: ${isStudent ? 'นักเรียน' : 'ทำงาน'}');
}
```

**กิจกรรม 2.2: เครื่องคิดเลขง่ายๆ**
```dart
void main() {
  // ตัวเลขสองจำนวน
  double num1 = 15.5;
  double num2 = 4.2;
  
  // คำนวณ
  double sum = num1 + num2;
  double difference = num1 - num2;
  double product = num1 * num2;
  double quotient = num1 / num2;
  
  // แสดงผล
  print('$num1 + $num2 = $sum');
  print('$num1 - $num2 = $difference');
  print('$num1 × $num2 = $product');
  print('$num1 ÷ $num2 = ${quotient.toStringAsFixed(2)}');
}
```

---

## Day 3: Operators และ Expressions

### 🎯 วัตถุประสงค์
- เข้าใจและใช้ Operators ต่างๆ ได้
- สร้าง Expressions ที่ซับซ้อนได้
- ประยุกต์ใช้ Operators ในการแก้ปัญหา

### 📚 เนื้อหาบทเรียน

#### 3.1 Arithmetic Operators (20 นาที)

```dart
void main() {
  int a = 15;
  int b = 4;
  
  print('a + b = ${a + b}');    // บวก = 19
  print('a - b = ${a - b}');    // ลบ = 11  
  print('a * b = ${a * b}');    // คูณ = 60
  print('a / b = ${a / b}');    // หาร = 3.75
  print('a ~/ b = ${a ~/ b}');  // หารเอาเฉพาะจำนวนเต็ม = 3
  print('a % b = ${a % b}');    // หารเอาเศษ = 3
}
```

#### 3.2 Assignment Operators (15 นาที)

```dart
void main() {
  int score = 80;
  
  print('คะแนนเริ่มต้น: $score');
  
  score += 10;  // score = score + 10
  print('หลังเพิ่ม 10: $score');
  
  score -= 5;   // score = score - 5
  print('หลังลด 5: $score');
  
  score *= 2;   // score = score * 2
  print('หลังคูณ 2: $score');
  
  score ~/= 3;  // score = score ~/ 3
  print('หลังหาร 3: $score');
}
```

#### 3.3 Comparison Operators (20 นาที)

```dart
void main() {
  int x = 10;
  int y = 20;
  
  print('x = $x, y = $y');
  print('x == y: ${x == y}');  // เท่ากัน? false
  print('x != y: ${x != y}');  // ไม่เท่ากัน? true
  print('x > y: ${x > y}');    // มากกว่า? false
  print('x < y: ${x < y}');    // น้อยกว่า? true
  print('x >= y: ${x >= y}');  // มากกว่าหรือเท่ากับ? false
  print('x <= y: ${x <= y}');  // น้อยกว่าหรือเท่ากับ? true
}
```

#### 3.4 Logical Operators (20 นาที)

```dart
void main() {
  bool isStudent = true;
  bool hasJob = false;
  int age = 20;
  
  // AND (&&) - ทั้งสองเงื่อนไขต้องเป็น true
  bool canGetLoan = isStudent && (age >= 18);
  print('สามารถกู้เงินได้: $canGetLoan');
  
  // OR (||) - เงื่อนไขใดเงื่อนไขหนึ่งเป็น true
  bool canWork = (age >= 18) || hasJob;
  print('สามารถทำงานได้: $canWork');
  
  // NOT (!) - กลับค่าความจริง
  bool isNotStudent = !isStudent;
  print('ไม่ใช่นักเรียน: $isNotStudent');
}
```

#### 3.5 Increment และ Decrement (10 นาที)

```dart
void main() {
  int counter = 5;
  
  print('เริ่มต้น: $counter');
  
  counter++;  // เพิ่มค่าทีหลัง
  print('หลัง counter++: $counter');
  
  ++counter;  // เพิ่มค่าก่อน
  print('หลัง ++counter: $counter');
  
  counter--;  // ลดค่าทีหลัง
  print('หลัง counter--: $counter');
  
  --counter;  // ลดค่าก่อน
  print('หลัง --counter: $counter');
}
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 3

**กิจกรรม 3.1: โปรแกรมคำนวณเกรด**
```dart
void main() {
  // คะแนนสอบ
  double midterm = 75.5;
  double final = 82.0;
  double homework = 90.0;
  
  // น้ำหนักคะแนน
  double midtermWeight = 0.30;
  double finalWeight = 0.50;
  double homeworkWeight = 0.20;
  
  // คำนวณเกรดรวม
  double totalScore = (midterm * midtermWeight) + 
                     (final * finalWeight) + 
                     (homework * homeworkWeight);
  
  print('=== ผลการคำนวณเกรด ===');
  print('คะแนนกลางภาค: $midterm (${midtermWeight * 100}%)');
  print('คะแนนปลายภาค: $final (${finalWeight * 100}%)');
  print('คะแนนงาน: $homework (${homeworkWeight * 100}%)');
  print('คะแนนรวม: ${totalScore.toStringAsFixed(2)}');
  
  // ตัดเกรด
  String grade = '';
  if (totalScore >= 80) {
    grade = 'A';
  } else if (totalScore >= 70) {
    grade = 'B';
  } else if (totalScore >= 60) {
    grade = 'C';
  } else if (totalScore >= 50) {
    grade = 'D';
  } else {
    grade = 'F';
  }
  
  print('เกรดที่ได้: $grade');
}
```

---

## Day 4-5: Control Flow (If-else, Switch, Loops)

### 🎯 วัตถุประสงค์
- ใช้ if-else ในการควบคุมการทำงานของโปรแกรม
- ประยุกต์ใช้ switch statement
- สร้าง loops สำหรับการทำงานซ้ำ

### 📚 เนื้อหาบทเรียน

#### 4.1 If-else Statements (30 นาที)

```dart
void main() {
  int score = 85;
  
  // If เดี่ยว
  if (score >= 50) {
    print('ผ่านการสอบ!');
  }
  
  // If-else
  if (score >= 80) {
    print('ได้เกรด A');
  } else {
    print('ได้เกรดต่ำกว่า A');
  }
  
  // If-else if-else
  if (score >= 80) {
    print('เกรด: A');
  } else if (score >= 70) {
    print('เกรด: B');
  } else if (score >= 60) {
    print('เกรด: C');
  } else if (score >= 50) {
    print('เกรด: D');
  } else {
    print('เกรด: F');
  }
}
```

**Nested If (If ซ้อน If)**
```dart
void main() {
  int age = 25;
  bool hasLicense = true;
  bool hasExperience = true;
  
  if (age >= 18) {
    print('อายุผ่านเกณฑ์');
    
    if (hasLicense) {
      print('มีใบขับขี่');
      
      if (hasExperience) {
        print('สามารถขับรถได้!');
      } else {
        print('ต้องฝึกขับก่อน');
      }
    } else {
      print('ต้องสอบใบขับขี่ก่อน');
    }
  } else {
    print('อายุไม่ถึงเกณฑ์');
  }
}
```

#### 4.2 Switch Statements (25 นาที)

```dart
void main() {
  String day = 'Monday';
  
  switch (day) {
    case 'Monday':
      print('วันจันทร์ - เริ่มต้นสัปดาห์ใหม่!');
      break;
    case 'Tuesday':
      print('วันอังคาร - ทำงานต่อ');
      break;
    case 'Wednesday':
      print('วันพุธ - ครึ่งสัปดาห์แล้ว');
      break;
    case 'Thursday':
      print('วันพฤหัสบดี - ใกล้หมดสัปดาห์');
      break;
    case 'Friday':
      print('วันศุกร์ - TGIF!');
      break;
    case 'Saturday':
    case 'Sunday':
      print('วันหยุด - พักผ่อน');
      break;
    default:
      print('ไม่รู้จักวันนี้');
  }
}
```

**Switch Expression (Dart 3.0+)**
```dart
void main() {
  int month = 12;
  
  String season = switch (month) {
    >= 3 && <= 5 => 'ฤดูร้อน',
    >= 6 && <= 10 => 'ฤดูฝน',
    >= 11 && <= 2 => 'ฤดูหนาว',
    _ => 'ไม่ทราบฤดู'
  };
  
  print('เดือน $month อยู่ใน$season');
}
```

#### 4.3 For Loops (30 นาที)

**For Loop พื้นฐาน**
```dart
void main() {
  // พิมพ์ตัวเลข 1-10
  print('นับ 1 ถึง 10:');
  for (int i = 1; i <= 10; i++) {
    print('หมายเลข: $i');
  }
  
  // นับถอยหลัง
  print('\nนับถอยหลัง:');
  for (int i = 5; i >= 1; i--) {
    print('$i...');
  }
  print('เริ่ม!');
}
```

**For-in Loop**
```dart
void main() {
  List<String> fruits = ['แอปเปิ้ล', 'กล้วย', 'ส้ม', 'มะม่วง'];
  
  print('ผลไม้ที่ชอบ:');
  for (String fruit in fruits) {
    print('- $fruit');
  }
}
```

#### 4.4 While และ Do-While Loops (25 นาที)

**While Loop**
```dart
void main() {
  int count = 1;
  
  print('While Loop:');
  while (count <= 5) {
    print('รอบที่ $count');
    count++;
  }
}
```

**Do-While Loop**
```dart
void main() {
  int number;
  
  print('Do-While Loop:');
  do {
    number = 6; // สมมติได้รับตัวเลข
    print('ตัวเลขที่ได้: $number');
  } while (number != 6);
  
  print('เดาถูกแล้ว!');
}
```

**ตัวอย่างการใช้งานจริง - เกมทายตัวเลข**
```dart
import 'dart:math';

void main() {
  int secretNumber = Random().nextInt(10) + 1; // 1-10
  int guess = 0;
  int attempts = 0;
  
  print('=== เกมทายตัวเลข ===');
  print('ฉันคิดตัวเลข 1-10 ทายดูสิ!');
  
  do {
    attempts++;
    guess = 7; // สมมติผู้เล่นทาย (ในโปรแกรมจริงจะรับ input)
    
    if (guess < secretNumber) {
      print('น้อยไป! ทายใหม่');
    } else if (guess > secretNumber) {
      print('มากไป! ทายใหม่');
    } else {
      print('ถูกต้อง! คำตอบคือ $secretNumber');
      print('คุณทายถูกในรอบที่ $attempts');
    }
    
    // ออกจาก loop เพื่อป้องกันการวนไม่สิ้นสุด
    break;
  } while (guess != secretNumber);
}
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 4-5

**โปรเจค: ระบบจัดการคะแนนนักเรียน**
```dart
void main() {
  // ข้อมูลนักเรียน
  List<String> studentNames = ['สมชาย', 'สมหญิง', 'สมศักดิ์', 'สมใจ'];
  List<double> scores = [85.5, 92.0, 78.5, 88.0];
  
  print('=== รายงานผลการเรียน ===\n');
  
  // แสดงผลแต่ละคน
  for (int i = 0; i < studentNames.length; i++) {
    String name = studentNames[i];
    double score = scores[i];
    
    // คำนวณเกรด
    String grade;
    String remark;
    
    if (score >= 80) {
      grade = 'A';
      remark = 'ดีเยี่ยม';
    } else if (score >= 70) {
      grade = 'B';  
      remark = 'ดี';
    } else if (score >= 60) {
      grade = 'C';
      remark = 'พอใช้';
    } else if (score >= 50) {
      grade = 'D';
      remark = 'อ่อน';
    } else {
      grade = 'F';
      remark = 'ตก';
    }
    
    // แสดงผล
    print('นักเรียน: $name');
    print('คะแนน: $score');
    print('เกรด: $grade ($remark)');
    print('-------------------');
  }
  
  // หาค่าสถิติ
  double total = 0;
  double highest = scores[0];
  double lowest = scores[0];
  
  for (double score in scores) {
    total += score;
    if (score > highest) highest = score;
    if (score < lowest) lowest = score;
  }
  
  double average = total / scores.length;
  
  print('\n=== สถิติรวม ===');
  print('คะแนนเฉลี่ย: ${average.toStringAsFixed(2)}');
  print('คะแนนสูงสุด: $highest');
  print('คะแนนต่ำสุด: $lowest');
  print('จำนวนนักเรียน: ${scores.length} คน');
}
```

### 📝 แบบฝึกหัดท้ายสัปดาห์

**โปรเจค: เครื่องคิดเลขอัจฉริยะ**
```dart
void main() {
  print('=== เครื่องคิดเลขอัจฉริยะ ===');
  
  double num1 = 25.0;
  double num2 = 5.0;
  String operation = '+'; // +, -, *, /, %
  
  print('ตัวเลขที่ 1: $num1');
  print('ตัวเลขที่ 2: $num2');
  print('การดำเนินการ: $operation');
  
  double result = 0;
  bool validOperation = true;
  
  switch (operation) {
    case '+':
      result = num1 + num2;
      break;
    case '-':
      result = num1 - num2;
      break;
    case '*':
      result = num1 * num2;
      break;
    case '/':
      if (num2 != 0) {
        result = num1 / num2;
      } else {
        print('❌ ข้อผิดพลาด: ไม่สามารถหารด้วยศูนย์ได้');
        validOperation = false;
      }
      break;
    case '%':
      if (num2 != 0) {
        result = num1 % num2;
      } else {
        print('❌ ข้อผิดพลาด: ไม่สามารถหารด้วยศูนย์ได้');
        validOperation = false;
      }
      break;
    default:
      print('❌ ข้อผิดพลาด: ไม่รู้จักการดำเนินการ "$operation"');
      validOperation = false;
  }
  
  if (validOperation) {
    print('✅ ผลลัพธ์: $num1 $operation $num2 = $result');
  }
}
```

### 📚 สรุปสัปดาห์ที่ 1

**สิ่งที่เรียนรู้:**
- ✅ การติดตั้งและใช้งาน Dart
- ✅ ตัวแปรและชนิดข้อมูลพื้นฐาน
- ✅ Operators ทุกประเภท
- ✅ Control Flow (if-else, switch, loops)
- ✅ การสร้างโปรแกรมง่ายๆ

**ทักษะที่ได้รับ:**
- สามารถเขียนโปรแกรม Dart พื้นฐานได้
- เข้าใจการใช้ตัวแปรและการคำนวณ
- ควบคุมการทำงานของโปรแกรมด้วย Control Flow
- แก้ปัญหาด้วยการเขียนโปรแกรม

---

## 🏠 การบ้านสัปดาห์ที่ 1

### โปรเจค: ระบบจัดการสินค้าง่ายๆ

**ความต้องการ:**
สร้างโปรแกรมจัดการสินค้าในร้านค้าเล็กๆ ที่สามารถ:
1. เก็บข้อมูลสินค้า (ชื่อ, ราคา, จำนวน)
2. คำนวณมูลค่ารวมของสินค้า
3. ตรวจสอบสินค้าที่ใกล้หมด
4. แสดงรายงานสรุป

**โครงสร้างโปรแกรม:**

```dart
void main() {
  print('=== ระบบจัดการสินค้า ===\n');
  
  // ข้อมูลสินค้า - ใช้ Lists เก็บข้อมูล
  List<String> productNames = [
    'ข้าวโพดกระป่อง',
    'น้ำดื่ม',
    'ขนมปัง',
    'นม',
    'ไข่'
  ];
  
  List<double> prices = [25.0, 7.0, 30.0, 45.0, 60.0];
  List<int> quantities = [5, 20, 8, 3, 12];
  
  // TODO: นักเรียนเขียนโค้ดต่อไปนี้
  
  // 1. แสดงรายการสินค้าทั้งหมด
  print('📦 รายการสินค้าทั้งหมด:');
  // เขียน for loop เพื่อแสดงสินค้าแต่ละรายการ
  
  // 2. คำนวณมูลค่ารวมของสินค้าทั้งหมด
  double totalValue = 0;
  // เขียนโค้ดคำนวณ
  
  // 3. หาสินค้าที่มีจำนวนน้อยกว่า 10 ชิ้น
  print('\n⚠️ สินค้าที่ใกล้หมด (น้อยกว่า 10 ชิ้น):');
  // เขียน if condition ในการตรวจสอบ
  
  // 4. หาสินค้าที่แพงที่สุดและถูกที่สุด
  // เขียนโค้ดหาค่าสูงสุดและต่ำสุด
  
  // 5. แสดงรายงานสรุป
  print('\n📊 รายงานสรุป:');
  print('จำนวนสินค้าทั้งหมด: ${productNames.length} รายการ');
  print('มูลค่ารวม: $totalValue บาท');
  // เพิ่มข้อมูลอื่นๆ
}
```

**เฉลยตัวอย่าง:**

```dart
void main() {
  print('=== ระบบจัดการสินค้า ===\n');
  
  // ข้อมูลสินค้า
  List<String> productNames = [
    'ข้าวโพดกระป่อง',
    'น้ำดื่ม', 
    'ขนมปัง',
    'นม',
    'ไข่'
  ];
  
  List<double> prices = [25.0, 7.0, 30.0, 45.0, 60.0];
  List<int> quantities = [5, 20, 8, 3, 12];
  
  // 1. แสดงรายการสินค้าทั้งหมด
  print('📦 รายการสินค้าทั้งหมด:');
  for (int i = 0; i < productNames.length; i++) {
    double itemValue = prices[i] * quantities[i];
    print('${i + 1}. ${productNames[i]}');
    print('   ราคา: ${prices[i]} บาท/ชิ้น');
    print('   จำนวน: ${quantities[i]} ชิ้น');
    print('   มูลค่า: $itemValue บาท');
    print('');
  }
  
  // 2. คำนวณมูลค่ารวม
  double totalValue = 0;
  for (int i = 0; i < productNames.length; i++) {
    totalValue += prices[i] * quantities[i];
  }
  
  // 3. หาสินค้าที่ใกล้หมด
  print('⚠️ สินค้าที่ใกล้หมด (น้อยกว่า 10 ชิ้น):');
  int lowStockCount = 0;
  for (int i = 0; i < productNames.length; i++) {
    if (quantities[i] < 10) {
      print('- ${productNames[i]}: ${quantities[i]} ชิ้น');
      lowStockCount++;
    }
  }
  
  if (lowStockCount == 0) {
    print('ไม่มีสินค้าที่ใกล้หมด');
  }
  
  // 4. หาสินค้าแพงและถูกที่สุด
  double maxPrice = prices[0];
  double minPrice = prices[0];
  String mostExpensive = productNames[0];
  String cheapest = productNames[0];
  
  for (int i = 1; i < prices.length; i++) {
    if (prices[i] > maxPrice) {
      maxPrice = prices[i];
      mostExpensive = productNames[i];
    }
    if (prices[i] < minPrice) {
      minPrice = prices[i];
      cheapest = productNames[i];
    }
  }
  
  // 5. รายงานสรุป
  print('\n📊 รายงานสรุป:');
  print('จำนวนสินค้าทั้งหมด: ${productNames.length} รายการ');
  print('มูลค่ารวม: ${totalValue.toStringAsFixed(2)} บาท');
  print('สินค้าแพงที่สุด: $mostExpensive ($maxPrice บาท)');
  print('สินค้าถูกที่สุด: $cheapest ($minPrice บาท)');
  print('สินค้าใกล้หมด: $lowStockCount รายการ');
  
  // คำนวณค่าเฉลี่ย
  double averagePrice = 0;
  for (double price in prices) {
    averagePrice += price;
  }
  averagePrice = averagePrice / prices.length;
  
  print('ราคาเฉลี่ย: ${averagePrice.toStringAsFixed(2)} บาท');
  
  // แนะนำการจัดการ
  print('\n💡 คำแนะนำ:');
  if (lowStockCount > 0) {
    print('⚠️ ควรสั่งซื้อสินค้าที่ใกล้หมด');
  }
  
  if (totalValue > 1000) {
    print('✅ มูลค่าสินค้าอยู่ในเกณฑ์ดี');
  } else {
    print('📈 ควรเพิ่มสต็อกสินค้า');
  }
}
```

---

## 🧪 แบบทดสอบความเข้าใจ

### ข้อ 1: Multiple Choice (10 คะแนน)

**1.1** ข้อใดคือการประกาศตัวแปรที่ถูกต้องใน Dart?
- a) `String name = "สมชาย";`
- b) `int age = 25;`
- c) `bool isStudent = true;`
- d) ถูกทุกข้อ ✅

**1.2** ผลลัพธ์ของ `15 % 4` คือ?
- a) 3 ✅
- b) 3.75
- c) 4
- d) 15

**1.3** ข้อใดจะทำให้ if statement เป็น true?
```dart
int x = 10;
if (x > 5 && x < 20) {
  print("ถูกต้อง");
}
```
- a) x = 3
- b) x = 15 ✅
- c) x = 25
- d) x = 5

### ข้อ 2: เขียนโค้ด (20 คะแนน)

**2.1** เขียนโปรแกรมคำนวณค่า BMI
```dart
void main() {
  double weight = 70.0;  // กิโลกรัม
  double height = 1.75;  // เมตร
  
  // เขียนโค้ดคำนวณ BMI = weight / (height * height)
  // และแสดงผลว่าอยู่ในเกณฑ์ไหน
  // ต่ำกว่า 18.5 = ผอม
  // 18.5-24.9 = ปกติ
  // 25-29.9 = อ้วน
  // 30 ขึ้นไป = อ้วนมาก
}
```

**2.2** เขียน for loop แสดงตารางสูตรคูณ
```dart
void main() {
  int number = 7;
  
  // แสดงตารางสูตรคูณแม่ 7 จาก 1-12
  // ผลลัพธ์: 
  // 7 x 1 = 7
  // 7 x 2 = 14
  // ...
  // 7 x 12 = 84
}
```

---

## 🔧 เครื่องมือช่วยการเรียนรู้

### 1. Dart Cheat Sheet

```dart
// ===== ตัวแปรและชนิดข้อมูล =====
int number = 42;
double decimal = 3.14;
String text = 'Hello';
bool flag = true;
var auto = 'Auto type';

// ===== Operators =====
// Arithmetic: + - * / ~/ %
// Comparison: == != < > <= >=
// Logical: && || !
// Assignment: = += -= *= /=

// ===== Control Flow =====
if (condition) {
  // code
} else if (condition2) {
  // code
} else {
  // code
}

switch (value) {
  case 'A': break;
  default: break;
}

for (int i = 0; i < 10; i++) {
  // code
}

while (condition) {
  // code
}

// ===== String Interpolation =====
print('Value: $variable');
print('Calculation: ${x + y}');
```

### 2. ข้อผิดพลาดที่พบบ่อย

**❌ ลืมใส่ semicolon**
```dart
int x = 5  // ผิด!
int x = 5; // ถูก!
```

**❌ เปรียบเทียบด้วย = แทน ==**
```dart
if (x = 5) { }  // ผิด!
if (x == 5) { } // ถูก!
```

**❌ ใช้ชื่อตัวแปรที่เป็น keyword**
```dart
int class = 5;  // ผิด! (class เป็น keyword)
int className = 5; // ถูก!
```

### 3. เทคนิคการ Debug

**ใช้ print() เพื่อตรวจสอบค่า:**
```dart
void main() {
  int x = 10;
  print('Debug: x = $x'); // ดูค่าของ x
  
  x = x * 2;
  print('Debug: x after multiply = $x');
}
```

**ตรวจสอบ Logic:**
```dart
if (score >= 80) {
  print('Debug: เข้าเงื่อนไข A');
  grade = 'A';
} else {
  print('Debug: ไม่เข้าเงื่อนไข A, score = $score');
}
```

---

## 📋 Checklist สำหรับอาจารย์

### เตรียมการสอน
- [ ] ตรวจสอบว่านักเรียนติดตั้ง Dart SDK แล้ว
- [ ] เตรียม Code examples ในแต่ละหัวข้อ
- [ ] ทดสอบโค้ดทุกตัวอย่างก่อนสอน
- [ ] เตรียมแบบฝึกหัดเพิ่มเติมสำหรับนักเรียนที่เรียนเร็ว

### ระหว่างการสอน
- [ ] ให้นักเรียนลองเขียนโค้ดตามทุก 15-20 นาที
- [ ] เดินไปช่วยนักเรียนที่มีปัญหา
- [ ] ถามคำถามเพื่อเช็คความเข้าใจ
- [ ] ใช้ตัวอย่างจากชีวิตจริงประกอบ

### หลังการสอน
- [ ] รวบรวมคำถามที่นักเรียนถามบ่อย
- [ ] เตรียมคำตอบสำหรับปัญหาที่พบ
- [ ] ปรับปรุงเนื้อหาตามความเข้าใจของนักเรียน
- [ ] ส่งไฟล์ตัวอย่างและการบ้านให้นักเรียน

---

## 🎯 เป้าหมายสำหรับสัปดาห์หน้า

หลังจบสัปดาห์ที่ 1 นักเรียนควรจะ:

✅ **ทำได้:**
- เขียนโปรแกรม Dart ง่ายๆ ได้เอง
- ใช้ตัวแปรและ operators ได้ถูกต้อง
- สร้าง if-else และ loops ได้
- แก้ไข error พื้นฐานได้

✅ **เข้าใจ:**
- แนวคิดของการเขียนโปรแกรม
- การควบคุมการทำงานของโปรแกรม
- การแก้ปัญหาด้วยโค้ด

✅ **พร้อมสำหรับ:**
- เรียนรู้ Object-Oriented Programming
- ใช้งาน Collections (Lists, Maps)
- เริ่มต้นเรียน Flutter

**การเตรียมตัวสำหรับสัปดาห์ที่ 2:**
- ทบทวนการใช้ Functions
- ฝึกเขียนโค้ดที่ซับซ้อนขึ้น
- เรียนรู้เรื่อง null safety เบื้องต้น