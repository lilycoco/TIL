# Pattern of Functions

## Closure

```text
function createCounter() {
  let count = 0
  const plus = () => {
    count+=1
  }
  return {
    plus,
    getCount() {return count}
  }
}

const counter = createCounter()

counter.plus()
counter.plus()
counter.plus()
counter.getCount()
```

## Prototype / Class

_`new` is required to declare it_

```text
function Counter() {
  this.count = 0
}

Counter.prototype.plus = function () {
  this.count += 1
}

Counter.prototype.getCount = function () {
  return this.count
}

// class Counter {
//   plus () {
//     this.count += 1
//   }
//   getCount () {
//     return this.count
//   }
// }

const counter = new Counter()
const counter2 = new Counter()

counter.plus()
counter.plus()
counter.plus()
console.log(counter.getCount())
console.log(counter2.getCount())
```

## Order rule

_functions are not care about the call order but arrow function should be called after it is declared_

```text
function createEggFry() {
  const egg = getEgg()

  return fry(egg)
}

function getEgg() {
...
}
```

```text
const getEgg = () => {
...
}

function createEggFry() {
  const egg = getEgg()

  return fry(egg)
}
```

## references

### Javascript Classes v. Closures

[https://medium.com/engineering-livestream/javascript-classes-vs-closures-cf6d6c1473f](https://medium.com/engineering-livestream/javascript-classes-vs-closures-cf6d6c1473f)

