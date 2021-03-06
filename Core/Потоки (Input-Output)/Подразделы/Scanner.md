---
up: [[Потоки (Input-Output)]]
---
# Scanner
### Отличие Scanner’a от BufferedReader’a
* **Scanner** предназначен для разбора любого текста и, ввода с терминала, как частный случай. Удобен для простого разбора текстов и для всяких учебных задачек типа "введите число от нуля до трёх".
* **BufferedReader** нужен, чтобы буфферизовать чтение с любого потока. Как частный случай, можно буфферизовать ввод с терминала. Иногда используется ради метода readLine, когда сложно обрабатывать текст блоками. Для целей чтения текста из стандартного вводы или ручного ввода пользователя вполне пригодный вариант. Но при таком подходе придётся парсить числа вручную, что не слишком большая сложность.

### Есть ли у сканера буфер
Да.

Из кода java.util.Scanner :
```java
// Internal buffer used to hold input
private CharBuffer buf;

// Size of internal character buffer
private static final int BUFFER_SIZE = 1024; // change to 1024;
```

### Что такое токен в Scanner
Токен это данные между разделителями.
.useDelimiter - позволяет самому указать что будет являтся разделителем