---
up: [[Исключения]]
---
# try-with-resources
---
### Что такое механизм try-with-resources?
Дает возможность объявлять один или несколько ресурсов в блоке try, которые будут закрыты автоматически без использования finally блока.  
В качестве ресурса можно использовать любой объект, класс которого реализует интерфейс `java.lang.AutoCloseable` или `java.io.Closeable`.

### Какие интерфейсы должен имплементировать объект в try с ресурсами. Их отличия?
В качестве ресурса можно использовать любой объект, класс которого реализует интерфейс java.lang.AutoCloseable или java.io.Closable.

До Java 7 уже существовал похожий интерфейс – `Closeable`. Смысл его точно такой же. Он всё еще доступен в стандартной библиотеке для обратной совместимости, но в новом коде рекомендуется использовать `AutoCloseable`. Чтобы экземпляры старого `Closeable` тоже можно было использовать в try-with-resource, _новый интерфейс был добавлен его родителем_.  
  
Проблема старого интерфейса `Closeable` была в узости типа исключений, которые может выбрасывать `close()`. Ковариантность позволила расширить тип в базовом интерфейсе `AutoCloseable` с `IOException` до `Exception`.  
  
Еще одно отличие – контракт метода `close()`. Старый `Closeable` требует его [идемпотентности](https://ru.wikipedia.org/wiki/%D0%98%D0%B4%D0%B5%D0%BC%D0%BF%D0%BE%D1%82%D0%B5%D0%BD%D1%82%D0%BD%D0%BE%D1%81%D1%82%D1%8C), тогда как новый `AutoCloseable` разрешает методу иметь побочные эффекты.

### Что такое ресурс в конструкции try-with-resources?
Любой объект, класс которого реализует интерфейс java.lang.AutoCloseable или java.io.Closable

### Когда происходит закрытие ресурса в конструкции try-with-resources если в try возникло исключение: до перехода в catch или после того как catch отработает?
До перехода в catch

### Что произойдет если исключение будет выброшено из блока catch после чего другое исключение будет выброшено из метода close() при использовании try-with-resources?
В try-with-resources добавленна возможность хранения "подавленных" исключений, и брошенное try-блоком исключение имеет больший приоритет, чем исключения получившиеся во время закрытия.