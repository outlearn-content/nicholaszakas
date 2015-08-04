<!--
{
"name" : "understanding-es6",
"version" : "0.1",
"title" : "Understanding ECMAScript 6",
"description" : "This module is aimed at intermediate-to-advanced JavaScript developers (both browser and Node.js environments) who want to learn about the future of the language",
"homepage" : "https://leanpub.com/understandinges6/read/",
"canonicalSource" : "https://leanpub.com/understandinges6/read/",
"freshnessDate" : 2015-07-20,
"license" : "CC BY-NC-ND"
}
-->

<!-- @section -->
# Introduction

The JavaScript core language features are defined in a standard called ECMA-262. The language defined in this standard is called ECMAScript, of which the JavaScript in the browser and Node.js environments are a superset. While browsers and Node.js may add more capabilities through additional objects and methods, the core of the language remains as defined in ECMAScript, which is why the ongoing development of ECMA-262 is vital to the success of JavaScript as a whole.

In 2007, JavaScript was at a crossroads. The popularity of Ajax was ushering in a new age of dynamic web applications while JavaScript hadn't changed since the third edition of ECMA-262 was published in 1999. TC-39, the committee responsible for driving the ECMAScript process, put together a large draft specification for ECMAScript 4. ECMAScript 4 was massive in scope, introducing changes both small and large to the language. Language features included new syntax, modules, classes, classical inheritance, private object members, optional type annotations, and more.

The scope of the ECMAScript 4 changes caused a rift to form in TC-39, with some members feeling that the fourth edition was trying to accomplish too much. A group of leaders from Yahoo, Google, and Microsoft came up with an alternate proposal for the next version of ECMAScript that they initially called ECMAScript 3.1. The "3.1" was intended to show that this was an incremental change to the existing standard.

ECMAScript 3.1 introduced very few syntax changes, instead focusing on property attributes, native JSON support, and adding methods to already-existing objects. Although there was an early attempt to reconcile ECMAScript 3.1 and ECMAScript 4, this ultimately failed as the two camps had difficulty with the very different perspectives on how the language should grow.

In 2008, Brendan Eich, the creator of JavaScript, announced that TC-39 would focus its efforts on standardizing ECMAScript 3.1. They would table the major syntax and feature changes of ECMAScript 4 until after the next version of ECMAScript was standardized, and all members of the committee would work to bring the best pieces of ECMAScript 3.1 and 4 together after that point into an effort initially nicknamed ECMAScript Harmony.

ECMAScript 3.1 was eventually standardized as the fifth edition of ECMA-262, also described as ECMAScript 5. The committee never released an ECMAScript 4 standard to avoid confusion with the now-defunct effort of the same name. Work then began on ECMAScript Harmony, with ECMAScript 6 being the first standard released in this new "harmonious" spirit.

ECMAScript 6 reached feature complete status in 2014. The features vary widely from completely new objects and patterns to syntax changes to new methods on existing objects. The exciting thing about ECMAScript 6 is that all of these changes are geared towards problems that developers are actually facing. And while it will still take time for adoption and implementation to reach the point where ECMAScript 6 is the minimum that developers can expect, there's a lot to be gained from a good understanding of what the future of JavaScript looks like.

## Browser and Node.js Compatibility

Many JavaScript environments, such as web browsers and Node.js, are actively working on implementing ECMAScript 6. This book does not attempt to address the inconsistencies between implementations and instead focuses on what the specification defines as the correct behavior. As such, it's possible that your JavaScript environment may not conform to the behavior described in this book.

## Who This Book is For

This book is intended as a guide for those who are already familiar with JavaScript and ECMAScript 5. While a deep understanding of the language isn't necessary to use this book, it is helpful in understanding the differences between ECMAScript 5 and 6. In particular, this book is aimed at intermediate-to-advanced JavaScript developers (both browser and Node.js environments) who want to learn about the future of the language.

This book is not for beginners who have never written JavaScript. You will need to have a good basic understanding of the language to make use of this book.

## Overview

**Chapter 1: The Basics** introduces the smallest changes in the language. These are the new features that don't necessarily introduce syntax changes, but rather are incremental changes on top of ECMAScript 5.

**Chapter 2: Functions** discusses the various changes to functions. This includes the arrow function form, default parameters, rest parameters, and more.

**Chapter 3: Objects** explains the changes to how objects are created, modified, and used. Topics include changes to object literal syntax, and new reflection methods.

**Chapter 4: Symbols** introduces the concept of symbols, a new way to define properties. Symbols are a new primitive type that can be used to obscure (but not hide) object properties and methods.

**Chapter 5: Arrays** details the changes to native arrays and the interesting new ways they can be used in JavaScript.

**Chapter 6: Collections** details the new collection types of `Set`, `WeakSet`, `Map`, and `WeakMap`. These types expand on the usefulness of arrays by adding semantics, de-duping, and memory management designed specifically for JavaScript.

**Chapter 7: Classes** introduces the first formal concept of classes in JavaScript. Often a point of confusion for those coming from other languages, the addition of class syntax in JavaScript makes the language more approachable to others and more concise for enthusiasts.

**Chapter 8: Iterators and Generators** discusses the addition of iterators and generators to the language. These features allow you to work with collections of data in powerful ways that were not possible in previous versions of JavaScript.

**Chapter 9: Proxies** discusses the new proxy object that allows you to intercept every operation performed on an object. Proxies give developers unprecedented control over objects and, as such, unlimited possibilities for defining new interaction patterns.

**Chapter 10: Promises** introduces promises as a new part of the language. Promises were a grassroots effort that eventually took off and gained in popularity due to extensive library support. ECMAScript 6 formalizes promises and makes them available by default.

**Chapter 11: Modules** details the official module format for JavaScript. The intent is that these modules can replace the numerous ad-hoc module definition formats that have appeared over the years.

**Chapter 12: Template Strings** discusses the new built-in templating functionality. Template strings are designed to easily create DSLs in a secure way.

**Chapter 13: Reflection** introduces the formalized reflection API for JavaScript. Similar to other languages, ECMAScript 6 reflection allows you to inspect objects at a granular level, even if you didn't create the object.

## Help and Support

You can file issues, suggest changes, and open pull requests against this book by visiting: [https://github.com/nzakas/understandinges6](https://github.com/nzakas/understandinges6)

For anything else, please send a message to the mailing list: [http://groups.google.com/group/zakasbooks](http://groups.google.com/group/zakasbooks).
<!-- @section -->
# The Basics

ECMAScript 6 makes a large number of changes on top of ECMAScript 5. Some of the changes are larger, such as adding new types or syntax, while others are quite small, providing incremental improvements on top of the language. This chapter covers those incremental improvements that likely won't gain a lot of attention but provide some important functionality that may make certain types of problems easier to solve.

## Better Unicode Support

Prior to ECMAScript 6, JavaScript strings were based solely on the idea of 16-bit character encodings. All string properties and methods, such as `length` and `charAt()`, were based around the idea that every 16-bit sequence represented a single character. ECMAScript 5 allowed JavaScript engines to decide which of two encodings to use, either UCS-2 or UTF-16 (both encodings use 16-bit *code units*, making all observable operations the same). While it's true that all of the world's characters used to fit into 16 bits at one point in time, that is no longer the case.

Keeping within 16 bits wasn't possible for Unicode's stated goal of providing a globally unique identifier to every character in the world. These globally unique identifiers, called *code points*, are simply numbers starting at 0 (you might think of these as character codes, though there is a subtle difference). A character encoding is responsible for encoding a code point into code units that are internally consistent. While UCS-2 had a one-to-one mapping of code point to code unit, UTF-16 is more variable.

The first 2^16 code points are represented as single 16-bit code units in UTF-16. This is called the *Basic Multilingual Plane* (BMP). Everything beyond that range is considered to be in a *supplementary plane*, where the code points can no longer be represented in just 16-bits. UTF-16 solves this problem by introducing *surrogate pairs* in which a single code point is represented by two 16-bit code units. That means any single character in a string can be either one code unit (for BMP, total of 16 bits) or two (for supplementary plane characters, total of 32 bits).

ECMAScript 5 kept all operations as working on 16-bit code units, meaning that you could get unexpected results from strings containing surrogate pairs. For example:

```javascript
var text = "𠮷";

console.log(text.length);           // 2
console.log(/^.$/.test(text));      // false
console.log(text.charAt(0));        // ""
console.log(text.charAt(1));        // ""
console.log(text.charCodeAt(0));    // 55362
console.log(text.charCodeAt(1));    // 57271
```

In this example, a single Unicode character is represented using surrogate pairs, and as such, the JavaScript string operations treat the string as having two 16-bit characters. That means `length` is 2, a regular expression trying to match a single character fails, and `charAt()` is unable to return a valid character string. The `charCodeAt()` method returns the appropriate 16-bit number for each code unit, but that is the closest you could get to the real value in ECMAScript 5.

ECMAScript 6 enforces encoding of strings in UTF-16. Standardizing on this character encoding means that the language can now support functionality designed to work specifically with surrogate pairs.

### The codePointAt() Method

The first example of fully supporting UTF-16 is the `codePointAt()` method, which can be used to retrieve the Unicode code point that maps to a given position in a string. This method accepts the code unit position (not the character position) and returns an integer value:

```javascript
var text = "𠮷a";

console.log(text.charCodeAt(0));    // 55362
console.log(text.charCodeAt(1));    // 57271
console.log(text.charCodeAt(2));    // 97

console.log(text.codePointAt(0));   // 134071
console.log(text.codePointAt(1));   // 57271
console.log(text.codePointAt(2));   // 97
```

The `codePointAt()` method works in the same manner as `charCodeAt()` except for non-BMP characters. The first character in `text` is non-BMP and is therefore comprised of two code units, meaning the entire length of the string is 3 rather than 2. The `charCodeAt()` method returns only the first code unit for position 0 whereas `codePointAt()` returns the full code point even though it spans multiple code units. Both methods return the same value for positions 1 (the second code unit of the first character) and 2 (the `"a"`).

This method is the easiest way to determine if a given character is represented by one or two code points:

```javascript
function is32Bit(c) {
    return c.codePointAt(0) > 0xFFFF;
}

console.log(is32Bit("𠮷"));         // true
console.log(is32Bit("a"));          // false
```

The upper bound of 16-bit characters is represented in hexadecimal as `FFFF`, so any code point above that number must be represented by two code units.

### String.fromCodePoint()

When ECMAScript provides a way to do something, it also tends to provide a way to do the reverse. You can use `codePointAt()` to retrieve the code point for a character in a string, while `String.fromCodePoint()` produces a single-character string for the given code point. For example:

```javascript
console.log(String.fromCodePoint(134071));  // "𠮷"
```

You can think of `String.fromCodePoint()` as a more complete version of `String.fromCharCode()`. Each method has the same result for all characters in the BMP; the only difference is with characters outside of that range.

### Escaping Non-BMP Characters

ECMAScript 5 allows strings to contain 16-bit Unicode characters represented by an *escape sequence*. The escape sequence is the `\u` followed by four hexadecimal values. For example, the escape sequence `\u0061` represents the letter `"a"`:

```javascript
console.log("\u0061");      // "a"
```

If you try to use an escape sequence with a number past `FFFF`, the upper bound of the BMP, then you can get some surprising results:

```javascript
console.log("\u20BB7");     // "₻7"
```

Since Unicode escape sequences were defined as always having exactly four hexadecimal characters, ECMAScript evaluates `\u20BB7` as two characters: `\u20BB` and `"7"`. The first character is unprintable and the second is the number 7.

ECMAScript 6 solves this problem by introducing an extended Unicode escape sequence where the hexadecimal numbers are contained within curly braces. This allows any number of hexadecimal characters to specify a single character:

```javascript
console.log("\u{20BB7}");     // "𠮷"
```

Using the extended escape sequence, the correct character is contained in the string.

> Make sure that you use this new escape sequence only in an ECMAScript 6 environment. In all other environments, doing so causes a syntax error. You may want to check and see if the environment supports the extended escape sequence using a function such as:
>
>```javascript
> function supportsExtendedEscape() {
>     try {
>         eval("'\\u{00FF1}'");
>         return true;
>     } catch (ex) {
>         return false;
>     }
> }
>```

### The normalize() Method

Another interesting aspect of Unicode is that different characters may be considered equivalent for the purposes of sorting or other comparison-based operations. There are two ways to define these relationships. First, *canonical equivalence* means that two sequences of code points are considered interchangeable in all respects. That even means that a combination of two characters can be canonically equivalent to one character. The second relationship is *compatibility*, meaning that two sequences of code points having different appearances but can be used interchangeably in certain situations.

The important thing to understand is that due to these relationships, it's possible to have two strings that represent fundamentally the same text and yet have them contain different code point sequences. For example, the character "æ" and the string "ae" may be used interchangeably even though they are different code points. These two strings would therefore be unequal in JavaScript unless they are normalized in some way.

ECMAScript 6 supports Unicode normalization forms through a new `normalize()` method on strings. This method optionally accepts a single string parameter indicating the Unicode normalization form to apply: `"NFC"` (default), `"NFD"`, `"NFKC"`, or `"NFKD"`. It's beyond the scope of this book to explain the differences between these four forms. Just keep in mind that, when comparing strings, both strings must be normalized to the same form. For example:

```javascript
var normalized = values.map(function(text) {
    return text.normalize();
});

normalized.sort(function(first, second) {
    if (first < second) {
        return -1;
    } else if (first === second) {
        return 0;
    } else {
        return 1;
    }
});
```

In this code, the strings in a `values` array are converted into a normalized form so that the array can be sorted appropriately. You can accomplish the sort on the original array by calling `normalize()` as part of the comparator:

```javascript
values.sort(function(first, second) {
    var firstNormalized = first.normalize(),
        secondNormalized = second.normalize();

    if (firstNormalized < secondNormalized) {
        return -1;
    } else if (firstNormalized === secondNormalized) {
        return 0;
    } else {
        return 1;
    }
});
```

Once again, the most important thing to remember is that both values must be normalized in the same way. These examples have used the default, NFC, but you can just as easily specify one of the others:

```javascript
values.sort(function(first, second) {
    var firstNormalized = first.normalize("NFD"),
        secondNormalized = second.normalize("NFD");

    if (firstNormalized < secondNormalized) {
        return -1;
    } else if (firstNormalized === secondNormalized) {
        return 0;
    } else {
        return 1;
    }
});
```

If you've never worried about Unicode normalization before, then you probably won't have much use for this method. However, knowing that it is available will help, should you ever end up working on an internationalized application.

### The Regular Expression u Flag

Many common string operations are accomplished by using regular expressions. However, as noted earlier, regular expressions also work on the basis of 16-bit code units each representing a single character. That's why the single character match in the earlier example didn't work. To address this problem, ECMAScript 6 defines a new flag for regular expressions: `u` for "Unicode".

When a regular expression has the `u` flag set, it switches modes to work on characters and not code units. That means the regular expression will no longer get confused about surrogate pairs in strings and can behave as expected. For example:

```javascript
var text = "𠮷";

console.log(text.length);           // 2
console.log(/^.$/.test(text));      // false
console.log(/^.$/u.test(text));     // true
```

Adding the `u` flag allows the regular expression to correctly match the string by characters. Unfortunately, ECMAScript 6 does not natively have a way of determining how many code points are present in a string; fortunately, regular expressions with the `u` flag can be used to figure it out:

```javascript
function codePointLength(text) {
    var result = text.match(/[\s\S]/gu);
    return result ? result.length : 0;
}

console.log(codePointLength("abc"));    // 3
console.log(codePointLength("𠮷bc"));   // 3
```

The regular expression in this example matches both whitespace and non-whitespace characters, and is applied globally with Unicode enabled. The `result` contains an array of matches when there's at least one match, so the array length ends up being the number of code points in the string.

> Although this approach works, it's not very fast, especially when applied to long strings. Try to minimize counting code points whenever possible. Hopefully ECMAScript 7 will bring a more performant means by which to count code points.

Since the `u` flag is a syntax change, attempting to use it in non-compliant JavaScript engines means a syntax error is thrown. The safest way to determine if the `u` flag is supported is with a function:

```javascript
function hasRegExpU() {
    try {
        var pattern = new RegExp(".", "u");
        return true;
    } catch (ex) {
        return false;
    }
}
```

This function uses the `RegExp` constructor to pass in the `u` flag as an argument. This is valid syntax even in older JavaScript engines, however, the constructor will throw an error if `u` isn't supported.

> If your code still needs to work in older JavaScript engines, it's best to use the `RegExp` constructor exclusively when using the `u` flag. This will prevent syntax errors and allow you to optionally detect and use the `u` flag without aborting execution.

### Unicode Identifiers

Better Unicode support in ECMAScript 6 also means changes to what characters may be used for an identifier. In ECMAScript 5, it was already possible to use Unicode escape sequences for identifiers, such as:

```javascript
// Valid in ECMAScript 5 and 6
var \u0061 = "abc";

console.log(\u0061);        // "abc"

// equivalent to
// console.log(a);          // "abc"
```

In ECMAScript 6, you can also use Unicode code point escape sequences as identifiers:

```javascript
// Valid in ECMAScript 5 and 6
var \u{61} = "abc";

console.log(\u{61});        // "abc"

// equivalent to
// console.log(a);          // "abc"
```

Additionally, ECMAScript 6 formally specifies valid identifiers in terms of [Unicode Standard Annex #31: Unicode Identifier and Pattern Syntax](http://unicode.org/reports/tr31/):

1. The first character must be `$`, `_`, or any Unicode symbol with a derived core property of `ID_Start`.
1. Each subsequent character must be `$`, `_`, `\u200c` (zero-width non-joiner), `\u200d` (zero-width joiner), or any Unicode symbol with a derived core property of `ID_Continue`.

The `ID_Start` and `ID_Continue` derived core properties are defined in Unicode Identifier and Pattern Syntax as a way to identify symbols that are appropriate for use in identifiers such as variables and domain names (the specification is not specific to JavaScript).

## Other String Changes

JavaScript strings have always lagged behind similar features of other languages. It was only in ECMAScript 5 that strings finally gained a `trim()` method, and ECMAScript 6 continues extending strings with new functionality.

### includes(), startsWith(), endsWith()

Developers have used `indexOf()` as a way to identify strings inside of other strings since JavaScript was first introduced. ECMAScript 6 adds three new methods whose purpose is to identify strings inside of other strings:

* `includes()` - returns true if the given text is found anywhere within the string or false if not.
* `startsWith()` - returns true if the given text is found at the beginning of the string or false if not.
* `endsWith()` - returns true if the given text is found at the end of the string or false if not.

Each of these methods accepts two arguments: the text to search for and an optional location from which to start the search. When the second argument is omitted, `includes()` and `startsWith()` start search from the beginning of the string while `endsWith()` starts from the end. In effect, the second argument results in less of the string being searched. Here are some examples:

```javascript
var msg = "Hello world!";

console.log(msg.startsWith("Hello"));       // true
console.log(msg.endsWith("!"));             // true
console.log(msg.includes("o"));             // true

console.log(msg.startsWith("o"));           // false
console.log(msg.endsWith("world!"));        // true
console.log(msg.includes("x"));             // false

console.log(msg.startsWith("o", 4));        // true
console.log(msg.endsWith("o", 8));          // true
console.log(msg.includes("o", 8));          // false
```

These three methods make it much easier to identify substrings without needing to worry about identifying their exact position.

> All of these methods return a boolean value. If you need to find the position of a string within another, use `indexOf()` or `lastIndexOf()`.

> The `startsWith()`, `endsWith()`, and `includes()` methods will throw an error if you pass a regular expression instead of a string. This stands in contrast to `indexOf()` and `lastIndexOf()`, which both convert a regular expression argument into a string and then search for that string.

### repeat()

ECMAScript 6 also adds a `repeat()` method to strings. This method accepts a single argument, which is the number of times to repeat the string, and returns a new string that has the original string repeated the specified number of times. For example:

```javascript
console.log("x".repeat(3));         // "xxx"
console.log("hello".repeat(2));     // "hellohello"
console.log("abc".repeat(4));       // "abcabcabcabc"
```

This method is really a convenience function above all else, which can be especially useful when dealing with text manipulation. One example where this functionality comes in useful is with code formatting utilities where you need to create indentation levels:

```javascript
// indent using a specified number of spaces
var indent = " ".repeat(size),
    indentLevel = 0;

// whenever you increase the indent
var newIndent = indent.repeat(++indentLevel);
```

## Other Regular Expression Changes

Regular expressions are an important part of working with strings in JavaScript, and like many parts of the language, haven't really changed very much in recent versions. ECMAScript 6, however, makes several improvements to regular expressions to go along with the updates to strings.

### The Regular Expression y Flag

ECMAScript 6 standardized the `y` flag after it had been implemented in Firefox as a proprietary extension to regular expressions. The `y` (sticky) flag starts matching at the position specified by its `lastIndex` property. If there is no match at that location, then the regular expression stops matching. For example:

```javascript
var text = "hello1 hello2 hello3",
    pattern = /hello\d\s?/,
    result = pattern.exec(text),
    globalPattern = /hello\d\s?/g,
    globalResult = globalPattern.exec(text),
    stickyPattern = /hello\d\s?/y,
    stickyResult = stickyPattern.exec(text);

console.log(result[0]);         // "hello1 "
console.log(globalResult[0]);   // "hello1 "
console.log(stickyResult[0]);   // "hello1 "

pattern.lastIndex = 1;
globalPattern.lastIndex = 1;
stickyPattern.lastIndex = 1;

result = pattern.exec(text);
globalResult = globalPattern.exec(text);
stickyResult = stickyPattern.exec(text);

console.log(result[0]);         // "hello1 "
console.log(globalResult[0]);   // "hello2 "
console.log(stickyResult[0]);   // Error! stickyResult is null
```

In this example, three regular expressions are used, one each with the `y` flag, the `g` flag, and no flags. When used the first time, all three regular expressions return the same value `"hello1 "` (with a space at the end). After that, the `lastIndex` property is changed to 1, meaning that the regular expression should start matching from the second character. The regular expression with no flags completely ignores the change to `lastIndex` and still matches `"hello1 "`; the regular expression with the `g` flag goes on to match `"hello2 "` because it is searching forward from the second character of the string ("e"); the sticky regular expression doesn't match anything beginning at the second character so `stickyResult` is `null`.

The sticky flag saves the index of the next character after the last match in `lastIndex` whenever an operation is performed. If an operation results in no match then `lastIndex` is set back to 0. This behavior is the same as the global flag:

```javascript
var text = "hello1 hello2 hello3",
    pattern = /hello\d\s?/,
    result = pattern.exec(text),
    globalPattern = /hello\d\s?/g,
    globalResult = globalPattern.exec(text),
    stickyPattern = /hello\d\s?/y,
    stickyResult = stickyPattern.exec(text);

console.log(result[0]);         // "hello1 "
console.log(globalResult[0]);   // "hello1 "
console.log(stickyResult[0]);   // "hello1 "

console.log(pattern.lastIndex);         // 0
console.log(globalPattern.lastIndex);   // 7
console.log(stickyPattern.lastIndex);   // 7

result = pattern.exec(text);
globalResult = globalPattern.exec(text);
stickyResult = stickyPattern.exec(text);

console.log(result[0]);         // "hello1 "
console.log(globalResult[0]);   // "hello2 "
console.log(stickyResult[0]);   // "hello2 "

console.log(pattern.lastIndex);         // 0
console.log(globalPattern.lastIndex);   // 14
console.log(stickyPattern.lastIndex);   // 14
```

The value of `lastIndex` changed to 7 after the first call to `exec()` and to 14 after the second call for both the sticky and global patterns.

There are also a couple other subtle details to the sticky flag:

1. The `lastIndex` property is only honored when calling methods on the regular expression object such as `exec()` and `test()`. Passing the regular expression to a string method, such as `match()`, will not result in the sticky behavior.
1. When using the `^` character to match the start of a string, sticky regular expressions will only match from the start of the string (or start of line in multiline mode). So long as `lastIndex` is 0, the `^` makes a sticky regular expression no different from a non-sticky one. If `lastIndex` doesn't correspond to the beginning of the string (in single-line mode) or the beginning of a line (in multiline mode), the sticky regular expression will never match

As with other regular expression flags, you can detect the presence of `y` by using a property. The `sticky` property is set to true with the sticky flag is present and false if not:

```javascript
var pattern = /hello\d/y;

console.log(pattern.sticky);    // true
```

The `sticky` property is read-only based on the presence of the flag and so cannot be changed in code.


Similar to the `u` flag, the `y` flag is a syntax change, so it will cause a syntax error in older JavaScript engines. You can use the same approach to detect support:

```javascript
function hasRegExpY() {
    try {
        var pattern = new RegExp(".", "y");
        return true;
    } catch (ex) {
        return false;
    }
}
```

Also similar to `u`, if you need to use `y` in code that runs in older JavaScript engines, be sure to use the `RegExp` constructor when defining those regular expressions to avoid a syntax error.

### Duplicating Regular Expressions

In ECMAScript 5, you can duplicate regular expressions by passing them into the `RegExp` constructor, such as:

```javascript
var re1 = /ab/i,
    re2 = new RegExp(re1);
```

However, if you provide the second argument to `RegExp`, which specifies the flags for the regular expression, then an error is thrown:

```javascript
var re1 = /ab/i,

    // throws an error in ES5, okay in ES6
    re2 = new RegExp(re1, "g");
```

If you execute this code in an ECMAScript 5 environment, you'll get an error stating that the second argument cannot be used when the first argument is a regular expression. ECMAScript 6 changed this behavior such that the second argument is allowed and will override whichever flags are present on the first argument. For example:

```javascript
var re1 = /ab/i,

    // throws an error in ES5, okay in ES6
    re2 = new RegExp(re1, "g");


console.log(re1.toString());            // "/ab/i"
console.log(re2.toString());            // "/ab/g"

console.log(re1.test("ab"));            // true
console.log(re2.test("ab"));            // true

console.log(re1.test("AB"));            // true
console.log(re2.test("AB"));            // false

```

In this code, `re1` has the case-insensitive `i` flag present while `re2` has only the global `g` flag. The `RegExp` constructor duplicated the pattern from `re1` and then substituted `g` for `i`. If the second argument was missing then `re2` would have the same flags as `re1`.

### The `flags` Property

In ECMAScript 5, it's possible to get the text of the regular expression by using the `source` property, but to get the flag string requires parsing the output of `toString()`, such as:

```javascript
function getFlags(re) {
    var text = re.toString();
    return text.substring(text.lastIndexOf("/") + 1, text.length);
}

// toString() is "/ab/g"
var re = /ab/g;

console.log(getFlags(re));          // "g"
```

ECMAScript 6 adds a `flags` property to go along with `source`. Both properties are prototype accessor properties with only a getter assigned (making them read-only). The addition of `flags` makes it easier to inspect regular expressions for both debugging and inheritance purposes.

A late addition to ECMAScript 6, the `flags` property returns the string representation of any flags applied to a regular expression. For example:

```javascript
var re = /ab/g;

console.log(re.source);     // "ab"
console.log(re.flags);      // "g"
```

Using `source` and `flags` together allow you to extract just the pieces of the regular expression that are necessary without needing to parse the regular expression string directly.


## Object.is()

When you want to compare two values, you're probably used to using either the equals operator (`==`) or the identically equals operator (`===`). Many prefer to use the latter to avoid type coercion during the comparison. However, even the identically equals operator isn't entirely accurate. For example, the values +0 and -0 are considered equal by `===` even though they are represented differently in the JavaScript engine. Also `NaN === NaN` returns `false`, which necessitates using `isNaN()` to detect `NaN` properly.

ECMAScript 6 introduces `Object.is()` to make up for the remaining quirks of the identically equals operator. This method accepts two arguments and returns `true` if the values are equivalent. Two values are considered equivalent when they are of the same type and have the same value. In many cases, `Object.is()` works the same as `===`. The only differences are that +0 and -0 are considered not equivalent and `NaN` is considered equivalent to `NaN`. Here are some examples:

```javascript
console.log(+0 == -0);              // true
console.log(+0 === -0);             // true
console.log(Object.is(+0, -0));     // false

console.log(NaN == NaN);            // false
console.log(NaN === NaN);           // false
console.log(Object.is(NaN, NaN));   // true

console.log(5 == 5);                // true
console.log(5 == "5");              // true
console.log(5 === 5);               // true
console.log(5 === "5");             // false
console.log(Object.is(5, 5));       // true
console.log(Object.is(5, "5"));     // false
```

In most cases you will probably still want to use `==` or `===` for comparing values, as special cases covered by `Object.is()` may not affect you directly.

## Block bindings

Traditionally, one of the tricky parts of JavaScript has been the way that `var` declarations work. In most C-based languages, variables are created at the spot where the declaration occurs. In JavaScript, however, this is not the case. Variables declared using `var` are *hoisted* to the top of the function (or global scope) regardless of where the actual declaration occurs. For example:

```javascript
function getValue(condition) {

    if (condition) {
        var value = "blue";

        // other code

        return value;
    } else {

        // value exists here with a value of undefined

        return null;
    }

    // value exists here with a value of undefined
}
```

If you are unfamiliar with JavaScript, you might expect that the variable `value` is only defined if `condition` evaluates to true. In fact, the variable `value` is declared regardless. The JavaScript engine changes the function to look like this:

```javascript
function getValue(condition) {

    var value;

    if (condition) {
        value = "blue";

        // other code

        return value;
    } else {

        return null;
    }
}
```

The declaration of `value` is moved to the top (hoisted) while the initialization remains in the same spot. That means the variable `value` is actually still accessible from within the `else` clause, it just has a value of `undefined` because it hasn't been initialized.

It often takes new JavaScript developers some time to get used to declaration hoisting and this unique behavior can end up causing bugs. For this reason, ECMAScript 6 introduces block level scoping options to make the control of variable lifecycle a little more powerful.

### Let declarations

The `let` declaration syntax is the same as for `var`. You can basically replace `var` with `let` to declare a variable but keep its scope to the current code block. For example:

```javascript
function getValue(condition) {

    if (condition) {
        let value = "blue";

        // other code

        return value;
    } else {

        // value doesn't exist here

        return null;
    }

    // value doesn't exist here
}
```

This function now behaves much closer to other C-based languages. The variable `value` is declared using `let` instead of `var`. That means the declaration is not hoisted to the top, and the variable `value` is destroyed once execution has flowed out of the `if` block. If `condition` evaluates to false, then `value` is never declared or initialized.

Perhaps one of the areas where developers most want block level scoping of variables is with `for` loops. It's not uncommon to see code such as this:

```javascript
for (var i=0; i < items.length; i++) {
    process(items[i]);
}

// i is still accessible here and is equal to items.length
```

In other languages, where block level scoping is the default, code like this works as intended. In JavaScript, the variable `i` is still accessible after the loop is completed because the `var` declaration was hoisted. Using `let` allows you to get the intended behavior:

```javascript
for (let i=0; i < items.length; i++) {
    process(items[i]);
}

// i is not accessible here
```

In this example, the variable `i` only exists within the `for` loop. Once the loop is complete, the variable is destroyed and is no longer accessible elsewhere.

> ### Using let in loops
>
> The behavior of `let` inside of loops is slightly different than with other blocks. Instead of creating a variable that is used with each iteration of the loop, each iteration actually gets its own variable to use. This is to solve an old problem with JavaScript loops. Consider the following:
>
>```javascript
>  var funcs = [];
>
>  for (var i=0; i < 10; i++) {
>      funcs.push(function() { console.log(i); });
>  }
>
>  funcs.forEach(function(func) {
>      func();     // outputs the number "10" ten times
>  });
>```
>
> This code will output the number `10` ten times in a row. That's because the variable `i` is shared across each iteration of the loop, meaning the closures created inside the loop all hold a reference to the same variable. The variable `i` has a value of `10` once the loop completes, and so that's the value each function outputs.
>
> To fix this problem, developers use immediately-invoked function expressions (IIFEs) inside of loops to force a new copy of the variable to be created:
>
>```javascript
>  var funcs = [];
>
>  for (var i=0; i < 10; i++) {
>      funcs.push((function(value) {
>          return function() {
>              console.log(value);
>          }
>      }(i)));
>  }
>
>  funcs.forEach(function(func) {
>      func();     // outputs 0, then 1, then 2, up to 9
>  });
>```
>
> This version of the example uses an IIFE inside of the loop. The `i` variable is passed to the IIFE, which creates its own copy and stores it as `value`. This is the value used of the function for that iteration, so calling each function returns the expected value.
>
> A `let` declaration does this for you without the IIFE. Each iteration through the loop results in a new variable being created and initialized to the value of the variable with the same name from the previous iteration. That means you can simplify the process by using this code:
>
>```javascript
>  var funcs = [];
>
>  for (let i=0; i < 10; i++) {
>      funcs.push(function() { console.log(i); });
>  }
>
>  funcs.forEach(function(func) {
>      func();     // outputs 0, then 1, then 2, up to 9
>  })
>```
>
>  This code works exactly like the code that used `var` and an IIFE but is, arguably, cleaner.

Unlike `var`, `let` has no hoisting characteristics. A variable declared with `let` cannot be accessed until after the `let` statement. Attempting to do so results in a reference error:

```javascript
if (condition) {
    console.log(value);     // ReferenceError!
    let value = "blue";
}
```

In this code, the variable `value` is defined and initialized using `let`, but that statement is never executed because the previous line throws an error. The issue is that `value` exists in what has become known as the *temporal dead zone* (TDZ). The TDZ is never named explicitly in the specification, but is a term often used to describe the non-hoisting behavior of `let`.

When a JavaScript parser looks through the upcoming block to find variable declarations, it results in either hoisting the declaration (for `var`) or placing the declaration in the TDZ. Any attempt to access a variable in the TDZ results in a runtime error. Only once execution flows to the variable declaration is that variable removed from the TDZ and therefore safe to use.

The same is true anytime you attempt to use a variable declared with `let` inside of the same block prior to it being defined. Even the normally safe-to-use `typeof` operator isn't safe:

```javascript
if (condition) {
    console.log(typeof value);     // ReferenceError!
    let value = "blue";
}
```

Here, `typeof value` throws the same error as the previous example. You cannot use a `let` variable before its declaration within the same block. However, you can use `typeof` outside of the block:

```javascript
console.log(typeof value);     // "undefined"

if (condition) {
    let value = "blue";
}
```

The variable `value` isn't in the TDZ when the `typeof` operation executes because it occurs outside of the block in which `value` is declared. That means there is no `value` binding and `typeof` simply returns `"undefined"`.

If an identifier has already been defined in the block, then using the identifier in a `let` declaration causes an error to be thrown. For example:

```javascript
var count = 30;

// Syntax error
let count = 40;
```

In this example, `count` is declared twice, once with `var` and once with `let`. Because `let` will not redefine an identifier that already exists in the same scope, the declaration throws an error. No error is thrown if a `let` declaration creates a new variable in a scope with the same name as a variable in the containing scope, such as:

```javascript
var count = 30;

// Does not throw an error
if (condition) {

    let count = 40;

    // more code
}
```

Here, the `let` declaration will not throw an error because it is creating a new variable called `count` within the `if` statement. This new variable shadows the global `count`, preventing access to it from within the `if` block.

> ### Global let Declarations
>
> There is the potential for naming collisions when using `let` in the global scope because the global object has predefined properties. Using `let` to define a variable that shares a name with a property of the global object can produce an error because global object properties may be nonconfigurable. Since `let` doesn't allow redefinition of the same identifier in the same scope, it's not possible to shadow nonconfigurable global properties. Attempting to do so will result in an error. For example:
>
>```javascript
>  let RegExp = "Hello!";          // ok
>  let undefined = "Hello!";       // throws error
>```
>
> The first line of this example redefines the global `RegExp` as a string. Even though this would be problematic, there is no error thrown. The second line throws an error because `undefined` is a nonconfigurable own property of the global object. Since its definition is locked down by the environment, the `let` declaration is illegal.
>
> It's unusual to use `let` in the global scope, but if you do, it's important to understand this situation.

The long term intent is for `let` to replace `var`, as the former behaves more like variable declarations in other languages. If you are writing JavaScript that will execute only in an ECMAScript 6 or higher environment, you may want to try using `let` exclusively and leaving `var` for other scripts that require backwards compatibility.

> Since `let` declarations are *not* hoisted to the top of the enclosing block, you may want to always place `let` declarations first in the block so that they are available to the entire block.

### Constant declarations

Another new way to define variables is to use the `const` declaration syntax. Variables declared using `const` are considered to be *constants*, so the value cannot be changed once set. For this reason, every `const` variable must be initialized. For example:

```javascript
// Valid constant
const MAX_ITEMS = 30;

// Syntax error: missing initialization
const NAME;
```

Constants are also block-level declarations, similar to `let`. That means constants are destroyed once execution flows out of the block in which they were declared, and declarations are not hoisted to the top of the block. For example:

```javascript
if (condition) {
    const MAX_ITEMS = 5;

    // more code
}

// MAX_ITEMS isn't accessible here
```

In this code, the constant `MAX_ITEMS` is declared within an `if` statement. Once the statement finishes executing, `MAX_ITEMS` is destroyed and is not accessible outside of that block.

Also similar to `let`, an error is thrown whenever a `const` declaration is made with an identifier for an already-defined variable in the same scope. It doesn't matter if that variable was declared using `var` (for global or function scope) or `let` (for block scope). For example:

```javascript
var message = "Hello!";
let age = 25;

// Each of these would cause an error given the previous declarations
const message = "Goodbye!";
const age = 30;
```

The big difference between `let` and `const` is that attempting to assign to a previously defined constant will throw an error in both strict and non-strict modes:

```javascript
const MAX_ITEMS = 5;

MAX_ITEMS = 6;      // throws error
```

> Several browsers implement pre-ECMAScript 6 versions of `const`. Implementations range from being simply a synonym for `var` (allowing the value to be overwritten) to actually defining constants but only in the global or function scope. For this reason, be especially careful with using `const` in a production system. It may not be providing you with the functionality you expect.

## Destructuring Assignment

JavaScript developers spend a lot of time pulling data out of objects and arrays. It's not uncommon to see code such as this:

```javascript
var options = {
        repeat: true,
        save: false
    };

// later

var localRepeat = options.repeat,
    localSave = options.save;
```

Frequently, object properties are stored into local variables for more succinct code and easier access. ECMAScript 6 makes this easy by introducing *destructuring assignment*, which systematically goes through an object or array and stores specified pieces of data into local variables.

> If the right side value of a destructuring assignment evaluates to `null` or `undefined`, an error is thrown.

### Object Destructuring

Object destructuring assignment syntax uses an object literal on the left side of an assignment operation. For example:

```javascript
var options = {
        repeat: true,
        save: false
    };

// later

var { repeat: localRepeat, save: localSave } = options;

console.log(localRepeat);       // true
console.log(localSave);         // false
```

In this code, the value of `options.repeat` is stored in a variable called `localRepeat` and the value of `options.save` is stored in a variable called `localSave`. These are both specified using the object literal syntax where the key is the property to find on `options` and the value is the variable in which to store the property value.

> If the property with the given name doesn't exist on the object, then the local variable gets a value of `undefined`.

If you want to use the property name as the local variable name, you can omit the colon and the identifier, such as:

```javascript
var options = {
        repeat: true,
        save: false
    };

// later

var { repeat, save } = options;

console.log(repeat);        // true
console.log(save);          // false
```

Here, two local variables called `repeat` and `save` are created. They are initialized with the value of `options.repeat` and `options.save`, respectively. This shorthand is helpful when there's no need to have different variable names.

Destructuring can also handle nested objects, such as the following:

```javascript
var options = {
        repeat: true,
        save: false,
        rules: {
            custom: 10,
        }
    };

// later

var { repeat, save, rules: { custom }} = options;

console.log(repeat);        // true
console.log(save);          // false
console.log(custom);        // 10
```

In this example, the `custom` property is embedded in another object. The extra set of curly braces allows you to descend into a nested object and pull out its properties.

> #### Syntax Gotcha
>
> If you try use destructuring assignment without a `var`, `let`, or `const`, you may be surprised by the result:
>
>```javascript
> // syntax error
> { repeat, save, rules: { custom }} = options;
>```
>
> This causes a syntax error because the opening curly brace is normally the beginning of a block and blocks can't be part of assignment expressions.
>
> The solution is to wrap the entire expression in parentheses:
>
>```javascript
> // no syntax error
> ({ repeat, save, rules: { custom }} = options);
>```
>
> This now works without any problems.

### Array Destructuring

Similarly, you can destructure arrays using array literal syntax on the left side of an assignment operation. For example:

```javascript
var colors = [ "red", "green", "blue" ];

// later

var [ firstColor, secondColor ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```

In this example, array destructuring pulls out the first and second values in the `colors` array. Keep in mind that the array itself isn't changed in any way.

Similar to object destructuring, you can also nest array destructuring. Just use another set of square brackets to descend into a subarray:

```javascript
var colors = [ "red", [ "green", "lightgreen" ], "blue" ];

// later

var [ firstColor, [ secondColor ] ] = colors;

console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```

Here, the `secondColor` variable refers to the `"green"` value inside of the `colors` array. That item is contained within a second array, so the extra square brackets around `secondColor` in the destructuring assignment is necessary.

### Mixed Destructuring

It's possible to mix objects and arrays together in a destructuring assignment expression using a mix of object and array literals. For example:

```javascript
var options = {
        repeat: true,
        save: false,
        colors: [ "red", "green", "blue" ]
    };

var { repeat, save, colors: [ firstColor, secondColor ]} = options;

console.log(repeat);            // true
console.log(save);              // false
console.log(firstColor);        // "red"
console.log(secondColor);       // "green"
```

This example extracts two property values, `repeat` and `save`, and then two items from the `colors` array, `firstColor` and `secondColor`. Of course, you could also choose to retrieve the entire array:

```javascript
var options = {
        repeat: true,
        save: false,
        colors: [ "red", "green", "blue" ]
    };

var { repeat, save, colors } = options;

console.log(repeat);                        // true
console.log(save);                          // false
console.log(colors);                        // "red,green,blue"
console.log(colors === options.colors);     // true
```

This modified example retrieves `options.colors` and stores it in the `colors` variable. Notice that `colors` is a direct reference to `options.colors` and not a copy.

Mixed destructuring is very useful for pulling values out of JSON configuration structures without navigating the entire structure.

## Numbers

JavaScript numbers can be particularly complex due to the dual usage of a single type for both integers and floats. Numbers are stored in the IEEE 754 double precision floating point format, and that same format is used to represent both types of numbers. As one of the foundational data types of JavaScript (along with strings and booleans), numbers are quite important to JavaScript developers. Given the new emphasis on gaming and graphics in JavaScript, ECMAScript 6 sought to make working with numbers easier and more powerful.

### Octal and Binary Literals

ECMAScript 5 sought to simplify some common numerical errors by removing the previously-included octal integer literal notation in two places: `parseInt()` and strict mode. In ECMAScript 3 and earlier, octal numbers were represented with a leading `0` followed by any number of digits. For example:

```javascript
// ECMAScript 3
var number = 071;       // 57 in decimal

var value1 = parseInt("71");    // 71
var value2 = parseInt("071");   // 57
```

Many developers were confused by this version of octal literal numbers, and many mistakes were made as a result of misunderstanding the effects of a leading zero in various places. The most egregious was in `parseInt()`, where a leading zero meant the value would be treated as an octal rather than a decimal. This led to one of Douglas Crockford's first JSLint rules: always use the second argument of `parseInt()` to specify how the string should be interpreted.

ECMAScript 5 cut down on the use of octal numbers. First, `parseInt()` was changed so that it ignores leading zeros in the first argument when there is no second argument. This means a number cannot accidentally be treated as octal anymore. The second change was to eliminate octal literal notation in strict mode. Attempting to use an octal literal in strict mode results in a syntax error.

```javascript
// ECMAScript 5
var number = 071;       // 57 in decimal

var value1 = parseInt("71");        // 71
var value2 = parseInt("071");       // 71
var value3 = parseInt("071", 8);    // 57

function getValue() {
    "use strict";
    return 071;     // syntax error
}
```

By making these two changes, ECMAScript 5 sought to eliminate a lot of the confusion and errors associated with octal literals.

ECMAScript 6 took things a step further by reintroducing an octal literal notation, along with a binary literal notation. Both of these notations take a hint from the hexadecimal literal notation of prepending `0x` or `0X` to a value. The new octal literal format begins with `0o` or `0O` while the new binary literal format begins with `0b` or `0B`. Each literal type must be followed by one or more digits, 0-7 for octal, 0-1 for binary. Here's an example:

```javascript
// ECMAScript 6
var value1 = 0o71;      // 57 in decimal
var value2 = 0b101;     // 5 in decimal
```

Adding these two literal types allows JavaScript developers to quickly and easily include numeric values in binary, octal, decimal, and hexadecimal formats, which is very important in certain types of mathematical operations.

The `parseInt()` method doesn't handle strings that look like octal or binary literals:

```javascript
console.log(parseInt("0o71"));      // 0
console.log(parseInt("0b101"));     // 0
```

However, the `Number()` function will convert a string containing octal or binary literals correctly:

```javascript
console.log(Number("0o71"));      // 57
console.log(Number("0b101"));     // 5
```

When using octal or binary literal in strings, be sure to understand your use case and use the most appropriate method for converting them into numeric values.

### isFinite() and isNaN()

JavaScript has long had a couple of global methods for identifying certain types of numbers:

* `isFinite()` determines if a value represents a finite number (not `Infinity` or `-Infinity`)
* `isNaN()` determines if a value is `NaN` (since `NaN` is the only value that is not equal to itself)

Although intended to work with numbers, these methods are capable of inferring a numeric value from any value that is passed in. That means both methods can return incorrect results when passed a value that isn't a number. For example:

```javascript
console.log(isFinite(25));      // true
console.log(isFinite("25"));    // true

console.log(isNaN(NaN));        // true
console.log(isNaN("NaN"));      // true
```

Both `isFinite()` and `isNaN()` pass their arguments through `Number()` to get a numeric value and then perform their comparisons on that numeric value rather than the original. This confusing outcome can lead to errors when value types are not checked before being used with one of these methods.

ECMAScript 6 adds two new methods that perform the same comparison but only for number values: `Number.isFinite()` and `Number.isNaN()`. These methods always return `false` when passed a non-number value and return the same values as their global counterparts when passed a number value:

```javascript
console.log(isFinite(25));              // true
console.log(isFinite("25"));            // true
console.log(Number.isFinite(25));       // true
console.log(Number.isFinite("25"));     // false

console.log(isNaN(NaN));                // true
console.log(isNaN("NaN"));              // true
console.log(Number.isNaN(NaN));         // true
console.log(Number.isNaN("NaN"));       // false
```

In this code, `Number.isFinite("25")` returns `false` even though `isFinite("25")` returns `true`; likewise `Number.isNaN("NaN")` returns `false` even though `isNaN("NaN")` returns `true`.

These two new methods are aimed at eliminating certain types of errors that can be caused when non-number values are used with `isFinite()` and `isNaN()` without dramatically changing the language.

### parseInt() and parseFloat()

The global functions `parseInt()` and `parseFloat()` now also reside at `Number.parseInt()` and `Number.parseFloat()`. These functions behave exactly the same as the global functions of the same name. The only purpose in making this move is to categorize purely global functions that clearly relate to a specific data type. Since these functions both create numbers from strings, they are now on `Number` along with the other functions that relate to numbers.

### Working with Integers

A lot of confusion has been caused over the years related to JavaScript's single number type that is used to represent both integers and floats. The language goes through great pains to ensure that developers don't need to worry about the details, but problems still leak through from time to time. ECMAScript 6 seeks to address this by making it easier to identify and work with integers.

#### Identifying Integers

The first addition is `Number.isInteger()`, which allows you to determine if a value represents an integer in JavaScript. Since integers and floats are stored differently, the JavaScript engine looks at the underlying representation of the value to make this determination. That means numbers that look like floats might actually be stored as integers and therefore return `true` from `Number.isInteger()`. For example:

```javascript
console.log(Number.isInteger(25));      // true
console.log(Number.isInteger(25.0));    // true
console.log(Number.isInteger(25.1));    // false
```

In this code, `Number.isInteger()` returns `true` for both `25` and `25.0` even though the latter looks like a float. Simply adding a decimal point to a number doesn't automatically make it a float in JavaScript. Since `25.0` is really just `25`, it is stored as an integer. The number `25.1`, however, is stored as a float because there is a fraction value.

#### Safe Integers

However, all is not so simple with integers. JavaScript can only accurately represent integers between -2^53^ and 2^53^, and outside of this "safe" range, binary representations end up reused for multiple numeric values. For example:

```javascript
console.log(Math.pow(2, 53));      // 9007199254740992
console.log(Math.pow(2, 53) + 1);  // 9007199254740992
```

This example doesn't contain a typo, two different numbers end up represented by the same JavaScript integer. The effect becomes more prevalent the further the value is outside of the safe range.

ECMAScript 6 introduces `Number.isSafeInteger()` to better identify integers that can accurately be represented in the language. There is also `Number.MAX_SAFE_INTEGER` and `Number.MIN_SAFE_INTEGER` that represent the upper and lower bounds of the same range, respectively. The `Number.isSafeInteger()` method ensures that a value is an integer and falls within the safe range of integer values:

```javascript
var inside = Number.MAX_SAFE_INTEGER,
    outside = inside + 1;

console.log(Number.isInteger(inside));          // true
console.log(Number.isSafeInteger(inside));      // true

console.log(Number.isInteger(outside));         // true
console.log(Number.isSafeInteger(outside));     // false
```

The number `inside` is the largest safe integer, so it returns `true` for both `Number.isInteger()` and `Number.isSafeInteger()`. The number `outside` is the first questionable integer value, so it is no longer considered safe even though it's still an integer.

Most of the time, you only want to deal with safe integers when doing integer arithmetic or comparisons in JavaScript, so it's a good idea to use `Number.isSafeInteger()` as part of input validation.

### New Math Methods

The aforementioned new emphasis on gaming and graphics in JavaScript led to the realization that many mathematical calculations could be done more efficiently by a JavaScript engine than with pure JavaScript code. Optimization strategies like asm.js, which works on a subset of JavaScript to improve performance, need more information to perform calculations in the fastest way possible. It's important, for instance, to know whether the numbers should be treated as 32-bit integers or as 64-bit floats.

As a result, ECMAScript 6 adds several new methods to the `Math` object. These new methods are important for improving the speed of common mathematical calculations, and therefore, improving the speed of applications that must perform many calculations (such as graphics programs). The new methods are listed below.

| Method | Description |
|--------|-------------|
|`Math.acosh(x)`| Returns the inverse hyperbolic cosine of `x`. |
|`Math.asinh(x)`| Returns the inverse hyperbolic sine of `x`. |
|`Math.atanh(x)`| Returns the inverse hyperbolic tangent of `x` |
|`Math.cbrt(x)`| Returns the cubed root of `x`. |
|`Math.clz32(x)`| Returns the number of leading zero bits in the 32-bit integer representation of `x`. |
|`Math.cosh(x)`| Returns the hyperbolic cosine of `x`. |
|`Math.expm1(x)`| Returns the result of subtracting 1 from the exponential function of `x`|
|`Math.fround(x)`| Returns the nearest single-precision float of `x`.|
|`Math.hypot(...values)`| Returns the square root of the sum of the squares of each argument. |
|`Math.imul(x, y)`| Returns the result of performing true 32-bit multiplication of the two arguments. |
|`Math.log1p(x)`| Returns the natural logarithm of `1 + x`. |
|`Math.log10(x)`| Returns the base 10 logarithm of `x`. |
|`Math.log2(x)`| Returns the base 2 logarithm of `x`. |
|`Math.sign(x)`| Returns -1 if the `x` is negative, 0 if `x` is +0 or -0, or 1 if `x` is positive.|
|`Math.sinh(x)`| Returns the hyperbolic sine of `x`. |
|`Math.tanh(x)`| Returns the hyperbolic tangent of `x`. |
|`Math.trunc(x)`| Removes fraction digits from a float and returns an integer.|

It's beyond the scope of this book to explain each new method and what it does in detail. However, if you are looking for a reasonably common calculation, be sure to check the new `Math` methods before implementing it yourself.

## Summary

ECMAScript 6 makes a lot of changes, both large and small, to JavaScript. Some of the smaller changes detailed in this chapter will likely be overlooked by many but they are just as important to the evolution of the language as the big changes.

Full Unicode support allows JavaScript to start dealing with UTF-16 characters in logical ways. The ability to transfer between code point and character via `codePointAt()` and `String.fromCodePoint()` is an important step for string manipulation. The addition of the regular expression `u` flag makes it possible to operate on code points instead of 16-bit characters, and the `normalize()` method allows for more appropriate string comparisons.

Additional methods for working with strings were added, allowing you to more easily identify substrings no matter where they are found, and more functionality was added to regular expressions. The `Object.is()` method performs strict equality on any value, effectively becoming a safer version of `===` when dealing with special JavaScript values.

The `let` and `const` block bindings introduce lexical scoping to JavaScript. These declarations are not hoisted and only exist within the block in which they are declared. That means behavior that is more like other languages and less likely to cause unintentional errors, as variables can now be declared exactly where they are needed. It's expected that the majority of JavaScript code going forward will use `let` and `const` exclusively, effectively making `var` a deprecated part of the language.

ECMAScript 6 makes it easier to work with numbers through the introduction of new syntax and new methods. The binary and octal literal forms allow you to embed numbers directly into source code while keeping the most appropriate representation visible. `Number.isFinite()` and `Number.isNaN()` are introduced as safer versions of their respective global methods. You can more easily identify integers using `Number.isInteger()` and `Number.isSafeInteger()`, as well as perform more mathematical operations thanks to new methods on `Math`.

Though many of these changes are small, they will make a significant difference in the lives of JavaScript developers for years to come. Each change addresses a particular concern that can otherwise require a lot of custom code to address. By building this functionality into the language, developers can focus on writing the code for their product rather than low-level utilities.
<!-- @section -->
# Functions

Functions are an important part of any programming language, and JavaScript functions hadn't changed much since the language was first introduced. This left a backlog of problems and nuanced behavior that made it easy to make mistakes or require more code just to achieve a very common behavior.

ECMAScript 6 functions made a big leap forward, taking into account years of complaints and asks from JavaScript developers. The result is a number of incremental improvements on top of ECMAScript 5 functions that make programming in JavaScript less error-prone and more powerful than ever before.

## Default Parameters

Functions in JavaScript are unique in that they allow any number of parameters to be passed regardless of the number of declared parameters in the function definition. This allows you to define functions that can handle different number of parameters, often by just filling in default values when ones aren't provided. In ECMAScript 5 and earlier, you would likely use the following pattern to accomplish this:

```javascript
function makeRequest(url, timeout, callback) {

    timeout = timeout || 2000;
    callback = callback || function() {};

    // the rest of the function

}
```

In this example, both `timeout` and `callback` are actually optional because they are given a default value if not provided. The logical OR operator (`||`) always returns the second operand when the first is falsy. Since named function parameters that are not explicitly provided are set to `undefined`, the logical OR operator is frequently used to provide default values for missing parameters. There is a flaw with this approach, however, in that a valid value for `timeout` might actually be `0`, but this would replace it with `2000` because `0` is falsy.

Other ways of determining if any parameters are missing include checking `arguments.length` for the number of parameters that were passed or directly inspecting each parameter to see if it is not `undefined`.

ECMAScript 6 makes it easier to provide default values for parameters by providing initializations that are used when the parameter isn't formally passed. For example:

```javascript
function makeRequest(url, timeout = 2000, callback = function() {}) {

    // the rest of the function

}
```

Here, only the first parameter is expected to be passed all the time. The other two parameters have default values, which makes the body of the function much smaller because you don't need to add any code to check for a missing value. When `makeRequest()` is called with all three parameters, then the defaults are not used. For example:

```javascript
// uses default timeout and callback
makeRequest("/foo");

// uses default callback
makeRequest("/foo", 500);

// doesn't use defaults
makeRequest("/foo", 500, function(body) {
    doSomething(body);
});
```

Any parameters with a default value are considered to be optional parameters while those without a default value are considered to be required parameters.

It's possible to specify default values for any arguments, including those that appear before arguments without default values. For example, this is fine:

```javascript
function makeRequest(url, timeout = 2000, callback) {

    // the rest of the function

}
```

In this case, the default value for `timeout` will only be used if there is no second argument passed in or if the second argument is explicitly passed in as `undefined`. For example:

```javascript
// uses default timeout
makeRequest("/foo", undefined, function(body) {
    doSomething(body);
});

// uses default timeout
makeRequest("/foo");

// doesn't use default timeout
makeRequest("/foo", null, function(body) {
    doSomething(body);
});
```

In the case of default parameter values, the value of `null` is considered to be valid and the default value will not be used.

Perhaps the most interesting feature of default parameter values is that the default value need not be a primitive value. You can, for example, execute a function to retrieve the default parameter:

```javascript
function getCallback() {
    return function() {
        // some code
    };
}

function makeRequest(url, timeout = 2000, callback = getCallback()) {

    // the rest of the function

}
```

Here, if the last argument isn't provided, the function `getCallback()` is called to retrieve the correct default value. This opens up a lot of interesting possibilities to dynamically inject information into functions.

## Rest Parameters

Since JavaScript functions can be passed any number of parameters, it's not always necessary to define each parameter specifically. Early on, JavaScript provided the `arguments` object as a way of inspecting all function parameters that were passed without necessarily defining each one individually. While that worked fine in most cases, it can become a little cumbersome to work with. For example:

```javascript
function pick(object) {
    let result = Object.create(null);

    for (let i = 1, len = arguments.length; i < len; i++) {
        result[arguments[i]] = object[arguments[i]];
    }

    return result;
}

let book = {
    title: "Understanding ECMAScript 6",
    author: "Nicholas C. Zakas",
    year: 2015
};

let bookData = pick(book, "author", "year");

console.log(bookData.author);   // "Nicholas C. Zakas"
console.log(bookData.year);     // 2015
```

This function mimics the `pick()` method from Underscore. The first argument is the object from which to copy properties and every other argument is the name of a property that should be copied on the result. There are couple of things to notice about this function. First, it's not at all obvious that the function is capable of handling more than one parameter. You could add in several more named parameters, but you would always fall short of indicating that this function can take any number of parameters. Second, because the first parameter is named and used directly, you have to start looking in the `arguments` object at index 1 instead of starting at index 0. Remembering to use the appropriate indices with `arguments` isn't necessarily difficult, but it's one more thing to keep track of. ECMAScript 6 introduces rest parameters to help with these issues.

Rest parameters are indicated by three dots (`...`) preceding a named parameter. That named parameter then becomes an `Array` containing the rest of the parameters (which is why these are called "rest" parameters). For example, `sum()` can be rewritten using rest parameters like this:

```javascript
function pick(object, ...keys) {
    let result = Object.create(null);

    for (let i = 0, len = keys.length; i < len; i++) {
        result[keys[i]] = object[keys[i]];
    }

    return result;
}
```

In this version of the function, `keys` is a rest parameter that contains all parameters after the first one (unlike `arguments`, which contains all parameters including the first one). That means you can iterate over `keys` from beginning to end without worry. As a bonus, you can tell by looking at the function that it is capable of handling any number of parameters.

The only restriction on rest parameters is that no other named arguments can follow in the function declaration. For example, this causes syntax error:

```javascript
// Syntax error: Can't have a named parameter after rest parameters
function pick(object, ...keys, last) {
    let result = Object.create(null);

    for (let i = 0, len = keys.length; i < len; i++) {
        result[keys[i]] = object[keys[i]];
    }

    return result;
}
```

Here, the parameter `last` follows the rest parameter `keys` and causes a syntax error.

Rest parameters were designed to replace `arguments` in ECMAScript. Originally ECMAScript 4 did away with `arguments` and added rest parameters to allow for an unlimited number of arguments to be passed to functions. Even though ECMAScript 4 never came into being, the idea was kept around and reintroduced in ECMAScript 6 despite `arguments` not being removed from the language.

## Destructured Parameters

In Chapter 1, you learned about destructuring assignment. Destructuring can also be used outside of the context of an assignment expression and perhaps the most interesting such case is with destructured parameters.

It's common for functions that take a large number of optional parameters to use an options object as one or more parameters. For example:

```javascript
function setCookie(name, value, options) {

    options = options || {};

    var secure = options.secure,
        path = options.path,
        domain = options.domain,
        expires = options.expires;

    // ...
}

setCookie("type", "js", {
    secure: true,
    expires: 60000
});
```

There are many `setCookie()` functions in JavaScript libraries that look similar to this. The `name` and `value` are required but everything else is not. And since there is no priority order for the other data, it makes sense to have an options object with named properties rather than extra named parameters. This approach is okay, although it makes the expected input for the function a bit opaque.

Using destructured parameters, the previous function can be rewritten as follows:

```javascript
function setCookie(name, value, { secure, path, domain, expires }) {

    // ...
}

setCookie("type", "js", {
    secure: true,
    expires: 60000
});
```

The behavior of this function is similar to the previous example, the biggest difference is the third argument uses destructuring to pull out the necessary data. Doing so makes it clear which parameters are really expected, and the destructured parameters also act like regular parameters in that they are set to `undefined` if they are not passed.

One quirk of this pattern is that the destructured parameters throw an error when the argument isn't provided. If `setCookie()` is called with just two arguments, it results in a runtime error:

```javascript
// Error!
setCookie("type", "js");
```

This code throws an error because the third argument is missing (`undefined`). To understand why this is an error, it helps to understand that destructured parameters are really just a shorthand for destructured assignment. The JavaScript engine is actually doing this:

```javascript
function setCookie(name, value, options) {

    var { secure, path, domain, expires } = options;

    // ...
}
```

Since destructuring assignment throws an error when the right side expression evaluates to `null` or `undefined`, the same is true when the third argument isn't passed.

You can work around this behavior by providing a default value for the destructured parameter:

```javascript
function setCookie(name, value, { secure, path, domain, expires } = {}) {

    // ...
}
```

This example now works exactly the same as the first example in this section. Providing the default value for the destructured parameter means that `secure`, `path`, `domain`, and `expires` will all be `undefined` if the third argument to `setCookie()` isn't provided.

> It's recommended to always provide the default value for destructured parameters to avoid these types of errors.

## The Spread Operator

Closely related to rest parameters is the spread operator. Whereas rest parameters allow you to specify that multiple independent arguments should be combined into an array, the spread operator allows you to specify an array that should be split and have its items passed in as separate arguments to a function. Consider the `Math.max()` method, which accepts any number of arguments and returns the one with the highest value. It's basic usage is as follows:

```javascript
let value1 = 25,
    value2 = 50;

console.log(Math.max(value1, value2));      // 50
```

When you're dealing with just two values, as in this example, `Math.max()` is very easy to use. The two values are passed in and the higher value is returned. But what if you have been tracking values in an array, and now you want to find the highest value? The `Math.max()` method doesn't allow you to pass in an array, so in ECMAScript 5 and earlier, you'd be stuck either searching the array yourself or using `apply()`:

```javascript
let values = [25, 50, 75, 100]

console.log(Math.max.apply(Math, values));  // 100
```

While possible, using `apply()` in this manner is a bit confusing - it actually seems to obfuscate the true meaning of the code with additional syntax.

The ECMAScript 6 spread operator makes this case very simple. Instead of calling `apply()`, you can pass in the array and prefix it with the same `...` pattern that is used with rest parameters. The JavaScript engine then splits up the array into individual arguments and passes them in:

```javascript
let values = [25, 50, 75, 100]

// equivalent to
// console.log(Math.max(25, 50, 75, 100));
console.log(Math.max(...values));           // 100
```

Now the call to `Math.max()` looks a bit more conventional and avoids the complexity of specifying a `this`-binding for a simple mathematical operation.

You can mix and match the spread operator with other arguments as well. Suppose you want the smallest number returned from `Math.max()` to be 0 (just in case negative numbers sneak into the array). You can pass that argument separately and still use the spread operator for the other arguments:

```javascript
let values = [-25, -50, -75, -100]

console.log(Math.max(...values, 0));        // 0
```

In this example, the last argument passed to `Math.max()` is `0`, which comes after the other arguments are passed in using the spread operator.

The spread operator for argument passing makes using arrays for function arguments much easier. You'll likely find it to be a suitable replacement for the `apply()` method in most circumstances.

## The name Property

Identifying functions can be challenging in JavaScript given the various ways a function can be defined. Additionally, the prevalence of anonymous function expressions makes debugging a bit more difficult, often resulting in stack traces that are hard to read and decipher. For these reasons, ECMAScript 6 adds the `name` property to all functions.

All functions in an ECMAScript 6 program will have an appropriate value for their `name` property while all others will have an empty string. For example:

```javascript
function doSomething() {
    // ...
}

var doAnotherThing = function() {
    // ...
};

console.log(doSomething.name);          // "doSomething"
console.log(doAnotherThing.name);       // "doAnotherThing"
```

In this code, `doSomething()` has a `name` property equal to `"doSomething"` because it's a function declaration. The anonymous function expression `doAnotherThing()` has a `name` of `"doAnotherThing"` due to the variable to which it is assigned.

While appropriate names for function declarations and function expressions are easy to find, ECMAScript 6 goes further to ensure that all functions have appropriate names:

```javascript
var doSomething = function doSomethingElse() {
    // ...
};

var person = {
    get firstName() {
        return "Nicholas"
    },
    sayName: function() {
        console.log(this.name);
    }
}

console.log(doSomething.name);      // "doSomethingElse"
console.log(person.sayName.name);   // "sayName"
console.log(person.firstName.name); // "get firstName"
```

In this example, `doSomething.name` is `"doSomethingElse"` because the function expression itself has a name and that name takes priority over the variable to which the function was assigned. The `name` property of `person.sayName()` is `"sayName"`, as the value was interpreted from the object literal. Similarly, `person.firstName` is actually a getter function, so its name is `"get firstName"` to indicate this difference (setter functions are prefixed with `"set"` as well).

There are a couple of other special cases for function names. Functions created using `bind()` will have their name prefixed with `"bound"` and functions created using the `Function` constructor have a name of `"anonymous"`:

```javascript
var doSomething = function() {
    // ...
};

console.log(doSomething.bind().name);   // "bound doSomething"

console.log((new Function()).name);     // "anonymous"
```

The `name` of a bound function will always be the `name` of the function being bound prefixed with the `"bound "`, so the bound version of `doSomething()` is `"bound doSomething"`.

## new.target, [[Call]], and [[Construct]]

In ECMAScript 5 and earlier, functions serve the double purpose of being callable with or without `new`. When used with `new`, the `this` value inside of a function is a new object and that new object is returned. For example:

```javascript
function Person(name) {
    this.name = name;
}

var person = new Person("Nicholas");
var notAPerson = Person("Nicholas");

console.log(person);        // "[Object object]"
console.log(notAPerson);    // "undefined"
```

Calling `Person()` without `new` results in `undefined` (and a `name` property being set on the global object in non-strict mode). It's fairly obvious from the code that the intent is to use `Person` with `new` to create a new object. The confusion over the dual roles of functions led to some changes in ECMAScript 6.

First, the specification defines two different internal-only methods that every function has: `[[Call]]` and `[[Construct]]`. When a function is called without `new`, the `[[Call]]` method is executed, which essentially executes the body of the function as it appears in the code. When a function is called with `new`, that's when the `[[Construct]]` method is called. The `[[Construct]]` method is responsible for creating a new object, called the *new target*, and then executing the function body with `this` set to the new target. Functions that have a `[[Construct]]` method are called *constructors*.

> Keep in mind that not all functions have `[[Construct]]`, and therefore not all function can be called with `new`. Arrow functions, discussed later in this chapter, do not have a `[[Construct]]` method.

The most popular way to determine if a function was called with `new` in ECMAScript 5 is to use `instanceof`, for example:

```javascript
function Person(name) {
    if (this instanceof Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person("Nicholas");  // throws error
```

Here, the `this` value is checked to see if it's an instance of the constructor, and if so, it continues as normal. If `this` isn't an instance of `Person`, then an error is thrown. This works because the `[[Construct]]` method creates a new instance of `Person` and assigns it to `this`. Unfortunately, this approach is not completely reliable because `this` can be an instance of `Person` without using `new`, for example:

```javascript
function Person(name) {
    if (this instanceof Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael");    // works!
```

The call to `Person.call()` passes the `person` variable as the first argument, which means `this` is set to `person` inside of the `Person` function. To the function, there's no way to distinguish this from being called with `new`.

To solve this problem, ECMAScript 6 introduces the `new.target` *metaproperty*. When a function's `[[Construct]]` method is called, `new.target` is filled with the target of the `new` operator, which is typically the constructor of the newly created object instance that will become `this` inside the function body. If `[[Call]]` is executed, then `new.target` is `undefined`. That means you can now safely detect if a function is called with `new` by checking that `new.target` is defined:

```javascript
function Person(name) {
    if (typeof new.target !== "undefined") {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael");    // error!
```

By using `new.target` instead of `this instanceof Person`, the `Person` constructor is now correctly throwing an error when used without `new`.

You can also check that `new.target` was called with a specific constructor, for instance:

```javascript
function Person(name) {
    if (typeof new.target === Person) {
        this.name = name;   // using new
    } else {
        throw new Error("You must use new with Person.")
    }
}

function AnotherPerson(name) {
    Person.call(this, name);
}

var person = new Person("Nicholas");
var anotherPerson = new AnotherPerson("Nicholas");  // error!
```

In this example, `new.target` must be `Person` in order to work correctly. When `new AnotherPerson("Nicholas")` is called, `new.target` is set to `AnotherPerson`, so the subsequent call to `Person.call(this, name)` will throw an error even though `new.target` is defined.

> Using `new.target` outside of a function is a syntax error.

## Block-Level Functions

In ECMAScript 3 and earlier, a function declaration occurring inside of a block (a *block-level function*) was technically a syntax error, but many browsers still supported it. Unfortunately, each browser that allowed the syntax behaved in a slightly different way, so it is considered a best practice to avoid function declarations inside of blocks (the best alternative is to use a function expression).

In an attempt to reign in this incompatible behavior, ECMAScript 5 strict mode introduced an error whenever a function declaration was used inside of a block. For example:

```javascript
"use strict";

if (true) {

    // Throws a syntax error in ES5, not so in ES6
    function doSomething() {
        // ...
    }
}
```

In ECMAScript 5, this code throws a syntax error. In ECMAScript 6, the `doSomething()` function is considered a block-level declaration and can be accessed and called within the same block in which it was defined. For example:

```javascript
"use strict";

if (true) {

    console.log(typeof doSomething);        // "function"

    function doSomething() {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);            // "undefined"
```

Block level functions are hoisted to the top of the block in which they are defined, so `typeof doSomething` returns `"function"` even though it appears before the function declaration in the code. Once the `if` block is finished executing, `doSomething()` no longer exists.

Block level functions are a similar to `let` function expressions in that the function definition is removed once execution flows out of the block in which it's defined. The key difference is that block level functions are hoisted to the top of the containing block while `let` function expressions are not hoisted. For example:

```javascript
"use strict";

if (true) {

    console.log(typeof doSomething);        // throws error

    let doSomething = function () {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);
```

Here, code execution stops when `typeof doSomething` is executed because the `let` statement hasn't been executed yet.

Whether you want to use block level functions or `let` expressions depends on whether or not you want the hoisting behavior.

ECMAScript 6 also allows block-level functions in nonstrict mode, but the behavior is slightly different. Instead of hoisting these declarations to the top of the block, they are hoisted all the way to the containing function or global environment. For example:

```javascript
// ECMAScript 6 behavior
if (true) {

    console.log(typeof doSomething);        // "function"

    function doSomething() {
        // ...
    }

    doSomething();
}

console.log(typeof doSomething);            // "function"
```

In this example, `doSomething()` is hoisted into the global scope so that it still exists outside of the `if` block. ECMAScript 6 standardized this behavior to remove the incompatible browser behaviors that previously existed. ECMAScript 6 runtimes will all behave in the same way.

## Arrow Functions

One of the most interesting new parts of ECMAScript 6 are arrow functions. Arrow functions are, as the name suggests, functions defined with a new syntax that uses an "arrow" (`=>`). However, arrow functions behave differently than traditional JavaScript functions in a number of important ways:

* **Lexical `this` binding** - The value of `this` inside of the function is determined by where the arrow function is defined not where it is used.
* **Not `new`able** - Arrow functions do not have a `[[Construct]]` method and therefore cannot be used as constructors. Arrow functions throw an error when used with `new`.
* **Can't change `this`** - The value of `this` inside of the function can't be changed, it remains the same value throughout the entire lifecycle of the function.
* **No `arguments` object** - You can't access arguments through the `arguments` object, you must use named arguments or other ES6 features such as rest arguments.

There are a few reasons why these differences exist. First and foremost, `this` binding is a common source of error in JavaScript. It's very easy to lose track of the `this` value inside of a function, which can result in unintended consequences. Second, by limiting arrow functions to simply executing code with a single `this` value, JavaScript engines can more easily optimize these operations (as opposed to regular functions, which might be used as a constructor or otherwise modified).

> Arrow functions also have a `name` property that follows the same rule as other functions.

## Syntax

The syntax for arrow functions comes in many flavors depending upon what you are trying to accomplish. All variations begin with function arguments, followed by the arrow, followed by the body of the function. Both the arguments and the body can take different forms depending on usage. For example, the following arrow function takes a single argument and simply returns it:

```javascript
var reflect = value => value;

// effectively equivalent to:

var reflect = function(value) {
    return value;
};
```

When there is only one argument for an arrow function, that one argument can be used directly without any further syntax. The arrow comes next and the expression to the right of the arrow is evaluated and returned. Even though there is no explicit `return` statement, this arrow function will return the first argument that is passed in.

If you are passing in more than one argument, then you must include parentheses around those arguments. For example:

```javascript
var sum = (num1, num2) => num1 + num2;

// effectively equivalent to:

var sum = function(num1, num2) {
    return num1 + num2;
};
```

The `sum()` function simply adds two arguments together and returns the result. The only difference is that the arguments are enclosed in parentheses with a comma separating them (same as traditional functions).

If there are no arguments to the function, then you must include an empty set of parentheses:

```javascript
var getName = () => "Nicholas";

// effectively equivalent to:

var getName = function() {
    return "Nicholas";
};
```

When you want to provide a more traditional function body, perhaps consisting of more than one expression, then you need to wrap the function body in braces and explicitly define a return value, such as:

```javascript
var sum = (num1, num2) => {
    return num1 + num2;
};

// effectively equivalent to:

var sum = function(num1, num2) {
    return num1 + num2;
};
```

You can more or less treat the inside of the curly braces as the same as in a traditional function with the exception that `arguments` is not available.

If you want to create a function that does nothing, then you need to include curly braces:

```javascript
var doNothing = () => {};

// effectively equivalent to:

var doNothing = function() {};
```

Because curly braces are used to denote the function's body, an arrow function that wants to return an object literal outside of a function body must wrap the literal in parentheses. For example:

```javascript
var getTempItem = id => ({ id: id, name: "Temp" });

// effectively equivalent to:

var getTempItem = function(id) {

    return {
        id: id,
        name: "Temp"
    };
};
```

Wrapping the object literal in parentheses signals that the braces are an object literal instead of the function body.

### Immediately-Invoked Function Expressions (IIFEs)

A popular use of functions in JavaScript is immediately-invoked function expressions (IIFEs). IIFEs allow you to define an anonymous function and call it immediately without saving a reference. This pattern comes in handy when you want to create a scope that is shielded from the rest of a program. For example:

```javascript
let person = function(name) {

    return {
        getName() {
            return name;
        }
    };

}("Nicholas");

console.log(person.getName());      // "Nicholas"
```

In this code, the IIFE is used to create an object with a `getName()` method. The method uses the `name` argument as the return value, effectively making `name` a private member of the returned object.

You can accomplish the same thing using arrow functions so long as you wrap the arrow function in parentheses:

```javascript
let person = ((name) => {

    return {
        getName() {
            return name;
        }
    };

})("Nicholas");

console.log(person.getName());      // "Nicholas"
```

Note that the location of the parentheses is around just the arrow function definition, and does not include `("Nicholas")`. This is different from a formal function, where the parentheses can be placed outside of the passed-in parameters as well as just as around the function definition.

### Lexical this Binding

One of the most common areas of error in JavaScript is the binding of `this` inside of functions. Since the value of `this` can change inside of a single function depending on the context in which it's called, it's possible to mistakenly affect one object when you meant to affect another. Consider the following example:

```javascript
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", function(event) {
            this.doSomething(event.type);     // error
        }, false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

In this code, the object `PageHandler` is designed to handle interactions on the page. The `init()` method is called to set up the interactions and that method in turn assigns an event handler to call `this.doSomething()`. However, this code doesn't work as intended. The call to `this.doSomething()` is broken because `this` is a reference to the element object (in this case `document`) that was the target of the event, instead of being bound to `PageHandler`. If you tried to run this code, you will get an error when the event handler fires because `this.doSomething()` doesn't exist on the target `document` object.

You can bind the value of `this` to `PageHandler` explicitly using the `bind()` method on the function:

```javascript
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click", (function(event) {
            this.doSomething(event.type);     // no error
        }).bind(this), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

Now the code works as expected, but may look a little bit strange. By calling `bind(this)`, you're actually creating a new function whose `this` is bound to the current `this`, which is `PageHandler`. The code now works as you would expect even though you had to create an extra function to get the job done.

Arrow functions have implicit `this` binding, which means that the value of `this` inside of an arrow function is always the same as the value of `this` in the scope in which the arrow function was defined. For example:

```javascript
var PageHandler = {

    id: "123456",

    init: function() {
        document.addEventListener("click",
                event => this.doSomething(event.type), false);
    },

    doSomething: function(type) {
        console.log("Handling " + type  + " for " + this.id);
    }
};
```

The event handler in this example is an arrow function that calls `this.doSomething()`. The value of `this` is the same as it is within `init()`, so this version of the example works similarly to the one using `bind()`. Even though the `doSomething()` method doesn't return a value, it is still the only statement executed necessary for the function body and so there is no need to include braces.

Arrow functions are designed to be "throwaway" functions and so cannot be used to define new types. This is evident by the missing `prototype` property that regular functions have. If you try to use the `new` operator with an arrow function, you'll get an error:

```javascript
var MyType = () => {},
    object = new MyType();  // error - you can't use arrow functions with 'new'
```

Also, since the `this` value is statically bound to the arrow function, you cannot change the value of `this` using `call()`, `apply()`, or `bind()`.

The concise syntax for arrow functions makes them ideal for use with array processing. For example, if you want to sort an array using a custom comparator, you typically write something like this:

```javascript
var result = values.sort(function(a, b) {
    return a - b;
});
```

That's a lot of syntax for a very simple procedure. Compare that to the more terse arrow function version:

```javascript
var result = values.sort((a, b) => a - b);
```

The array methods that accept callback functions such as `sort()`, `map()`, and `reduce()` all can benefit from simpler syntax with arrow functions to change what would appear to be more complex processes into simpler code.

Generally speaking, arrow functions are designed to be used in places where anonymous functions have traditionally been used. They are not really designed to be kept around for long periods of time, hence the inability to use arrow functions as constructors. Arrow functions are best used for callbacks that are passed into other functions, as seen in the examples in this section.

### Lexical arguments Binding

Even though arrow functions don't have their own `arguments` object, it's possible for them to access the `arguments` object from a containing function. That `arguments` object is then available no matter where the arrow function is executed later on. For example:

```javascript
function createArrowFunctionReturningFirstArg() {
    return () => arguments[0];
}

var arrowFunction = createArrowFunctionReturningFirstArg(5);

console.log(arrowFunction());       // 5
```

Inside of `createArrowFunctionReturningFirstArg()`, `arguments[0]` is referenced by the created arrow function. That reference contains the first argument passed to `createArrowFunctionReturningFirstArg()`. When the arrow function is later executed, it returns `5`, which was the first argument passed in to `createArrowFunctionReturningFirstArg()`. Even though the arrow function is no longer in the scope of the function that created it, `arguments` remains accessible as a lexical binding.

### Identifying Arrow Functions

Despite the different syntax, arrow functions are still functions and are identified as such:

```javascript
var comparator = (a, b) => a - b;

console.log(typeof comparator);                 // "function"
console.log(comparator instanceof Function);    // true
```

Both `typeof` and `instanceof` behave the same with arrow functions as they do with other functions.

Also like other functions, you can still use `call()`, `apply()`, and `bind()`, although the `this`-binding of the function will not be affected. Here are some examples:

```javascript
var sum = (num1, num2) => num1 + num2;

console.log(sum.call(null, 1, 2));      // 3
console.log(sum.apply(null, [1, 2]));   // 3

var boundSum = sum.bind(null, 1, 2);

console.log(boundSum());                // 3
```

In this example, the `sum()` function is called using `call()` and `apply()` to pass arguments as you would with any function. The `bind()` method is used to create `boundSum()`, which has its two arguments bound to `1` and `2` so that they don't need to be passed directly.

Arrow functions are appropriate to use anywhere you're currently using an anonymous function expression, such as with callbacks.

## Summary

Functions haven't undergone a huge change in ECMAScript 6, but rather, a series of incremental changes that make them easier to work with.

Default function parameters allow you to easily specify what value to use when a particular argument isn't passed. Prior to ECMAScript 6, this would require some extra code inside of the function to both check for the presence of arguments and assign a different value.

Rest parameters allow you to specify an array into which all remaining parameters should be placed. Using a real array and letting you indicate which parameters to include makes rest parameters a much more flexible solution than `arguments`.

Destructured parameters use the destructuring syntax to make options objects more transparent when used as function parameters. The actual data you're interested in can be listed out along with other named parameters.

The spread operator is a companion to rest parameters, allowing you to destructure an array into separate parameters when calling a function. Prior to ECMAScript 6, the only ways to pass individual parameters that were contained in an array were either manually specifying each parameter or using `apply()`. With the spread operator, you can easily pass an array to any function without worrying about the `this` binding of the function.

The addition of the `name` property helps to more easily identify functions for debugging and evaluation purposes. Additionally, ECMAScript 6 formally defines the behavior of block-level functions so they are no longer a syntax error in strict mode.

The behavior of a function has been defined by `[[Call]]`, normal function execution, and `[[Construct]]`, when a function is called with `new`. The `new.target` metaproperty allows you to determine if a function was called using `new` or not.

The biggest change to functions in ECMAScript 6 was the addition of arrow functions. Arrow functions are designed to be used in places where anonymous function expressions have traditionally been used. Arrow functions have a more concise syntax, lexical `this` binding, and no `arguments` object. Additionally, arrow functions can't change their `this` binding and so can't be used as constructors.
<!-- @section -->
# Objects

A lot of ECMAScript 6 focused on improving the utility of objects. The focus makes sense given that nearly every value in JavaScript is represented by some type of object. Additionally, the number of objects used in an average JavaScript program continues to increase, meaning that developers are writing more objects all the time. With more objects comes the necessity to use them more effectively.

ECMAScript 6 improves objects in a number of ways, from simple syntax to new ways of manipulating and interacting with objects.

## Object Categories

The ECMAScript 6 specification introduced some new terminology to help distinguish between categories of objects. JavaScript has long been littered with a mix of terminology used to describe objects found in the standard as opposed to those that are added by execution environments such as the browser. ECMAScript 6 takes the time to clearly define each category of object, and it's important to understand this terminology to have a good understanding of the language as a whole. The object categories are:

* *Ordinary objects* are objects that have all of the default internal behaviors for objects in JavaScript.
* *Exotic objects* are objects whose internal behavior is different than the default in some way.
* *Standard objects* are objects defined by ECMAScript 6, such as `Array`, `Date`, etc. Standard objects may be ordinary or exotic.
* *Built-in objects* are objects that are present in a JavaScript execution environment when a script begins to execute. All standard objects are built-in objects.

These terms are used throughout the book to explain the various objects defined by ECMAScript 6.

## Object Literal Extensions

One of the most popular patterns in JavaScript is the object literal. It's the syntax upon which JSON is built and can be seen in nearly every JavaScript file on the Internet. The reason for the popularity is clear: a succinct syntax for creating objects that otherwise would take several lines of code to accomplish. ECMAScript 6 recognized the popularity of the object literal and extends the syntax in several ways to make object literals more powerful and even more succinct.

### Property Initializer Shorthand

In ECMAScript 5 and earlier, object literals were simply collections of name-value pairs. That meant there could be some duplication when property values are being initialized. For example:

```javascript
function createPerson(name, age) {
    return {
        name: name,
        age: age
    };
}
```

The `createPerson()` function creates an object whose property names are the same as the function parameter names. The result is what appears to be duplication of `name` and `age` even though each represents a different aspect of the process.

In ECMAScript 6, you can eliminate the duplication that exists around property names and local variables by using the property initializer shorthand. When the property name is going to be the same as the local variable name, you can simply include the name without a colon and value. For example, `createPerson()` can be rewritten as follows:

```javascript
function createPerson(name, age) {
    return {
        name,
        age
    };
}
```

When a property in an object literal only has a name and no value, the JavaScript engine looks into the surrounding scope for a variable of the same name. If found, that value is assigned to the same name on the object literal. So in this example, the object literal property `name` is assigned the value of the local variable `name`.

The purpose of this extension is to make object literal initialization even more succinct than it already was. Assigning a property with the same name as a local variable is a very common pattern in JavaScript and so this extension is a welcome addition.

### Method Initializer Shorthand

ECMAScript 6 also improves syntax for assigning methods to object literals. In ECMAScript 5 and earlier, you must specify a name and then the full function definition to add a method to an object. For example:

```javascript
var person = {
    name: "Nicholas",
    sayName: function() {
        console.log(this.name);
    }
};
```

In ECMAScript 6, the syntax is made more succinct by eliminating the colon and the `function` keyword. You can then rewrite the previous example as:

```javascript
var person = {
    name: "Nicholas",
    sayName() {
        console.log(this.name);
    }
};
```

This shorthand syntax, also called *concise method* syntax, creates a method on the `person` object just as the previous example did. There is no difference aside from saving you some keystrokes, so `sayName()` is assigned an anonymous function expression and has all of the same characteristics as the function defined in the previous example.

> The `name` property of a method created using this shorthand is the name used before the parentheses. In the previous example, the `name` property for `person.sayName()` is `"sayName"`.

### Computed Property Names

JavaScript objects have long had computed property names through the use of square brackets instead of dot notation. The square brackets allow you to specify property names using variables and string literals that may contain characters that would be a syntax error if used in an identifier. For example:

```javascript
var person = {},
    lastName = "last name";

person["first name"] = "Nicholas";
person[lastName] = "Zakas";

console.log(person["first name"]);      // "Nicholas"
console.log(person[lastName]);          // "Zakas"
```

Both of the property names in this example have a space, making it impossible to reference those names using dot notation. However, bracket notation allows any string value to be used as a property name.

In ECMAScript 5, you could use string literals as property names in object literals, such as:

```javascript
var person = {
    "first name": "Nicholas"
};

console.log(person["first name"]);      // "Nicholas"
```

If you could provide the string literal inside of the object literal property definition then you were all set. If, however, the property name was contained in a variable or had to be calculated, then there was no way to define that property using an object literal.

ECMAScript 6 adds computed property names to object literal syntax by using the same square bracket notation that has been used to reference computed property names in object instances. For example:

```javascript
var lastName = "last name";

var person = {
    "first name": "Nicholas",
    [lastName]: "Zakas"
};

console.log(person["first name"]);      // "Nicholas"
console.log(person[lastName]);          // "Zakas"
```

The square brackets inside of the object literal indicate that the property name is computed, so its contents are evaluated as a string. That means you can also include expressions such as:

```javascript
var suffix = " name";

var person = {
    ["first" + suffix]: "Nicholas",
    ["last" + suffix]: "Zakas"
};

console.log(person["first name"]);      // "Nicholas"
console.log(person["last name"]);       // "Zakas"
```

Anything you would put inside of square brackets while using bracket notation on object instances will also work for computed property names inside of object literals.

## Object.assign()

One of the most popular patterns for object composition is *mixins*, in which one object receives properties and methods from another object. Many JavaScript libraries have a mixin method similar to this:

```javascript
function mixin(receiver, supplier) {
    Object.keys(supplier).forEach(function(key) {
        receiver[key] = supplier[key];
    });

    return receiver;
}
```

The `mixin()` function iterates over the own properties of `supplier` and copies them onto `receiver`. This allows the `receiver` to gain new behaviors without inheritance. For example:

```javascript
function EventTarget() { /*...*/ }
EventTarget.prototype = {
    constructor: EventTarget,
    emit: function() { /*...*/ },
    on: function() { /*...*/ }
};

var myObject = {};
mixin(myObject, EventTarget.prototype);

myObject.emit("somethingChanged");
```

In this example, `myObject` receives behavior from `EventTarget.prototype`. This gives `myObject` the ability to publish events and let others subscribe to them using `emit()` and `on()`, respectively.

This pattern became popular enough that ECMAScript 6 added `Object.assign()`, which behaves the same way. The difference in name is to reflect the actual operation that occurs. Since the `mixin()` method uses the assignment operator (`=`), it cannot copy accessor properties to the receiver as accessor properties. The name `Object.assign()` was chosen to reflect this distinction.

> Similar methods in various libraries may have other names. Some popular alternate names for the same basic functionality are `extend()` and `mix()`. There was also, briefly, an `Object.mixin()` method in ECMAScript 6 in addition to `Object.assign()`. The primary difference was that `Object.mixin()` also copied over accessor properties, but the method was removed due to concerns over the use of `super` (discussed later in this chapter).

You can use `Object.assign()` anywhere the `mixin()` function would have been used:

```javascript
function EventTarget() { /*...*/ }
EventTarget.prototype = {
    constructor: EventTarget,
    emit: function() { /*...*/ },
    on: function() { /*...*/ }
}

var myObject = {}
Object.assign(myObject, EventTarget.prototype);

myObject.emit("somethingChanged");
```

The `Object.assign()` method accepts any number of suppliers, and the receiver receives the properties in the order in which the suppliers are specified. That means the second supplier might overwrite a value from the first supplier on the receiver. For example:

```javascript
var receiver = {};

Object.assign(receiver, {
        type: "js",
        name: "file.js"
    }, {
        type: "css"
    }
);

console.log(receiver.type);     // "css"
console.log(receiver.name);     // "file.js"
```

The value of `receiver.type` is `"css"` because the second supplier overwrote the value of the first.

The `Object.assign()` method isn't a big addition to ECMAScript 6, but it does formalize a common function that is found in many JavaScript libraries.

> ### Working with Accessor Properties
>
> Keep in mind that you cannot create accessor properties on the receiver by using a supplier with accessor properties. Since `Object.assign()` uses the assignment operator, an accessor property on a supplier will become a data property on the receiver. For example:
>
> ```javascript
> var receiver = {},
>     supplier = {
>         get name() {
>             return "file.js"
>         }
>     };
>
> Object.assign(receiver, supplier);
>
> var descriptor = Object.getOwnPropertyDescriptor(receiver, "name");
>
> console.log(descriptor.value);      // "file.js"
> console.log(descriptor.get);        // undefined
> ```
>
> In this code, the `supplier` has an accessor property called `name`. After using `Object.assign()`, `receiver.name` exists as a data property with the value of `"file.js"`. That's because `supplier.name` returned "file.js" at the time `Object.assign()` was called.

## Duplicate Object Literal Properties

ECMAScript 5 strict mode introduced a check for duplicate object literal properties that would throw an error if a duplicate was found. For example:

```javascript
var person = {
    name: "Nicholas",
    name: "Greg"        // syntax error in ES5 strict mode
};
```

When running in ECMAScript 5 strict mode, this example results in a syntax error on the second `name` property.

In ECMAScript 6, the duplicate property check has been removed. Both strict and nonstrict mode code no longer check for duplicate properties and instead take the last property of the given name as the actual value.

```javascript
var person = {
    name: "Nicholas",
    name: "Greg"        // not an error in ES6
};

console.log(person.name);       // "Greg"
```

In this example, the value of `person.name` is `"Greg"` because that was the last value assigned to the property.

## Changing Prototypes

Prototypes are the foundation of inheritance in JavaScript, and so ECMAScript 6 continues to make prototypes more powerful. ECMAScript 5 added the `Object.getPrototypeOf()` method for retrieving the prototype of any given object. ECMAScript 6 adds the reverse operation, `Object.setPrototypeOf()`, which allows you to change the prototype of any given object.

Normally, the prototype of an object is specified at the time of its creation, either by using a constructor or via `Object.create()`. Prior to ECMAScript 6, there was no standard way to change an object's prototype after it had already been created. In a way, `Object.setPrototypeOf()` changes one of the biggest assumptions about objects in JavaScript to this point, which is that an object's prototype remains unchanged after creation.

The `Object.setPrototypeOf()` method accepts two arguments, the object whose prototype should be changed and the object that should become the first argument's prototype. For example:

```javascript
let person = {
    getGreeting() {
        return "Hello";
    }
};

let dog = {
    getGreeting() {
        return "Woof";
    }
};

// prototype is person
let friend = Object.create(person);
console.log(friend.getGreeting());                      // "Hello"
console.log(Object.getPrototypeOf(friend) === person);  // true

// set prototype to dog
Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting());                      // "Woof"
console.log(Object.getPrototypeOf(friend) === dog);     // true
```

This code defines two base objects: `person` and `dog`. Both objects have a method `getGreeting()` that returns a string. The object `friend` starts out inheriting from `person`, meaning that `getGreeting()` will output `"Hello"`. When the prototype is changed to be `dog` instead, `person.getGreeting()` outputs `"Woof"` because the original relationship to `person` is broken.

The actual value of an object's prototype is stored in an internal-only property called `[[Prototype]]`. The `Object.getPrototypeOf()` method returns the value stored in `[[Prototype]]` and `Object.setPrototypeOf()` changes the value stored in `[[Prototype]]`. However, these aren't the only ways to work with the value of `[[Prototype]]`.

Even before ECMAScript 5 was finished, several JavaScript engines already implemented a custom property called `__proto__` that could be used to both get and set the prototype of an object. Effectively, `__proto__` was an early precursor to both `Object.getPrototypeOf()` and `Object.setPrototypeOf()`. It was unrealistic to expect all of the JavaScript engines to remove this property, so ECMAScript 6 formalized the behavior of `__proto__`.

In ECMAScript 6 engines, `Object.prototype.__proto__` is defined as an accessor property whose `get` method calls `Object.getPrototypeOf()` and whose `set` method calls `Object.setPrototypeOf()`. This means that there is no real difference between using `__proto__` and the other methods except that `__proto__` allows you to set the prototype of an object literal directly. For example:

```javascript
let person = {
    getGreeting() {
        return "Hello";
    }
};

let dog = {
    getGreeting() {
        return "Woof";
    }
};

// prototype is person
let friend = {
    __proto__: person
};
console.log(friend.getGreeting());                      // "Hello"
console.log(Object.getPrototypeOf(friend) === person);  // true
console.log(friend.__proto__ === person);               // true

// set prototype to dog
friend.__proto__ = dog;
console.log(friend.getGreeting());                      // "Woof"
console.log(friend.__proto__ === dog);                  // true
console.log(Object.getPrototypeOf(friend) === dog);     // true
```

This example is functionally equivalent to the previous. The call to `Object.create()` has been replaced with an object literal that assigns a value to `__proto__`. The only real difference between creating an object using `Object.create()` or an object literal with `__proto__` is that the former requires you to specify full property descriptors for any additional object properties while the latter is just a standard object literal.

> The `__proto__` property is special in a number of ways:
>
> 1. You can only specify it once in an object literal. If you specify two `__proto__` properties, then an error is thrown. This is the only object literal property that has this restriction.
> 1. The computed form `["__proto__"]` acts like a regular property and does not set or return the current object's prototype. All rules related to object literal properties apply in this form, as opposed to the non-computed form, for which there are exceptions.
>
> It's best to be careful when using `__proto__` to make sure you don't get caught by these differences.

## Super References

As previously mentioned, prototypes are very important for JavaScript and a lot of work went into making them easier to use in ECMAScript 6. Among the improvements is the introduction of `super` references to more easily access functionality on an object's prototype. For example, if you want to override a method on an object instance such that it also calls the prototype method of the same name, you would need to do the following in ECMAScript 5:

```javascript
let person = {
    getGreeting() {
        return "Hello";
    }
};

let dog = {
    getGreeting() {
        return "Woof";
    }
};

// prototype is person
let friend = {
    __proto__: person,
    getGreeting() {
        // same as this.__proto__.getGreeting.call(this)
        return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
    }
};

console.log(friend.getGreeting());                      // "Hello, hi!"
console.log(Object.getPrototypeOf(friend) === person);  // true
console.log(friend.__proto__ === person);               // true

// set prototype to dog
friend.__proto__ = dog;
console.log(friend.getGreeting());                      // "Woof, hi!"
console.log(friend.__proto__ === dog);                  // true
console.log(Object.getPrototypeOf(friend) === dog);     // true
```

In this example, `getGreeting()` on `friend` calls the prototype method of the same name. The `Object.getPrototypeOf()` method is used to ensure the method is always getting the accurate prototype and then an additional string is appended. The additional `.call(this)` ensures that the `this` value inside the prototype method is set correctly.

Needing to remember to use `Object.getPrototypeOf()` and `.call(this)` to call a method on the prototype is a bit involved, so ECMAScript 6 introduced `super`.

At it's simplest, `super` acts as a pointer to the current object's prototype, effectively acting like `Object.getPrototypeOf(this)`. So you can simplify the `getGreeting()` method by rewriting it as:

```javascript
let friend = {
    __proto__: person,
    getGreeting() {
        // in the previous example, this is the same as:
        // 1. Object.getPrototypeOf(this).getGreeting.call(this)
        // 2. this.__proto__.getGreeting.call(this)
        return super.getGreeting() + ", hi!";
    }
};
```

The call to `super.getGreeting()` is the same as `Object.getPrototypeOf(this).getGreeting.call(this)` or `this.__proto__.getGreeting.call(this)`. Similarly, you can call any method on an object's prototype by using a `super` reference.

Where `super` is really powerful is when you have multiple levels of inheritance. In such a case, `Object.getPrototypeOf()` no longer works in all circumstances. For example:

```javascript
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    __proto__: person,
    getGreeting() {
        return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
    }
};

// prototype is friend
let relative = {
    __proto__: friend
};

console.log(person.getGreeting());                  // "Hello"
console.log(friend.getGreeting());                  // "Hello, hi!"
console.log(relative.getGreeting());                // error!
```

In this example, `Object.getPrototypeOf()` results in an error when `relative.getGreeting()` is called. That's because `this` is `relative`, and the prototype of `relative` is `friend`. When `friend.getGreeting().call()` is called with `relative` as `this`, the process starts over again and continues to call recursively until a stack overflow error occurs. This is a difficult problem to solve in ECMAScript 5, but with ECMAScript 6 and `super`, it's easy:

```javascript
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    __proto__: person,
    getGreeting() {
        return super.getGreeting() + ", hi!";
    }
};

// prototype is friend
let relative = {
    __proto__: friend
};

console.log(person.getGreeting());                  // "Hello"
console.log(friend.getGreeting());                  // "Hello, hi!"
console.log(relative.getGreeting());                // "Hello, hi!"
```

Because `super` references are not dynamic, they always refer to the correct object. In this case, `super.getGreeting()` always refers to `person.getGreeting()`, regardless of how many other object inherit this method.

> `super` references can only be used inside of concise methods and cannot be used in other functions or the global scope. Attempting to use `super` outside of concise methods results in a syntax error.

### Methods

Prior to ECMAScript 6, there was no formal definition of a "method" - methods were just object properties that contained functions instead of data. ECMAScript 6 formally defines a method as a function that has an internal `[[HomeObject]]` property containing the object to which the method belongs. Consider the following:

```javascript
let person = {

    // method
    getGreeting() {
        return "Hello";
    }
};

// not a method
function shareGreeting() {
    return "Hi!";
}
```

This example defines `person` with a single method called `getGreeting()`. The `[[HomeObject]]` for `getGreeting()` is `person` by virtue of assigning the function directly to an object. The `shareGreeting()` function, on the other hand, has no `[[HomeObject]]` specified because it wasn't assigned to an object when it was created. In most cases this difference isn't important, but it becomes very important when using `super`.

Any reference to `super` uses the `[[HomeObject]]` to determine what to do. The first step is to call `Object.getPrototypeOf()` on the `[[HomeObject]]` to retrieve a reference to the prototype. Then, the prototype is searched for a function with the same name as the executing function. Last, the `this`-binding is set and the method is called. If a function has no `[[HomeObject]]`, or has a different one than expected, then this process won't work. For example:

```javascript
let person = {
    getGreeting() {
        return "Hello";
    }
};

// prototype is person
let friend = {
    __proto__: person,
    getGreeting() {
        return super() + ", hi!";
    }
};

function getGlobalGreeting() {
    return super.getGreeting() + ", yo!";
}

console.log(friend.getGreeting());  // "Hello, hi!"

getGlobalGreeting();                      // throws error
```

Calling `friend.getGreeting()` returns a string while calling `getGlobalGreeting()` throws an error for improper use of `super`. Since the `getGlobalGreeting()` function has no `[[HomeObject]]`, it's not possible to perform a lookup. Interestingly, the situation doesn't change if `getGlobalGreeting()` is later assigned as a method on `friend`:

```javascript
// prototype is person
let friend = {
    __proto__: person,
    getGreeting() {
        return super() + ", hi!";
    }
};

function getGlobalGreeting() {
    return super.getGreeting() + ", yo!";
}

console.log(friend.getGreeting());  // "Hello, hi!"

// assign getGreeting to the global function
friend.getGreeting = getGlobalGreeting;

friend.getGreeting();               // throws error
```

Here the global `getGlobalGreeting()` function is used to overwrite the previously-defined `getGreeting()` method on `friend`. Calling `friend.getGreeting()` at that point results in an error as well. The value of `[[HomeObject]]` is only set when the function is first created, so even assigning onto an object doesn't fix the problem.

## Summary

Objects are at the center of programming in JavaScript, and ECMAScript 6 has made some helpful changes to objects that both make them easier to deal with and more powerful.

ECMAScript 6 makes several changes to object literals. Shorthand property definitions make it easier to assign properties whose names are the same as in-scope variables. Computed property names allow you to specify non-literal values as property names, which is something you've already been able to do in other areas of the language. Shorthand methods let you type a lot fewer characters in order to define methods on object literals by completely omitting the colon and `function` keyword. A loosening of the strict mode check for duplicate object literal property names was introduced as well, meaning you can now have two properties with the same name in a single object literal without an error being thrown.

The `Object.assign()` method makes it easier to change multiple properties on a single object at once. This can be very useful if you use the mixin pattern.

It's now possible to modify an object's prototype after it's already created using `Object.setPrototypeOf()`. ECMAScript 6 also defines the behavior of the `__proto__` property, which is an accessor property whose getter calls `Object.getPrototypeOf()` and whose setter calls `Object.setPrototypeOf()`.

The `super` keyword can now be used to call methods on an object's prototype. It can be used either standalone as a method, such as `super()`, or as a reference to the prototype itself, such as `super.getGreeting()`. In both cases, the `this`-binding is setup automatically to work with the current value of `this`.
<!-- @section -->
# Symbols

> This chapter is a work-in-progress. As such, it may have more typos or content errors than others.

ECMAScript 6 symbols began as a way to create private object members, a feature JavaScript developers have long wanted. The focus was around creating properties that were not identified by string names. Any property with a string name was easy picking to access regardless of the obscurity of the name. The initial "private names" feature aimed to create non-string property names. That way, normal techniques for detecting these private names wouldn't work.

The private names proposal eventually evolved into ECMAScript 6 symbols. While the implementation details remained the same (non-string values for property identifiers), TC-39 dropped the requirement that these properties be private. Instead, the properties would be categorized separately, being non-enumerable by default but still discoverable.

Symbols are actually a new kind of primitive value, joining strings, numbers, booleans, `null`, and `undefined`. They are unique among JavaScript primitives in that they do not have a literal form. The ECMAScript 6 standard uses a special notation to indicate symbols, prefixing the identifier with `@@`, such as `@@create`. This book uses this same convention for ease of understanding.

> Despite the notation, symbols do not exactly map to strings beginning with "@@". Don't try to use string values where symbols are required.

## Creating Symbols

You can create a symbol by using the `Symbol` function, such as:

```javascript
var firstName = Symbol();
var person = {};

person[firstName] = "Nicholas";
console.log(person[firstName]);     // "Nicholas"
```

In this example, the symbol `firstName` is created and used to assign a new property on `person`. That symbol must be used each time you want to access that same property. It's a good idea to name the symbol variable appropriately so you can easily tell what the symbol represents.

> Because symbols are primitive values, `new Symbol()` throws an error when called. It is possible to create an instance of `Symbol` via `new Object(yourSymbol)`, but it's unclear when this capability would be useful.

The `Symbol` function accepts an optional argument that is the description of the symbol. The description itself cannot be used to access the property but is used for debugging purposes. For example:

```javascript
var firstName = Symbol("first name");
var person = {};

person[firstName] = "Nicholas";

console.log("first name" in person);        // false
console.log(person[firstName]);             // "Nicholas"
console.log(firstName);                     // "Symbol(first name)"
```

A symbol's description is stored internally in a property called `[[Description]]`. This property is read whenever the symbol's `toString()` method is called either explicitly or implicitly (as in this example). It is not otherwise possible to access `[[Description]]` directly from code. It's recommended to always provide a description to make both reading and debugging code using symbols easier.

## Identifying Symbols

Since symbols are primitive values, you can use the `typeof` operator to identify them. ECMAScript 6 extends `typeof` to return `"symbol"` when used on a symbol. For example:

```javascript
var symbol = Symbol("test symbol");
console.log(typeof symbol);         // "symbol"
```

While there are other indirect ways of determining whether a variable is a symbol, `typeof` is the most accurate and preferred way of doing so.

## Using Symbols

You can use symbols anywhere you would use a computed property name. You've already seen the use of bracket notation in the previous sections, but you can use symbols in computed object literal property names as well as with `Object.defineProperty()`, and `Object.defineProperties()`, such as:

```javascript
var firstName = Symbol("first name");
var person = {
    [firstName]: "Nicholas"
};

// make the property read only
Object.defineProperty(person, firstName, { writable: false });

var lastName = Symbol("last name");

Object.defineProperties(person, {
    [lastName]: {
        value: "Zakas",
        writable: false
    }
});

console.log(person[firstName]);     // "Nicholas"
console.log(person[lastName]);      // "Zakas"
```

With computed property names in object literals, symbols are very easy to work with.

## Sharing Symbols

You may find that you want different parts of your code to use the same symbols. For example, suppose you have two different object types in your application that should use the same symbol property to represent a unique identifier. Keeping track of symbols across files or large codebases can be difficult and error-prone. That's why ECMAScript 6 provides a global symbol registry that you can access at any point in time.

When you want to create a symbol to be shared, use the `Symbol.for()` method instead of calling `Symbol()`. The `Symbol.for()` method accepts a single parameter, which is a string identifier for the symbol you want to create (this value is also used as the description). For example:

```javascript
var uid = Symbol.for("uid");
var object = {};

object[uid] = "12345";

console.log(object[uid]);       // "12345"
console.log(uid);               // "Symbol(uid)"
```

The `Symbol.for()` method first searches the global symbol registry to see if a symbol with the key `"uid"` exists. If so, then it returns the already existing symbol. If no such symbol exists, then a new symbol is created and registered into the global symbol registry using the specified key. The new symbol is then returned. That means subsequent calls to `Symbol.for()` using the same key will return the same symbol:

```javascript
var uid = Symbol.for("uid");
var object = {
    [uid]: "12345"
};

console.log(object[uid]);       // "12345"
console.log(uid);               // "Symbol(uid)"

var uid2 = Symbol.for("uid");

console.log(uid === uid2);      // true
console.log(object[uid2]);      // "12345"
console.log(uid2);              // "Symbol(uid)"
```

In this example, `uid` and `uid2` contain the same symbol and so they can be used interchangeably. The first call to `Symbol.for()` creates the symbol and second call retrieves the symbol from the global symbol registry.

Another unique aspect of shared symbols is that you can retrieve the key associated with a symbol in the global symbol registry by using `Symbol.keyFor()`, for example:

```javascript
var uid = Symbol.for("uid");
console.log(Symbol.keyFor(uid));    // "uid"

var uid2 = Symbol.for("uid");
console.log(Symbol.keyFor(uid2));   // "uid"

var uid3 = Symbol("uid");
console.log(Symbol.keyFor(uid3));   // undefined
```

Notice that both `uid` and `uid2` return the key `"uid"`. The symbol `uid3` doesn't exist in the global symbol registry, so it has no key associated with it and `Symbol.keyFor()` returns `undefined`.

> The global symbol registry is a shared environment, just like the global scope. That means you can't make assumptions about what is or is not already present in that environment. You should use namespacing of symbol keys to reduce the likelihood of naming collisions when using third-party components. For example, jQuery might prefix all keys with `"jquery."`, such as `"jquery.element"`.

## Finding Object Symbols

Similar to other properties on objects, you can access symbol properties using the `Object.getOwnPropertySymbols()` method. This method works exactly the same as `Object.getOwnPropertyNames()` except that the returned values are symbols rather than strings. Since symbols technically aren't property names, they are omitted from the result of `Object.getOwnPropertyNames()`.

The return value of `Object.getOwnPropertySymbols()` is an array of symbols that represent own properties. For example:

```javascript
var uid = Symbol.for("uid");
var object = {
    [uid]: "12345"
};

var symbols = Object.getOwnPropertySymbols(object);

console.log(symbols.length);        // 1
console.log(symbols[0]);            // "Symbol(uid)"
console.log(object[symbols[0]]);    // "12345"
```

In this code, `object` has a single symbol property. The array returned from `Object.getOwnPropertySymbols()` is an array containing just that symbol.

> All objects start off with zero own symbol properties (although they do have some inherited symbol properties).

## Coercing Symbols to Strings

TODO

String(symbol) works but symbol + "" throws

## Well-Known Symbols

In addition to the symbols you defined, there are some predefined symbols as well (called *well-known* symbols in the specification). These symbols represent common behaviors in JavaScript that were previously considered internal-only operations. Each well-known symbol is represented by a property on `Symbol`, such as `Symbol.create` for the `@@create` symbol.

A central theme for both ECMAScript 5 and ECMAScript 6 was exposing and defining some of the "magic" parts of JavaScript - the parts that couldn't be emulated by a developer. ECMAScript 6 follows this tradition by exposing even more of the previously internal-only logic of the language. It does so primarily through the use of symbol prototype properties that define the basic behavior of certain objects.

> Overwriting a method defined with a well-known symbol changes an ordinary object to an exotic object because this changes some internal default behavior.

The well-known symbols are:

* `@@hasInstance` - a method used by `instanceof` to determine an object's inheritance.
* `@@isConcatSpreadable` - a Boolean value indicating if use with `Array.prototype.concat()` should flatten the collection's elements.
* `@@iterator` - a method that returns an iterator (see Chapter 7).
* `@@match` - a method used by `String.prototype.match()` to compare strings.
* `@@replace` - a method used by `String.prototype.replace()` to replace substrings.
* `@@search` - a method used by `String.prototype.search()` to locate substrings.
* `@@species` - the constructor from which derived objects are made.
* `@@split` - a method used by `String.prototype.split()` to split up strings.
* `@@toPrimitive` - a method that returns a primitive value representation of the object.
* `@@toStringTag` - a string used by `Object.prototype.toString()` to create an object description.
* `@@unscopeables` - an object whose properties are the names of object properties that should not be included in a `with` statement.

Some of the well-known symbols are discussed below while others are discussed throughout the book to keep them in the correct context.

### @@toStringTag

One of the most interesting problems in JavaScript has been the availability of multiple global execution environments. This occurs in web browsers when a page includes an iframe, as the page and the iframe each have their own execution environments. In most cases, this isn't a problem, as data can be passed back and forth between the environments with little cause for concern. The problem arises when trying to identify what type of an object you're dealing with.

The canonical example of this is passing an array from the iframe into the containing page or vice-versa. Now in a different execution environment, `instanceof Array` returns `false` because the array was created with a constructor from a different environment.

Developers soon found a good way to identify arrays. It was discovered that by calling the standard `toString()` method on the object, a predictable string was always returned. Thus, many JavaScript libraries began including a function that works similar to this:

```javascript
function isArray(value) {
    return Object.prototype.toString.call(value) === "[object Array]";
}

console.log(isArray([]));   // true
```

This may look a bit roundabout, but in reality it was found to work quite well in all browsers. The `toString()` method on arrays isn't very useful for this purpose because it returns a string representation of the items it contains. The `toString()` method on `Object.prototype`, however, had this quirk where it included some internally-defined name in the result. By using this method on an object, you could retrieve what the JavaScript environment thought the data type was.

Developers quickly realized that since there was no way to change this behavior, it was possible to use the same approach to distinguish between native objects and those created by developers. The most important case of this was the ECMAScript 5 `JSON` object.

Prior to ECMAScript 5, many used Douglas Crockford's `json2.js`, which created a global `JSON` object. As browsers started to implement the `JSON` global object, it became necessary to tell whether the global `JSON` was provided by the JavaScript environment itself or through some other library. Using the same technique, many created functions like this:

```javascript
function supportsNativeJSON() {
    return typeof JSON !== "undefined" &&
        Object.prototype.toString.call(JSON) === "[object JSON]";
}
```

Here, the same characteristic that allowed developers to identify arrays across iframe boundaries also provided a way to tell if `JSON` was the native one or not. A non-native `JSON` object would return `[object Object]` while the native version returned `[object JSON]`. From that point on, this approach became the de facto standard for identifying native objects.

ECMAScript 6 explains this behavior through the `@@toStringTag` symbol. This symbol represents a property on each object that defines what value should be produced when `Object.prototype.toString.call()` is called on it. So the value returned for arrays is explained by having the `@@toStringTag` property equal `"Array"`. Likewise, you can define that value for your own objects:

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype[Symbol.toStringTag] = "Person";

var me = new Person("Nicholas");

console.log(me.toString());                         // "[object Person]"
console.log(Object.prototype.toString.call(me));    // "[object Person]"
```

In this example, a `@@toStringTag` property is defined on `Person.prototype` to provide the default behavior for creating a string representation. Since `Person.prototype` inherits `Object.prototype.toString()`, the value returned from `@@toStringTag` is also used when calling `me.toString()`. However, you can still define your own `toString()` that provides a different behavior without affecting the use of `Object.prototype.toString.call()`:

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype[Symbol.toStringTag] = "Person";

Person.prototype.toString = function() {
    return this.name;
};

var me = new Person("Nicholas");

console.log(me.toString());                         // "Nicholas"
console.log(Object.prototype.toString.call(me));    // "[object Person]"
```

This code defines `Person.prototype.toString()` to return the value of the `name` property. Since `Person` instances no longer inherit `Object.prototype.toString()`, calling `me.toString()` exhibits a different behavior.

> All objects inherit `@@toStringTag` from `Object.prototype` unless otherwise specified. This default property value is `"Object"`.

There is no restriction on which values can be used for `@@toStringTag` on developer-defined objects. For example, there's nothing preventing you from using `"Array"` as the value of `@@toStringTag`, such as:

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype[Symbol.toStringTag] = "Array";

Person.prototype.toString = function() {
    return this.name;
};

var me = new Person("Nicholas");

console.log(me.toString());                         // "Nicholas"
console.log(Object.prototype.toString.call(me));    // "[object Array]"
```

Here, the result of calling `Object.prototype.toString()` is `"[object Array]"`, which is the same as you would get from an actual array. This highlights the fact that `Object.prototype.toString()` is no longer a completely reliable way of identifying an object's type.

It's possible to change the string tag for native objects by assigning to `@@toStringTag` on their prototype. For example:

```javascript
Array.prototype[Symbol.toStringTag] = "Magic";

var values = [];

console.log(Object.prototype.toString.call(values));    // "[object Magic]"
```

Even though `@@toStringTag` is overwritten for arrays in this example, the call to `Object.prototype.toString()` results in `"[object Magic]"`. While it's recommended not to change built-in objects in this way, there's nothing in the language that forbids it.

### @@toPrimitive

JavaScript frequently attempts to convert objects into primitive values implicitly when certain operations are applied. For instance, when you compare a string to an object using double equals (`==`), the object is converted into a primitive value before comparing. Exactly what value should be used was previously an internal operation that is exposed in ECMAScript 6 through the `@@toPrimitive` method.

The `@@toPrimitive` method is defined on the prototype of each standard type and prescribes the exact behavior. When a primitive conversion is needed, `@@toPrimitive` is called with a single argument, referred to as `hint` in the specification. The `hint` argument is `"default"`, specifying that the operation has no preference as to the type, `"string"`, indicating a string should be returned, or `"number"`, if a number is necessary to perform the operation. Most standard objects treat `"default"` as equivalent to `"number"` (except for `Date`, which treats `"default"` as `"string"`).

TODO

### @@isConcatSpreadable

TODO

### @@species

TODO

### @@unscopeables

TODO

Only applied to `with` statement object records - does not refer to other scopes.


### @@hasInstance

TODO

## Summary

TODO
<!-- @section -->
# Classes

Ever since JavaScript was first created, many developers have been confused by its lack of classes. Most formal object-oriented programming languages support classes and classical inheritance as the primary way of defining similar and related objects. From pre-ECMAScript 1 all the way through ECMAScript 5, this point of confusion led many libraries to create utilities designed to make JavaScript look like it had support for classes.

While there are some JavaScript developers who feel strongly that the language doesn't need classes, the fact that so many libraries were created specifically for this purpose led to the inclusion of classes in ECMAScript 6. However, ECMAScript 6 classes aren't exactly the same as classes in other language. There's a uniqueness about them that embraces the dynamic of JavaScript as a language.

## Class-Like Structures in ECMAScript 5

Before exploring classes, it's helpful to understand the underlying mechanisms that classes use. In ECMAScript 5 and earlier, there were no classes, and the closest equivalent was creating a constructor and then assigning methods to its prototype. This approach is called creating a custom type. For example:

```javascript
function PersonType(name) {
    this.name = name;
}

PersonType.prototype.sayName = function() {
    console.log(this.name);
};

let person = new PersonType("Nicholas");
person.sayName();   // outputs "Nicholas"

console.log(person instanceof PersonType);  // true
console.log(person instanceof Object);      // true
```

In this code, `PersonType` is a constructor function that creates a single property called `name`. The `sayName()` method is assigned to the prototype so the same function is shared by all instances of `PersonType`. Then, a new instance of `PersonType` is created via the `new` operator, and the resulting `person` object is considered an instance of `PersonType` and of `Object` (through prototypal inheritance).

This same basic pattern underlies a lot of the class-mimicking JavaScript libraries. And that's where ECMAScript 6 classes start.

## Class Declarations

The simplest form of classes to understand is the one that looks similar to other languages: the class declaration. Class declarations begin with the `class` keyword followed by the name of the class. The rest of the syntax looks similar to concise methods in object literals without requiring commas between them. For example, here's the class equivalent of the previous example:

```javascript
class PersonClass {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
}

let person = new PersonClass("Nicholas");
person.sayName();   // outputs "Nicholas"

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // "function"
console.log(typeof PersonClass.prototype.sayName);  // "function"
```

The class declaration `PersonClass` behaves quite similarly to `PersonType` from the previous example. Instead of defining a function as the constructor, class declarations allow you to define the constructor directly inside of the class using the special `constructor` method name. Since class methods use the concise syntax, there's no need to use the `function` keyword. All other method names have no special meaning, so you can add as many as you want.

> Own properties, properties that occur on the instance rather than the prototype, can only be created inside of a class constructor or method. In the previous example, `name` is an own property. It's recommended to create all possible own properties inside of the constructor function so there's a single place that's responsible for all of them.

Perhaps the worst-kept secret in ECMAScript 6 is that class declarations such as this example are actually just syntactic sugar on top of the existing custom type declarations. The `PersonClass` declaration actually creates a function that has the behavior of the `constructor` method, which is why `typeof PersonClass` is `"function"`. Similarly, the `sayName()` method ends up as a method on `PersonClass.prototype`, similar to `PersonType.prototype` in the earlier example. These similarities allow you to mix custom types and classes without worry too much about which you're using.

Despite the similarities, there are some important differences to keep in mind:

1. Class declarations, unlike function declarations, are not hoisted. Class declarations act like `let` declarations and so exist in the temporal dead zone until execution reaches the declaration.
1. All code inside of class declarations runs in strict mode automatically. There's no way to opt-out of strict mode inside of classes.
1. All methods are non-enumerable. This is a significant change from custom types, where you need to use `Object.defineProperty()` to make a method non-enumerable.
1. Calling the class constructor without `new` throws an error.
1. Attempting to overwrite the class name within a class method throws an error.

With all of this in mind, the `PersonClass` declaration from the previous example is directly equivalent to the following:

```javascript
// direct equivalent of PersonClass
let PersonType2 = (function() {

    "use strict";

    const PersonType2 = function(name) {

        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.name = name;
    }

    Object.defineProperty(PersonType2.prototype, "sayName", {
        value: function() {
            console.log(this.name);
        },
        enumerable: false,
        writable: true,
        configurable: true
    });

    return PersonType2;
}());
```

While it was possible to do everything that classes do without adding new syntax, you can see how the class syntax makes all of the functionality a lot simpler than it would be otherwise.

> ### Constant Class Names
>
> The name of a class is specified as if using `const`, but only inside of the class itself. That means you can overwrite the class name outside of the class but not inside a class method. For example:
>
> ```javascript
> class Foo {
>    constructor() {
>        Foo = "bar";    // throws an error when executed
>    }
> }
>
>// but this is okay
> Foo = "baz";
> ```
>
> In this code, the `Foo` inside of the class constructor is a separate binding than the `Foo` outside of the class. The internal `Foo` is defined as if it's a `const` and so cannot be overwritten, which means and error is thrown when an attempt is made to do so; the external `Foo` is defined as if it's a `let` declaration and so its value may be overwritten at any time.

## Class Expressions

Classes and functions are similar in that they have two forms: declarations and expressions. Function and class declarations begin with an appropriate keyword (`function` or `class`, respectively) followed by an identifier. Functions have an expression form that doesn't require an identifier after `function`, and similarly, classes have an expression form that doesn't require an identifier after `class`.

These *class expressions* are designed to be used in variable declarations or passed into functions as arguments. Here's the class expression equivalent of the previous examples:

```javascript
// class expressions do not require identifiers after "class"
let PersonClass = class {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
};

let person = new PersonClass("Nicholas");
person.sayName();   // outputs "Nicholas"

console.log(person instanceof PersonClass);     // true
console.log(person instanceof Object);          // true

console.log(typeof PersonClass);                    // "function"
console.log(typeof PersonClass.prototype.sayName);  // "function"
```

Aside from the syntax, class expressions are exactly equivalent to class declarations. This example omits the identifier after `class`, but you can also include it:

```javascript
let PersonClass = class PersonClass2 {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }
};

console.log(PersonClass === PersonClass2);  // true
```

In this case, `PersonClass` and `PersonClass2` both reference the same class, and so they can be used interchangeably.

Class expressions can also be used in some interesting ways. For example, they can be passed into functions as arguments:

```javascript
function createObject(classDef) {
    return new classDef();
}

let obj = createObject(class {

    sayHi() {
        console.log("Hi!");
    }
});

obj.sayHi();        // "Hi!"
```

In this example, an anonymous class expression is passed into `createObject()`. An instance is then created by using `new` and that object is returned.

Another interesting use of class expressions is to create singletons by immediately invoking the class constructor. To do so, you must use `new` with a class expression and include parentheses at the end. For example:

```javascript
let person = new class {

    constructor(name) {
        this.name = name;
    }

    sayName() {
        console.log(this.name);
    }

}("Nicholas");

person.sayName();       // "Nicholas"
```

Here, the anonymous class expression is created and then executed immediately. This pattern allows you to use the class syntax for creating singletons without leaving a class reference available for inspection. The parentheses at the end are the indicator that you're calling a function while also allowing you to pass in an argument.

Whether you use class declarations or class expressions is purely a matter of style. Unlike function declarations and function expressions, both class declarations and class expressions are not hoisted, and so the choice has little bearing on the runtime behavior of the code.

## Accessor Properties

While own properties should be created inside of class constructors, classes allow you to define accessor properties on the prototype by using a syntax similar to that of object literal accessor format. To create a getter, use the keyword `get` followed by a space followed by an identifier; to create a setter, do the same using the keyword `set`. For example:

```javascript
class CustomHTMLElement {

    constructor(element) {
        this.element = element;
    }

    get html() {
        return this.element.innerHTML;
    }

    set html(value) {
        this.element.innerHTML = value;
    }
}

var descriptor = Object.getOwnPropertyDescriptor(CustomHTMLElement.prototype, "html");
console.log("get" in descriptor);   // true
console.log("set" in descriptor);   // true
console.log(descriptor.enumerable); // false
```

In this example, the `CustomHTMLElement` class is made as a simple wrapper around an existing DOM element. It has both a getter and setter for `html` that simply delegates to the `innerHTML` method on the element itself. This accessor property is created as non-enumerable, just like any other method would be, and is created on the `CustomHTMLElement.prototype`. The equivalent non-class representation is:

```
// direct equivalent to previous example
let CustomHTMLElement = (function() {

    "use strict";

    const CustomHTMLElement = function(element) {

        // make sure the function was called with new
        if (typeof new.target === "undefined") {
            throw new Error("Constructor must be called with new.");
        }

        this.element = element;
    }

    Object.defineProperty(CustomHTMLElement.prototype, "html", {
        enumerable: false,
        configurable: true,
        get: function() {
            return this.element.innerHTML;
        },
        set: function(value) {
            this.element.innerHTML = value;
        }
    });

    return CustomHTMLElement;
}());
```

As with previous examples, this one shows just how much code you're saving by using a class instead of the non-class equivalent. The accessor property definition alone is almost the size of the equivalent class declaration.

## Static Members

Another common pattern in JavaScript is adding addition methods directly onto constructors to simulate static members. For example:

```javascript
function PersonType(name) {
    this.name = name;
}

// static method
PersonType.create = function(name) {
    return new PersonType(name);
};

// instance method
PersonType.prototype.sayName = function() {
    console.log(this.name);
};

var person = PersonType.create("Nicholas");
```

This code creates a factory method called `PersonType.create()`. In other programming languages, this would be considered a static method as it is not dependent on an instance of `PersonType` for its data.

Classes simplify the creation of static members by using the formal `static` annotation before the method or accessor property name. Here's the equivalent of the last example:

```javascript
class PersonClass {

    // equivalent of the PersonType constructor
    constructor(name) {
        this.name = name;
    }

    // equivalent of PersonType.prototype.sayName
    sayName() {
        console.log(this.name);
    }

    // equivalent of PersonType.create
    static create(name) {
        return new PersonClass(name);
    }
}

let person = PersonClass.create("Nicholas");
```

The `PersonClass` definition defines a single static method called `create()` by adding the `static` keyword.

You can use the `static` keyword on any method or accessor property definition within a class. The only restriction is that you cannot use `static` with the `constructor` method definition.

> Just as with other class members, static members are not enumerable by default.

## Derived Classes

Another problem with custom types in ECMAScript 5 and earlier was the extensive process necessary to implement inheritance. To properly inherit, you would need multiple steps. For instance:

```javascript
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

function Square(length) {
    Rectangle.call(this, length, length);
}

Square.prototype = Object.create(Rectangle.prototype, {
    constructor: {
        value:Square,
        enumerable: true,
        writable: true,
        configurable: true
    }
});

var square = new Square(3);

console.log(square.getArea());              // 9
console.log(square instanceof Square);      // true
console.log(square instanceof Rectangle);   // true
```

Here, `Square` inherits from `Rectangle`, and to do so, it must be overwrite `Square.prototype` with a new object created from `Rectangle.prototype` as well as call `Rectangle.call()`. These steps often confused newcomers to the language and were a source of errors for experienced developers.

Derived classes use the `extends` keyword to specify the function from which the class should inherit. The prototypes are automatically adjusted and you can access the base class constructor using `super()`. Here's the equivalent of the previous example:

```javascript
class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }
}

class Square extends Rectangle {
    constructor(length) {

        // same as Rectangle.call(this, length, length)
        super(length, length);
    }
}

var square = new Square(3);

console.log(square.getArea());              // 9
console.log(square instanceof Square);      // true
console.log(square instanceof Rectangle);   // true
```

In this example, the `Square` class inherits from `Rectangle` using the `extends` keyword. The `Square` constructor uses `super()` to call the `Rectangle` constructor with the specified arguments. Note that unlike the ECMAScript 5 version of the code, the identifier `Rectangle` is only used within the class declaration (after `extends`).

Using `super()` is a requirement of derived classes if you specify a constructor (if you don't, an error will occur). If you choose not to use a constructor, then `super()` is automatically called for you with all arguments upon creating a new instance of the class. For instance, the following two classes are identical:

```javascript
class Square extends Rectangle {
    // no constructor
}

// Is equivalent to

class Square extends Rectangle {
    constructor(...args) {
        super(...args);
    }
}
```

The second class in this example shows the equivalent of the default constructor for all derived classes. All of the arguments are passed, in order, to the base class constructor. In this case, the functionality isn't quite correct because the `Square` constructor needs only one argument and so it's best to manually define the constructor.

> There are a few things to keep in mind when using `super()`:
>
> 1. You can only use `super()` in a derived class. If you try to use it in a non-derived class (a class that doesn't use `extends`) or a function, it will throw an error.
> 1. You must call `super()` before accessing `this` in the constructor. Since `super()` is responsible for initializing `this`, attempting to access `this` before calling `super()` results in an error.
> 1. The only way to avoid calling `super()` is to return an object from the class constructor.

### Class Methods

The methods on derived classes always shadow methods of the same name on the base class. For instance, you can add `getArea()` to `Square` in order to redefine that functionality:

```javascript
class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }

    // override and shadow Rectangle.prototype.getArea()
    getArea() {
        return this.length * this.length;
    }
}
```

In this code, `getArea()` is now defined as part of `Square` and therefore `Rectangle.prototype.getArea()` will no longer be called by any instances of `Square`. Of course, you can always decide to call the base class version of the method by using `super.getArea()`, such as:

```javascript
class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }

    // override, shadow, and call Rectangle.prototype.getArea()
    getArea() {
        return super.getArea();
    }
}
```

Using `super` in this way is the same as discussed in Chapter 3: the `this` value is automatically set correctly so you can make a simple method call.

Another interesting aspect of class methods is that they are missing the `[[Construct]]` internal method and therefore cannot be used with `new`. For example:

```javascript
// throws an error
var x = new Square.prototype.getArea();
```

Since class methods can't be called with `new`, you are prevented from accidentally calling these methods in a manner for which they were not intended.

Class methods can have computed names, just like computed names in object literals, by using square brackets around an expression. Here's an example:

```javascript
let methodName = "getArea";
class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }

    // override, shadow, and call Rectangle.prototype.getArea()
    [methodName]() {
        return super.getArea();
    }
}
```

This example is equivalent to the previous. The only difference is that a computed name is used for the `getArea()` method.

### Static Members

If a base class has static members then those static members are also available on the derived class. This maps to how inheritance works in other languages, but is a new concept for JavaScript. Here's an example:

```javascript
class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }

    getArea() {
        return this.length * this.width;
    }

    static create(length, width) {
        return new Rectangle(length, width);
    }
}

class Square extends Rectangle {
    constructor(length) {

        // same as Rectangle.call(this, length, length)
        super(length, length);
    }
}

var rect = Square.create(3, 4);

console.log(rect instanceof Rectangle);     // true
console.log(rect.getArea());                // 12
console.log(rect instanceof Square);        // false
```

In this code, a new static `create()` method is added to `Rectangle`. Through inheritance, that method is available as `Square.create()` and behaves in the same manner as `Rectangle.create()`.

### Derived Classes from Expressions

Perhaps the most powerful aspect of derived classes in ECMAScript 6 is the ability to derive a class from an expression. You can use `extends` with any expression, and if the expression resolves to a function with `[[Construct]]` and a prototype, the class will work. For example:

```javascript
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

class Square extends Rectangle {
    constructor(length) {
        super(length, length);
    }
}

var x = new Square(3);
console.log(x.getArea());               // 9
console.log(x instanceof Rectangle);    // true
```

This example defines `Rectangle` as an ECMAScript 5-style constructor while `Square` is a class. Since `Rectangle` has `[[Construct]]` and a prototype, the class can still inherit directly from it.

Accepting any type of expression after `extends` allows for some powerful possibilities, such as dynamically determining what to inherit from. For example:

```javascript
function Rectangle(length, width) {
    this.length = length;
    this.width = width;
}

Rectangle.prototype.getArea = function() {
    return this.length * this.width;
};

function getBase() {
    return Rectangle;
}

class Square extends getBase() {
    constructor(length) {
        super(length, length);
    }
}

var x = new Square(3);
console.log(x.getArea());               // 9
console.log(x instanceof Rectangle);    // true
```

Here, the `getBase()` function is called directly as part of the class declaration. It returns `Rectangle`, which means this example is functionally equivalent to the previous one. And since you can determine the base dynamically, that means it's possible to create different inheritance approaches. For instance, you can effectively create mixins:

```javascript
let SerializableMixin = {
    serialize() {
        return JSON.stringify(this);
    }
};

let AreaMixin = {
    getArea() {
        return this.length * this.width;
    }
};

function mixin(...mixins) {
    var base = function() {};
    Object.assign(base.prototype, ...mixins);
    return base;
}

class Square extends mixin(AreaMixin, SerializableMixin) {
    constructor(length) {
        super();
        this.length = length;
        this.width = length;
    }
}

var x = new Square(3);
console.log(x.getArea());               // 9
console.log(x.serialize());             // "{"length":3,"width":3}"
```

In this example, mixins are used instead of classical inheritance. The `mixin()` function takes any number of arguments that represent mixin objects. It creates a function called `base` and assigns the properties of each mixin object to the prototype. The function is then returned so `Square` can use `extends`. Keep in mind that since `extends` is still used, you are required to call `super()` in the constructor.

The instance of `Square` has both `getArea()` from `AreaMixin` and `serialize` from `SerializableMixin`. This is accomplished through prototypal inheritance, as the `mixin()` function dynamically populates the prototype of a new function with all of the own properties of each mixin.

> Even though any expression can be used after `extends`, not all expressions result in a valid class. Specifically, the following expression types causes errors:
>
> * `null`
> * generator functions (chapter 8)
>
> In these cases, attempting to create a new instance of the class will throw an error because there is `[[Construct]]` to call.

### Inheriting from Built-ins

For almost as long as there have been JavaScript arrays, developers have wanted to inherit from arrays to create their own special array types. However, in ECMAScript 5 and earlier, this wasn't possible. Here's an example:

```javascript
// built-in array behavior
var colors = [];
colors[0] = "red";
console.log(colors.length);         // 1

colors.length = 0;
console.log(colors[0]);             // undefined

// trying to inherit from array in ES5

function MyArray() {
    Array.apply(this, arguments);
}

MyArray.prototype = Object.create(Array.prototype, {
    constructor: {
        value: MyArray,
        writable: true,
        configurable: true,
        enumerable: true
    }
});

var colors = new MyArray();
colors[0] = "red";
console.log(colors.length);         // 0

colors.length = 0;
console.log(colors[0]);             // "red"
```

As you can see, using the classical form of JavaScript inheritance results in unexpected behavior. The `length` and numeric properties don't behave the same as they built-in array because this functionality isn't covered either by `Array.apply()` or by assigning the prototype.

One of the goals of ECMAScript 6 classes is to allow inheritance from all built-ins. In order to accomplish this, the inheritance model of classes is slightly different than the classical inheritance model found in ECMAScript 5 and earlier:

* In ECMAScript 5 classical inheritance, the value of `this` is first created by the derived type (for example, `MyArray`) and then the base type constructor is called (`Array.apply()`). That means `this` starts out as an instance of `MyArray` and then is decorated with additional properties from `Array`.
* In ECMAScript 6 class-based inheritance, the value of `this` is first created by the base (`Array`) and then modified by the derived class constructor (`MyArray`). The result is that `this` is starts out with all of the built-in functionality of the base and correctly receives all functionality related to it.

The following class-based special array works as you would expect:

```javascript
class MyArray extends Array {
    // ...
}

var colors = new MyArray();
colors[0] = "red";
console.log(colors.length);         // 1

colors.length = 0;
console.log(colors[0]);             // undefined
```

In this example, `MyArray` inherits directly from `Array` and therefore works in the exact same way. Interacting with numeric properties updates the `length` property, and manipulating the `length` property updates the numeric properties.

Additionally, `MyArray` inherits all static members from `Array`, which means that `MyArray.of()` already exists and can be used:

```javascript
class MyArray extends Array {
    // ...
}

var colors = MyArray.of(["red", "green", "blue"]);
console.log(colors instanceof MyArray);     // true
```

The inherited `MyArray.of()` behaves the same as `Array.of()` except that it creates an instance of `MyArray` rather than an instance of `Array`. Many built-in objects are specifically defined so that their static methods work appropriately for derived classes.

This same approach can be used to inherit from any of the built-in JavaScript objects, with a full guarantee that it will work the same way as the built-ins.

## new.target

In Chapter 2, you learned about `new.target` and how its value changes depending on how a function is called. You can also use `new.target` in class constructors to determine how the class is being invoked. In the simple case, `new.target` is equal to the constructor function for the class, as in this example:

```javascript
class Rectangle {
    constructor(length, width) {
        console.log(new.target === Rectangle);
        this.length = length;
        this.width = width;
    }
}

// new.target is Rectangle
var obj = new Rectangle(3, 4);      // outputs true
```

In this code, you can see that `new.target` is equivalent to `Rectangle` when `new Rectangle(3, 4)` is called. Since class constructors cannot be called without `new`, `new.target` is always defined inside of class constructors. However, the value may not always be the same.

```javascript
class Rectangle {
    constructor(length, width) {
        console.log(new.target === Rectangle);
        this.length = length;
        this.width = width;
    }
}

class Square extends Rectangle {
    constructor(length) {
        super(length, length)
    }
}

// new.target is Square
var obj = new Square(3);      // outputs false
```

Here, `Square` is calling the `Rectangle` constructor, so `new.target` is equal to `Square` when the `Rectangle` constructor is called. This is important because it gives each constructor the ability to alter its behavior based on how it's being called. For instance, you can create an abstract base class (one that cannot be instantiated directly) by using `new.target`:

```javascript
// abstract base class
class Shape {
    constructor() {
        if (new.target === Shape) {
            throw new Error("This class cannot be instantiated directly.")
        }
    }
}

class Rectangle extends Shape {
    constructor(length, width) {
        super();
        this.length = length;
        this.width = width;
    }
}

var x = new Shape();                // throws error

var y = new Rectangle(3, 4);        // no error
console.log(y instanceof Shape);    // true
```

In this example, the `Shape` class constructor throws an error whenever `new.target` is `Shape`, meaning that `new Shape()` always throws an error. However, you can still use it as a base class, which is what `Rectangle` does.

## Summary

For those who have struggled to understand JavaScript in the absence of classes, ECMAScript 6 classes provide an easier way to become acclimated with the language without needing to completely throw away their understanding of inheritance. ECMAScript 6 classes start out as syntatic sugar for the classical inheritance model of ECMAScript 5, but adds a lot of features to reduce mistakes.

ECMAScript 6 classes work with prototypal inheritance by defining non-static methods on the class prototype while static methods end up on the constructor itself. All methods are non-enumerable, which better matches the behavior of built-in objects for which methods are typically not enumerable by default. Additionally, class constructors cannot be called without `new`, ensuring that you can't accidentally call a class as a function.

Class-based inheritance allows you to derive a class from another class, function, or expression. This ability means you can call function to determine the correct base to inherit from, allowing you to use mixins and other different composition patterns to create a new class. Inheritance works in such a way that inheriting from built-in objects, such as `Array`, is now possible and works as expected.

You can use `new.target` in class constructors to behave differently depending on how the class is called. The most common use is to create an abstract base class that throws an error when instantiated directly but still allows inheritance via other classes.

Overall, classes are an important addition to the language that provides a more concise syntax and better functionality for defining custom object types in a safe, consistent manner.
<!-- @section -->
# Iterators and Generators

Iterators have been used in many programming languages as a way to more easily work with collections of data. In ECMAScript 6, JavaScript adds iterators as an important feature of the language. When coupled with new array methods and new types of collections (such as sets and maps), iterators become even more important for efficient processing of data.

## What are Iterators?

Iterators are nothing more than objects with a certain interface. That interface consists of a method called `next()` that returns a result object. The result object has two properties, `value`, which is the next value, and `done`, which is a boolean value that's `true` when there are no more values to return. The iterator keeps an internal pointer to a location within a collection of values and, with each call to `next()`, returns the next appropriate value.

If you call `next()` after the last value has been returned, the method returns `done` as `true` and `value` contains the return value for the iterator. The *return value* is not considered part of the data set, but rather a final piece of related data or `undefined` if no such data exists. (This concept will become clearer in the generators section later in this chapter.)

With that understanding, it's fairly easy to create an iterator using ECMAScript 5, for example:

```javascript
function createIterator(items) {

    var i = 0;

    return {
        next: function() {

            var done = (i >= items.length);
            var value = !done ? items[i++] : undefined;

            return {
                done: done,
                value: value
            };

        }
    };
}

var iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

The `createIterator()` function in this example returns an object with a `next()` method. Each time the method is called, the next value in the `items` array is returned as `value`. When `i` is 3, `items[i++]` returns `undefined` and `done` is `true`, which fulfills the special last case for iterators in ECMAScript 6.

ECMAScript 6 makes use of iterators in a number of places to make dealing with collections of data easier, so having a good basic understanding allows you to better understand the language as a whole.

## Generators

You might be thinking that iterators sound interesting but they look like a bunch of work. Indeed, writing iterators so that they adhere to the correct behavior is a bit difficult, which is why ECMAScript 6 provides generators. A *generator* is a special kind of function that returns an iterator. Generator functions are indicated by inserting a star character (`*`) after the `function` keyword (it doesn't matter if the star is directly next to `function` or if there's some whitespace between them). The `yield` keyword is used inside of generators to specify the values that the iterator should return when `next()` is called. So if you want to return three different values for each successive call to `next()`, you can do so as follows:

```javascript
// generator
function *createIterator() {
    yield 1;
    yield 2;
    yield 3;
}

// generators are called like regular functions but return an iterator
let iterator = createIterator();

for (let i of iterator) {
    console.log(i);
}
```

This code outputs the following:

```
1
2
3
```

In this example, the `createIterator()` function is a generator (as indicated by the `*` before the name) and it's called like any other function. The value returned is an object that adheres to the iterator pattern. Multiple `yield` statements inside the generator indicate the progression of values that should be returned when `next()` is called on the iterator. First, `next()` should return `1`, then `2`, and then `3` before the iterator is finished.

Perhaps the most interesting aspect of generator functions is that they stop execution after each `yield` statement, so `yield 1` executes and then the function doesn't execute anything else until the iterator's `next()` method is called. At that point, execution resumes with the next statement after `yield 1`, which in this case is `yield 2`. This ability to stop execution in the middle of a function is extremely powerful and lends to some interesting uses of generator functions (discussed later in this chapter).

The `yield` keyword can be used with any value or expression, so you can do interesting things like use `yield` inside of a `for` loop:

```javascript
function *createIterator(items) {
    for (let i=0; i < items.length; i++) {
        yield items[i];
    }
}

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this example, an array is used in a `for` loop, yielding each item as the loop progresses. Each time `yield` is encountered, the loop stops, and each time `next()` is called on `iterator`, the loop picks back up where it left off.

Generator functions are an important part of ECMAScript 6, and since they are just functions, they can be used in all the same places.

### Generator Function Expressions

Generators can be created using function expressions in the same way as using function declarations by including a star (`*`) character between the `function` keyword and the opening paren, for example:

```javascript
let createIterator = function *(items) {
    for (let i=0; i < items.length; i++) {
        yield items[i];
    }
};

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"

// for all further calls
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this code, `createIterator()` is created by using a generator function expression. This behaves exactly the same as the example in the previous section.

### Generator Object Methods

Because generators are just functions, they can be added to objects the same way as any other functions. For example, you can use an ECMAScript 5-style object literal with a function expression:

```javascript
var o = {

    createIterator: function *(items) {
        for (let i=0; i < items.length; i++) {
            yield items[i];
        }
    }
};

let iterator = o.createIterator([1, 2, 3]);
```

You can also use the ECMAScript 6 method shorthand by prepending the method name with a star (`*`):

```javascript
var o = {

    *createIterator(items) {
        for (let i=0; i < items.length; i++) {
            yield items[i];
        }
    }
};

let iterator = o.createIterator([1, 2, 3]);
```

This example is functionally equivalent to the previous one, the only difference is the syntax used.

### Generator Class Methods

Similar to objects, you can add generator methods directly to classes using almost the same syntax:

```javascript
class MyClass {

    *createIterator(items) {
        for (let i=0; i < items.length; i++) {
            yield items[i];
        }
    }

}

let o = new MyClass();
let iterator = o.createIterator([1, 2, 3]);
```

The syntax is very similar to using shorthand object literal methods, as the asterisk needs to come before the method name.

## Iterables and for-of

Closely related to the concept of an iterator is an iterable. An *iterable* is an object that has a default iterator specified using the `@@iterator` symbol. More specifically, `@@iterator` contains a function that returns an iterator for the given object. All of the collection objects, including arrays, sets, and maps, as well as strings, are iterables and so have a default iterator specified. Iterables are designed to be used with a new addition to ECMAScript: the `for-of` loop.

The `for-of` loop is similar to the other loops in ECMAScript except that it is designed to work with iterables. The loop itself calls `next()` behind the scenes and exits when the `done` property of the returned object is `true`. For example:

```javascript
let values = [1, 2, 3];

for (let i of values) {
    console.log(i);
}
```

This code outputs the following:

```
1
2
3
```

The `for-of` loop in this example is first calling the `@@iterator` method to retrieve an iterator, and then calling `iterator.next()` and assigning the variable `i` to the value returned on the `value` property. So `i` is first 1, then 2, and finally 3. When `done` is `true`, the loop exits, so `i` is never assigned the value of `undefined`.

> The `for-of` statement will throw an error when used on, a non-iterable, `null`, or `undefined`.

### Accessing the Default Iterator

You can access the default iterator for an object using `Symbol.iterator`, for example:

```javascript
let values = [1, 2, 3];
let iterator = values[Symbol.iterator]();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

This code gets the default iterator for `values` and uses that to iterate over the values in the array. Knowing that `Symbol.iterator` specifies the default iterator, it's possible to detect if an object is iterable by using the following:

```javascript
function isIterable(object) {
    return typeof object[Symbol.iterator] === "function";
}

console.log(isIterable([1, 2, 3]));     // true
console.log(isIterable("Hello"));       // true
console.log(isIterable(new Map()));     // true
console.log(isIterable(new Set()));     // true
```

The `isIterable()` function simply checks to see if a default iterator exists on the object and is a function. This is similar to the check that the `for-of` loop does before executing.

### Creating Iterables

Developer-defined objects are not iterable by default, but you can make them iterable by using the `@@iterator` symbol. For example:

```javascript
let collection = {
    items: [],
    *[Symbol.iterator]() {
        yield *this.items.values();
    }

};

collection.items.push(1);
collection.items.push(2);
collection.items.push(3);

for (let x of collection) {
    console.log(x);
}

// Output:
// 1
// 2
// 3
```

This code defines a default iterator for a variable called `collection` using object literal method shorthand and a computed property using `Symbol.iterator`. The generator then delegates to the `values()` iterator of `this.items` using the `yield *` notation, discussed later in this chapter. The `for-of` loop then uses the generator to create an iterator and execute the loop.

You can also define a default iterator using classes, such as:

```javascript
class Collection {

    constructor() {
        this.items = [];
    }

    *[Symbol.iterator]() {
        yield *this.items.values();
    }
}

var collection = new Collection();
collection.items.push(1);
collection.items.push(2);
collection.items.push(3);

for (let x of collection) {
    console.log(x);
}

// Output:
// 1
// 2
// 3
```

This example mirrors the previous one with the exception that a class is used instead of an object literal.

Default iterators can be added to any object by assigning a generator to `Symbol.iterator`. It doesn't matter if the property is an own or prototype property, as `for-of` normal prototype chain lookup applies.

## Built-in Iterators

Another way that ECMAScript 6 makes using iterators easier is by making iterators available on many objects by default. You don't actually need to create your own iterators for many of the built-in types because the language has them already. You only need to create iterators when you find that the built-in ones don't serve your purpose.

### Collection Iterators

The ECMAScript 6 collection objects, arrays, maps, and sets, all have three default iterators to help you navigate data. You can retrieve an iterator for a collection by calling one of these methods:

* `entries()` - returns an iterator whose values are a key-value pair.
* `values()` - returns an iterator whose values are the values of the collection.
* `keys()` - returns an iterator whose values are the keys contained in the collection.

The `entries()` iterator actually returns a two-item array where the first item is the key and the second item is the value. For arrays, the first item is the numeric index; for sets, the first item is also the value (since values double as keys in sets). Here are some examples:

```javascript
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

for (let entry of colors.entries()) {
    console.log(entry);
}

for (let entry of tracking.entries()) {
    console.log(entry);
}

for (let entry of data.entries()) {
    console.log(entry);
}
```

This example outputs the following:

```
[0, "red"]
[1, "green"]
[2, "blue"]
[1234, 1234]
[5678, 5678]
[9012, 9012]
["title", "Understanding ECMAScript 6"]
["format", "ebook"]
```

The `values()` iterator simply returns the values as they are stored in the collection. For example:

```javascript
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

for (let value of colors.values()) {
    console.log(value);
}

for (let value of tracking.values()) {
    console.log(value);
}

for (let value of data.values()) {
    console.log(value);
}
```

This example outputs the following:

```
"red"
"green"
"blue"
1234
5678
9012
"Understanding ECMAScript 6"
"ebook"
```

In this case, using `values()` returns the exact data contained in the `value` property returned from `next()`.

The `keys()` iterator returns each key present in the collection. For arrays, this is the numeric keys only (it never returns other own properties of the array); for sets, the keys are the same as the values and so `keys()` and `values()` return the same iterator.

```javascript
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

for (let key of colors.keys()) {
    console.log(key);
}

for (let key of tracking.keys()) {
    console.log(key);
}

for (let key of data.keys()) {
    console.log(key);
}
```

This example outputs the following:

```
0
1
2
1234
5678
9012
"title"
"format"
```

Additionally, each collection type has a default iterator that is used by `for-of` whenever an iterator isn't explicitly specified. The default iterator for arrays and sets is `values()` while the default iterator for maps is `entries()`. This makes it a little bit easier to use collection objects in `for-of`:

```javascript
let colors = [ "red", "green", "blue" ];
let tracking = new Set([1234, 5678, 9012]);
let data = new Map();

data.set("title", "Understanding ECMAScript 6");
data.set("format", "ebook");

// same as using colors.values()
for (let value of colors) {
    console.log(value);
}

// same as using tracking.values()
for (let num of tracking) {
    console.log(num);
}

// same as using data.entries()
for (let entry of data) {
    console.log(entry);
}
```

This example outputs the following:

```
"red"
"green"
"blue"
1234
5678
9012
["title", "Understanding ECMAScript 6"]
["format", "ebook"]
```

### String Iterators

Beginning with ECMAScript 5, JavaScript strings have slowly been evolving to be more array-like. ECMAScript 5 formalizes bracket notation for access characters (`text[0]` to get the first character). Unfortunately, bracket notation works on code units rather than characters, so it cannot be used to access double-byte characters correctly. ECMAScript 6 has added a lot of functionality to fully support Unicode (see Chapter 1) and as such, the default iterator for strings works on characters rather than code units.

Using bracket notation and the `length` property, the code units are used instead of characters and the output is a bit unexpected:


```javascript
var message = "A 𠮷 B";

for (let i=0; i < message.length; i++) {
    console.log(message[i]);
}
```

This code outputs the following:

```
A
(blank)
(blank)
(blank)
(blank)
B
```

Since the double-byte character is treated as two separate code units, there are four empty lines between `A` and `B` in the output.

Using the default string iterator with a `for-of` loop results in a more appropriate result:


```javascript
var message = "A 𠮷 B";

for (let c of message) {
    console.log(c);
}
```

This code outputs the following:

```
A
(blank)
𠮷
(blank)
B
```

This output is more in line with what you might expect when working with characters. The default string iterator is ECMAScript 6's attempt at solving the iteration problem by using characters instead of code units.

### NodeList Iterators

In the Document Object Model (DOM), there is a `NodeList` type that represents a collection of elements in a document. For those who write JavaScript to run in web browsers, understanding the difference between `NodeList` objects and arrays has always been a bit difficult. Both use the `length` property to indicate the number of items and both use bracket notation to access individual items. However, internally a `NodeList` and an array behave quite differently, and so that has led to a lot of confusion.

With the addition of default iterators in ECMAScript 6, the DOM definition of `NodeList` now specifically includes a default iterator that behaves in the same manner as the array default iterator. That means you can use `NodeList` in a `for-of` loop or any other place that uses an object's default iterator. For example:

```javascript
var divs = document.getElementsByTagName("div");

for (let div of divs) {
    console.log(div.id);
}
```

This code uses `getElementsByTagName()` method to retrieve a `NodeList` that represents all of the `<div>` elements in the document. The `for-of` loop then iterates over each element and outputs its ID, effectively making the code the same as it would be for a standard array.

## Advanced Functionality

There's a lot that can be accomplished with the basic functionality of iterators and the convenience of creating them using generators. However, developers have discovered that iterators are much more powerful when used for tasks other than simply iterating over a collection of values. During the development of ECMAScript 6, a lot of unique ideas and patterns emerged that caused the addition of more functionality. Some of the changes are subtle, but when used together, can accomplish some interesting interactions.

### Passing Arguments to Iterators

Throughout this chapter, you've seen that iterators can pass values out via the `next()` method or by using `yield` in a generator. It's also possible to pass arguments into the iterator through the `next()` method. When an argument is passed to `next()`, it becomes the value of the `yield` statement inside a generator. For example:

```javascript
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2;       // 4 + 2
    yield second + 3;                   // 5 + 3
}

let iterator = createIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next(4));          // "{ value: 6, done: false }"
console.log(iterator.next(5));          // "{ value: 8, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

The first call to `next()` is a special case where any argument passed to it is lost. Since arguments passed to `next()` become the value returned by `yield`, there would have to be a way to access that argument before the first `yield` in the generator function. That's not possible, so there's no reason to pass an argument the first time `next()` is called.

On the second call to `next()`, the value `4` is passed as the argument. The `4` ends up assigned to the variable `first` inside the generator function. In a `yield` statement including an assignment the right side of the expression is evaluated on the first call to `next()` and the left side is evaluated on the second call to `next()` before the function continues executing. Since the second call to `next()` passes in `4`, that value is assigned to `first` and then execution continues.

The second `yield` uses the result of the first `yield` and adds two, which means it returns a value of six. When `next()` is called a third time, the value `5` is passed as an argument. That value is assigned to the variable `second` and then used in the third `yield` statement to return eight.

It's a bit easier to think about what's happening by considering which code is executing each time execution continues inside the generator function. Figure 6-1 uses colors to show the code being executed before yielding.

![Figure 6-1: Code execution inside a generator](https://raw.githubusercontent.com/outlearn-content/nicholaszakas/master/assets/fg0601.png)

The color yellow represents the first call to `next()` and all of the code that is executed inside of the generator as a result; the color aqua represents the call to `next(4)` and the code that is executed; the color purple represents the call to `next(5)` and the code that is executed as a result. The tricky part is the code on the right side of each expression executing and stopping before the left side is executed. This makes debugging complicated generators a bit more involved than regular functions.

### Throwing Errors in Iterators

It's not only possible to pass data into iterators, it's also possible to pass error conditions. Iterators can choose to implement a `throw()` method that instructs the iterator to throw an error when it resumes. You can pass in an error object that should be thrown when the iterator continues processing. For example:

```javascript
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2;       // yield 4 + 2, then throw
    yield second + 3;                   // never is executed
}

let iterator = createIterator();

console.log(iterator.next());                   // "{ value: 1, done: false }"
console.log(iterator.next(4));                  // "{ value: 6, done: false }"
console.log(iterator.throw(new Error("Boom"))); // error thrown from generator
```

In this example, the first two `yield` expressions are evaluated as normal, but when `throw()` is called, an error is thrown before `let second` is evaluated. This effectively halts code execution similar to directly throwing an error. The only difference is the location in which the error is thrown. Figure 6-2 shows which code is executed at each step.

![Figure 6-2: Throwing an error inside a generator](https://raw.githubusercontent.com/outlearn-content/nicholaszakas/master/assets/fg0602.png)

In this figure, the color red represents the code executed when `throw()` is called and the red star shows approximately when the error is thrown inside the generator. The first two `yield` statements are evaluated fine, it's only when `throw()` is called that an error is thrown before any other code is executed. Knowing this, it's possible to catch such errors inside the generator using a `try-catch` block, such as:

```javascript
function *createIterator() {
    let first = yield 1;
    let second;

    try {
        second = yield first + 2;       // yield 4 + 2, then throw
    } catch (ex) {
        second = 6;                     // on error, assign a different value
    }
    yield second + 3;
}

let iterator = createIterator();

console.log(iterator.next());                   // "{ value: 1, done: false }"
console.log(iterator.next(4));                  // "{ value: 6, done: false }"
console.log(iterator.throw(new Error("Boom"))); // "{ value: 9, done: false }"
console.log(iterator.next());            // "{ value: undefined, done: true }"
```

In this example, a `try-catch` block is wrapped around the second `yield` statement. While this `yield` executes without error, the error is thrown before any value can be assigned to `second`, so the `catch` block assigns it a value of six. Execution then flows to the next `yield` and returns nine.

You'll also notice something interesting happened - the `throw()` method returned a value similar to that returned by `next()`. Because the error was caught inside the generator, code execution continued on to the next `yield` and returned the appropriate value.

It helps to think of `next()` and `throw()` as both being instructions to the iterator: `next()` instructs the iterator to continue executing (possibly with a given value) and `throw()` instructs the iterator to continue executing by throwing an error. What happens after that point depends on the code inside the generator.

### Generator Return Statements

Since generators are functions, you can use the `return` statement both to exit early and to specify a return value for the last call to `next()`. For most of this chapter you've seen examples where the last call to `next()` on an iterator returns `undefined`. It's possible to specify an alternate value by using `return` as you would in any other function. In a generator, `return` indicates that all processing is done, so the `done` property is set to `true` and the value, if provided, becomes the `value` field. Here's an example that simply exits early using `return`:

```javascript
function *createIterator() {
    yield 1;
    return;
    yield 2;
    yield 3;
}

let iterator = createIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this code, the generator has a `yield` statement followed by a `return` statement. The `return` indicates that there are no more values to come and so the rest of the `yield` statements will not execute (they are unreachable).

You can also specify a return value that will end up in the `value` field of the returned object. For example:

```javascript
function *createIterator() {
    yield 1;
    return 42;
}

let iterator = createIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 42, done: true }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

Here, the value `42` is returned in the `value` field on the second call to `next()` (which is the first time that `done` is `true`). The third call to `next()` returns an object whose `value` property is once again `undefined`. Any value you specify with `return` is only available on the returned object one time before the `value` field is reset to `undefined`.

> Any value specified by `return` is ignored by `for-of`.

### Delegating Generators

In some cases it may be useful to combine the values from two iterators into one. Using generators, it's possible to delegate to another generator using a special form of `yield` with a star (`*`). As with generator definitions, it doesn't matter where the star appears so as long as it is between the keyword `yield` and the generator function name. Here's an example:

```javascript
function *createNumberIterator() {
    yield 1;
    yield 2;
}

function *createColorIterator() {
    yield "red";
    yield "green";
}

function *createCombinedIterator() {
    yield *createNumberIterator();
    yield *createColorIterator();
    yield true;
}

var iterator = createCombinedIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: "red", done: false }"
console.log(iterator.next());           // "{ value: "green", done: false }"
console.log(iterator.next());           // "{ value: true, done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this example, the `createCombinedIterator()` generator delegates first to `createNumberIterator()` and then to `createColorIterator()`. The returned iterator appears, from the outside, to be one consistent iterator that has produced all of the values. Each call to `next()` is delegated to the appropriate iterator until they are empty, and then the final `yield` is executed to return `true`.

Generator delegation also lets you use make of generator return values (as seen in the previous section). This is the easiest way to access such returned values and can be quite useful in performing complex tasks. For example:

```javascript
function *createNumberIterator() {
    yield 1;
    yield 2;
    return 3;
}

function *createRepeatingIterator(count) {
    for (let i=0; i < count; i++) {
        yield "repeat";
    }
}

function *createCombinedIterator() {
    let result = yield *createNumberIterator();
    yield *createRepeatingIterator(result);
}

var iterator = createCombinedIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

Here, the `createCombinedIterator()` generator delegates to `createNumberIterator()` and assigns the return value to `result`. Since `createNumberIterator()` contains `return 3`, the returned value is `3`. The `result` variable is then passed to `createRepeatingIterator()` as an argument indicating how many times to yield the same string (in this case, three times).

Notice that the value `3` was never output from any call to `next()`, it existed solely inside of `createCombinedIterator()`. It is possible to output that value as well by adding another `yield` statement, such as:

```javascript
function *createNumberIterator() {
    yield 1;
    yield 2;
    return 3;
}

function *createRepeatingIterator(count) {
    for (let i=0; i < count; i++) {
        yield "repeat";
    }
}

function *createCombinedIterator() {
    let result = yield *createNumberIterator();
    yield result;
    yield *createRepeatingIterator(result);
}

var iterator = createCombinedIterator();

console.log(iterator.next());           // "{ value: 1, done: false }"
console.log(iterator.next());           // "{ value: 2, done: false }"
console.log(iterator.next());           // "{ value: 3, done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: "repeat", done: false }"
console.log(iterator.next());           // "{ value: undefined, done: true }"
```

In this code, the extra `yield` statement explicitly outputs the returned value from `createNumberIterator()`.

Generator delegation using the return value is a very powerful paradigm that allows for some very interesting possibilities, especially when used in conjunction with asynchronous operations.

> You can use `yield *` directly on strings, such as `yield * "hello"` and the string's default iterator will be used.

### Asynchronous Task Scheduling

A lot of the excitement around generators is directly related to usage with asynchronous programming. Asynchronous programming in JavaScript is a double-edged sword: it's very easy to do simple things while complex things become an errand in code organization. Since generators allow you to effectively pause code in the middle of execution, this opens up a lot of possibilities as it relates to asynchronous processing.

The traditional way to perform asynchronous operations is to call a function that has a callback. For example, consider reading a file from disk in Node.js:

```javascript
var fs = require("fs");

function readConfigFile(callback) {
    fs.readFile("config.json", callback);
}

function init(callback) {
    readConfigFile(function(err, contents) {
        if (err) {
            throw err;
        }

        doSomethingWith(contents);
        console.log("Done");
    });
}

init();
```

Instead of providing a callback, you can `yield` and just wait for a response before starting again:

```javascript
var fs = require("fs");

var task;

function readConfigFile() {
    fs.readFile("config.json", function(err, contents) {
        if (err) {
            task.throw(err);
        } else {
            task.next(contents);
        }
    });
}

function *init() {
    var contents = yield readConfigFile();
    doSomethingWith(contents);
    console.log("Done");
}

task = init();
task.next();
```

The difference between `init()` in this example and the previous one is why developers are excited about generators for asynchronous operation. Instead of using callbacks, `init()` yields to `readConfigFile()`, which does the asynchronous read operation and, when complete, either calls `throw()` if there's an error or `next()` if the contents have been ready. That means the `yield` operation inside of `init()` will throw an error if there's a read error or else the file contents will be returned almost as if the operation was synchronous.

Managing the `task` variable is a bit cumbersome in this example, but it's only important that you understand the theory. There are more powerful ways of doing asynchronous task scheduling using promises, and that will be covered further in Chapter 10.

## Summary

Iterators are an important part of ECMAScript 6 and are at the root of several important parts of the language. On the surface, iterators provide a simple way to return a sequence of values using a simple API. However, there are far more complex ways to use iterators in ECMAScript 6.

The `@@iterator` symbol is used to define default iterators for objects. Both built-in objects and developer-defined objects can use this symbol to provide a method that returns an iterator. When `@@iterator` is provided, the object is considered an iterable.

The `for-of` loop uses iterables to return a series of values in a loop. This makes creating loops easier than the traditional `for` loop because you no longer need to track values and control when the loop ends. The `for-of` loop automatically reads all values from the iterator until there are no more and then exits.

To make it easier to use `for-of`, many values in ECMAScript 6 have default iterators. All the collection types, arrays, maps, and sets, have iterators designed for easy access to their contents. Strings also have a default iterator so it's easy to iterate over the code points of the string (rather than the code units).

Generators are a special type of function that automatically creates an iterator when called. These functions are indicated by the start (`*`) and make use of the `yield` keyword to indicate which value to return for each successive call to `next()`.

Generator delegation encourages good encapsulation of iterator behavior by letting you reuse existing generators in new ones. This is done using `yield *` instead of `yield`, allowing you to create an iterator that returns values from multiple iterators.

Perhaps the most interesting and exciting aspect of generators and iterators is the possibility of creating cleaner-looking asynchronous code. Instead of needing to use callbacks everywhere, you can setup code that looks synchronous but in fact uses `yield` to wait for asynchronous operations to complete.
<!-- @section -->
# Promises

One of the most powerful aspects of JavaScript is how easy it handles asynchronous programming. Since JavaScript originated as a language for the web, it was a requirement to be able to respond to user interactions such as clicks and key presses. Node.js further popularized asynchronous programming in JavaScript by using callbacks as an alternative to events. As more and more programs started using asynchronous programming, there was a growing sense that these two models, events and callbacks, weren't powerful enough to support everything that developers wanted to do. Promises are the solution to this problem.

Promises are another option for asynchronous programming, and similar functionality is available in other languages under names such as futures and deferreds. The basic idea is to specify some code to be executed later (as with events and callbacks) and also explicitly indicate if the code succeeded or failed in its job. In that way, you can chain promises together based on success or failure in ways that are easier to understand and debug.

Before you can get a good understanding of how promises work, however, it's important to understand some of the basic concepts upon which they are built.

## Asynchronous Programming Background

JavaScript engines are built on the concept of a single-threaded event loop. Single-threaded means that only one piece of code is executed at any given in point in time. This stands in contrast to other languages such as Java or C++ that may use threads to allow multiple different pieces of code to execute at the same time. Maintaining, and protecting, state when multiple pieces of code can access and change that state is a difficult problem and the source of frequent bugs in thread-based software.

Because JavaScript engines can only execute one piece of code at a time, it's necessary to keep track of code that is meant to run. That code is kept in a *job queue*. Whenever a piece of code is ready to be executed, it is added to the job queue. When the JavaScript engine is finished executing code, the event loop picks the next job in the queue and executes it. The *event loop* is a process inside of the JavaScript engine that monitors code execution and manages the job queue. Keep in mind that as a queue, job execution runs from the first job in the queue to the last.

### Events

When a user clicks a button or presses key on the keyboard, an *event* is triggered (such as `onclick`). That event may be used to respond to the interaction by adding a new job to the back of the job queue. This is the most basic form of asynchronous programming JavaScript has: the event handler code doesn't execute until the event fires, and when it does execute, it has the appropriate context. For example:

```javascript
let button = document.getElementById("my-btn");
button.onclick = function(event) {
    console.log("Clicked");
};
```

In this code, `console.log("Clicked")` will not be executed until `button` is clicked. When `button` is clicked, the function assigned to `onclick` is added to the back of the job queue and will be executed when all other jobs ahead of it are complete.

Events work well for simple interactions such as this, but chaining multiple separate asynchronous calls together becomes more complicated because you must keep track of the event target (`button` in the previous example) for each event. Additionally, you need to ensure all appropriate event handlers are added before the first instance of an event occurs. For instance, if `button` in the previous example was clicked before `onclick` is assigned, then nothing will happen.

So while events are useful for responding to user interactions and similar functionality that occurs infrequently, they aren't very flexible for more complex needs.

### Callbacks

When Node.js was created, it furthered the asynchronous programming model by popularizing the callback pattern. The callback pattern is similar to the event model because it doesn't execute code until a later point in time; it is different because the function to call is passed in as an argument. For example:

```javascript
readFile("example.txt", function(err, contents) {
    if (err) {
        throw err;
    }

    console.log(contents);
});
console.log("Hi!");
```

This example uses the traditional Node.js style of error-first callback. The `readFile()` function is intended to read from a file on disk (specified as the first argument) and then execute the callback (the second argument) when complete. If there's an error, the `err` argument of the callback is an error object; otherwise, the `contents` argument contains the file contents as a string.

Using the callback pattern, `readFile()` begins executing immediately and pauses when it begins reading from the disk. That means `console.log("Hi!")` is output immediately after `readFile()` is called (before `console.log(contents)`). When `readFile()` has finished, it adds a new job to the end of the job queue with the callback function and its arguments. That job is then executed upon completion of all other jobs ahead of it.

The callback pattern is more flexible than events because it is easier to chain multiple calls together. For example:

```javascript
readFile("example.txt", function(err, contents) {
    if (err) {
        throw err;
    }

    writeFile("example.txt", function(err) {
        if (err) {
            throw err;
        }

        console.log("File was written!");
    });
});
```

In this code, a successful call to `readFile()` results in another asynchronous call, this time to `writeFile()`. Note that the same basic pattern of checking `err` is present in both functions. When `readFile()` is complete, it adds a job to the job queue that results in `writeFile()` being called (assuming no errors). Then, `writeFile()` adds a job to the job queue when it is complete.

While this works fairly well, you can quickly get into a pattern that has come to be known as *callback hell*. Callback hell occurs when you nest too many callbacks:

```javascript
method1(function(err, result) {

    if (err) {
        throw err;
    }

    method2(function(err, result) {

        if (err) {
            throw err;
        }

        method3(function(err, result) {

            if (err) {
                throw err;
            }

            method4(function(err, result) {

                if (err) {
                    throw err;
                }

                method5(result);
            });

        });

    });

});
```

Nesting multiple method calls, as in this example, creates a tangled web of code that is hard to understand and debug.

Callbacks also present problems when you want to accomplish more complex functionality. What if you'd like two asynchronous operations to run in parallel and be notified when they both are complete? What if you'd like to kick off two asynchronous operations but only take the first one to complete? In these cases, you end needing to keep track of multiple callbacks and cleanup operations. This is precisely where promises greatly improve the situation.

## Promise Basics

A promise is a placeholder for the result of an asynchronous operation. Instead of subscribing to an event or passing a callback to a function, the function can return a promise, such as:

```javascript
// readFile promises to complete at some point in the future
let promise = readFile("example.txt");
```

In this code, `readFile()` doesn't actually start reading the file immediately (that will happen later). It returns a promise object that represents the asynchronous operation so you can work with it later.

### Lifecycle

Each promise goes through a short lifecycle. It starts in the *pending* state, which is an indicator that the asynchronous operation has not yet completed. The promise in the last example is in the pending state as soon as it is returned from `readFile()`. Once the asynchronous operation completes, the promise is considered *settled* and enters one of two possible states:

1. *Fulfilled* - the promise's asynchronous operation has completed successfully
1. *Rejected* - the promise's asynchronous operation did not complete successfully (either due to error or some other cause)

You can't determine which state the promise is in programmatically, but you can take a specific action when a promise changes state by using the `then()` method.

> There is an internal `[[PromiseState]]` property that is set to `"pending"`, `"fulfilled"`, or `"rejected"` to reflect the promise's state.

The `then()` method is present on all promises and takes two arguments (any object that implements `then()` is called a *thenable*). The first argument is a function to call when the promise is fulfilled. Any additional data related to the asynchronous operation is passed into this fulfillment function. The second argument is a function to call when the promise is rejected. Similar to the fulfillment function, the rejection function is passed any additional data related to the rejection.

Both arguments are optional, so you can listen for any combination of fulfillment and rejection. For example:

```javascript
let promise = readFile("example.txt");

// listen for both fulfillment and rejection
promise.then(function(contents) {
    // fulfillment
    console.log(contents);
}, function(err) {
    // rejection
    console.error(err.message);
});

// listen for just fulfillment - errors are not reported
promise.then(function(contents) {
    // fulfillment
    console.log(contents);
});

// listen for just rejection - success is not reported
promise.then(null, function(err) {
    // rejection
    console.error(err.message);
});
```

There is also a `catch()` method that behaves the same as `then()` when only a rejection handler is passed. For example:

```javascript
promise.catch(function(err) {
    // rejection
    console.error(err.message);
});

// is the same as:

promise.then(null, function(err) {
    // rejection
    console.error(err.message);
});
```

The intent is to use a combination of `then()` and `catch()` to properly handle the result of asynchronous operations. The benefit of this over both events and callbacks is that it's completely clear whether the operation succeeded or failed. (Events tend not to fire when there's an error and in callbacks you must always remember to check the error argument.)

> If you don't attach a rejection handler to a promise, all failures happen silently. It's a good idea to always attach a rejection handler even if it just logs the failure.

One of the unique aspects of promises is that a fulfillment or rejection handler will still be executed if it is added after the promise is already settled. This allows you to add new fulfillment and rejection handlers at any point in time and be assured that they will be called. For example:

```javascript
let promise = readFile("example.txt");

// original fulfillment handler
promise.then(function(contents) {
    console.log(contents);

    // now add another
    promise.then(function(contents) {
        console.log(contents);
    });
});
```

In this example, the fulfillment handler adds another fulfillment handler to the same promise.The promise is already fulfilled at this point, so the new fulfillment handler is added to the job queue and called when ready. Rejection handlers work the same way in that they can be added at any point and are guaranteed to be called.

> Each call to `then()` or `catch()` creates a new job to be executed when the promise is resolved. However, these jobs end up in a separate job queue that is reserved strictly for promises. The precise details of this second job queue aren't all that important for understanding how to use promises so long as you understand how job queues work in general.

### Creating Unsettled Promises

New promises are created through the `Promise` constructor. This constructor accepts a single argument, which is a function (called the *executor*) containing the code to execute when the promise is added to the job queue. The executor is passed two functions as arguments, `resolve()` and `reject()`. The `resolve()` function is called when the executor has finished successfully in order to signal that the promise is ready to be resolved while the `reject()` function indicates that the executor has failed. Here's an example using a promise in Node.js to implement the `readFile()` function from earlier in this chapter:

```javascript
// Node.js example

let fs = require("fs");

function readFile(filename) {
    return new Promise(function(resolve, reject) {

        // trigger the asynchronous operation
        fs.readFile(filename, { encoding: "utf8" }, function(err, contents) {

            // check for errors
            if (err) {
                reject(err);
                return;
            }

            // the read succeeded
            resolve(contents);

        });
    });
}

let promise = readFile("example.txt");

// listen for both fulfillment and rejection
promise.then(function(contents) {
    // fulfillment
    console.log(contents);
}, function(err) {
    // rejection
    console.error(err.message);
});

```

In this example, the native Node.js `fs.readFile()` asynchronous call is wrapped in a promise. The executor either passes the error object to `reject()` or the file contents to `resolve()`.

Keep in mind that the executor doesn't run immediately when `readFile()` is called. Instead, it is added as a job to the job queue. This is called *job scheduling*, and if you've ever used `setTimeout()` or `setInterval()`, then you're already familiar with it. The idea is that a new job is added to the job queue so as to say, "don't execute this right now, but execute later." In the case of `setTimeout()` and `setInterval()`, you're specifying a delay before the job is added to the queue:

```javascript
// add this function to the job queue after 500ms have passed
setTimeout(function() {
    console.log("Timeout");
}, 500)

console.log("Hi!");
```

In this example, the code schedules a job to be added to the job queue after 500ms. That results in the following output:

```
Hi!
Timeout
```

You can tell from the output that the function passed to `setTimeout()` was executed after `console.log("Hi!")`. Promises work in a similar way.

The promise executor is added to the job queue immediately, meaning it will execute only after all previous jobs are complete. For example:

```javascript
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});

console.log("Hi!");
```

The output for this example is:

```
Hi!
Promise
```

The takeaway is that the executor doesn't run until sometime after the current job has finished executing. The same is true for the functions passed to `then()` and `catch()`, as these will also be added to the job queue, but only after the executor job. Here's an example:

```javascript
let promise = new Promise(function(resolve, reject) {
    console.log("Promise");
    resolve();
});

promise.then(function() {
    console.log("Resolved.");
});

console.log("Hi!");
```

The output for this example is:

```
Hi!
Promise
Resolved
```

The fulfillment and rejection handlers are always added to the end of the job queue after the executor has completed.

### Creating Settled Promises

The `Promise` constructor is the best way to create unsettled promises due to the dynamic nature of what the promise executor does. However, if you want a promise to represent just a single known value, then it doesn't make sense to go through the work of scheduling a job that simply passes a value to `resolve()`. Instead, there are two methods that create settled promises given a specific value.

The `Promise.resolve()` method accepts a single argument and returns a promise in the fulfilled state. That means there is no job scheduling that occurs and you need to add one or more fulfillment handlers to the promise to retrieve the value. For example:

```javascript
let promise = Promise.resolve(42);

promise.then(function(value) {
    console.log(value);         // 42
});
```

This code creates a fulfilled promise so the fulfillment handler receives 42 as `value`. If a rejection handler were added to this promise, it would never be called because the promise will never be in the rejected state.

You can also create rejected promises by using the `Promise.reject()` method. This works in the same way as `Promise.resolve()` except that the created promise is in the rejected state. That means any additional rejection handlers added to the promise will be called but not fulfillment handlers will be called:

```javascript
let promise = Promise.reject(42);

promise.catch(function(value) {
    console.log(value);         // 42
});
```

> If you pass a promise to either `Promise.resolve()` or `Promise.reject()`, the promise is returned without modification.

Both `Promise.resolve()` and `Promise.reject()` also accept non-promise thenables as arguments and will create a new promise that is called after `then()`. A non-promise thenable is created when a object has a `then()` method that accepts two arguments: `resolve` and `reject`. For example:

```javascript
let thenable = {
    then: function(resolve, reject) {
        resolve(42);
    }
};
```

The `thenable` object in this example has no characteristics associated with a promise other than the `then()` method. It can be converted into a fulfilled promise using `Promise.resolve()`:

```javascript
let thenable = {
    then: function(resolve, reject) {
        resolve(42);
    }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value) {
    console.log(value);     // 42
});
```

In this example, `Promise.resolve()` calls `thenable.then()` so that a promise state can be determined. Since this code calls `resolve(42)`, the promise state for `thenable` is fulfilled. A new promise is created in the fulfilled state with the value passed from the thenable (42) so the fulfillment handler for `p1` receives 42 as the value. The same process can be used with `Promise.reject()` in order to create a rejected promise from a thenable:

```javascript
let thenable = {
    then: function(resolve, reject) {
        reject(42);
    }
};

let p1 = Promise.reject(thenable);
p1.catch(function(value) {
    console.log(value);     // 42
});
```

This example is similar to the last except that `Promise.reject()` is used on `thenable`. Doing so executes `thenable.then()` and creates a new promise in the rejected state with a value of 42. That value is then passed to the rejection handler for `p1`.

Both `Promise.resolve()` and `Promise.reject()` work in this way to allow you to easily work with non-promise thenables. Whenever you're unsure if an object is a promise, passing the object through `Promise.resolve()` or `Promise.reject()` (depending on your anticipated result) is the best approach since promises are just passed through without any change.

### Executor Errors

If an error is thrown inside of an executor, then the promise's rejection handler is called. For example:

```javascript
let promise = new Promise(function(resolve, reject) {
    throw new Error("Explosion!");
});

promise.catch(function(error) {
    console.log(error.message);     // "Explosion!"
});
```

In this code, the executor intentionally throws an error. There is an implicit `try-catch` inside of every executor such that the error is caught and then passed to the rejection handler. In effect, the previous example is equivalent to:

```javascript
// equivalent of previous example
let promise = new Promise(function(resolve, reject) {
    try {
        throw new Error("Explosion!");
    } catch (ex) {
        reject(ex);
    }
});

promise.catch(function(error) {
    console.log(error.message);     // "Explosion!"
});
```

The executor handles catching any thrown errors in order to simplify this common use case.

## Chaining Promises

To this point, promises may seem like little more than an incremental improvement over using some combination of a callback and `setTimeout()`, but there is much more to promises than meets the eye. More specifically, there are a number of ways to chain promises together to accomplish more complex asynchronous behavior.

Each call to `then()` or `catch()` actually creates and returns another promise. This second promise is resolved only once the first has been fulfilled or rejected. For example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);
}).then(function() {
    console.log("Finished");
});
```

The output from this example is:

```
42
Finished
```

The call to `p1.then()` returns a second promise on which `then()` is called. The second `then()` fulfillment handler is only called after the first promise has been resolved. If you unchain this example, it looks like this:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

// same as

let p2 = p1.then(function(value) {
    console.log(value);
})

p2.then(function() {
    console.log("Finished");
});
```

As you might have guessed, `p2.then()` also returns a promise, but it's not used in this example.

### Catching Errors

Promise chaining allows you to catch errors that may occur in a fulfillment or rejection handler from a previous promise. For example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    throw new Error("Boom!");
}).catch(function(error) {
    console.log(error.message);     // "Boom!"
});
```

In this example, the fulfillment handler for `p1` throws an error. The chained call to `catch()`, which is on a second promise, is able to receive that error through its rejection handler. The same is true if a rejection handler throws an error:

```javascript
let p1 = new Promise(function(resolve, reject) {
    throw new Error("Explosion!");
});

p1.catch(function(error) {
    console.log(error.message);     // "Explosion!"
    throw new Error("Boom!");
}).catch(function(error) {
    console.log(error.message);     // "Boom!"
});
```

Here, the executor throws an error then triggers `p1`'s rejection handler. That handler then throws another error that is caught by the second promise's rejection handler. In this way, chained promise calls can be made aware of errors in other promises in the chain.

> It's recommended to always have a rejection handler at the end of a promise chain to ensure that you can properly handle any errors that may occur.

### Returning Values in Promise Chains

Another important aspect of promise chains is the ability to pass data from one promise to the next. You've already seen that a value passed to the `resolve()` handler inside an executor is passed to the fulfillment handler for that promise. You can continue passing data along by specifying a return value from the fulfillment handler. For example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);         // "42"
    return value + 1;
}).then(function(value) {
    console.log(value);         // "43"
});
```

In this example, the fulfillment handler for `p1` returns a value (`value + 1`). Since `value` is 42 (from the executor) then the fulfillment handler returns 43. That value is then passed to the fulfillment handler of the second promise that can output it to the console.

The same thing is possible using the rejection handler. When a rejection handler is called, it has the option of return a value. That value is then used to fulfill the next promise in the chain. For example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    reject(42);
});

p1.catch(function(value) {
    console.log(value);         // "42"
    return value + 1;
}).then(function(value) {
    console.log(value);         // "43"
});
```

Here, the executor calls `reject()` with 42. That value is passed into the rejection handler for the promise, where `value + 1` is returned. Even though this return value is coming from a rejection handler, it is still used in the fulfillment handler of the next promise in the chain. This allows for the failure of one promise to allow recovery of the entire chain if necessary.

## Returning Promise in Promise Chains

Returning primitive values from fulfillment and rejection handlers allows passing of data between promises, but what if you return an object? If the object is a promise, then there's an extra step that's taken to determine how to proceed. Consider the following example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

p1.then(function(value) {
    console.log(value);     // 42
    return p2;
}).then(function(value) {
    console.log(value);     // 43
});
```

In this code, `p1` schedules a job that resolves to 42. In the fulfillment handler for `p1`, `p2`, a promise is already in the resolved state, is returned. The second fulfillment handler is called because `p2` has been fulfilled. If `p2` was rejected, the second fulfillment handler would not be called and instead a rejection handler (if present) would be called.

The important thing to recognize about this pattern is that the second fulfillment handler is not added to `p2`, but rather to a third promise. It's this third promise that the second fulfillment handler is attached to. The previous example is equivalent to this:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = p1.then(function(value) {
    console.log(value);     // 42
    return p2;
});

p3.then(function(value) {
    console.log(value);     // 43
});
```

Here, it's clear that the second fulfillment handler is attached to `p3` rather than `p2`. This is a subtle but important distinction as the second fulfillment handler will not be called if `p2` is rejected. For example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    reject(43);
});

p1.then(function(value) {
    console.log(value);     // 42
    return p2;
}).then(function(value) {
    console.log(value);     // never called
});
```

In this example, the second fulfillment handler is never called because `p2` is rejected. You could, however, attach a rejection handler instead:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    reject(43);
});

p1.then(function(value) {
    console.log(value);     // 42
    return p2;
}).catch(function(value) {
    console.log(value);     // 43
});
```

Here, the rejection handler is called as a result of `p2` being rejected. The rejected value 43 from `p2` is passed into that rejection handler.

Returning thenables from fulfillment or rejection handlers doesn't change when the promise executors are executed. The first defined promise will run its executor first, followed by the second, and so on. Returning thenables simply allows you to define additional responses. You defer the execution of fulfillment handlers by creating a new promise within a fulfillment handler. For example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

p1.then(function(value) {
    console.log(value);     // 42

    // create a new promise
    let p2 = new Promise(function(resolve, reject) {
        resolve(43);
    });

    return p2
}).then(function(value) {
    console.log(value);     // 43
});
```

In this example, a new promise is created within the fulfillment handler for `p1`. That means the second fulfillment handler will not be executed until after `p2` has been fulfilled. This pattern useful when you want to wait until a previous promise has been settled before before triggering another promise.

## Responding to Multiple Promises

Up to this point, each example has dealt with responding to one promise at a time. There are times, however, when you'll want to monitor the progress of multiple promises in order to determine the next action. ECMAScript 6 provides two methods that monitor multiple promises: `Promise.all()` and `Promise.race()`.

### Promise.all()

The `Promise.all()` method accepts a single argument, which is an iterable of promises to monitor, and returns a promise that is resolved only when every promise in the iterable is resolved. The returned promise is fulfilled when every promise in the iterable is fulfilled, for example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);

p4.then(function(value) {
    console.log(value);     // [42, 43, 44]
});
```

Each of the promises in this example resolves with a number. The call to `Promise.all()` creates a new promise, `p4`, that ultimately is fulfilled because each of the promises is fulfilled. The result passed to the fulfillment handler for `p4` is an array containing each resolved value: 42, 43, and 44. In this way, you can match promise results to the promises that resolved to them.

If any of the promises passed to `Promise.all()` is rejected, the returned promise is immediately rejected without waiting for the other promises to complete:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = new Promise(function(resolve, reject) {
    reject(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.all([p1, p2, p3]);

p4.catch(function(value) {
    console.log(value);     // 43
});
```

In this example, `p2` is rejected with a value of 43. The rejection handler for `p4` is called immediately without waiting for either `p1` or `p3` to finish executing (they still finish executing, it's just that `p4` doesn't wait). The rejection handler is passed 43 to reflect the rejection from `p2`.

### Promise.race()

The `Promise.race()` method provides a slightly different take on monitoring multiple promises. This method also accepts an iterable of promises to monitor and returns a promise, however, the returned promise is settled as soon as the first promise is settled. So instead of waiting for all promises to be fulfilled, as in `Promise.all()`, the returned promise is fulfilled as soon as any of the promises is fulfilled. For example:

```javascript
let p1 = Promise.resolve(42);

let p2 = new Promise(function(resolve, reject) {
    resolve(43);
});

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.race([p1, p2, p3]);

p4.then(function(value) {
    console.log(value);     // 42
});
```

In this code, `p1` is created as a fulfilled promise while the others schedule jobs. The fulfillment handler for `p4` is then called with the value of 42 and ignores the other promises completely. The promises passed to `Promise.race()` are truly in a race to see which is settled first. If the first promise to settle is fulfilled, then the returned promise is fulfilled; if the first promise to settle is rejected, then the returned promise is rejected. Here's an example with a rejection:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = Promise.reject(43);

let p3 = new Promise(function(resolve, reject) {
    resolve(44);
});

let p4 = Promise.race([p1, p2, p3]);

p4.catch(function(value) {
    console.log(value);     // 43
});
```

Here, `p4` is rejected because `p2` is already in the rejected state when `Promise.race()` is called. Even though `p1` and `p3` are fulfilled, those results are ignored because they occur after `p2` is rejected.

### Asynchronous Task Scheduling

Back in chapter 8, you learned about generators and how they can be used for asynchronous task scheduling such as the following:

```javascript
let fs = require("fs");

let task;

function readConfigFile() {
    fs.readFile("config.json", function(err, contents) {
        if (err) {
            task.throw(err);
        } else {
            task.next(contents);
        }
    });
}

function *init() {
    let contents = yield readConfigFile();
    doSomethingWith(contents);
    console.log("Done");
}

task = init();
task.next();
```

The pain point of this implementation was needing to keep track of `task` and calling the appropriate methods on it in every single asynchronous function you use (such as `readConfigFile()`). With promises, you can greatly simplify and generalize this process by ensuring that each asynchronous operation returns a promise. That common interface means you can greatly simplify asynchronous code:

```javascript
let fs = require("fs");

function run(taskDef) {

    // create the iterator
    let task = taskDef();

    // start the task
    task.next();

    // recursive function to iterate through
    (function step() {

        let result = task.next(),
            promise;

        // if there's more to do
        if (!result.done) {

            // resolve to a promise to make it easy
            promise = Promise.resolve(result.value);
            promise.then(function(value) {
                task.next(value);
                step();
            }).catch(function(error) {
                task.throw(error);
                step();
            });
        }
    }());
}

function readConfigFile() {
    return new Promise(resolve, reject) {
        fs.readFile("config.json", function(err, contents) {
            if (err) {
                reject(err);
            } else {
                resolve(contents);
            }
        });
    });
}

run(function *() {
    let contents = yield readConfigFile();
    doSomethingWith(contents);
    console.log("Done");
});
```

In this version of the code, a generic `run()` function is used to execute a generator. The `run()` function executes the generator to create an iterator, starts the task by calling `task.next()`, and then recursively calls `step()` until the iterator is complete. Inside of `step()`, `task.next()` returns the iterator result. If there's more work to do then `response.done` is `false`. At that point, `result.value` should be a promise, but `Promise.resolve()` is used just in case the function in question didn't return a promise. Then, a fulfillment handler is added that retrieves the promise value and passes it back to the iterator before calling `step()` once again. A rejection handler is also added and assumes any rejection results in an error object. That error object is passed back into the iterator using `task.throw()` and `step()` is called to continue.

This same `run()` function can be used any to run any generator that uses `yield` as a way to achieve asynchronous code without exposing promises (or callbacks) to the developer.

## Inheriting from Promises

Just like other built-in types, promises can be used as a base upon which you can create a derived class. This allows you to define your own variation of promises to extend what the built-in promises can do. Suppose, for instance, you'd like to create a promise that uses `success()` and `failure()` in addition to `then()` and `catch()`. You could do so as follows:

```javascript
class MyPromise extends Promise {

    // use default constructor

    success(resolve, reject) {
        return this.then(resolve, reject);
    }

    failure(reject) {
        return this.catch(reject);
    }

}

let promise = new MyPromise(function(resolve, reject) {
    resolve(42);
});

promise.success(function(value) {
    console.log(value);             // 42
}).failure(function(value) {
    console.log(value);
});
```

In this example, `MyPromise` is derived from `Promise` and two additional methods are added. Both `success()` and `failure()` use `this` to call the appropriate method they are mimicking. The created promise functions the same as the built-in version, except now you can use `success()` and `failure()` in addition to `then()` and `catch()`.

Since static methods are also inherited, that means `MyPromise.resolve()`, `MyPromise.reject()`, `MyPromise.race()`, and `MyPromise.all()` are also present. While the last two behave the same as the built-in methods, the first two are slightly different.

Both `MyPromise.resolve()` and `MyPromise.reject()` will return an instance of `MyPromise` regardless of the value passed. So if a built-in promise is passed to either, it will be resolved or rejected and a new `MyPromise` so you can assign fulfillment and rejection handlers. For example:

```javascript
let p1 = new Promise(function(resolve, reject) {
    resolve(42);
});

let p2 = MyPromise.resolve(p1);
p2.success(function(value) {
    console.log(value);         // 42
});

console.log(p2 instanceof MyPromise);   // true
```

Here, `p1` is a built-in promise that is passed to `MyPromise.resolve()`. The result, `p2`, is an instance of `MyPromise` where the resolved value from `p1` is passed into the fulfillment handler.

If an instance of `MyPromise` is passed to `MyPromise.resolve()` or `MyPromise.reject()`, it will just be returned directly without being resolved. In all other ways these two methods behave the same as `Promise.resolve()` and `Promise.reject()`.

## Summary

Promises are designed to improve asynchronous programming in JavaScript. Whereas events and callbacks have several limitations, the permutations available via promises mean more control and composability over asynchronous operations. This is accomplished by scheduling jobs to be added to the JavaScript engine's job queue for execution later. A second job queue keeps track of promise fulfillment and rejection handlers to ensure proper execution.

Promises have three states: pending, fulfilled, and rejected. A promise starts out in a pending state and is either fulfilled (a success) or rejected (a failure). In either case, handlers can be added to be notified when a promise is settled. The `then()` method allows you to assign a fulfillment and rejection handler and the `catch()` method allows you to assign only a rejection handler.

You can chain promises together in a variety of ways and pass information between them. Each call to `then()` creates and returns a new promise that is resolved when the previous one is resolved. Such chains can be used to trigger responses to a series of asynchronous events. You can also use `Promise.race()` and `Promise.all()` to monitor the progress of multiple promises and respond accordingly.

Asynchronous task scheduling is made easier using generators in addition to promises, as promises give a common interface that asynchronous operations can return. You can then use generators and the `yield` operator to wait for asynchronous responses and respond appropriately.

Most new web APIs are being built on top of promises, and you can expect many more to follow suit in the future.
<!-- @section -->
# Modules

One of the most error-prone and confusing aspects of JavaScript has long been the "shared everything" approach to loading code. Whereas other languages have concepts such as packages, JavaScript lagged behind, and everything defined in every file shared the same global scope. As web applications became more complex and the amount of JavaScript used grew, the "shared everything" approach began to show problems with naming collisions, security concerns, and more. One of the goals of ECMAScript 6 was to solve this problem and bring some order into JavaScript applications. That's where modules come in.

## What are Modules?

*Modules* are JavaScript files that are loaded in a special mode (as opposed to *scripts*, which are loaded in the original way JavaScript worked). At the time of my writing, neither browsers nor Node.js have a way to natively load ECMAScript 6 modules, but both have indicated that there will need to be some sort of opt-in to do so. The reason this opt-in is necessary is because module files have very different semantics than non-module files:

1. Module code automatically runs in strict mode and there's no way to opt-out of strict mode.
1. Variables created in the top level of a module are not automatically added to the shared global scope. They exist only within the top-level scope of the module.
1. The value of `this` in the top level of a module is `undefined`.
1. Does not allow HTML-style comments within the code (a leftover feature from the early browser days).
1. Modules must export anything that should be available to code outside of the module.

These differences may seem small at first glance, however, they represent a significant change in how JavaScript code is loaded and evaluated.

Module JavaScript files are created just like any other JavaScript file: in a text editor and typically with the `.js` extension. The only difference during development is that you use some different syntax.

## Basic Exporting and Importing

The `export` keyword is used to expose parts of published code to other modules. In the simplest case, you can place `export` in front of any variable, function, or class declaration to export it from the module. For example:

```javascript
// export data
export var color = "red";
export let name = "Nicholas";
export const magicNumber = 7;

// export function
export function sum(num1, num2) {
    return num1 + num1;
}

// export class
export class Rectangle {
    constructor(length, width) {
        this.length = length;
        this.width = width;
    }
}

// this function is private to the module
function subtract(num1, num2) {
    return num1 - num2;
}

// define a function
function multiply(num1, num2) {
    return num1 * num2;
}

// export later
export multiply;
```

There are a few things to notice in this example:

1. Every declaration is exactly the same as it would otherwise be without the `export` keyword.
1. Both function and class declarations require a name. You cannot export anonymous functions or classes using this syntax.
1. You need not always export the declaration, you can also export references, as with `multiply` in this example.
1. Any variables, functions, or classes that are not explicitly exported remain private to the module. In this example, `subtract()` is not exported and is therefore not accessible from outside the module.

An important limitation of `export` is that it must be used in the top-level of the module. For instance, this is a syntax error:

```javascript
if (flag) {
    export flag;    // syntax error
}
```

This example is a syntax error because `export` is inside of an `if` statement. Exports cannot be condition or done dynamically in any way. Part of the benefit of module syntax is so the JavaScript engine can staticly determine what will be exported. As such, you can only use `export` at the top-level of a module.

> If you are using a transpiler like Babel.js, you may find that `export` can be used anywhere. This only works when code is converted to ECMAScript 5 and will not work with a native ECMAScript 6 module system.

Once you have a module with exports, you can access the functionality in another module by using the `import` keyword. An `import` statement has two parts: the identifiers you're importing and the module from which those identifiers should be imported. The basic form is as follows:

```javascript
import { identifier1, identifier2 } from "module";
```

The curly braces after `import` indicate the identifiers to import from the given module. The keyword `from` is used to indicate the module from which to import the given identifiers. The module is specified using a string. At the time of my writing, it is still undecided what module identifiers will look like. They may end up being full file paths (such as "../mymodule.js"), file paths without extensions (such as "../mymodule"), or something else. This likely won't be determined until browsers and Node.js begin implementing modules natively.

> Even though it looks similar, the list of identifiers to import is not a destructured object.

When importing an identifier from a module, the identifier acts as if were defined using `const`. That means you cannot define another variable with the same name, use the identifier prior to the `import` statement, or change its value.

Suppose that the first example in this section is in a module named `"example"`. You can import and use identifiers from that module in a number of ways. You can just import one identifier:

```javascript
// import just one
import { sum } from "example";

console.log(sum(1, 2));     // 3

sum = 1;        // error
```

This example imports only `sum()` from the example module. Even though the example module exports more than just that one function, they are not exposed here. If you try to assign a new value to `sum`, the result is an error, as you cannot reassign imported identifiers.

If you want to import multiple identifiers from the example module, you can explicitly list them out:

```javascript
// import multiple
import { sum, multiply, magicNumber } from "example";
console.log(sum(1, magicNumber));   // 8
console.log(multiply(1, 2));        // 2
```

Here, three identifiers are imported from the example module: `sum`, `multiply`, and `magicNumber`. They are then used as if they were locally defined.

There's also a special case that allows you to import the entire module as a single object. All of the exports are then available on that object as properties. For example:

```javascript
// import everything
import * as example from "example";
console.log(example.sum(1,
        example.magicNumber));          // 8
console.log(example.multiply(1, 2));    // 2
```

In this code, the entirety of the example module is loaded into an object called `example`. The named exports `sum()`, `multiple()`, and `magicNumber` are then accessible as properties on `example`.

## Renaming Exports and Imports

Sometimes the original name of a variable, function, or class isn't what you want to use. It's possible to change the name of an export both during the export and when the identifier is being imported.

In the first case, suppose you have a function that you'd like to export with a different name. You can use the `as` keyword to specify the name that the function should be known as outside of the module:

```javascript
function sum(num1, num2) {
    return num1 + num2;
}

export { sum as add };
```

Here, the `sum()` function (`sum` is the *local name*) is exported as `add()` (`add` is the *exported name*). That means when another module wants to import this function, it will have to use the name `add` instead:

```javascript
import { add } from "example";
```

If the module importing the function wants to use a different name, it can also use `as`:

```javascript
import { add as sum } from "example";
console.log(typeof add);            // "undefined"
console.log(sum(1, 2));             // 3
```

This code imports the `add()` function (the *import name*) and renames it to `sum()` (the local name). That means there is no identifier named `add` in this module.

> ### Imported Bindings
>
> A subtle but important point about the `import` statements is that they create bindings to variables, functions, and classes rather than simply referencing them. That means even though you cannot change an imported identifier, it can still change on its own. For example, suppose you have this module:
>
> ```javascript
> export var name = "Nicholas";
> export function setName(newName) {
>     name = newName;
> }
> ```
>
> When you import `name` and `setName()`, you can see that `setName()` is able to change the value of `name`:
>
> ```javascript
> import { name, setName } from "example";
>
> console.log(name);       // "Nicholas"
> setName("Greg");
> console.log(name);       // "Greg"
>
> name = "Nicholas";       // error
> ```
>
> The call to `setName("Greg")` goes back into the module from which `setName()` was exported and executes there, setting `name` to `"Greg"`. Note this change is automatically reflected on the imported `name` binding. That's because `name` is the local name for the exported *name* identifier so they are not the same thing.

## Exporting and Importing Defaults

The module syntax is really optimized for exporting and importing default values from modules. The default value for a module is a single variable, function, or class as specified by the `default` keyword. For example:

```javascript
export default function(num1, num2) {
    return num1 + num2;
}
```

This module exports a function as the default. The `default` keyword indicates that this is a default export and the function doesn't require a name because the module itself represents the function.

You can also specify an identifier as being the default export using the renaming syntax, such as:

```javascript
// equivalent to previous example
function sum(num1, num2) {
    return num1 + num2;
}

export { sum as default };
```

The `as default` specifies that `sum` should be the default export of the module. This syntax is equivalent to the previous example.

> You can only have one default export per module. It is a syntax error to use the `default` keyword with multiple exports.

You can import a default value from a module using the following syntax:

```javascript
// import the default
import sum from "example";

console.log(sum(1, 2));     // 3
```

This import statement imports the default from the module `"example"`. Note that there are no curly braces used in this case, as would be with a non-default export. The local name `sum` is used to represent the function that the module exports. This syntax is the cleanest as it's anticipated to be the dominant form of import on the web, allowing you to use already-existing object, such as:

```javascript
import $ from "jquery";
```

For modules that export both a default and one or more non-defaults, you can import them with one statment. For instance, suppose you have this module:

```javascript
export let color = "red";

export default function(num1, num2) {
    return num1 + num2;
}
```

You can then import both `color` and the default function using the following:

```javascript
import sum, { color } from "example";

console.log(sum(1, 2));     // 3
console.log(color);         // "red"
```

The comma separates the default local name from the non-defaults (which are also surrounded by curly braces).

As with exporting defaults, importing defaults can also be accomplished using the renaming syntax:

```javascript
// equivalent to previous example
import { default as sum, color } from "example";

console.log(sum(1, 2));     // 3
console.log(color);         // "red"
```

In this code, the default export (`default`) is renamed to `sum` and the additional `color` export is also imported. This example is equivalent to the previous example.

## Re-exporting

There may be a time when you'd like to re-export something that your module has imported. You can do this using the patterns already discussed in this chapter, such as:

```javascript
import { sum } from "example";
export { sum }
```

However, there's also a single statement that can accomplish the same thing:

```javascript
export { sum } from "example";
```

This form of `export` looks into the specified module for the declaration of `sum` and then exports it. Of course, you can also choose to export a different name for the same thing:

```javascript
export { sum as add } from "example";
```

Here, `sum` is imported from `"example"` and then exported as `add`.

If you'd like to export everything from another module, you can use the `*` pattern:

```javascript
export * from "example";
```

By exporting everything, you're including the default as well as any named exports, which may affect what you can export from your module. For instance, if `"example"` has a default export, you'll be unable to define a new default export when using this syntax.

## Importing Without Bindings

Some modules may not export anything, and instead, only make modifications to objects in the global scope. Even though top-level variables, functions, and classes inside of modules do not automatically end up in the global scope, that doesn't mean modules cannot access the global scope. The shared definitions of built-in objects such as `Array` and `Object` are accessible inside of a module and changes to those objects will be reflected in other modules.

For instance, suppose you want to add a method to all arrays called `pushAll()`, you may define a module like this:

```javascript
// module code without exports or imports
Array.prototype.pushAll = function(items) {

    // items must be an array
    if (!Array.isArray(items)) {
        throw new TypeError("Argument must be an array.");
    }

    // use built-in push() and spread operator
    return this.push(...items);
};
```

This is a valid module even though there are no exports or imports. This code can be used both as a module and a script. Since it doesn't export anything, you can use a simplified import to execute the module code without importing any bindings:

```javascript
import from "example";

let colors = ["red", "green", "blue"];
let items = [];

items.pushAll(colors);
```

In this example, the module is imported and executed, so `pushAll()` is added to the array prototype. That means `pushAll()` is now available for use on all arrays inside of this module.

> Imports without bindings are most likely to be used to create polyfills and shims.

## Summary

ECMAScript 6 adds modules to the language as a way to package up and encapsulate functionality. Modules behave differently than scripts, as they do not modify the global scope with their top-level variables, functions, and classes, and `this` is `undefined`. In order to work differently than scripts, modules must be loaded using a different mode.

You must export any functionality you'd like to make available to consumers of a module. Variables, functions, and classes can all be exported, and there is also one default export allowed per module. After exporting, another module can import all or some of the exported names. These names act as if defined by `let`, and so operate as block bindings that cannot be redeclared in the same module.

Modules need not export anything if they are manipulating something in the global scope. In that case, it's possible to import from such a module without introducing any bindings into the module scope.
<!-- @section -->
# Template Strings

JavaScript's strings have been fairly limited when compared to those in other languages. Template strings add new syntax to allow the creation of domain-specific languages (DSLs) for working with content in a way that is safer than the solutions we have today. The description on the template string strawman was as follows:

> This scheme extends ECMAScript syntax with syntactic sugar to allow libraries to provide DSLs that easily produce, query, and manipulate content from other languages that are immune or resistant to injection attacks such as XSS, SQL Injection, etc.

In reality, though, template strings are ECMAScript 6's answer to several ongoing problems in JavaScript:

* **Multiline strings** - JavaScript has never had a formal concept of multiline strings.
* **Basic string formatting** - The ability to substitute parts of the string for values contained in variables.
* **HTML escaping** - The ability to transform a string such that it is safe to insert into HTML.

Rather than trying to add more functionality to JavaScript's already-existing strings, template strings represent an entirely new approach to solving these problems.

## Basic Syntax

At their simplest, template strings act like regular strings that are delimited by backticks (`` ` ``) instead of double or single quotes. For example:

```javascript
let message = `Hello world!`;

console.log(message);               // "Hello world!"
console.log(typeof message);        // "string"
console.log(message.length);        // 12
```

This code demonstrates that the variable `message` contains a normal JavaScript string. The template string syntax only is used to create the string value, which is then assigned to `message`.

If you want to use a backtick in your string, then you need only escape it by using a backslash (`\`):

```javascript
let message = `\`Hello\` world!`;

console.log(message);               // "`Hello` world!"
console.log(typeof message);        // "string"
console.log(message.length);        // 14
```

There's no need to escape either double or single quotes inside of template strings.

## Multiline Strings

Ever since the first version of JavaScript, developers have longed for a way to create multiline strings in JavaScript. When using double or single quotes, strings must be completely contained on a single line. JavaScript has long had a syntax bug that would allow multiline strings by using a backslash (`\`) before a newline, such as:

```javascript
let message = "Multiline \
string";

console.log(message);       // "Multiline string"
```

Note that the string has no newlines present when output, that's because the backslash is treated as a continuation rather than a newline. In order to have a newline at that point, you would need to manually include it, such as:

```javascript
let message = "Multiline \n\
string";

console.log(message);       // "Multiline
                            //  string"
```

Despite this working in all major JavaScript engines, the behavior was defined as a bug and many recommended avoiding its usage.

Other attempts to create multiline strings usually relied on arrays or string concatenation, such as:

```javascript
let message = [
    "Multiline ",
    "string"
].join("\n");

let message = "Multiline \n" +
    "string";
```

All of the ways developers worked around JavaScript's lack of multiline strings left something to be desired.

Template strings make multiline strings easy because there is no special syntax. Just include a newline where you want and it shows up in the result. For example:

```javascript
let message = `Multiline
string`;

console.log(message);           // "Multiline
                                //  string"
console.log(message.length);    // 16
```

All whitespace inside of the backticks is considered to be part of the string, so be careful with indentation. For example:

```javascript
let message = `Multiline
               string`;

console.log(message);           // "Multiline
                                //                 string"
console.log(message.length);    // 31
```

In this code, all of the whitespace before the second line of the template string is considered to be a part of the string itself. If making the text line up with proper indentation is important to you, then you consider leaving nothing on the first line of a multiline template string and then indenting after that, such as this:

```javascript
let html = `
<div>
    <h1>Title</h1>
</div>`.trim();
```

This code begins the template string on the first line but doesn't have any text until the second. The HTML tags are indented to look correct and then the `trim()` method is called to remove the initial (empty) line.

> If you prefer, you can also use `\n` in a template string to indicate where a newline should be inserted:
>```javascript
>
> let message = `Multiline\nstring`;
>
> console.log(message);           // "Multiline
>                                 //  string"
> console.log(message.length);    // 16
>```

## Substitutions

To this point, template strings may look like a fancier way of defining normal JavaScript strings. The real difference is with template string substitutions. Substitutions allow you to embed any valid JavaScript expression inside of a template string and have the result be output as part of the string.

Substitutions are delimited by an opening `${` and a closing `}`, within which you can use any JavaScript expression. At its simplest, substitutions let you embed local variables directly into the result string, like this:

```javascript
let name = "Nicholas",
    message = `Hello, ${name}.`;

console.log(message);       // "Hello, Nicholas."
```

The substitution `${name}` accessed the local variable `name` to insert it into the string. The `message` variable then holds the result of the substitution immediately.

> Template strings can access any variable that is accessible in the scope in which it is defined. Attempting to use an undeclared variable in a template string results in an error being thrown in both strict and non-strict modes.

Since all substitutions are JavaScript expressions, it's possible to substitute more than just simple variable names. You can easily embed calculations, function calls, and more. For example:

```javascript
let count = 10,
    price = 0.25,
    message = `${count} items cost $${(count * price).toFixed(2)}.`;

console.log(message);       // "10 items cost $2.50."
```

This code performs a calculation as part of the template string. The variables `count` and `price` are multiplied together to get a result, and then formatted to two decimal places using `.toFixed()`. The dollar sign before the second substitution is output as-is because it's not followed by an opening curly brace.

## Tagged Templates

To this point, you've seen how template strings can be used for multiline strings and to insert values into strings without using concatenation. The real power of template strings comes from tagged templates. A *template tag* performs a transformation on the template string and returns the final string value. This tag is specified at the start of the template, just before the first `` ` `` character, such as:

```javascript
let message = tag`Hello world`;
```

In this example, `tag` is the template tag to apply to `` `Hello world` ``.

### Defining Tags

A tag is simply a function that is called with the processed template string data. The function receives data about the template string as individual pieces that the tag must then combined to create the finished value. The first argument is an array containing the literal strings as they are interpreted by JavaScript. Each subsequent argument is the interpreted value of each substitution. Tag functions are typically defined using rest arguments to make dealing with the data easier:

```javascript
function tag(literals, ...substitutions) {
    // return a string
}
```

To better understand what is passed to tags, consider the following:

```javascript
let count = 10,
    price = 0.25,
    message = passthru`${count} items cost $${(count * price).toFixed(2)}.`;
```

If you had a function called `passthru()`, that function would receive three arguments:

1. `literals`, containing:
    * `""` - the empty string before the first substitution
    * `" items cost $"` - the string after the first substitution and before the second
    * `"."` - the string after the second substitution
1. `10` - the interpreted value for `count` (this becomes `substitutions[0]`)
1. `"2.50"` - the interpreted value for `(count * price).toFixed(2)` (this becomes `substitutions[1]`)

Note that the first item in `literals` is an empty string. This is to ensure that `literals[0]` is always the start of the string, just like `literals[literals.length - 1]` is always the end of the string. There is always one fewer substitution than literal, which is to say that `substitutions.length === literals.length - 1` all the time.

Using this pattern, the `literals` and `substitutions` arrays can be interweaved to create the result. The first item in `literals` comes first, then the first item in `substitutions`, and so on, until the string has been completed. So to mimic the default behavior of template, you need only define a function that performs this operation:

```javascript
function passthru(literals, ...substitutions) {
    let result = "";

    // run the loop only for the substitution count
    for (let i = 0; i < substitutions.length; i++) {
        result += literals[i];
        result += substitutions[i];
    }

    // add the last literal
    result += literals[literals.length - 1];

    return result;
}

let count = 10,
    price = 0.25,
    message = passthru`${count} items cost $${(count * price).toFixed(2)}.`;

console.log(message);       // "10 items cost $2.50."
```

This example defines a `passthru` tag that performs the same transformation as the default template string behavior. The only trick is to use `substitutions.length` for the loop rather than `literals.length` to avoid accidentally going past the end of `substitutions`. This works because the relationship between `literals` and `substitutions` is well-defined.

> The values contained in `substitutions` are not necessarily strings. If an expression is evaluated to be a number, as in the previous example, then the numeric value is passed in. It's part of the tag's job to determine how such values should be output in the result.

### Using Raw Values

Template tags also have access to raw string information, which primarily means access to character escapes before they are transformed into their character equivalents. The simplest way to work with raw string values is to the built-in `String.raw()` tag. For example:

```javascript
let message1 = `Multiline\nstring`,
    message2 = String.raw`Multiline\nstring`;

console.log(message1);          // "Multiline
                                //  string"
console.log(message2);          // "Multiline\\nstring"
```

In this code, the `\n` in `message1` is interpreted as a newline while the `\n` in `message2` is returned in its raw form of `"\\n"` (two characters, the slash and `n`). Retrieving the raw string information in this way allows for more complex processing (when necessary).

The raw string information is also passed into template tags. The first argument in a tag function is an array with an extra property called `raw`. The `raw` property is an array containing the raw equivalent of each literal value. So the value in `literals[0]` always has an equivalent `literals.raw[0]` that contains the raw string information. Knowing that, it's possible to mimic `String.raw()` using the following:

```javascript
function raw(literals, ...substitutions) {
    let result = "";

    // run the loop only for the substitution count
    for (let i = 0; i < substitutions.length; i++) {
        result += literals.raw[i];      // use raw values instead
        result += substitutions[i];
    }

    // add the last literal
    result += literals.raw[literals.length - 1];

    return result;
}

let message = raw`Multiline\nstring`;

console.log(message);           // "Multiline\\nstring"
console.log(message.length);    // 17
```

This example uses `literals.raw` instead of `literals` to output the string result. That means any character escapes, including Unicode code point escapes, will be returned in their raw form.

## Summary

Template strings are an important addition to ECMAScript 6 that allows the creating of domain-specific languages (DSLs) to make creating strings easier. The ability to embed variables directly into template strings means that developers have a safer tool than string concatenation for composing long strings with variables.

Built-in support for multiline strings also makes template strings a useful upgrade over normal JavaScript strings, which have never had this ability. Despite allowing newlines directly inside the template string, you can still use `\n` and other character escape sequences.

Template tags are the most important part of this feature for creating DSLs. Tags are functions that receive the pieces of the template string as arguments. You can then use that data to return an appropriate string value. The data provided includes literals, their raw equivalents, and any substitution values. These pieces of information can then be used to determine the correct output for the tag.

ECMAScript 6 has only one built-in tag, which is `String.raw()`. This tag simply returns the template string in its raw form, with character escape sequences in their original form rather than transforming them into their character equivalents.
