# Observables

## 用途

a 为一个数组，b 为与 a 有映射关系的数组。特殊需求是：当 a 改变（增/删/改）时，b 中对应的值也要相应改变。

## 实例

```javascript
// 1. 合一方式
// producer:
// a为Subject,表示a既是Observable，又是Observer
var a = new Rx.Subject();

setInterval(function everySecond() {
  a.next(Math.random());
}, 1000);

// **************************
// consumer:
var b = a.map(function double(v) {
  return v * 2; // 由于a是Subject。当它改变时，a会执行map，b也就得到了更新。
});

b.subscribe(function onValue(v) {
  console.log(v);
});

// 2. 分离方式
// a是Observable，observer为Observer
var a = Rx.Observable.create(function onObserve(observer) {
  setInterval(function everySecond() {
    observer.next(Math.random());
  }, 1000);
});
```
