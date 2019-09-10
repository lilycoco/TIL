# Pattern of Functions

## Closure

```
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

## Prototype
```
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

```
function createEggFry() {
  const egg = getEgg()

  return fry(egg)
}

function getEgg() {
...
}

```
