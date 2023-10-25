---
title: 数据类型（十一）手写Promise
date: 2023-10-20 21:57:13
tags:
- 前端面试基本功
categories:
- [前端面试]
- [前端面试题]
---

```js
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

const resolvePromise = (promise2, x, resolve, rejected) => {
  // 循环引用报错
  if (x === promise2) {
    return rejected(new TypeError("Chaining cycle detected for promise"));
  }
  // 防止多次调用
  let called = false;
  // x不是null 且x是对象或者函数
  if ((typeof x === "object" && x !== "null") || typeof x === "function") {
    try {
      let then = x.then;
      if (typeof then === "function") {
        then.call(
          x,
          (y) => {
            if (called) return;
            called = true;
            resolvePromise(promise2, y, resolve, rejected);
          },
          (r) => {
            if (called) return;
            called = true;
            rejected(r);
          }
        );
      } else {
        resolve(x);
      }
    } catch (error) {
      if (called) return;
      called = true;
      rejected(error);
    }
  } else {
    resolve(x);
  }
};

class MyPromise {
  constructor(executor) {
    this.state = PENDING;
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledCallbacks = [];
    this.onRejectedCallbacks = [];

    let resolve = (value) => {
      if (value instanceof MyPromise) {
        return value.then(resolve, reject);
      }
      if (this.state === PENDING) {
        this.state = FULFILLED;
        this.value = value;
        this.onFulfilledCallbacks.forEach((fn) => fn());
      }
    };
    let rejected = (reason) => {
      if (this.state === PENDING) {
        this.state = REJECTED;
        this.reason = reason;
        this.onRejectedCallbacks.forEach((fn) => fn());
      }
    };
    try {
      executor(resolve, rejected);
    } catch (error) {
      rejected(error);
    }
  }

  static resolve(value) {
    return new MyPromise((resolve, rejected) => {
      resolve(value);
    });
  }

  static rejected(reason) {
    return new MyPromise((resolve, rejected) => {
      rejected(reason);
    });
  }

  static all(promises) {
    if (!Array.isArray(promises)) {
      return new TypeError(
        `TypeError: ${typeof promises} ${promises} is not iterable`
      );
    }
    return new MyPromise((resolve, rejected) => {
      const result = [];
      let index = 0;
      const processValue = (value, index) => {
        result[index] = value;
        if (++index === promises.length) {
          resolve(result);
        }
      };

      for (let i = 0; i < promises.length; i++) {
        const current = result[i];
        if (current && typeof current === "function") {
          value.then((res) => {
            processValue(res, i);
          }, rejected);
        } else {
          processValue(current, i);
        }
      }
    });
  }

  static race(promises) {
    return new MyPromise((resolve, rejected) => {
      for (let i = 0; i < promises.length; i++) {
        let current = promises[i];
        if (current && typeof current === "function") {
          current.then(resolve, rejected);
        } else {
          resolve(current);
        }
      }
    });
  }

  then(onFulfilled, onRejected) {
    onFulfilled =
      typeof onFulfilled === "function" ? onFulfilled : (val) => val;
    onRejected = typeof onRejected === "function" ? onRejected : (err) => err;
    const promise2 = new MyPromise((resolve, rejected) => {
      if (this.state === FULFILLED) {
        // 需要模拟微任务队列
        setTimeout(() => {
          try {
            const x = onFulfilled(this.value);
            resolvePromise(promise2, x, resolve, rejected);
          } catch (error) {
            rejected(error);
          }
        }, 0);
      }
      if ((this.state = REJECTED)) {
        setTimeout(() => {
          try {
            const x = onRejected(this.reason);
            resolvePromise(promise2, x, resolve, rejected);
          } catch (error) {
            rejected(error);
          }
        }, 0);
      }
      if (this.state === PENDING) {
        this.onFulfilledCallbacks.push(() => {
          try {
            setTimeout(() => {
              const x = onFulfilled(this.value);
              resolvePromise(promise2, x, resolve, rejected);
            }, 0);
          } catch (error) {
            rejected(error);
          }
        });
        this.onRejectedCallbacks.push(() => {
          try {
            setTimeout(() => {
              const x = onRejected(this.reason);
              resolvePromise(promise2, x, resolve, rejected);
            }, 0);
          } catch (error) {
            rejected(error);
          }
        });
      }
    });
    return promise2;
  }

  catch(callback) {
    return this.then(null, callback);
  }

  finally(callback) {
    return this.then(
      (value) => {
        return MyPromise.resolve(callback()).then(() => value);
      },
      (error) => {
        return MyPromise.rejected(
          callback().then(() => {
            throw error;
          })
        );
      }
    );
  }
}
```