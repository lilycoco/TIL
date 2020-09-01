# Test

{% embed url="https://www.playframework.com/documentation/2.8.x/ScalaTestingWithScalaTest" %}

* To run all tests, run `test`.
* To run only one test class, run `test-only` followed by the name of the class, i.e., `test-only my.namespace.MySpec`.
* To run only the tests that have failed, run `test-quick`.
* To run tests continually, run a command with a tilde in front, i.e. `~test-quick`.
* To access test helpers such as `FakeRequest` in console, run `test:console`.



In _ScalaTest + Play_, you define test classes by extending the [`PlaySpec`](https://www.playframework.com/documentation/2.8.x/api/scala/org/scalatestplus/play/PlaySpec.html) trait. Here’s an example:

```text
import org.scalatestplus.play._

import scala.collection.mutable

class StackSpec extends PlaySpec {

  "A Stack" must {
    "pop values in last-in-first-out order" in {
      val stack = new mutable.Stack[Int]
      stack.push(1)
      stack.push(2)
      stack.pop() mustBe 2
      stack.pop() mustBe 1
    }
    "throw NoSuchElementException if an empty stack is popped" in {
      val emptyStack = new mutable.Stack[Int]
      a[NoSuchElementException] must be thrownBy {
        emptyStack.pop()
      }
    }
  }
}
```

You can alternatively [define your own base classes](http://scalatest.org/user_guide/defining_base_classes) instead of using `PlaySpec`.

{% hint style="info" %}
Should define your own test classes If you have multiple varieties of tests \(ex: normal QA test and Performance test\)

If you only do normal QA, no need to define your own test classes and use PlaySpec
{% endhint %}

you might create a `UnitSpec` class \(not trait, for speedier compiles\) for unit tests that looks like:

```text
package com.mycompany.myproject

import org.scalatest._
import matchers._

abstract class UnitSpec extends AnyFlatSpec with should.Matchers with
  OptionValues with Inside with Inspectors
```

You can then write unit tests for your project using the custom base class, like this:

```text
package com.mycompany.myproject

import org.scalatest._

class MySpec extends UnitSpec {
  // Your tests here
}
```

