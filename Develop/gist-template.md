# Regular Expressions

Regex (Regular expressions), a powerful string pattern match tool, can greatly improve our searching and replacing capabilities. Without regex, validating an email without any third party tools would require an algorithm that would take many daunting hours to create. Thankfully, regex was created for us. Which allows us to specify a pattern to search/match for with an easy setup of characters.  

## Summary

To better understand regex we are going to analyze this popular regular expression. 

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

From looking at the regex above, could you tell what this regex is looking for? If not, don't worry, we will go over every character to better familiarize us with it. 

NOTE: This popular regex will be provided at the top of every section so you don't have to scroll back to the top during each topic.

## Table of Contents

- [Basics](#basics)
- [Simple Pattern](#simple-patterns)
- [Anchors](#anchors)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Quantifiers](#quantifiers)
- [Character Classes](#character-classes)
- [Author](#author)

## Basics

Regular expressions are used with `RegExp` methods : 

```javascript
RegExp.test()
RegExp.exec()
```

and String methods : 

```javascript
String.match()
String.matchAll()
String.search()
String.replace()
String.replaceAll()
String.split()
```

There are two ways to create a regular expression. 

Regular Expression Literal

```javascript
const rgx = /abc/;
```

RegExp Constructor Function

```javascript
const rgx = new RegExp("abc");
```

Both do the same thing, which is to look for `abc`. However, if you know are aren't going to change the pattern use the Literal. If you do plan on changing the pattern based on algorithm changes, use the Constructor. 

So now we understand that the forward slashes 

```javascript
const rgx = / /;
```

mean it's a literal regex and not a regex constructor.

### Simple patterns

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

There are a few characters in this pattern that are simple. They are both `@` and `\.`.

From the previous topic (basic) we have this regex : 

```javascript
const rgx = /abc/;
```

Which is simple in that it only looks for `abc` sequentially. So from looking at the popular expression, we are definitely looking for an `@` in the pattern. But what about the `\.`, doesn't look simple. What this actually means is that it is looking for `.` in the string. Any time you see a `\`, it means an escaping character. There are characters in a regex that are reserved for all regexes. A few are :

- $
- \+
- ?
- .

When including this in your regex, the RegExp object will use the reserved characters to apply special functionality to the pattern. Don't worry if it doesn't make sense, it will be covered later. Just know that `\.` is looking for the character behind the backslash. And you do this so the RegExp doesn't try to use a special functionality when you are really trying to find a literal period. 

So now we can visualize our understanding our regex. So far, we should understand 

```javascript
const rgx = /@\./;
```

Which means we are looking for a `@` and a `.` in our input.

### Anchors

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

Anchors are the `^` and the `$` at the begining and the end of the regex. 

The caret (^) means that the beginning of the input should be whatever follows after it. 

For example using 

```javascript
const string = 'abcdef123'
const regex = /^abc/;
const results = regex.match(string);

// Outputs to 'abc'
```

would mean that that regex would match should any input begin with `abc`. Since the beginning of the string starts with an `abc`, the match would be a success.

The inverse applies for `$`. 

```javascript
const string = 'abcdef123'
const regex = /xyz$/;
const results = regex.match(string);

// Outputs to 'null'
```

This doesn't work because `xyz` is not at the end of the input. However, if we did `/123$/`, that would work since the end of the input is `123`.

So now let's update our visual of our understanding of regex. So far, we should understand 

```javascript
const rgx = /^@\.$/;
```

Which means to look for an `@` at the beginning and `.` at the end of an input. 

### Grouping and Capturing

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

`()` are used for grouping and capturing. However, in our guide we are only matching a limited specific input, so there is no need to explain capturing any further. But we are grouping. Grouping can be thought of as subexpressions.

So now let's update our visual of our understanding of regex. So far, we should understand 

```javascript
const rgx = /^()@()\.()$/;
```

Which means we are looking for a subexpression in the beginning of the input, then an `@`, then another subexpression, then a `.`, then a subexpression at the end. 

### Bracket Expressions

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

Let's go back to our to simple regex.

```javascript
const regex = /abc/;
```

The expression above looks for `abc` exactly. But what if we wanted to look for all `abc`'s in the input. We can do that with brackets. 

```javascript
const string = 'aabbbcccc';
const regex = /[abc]/;
const results = regex.match(string);

// Outputs to 'a'
```

However this just looks for the first valid character specified in the bracket expression. To get multiple matches, you need to enable global or at the `+` quantifer. Quantifiers will be explanined later. 

Also what if we wanted to look for the entire alphabet. Writing each letter of the alphabet inside the square bracket is not ideal. There is a much easier way to include the alphabet and that's with the dash. A dash is a range. 

```javascript
const regex = /[a-z]+/;
```

This gets the entire alphabet in lowercase from `a-z`. To include upper case, add the uppercase and it's range. 

```javascript
const regex = /[a-zA-Z]+/;
```

There we go, we have now added the entire alphabet, lower and upper case. We can also do the same thing for numbers. Instead of typing 0123... and so on, we can just do 0-9.

```javascript
const regex = /[a-zA-Z0-9]+/;
```

So now the expression looks for all letters and all numbers but it isn't just limited to that. We can keep adding to what we want to find. Let's say we want to find a `-`, `_`, and a `.`.

```javascript
const regex = /[a-zA-Z0-9_\.-]+/;
```

Remember that the `\.` is being used to to prevent functionality and actually look for a `.`. However in this case, the back slash can be excluded because characters in brackets are literal.

So now we can update our visual understanding of regex.

```javascript
/^([a-z0-9_\.-]+)@([a-z\.-]+)\.([a-z\.])$/
```

Which means to look for lowercase letter, number, underscore, period, or a dash at the beginning of the input. Followed by an @. Then a lowercase letter, period, or dash subexpression. Followed by a period, and then a lowercase letter or period subexpression at the end of the input.

### Quantifiers

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

Quantifers indicate how many characters or expressions we would like to match against. The two quantifiers in our popular regex are: 

- \+
- {2,6}

The + quantifer means that it will match against one or more character before it. For example : 

```javascript
/a+/
```

will match against all sequence of a's in the beginning. But finding one character isn't fun. Let's look for all sequences of the alphabet. 

```javascript
/([a-zA-Z])+/
```

NOTE: this would be mostly be used when the global modifier is disabled. Disabling global and using the `+` quanitfier can grab more specific results. 

Finding the entire sequence is pretty cool. But what if we want to find a certain number of characters. We can do that with curly braces. 

```javascript
/[a-zA-Z]{2, 6}/
```

Now, we are matching against an input that has a min of 2 and a max of 6 characters. To get match exactly 2 characters, we exclude the `6` and `,`. To match against at least 2 and beyond characters, just bring back the `,` but not the 6.  

```javascript
/[a-zA-Z]{2}/
/[a-zA-Z]{2,}/
```

So now we can update our understanding our regex. So far, we should understand 

```javascript
/^([a-z0-9_\.-]+)@([a-z\.-]+)\.([a-z\.]{2,6})$/
```

Summarily, means look for a subexpression, then a @, then a subexpression, then a period, then a end of input that's between 2 - 6 character length.

There are other two other quantifiers not in this expression, such as:

- *
- ?

that are available but not covered. Feel free to to research them in depth on your own. 

### Character Classes

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

The last thing we need to learn, are character classes. Character classes are like shortcuts. When we did the regex...

```javascript
/[a-zA-Z0-9]/
```

although already short, it can be even shorter. We can do `\w` which means find any lower/upper case letter and numbers.

```javascript
/[\w]/
```

But what if we want to find just numbers. There is one for just numbers, and it's `\d`. Which means `0-9`. 

```javascript
/[\d]/
```

There are other character classes and topics not covered in this guide. 

The other character classes not covered are: 

- [^]
- .
- \s
- \t
- \r
- \n
- \v
- \f

As well as the capital versions of each letter, which inverts the selection. Feel free to research them on your time. 

### Final results

```javascript
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/
```

Now that we have learned everything we need to understand this popular regex. We can decipher what the popular expression is trying to find. 

Let's break it down in sections!

```javascript
/^([a-z0-9_\.-]+)/
```

1. We are trying to find in the beginning of an input, a sub expression, which looks for lowercase letters, all single digit numbers, a underscore, a period/dot, and a dash.

```javascript
/@/
```

2. Then look for a @ character

```javascript
/([\da-z\.-]+)/
```

3. Then look for another sub expression, that matches for multiple digits, lowercase letters, a periods/dots, and a dashes.

```javascript
/\./
```

4. Then look for a period.

```javascript
/([a-z\.]{2,6})$/
```

5. Then at the end of the input, look for a min of 2 and a max of 6 characters of lowercase letters and a period. 

This popular expression is meant for emails. Congratulations! You learned quite a bit about regex and how it works. 

## Author

Also, my name is Javier. I am an aspiring full stack developer just working along a bootcamp and creating a gist. Want to check out my journey. Here is my Github! 

GitHub - https://github.com/javieb5186 
