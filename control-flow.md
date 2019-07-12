---
type: doc
layout: reference
category: "Syntax"
title: "Управляючі інструкції"
url: https://kotlinlang.ru/docs/reference/control-flow.html
---

# Управляючі інструкції

<a name="if-expression"></a>

## Умовний вираз <b class="keyword">if</b>

В мові Kotlin ключове слово <b class="keyword">if</b> є виразом, тобто воно повертає значення.
Це дозволяє відмовитись від тернарного оператора (умова ? умова істинна : умова хибна), оскільки виразу <b class="keyword">if</b> цілком під силу його замінити.

``` kotlin
// звичайне використання 
var max = a 
if (a < b) 
  max = b 
 
// з блоком else 
var max: Int
if (a > b) 
  max = a 
else 
  max = b 
 
// у вигляді виразу 
val max = if (a > b) a else b
```

"Гілки" виразу <b class="keyword">if</b> можуть містити декілька рядків коду, при цьому останній вираз є значенням блоку:

``` kotlin
val max = if (a > b) { 
    print("повертаєм a") 
    a 
  } 
  else { 
    print("повертаєм b") 
    b 
  }
```

Якщо ви використовуєте конструкцію <b class="keyword">if</b> як вираз (наприклад, повертаючи його значення або присвоюючи його змінній), то використання гілки `else` є обов'язковим.

Див. [використання <b class="keyword">if</b>](grammar.html#if).

<a name="when-expression"></a>

## Умовний вираз <b class="keyword">when</b>

Ключове слово <b class="keyword">when</b> призначене замінити оператор switch, присутній в C-подібних мовах. В найпростішому вигляді його використання виглядає так:

``` kotlin
when (x) {
  1 -> print("x == 1")
  2 -> print("x == 2")
  else -> { // зверніть увагу на блок
    print("x is neither 1 nor 2")
  }
}
```

Вираз <b class="keyword">when</b> послідовно порівнює аргумент зі всіма вказаними значеннями до задоволення однієї з умов.
<b class="keyword">when</b> можна використовувати і як вираз, і як оператор. При використання у вигляді виразу, значення гілки, ущо задовольняє умову, стає значенням всього виразу. При використанні у вигляді оператора значення окремих гілок відкидаються. (В точності як <b class="keyword">if</b>: кожна гілка може бути блоком і її значенням є значення останнього виразу блоку.)

Значення гілки <b class="keyword">else</b> обчислюється в тому випадку, коли жодна з умов в інших гілках не задоволена.
Якщо <b class="keyword">when</b> використовується як вираз, то гілка <b class="keyword">else</b> є обов'язковою, за виключенням випадків, в яких компілятор може пересвідчитись, що гілки покривають всі можливі значення. 


Якщо для декількох значень виконується одна й та сама дія, то умови можна перераховувати в одній гілці через кому:

``` kotlin
when (x) {
  0, 1 -> print("x == 0 or x == 1")
  else -> print("otherwise")
}
```

Крім констант, в гілках можна використовувати довільні вирази:

``` kotlin
when (x) {
  parseInt(s) -> print("s encodes x")
  else -> print("s does not encode x")
}
```

Також можна перевірити входження аргумента в [інтервал](ranges.html) <b class="keyword">in</b> або <b class="keyword">!in</b> чи його наявність в колекції:

``` kotlin
when (x) {
  in 1..10 -> print("x is in the range")
  in validNumbers -> print("x is valid")
  !in 10..20 -> print("x is outside the range")
  else -> print("none of the above")
}
```

Крім цього Кotlin дозволяє за допомогою <b class="keyword">is</b> або <b class="keyword">!is</b> перевірити тип аргумента. Зверніть увагу, що завдяки [розумним приведенням](typecasts.html#smart-casts) ви можете отримати доступ к методам та властивостям типу без додаткової перевірки:

```kotlin
val hasPrefix = when(x) {
  is String -> x.startsWith("prefix")
  else -> false
}
```

<b class="keyword">when</b> зручно використовувати замість ланцюжка умов виду <b class="keyword">if</b>-<b class="keyword">else</b> <b class="keyword">if</b>. При відсутності аргумента, умови працюють як прості логічні вирази, а тіло гілки виконується при його істинності:

``` kotlin
when {
  x.isOdd() -> print("x is odd")
  x.isEven() -> print("x is even")
  else -> print("x is funny")
}
```

Див. [використання <b class="keyword">when</b>](grammar.html#when).

<a name="for-loops"></a>

## Цикли <b class="keyword">for</b>

Цикл <b class="keyword">for</b> забезпечує перебір всіх значень, які поставляються ітератором. Для цього використовується такий синтаксис:

``` kotlin
for (item in collection)
  print(item)
```

Тілом циклу може бути блок коду:

``` kotlin
for (item: Int in ints) {
  // ...
}
```

Як відмічалось вище, цикл <b class="keyword">for</b> дозволяє проходити по всіх елементах об'єкту, що має ітератор, наприклад:

* який володіє внутрішнею або зовнішнею функцієй `iterator()`, такою, що її тип, що повертається
  * має внутрішню або зовнішню функцію `next()`, та
  * має внутрішню або зовнішню функцію `hasNext()`, що повертає `Boolean`.

Всі три вказані функції повинні бути оголошені як `operator`.

Якщо при проході по масиву або списку необхідний порядковий номер елементу, використовуйте такий підхід:

``` kotlin
for (i in array.indices)
  print(array[i])
```

Зверніть увагу, що така "ітерація по ряду" компілюється в більш продуктивний код без створення додаткових об'єктів.

Також ви можете використовувати бібліотечну функцію `withIndex`:

``` kotlin
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```

Див. [використання <b class="keyword">for</b>](grammar.html#for).

<a name="while-loops"></a>

## Цикли <b class="keyword">while</b>

Ключові слова <b class="keyword">while</b> та <b class="keyword">do</b>..<b class="keyword">while</b> працюють як і в інших мовах:

``` kotlin
while (x > 0) {
  x--
}

do {
  val y = retrieveData()
} while (y != null) // y is visible here!
```

Див. [використання <b class="keyword">while</b>](grammar.html#while).

## Ключові слова <b class="keyword">break</b> та <b class="keyword">continue</b> в циклах

Kotlin підтримує звичні оператори <b class="keyword">break</b> та <b class="keyword">continue</b> в циклах. Див. [Оператори переходу](returns.html).


