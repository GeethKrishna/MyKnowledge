- This tag is used to embed client side script (JavaScript file) into the HTML file.
- The tag either contains the scripting statements or points to file containing these statements through `src` attribute.
- Some good to know attributes are:
	1. **async**: This keyword specifies that the script can be downloaded parallel to parsing the HTML document and execute as soon as the download is complete even if the parsing is still in process. (only for external scripts)
		- This is used mostly to not block the page rendering for any script to be loaded. You want your initial paint to be as fast as possible.
	2. **defer**: This keyword specifies that the script to be downloaded parallelly to parsing of page and can be executed after the page has be finished parsing.(only for external scripts)
- With these two attributes we now can place the `<script>` element on top at the `<head>` tag with no problem. Generally, people prefer to place the `<script>` tag at the end of the page in the end of `<body>` tag because the `<script>` tag stops parsing of the HTML to download and execute the script first which may lead to unexpected behaviors in the page. 
- Using `async & defer` we can place the `<script>` tag anywhere we want in the page, preferably at the top in the `<head>` tag so that the download process begins the earliest.


```
<script src="path/to/script1.js" async></script>
<script src="path/to/script2.js" async></script>
```
- **Note**: Scripts with the async attribute are executed asynchronously. This means the script is executed as soon as it's downloaded, without blocking the browser in the meantime. This implies that it's possible that script 2 is downloaded and executed before script.**** Order of execution is not guaranteed.****

```
<script src="path/to/script1.js" defer></script>
<script src="path/to/script2.js" defer></script>
```
- Scripts with the defer attribute are executed in order (i.e. first script 1, then script 2). This also does not block the browser.
- Unlike async scripts, defer scripts are only executed after the entire document has been loaded.

```
<script type="module" src="path/to/srcipt-module.js"></script>
```
- Scripts that include `type="module"` will be treated as JavaScript modules and are loaded like `'defer'red` scripts.

```
<script src="script.js" async defer></script>
```
- **`async defer`** script falls back to the defer behavior if async is not supported.

For more discussion refer this: https://stackoverflow.com/questions/436411/where-should-i-put-script-tags-in-html-markup