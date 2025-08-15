# สัปดาห์ที่ 2: Object-Oriented Programming และ Collections

## Day 1: รู้จักกับ Functions และ Classes

### 🎯 วัตถุประสงค์
- เข้าใจการสร้างและใช้งาน Functions
- เข้าใจแนวคิด Object-Oriented Programming
- สร้าง Classes และ Objects พื้นฐานได้

### 📚 เนื้อหาบทเรียน

#### 1.1 Functions ขั้นสูง (45 นาที)

**Function พื้นฐาน - ทบทวน**
```dart
// Function แบบไม่มี return value
void greetUser(String name) {
  print('สวัสดี $name!');
}

// Function แบบมี return value
int addNumbers(int a, int b) {
  return a + b;
}

// Function แบบ expression
int multiply(int x, int y) => x * y;
```

**Parameters แบบต่างๆ**
```dart
// Required Parameters
double calculateArea(double width, double height) {
  return width * height;
}

// Optional Positional Parameters
void showInfo(String name, [int? age, String? city]) {
  print('ชื่อ: $name');
  if (age != null) print('อายุ: $age ปี');
  if (city != null) print('เมือง: $city');
}

// Named Parameters
void createUser({required String name, int age = 18, String role = 'user'}) {
  print('สร้างผู้ใช้: $name, อายุ: $age, บทบาท: $role');
}

void main() {
  // การเรียกใช้
  showInfo('สมชาย');
  showInfo('สมหญิง', 25);
  showInfo('สมศักดิ์', 30, 'กรุงเทพ');
  
  createUser(name: 'Admin');
  createUser(name: 'John', age: 25, role: 'manager');
}
```

**Anonymous Functions และ Arrow Functions**
```dart
void main() {
  // Anonymous function
  var calculate = (int x, int y) {
    return x + y;
  };
  
  // Arrow function
  var square = (int n) => n * n;
  
  print('ผลบวก: ${calculate(5, 3)}');
  print('ยกกำลังสอง: ${square(4)}');
  
  // ใช้กับ List methods
  List<int> numbers = [1, 2, 3, 4, 5];
  
  List<int> doubled = numbers.map((n) => n * 2).toList();
  print('คูณ 2: $doubled');
  
  List<int> evens = numbers.where((n) => n % 2 == 0).toList();
  print('เลขคู่: $evens');
}
```

#### 1.2 แนะนำ Object-Oriented Programming (30 นาที)

**OOP คืออะไร?**

Object-Oriented Programming เป็นรูปแบบการเขียนโปรแกรมที่มุ่งเน้นการสร้าง **Objects** ซึ่งประกอบด้วย:

- **Properties (คุณสมบัติ)** - ข้อมูลที่เก็บใน Object
- **Methods (พฤติกรรม)** - ฟังก์ชันที่ Object สามารถทำได้

**ประโยชน์ของ OOP:**
- **Code Reusability** - เขียนครั้งเดียว ใช้ได้หลายที่
- **Modularity** - แบ่งโค้ดเป็นส่วนๆ ที่จัดการง่าย
- **Maintainability** - แก้ไขและอัพเดทได้ง่าย
- **Real-world Modeling** - สร้างโมเดลที่ใกล้เคียงโลกจริง

#### 1.3 Classes และ Objects (45 นาที)

**การสร้าง Class แรก**
```dart
// ประกาศ Class
class Student {
  // Properties (ตัวแปรภายใน Class)
  String name;
  int age;
  String studentId;
  double gpa;
  
  // Constructor (ตัวสร้าง Object)
  Student(this.name, this.age, this.studentId, this.gpa);
  
  // Methods (ฟังก์ชันภายใน Class)
  void introduce() {
    print('สวัสดีครับ ผมชื่อ $name');
    print('อายุ $age ปี รหัสนักศึกษา $studentId');
  }
  
  void showGPA() {
    print('เกรดเฉลี่ย: $gpa');
    
    if (gpa >= 3.5) {
      print('ผลการเรียนดีเยี่ยม! 🎉');
    } else if (gpa >= 3.0) {
      print('ผลการเรียนดี 👍');
    } else if (gpa >= 2.5) {
      print('ผลการเรียนพอใช้ 📚');
    } else {
      print('ควรตั้งใจเรียนมากขึ้น 💪');
    }
  }
  
  // Method ที่ return ค่า
  int calculateBirthYear() {
    return DateTime.now().year - age;
  }
  
  // Method ที่รับ parameter
  void updateGPA(double newGPA) {
    if (newGPA >= 0 && newGPA <= 4.0) {
      gpa = newGPA;
      print('อัพเดท GPA เป็น $gpa แล้ว');
    } else {
      print('GPA ต้องอยู่ระหว่าง 0-4.0');
    }
  }
}

void main() {
  // สร้าง Objects จาก Class
  Student student1 = Student('สมชาย ใจดี', 20, 'STU001', 3.25);
  Student student2 = Student('สมหญิง ขยัน', 19, 'STU002', 3.75);
  
  // เรียกใช้ Methods
  student1.introduce();
  student1.showGPA();
  print('ปีเกิด: ${student1.calculateBirthYear()}');
  
  print('\n--- นักเรียนคนที่ 2 ---');
  student2.introduce();
  student2.showGPA();
  
  // แก้ไข Properties
  student1.updateGPA(3.50);
  student1.showGPA();
}
```

**Constructor แบบต่างๆ**
```dart
class Product {
  String name;
  double price;
  int quantity;
  String category;
  
  // Default Constructor
  Product(this.name, this.price, this.quantity, this.category);
  
  // Named Constructor
  Product.electronics(this.name, this.price, this.quantity) 
    : category = 'Electronics';
  
  Product.food(this.name, this.price, this.quantity) 
    : category = 'Food';
  
  // Constructor with optional parameters
  Product.simple({
    required this.name, 
    required this.price,
    this.quantity = 1,
    this.category = 'General'
  });
  
  void displayInfo() {
    print('สินค้า: $name');
    print('ราคา: $price บาท');
    print('จำนวน: $quantity ชิ้น');
    print('หมวดหมู่: $category');
    print('มูลค่ารวม: ${price * quantity} บาท');
  }
}

void main() {
  // ใช้ constructor แบบต่างๆ
  Product laptop = Product.electronics('Laptop Gaming', 25000, 2);
  Product apple = Product.food('แอปเปิ้ล', 120, 10);
  Product book = Product.simple(name: 'หนังสือ Dart', price: 590);
  
  laptop.displayInfo();
  print('');
  apple.displayInfo();
  print('');
  book.displayInfo();
}
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 1

**กิจกรรม 1.1: สร้าง Class BankAccount**
```dart
class BankAccount {
  String accountNumber;
  String ownerName;
  double balance;
  
  // TODO: สร้าง Constructor
  BankAccount(this.accountNumber, this.ownerName, this.balance);
  
  // TODO: สร้าง Methods ต่อไปนี้
  
  // Method ฝากเงิน
  void deposit(double amount) {
    // เขียนโค้ดฝากเงิน
    // ตรวจสอบว่า amount > 0
    // เพิ่มเงินเข้า balance
    // แสดงผลการทำรายการ
  }
  
  // Method ถอนเงิน  
  void withdraw(double amount) {
    // เขียนโค้ดถอนเงิน
    // ตรวจสอบว่า amount > 0
    // ตรวจสอบว่าเงินในบัญชีเพียงพอ
    // หักเงินจาก balance
    // แสดงผลการทำรายการ
  }
  
  // Method แสดงยอดเงิน
  void showBalance() {
    // แสดงยอดเงินคงเหลือ
  }
  
  // Method โอนเงิน
  void transfer(BankAccount targetAccount, double amount) {
    // เขียนโค้ดโอนเงิน
    // ถอนเงินจากบัญชีตัวเอง
    // ฝากเงินเข้าบัญชีปลายทาง
  }
}
```

---

## Day 2: Inheritance และ Polymorphism

### 🎯 วัตถุประสงค์
- เข้าใจแนวคิด Inheritance (การสืบทอด)
- ใช้งาน Polymorphism ได้
- สร้าง Class hierarchy ที่มีประสิทธิภาพ

### 📚 เนื้อหาบทเรียน

#### 2.1 Inheritance - การสืบทอด (40 นาที)

**แนวคิดของ Inheritance**

Inheritance คือการที่ Class ลูก (Child Class) สามารถสืบทอดคุณสมบัติและพฤติกรรมจาก Class แม่ (Parent Class) ได้

```dart
// Parent Class (Base Class/Super Class)
class Vehicle {
  String brand;
  String model;
  int year;
  double speed;
  
  Vehicle(this.brand, this.model, this.year) : speed = 0;
  
  void start() {
    print('🚗 $brand $model เริ่มต้นเครื่องยนต์');
  }
  
  void stop() {
    speed = 0;
    print('🛑 $brand $model หยุดเครื่องยนต์');
  }
  
  void accelerate(double acceleration) {
    speed += acceleration;
    print('⚡ เร่งความเร็วเป็น $speed km/h');
  }
  
  void displayInfo() {
    print('=== ข้อมูลยานพาหนะ ===');
    print('ยี่ห้อ: $brand');
    print('รุ่น: $model');
    print('ปี: $year');
    print('ความเร็วปัจจุบัน: $speed km/h');
  }
}

// Child Class ที่สืบทอดจาก Vehicle
class Car extends Vehicle {
  int numberOfDoors;
  String fuelType;
  
  // Constructor ของ Child Class
  Car(String brand, String model, int year, this.numberOfDoors, this.fuelType) 
    : super(brand, model, year);  // เรียก Constructor ของ Parent
  
  // Method เพิ่มเติมสำหรับ Car
  void honk() {
    print('🔊 แบบบบบ! (เสียงแตร)');
  }
  
  void openTrunk() {
    print('📦 เปิดท้ายรถ');
  }
  
  // Override Method จาก Parent Class
  @override
  void displayInfo() {
    super.displayInfo();  // เรียก Method ของ Parent ก่อน
    print('จำนวนประตู: $numberOfDoors');
    print('ประเภทเชื้อเพลิง: $fuelType');
  }
}

class Motorcycle extends Vehicle {
  String type;  // Sport, Cruiser, etc.
  bool hasSidecar;
  
  Motorcycle(String brand, String model, int year, this.type, {this.hasSidecar = false}) 
    : super(brand, model, year);
  
  void wheelie() {
    print('🏍️ ยกล้อหน้า! (Wheelie)');
  }
  
  void lean(String direction) {
    print('🏍️ เอียงตัว$direction');
  }
  
  @override
  void displayInfo() {
    super.displayInfo();
    print('ประเภท: $type');
    print('มีรถข้าง: ${hasSidecar ? 'ใช่' : 'ไม่'}');
  }
}

void main() {
  // สร้าง Objects จาก Child Classes
  Car myCar = Car('Toyota', 'Camry', 2023, 4, 'Hybrid');
  Motorcycle myBike = Motorcycle('Honda', 'CBR', 2023, 'Sport');
  
  print('=== ทดสอบรถยนต์ ===');
  myCar.start();
  myCar.accelerate(60);
  myCar.honk();
  myCar.displayInfo();
  
  print('\n=== ทดสอบมอเตอร์ไซค์ ===');
  myBike.start();
  myBike.accelerate(80);
  myBike.wheelie();
  myBike.lean('ซ้าย');
  myBike.displayInfo();
}
```

#### 2.2 Method Overriding (25 นาที)

```dart
class Animal {
  String name;
  String species;
  
  Animal(this.name, this.species);
  
  void makeSound() {
    print('$name ส่งเสียงทักทาย');
  }
  
  void eat() {
    print('$name กำลังกิน');
  }
  
  void sleep() {
    print('$name กำลังหลับ');
  }
}

class Dog extends Animal {
  String breed;
  
  Dog(String name, this.breed) : super(name, 'สุนัข');
  
  @override
  void makeSound() {
    print('🐕 $name เห่า: โฮ่ง โฮ่ง!');
  }
  
  @override
  void eat() {
    print('🐕 $name กินอาหารสุนัขอร่อยๆ');
  }
  
  // Method เพิ่มเติมเฉพาะสุนัข
  void fetch() {
    print('🐕 $name วิ่งไปเก็บของ');
  }
  
  void wagTail() {
    print('🐕 $name โบกหาง');
  }
}

class Cat extends Animal {
  String color;
  
  Cat(String name, this.color) : super(name, 'แมว');
  
  @override
  void makeSound() {
    print('🐱 $name ร้อง: เมี้ยว เมี้ยว!');
  }
  
  @override
  void eat() {
    print('🐱 $name กินอาหารแมวแสนอร่อย');
  }
  
  // Method เพิ่มเติมเฉพาะแมว
  void purr() {
    print('🐱 $name ร้องปรี้ดปร๊าด');
  }
  
  void scratch() {
    print('🐱 $name ข่วนเฟอร์นิเจอร์');
  }
}

void main() {
  Dog myDog = Dog('บัดดี้', 'โกลเด้น รีทรีฟเวอร์');
  Cat myCat = Cat('วิสกี้', 'ส้ม');
  
  // เรียกใช้ Methods ที่ Override
  myDog.makeSound();
  myDog.eat();
  myDog.fetch();
  
  print('');
  
  myCat.makeSound();
  myCat.eat();
  myCat.purr();
}
```

#### 2.3 Polymorphism (20 นาที)

```dart
// Polymorphism - ความสามารถในการมีหลายรูปแบบ
void main() {
  // สร้าง List ของ Animals ที่มีประเภทต่างกัน
  List<Animal> pets = [
    Dog('มี่โก่', 'ชิวาวา'),
    Cat('เหมียว', 'เปอร์เซีย'),
    Dog('ดำ', 'ลาบราดอร์'),
    Cat('ขาว', 'สยาม'),
  ];
  
  print('=== เวลาให้อาหารสัตว์เลี้ยง ===');
  
  // Polymorphism ทำงาน - Method ที่เรียกจะขึ้นอยู่กับประเภทจริงของ Object
  for (Animal pet in pets) {
    pet.makeSound();  // จะเรียก makeSound() ของแต่ละประเภท
    pet.eat();        // จะเรียก eat() ของแต่ละประเภท
    
    // Type checking และ casting
    if (pet is Dog) {
      Dog dog = pet as Dog;  // cast เป็น Dog
      dog.wagTail();
    } else if (pet is Cat) {
      Cat cat = pet as Cat;  // cast เป็น Cat
      cat.purr();
    }
    
    print('');
  }
}
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 2

**กิจกรรม 2.1: ระบบจัดการพนักงาน**
```dart
// Parent Class
class Employee {
  String name;
  String id;
  double baseSalary;
  
  Employee(this.name, this.id, this.baseSalary);
  
  double calculateSalary() {
    return baseSalary;
  }
  
  void displayInfo() {
    print('=== ข้อมูลพนักงาน ===');
    print('ชื่อ: $name');
    print('รหัส: $id');
    print('เงินเดือนพื้นฐาน: $baseSalary บาท');
  }
  
  void work() {
    print('$name กำลังทำงาน');
  }
}

// TODO: สร้าง Child Classes ต่อไปนี้

class FullTimeEmployee extends Employee {
  double overtimeHours;
  double overtimeRate;
  
  // TODO: สร้าง Constructor
  // TODO: Override calculateSalary() - คำนวณรวม overtime
  // TODO: Override displayInfo() - แสดงข้อมูล overtime
}

class PartTimeEmployee extends Employee {
  double hoursWorked;
  double hourlyRate;
  
  // TODO: สร้าง Constructor  
  // TODO: Override calculateSalary() - คำนวณจากชั่วโมงทำงาน
  // TODO: Override displayInfo() - แสดงข้อมูลชั่วโมง
}

class Manager extends Employee {
  double bonus;
  List<String> teamMembers;
  
  // TODO: สร้าง Constructor
  // TODO: Override calculateSalary() - เงินเดือน + โบนัส
  // TODO: สร้าง Method manageTeam()
  // TODO: Override displayInfo() - แสดงข้อมูลทีม
}
```

---

## Day 3: Abstract Classes และ Interfaces

### 🎯 วัตถุประสงค์
- เข้าใจและใช้งาน Abstract Classes
- เข้าใจแนวคิด Interfaces
- ประยุกต์ใช้ในการออกแบบโปรแกรม

### 📚 เนื้อหาบทเรียน

#### 3.1 Abstract Classes (35 นาที)

**Abstract Class คืออะไร?**

Abstract Class คือ Class ที่ไม่สามารถสร้าง Object ได้โดยตรง แต่ใช้เป็น Template สำหรับ Child Classes

```dart
// Abstract Class - ไม่สามารถสร้าง Object ได้โดยตรง
abstract class Shape {
  String name;
  String color;
  
  Shape(this.name, this.color);
  
  // Abstract method - ต้อง implement ใน Child Class
  double calculateArea();
  double calculatePerimeter();
  
  // Concrete method - สามารถใช้ได้เลย
  void displayInfo() {
    print('=== รูปทรง: $name ===');
    print('สี: $color');
    print('พื้นที่: ${calculateArea().toStringAsFixed(2)} ตร.ซม.');
    print('เส้นรอบรูป: ${calculatePerimeter().toStringAsFixed(2)} ซม.');
  }
  
  void paint(String newColor) {
    color = newColor;
    print('ทาสี $name เป็นสี $newColor');
  }
}

// Child Class ต้อง implement abstract methods
class Rectangle extends Shape {
  double width;
  double height;
  
  Rectangle(this.width, this.height, String color) 
    : super('สี่เหลี่ยมผืนผ้า', color);
  
  @override
  double calculateArea() {
    return width * height;
  }
  
  @override
  double calculatePerimeter() {
    return 2 * (width + height);
  }
  
  // Method เพิ่มเติม
  bool isSquare() {
    return width == height;
  }
}

class Circle extends Shape {
  double radius;
  
  Circle(this.radius, String color) : super('วงกลม', color);
  
  @override
  double calculateArea() {
    return 3.14159 * radius * radius;
  }
  
  @override
  double calculatePerimeter() {
    return 2 * 3.14159 * radius;
  }
  
  // Method เพิ่มเติม
  double getDiameter() {
    return radius * 2;
  }
}

class Triangle extends Shape {
  double side1, side2, side3;
  double height;
  
  Triangle(this.side1, this.side2, this.side3, this.height, String color) 
    : super('สามเหลี่ยม', color);
  
  @override
  double calculateArea() {
    // ใช้ base (side1) และ height
    return 0.5 * side1 * height;
  }
  
  @override
  double calculatePerimeter() {
    return side1 + side2 + side3;
  }
  
  // Method เพิ่มเติม
  bool isEquilateral() {
    return side1 == side2 && side2 == side3;
  }
}

void main() {
  // ไม่สามารถสร้าง Object จาก Abstract Class ได้
  // Shape shape = Shape('test', 'red'); // ❌ Error!
  
  // สร้าง Objects จาก Child Classes
  Rectangle rect = Rectangle(10, 5, 'แดง');
  Circle circle = Circle(7, 'น้ำเงิน');
  Triangle triangle = Triangle(3, 4, 5, 2.4, 'เขียว');
  
  // ใช้งาน Polymorphism กับ Abstract Class
  List<Shape> shapes = [rect, circle, triangle];
  
  for (Shape shape in shapes) {
    shape.displayInfo();
    print('');
  }
  
  // ทดสอบ Methods เฉพาะ
  if (rect.isSquare()) {
    print('สี่เหลี่ยมผืนผ้าเป็นสี่เหลี่ยมจัตุรัส');
  }
  
  print('เส้นผ่านศูนย์กลางของวงกลม: ${circle.getDiameter()} ซม.');
  
  if (triangle.isEquilateral()) {
    print('สามเหลี่ยมนี้เป็นสามเหลี่ยมด้านเท่า');
  }
}
```

#### 3.2 Mixins และ Interfaces (30 นาที)

**Mixins - การผสมความสามารถ**
```dart
// Mixin - ชุดความสามารถที่สามารถผสมเข้ากับ Class ได้
mixin Flyable {
  double altitude = 0;
  
  void takeOff() {
    altitude = 100;
    print('🛫 ขึ้นบิน! ความสูง: $altitude เมตร');
  }
  
  void land() {
    altitude = 0;
    print('🛬 ลงจอด! ความสูง: $altitude เมตร');
  }
  
  void fly(double newAltitude) {
    altitude = newAltitude;
    print('✈️ บินที่ความสูง: $altitude เมตร');
  }
}

mixin Swimmable {
  double depth = 0;
  
  void dive(double newDepth) {
    depth = newDepth;
    print('🏊‍♂️ ดำน้ำลึก: $depth เมตร');
  }
  
  void surface() {
    depth = 0;
    print('🏊‍♂️ ผุดขึ้นผิวน้ำ');
  }
  
  void swim() {
    print('🏊‍♂️ ว่ายน้ำ');
  }
}

mixin Walkable {
  double speed = 0;
  
  void walk(double walkSpeed) {
    speed = walkSpeed;
    print('🚶‍♂️ เดินด้วยความเร็ว: $speed km/h');
  }
  
  void run(double runSpeed) {
    speed = runSpeed;
    print('🏃‍♂️ วิ่งด้วยความเร็ว: $speed km/h');
  }
  
  void stop() {
    speed = 0;
    print('⏹️ หยุด');
  }
}

// Base Class
class Animal {
  String name;
  String species;
  
  Animal(this.name, this.species);
  
  void eat() {
    print('$name กำลังกิน');
  }
  
  void sleep() {
    print('$name กำลังหลับ');
  }
}

// Classes ที่ใช้ Mixins
class Bird extends Animal with Flyable, Walkable {
  String wingSpan;
  
  Bird(String name, this.wingSpan) : super(name, 'นก');
  
  void chirp() {
    print('🐦 $name ร้องเพลง: จิ๊บๆ!');
  }
}

class Duck extends Animal with Flyable, Swimmable, Walkable {
  Duck(String name) : super(name, 'เป็ด');
  
  void quack() {
    print('🦆 $name ร้อง: แกะๆ!');
  }
}

class Fish extends Animal with Swimmable {
  String finType;
  
  Fish(String name, this.finType) : super(name, 'ปลา');
  
  void bubble() {
    print('🐟 $name สร้างฟองอากาศ');
  }
}

class Human extends Animal with Walkable, Swimmable {
  String occupation;
  
  Human(String name, this.occupation) : super(name, 'มนุษย์');
  
  void work() {
    print('👨‍💼 $name ทำงานเป็น $occupation');
  }
  
  void think() {
    print('🤔 $name กำลังคิด');
  }
}

void main() {
  Bird eagle = Bird('นกอินทรี', '2 เมตร');
  Duck donald = Duck('โดนัลด์');
  Fish nemo = Fish('นีโม', 'หางใหญ่');
  Human john = Human('จอห์น', 'นักพัฒนา');
  
  print('=== ทดสอบความสามารถของสัตว์ ===');
  
  // นกอินทรี
  eagle.eat();
  eagle.walk(5);
  eagle.takeOff();
  eagle.fly(500);
  eagle.chirp();
  
  print('');
  
  // เป็ด - มีความสามารถ 3 อย่าง
  donald.quack();
  donald.walk(3);
  donald.takeOff();
  donald.land();
  donald.swim();
  donald.dive(2);
  
  print('');
  
  // ปลา
  nemo.swim();
  nemo.dive(10);
  nemo.bubble();
  
  print('');
  
  // มนุษย์
  john.work();
  john.walk(4);
  john.run(10);
  john.swim();
  john.think();
}
```

#### 3.3 Interfaces และ implements (20 นาที)

```dart
// Interface - กำหนดรูปแบบที่ Class ต้องปฏิบัติตาม
abstract class Drawable {
  void draw();
  void erase();
}

abstract class Resizable {
  void resize(double factor);
}

abstract class Movable {
  void moveTo(double x, double y);
}

// Class ที่ implement หลาย interfaces
class GraphicElement implements Drawable, Resizable, Movable {
  String name;
  double x, y;
  double width, height;
  
  GraphicElement(this.name, this.x, this.y, this.width, this.height);
  
  @override
  void draw() {
    print('🎨 วาด $name ที่ตำแหน่ง ($x, $y)');
  }
  
  @override
  void erase() {
    print('🗑️ ลบ $name');
  }
  
  @override
  void resize(double factor) {
    width *= factor;
    height *= factor;
    print('📏 ปรับขนาด $name เป็น ${width}x${height}');
  }
  
  @override
  void moveTo(double newX, double newY) {
    x = newX;
    y = newY;
    print('📍 ย้าย $name ไปยัง ($x, $y)');
  }
  
  void displayInfo() {
    print('$name: ตำแหน่ง($x, $y) ขนาด ${width}x${height}');
  }
}

class Rectangle extends GraphicElement {
  Rectangle(double x, double y, double width, double height) 
    : super('สี่เหลี่ยม', x, y, width, height);
  
  @override
  void draw() {
    super.draw();
    print('  📐 วาดสี่เหลี่ยมผืนผ้า');
  }
}

class Circle extends GraphicElement {
  Circle(double x, double y, double radius) 
    : super('วงกลม', x, y, radius * 2, radius * 2);
  
  @override
  void draw() {
    super.draw();
    print('  ⭕ วาดวงกลม');
  }
}

void main() {
  List<GraphicElement> elements = [
    Rectangle(10, 20, 100, 50),
    Circle(50, 50, 25),
  ];
  
  print('=== การจัดการกราฟิก ===');
  
  for (GraphicElement element in elements) {
    element.displayInfo();
    element.draw();
    element.resize(1.5);
    element.moveTo(element.x + 10, element.y + 10);
    print('');
  }
}
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 3

**กิจกรรม 3.1: ระบบจัดการยานพาหนะ**
```dart
// TODO: สร้าง Abstract Class Vehicle
abstract class Vehicle {
  String brand;
  String model;
  double speed;
  
  Vehicle(this.brand, this.model) : speed = 0;
  
  // Abstract methods
  void start();
  void stop();
  double getFuelEfficiency();
  
  // Concrete method
  void displayInfo() {
    print('ยี่ห้อ: $brand, รุ่น: $model, ความเร็ว: $speed km/h');
  }
}

// TODO: สร้าง Mixins
mixin Electric {
  double batteryLevel = 100.0;
  
  void chargeBattery() {
    batteryLevel = 100.0;
    print('🔋 ชาร์จแบตเตอรี่เต็มแล้ว');
  }
  
  void showBatteryLevel() {
    print('🔋 แบตเตอรี่เหลือ: $batteryLevel%');
  }
}

mixin GPS {
  double latitude = 0;
  double longitude = 0;
  
  void setLocation(double lat, double lng) {
    latitude = lat;
    longitude = lng;
    print('📍 ตำแหน่งปัจจุบัน: ($latitude, $longitude)');
  }
  
  void navigate(String destination) {
    print('🗺️ นำทางไป: $destination');
  }
}

// TODO: สร้าง Child Classes พร้อม Mixins
// class ElectricCar extends Vehicle with Electric, GPS
// class Motorcycle extends Vehicle with GPS  
// class ElectricBike extends Vehicle with Electric
```

---

## Day 4-5: Collections - Lists, Sets, Maps

### 🎯 วัตถุประสงค์
- เข้าใจและใช้งาน Collections ประเภทต่างๆ
- ประยุกต์ใช้ Methods ของ Collections
- จัดการข้อมูลที่ซับซ้อนได้

### 📚 เนื้อหาบทเรียน

#### 4.1 Lists - การจัดการรายการ (45 นาที)

**List พื้นฐาน**
```dart
void main() {
  // การสร้าง List
  List<String> fruits = ['แอปเปิ้ล', 'กล้วย', 'ส้ม'];
  List<int> numbers = [1, 2, 3, 4, 5];
  var mixedList = ['ข้อความ', 42, true, 3.14];
  
  // การเข้าถึงข้อมูล
  print('ผลไม้แรก: ${fruits[0]}');
  print('ตัวเลขสุดท้าย: ${numbers[numbers.length - 1]}');
  
  // การเพิ่มข้อมูล
  fruits.add('มะม่วง');
  fruits.insert(1, 'สับปะรด');  // แทรกที่ตำแหน่ง 1
  fruits.addAll(['ทุเรียน', 'มังคุด']);
  
  print('ผลไม้ทั้งหมด: $fruits');
  
  // การลบข้อมูล
  fruits.remove('กล้วย');           // ลบตามค่า
  fruits.removeAt(0);              // ลบตามตำแหน่ง
  fruits.removeLast();             // ลบตัวสุดท้าย
  fruits.removeWhere((fruit) => fruit.startsWith('ส'));  // ลบตามเงื่อนไข
  
  print('หลังลบ: $fruits');
  
  // การตรวจสอบ
  print('มีแอปเปิ้ลไหม: ${fruits.contains('แอปเปิ้ล')}');
  print('จำนวนผลไม้: ${fruits.length}');
  print('List ว่างไหม: ${fruits.isEmpty}');
}
```

**List Methods ขั้นสูง**
```dart
void main() {
  List<int> scores = [85, 92, 78, 95, 88, 76, 89, 94];
  
  print('คะแนนเดิม: $scores');
  
  // การเรียงลำดับ
  List<int> sortedScores = [...scores];  // คัดลอก List
  sortedScores.sort();
  print('เรียงจากน้อยไปมาก: $sortedScores');
  
  sortedScores.sort((a, b) => b.compareTo(a));  // เรียงจากมากไปน้อย
  print('เรียงจากมากไปน้อย: $sortedScores');
  
  // การค้นหา
  int highestScore = scores.reduce((a, b) => a > b ? a : b);
  int lowestScore = scores.reduce((a, b) => a < b ? a : b);
  print('คะแนนสูงสุด: $highestScore');
  print('คะแนนต่ำสุด: $lowestScore');
  
  // การกรอง
  List<int> highScores = scores.where((score) => score >= 90).toList();
  List<int> lowScores = scores.where((score) => score < 80).toList();
  print('คะแนนสูง (>=90): $highScores');
  print('คะแนนต่ำ (<80): $lowScores');
  
  // การแปลงข้อมูล
  List<String> grades = scores.map((score) {
    if (score >= 90) return 'A';
    if (score >= 80) return 'B';
    if (score >= 70) return 'C';
    if (score >= 60) return 'D';
    return 'F';
  }).toList();
  
  print('เกรด: $grades');
  
  // การคำนวณ
  double average = scores.reduce((a, b) => a + b) / scores.length;
  print('คะแนนเฉลี่ย: ${average.toStringAsFixed(2)}');
  
  // การแบ่งกลุ่ม
  Map<String, List<int>> groupedScores = {
    'สูง': scores.where((s) => s >= 90).toList(),
    'ปานกลาง': scores.where((s) => s >= 80 && s < 90).toList(),
    'ต่ำ': scores.where((s) => s < 80).toList(),
  };
  
  print('\nการแบ่งกลุ่มคะแนน:');
  groupedScores.forEach((group, scoreList) {
    print('$group: $scoreList (${scoreList.length} คน)');
  });
}
```

#### 4.2 Sets - ชุดข้อมูลที่ไม่ซ้ำ (25 นาที)

```dart
void main() {
  // การสร้าง Set
  Set<String> uniqueFruits = {'แอปเปิ้ล', 'กล้วย', 'ส้ม'};
  Set<int> uniqueNumbers = {1, 2, 3, 4, 5, 1, 2, 3}; // ซ้ำจะถูกลบออก
  
  print('ผลไม้ไม่ซ้ำ: $uniqueFruits');
  print('ตัวเลขไม่ซ้ำ: $uniqueNumbers');
  
  // การเพิ่มข้อมูล
  uniqueFruits.add('มะม่วง');
  uniqueFruits.add('แอปเปิ้ล');  // ไม่เพิ่มเพราะมีอยู่แล้ว
  uniqueFruits.addAll({'ทุเรียน', 'มังคุด', 'ส้ม'});
  
  print('หลังเพิ่ม: $uniqueFruits');
  
  // Set Operations
  Set<String> tropicalFruits = {'มะม่วง', 'ทุเรียน', 'มังคุด', 'ลำไย'};
  Set<String> citrusFruits = {'ส้ม', 'มะนาว', 'ส้มโอ', 'แอปเปิ้ล'};
  
  // Union - รวมกัน (ไม่ซ้ำ)
  Set<String> allFruits = uniqueFruits.union(tropicalFruits);
  print('ผลไม้ทั้งหมด: $allFruits');
  
  // Intersection - ตัดกัน (มีทั้งสอง Set)
  Set<String> commonFruits = uniqueFruits.intersection(citrusFruits);
  print('ผลไม้ที่มีทั้งสอง Set: $commonFruits');
  
  // Difference - แตกต่างกัน
  Set<String> uniqueToFirst = uniqueFruits.difference(citrusFruits);
  print('ผลไม้ที่มีแค่ใน Set แรก: $uniqueToFirst');
  
  // การตรวจสอบ
  print('มีมะม่วงไหม: ${uniqueFruits.contains('มะม่วง')}');
  print('เป็น subset ของผลไม้ทั้งหมดไหม: ${tropicalFruits.isSubsetOf(allFruits)}');
  
  // แปลงเป็น List
  List<String> fruitList = uniqueFruits.toList()..sort();
  print('เรียงลำดับผลไม้: $fruitList');
}
```

#### 4.3 Maps - คู่ Key-Value (40 นาที)

```dart
void main() {
  // การสร้าง Map
  Map<String, int> studentGrades = {
    'สมชาย': 85,
    'สมหญิง': 92,
    'สมศักดิ์': 78,
    'สมใจ': 88
  };
  
  // การเข้าถึงข้อมูล
  print('เกรดของสมชาย: ${studentGrades['สมชาย']}');
  print('เกรดของคนที่ไม่มี: ${studentGrades['คนแปลกหน้า']}'); // null
  
  // การเพิ่มและแก้ไข
  studentGrades['สมปอง'] = 95;           // เพิ่มใหม่
  studentGrades['สมชาย'] = 90;           // แก้ไขเกรดเดิม
  studentGrades.addAll({                 // เพิ่มหลายคน
    'สมศรี': 87,
    'สมเกียรติ': 91
  });
  
  print('หลังเพิ่มข้อมูล: $studentGrades');
  
  // การลบข้อมูล
  studentGrades.remove('สมศักดิ์');
  print('หลังลบสมศักดิ์: $studentGrades');
  
  // การวนลูป
  print('\n=== รายชื่อและเกรดทั้งหมด ===');
  studentGrades.forEach((name, grade) {
    String level = '';
    if (grade >= 90) level = 'ดีเยี่ยม';
    else if (grade >= 80) level = 'ดี';
    else if (grade >= 70) level = 'พอใช้';
    else level = 'ควรปรับปรุง';
    
    print('$name: $grade คะแนน ($level)');
  });
  
  // การประมวลผล
  print('\n=== สถิติ ===');
  List<int> allGrades = studentGrades.values.toList();
  List<String> allNames = studentGrades.keys.toList();
  
  int totalGrades = allGrades.reduce((a, b) => a + b);
  double averageGrade = totalGrades / allGrades.length;
  int highestGrade = allGrades.reduce((a, b) => a > b ? a : b);
  int lowestGrade = allGrades.reduce((a, b) => a < b ? a : b);
  
  print('จำนวนนักเรียน: ${studentGrades.length} คน');
  print('คะแนนเฉลี่ย: ${averageGrade.toStringAsFixed(2)}');
  print('คะแนนสูงสุด: $highestGrade');
  print('คะแนนต่ำสุด: $lowestGrade');
  
  // หาคนที่ได้คะแนนสูงสุด
  String topStudent = '';
  studentGrades.forEach((name, grade) {
    if (grade == highestGrade) {
      topStudent = name;
    }
  });
  print('นักเรียนเก่งที่สุด: $topStudent');
}
```

**Nested Collections - Collections ซ้อนกัน**
```dart
void main() {
  // Map ที่มี List เป็น Value
  Map<String, List<int>> classGrades = {
    'คณิตศาสตร์': [85, 92, 78, 95, 88],
    'วิทยาศาสตร์': [79, 88, 94, 87, 91],
    'ภาษาอังกฤษ': [92, 85, 89, 93, 87],
    'สังคมศึกษา': [88, 91, 82, 89, 94]
  };
  
  print('=== สถิติคะแนนแต่ละวิชา ===');
  
  classGrades.forEach((subject, grades) {
    double average = grades.reduce((a, b) => a + b) / grades.length;
    int highest = grades.reduce((a, b) => a > b ? a : b);
    int lowest = grades.reduce((a, b) => a < b ? a : b);
    
    print('\n$subject:');
    print('  คะแนนทั้งหมด: $grades');
    print('  เฉลี่ย: ${average.toStringAsFixed(2)}');
    print('  สูงสุด: $highest');
    print('  ต่ำสุด: $lowest');
  });
  
  // List ของ Maps
  List<Map<String, dynamic>> students = [
    {
      'name': 'สมชาย',
      'age': 20,
      'grades': {'คณิต': 85, 'วิทยา': 79, 'อังกฤษ': 92},
      'isActive': true
    },
    {
      'name': 'สมหญิง',
      'age': 19,
      'grades': {'คณิต': 92, 'วิทยา': 88, 'อังกฤษ': 85},
      'isActive': true
    },
    {
      'name': 'สมศักดิ์',
      'age': 21,
      'grades': {'คณิต': 78, 'วิทยา': 94, 'อังกฤษ': 89},
      'isActive': false
    }
  ];
  
  print('\n\n=== ข้อมูลนักเรียนแต่ละคน ===');
  
  for (Map<String, dynamic> student in students) {
    String name = student['name'];
    int age = student['age'];
    Map<String, int> grades = student['grades'];
    bool isActive = student['isActive'];
    
    print('\n--- $name (อายุ $age ปี) ---');
    print('สถานะ: ${isActive ? 'กำลังเรียน' : 'พักการเรียน'}');
    
    if (isActive) {
      double totalGrade = 0;
      grades.forEach((subject, grade) {
        print('$subject: $grade คะแนน');
        totalGrade += grade;
      });
      
      double avgGrade = totalGrade / grades.length;
      print('เกรดเฉลี่ย: ${avgGrade.toStringAsFixed(2)}');
    }
  }
}
```

### 🏃‍♂️ กิจกรรมปฏิบัติ Day 4-5

**โปรเจคใหญ่: ระบบจัดการห้องสมุด**
```dart
class Book {
  String isbn;
  String title;
  String author;
  String category;
  int publishYear;
  bool isAvailable;
  
  Book({
    required this.isbn,
    required this.title,
    required this.author,
    required this.category,
    required this.publishYear,
    this.isAvailable = true,
  });
  
  void displayInfo() {
    print('📚 $title');
    print('   ผู้แต่ง: $author');
    print('   หมวดหมู่: $category');
    print('   ปีที่พิมพ์: $publishYear');
    print('   สถานะ: ${isAvailable ? 'ว่าง' : 'ถูกยืม'}');
    print('   ISBN: $isbn');
  }
  
  @override
  String toString() {
    return '$title โดย $author';
  }
}

class Member {
  String memberId;
  String name;
  String email;
  List<String> borrowedBooks;
  
  Member({
    required this.memberId,
    required this.name,
    required this.email,
  }) : borrowedBooks = [];
  
  void displayInfo() {
    print('👤 สมาชิก: $name');
    print('   รหัส: $memberId');
    print('   อีเมล: $email');
    print('   หนังสือที่ยืม: ${borrowedBooks.length} เล่ม');
    if (borrowedBooks.isNotEmpty) {
      borrowedBooks.forEach((isbn) => print('     - $isbn'));
    }
  }
}

class Library {
  String name;
  Map<String, Book> books;        // ISBN -> Book
  Map<String, Member> members;    // MemberID -> Member
  Map<String, Set<String>> categories; // Category -> Set of ISBNs
  
  Library(this.name) 
    : books = {},
      members = {},
      categories = {};
  
  // จัดการหนังสือ
  void addBook(Book book) {
    books[book.isbn] = book;
    
    // เพิ่มเข้าหมวดหมู่
    if (!categories.containsKey(book.category)) {
      categories[book.category] = <String>{};
    }
    categories[book.category]!.add(book.isbn);
    
    print('✅ เพิ่มหนังสือ "${book.title}" แล้ว');
  }
  
  void removeBook(String isbn) {
    Book? book = books.remove(isbn);
    if (book != null) {
      categories[book.category]?.remove(isbn);
      print('🗑️ ลบหนังสือ "${book.title}" แล้ว');
    } else {
      print('❌ ไม่พบหนังสือ ISBN: $isbn');
    }
  }
  
  // จัดการสมาชิก
  void addMember(Member member) {
    members[member.memberId] = member;
    print('✅ เพิ่มสมาชิก "${member.name}" แล้ว');
  }
  
  // ยืม-คืนหนังสือ
  void borrowBook(String memberId, String isbn) {
    Member? member = members[memberId];
    Book? book = books[isbn];
    
    if (member == null) {
      print('❌ ไม่พบสมาชิก: $memberId');
      return;
    }
    
    if (book == null) {
      print('❌ ไม่พบหนังสือ: $isbn');
      return;
    }
    
    if (!book.isAvailable) {
      print('❌ หนังสือ "${book.title}" ถูกยืมแล้ว');
      return;
    }
    
    if (member.borrowedBooks.length >= 3) {
      print('❌ ${member.name} ยืมหนังสือครบ 3 เล่มแล้ว');
      return;
    }
    
    book.isAvailable = false;
    member.borrowedBooks.add(isbn);
    print('📖 ${member.name} ยืม "${book.title}" สำเร็จ');
  }
  
  void returnBook(String memberId, String isbn) {
    Member? member = members[memberId];
    Book? book = books[isbn];
    
    if (member == null || book == null) {
      print('❌ ไม่พบข้อมูลสมาชิกหรือหนังสือ');
      return;
    }
    
    if (member.borrowedBooks.contains(isbn)) {
      member.borrowedBooks.remove(isbn);
      book.isAvailable = true;
      print('📚 ${member.name} คืน "${book.title}" สำเร็จ');
    } else {
      print('❌ ${member.name} ไม่ได้ยืมหนังสือเล่มนี้');
    }
  }
  
  // ค้นหาหนังสือ
  List<Book> searchByTitle(String title) {
    return books.values
        .where((book) => book.title.toLowerCase().contains(title.toLowerCase()))
        .toList();
  }
  
  List<Book> searchByAuthor(String author) {
    return books.values
        .where((book) => book.author.toLowerCase().contains(author.toLowerCase()))
        .toList();
  }
  
  List<Book> getBooksByCategory(String category) {
    Set<String>? categoryBooks = categories[category];
    if (categoryBooks == null) return [];
    
    return categoryBooks
        .map((isbn) => books[isbn]!)
        .toList();
  }
  
  // รายงาน
  void showLibraryStats() {
    print('\n📊 === สถิติห้องสมุด $name ===');
    print('จำนวนหนังสือทั้งหมด: ${books.length} เล่ม');
    print('จำนวนสมาชิก: ${members.length} คน');
    
    int availableBooks = books.values.where((book) => book.isAvailable).length;
    int borrowedBooks = books.length - availableBooks;
    
    print('หนังสือว่าง: $availableBooks เล่ม');
    print('หนังสือถูกยืม: $borrowedBooks เล่ม');
    
    print('\nหมวดหมู่หนังสือ:');
    categories.forEach((category, bookSet) {
      print('  $category: ${bookSet.length} เล่ม');
    });
    
    // Top borrowers
    List<Member> activeBorrowers = members.values
        .where((member) => member.borrowedBooks.isNotEmpty)
        .toList();
    activeBorrowers.sort((a, b) => b.borrowedBooks.length.compareTo(a.borrowedBooks.length));
    
    if (activeBorrowers.isNotEmpty) {
      print('\nสมาชิกที่ยืมหนังสือมากที่สุด:');
      for (int i = 0; i < activeBorrowers.length && i < 3; i++) {
        Member member = activeBorrowers[i];
        print('  ${i + 1}. ${member.name}: ${member.borrowedBooks.length} เล่ม');
      }
    }
  }
  
  void showAllBooks() {
    print('\n📚 === รายการหนังสือทั้งหมด ===');
    if (books.isEmpty) {
      print('ไม่มีหนังสือในระบบ');
      return;
    }
    
    categories.forEach((category, bookSet) {
      if (bookSet.isNotEmpty) {
        print('\n--- หมวด: $category ---');
        bookSet.forEach((isbn) {
          Book book = books[isbn]!;
          print('${book.isAvailable ? '📗' : '📕'} ${book.title} - ${book.author}');
        });
      }
    });
  }
}

// ตัวอย่างการใช้งาน
void main() {
  // สร้างห้องสมุด
  Library library = Library('ห้องสมุดกลาง มหาวิทยาลัย ABC');
  
  // เพิ่มหนังสือ
  library.addBook(Book(
    isbn: '978-0-123456-78-9',
    title: 'การเขียนโปรแกรม Dart',
    author: 'สมชาย โค้ดดี',
    category: 'เทคโนโลยี',
    publishYear: 2023,
  ));
  
  library.addBook(Book(
    isbn: '978-0-234567-89-0',
    title: 'Flutter สำหรับผู้เริ่มต้น',
    author: 'สมหญิง แอปป์เก่ง',
    category: 'เทคโนโลยี',
    publishYear: 2023,
  ));
  
  library.addBook(Book(
    isbn: '978-0-345678-90-1',
    title: 'ประวัติศาสตร์ไทย',
    author: 'ศ.ดร.สมศักดิ์ รู้เก่า',
    category: 'ประวัติศาสตร์',
    publishYear: 2022,
  ));
  
  library.addBook(Book(
    isbn: '978-0-456789-01-2',
    title: 'คณิตศาสตร์ขั้นสูง',
    author: 'อ.สมใจ คำนวณ',
    category: 'คณิตศาสตร์',
    publishYear: 2023,
  ));
  
  library.addBook(Book(
    isbn: '978-0-567890-12-3',
    title: 'วรรณกรรมไทยร่วมสมัย',
    author: 'คุณหญิงสมศรี เขียนดี',
    category: 'วรรณกรรม',
    publishYear: 2022,
  ));
  
  // เพิ่มสมาชิก
  library.addMember(Member(
    memberId: 'M001',
    name: 'นายอภิชาติ รักการอ่าน',
    email: 'apichart@email.com',
  ));
  
  library.addMember(Member(
    memberId: 'M002',
    name: 'นางสาวพิมพ์ใจ หนังสือดี',
    email: 'pimjai@email.com',
  ));
  
  library.addMember(Member(
    memberId: 'M003',
    name: 'นายกิตติ์ ใฝ่รู้',
    email: 'kit@email.com',
  ));
  
  print('\n=== ทดสอบระบบห้องสมุด ===');
  
  // ยืมหนังสือ
  library.borrowBook('M001', '978-0-123456-78-9');
  library.borrowBook('M001', '978-0-234567-89-0');
  library.borrowBook('M002', '978-0-345678-90-1');
  library.borrowBook('M003', '978-0-456789-01-2');
  
  // ลองยืมหนังสือที่ถูกยืมแล้ว
  library.borrowBook('M002', '978-0-123456-78-9');
  
  // ค้นหาหนังสือ
  print('\n🔍 === การค้นหาหนังสือ ===');
  
  List<Book> dartBooks = library.searchByTitle('Dart');
  print('หนังสือที่มีคำว่า "Dart":');
  dartBooks.forEach((book) => print('  - ${book.title}'));
  
  List<Book> techBooks = library.getBooksByCategory('เทคโนโลยี');
  print('\nหนังสือหมวดเทคโนโลยี:');
  techBooks.forEach((book) => print('  - ${book.title}'));
  
  // แสดงสถิติ
  library.showLibraryStats();
  
  // แสดงหนังสือทั้งหมด
  library.showAllBooks();
  
  // คืนหนังสือ
  print('\n📚 === การคืนหนังสือ ===');
  library.returnBook('M001', '978-0-123456-78-9');
  library.returnBook('M002', '978-0-345678-90-1');
  
  // แสดงสถิติหลังคืนหนังสือ
  library.showLibraryStats();
}
```

---

## 📝 แบบฝึกหัดท้ายสัปดาห์

### โปรเจคท้าทาย: ระบบจัดการร้านอาหาร

```dart
// TODO: นักเรียนสร้างระบบจัดการร้านอาหารที่ประกอบด้วย

// 1. Class MenuItems
class MenuItem {
  String id;
  String name;
  double price;
  String category;
  List<String> ingredients;
  bool isAvailable;
  
  // TODO: สร้าง Constructor และ Methods
}

// 2. Class Customer
class Customer {
  String customerId;
  String name;
  String phoneNumber;
  List<String> orderHistory;
  
  // TODO: สร้าง Constructor และ Methods
}

// 3. Class Order
class Order {
  String orderId;
  String customerId;
  Map<String, int> items;  // MenuItemId -> Quantity
  DateTime orderTime;
  String status;  // 'pending', 'cooking', 'ready', 'delivered'
  double totalPrice;
  
  // TODO: สร้าง Constructor และ Methods
}

// 4. Class Restaurant
class Restaurant {
  String name;
  Map<String, MenuItem> menu;
  Map<String, Customer> customers;
  Map<String, Order> orders;
  Map<String, List<String>> categories;
  
  // TODO: สร้าง Methods สำหรับ:
  // - เพิ่ม/ลบ/แก้ไขเมนู
  // - จัดการลูกค้า
  // - สั่งอาหาร
  // - อัพเดทสถานะออเดอร์
  // - คำนวณยอดขาย
  // - ค้นหาเมนู
  // - แสดงรายงาน
}

// ตัวอย่างการใช้งาน
void main() {
  Restaurant restaurant = Restaurant('ร้านอาหารดีเลิศ');
  
  // เพิ่มเมนู
  restaurant.addMenuItem(MenuItem(
    id: 'F001',
    name: 'ผัดไทย',
    price: 60.0,
    category: 'อาหารจานเดียว',
    ingredients: ['เส้นหมี่', 'กุ้ง', 'ไข่', 'ถั่วงอก'],
    isAvailable: true,
  ));
  
  // เพิ่มลูกค้า
  restaurant.addCustomer(Customer(
    customerId: 'C001',
    name: 'คุณสมชาย',
    phoneNumber: '081-234-5678',
  ));
  
  // สั่งอาหาร
  restaurant.createOrder('C001', {
    'F001': 2,  // ผัดไทย 2 จาน
  });
  
  // แสดงรายงาน
  restaurant.showDailySummary();
}
```

---

## 🧪 แบบทดสอบสัปดาห์ที่ 2

### ข้อ 1: Multiple Choice (20 คะแนน)

**1.1** ข้อใดคือการประกาศ Class ที่ถูกต้อง?
```dart
a) class Student { String name; }
b) Class Student { String name; }
c) student class { String name; }
d) class student { String name; }
```
**คำตอบ: a**

**1.2** การสืบทอด (Inheritance) ใน Dart ใช้คำสั่งใด?
- a) inherits
- b) extends ✅
- c) implements  
- d) with

**1.3** Set แตกต่างจาก List อย่างไร?
- a) Set เก็บข้อมูลได้มากกว่า
- b) Set ไม่สามารถลบข้อมูลได้
- c) Set ไม่อนุญาตให้มีข้อมูลซ้ำ ✅
- d) Set ช้ากว่า List เสมอ

**1.4** ผลลัพธ์ของโค้ดนี้คือ?
```dart
List<int> numbers = [1, 2, 3, 4, 5];
List<int> doubled = numbers.map((n) => n * 2).toList();
print(doubled);
```
- a) [1, 2, 3, 4, 5]
- b) [2, 4, 6, 8, 10] ✅
- c) [1, 4, 9, 16, 25]
- d) Error

### ข้อ 2: เขียนโค้ด (30 คะแนน)

**2.1** สร้าง Class BankAccount ที่มี properties และ methods ตามที่กำหนด
```dart
// TODO: เขียน Class BankAccount
// Properties: accountNumber, ownerName, balance
// Methods: deposit(), withdraw(), getBalance(), transfer()
// Constructor: รับ accountNumber, ownerName, balance เริ่มต้น

// ตัวอย่างการใช้งาน:
// BankAccount account = BankAccount('001', 'สมชาย', 1000);
// account.deposit(500);
// account.withdraw(200);
// print(account.getBalance()); // ควรแสดง 1300
```

**2.2** เขียนโค้ดจัดการรายการนักเรียนด้วย Map และ List
```dart
void main() {
  // TODO: สร้าง Map เก็บข้อมูลนักเรียน
  // Key: รหัสนักเรียน, Value: ชื่อนักเรียน
  
  // TODO: สร้าง Map เก็บคะแนนของแต่ละคน
  // Key: รหัสนักเรียน, Value: List ของคะแนน
  
  // TODO: เขียน Functions:
  // 1. addStudent() - เพิ่มนักเรียนใหม่
  // 2. addGrade() - เพิ่มคะแนนให้นักเรียน
  // 3. calculateAverage() - คำนวณคะแนนเฉลี่ยของนักเรียน
  // 4. getTopStudent() - หานักเรียนที่มีคะแนนเฉลี่ยสูงสุด
  // 5. showAllStudents() - แสดงข้อมูลนักเรียนทั้งหมด
}
```

---

## 🔧 เครื่องมือและ Tips

### 1. OOP Best Practices

```dart
// ✅ ดี - ใช้ Encapsulation
class BankAccount {
  String _accountNumber;  // private
  double _balance;        // private
  
  BankAccount(this._accountNumber, this._balance);
  
  double get balance => _balance;  // getter
  
  bool deposit(double amount) {
    if (amount > 0) {
      _balance += amount;
      return true;
    }
    return false;
  }
}

// ❌ ไม่ดี - Properties เป็น public ทั้งหมด
class BadBankAccount {
  String accountNumber;
  double balance;
  
  BadBankAccount(this.accountNumber, this.balance);
}
```

### 2. Collections Performance Tips

```dart
// ✅ ดี - ใช้ Set สำหรับการตรวจสอบ membership
Set<String> validIds = {'A001', 'A002', 'A003'};
bool isValid = validIds.contains('A001');  // O(1)

// ❌ ช้า - ใช้ List สำหรับการตรวจสอบ
List<String> validIdsList = ['A001', 'A002', 'A003'];
bool isValidSlow = validIdsList.contains('A001');  // O(n)

// ✅ ดี - ใช้ Map สำหรับ lookup
Map<String, String> studentNames = {'S001': 'สมชาย', 'S002': 'สมหญิง'};
String? name = studentNames['S001'];  // O(1)

// ❌ ช้า - วนหา List
List<Student> students = [/* ... */];
Student? found = students.firstWhere((s) => s.id == 'S001');  // O(n)
```

### 3. Common Mistakes

```dart
// ❌ ผิด - ลืม override toString()
class Student {
  String name;
  Student(this.name);
}
print(Student('สมชาย'));  // Instance of 'Student'

// ✅ ถูก - override toString()
class Student {
  String name;
  Student(this.name);
  
  @override
  String toString() => 'Student: $name';
}
print(Student('สมชาย'));  // Student: สมชาย

// ❌ ผิด - ไม่เช็ค null
Map<String, int> grades = {'A': 85};
int grade = grades['B']!;  // Runtime error!

// ✅ ถูก - เช็ค null
int? grade = grades['B'];
if (grade != null) {
  print('Grade: $grade');
} else {
  print('No grade found');
}
```

---

## 📋 Checklist สำหรับอาจารย์

### การเตรียมการสอน
- [ ] เตรียมตัวอย่าง Class ที่เกี่ยวข้องกับชีวิตจริง
- [ ] สร้างแบบฝึกหัดที่มีความยากง่ายหลากหลาย
- [ ] เตรียม Live coding examples
- [ ] ทบทวนแนวคิด OOP ให้แจ่มแจ้ง

### ระหว่างการสอน
- [ ] ให้นักเรียนลอง inherit Class ที่เขียนเอง
- [ ] สาธิตการใช้ Collections ในสถานการณ์จริง
- [ ] เปรียบเทียบ List, Set, Map ให้เห็นความแตกต่าง
- [ ] ใช้ Debugging เพื่อแสดง Object ใน memory

### การประเมินผล
- [ ] ตรวจสอบการใช้ OOP principles ที่ถูกต้อง
- [ ] ดูการเลือกใช้ Collections ที่เหมาะสม
- [ ] ประเมินการออกแบบ Class hierarchy
- [ ] ให้คะแนนการเขียนโค้ดที่สะอาดและอ่านง่าย

---

## 🎯 สรุปสัปดาห์ที่ 2

### สิ่งที่นักเรียนควรทำได้หลังเรียนจบ:

✅ **Object-Oriented Programming:**
- สร้างและใช้งาน Classes และ Objects
- เข้าใจ Inheritance และ Polymorphism
- ใช้ Abstract Classes และ Mixins ได้

✅ **Collections:**
- เลือกใช้ List, Set, Map ตามความเหมาะสม
- ใช้ Methods ของ Collections เพื่อจัดการข้อมูล
- จัดการข้อมูลที่ซับซ้อนด้วย Nested Collections

✅ **Problem Solving:**
- แก้ปัญหาด้วยการออกแบบ Classes
- จัดการข้อมูลแบบมีประสิทธิภาพ
- สร้างโปรแกรมที่มีโครงสร้างที่ดี

### เตรียมพร้อมสำหรับสัปดาห์ที่ 3:
- ทบทวน OOP concepts
- ฝึกใช้ Collections ให้คล่อง
- เรียนรู้เรื่อง null safety
- เตรียมใจสำหรับ Flutter!