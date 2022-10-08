# Домашнее задание по теме «JVM. Организация памяти, сборщики мусора, VisualVM»

## Процессы JVM в классе JvmComprehension 

### public class JvmComprehension {

### public static void main(String[] args) {
###         int i = 1;                      // 1
###         Object o = new Object();        // 2
###         Integer ii = 2;                 // 3
###         printAll(o, i, ii);             // 4
###         System.out.println("finished"); // 7
###     }

###     private static void printAll(Object o, int i, Integer ii) {
###         Integer uselessVar = 700;                   // 5
###         System.out.println(o.toString() + i + ii);  // 6
###     }
### }

#### public static void main(String[] args) { - подгружаются класс JvmComprehension и System.out.println() с помощью ClassLoading 
#### затем работает подсистема загрузчиков классов Aplication-->Platform-->Bootstrap класса ClassLoader
#### далее происходит связывание Linking в три этапа: Verify (проверка) --> Prepare (подготовка) --> Resolve (разрешение символьных ссылок)
#### инициализация Initialozation
#### выполняются инициализаторы static printAll()
#### В момент вызова метода main создаётся фрейм (кадр) в стеке Stack JvmComprehension, heap (куча) пустая, Metaspace JvmComprehension.class system classes
#### //1 в стеке Stack JvmComprehension добавляются int i = 1
#### //2 в heap добавляется Object, о --> Stack
#### //3 в стеке Stack --> Integer ii = 2
#### //4 в стеке Stack --> printAll(), o связывается с кучей, i связывается с кучей, ii связывается с кучей
#### //5 в стеке Stack --> uselessVar = 700 связывается с кучей Integer
#### //6 создается новый фрейм в стеке Stack, куда передается ссылка на o.toString(), i, ii
#### //7 создается новый фрейм в стеке Stack, куда передается "finished"
#### далее в памяти происходит множество процессов, которые должны правильно выполняться, размер сущностей которых мы можем отрегулировать
#### код выполняется построчно, методы компилируются в машинный код с помощью движка выполнения, движок состоит из интерпритатора, JIT компилятора и сборщика мусора
#### Garbage Collection периодически собирает объекты из heap (куча), которые больше не используются     
