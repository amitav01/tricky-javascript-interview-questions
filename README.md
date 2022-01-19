# Tricky JavaScript interview questions

## Hoisting and Scoping
1. ### What will be the output here?

```javascript
console.log(x);
var x = 5;
```
**Answer**
```
undefined
```

2. ### What will be the output here?

```javascript
fn();
function fn() {
  console.log('hello');
}
```
**Answer**
```
hello
```
3. ### What will be the output here?

```javascript
fn();
var fn = function() {
  console.log('hello');
}
```
**Answer**
```
TypeError: fn is not a function
```

4. ### What will be the output here?

```javascript
console.log(x);
let x = 5;
```
**Answer**
```
ReferenceError: Cannot access 'x' before initialization
```
Same happens with variable created with `const`.

5. ### What will be the output here?

```javascript
console.log(x);
x = 5;
console.log(x);
var x = 20;
```
**Answer**
```
undefined
5
```

6. ### What would be the output?

```javascript
var x = 20;
if (true) {
 var x = 10;
 console.log(x);
}
console.log(x);
```
**Answer**
```
10
10
```








## Closures
1. ### What will be the output here?

```javascript
for (var i = 0; i < 5; i++) {
  setTimeout(() => { console.log(i); }, 100);
}
```
**Answer**
```
5 5 5 5 5
```
2. ### What will be the output here?

```javascript
for (let i = 0; i < 5; i++) {
  setTimeout(() => { console.log(i); }, 100);
}
```
**Answer**
```
0 1 2 3 4
```
2. ### Write above code using var and print 0 1 2 3 4.

```javascript
for (var i = 0; i < 5; i++) {
    function fn(val) {
        setTimeout(() => { console.log(val); }, 100);
    }
    fn(i);
}

// alternative using IIFE
for (var i = 0; i < 5; i++) {
   setTimeout(((val) => { console.log(val); })(i), 100);
}

```
**Answer**
```
0 1 2 3 4
```


## Event loop

1. ### What will be the output here?

```javascript
let flag = true;

setTimeout(() => { flag = false; }, 100);

while (flag) {
  console.log('inside while loop');
}
```
**Answer**
It will go into infinite loop and keeps on printing "inside while loop". As JavaScript runs on single thread, the main thred will be stuck inside `while` loop and the callback of `seTimeout` will never get a chance to execute. 


2. ### What will be the output here and explain what is wrong in this code ?

```javascript
let flag = true;

setTimeout(() => { flag = false; }, 500);

setInterval(() => { 
  if (flag) console.log('inside setInterval');
}, 100);
```
**Answer**
It will keep on printing "inside setInterval" until `flag` becomes `false` inside `setTimeout` but the `setInterval` keeps on running all the time and it never stopped. We should also consider clearing the interval.
```javascript
setTimeout(() => { clearInterval(interval) }, 500);

const interval = setInterval(() => { 
  console.log('inside setInterval');
}, 100);
```



## Other
1. ### If `value` is a variable, what would be it's value so that the below expression evaluates to `true` ?

```javascript
value !== value
```
**Answer**
NaN. Because `NaN !== NaN`

2. ### What would be the output?

```javascript
console.log(2 + '2');
console.log(2 - '2');
console.log(2 * '2');
console.log(2 / '2');
```
**Answer**
```
22
0
4
1
```
Except unary operator(+), incase of every other operator. string will be converted to number first then expression will be evaluated.

3. ### What would be the output?

```javascript
console.log(7 > 5 > 2);
console.log(9 < 4 < 1);
```
**Answer**
```
false
true
```
Here `7 > 5` will be `true`. And `true > 2` will be converted to `1 > 2` and thus evaluates to `false`. Same happens with `9 < 4` will be `false` and `false < 1` will convert to `0 < 1` and evaluates to `true`;


4. ### What would be the output?

```javascript
function fn() {
  let a = b = 0;
  a++;
  return a;
}
fn();
console.log(typeof a);
console.log(typeof b);
```
**Answer**
```
undefined
'number'
```
Here `let a = b = 0` is same as `b = 0; let a = b;`. So, here `a` is a local variable but `b` is created in global scope, thus we can access it outside the function as well.


5. Write code for debouncing.
**Answer**
```html
<button onclick="debounceSearch(1)"> Search </button>
```
```javascript
function search(x){
	console.log('searching..', x)
}

function debounce(fn, delay) {
	let timeout;
	return function(...args) {
  	clearTimeout(timeout);
  	timeout = setTimeout(() => fn(...args), delay);
  }
}

const debounceSearch = debounce(search, 500);
```
6. Write code for throttling.
**Answer**
```html
<button onclick="throttleSearch()"> Search </button>
```

```javascript
function search(x){
	console.log('searching..', x)
}

function throttle(fn, delay) {
	let flag = false;
	return function(...args) {
  	if (!flag) {
    	fn(...args);
    	flag = true;
      setTimeout(() => { flag = false; }, delay);
    }
  	
  }
}

const throttleSearch = throttle(search, 500);
```
