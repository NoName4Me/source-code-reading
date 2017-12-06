
* 数组和对象遍历

```js
function printElement(obj, dir) {
    // obj可以是数组也可以是对象
    var keys = !Array.isArray(obj) && Object.keys(obj);
    var length = (keys || obj).length;
    console.log(length)
    // 正反向遍历, dir = 1 or -1
    for(let idx = dir > 0 ? 0 : length -1; idx >= 0 && idx < length; idx += dir) {
        console.log('key: %s,\t value: %s',(keys ? keys[idx] : idx),obj[keys ? keys[idx] : idx]);
    }
}
```


* `each`/`forEach`
兼容数组和可迭代对象，核心：
```js
 _.each = _.forEach = function(obj, iteratee, context) {
    iteratee = optimizeCb(iteratee, context);
    var i, length;
    if (isArrayLike(obj)) {
        for (i = 0, length = obj.length; i < length; i++) {
        iteratee(obj[i], i, obj);
        }
    } else {
        var keys = _.keys(obj);
        for (i = 0, length = keys.length; i < length; i++) {
        iteratee(obj[keys[i]], keys[i], obj);
        }
    }
    return obj;
};
```

* `map`/`collect`

```js
// 注意和each中数组于对象处理的不同
_.map = _.collect = function(obj, iteratee, context) {
    iteratee = cb(iteratee, context);
    var keys = !isArrayLike(obj) && _.keys(obj),
        length = (keys || obj).length,
        results = Array(length);
    for (var index = 0; index < length; index++) {
        var currentKey = keys ? keys[index] : index;
        results[index] = iteratee(obj[currentKey], currentKey, obj);
    }
    return results;
};
```

* `find`/`detect`

检测满足某个条件的元素（注意不是索引）。看实现，是无法通过`_.find(arr, element)`直接查找元素的，只能通过`_.find(arr, (x)=>x==element)`来实现。

`findIndex`才是返回索引，同样不能直接检索元素。

* `filter`/`select`（返回数组）

如果不是因为返回的是数组，`every`等都可以直接使用`filter`。

* `pluck`