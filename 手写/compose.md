## 方法一

```js
const compose = (functions) => {
  return (x) => {
    if (!functions.length) return x;
    let i = functions.length - 1;
    while (i >= 0) {
      const fn = functions[i];
      x = fn(x);
      i--;
    }
    return x;
  };
};
```

## 方法二

```js
const compose = (functions) => {
  return (x) => {
    return functions.reduceRight((acc, f) => {
      return f(acc);
    }, x);
  };
};
```
