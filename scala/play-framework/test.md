---
description: Test on Play framework with ScalaTest
---

# Test

{% embed url="https://www.scalatest.org/user\_guide/selecting\_a\_style" %}

{% embed url="https://www.playframework.com/documentation/2.8.x/ScalaTestingWithScalaTest" %}

{% embed url="https://qiita.com/verdoyant/items/e8d81e80268b714fdfbc" %}

{% embed url="https://scala-text.github.io/scala\_text/test.html" %}



## Test command

### To run all tests

```text
sbt run
```

### To run only one test class

```scala
test-only 

// followed by the name of the class, i.e., 
test-only my.namespace.MySpec.
```

### To run only the tests that have failed

```text
test-quick
```

### To run tests continually

```scala
~test-quick
// run a command with a tilde in front, i.e. .
```

### To access test helpers such as `FakeRequest` in console

```scala
test:console
```

## Testing Style

### FunSuite

{% code title="SetSuite.scala" %}
```scala
import org.scalatest._
import org.scalatestplus.play._  

class SetSuite extends FunSuite {

  test("An empty Set should have size 0") {
    assert(Set.empty.size == 0)
  }

  test("Invoking head on an empty Set should produce NoSuchElementException") {
    assertThrows[NoSuchElementException] {
      Set.empty.head
    }
  }
  
}
```
{% endcode %}

###  PlaySpec, WordSpec, FlatSpec

一つのメソッドの別の条件でのテストが分かりやすく書ける  
WordSpecで使える機能はPlaySpecでも使える\(PlaySpecはWordSpecをextendsしたもの\)

```scala
class SetSuite extends PlaySpec {

  "An empty Set should have size 0" in {
    assert(Hoge.hode() == 1)
  }
  
  "An empty Set should have size 0" in {
    Set.empty.size mustBe 0
  }
  
  "Invoking head on an empty Set should produce NoSuchElementException"  in {
    a [NoSuchElementException] must be thrownBy {
      Set.empty.head
    }
  }
}
```

{% hint style="info" %}
You can alternatively [define your own base classes](http://scalatest.org/user_guide/defining_base_classes) instead of using **`PlaySpec`**.

You should define your own test classes If you have multiple varieties of tests \(ex: normal QA test and Performance test\)

If you only do normal QA, no need to define your own test classes and use PlaySpec
{% endhint %}

## Mockito

インスタンスを作成しなくても、偽装したインスタンスを作成して引数で渡すことでテストの依存する部分が減らせる。

データベースやAPIに依存するものをテストする際に有効で、mockによってAPIやデータベースがちゃんと動いているかという懸念点をなくし、純粋な機能テストが出来る。

{% code title="Controller.scala" %}
```scala
package main

case class Data(date:java.util.Date)

class Controller(data:Data) {
   def getMessage(message:String):String = message+"Controlled"
}
```
{% endcode %}

{% code title="ControllerSpec.scala" %}
```scala
import org.scalatest._
import org.scalatestplus.play._
import org.specs2.mock.Mockito
import main._

class ControllerSpec extends PlaySpec with Mockito {
  "Controllerのテスト" in {
    val expected = "Test"
    val controller = new Controller(mock[Data]) {　
    // mock[Data]によってテストケースに関係ないものを無視できる。

      override def getMessage(message:String): String = message + "Controlled"
    }
    controller.getMessage(expected) must include (expected)
  }
}
```
{% endcode %}

