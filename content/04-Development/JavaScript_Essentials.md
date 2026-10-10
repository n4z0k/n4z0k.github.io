---
author: Alfeze
created: 2026-10-07
---

# JavaScript Essentials

> JavaScript (JS) is a popular scripting language that allows web developers to add interactive features to websites containing HTML and CSS (styling). JavaScript is a interpreted language.

---
## JS Overview 

```javascript
 // Hello, World! program
console.log("Hello, World!");

// Variable and Data Type
let age = 25; // Number type

// Control Flow Statement
if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}

// Function
function greet(name) {
    console.log("Hello, " + name + "!");
}

// Calling the function
greet("Bob");
```

## Variables

There are three ways to declare variables in JS: `var`, `let`, and `const`. 

While `var` is function-scoped, both `let`, and `const` are block-scoped, offering better control over variable visibility within specific code blocks.


`Ctrl + Shift + I` to open the `Console` or right-click anywhere on the page and select `Inspect`


```javascript
let x = 5;
let y = 10;
let result = x + y;
console.log("The result is: " + result);
```

## Integrating JS in HTML 

- Internal JavaScript 
`index.html`

```html
 <!DOCTYPE html>
<html lang="en">
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script>
        let x = 5;
        let y = 10;
        let result = x + y;
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```

- External JavaScript
`script.js`

```javascript
let x = 5;
let y = 10;
let result = x + y;
document.getElementById("result").innerHTML = "The result is: " + result;
```

`external.html`

```html
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <!-- Link to the external JS file -->
    <script src="script.js"></script>
</body>
</html>
```

## Dialogue Functions

### Alert

The alert function displays a message in a dialogue box with an "`OK`" button, typically used to convey information or warnings to users. For example, if we want to display "`Hello THM`" to the user, we would use an `alert("HelloTHM");`.

### Prompt

```console
 name = prompt("What is your name?");
    alert("Hello " + name);
```

### Confirm

```console
confirm("Do you want to proceed?")
```

## Obfuscation 

Obfuscate the JS code using an online tool :

[JavaScript Obfuscator Online: JS Code Obfuscator](https://codebeautify.org/javascript-obfuscator)


Deobfuscator tool :

[Obfuscator.io Deobfuscator](https://obf-io.deobfuscate.io/)


> [!TIP]
> Minifying and obfuscating JS code reduces its size, improves load time, and makes it harder for attackers to understand the logic of the code. Therefore, always **minify** and **obfuscate** the code when using code in production. The attacker can eventually reverse engineer it, but getting the original code will take at least some effort.


---
## Related 

[[MOC_Development|Development]]




