---
up: [[Класс Object]]
---
# Equals и HashCode
---
### Расскажите про equals и hashcode
**Хеш-код** — это целочисленный результат работы метода, которому в качестве входного параметра передан объект.  
Если более точно, то это битовая строка фиксированной длины, полученная из массива произвольной длины.  
  
**Equals** - это метод, определенный в Object, который служит для сравнения объектов. При сравнении объектов при помощи == идет сравнение по ссылкам. При сравнении по equals() идет сравнение по состояниям объектов.  

При переопределении equals() обязательно нужно переопределить метод hashCode(). Равные объекты должны возвращать одинаковые хэш коды.

### Для чего нужен метод hashCode()?
hachCode() вычисляет целочисленное значение для конкретного элемента класса, чтобы использовать его для быстрого поиска и доступа к этому элементу в hash-структурах данных, например, HashMap, HashSet и прочих.

### Зачем нужен equals(). Чем он отличается от операции \==?
equals() - сравнение по состоянию, == - по ссылкам

### Контракты equals
Контракты equals():  
* **Симметричность**: Для двух ссылок, a и b, `a.equals(b)` тогда и только тогда, когда `b.equals(a)`
* **Рефлексивность**: для любого заданного значения x, выражение x.equals(x) должно возвращать true. Заданного — имеется в виду такого, что `x != null`
* **Постоянство**: повторный вызов метода `equals()` должен возвращать одно и тоже значение до тех пор, пока какое-либо значение свойств объекта не будет изменено.  
* **Транзитивность**: Если `a.equals(b)` и `b.equals(c)`, то тогда `a.equals(c)`
* **Сравнение с null**: Для каждого экземпляра x `x.equals(null)` должно возвращать false
* **Совместимость с hashCode()**: Два тождественно равных объекта должны иметь одно и то же значение hashCode()  

### Контракты hashCode
* **Консистентность**: Для каждого экземпляра x повторное выполнение **x.hashCode()** должно возвращать одинаковый хэш
* **Схожие объекты имеют одинаковый хэш**: Если два экземпляра схожи друг с другом, то вызов **hashCode()** у каждого из них должен возвращать одинаковый результат

### Какой контракт между hashCode() и equals()?
* Если два объекта возвращают разные значения hashcode(), то они не могут быть равны  
*  Если equals объектов true, то и хэшкоды должны быть равны.   
* Переопределив equals, всегда переопределять и hashcode.

### Какой тип данных у hashcode? Может ли быть hashcode отрицательным?
`hashCode()` возвращает тип `int` , и поскольку `int` может принимать отрицательные значения то и hashCode может быть отрицательным

### Каким образом реализованы методы hashCode() и equals() в классе Object?
Реализация метода Object.equals() сводится к проверке на равенство двух ссылок:  
```java
public boolean equals(Object obj) {  
	return (this == obj);  
}  
```  
  
HashCode реализован таким образом, что для одного и того же входного объекта, хеш-код всегда будет одинаковым.  
Реализация метода `Object.hashCode()` описана как native, т.е. написана не на Java. Непереопределенный hashCode возвращает идентификационный хеш, основанный на состоянии потока, объединённого с xorshift (в OpenJDK8). А вообще, функция предлагает шесть методов на базе значения переменной hashCode.  
  
* Случайно сгенерированное число.  
* Функция адреса объекта в памяти.  
* Жёстко запрограммированное значение 1 (используется при тестировании на чувствительность (sensitivity testing)).  
* Последовательность.  
* Адрес объекта в памяти, приведённый к целочисленному значению.  
* Состояние потока, объединённое с xorshift ([Подробнее](https://en.wikipedia.org/wiki/Xorshift))

### Что будет, если переопределить equals() не переопределяя hashCode()? Какие могут возникнуть проблемы?
Нарушится контракт. Классы и методы, которые использовали правила этого контракта могут некорректно работать. Так для объекта HashMap это может привести к тому, что пара, которая была помещена в Map возможно не будет найдена в ней при обращении к Map, если используется новый экземпляр ключа.

### Могут ли у разных объектов быть одинаковые hashCode()?
Когда у разных объектов одинаковые хеш-коды называется — **коллизией.**

### Из-за чего происходят коллизии?
Вероятность возникновения коллизии зависит от используемого алгоритма генерации хеш-кода.

### Почему нельзя реализовать hashcode() который будет гарантированно уникальным для каждого объекта?
В Java множество возможных хэш кодов ограничено типом int, а множество объектов ничем не ограничено.  
Из-за этого, вполне возможна ситуация, что хэш коды разных объектов могут совпасть

### Есть класс Point{int x, y;}. Почему хэш-код в виде 31 * x + y предпочтительнее чем x + y?
Множитель создает зависимость значения хэш-кода от очередности обработки полей, а это дает гораздо лучшую хэш-функцию.

### Есть ли какие-либо рекомендации о том, какие поля следует использовать при подсчете hashCode()?
Выбирать поля, которые с большой долью вероятности будут различаться. Для этого необходимо использовать уникальные, лучше всего примитивные поля, например такие как id, uuid. При этом нужно следовать правилу, если поля задействованы при вычислении `hashCode()`, то они должны быть задействованы и при выполнении `equals()`.
