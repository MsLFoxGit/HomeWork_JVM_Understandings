# Task 1
```java
public class JvmComprehension {
    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }
    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}
```
#### Не забудьте упомянуть про:
```
* ClassLoader'ы
* области памяти (стэк (и его фреймы), хип, метаспейс)
* сборщик мусора
```

**Классы `JvmComprehension`, `Object, Integer, System`  загружаются в ClassLoader.**

**Далее:** 
Bootstrap ClassLoader грузит в `MetaSpace` классы `Object, Imteger, System`
Application ClassLoader  грузит в `MetaSpace` класс `JvmComprehension`. Вместе с этим загружаются статистические поля и методы классов.

**Далее:** запускается метод `main` и в `Stack` создается фрейм метода `main` 

1. В `Stack` во фрейм `main` записывается `int i = 1` 
2. В `heap` записывается объект `Object o`, в `Stack` во фрейм `main` записвается ссылка на `o`
3. В `heap` записывается объект `Integer ii = 2`, в `Stack` во фрейм `main` записвается ссылка на `ii`

**Далее:** запускается метод `printAll` и в `Stack` создается фрейм метода `printAll`. 

4. В `Stack` во фрейм `printAll` записывается ссылка на `o`, записывается `int i = передаваемому значению`, записывается ссылка на `ii`  
5. В `heap` записывается объект `Integer uselessVar = 700`, в `Stack` во фрейм `printAll` записвается ссылка на `uselessVar`
6. **Далее:** запускается метод `out.println` и в `Stack` создается фрейм метода `out.println`. 

>**Далее:** запускается метод `toString()` и в `Stack` создается фрейм метода `toString()`. 

>> В `heap` записывается объект `String = o.toString()`. В `Stack` во фрейм `out.println` записывается ссылка на `String = o.toString()`

>> Далле удалется исполненый `toString()` фреймы со ссылками, 

>> Далее тоже самое делается для объектов  `ii` и `i` (через внутреннюю реализацию )

>  Далее сложение строк `heap` приводит к созданию нового объекта в  `String ` в `heap`  ссылка на который сохраняется в стеке во фрейме 'out.println'

>  `String` выводится в консоль

>  Далее удалеятся фрейм `out.println` со всемии ссылками `String`. впоследствии удаляется объект `String`, `uselessVar` из `heap`

>  Далее удалеятся фрейм метода `printAll` со всемии ссылками  `String`, `uselessVar`. впоследствии удаляется объект `String`,  `uselessVar` из `heap`


7. **Далее:** запускается метод `out.println` и в `Stack` создается фрейм метода `out.println`

> В `heap` записывается объект `String = "finished"`. В `Stack` во фрейм `out.println` записывается ссылка на `String = "finished"`

> `String = "finished"` выводится в консоль

> удалеятся фрейм `out.println` . впоследствии удаляется объект `String = "finished"` из `heap`

**Далее:** удалеятся фрейм метода `main` 

** Память очищается ВСЯ. `heap` `MetaSpace`, 

