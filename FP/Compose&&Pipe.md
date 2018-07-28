# Compose & Pipe

## Compose

Compose 用于将几个功能组装起来。从右往左执行。

`finalValue <-- func1 <-- func2 <-- ... <-- funcN <-- origValue`

*words*输入为字符串，输出为数组。*unique*输入为数组，输出为清除重复元素了的数组。

```javascript
function compose(...fns) {
  return function composed(result) {
    // copy the array of functions
    var list = [...fns];

    while (list.length > 0) {
      // take the last function off the end of the list
      // and execute it
      result = list.pop()(result);
    }

    return result;
  };
}
```

```jajascript
function words(str) {
    return String( str )
        .toLowerCase()
        .split( /\s|\b/ )
        .filter( function alpha(v){
            return /^[\w]+$/.test( v );
        } );
}

function unique(list) {
    var uniqList = [];

    for (let v of list) {
        // value not yet in the new list?
        if (uniqList.indexOf( v ) === -1 ) {
            uniqList.push( v );
        }
    }

    return uniqList;
}


var text = "To compose two functions together, pass the output of the first function call as the input of the second function call.";

// words的output作为unique的input.写代码是从左到右，代码执行是从右到左。
var wordsUsed = unique( words( text ) );
// var wordsUsed = compose(unique,words)(text);

wordsUsed;
// ["to","compose","two","functions","together","pass","the","output","of","first","function","call","as","input","second"]
```

## Pipe

从左往右执行。

```javascript
function pipe(...fns) {
  return function piped(result) {
    var list = [...fns];

    while (list.length > 0) {
      // take the first function from the list
      // and execute it
      result = list.shift()(result);
    }

    return result;
  };
}
```

OR

```javascript
var pipe = reverseArgs(compose);
```
