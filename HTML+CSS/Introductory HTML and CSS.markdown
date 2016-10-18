# Introductory HTML and CSS

This document is an attempt to give some of the main background bits to understanding HTML and CSS. It is far from comprehensive, but aims to be approachable and give enough background to understand more complex topics…later.

## Server and Browser

A very basic overview:

- The browser sends a **request** for a URL to a server. The URL includes a host, a path, and a query string, and the full request includes a set of HTTP headers.
  - The request is sent via the HTTP protocol, which sends basically a text file to the server.
  - The first part of the file is [**headers**](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields), which have some metadata about the request.
    - This includes information about the browser, timestamps, whether it is a GET, POST, or other request, the format the request expects, etc.
  - After a blank line, the rest of the file is any **body** of the request that might exist.
- The **response** is sent by the server in much the same format, with the **body** section containing the HTML page or image data or whatever.
- Note that there is no **necessary** correlation between files on disk and the URL. In other words, `http://example.com/important_file.html` could actually retrieve a file on disk called `/stuff/not_important.html`. Further, there may not even be a file on disk that represents the HTML, if you content is being dynamically generated. It's all up to how the HTTP server is configured!

Example request/response (from [Wikipedia](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Example_session))

### Request

```
GET /index.html HTTP/1.1
Host: www.example.com
```

### Response

```
HTTP/1.1 200 OK
Date: Mon, 23 May 2005 22:38:34 GMT
Content-Type: text/html; charset=UTF-8
Content-Encoding: UTF-8
Content-Length: 138
Last-Modified: Wed, 08 Jan 2003 23:11:55 GMT
Server: Apache/1.3.3.7 (Unix) (Red-Hat/Linux)
ETag: "3f80f-1b6-3e1cb03b"
Accept-Ranges: bytes
Connection: close

<html>
<head>
  <title>An Example Page</title>
</head>
<body>
  Hello World, this is a very simple HTML document.
</body>
</html>
```

Browsers will make further requests depending on the content of the page, but all requests take this basic format. (Well, WebSockets and HTTP 2 change that, but that's a different discussion.)

### Miscellaneous Notes

- Browsers typically limit the number of requests to a single domain they will make at once…perhaps the limit is currently 8 or 32?
- You can see the request/response cycle in the browser's dev tools.


## Tags (HTML)

### Theory

- Used to indicate structure or meaning or "semantics" of a document. (so it's not just a flow of words)
- Browsers give default styles to tags, but everything can be overriden.
  - (You'll want to use a CSS reset to normalize the browser's default styles.)
- As much as possible, don't add tags just to change the appearance if it doesn't change the meaning.
- [Mozilla has a nice reference of all the available tags.](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

#### Why "Meaning of the document"?

Imagine you have some text. Using this section as an example, the raw text might look like this (abbreviated):

```
Tags (HTML)

Theory

Used to indicate…

Practice

Tags have parent-children relationship…
```

It's pretty difficult to navigate what those different bits of text are doing. How is it organized? If you were writing by hand, you might use larger and smaller writing, underlining, or different colors to communicate the organization and meaning. If using a word processor, you'd probably use some formatting features. If you only had plain-text, you might use something like Markdown:

```
# Tags (HTML)

## Theory

- Used to indicate…

## Practice

- Tags have parent-children relationship…
```

HTML is just another method of trying to communicate that structure, though primarily for **computer** consumption rather tha human consumption. A web  browser takes that *meaning* and uses its default styles to show it to the user in a way that makes sense to humans.

```
<h1>Tags (HTML)</h1>

<h2Theory</h2>

<ul>
  <li>Used to indicate…</li>
</ul>

<h2>Practice</h2>

<ul>
  <li>Tags have parent-children relationship…</li>
</ul>
```

### Practice

- Tags have **parent-children relationship**, and structure something like a family tree.
  - This relationship matters! Some tags cannot be children of other tags, and tags will frequently depend on their parent for some attributes of appearance or behavior.
- Tags may come in **open-close pairs** that surround content `<p>text</p>` or be "self-closing" `<input type="text" />`. While the HTML spec is not strict, I'd recommend **closing all tags**, either with a closing tag or by self-closing.
  - A few tags cannot have any content, such as the `<img />` tag.
- Tags can have **attributes** that can alter the default functionality of the tag or provide further context (i.e. `<p class="important">` might indicate how important the paragraph is).
  - Attributes live inside the angle brackets after the tag name, with a label, an equals sign, and a value.
  - Some true/false attributes *only* need the label of the attribute.
  - Values technically don't need quotes, but it's simpler. Double or single is fine.
- **Text** usually just shows up. It *should* probably be wrapped inside a tag, but doesn't have to be.
    - Spaces are collapsed—HTML enforces single spaces between words and sentences.
    - Use [HTML entities](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references) for `<` (`&lt;`) or `>` (`&gt;`).
- Browsers are (somewhat unfortunately) very forgiving and will try guess what tags you *meant* to have. Validation tools are recommended, especially ones built right in to your editor.

### Default Styles

It isn't true for every tag (with the form elements being the most notable exception), but it is often true that the only difference between one tag and another is the default styling that the browser provides.

(You'll likely want to include [Normalize.css](https://necolas.github.io/normalize.css/) to try make the browser default styles match across browsers. Bootstrap already includes this by default.)

### Browser Development Tools

While all major browser include quality developer's tools, I will focus on [Chrome's DevTools](https://developers.google.com/web/tools/chrome-devtools/). Each browser's tools has their own strengths and weaknesses, and it isn't too uncommon to use more than one set when debugging certain issues.

- Right-click on a page element and choose "Inspect Element". This will open the dev tools in most browsers, and focus that particular element in the element tree.
  - This is a live-updating representation of the HTML on your page, including any changes that JavaScript has made.
  - You can see the tree structure of the HTML DOM.
  - You can highlight any given element to see information about the CSS rules applied to that node, JavaScript event listeners, and more.
  - You can also live-edit the page—though of course your edits don't get saved. (Well, if you are working on something you have access to, there is theoretically a way for the changes to get saved…but I've never set that up.)
- Get applied style information.
  - When you have an element focused in the Elements pane, you can look at the currently applied styles.
  - This shows **all** styles, organized with the most specific ones at the top, from any location that applies to this element, including the browser's default "user agent stylesheet".
  - There is also a graphic showing a bit of the box model, the margin, border, and padding applied to the element.
- Get computed style information.
  - Sometimes, even when you see all the applied styles, you just want to know what the **end result** of applying all those styles was. That's what the "Computed" tab is for.
  - There is sometimes a disclosure triangle to the left of the properties, allowing you to trace back which CSS rules were used to make this computation.
- [Lots more in the Chrome documentation](https://developer.chrome.com/devtools/docs/elements-styles).

### Useful `<head>` Tags

Note that the `<head>` tag is has CSS `display: none` by default…but even that can be overridden!

- `<title>` is required
- `<script>` is used for loading external JavaScript files…but, you probably want to put this **after** the closing `</body>` tag. Otherwise, your script will run **before** all the HTML is rendered on the screen.
- `<link>` is used to pull in external CSS stylesheets.
- There are lots of `<meta>` tag options that you can use for various extra indications to the browser, such as the character encoding (if not in the HTTP headers) or mobile device indications or favicons, etc.

## Selectors (CSS)

> ### TODO!!

## Styles (CSS)

## Inheritance (CSS)

Most text properties (font, size, etc.) get inherited, but most other properties do not. Look up the CSS property (or just try it!) to find out for sure.

## Cascading—which rule takes precedence if many apply (CSS)

- Top to bottom.
- The more "specific" the selector (i.e. adding up points)
  - Tags worth 1 point
  - Classes worth 10 points
  - IDs worth 100 points
  - inline styles worth 1000 points

## Units (CSS)

- **percentages**: based on width of parent element (even when top and bottom!)

## Box Model

- content, padding, border, margin
- collapsing margins (two top-to-bottom margins next to each other, takes largest)
- negative margins
-
  - **block**
    - break before and after
    - full width of viewport (not sure how exactly specified: 100%? 100vw? other?)
    - height via CSS **or** content
    - `<div />` by default, though can change
  - **inline**
    - no break, same line as content before & after
    - left/right padding, left/right margins
    - `<span />` by default, though can change
   - **inline-block**
    - like `inline`, but accepts top/bottom margin, top/bottom padding, height


## Tools

If you don't have a web server set up, you can use the following Python snippet to start one in the current directory (**don't use this in production!**):

```
python -m SimpleHTTPServer
```

And then access your pages at [http://localhost:8000](http://localhost:8000).