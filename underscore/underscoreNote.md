

`each`/`forEach`
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

`map`/`collect`
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

`find`/`detect`
检测满足某个条件的元素（注意不是索引）
看实现，是无法通过`_.find(arr, element)`直接查找元素的，只能通过`_.find(arr, (x)=>x==element)`来实现。
_.findIndex才是返回索引，同样不能直接检索元素。
