bool topLvl = true;
void main() {
  //Область видимости
  bool insideMain = true;
  void myFunction(){
    bool insideMyFunction = true;
    print(topLvl);
    print(insideMain);
    print(insideMyFunction);
  }
  myFunction();
  
  print('Фабрики');
  Logger logger1 = Logger('log');
  Logger logger2 = Logger('log');
  Logger logger3 = Logger('log1');  
  print(logger1);
  print(logger2);
  print(logger3);
  
  print('Геттер и сеттер');
  var rect = Rectangle(10,15,20,40);
  print(rect.left);
  rect.right = 12;
  print(rect.left);
  
  print('Перечисления');
  print(Color.red.index);
  print(Color.values);
  
  //==== Переопределение операторов
  print('Переопределение операторов');
  Vector v1 = Vector (1,2); Vector v2 = Vector (3,4);
  print('(${(v1+v2).x}.${(v1+v2).y})');
  
  //==== Расширяем стандатрные классы
  print('Расширяем стандатрные классы');
  print('113'.parseInt() is int);
  print('3.22'.parseDouble() is double);
    
  //Цикл
  var arrs = [1,2,3,4,5];
  for (var x in arrs) 
    print(x);
  
  //====Округление
  var lvl = 66 ~/ 10; // (66 / 10 ).rtuncate.toInt()
   
  
  //==== Замыкание
  Function addFunction(String add){
    return (String i) => 'Cad $add - zam $i';
  }  
  var add2 = addFunction('kek');  
  print(add2('lul'));
  
  //==== Анонимная функция
  var arr = [1,2,3];  
  arr.forEach((v) => print(v));
  
  //==== Вызов функции
  var toup = (value) => '${value.toUpperCase()}';
  print(toup('raz'));
  
  //==== Именованные параметры в функции через {}
  String getModel({String title, int value}) {
    return '$title - ${value + 10}';
  }  
  print(getModel(title: 'lul', value: 1));
  
  void createListAndMap({
      List<int> list = const [1,2,3], 
      Map<String, String> games = const {
        'first': 'Gotic 2',
        'second': 'Withcer 3'
      }
  }){
    print('Список по умолчанию: $list');
    games.forEach((k,v) => print(v));
  }
  createListAndMap();  
  
  print('Определение типов');
  int sort(int a, int b) => a - b;
  print(sort is Compare<int>);
  
  print('Stream');
  streamTest();
}

//Кеширование и фабрики
class Logger {
  final String name;
  
  static final Map<String, Logger> _cache = <String, Logger> {};
  
  factory Logger(String name) {
    return _cache.putIfAbsent(name, () => Logger._internal(name));
  }
  
  Logger._internal(this.name) {
    print('Call Constructor');
  }
}

//Геттер и сеттер
class Rectangle {
  num left, top, width, height;
  
  Rectangle(this.left, this.top, this.width, this.height);
  
  num get right => left + width;
  set right(num value) => left = value - width;
  
  num get bottom => top + height;
  set bottom(num value) => top = value - height;  
}

//==== Переопределение операторов
class Vector { 
  final int x, y;

  Vector(this.x, this.y);

  Vector operator + (Vector v) => Vector (x + v.x, y + v.y); 
  Vector operator - (Vector v) => Vector (x - v.x, y - v.y);
}

//==== Расширяем стандатрные классы
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
  
  double parseDouble() {
    return double.parse(this);
  }
}

//==== Перечисления
enum Color {red, green, blue}

//==== Stream
streamTest() async {
  Duration interval = Duration(seconds: 1);
  Stream<int> stream$ = Stream<int>.periodic(interval, (x) => (x + 1) * 2);
  
  stream$ = stream$.take(5);
  
  await for(int i in stream$) {
    print('stream ${i}');
  }
}

//==== Определение типов
typedef Compare<T> = int Function(T a, T b);



             
