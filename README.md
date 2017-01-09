# JavaScript Standard Methods
JavaScript includes a small set of standard methods that are available on the standard types.

## Array

### array.concat(item...)
The concat method produces a new array containing a shallow copy of this array with the items appended to it. If an item is an array, then each of its elements is appended individually.

```JavaScript
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.concat(b, true);
// c is ['a', 'b', 'c', 'x', 'y', 'z', true]
```

### array.pop()
The pop and push methods make an array work like a stack. The pop method removes and returns the last element in this array. If the array is empty, it returns undefined.

```JavaScript
var a = ['a', 'b', 'c'];
var c = a.pop( ); // a is ['a', 'b'] & c is 'c'
```

### array.push(item…)
The push method appends items to the end of an array. Unlike the concat method, it modifies the array and appends array items whole. It returns the new length of the array:

```JavaScript
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.push(b, true);
// a is ['a', 'b', 'c', ['x', 'y', 'z'], true]
// c is 5;
```

### array.reverse( )
The reverse method modifies the array by reversing the order of the elements. It returns the array:

```JavaScript
var a = ['a', 'b', 'c'];
var b = a.reverse( );
// both a and b are ['c', 'b', 'a']
```

### array.shift( )
The shift method removes the first element from an array and returns it. If the array is empty, it returns undefined. shift is usually much slower than pop:

```JavaScript
var a = ['a', 'b', 'c'];
var c = a.shift( ); // a is ['b', 'c'] & c is 'a'
```

### array.slice(start, end)
The slice method makes a shallow copy of a portion of an array. The first element copied will be array[start]. It will stop before copying array[end]. The end parameter is optional, and the default is array.length. If either parameter is negative, array.length will be added to them in an attempt to make them nonnegative. If start is greater than or equal to array.length, the result will be a new empty array. 

```JavaScript
var a = ['a', 'b', 'c'];
var b = a.slice(0, 1); // b is ['a']
var c = a.slice(1); // c is ['b', 'c']
var d = a.slice(1, 2); // d is ['b']
```

### array.sort(comparefn)
The sort method sorts the contents of an array in place. It sorts arrays of numbers incorrectly:

```JavaScript
var n = [4, 8, 15, 16, 23, 42];
n.sort( );
// n is [15, 16, 23, 4, 42, 8]
```

JavaScript’s default comparison function assumes that the elements to be sorted are strings.
It isn’t clever enough to test the type of the elements before comparing them, so it converts the numbers to strings as it compares them, ensuring a shockingly incorrect result.
Fortunately, you may replace the comparison function with your own. Your comparison function should take two parameters and return 0 if the two parameters are equal, a negative number if the first parameter should come first, and a positive number if the second parameter should come first. 

```JavaScript
n.sort(function (a, b) {
return a - b;
});
// n is [4, 8, 15, 16, 23, 42];
```
With a smarter comparison function, we can sort an array of objects. To make things easier for the general case, we will write a function that will make comparison functions:

```JavaScript
var s = [
  {first: 'Joe', last: 'Besser'},
  {first: 'Moe', last: 'Howard'},
  {first: 'Joe', last: 'DeRita'},
  {first: 'Shemp', last: 'Howard'},
  {first: 'Larry', last: 'Fine'},
  {first: 'Curly', last: 'Howard'}
];

var by = function (name, minor) {
  return function (o, p) {
  var a, b;
  if (o && p && typeof o === 'object' && typeof p === 'object') {
    a = o[name];
    b = p[name];
    if (a === b) {
      return typeof minor === 'function' ? minor(o, p) : 0;
    }
    if (typeof a === typeof b) {
      return a < b ? -1 : 1;
    }
    return typeof a < typeof b ? -1 : 1;
    } else {
      throw {
        name: 'Error',
        message: 'Expected an object when sorting by ' + name
      };
    }
  };
};

s.sort(by('last', by('first'))); // s is [
// {first: 'Joe', last: 'Besser'},
// {first: 'Joe', last: 'DeRita'},
// {first: 'Larry', last: 'Fine'},
// {first: 'Curly', last: 'Howard'},
// {first: 'Moe', last: 'Howard'},
// {first: 'Shemp', last: 'Howard'}
// ]
```

### array.splice(start, deleteCount, item…)

The splice method removes elements from an array, replacing them with new items. The start parameter is the number of a position within the array. The deleteCount parameter is the number of elements to delete starting from that position. If there are additional parameters, those items will be inserted at the position. It returns an array containing the deleted elements.
The most popular use of splice is to delete elements from an array. Do not confuse splice with slice:

```JavaScript
var a = ['a', 'b', 'c'];
var r = a.splice(1, 1, 'ache', 'bug');
// a is ['a', 'ache', 'bug', 'c']
// r is ['b']
```

### array.unshift(item…)

The unshift method is like the push method except that it shoves the items onto the front of this array instead of at the end. It returns the array’s new length:

```JavaScript
var a = ['a', 'b', 'c'];
var r = a.unshift('?', '@');
// a is ['?', '@', 'a', 'b', 'c']
// r is 5
```

## Function

## function.apply(thisArg, argArray)
The apply method invokes a function, passing in the object that will be bound to this and an optional array of arguments. The apply method is used in the apply invocation pattern:

```JavaScript
Function.method('bind', function (that) {
  // Return a function that will call this function as
  // though it is a method of that object.
  var method = this;
  var slice = Array.prototype.slice;
  var args = slice.apply(arguments, [1]);
  return function ( ) {
    return method.apply(that,
    args.concat(slice.apply(arguments, [0])));
  };
});

var x = function ( ) {
  return this.value;
}.bind({value: 666});

alert(x( )); // 666
```

## Number

### number.toExponential(fractionDigits)
The toExponential method converts this number to a string in the exponential form. The optional fractionDigits parameter controls the number of decimal places. It should be between 0 and 20:

```JavaScript
Math.PI.toExponential(0);
Math.PI.toExponential(2);
Math.PI.toExponential(7);
Math.PI.toExponential(16);
Math.PI.toExponential( );
// Produces
// 3e+0
// 3.14e+0
// 3.1415927e+0
// 3.1415926535897930e+0
// 3.141592653589793e+0
```

### number.toFixed(fractionDigits)
The toFixed method converts this number to a string in the decimal form. The optional fractionDigits parameter controls the number of decimal places. It should be between 0 and 20. The default is 0:

```JavaScript
Math.PI.toFixed(0);
Math.PI.toFixed(2);
Math.PI.toFixed(7);
Math.PI.toFixed(16);
(Math.PI.toFixed( );
// Produces
// 3
// 3.14
// 3.1415927
// 3.1415926535897930
// 3
```

#### number.toPrecision(precision)
The toPrecision method converts this number to a string in the decimal form. The optional precision parameter controls the number of digits of precision. It should be between 1 and 21:

```JavaScript
Math.PI.toPrecision(2);
Math.PI.toPrecision(7);
Math.PI.toPrecision(16);
Math.PI.toPrecision( );
// Produces
// 3.1
// 3.141593
// 3.141592653589793
// 3.141592653589793
```

### number.toString(radix)
The toString method converts this number to a string. The optional radix parameter controls radix, or base. It should be between 2 and 36. The default radix is base 10. The radix parameter is most commonly used with integers, but it can be used on any number.
The most common case, number.toString( ), can be written more simply as String(number):

```JavaScript
Math.PI.toString(2);
Math.PI.toString(8);
Math.PI.toString(16);
Math.PI.toString( );
// Produces
// 11.001001000011111101101010100010001000010110100011
// 3.1103755242102643
// 3.243f6a8885a3
// 3.141592653589793
```

## Object

### object.hasOwnProperty(name)

The hasOwnProperty method returns true if the object contains a property having the name. The prototype chain is not examined. This method is useless if the name is hasOwnProperty:

```JavaScript
var a = {member: true};
var b = Object.create(a);
var t = a.hasOwnProperty('member'); // t is true
var u = b.hasOwnProperty('member'); // u is false
var v = b.member; // v is true
```

## String

### string.charAt(pos)

The charAt method returns the character at position pos in this string. If pos is less than zero or greater than or equal to string.length, it returns the empty string. JavaScript does not have a character type. The result of this method is a string:

```JavaScript
var name = 'Curly';
var initial = name.charAt(0); // initial is 'C'
```

### string.charCodeAt(pos)

The charCodeAt method is the same as charAt except that instead of returning a string, it returns an integer representation of the code point value of the character at position pos in that string. If pos is less than zero or greater than or equal to string.length, it returns NaN:

```JavaScript
var name = 'Curly';
var initial = name.charCodeAt(0); // initial is 67
```

### string.concat(string…)
The concat method makes a new string by concatenating other strings together. It is rarely used because the + operator is more convenient:

```JavaScript
var s = 'C'.concat('a', 't'); // s is 'Cat'
```

### string.indexOf(searchString, position)

The indexOf method searches for a searchString within a string. If it is found, it returns the position of the first matched character; otherwise, it returns –1. The optional position parameter causes the search to begin at some specified position in the string:

```JavaScript
var text = 'Mississippi';
var p = text.indexOf('ss'); // p is 2
p = text.indexOf('ss', 3); // p is 5
p = text.indexOf('ss', 6); // p is -1
```

### string.lastIndexOf(searchString, position)

The lastIndexOf method is like the indexOf method, except that it searches from the end of the string instead of the front:

```JavaScript
var text = 'Mississippi';
var p = text.lastIndexOf('ss'); // p is 5
p = text.lastIndexOf('ss', 3); // p is 2
p = text.lastIndexOf('ss', 6); // p is 5
```

### string.localeCompare(that)
The localCompare method compares two strings. The rules for how the strings are compared are not specified. If this string is less than that string, the result is negative. If they are equal, the result is zero. This is similar to the convention for the array.sort comparison function:

```JavaScript
var m = ['AAA', 'A', 'aa', 'a', 'Aa', 'aaa'];

m.sort(function (a, b) {
  return a.localeCompare(b);
});

// m (in some locale) is
// ['a', 'A', 'aa', 'Aa', 'aaa', 'AAA']
```

### string.match(regexp)
The match method matches a string and a regular expression. How it does this depends on the g flag. If there is no g flag, then the result of calling string.match(regexp) is the same as calling regexp.exec(string). However, if the regexp has the g flag, then it produces an array of all the matches but excludes the capturing groups:

```JavaScript
var text = '<html><body><p>' + 'This is <b>bold<\/b>!<\/p><\/body><\/html>';
var tags = /[^<>]+|<(\/?)([A-Za-z]+)([^<>]*)>/g;
var i;
var a = text.match(tags);

for (i = 0; i < a.length; i += 1) {
  document.writeln(('// [' + i + '] ' + a[i]).entityify( ));
}

// The result is
// [0] <html>
// [1] <body>
// [2] <p>
// [3] This is
// [4] <b>
// [5] bold
// [6] </b>
// [7] !

// [8] </p>
// [9] </body>
// [10] </html>
```
### string.replace(searchValue, replaceValue)

The replace method does a search and replace operation on this string, producing a new string. The searchValue argument can be a string or a regular expression object. If it is a string, only the first occurrence of the searchValue is replaced, so:

```JavaScript
var result = "mother_in_law".replace('_', '-');
```

will produce "mother-in_law", which might be a disappointment.
If searchValue is a regular expression and if it has the g flag, then it will replace all occurrences.
If it does not have the g flag, then it will replace only the first occurrence.
The replaceValue can be a string or a function. If replaceValue is a string, the character $ has special meaning:

```JavaScript
// Capture 3 digits within parens
var oldareacode = /\((\d{3})\)/g;
var p = '(555)666-1212'.replace(oldareacode, '$1-');
// p is '555-555-1212'
```

### string.search(regexp)

The search method is like the indexOf method, except that it takes a regular expression object instead of a string. It returns the position of the first character of the first match, if there is one, or –1 if the search fails. The g flag is ignored. There is no position parameter:

```JavaScript
var text = 'and in it he says "Any damn fool could';
var pos = text.search(/["']/); // pos is 18
```

### string.slice(start, end)

The slice method makes a new string by copying a portion of another string. If the start parameter is negative, it adds string.length to it. The end parameter is optional, and its default value is string.length. If the end parameter is negative, then string.length is added
to it. The end parameter is one greater than the position of the last character. To get n characters starting at position p, u se string.slice(p,p + n). Also see string.substring and array.slice, later and earlier in this chapter, respectively.

```JavaScript
var text = 'and in it he says "Any damn fool could';
var a = text.slice(18);
// a is '"Any damn fool could'
var b = text.slice(0, 3);
// b is 'and'
var c = text.slice(-5);
// c is 'could'
var d = text.slice(19, 32);
// d is 'Any damn fool'
```

### string.split(separator, limit)

The split method creates an array of strings by splitting this string into pieces. The optional limit parameter can limit the number of pieces that will be split. The separator parameter can be a string or a regular expression.
If the separator is the empty string, an array of single characters is produced:

```JavaScript
var digits = '0123456789';
var a = digits.split('', 5);
// a is ['0', '1', '2', '3', '456789']
```
Otherwise, the string is searched for all occurrences of the separator. Each unit of text between the separators is copied into the array. The g flag is ignored:

```JavaScript
var ip = '192.168.1.0';
var b = ip.split('.');
// b is ['192', '168', '1', '0']
var c = '|a|b|c|'.split('|');
// c is ['', 'a', 'b', 'c', '']
var text = 'last, first ,middle';
var d = text.split(/\s*,\s*/);
// d is ['last', 'first', 'middle']
```

There are some special cases to watch out for. Text from capturing groups will be includedin the split:

```JavaScript
var e = text.split(/\s*(,)\s*/);
// e is ['last', ',', 'first', ',', 'middle']
```

Some implementations suppress empty strings in the output array when the separator is a regular expression:

```JavaScript
var f = '|a|b|c|'.split(/\|/);
// f is ['a', 'b', 'c'] on some systems, and
// f is ['', 'a', 'b', 'c', ''] on others
```

### string.substring(start, end)

The substring method is the same as the slice method except that it doesn’t handle the adjustment for negative parameters. There is no reason to use the substring method. Use slice instead.

### string.toLocaleLowerCase( )

The toLocaleLowerCase method produces a new string that is made by converting this string to lowercase using the rules for the locale. This is primarily for the benefit of Turkish because in that language ‘I’ converts to ı, not ‘i’.

### string.toLocaleUpperCase( )

The toLocaleUpperCase method produces a new string that is made by converting this string to uppercase using the rules for the locale. This is primarily for the benefit of Turkish, because in that language ‘i’ converts to ‘ ’, not ‘I’.

### string.toLowerCase( )

The toLowerCase method produces a new string that is made by converting this string to lowercase.

### string.toUpperCase( )

The toUpperCase method produces a new string that is made by converting this string to ppercase.

### String.fromCharCode(char…)

The String.fromCharCode function produces a string from a series of numbers.

```JavaScript
var a = String.fromCharCode(67, 97, 116);
// a is 'Cat'
```





