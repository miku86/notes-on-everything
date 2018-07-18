# DOM Enlightment

# Introduction

This book is not an exhaustive reference on DOM scripting or JavaScript. It may be the most exhaustive book written about DOM scripting without the use of a library/framework. The lack of authorship around this topic is not without good reason. Most technical authors are not willing to wrangle this topic because of the differences that exist among legacy browsers and their implementations of the DOM specifications (or lack thereof).

For the purpose of this book, I'm going to sidestep the browser API mess and dying browser discrepancies in an effort to expose the modern DOM. That's right, I'm going to sidestep the ugliness in an effort to focus on the here and now. After all, we have solutions like jQuery to deal with all that browser ugliness, and you should definitely be leveraging something like jQuery when dealing with deprecated browsers.

While I am not promoting the idea of only going native when it comes to DOM scripting, I did write this book in part so that developers may realize that DOM libraries are not always required when scripting the DOM. I also wrote for the lucky few who get to write JavaScript code for a single environment (i.e. one browser, mobile browsers, or HTML+CSS+JavaScript-to-native via something like PhoneGap). What you learn in this book may just make a DOM library unnecessary in ideal situations, say for example, some light DOM scripting for deployment on a Webkit mobile browser only.

## Who should read this book

I specifically had two types of developers in mind. I assume both types already have an intermediate to advanced knowledge of JavaScript, HTML, and CSS. The first developer is someone who has a good handle on JavaScript or jQuery, but has really never taken the time to understand the purpose and value of a library like jQuery (the reason for its rhyme, if you will). Equipped with the knowledge from this book, that developer should fully be able to understand the value provided by jQuery for scripting the DOM. And not just the value, but how jQuery abstracts the DOM and where and why jQuery is filling the gaps. The second type of developer is an engineer who is tasked with scripting HTML documents that will only run in modern browsers or that will get ported to native code for multiple OS's and device distributions (e.g. PhoneGap) and needs to avoid the overhead (i.e. size or size v.s. use) of a library.

# Preface

## This book is not like other programming books

The book is written in a style that favors small, isolated, immediately executable code over wordy explanations and monolithic programs. One of my favorite authors, C.S Lewis, asserts that words are the lowest form of communication that humans traffic in. I totally agree with this assertion and use it as the basis for the style of these books.

I feel that technical information is best covered with as few words as possible, in conjunction with just the right amount of executable code and commenting required to express an idea. The style of this book attempts to present a clearly defined idea with as few words as possible, backed with real code.

Because of this, you should execute and examine the code, thereby forming the foundation of a mental model for the words used to describe the concepts. Additionally, the format of these books attempts to systematically break ideas down into their smallest possible form and examine each one in an isolated context. All this to say that this is not a book with lengthy explanations or in-depth coverage on broad topics.

---

# Chapter 1 - Node Overview

## 1.1 The Document Object Model (aka DOM) is a hierarchy/tree of JS node objects

When you write an HTML document you encapsulate HTML content inside of other HTML content. By doing this you setup a hierarchy that can be expressed as a tree. Often this hierarchy or encapsulation system is indicated visually by indenting markup in an HTML document. The browser when loading the HTML document interrupts and parses this hierarchy to create a tree of node objects that simulates how the markup is encapsulated.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<title>HTML</title>
</head>
<body>
<!-- Add your content here-->
</body>
</html>
```

The above HTML code when parsed by a browser creates a document that contains nodes structrured in a tree format (i.e. DOM).

For example, the `body` element is an element node and an instance of the `HTMLBodyElement` interface.

What you should take away here is that html documents get parsed by a browser and converted into a tree structure of node objects representing a live document. The purpose of the DOM is to provide a programatic interface for scripting (removing, adding, replacing, eventing, modifiying) this live document using JavaScript.

## 1.2 Node object types

The most common types of nodes one encounters when working with HTML documents are:

- DOCUMENT_NODE (e.g. window.document)
- ELEMENT_NODE (e.g. `<body>, <a>, <p>, <script>, <style>, <html>, <h1>` etc...)
- ATTRIBUTE_NODE (e.g. `class="funEdges"`)
- TEXT_NODE (e.g. text characters in an html document including carriage returns and white space)
- DOCUMENT_FRAGMENT_NODE (e.g. `document.createDocumentFragment()`)
- DOCUMENT_TYPE_NODE (e.g. `<!DOCTYPE html>`)

I've listed the node types above formatted exactly as the constant property is written in the JavaScript browser environment as a property of the `Node` object. These `Node` properties are constant values and are used to store numeric code values which map to a specific type of node object. For example, `Node.ELEMENT_NODE` is equal to 1. And 1 is the code value used to identify element nodes.

What I hope you take away is that the `nodeType` value (i.e. 1) is just a numeric classificaiton used to describe a certain type of node constructed from a certain JavaScript interface/constructor. For example, the `HTMLBodyElement` interface represents a node object that has a node type of 1, which is a classification for `ELEMENT_NODE`'s.

## 1.3 Sub-node objects inherit from the Node object

Each node object in a typical DOM tree inherits properties and methods from `Node`. Depending upon the type of node in the document there are also additional sub node object/interfaces that extend the `Node` object. Below I detail the inheritance model implemented by browsers for the most common node interfaces (< indicates inherited from).

- Object < Node < Element < HTMLElement < (e.g. HTML\*Element)
- Object < Node < Attr (deprecated in DOM 4)
- Object < Node < CharacterData < Text
- Object < Node < Document < HTMLDocument
- Object < Node < DocumentFragment

Its important not only to remember that all nodes types inherit from `Node` but that the chain of inheritance can be long. For example, all `HTMLAnchorElement` nodes inherit properties and methods from `HTMLElement, Element, Node, and Object` objects.

`Node` is just a JavaScript constructor function. And so logically `Node` inherits from `Object.prototype` just like all objects in JavaScript.

You will see a long list of properties that are available to the element node object. The properties & methods inherited from the `Node` object are in this list as well as a great deal of other inherited properties and methods from the `Element, HTMLElement, HTMLAnchorElement, Node, and Object` object.

Its not my point to examine all of these properties and methods now but simply to point out that all nodes inherit a set of baseline properties and methods from its constructor as well as properties from the prototype chain.

## 1.4 Properties and methods for working nodes

Like we have been discussing all node objects (e.g `Element, Attr, Text` etc...) inherit properties and methods from a primary `Node` object. These properties and methods are the baseline values and functions for manipulating, inspecting, and traversing the DOM.

In addtion to the properties and methods provided by the node interface there are a great deal of other relevant properties and methods that are provided by sub node interfaces such as the `document, HTMLElement, or HTML*Element` interface.

Below I list out the most common `Node` properties and methods inherited by all node objects including the relevant inherited properties for working with nodes from sub-node interfaces.

Node Properties:

- `childNodes`
- `firstChild`
- `lastChild`
- `nextSibling`
- `nodeName`
- `nodeType`
- `nodeValue`
- `parentNode`
- `previousSibling`

Node Methods:

- `appendChild()`
- `cloneNode()`
- `compareDocumentPosition()`
- `contains()`
- `hasChildNodes()`
- `insertBefore()`
- `isEqualNode()`
- `removeChild()`
- `replaceChild()`

Document Methods:

- `document.createElement()`
- `document.createTextNode()`

HTML \* Element Properties:

- `innerHTML`
- `outerHTML`
- `textContent`
- `innerText`
- `outerText`
- `firstElementChild`
- `lastElementChild`
- `nextElementChild`
- `previousElementChild`
- `children`

HTML element Methods:

- `insertAdjacentHTML()`

## 1.5 Identifying the type and name of a node

Every node has a `nodeType` and `nodeName` property that is inherited from `Node`. For example Text nodes have a `nodeType` code of 3 and `nodeName` value of '#text'. As previously mentioned the numeric value 3 is a numeric code representing the type of underlying object the node represents (i.e. Node.TEXT_NODE === 3).

It makes sense to simply memorize these numeric code's for the more common nodes given that we are only dealing with 5 numeric codes.

## 1.6 Getting a nodes value

The `nodeValue` property returns null for most of the node types (except Text and Comment). It's use is centered around extracting actual text strings from Text and Comment nodes.

## 1.7 Creating element and text nodes using JavaScript methods

When a browser parses an HTML document it constructs the nodes and tree based on the contents of the HTML file. The browser deals with the creation of nodes for the intial loading of the HTML document. However its possible to create your own nodes using JavaScript. The following two methods allow us to programatically create Element and Text nodes using JavaScript:

- `createElement()`
- `createTextNode()`

```js
var elementNode = document.createElement('div');
console.log(elementNode, elementNode.nodeType); //log <div> 1, and 1 indicates an element node

var textNode = document.createTextNode('Hi');
console.log(textNode, textNode.nodeType); //logs Text {} 3, and 3 indicates a text node
```

## 1.8 Creating and adding element and text nodes to the DOM using JS strings

The `innerHTML, outerHTML, textContent and insertAdjacentHTML()` properties and methods provide the functionality to create and add nodes to the DOM using JavaScript strings.

```js
//create a strong element and text node and add it to the DOM
document.getElementById('A').innerHTML = '<strong>Hi</strong>';

//create a div element and text node
document.getElementById('B').outerHTML = '<div id="B">Whats Shaking</div>';

//create a text node and update the #C with the text node
document.getElementById('C').textContent = 'dude';
```

The `insertAdjacentHTML()` method, which only works on Element nodes, is a good deal more precise than the previously mentioned methods. Using this method its possible to insert nodes before the beginning tag, after the beginning tag, before the end tag, and after the end tag.

## 1.9 Extracting parts of the DOM tree as JavaScript strings

The same exact properties (`innerHTML, outerHTML, textContent`) that we use to create and add nodes to the DOM can also be used to extract parts of the DOM (or really the entire DOM) as a JavaScript string. In the code example below I use these properties to return a string value containing text and html values from the HTML document.

```js
console.log(document.getElementById('A').innerHTML); //logs '<i>Hi</i>'
console.log(document.getElementById('A').outerHTML); //logs <div id="A">Hi</div>
// all text is returned even if its in child element nodes (i.e. <strong> !</strong>)
console.log(document.getElementById('B').textContent); //logs 'Dude !'
```

## 1.10 Adding node objects to the DOM using appendChild()& insertBefore()

The `appendChild()` and `insertBefore()` Node methods allow us to insert JavaScript node objects into the DOM tree.

The `appendChild()` method will append a node(s) to the end of the child node(s) of the node the method is called on. If there are no child node(s) then the node being appended is appended as the first child.

In the code below we are creating a element node (`<strong>`) and text node (Dude). Then the `<p>` is selected from the DOM and our `<strong>` element is appended using `appendChild()`. Notice that the `<strong>` element is encapsulated inside of the `<p>` element and added as the last child node. Next the `<strong>` element is selected and the text 'Dude' is appended to the `<strong>` element.

```js
//create a blink element node and text node
var elementNode = document.createElement('strong');
var textNode = document.createTextNode(' Dude');

//append these nodes to the DOM
document.querySelector('p').appendChild(elementNode);
document.querySelector('strong').appendChild(textNode);
```

When it becomes necessary to control the location of insertion beyond appending nodes to the end of a child list of nodes we can use `insertBefore()`.

```js
//create a text node and li element node and append the text to the li
var text1 = document.createTextNode('1');
var li = document.createElement('li');
li.appendChild(text1);

//select the ul in the document
var ul = document.querySelector('ul');

// add the new li element to the DOM using ul.firstChild
ul.insertBefore(li, ul.firstChild);
```

## 1.11 Removing and replacing nodes using removeChild() and replaceChild()

Removing a node from the DOM is a bit of a multi-step process. First you have to select the node you want to remove. Then you need to gain access to its parent element typically using the `parentNode` property. Its on the parent node that you invoke the `removeChild()` method passing it the reference to the node to be removed.

```js
//remove element node
var divA = document.getElementById('A');
divA.parentNode.removeChild(divA);
```

```js
//replace element node
var divA = document.getElementById('A');
var newSpan = document.createElement('span');
newSpan.textContent = 'Howdy';
divA.parentNode.replaceChild(newSpan, divA);
```

## 1.12 Cloning nodes using cloneNode()

Using the `cloneNode()` method its possible to duplicate a single node or a node and all its children nodes.

`var cloneUL = document.querySelector('ul').cloneNode();`

To clone a node and all of its children nodes you pass the `cloneNode()` method a parameter of of true.

`var cloneUL = document.querySelector('ul').cloneNode(true);`

## 1.13 Grokking node collections (i.e. Nodelist & HTMLcollection)

When selecting groups of nodes from a tree or accessing pre-defined sets of nodes, the nodes are either placed in a `NodeList` (e.g. `document.querySelectorAll('*')`) or `HTMLCollection` (e.g. document.scripts). These array like (i.e. not a real Array) object collections that have the following characteristics:

- A collection can either be live or static. Meaning that the nodes contained in the collection are either literally part of the live document or a snapshot of the live document.
- By default nodes are sorted inside of the collection by tree order. Meaning the order matches the liner path from tree trunk to branches.
- The collections have a length property that reflects the number of elements in the list

## 1.14 Gettting a list/collection of all immediate child nodes

Using the `childNodes` property produces an array like list (i.e. `NodeList`) of the immediate child nodes. Below I select the `<ul>` element which I then use to create a list of all of the immediate child nodes contained inside of the `<ul>`.

`var ulElementChildNodes = document.querySelector('ul').childNodes;`

## 1.15 Convert a NodeList or HTMLCollection to JavaScript Array

Node lists and html collections are array like but not a true JavaScript array which inherits array methods. We programtically confirm this using `isArray()`.

`console.log(Array.isArray(document.links));`

Converting a node list and html collection list to a true JavaScript array can provide a good deal of advantages.

First it gives us the ability to create a snapshot of the list that is not tied to the live DOM considering that `NodeList` and `HTMLCollection` are live lists.

Secondly, converting a list to a JavaScript array gives access to the methods provided by the Array object (e.g. `forEach, pop, map, reduce` etc...).

In ES6 we have `Array.from` to look forward to which converts a single argument that is an array-like object or list (eg. arguments, NodeList, DOMTokenList, NamedNodeMap) into a `new Array()` and returns it.

## 1.16 Traversing nodes in the DOM

From a node reference (i.e. `document.querySelector('ul')`) its possible to get a different node reference by traversing the DOM using the following properties:

- `parentNode`
- `firstChild`
- `lastChild`
- `nextSibling`
- `previousSibling`

If you have been around the DOM much then it should be no surprise that traversing the DOM includes not just traversing element nodes but also text and comment nodes, and this is not exactly ideal. Using the following properties we can traverse the DOM ignoring text and comment nodes:

- `firstElementChild`
- `lastElementChild`
- `nextElementChild`
- `previousElementChild`
- `children`

## 1.17 Verify a node position in the DOM tree with contains() & compareDocumentPosition()

It's possible to know if a node is contained inside of another node by using the `contains()` Node method.

```js
// is <body> inside <html lang="en"> ?
var inside = document
  .querySelector('html')
  .contains(document.querySelector('body'));
```

If you need more robust information about the position of a node in the DOM tree in regards to the nodes around it you can use the `compareDocumentPosition()` Node method. Basically this method gives you the ability to request information about a selected node relative to the node passed in. The information that you get back is a number.

## 1.18 How to determine if two nodes are identical

Two nodes are equal if and only if the following conditions are satisfied:

- The two nodes are of the same type.
- The following string attributes are equal: `nodeName, localName, namespaceURI, prefix, nodeValue`. That is: they are both null, or they have the same length and are character for character identical.
- The `attributes NamedNodeMaps` are equal. That is: they are both null, or they have the same length and for each node that exists in one map there is a node that exists in the other map and is equal, although not necessarily at the same index.
- The `childNodes NodeLists` are equal. That is: they are both null, or they have the same length and contain equal nodes at the same index. Note that normalization can affect equality; to avoid this, nodes should be normalized before being compared.

Calling the `isEqualNode()` method on a node in the DOM will ask if that node is equal to the node that you pass it as a parameter.

---