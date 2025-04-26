# markdown-it-toc

> This project is forked from [samchrisinger/markdown-it-toc](https://www.npmjs.com/package/markdown-it-toc) (original repository has been removed by the author), with security fixes implemented. See [Security Fixes (v1.1.1)](#security-fixes-v111) for details.

> markdown-it plugin for adding a table of contents to markdown documents

---

## Security Fixes (v1.1.1)

### XSS Vulnerability Patches
Addressed potential HTML injection vulnerabilities through comprehensive escaping:

#### 1. TOC Title Escaping
Sanitize user-provided TOC titles using Markdown's built-in HTML escaper:
```javascript
const escapedTitle = md.utils.escapeHtml(tokens[index].content);
```

#### 2. Heading Content Escaping
All heading contents in TOC entries are now properly sanitized:
```javascript
const escapedContent = md.utils.escapeHtml(heading.content);
```

#### 3. Defense-in-Depth Anchors
Maintained anchor sanitization while adding extra protections:
```javascript
// makeSafe() removes special chars + HTML escaping
anchor: makeSafe(escapedContent) + '_' + heading.map[0]
```

#### 4. Early Content Sanitization
Added proactive escaping when setting token content to establish multiple defense layers:
```javascript
// In toc() function:
token.content = md.utils.escapeHtml(label); // Escaped at source

---

## Usage

#### Enable plugin

```js
var md = require('markdown-it')({
  html: true,
  linkify: true,
  typography: true
}).use(require('markdown-it-toc')); // <-- this use(package_name) is required
```

#### Example

```md
@[toc](Title)
```

Adding this tag with add anchors to each ```<h[n]>``` tag on your document, and will add a ```<ul>``` of hyperlinks pointing to these places on the page.

The end results looks like:

```html
<p>
     <h3>Title</h3>
     <ul>
	<li><a href="...">Heading 1</a></li>
	...
	... 
     </ul> 
</p>
...
...
<h1><a href="..."></a>Heading 1</h1>
```

### Testing

To run the tests use:
```bash
make test
```