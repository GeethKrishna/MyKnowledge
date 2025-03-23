- It's an element which is used to create user input forms. 
- Forms can be created even with normal div's but forms have some additional attributes which are really helpful for both user side and for communication with the server.
	- **action**: Specifies where to send this data (API endpoint to send)
	- **method**: `GET or POST`
		- GET:
			- Appends the form data to the URL as name & value pairs like a query parameters.
			- The length of the URL is limited (approx. 3000 characters).
			- The data is visible on the URL so do not use this for sensitive forms.
			- Useful for situations where the form generates or goes to a page which the user may want to bookmark or share with others.
		- POST:
			- Appends the form data inside the request body.
			- Has no size limitations like GET (technically correct but it's not good to have very large data in a single request).
			- But, submissions with POST cannot be bookmarked or shared.
	- **enctype**: Specifies how the form data should be encoded when submitting the form to the server. (this only relevant to POST method).
		- Example code: 
		- `<form action="/plaintext" method="POST" enctype="text/plain (or) application/x-www-form-urlencoded (or) multipart/form-data">          <input type="text" name="message" placeholder="Enter message">         <input type="submit" value="Send">                                   </form>`

| Value                             | Description                                                                                                                                                                                                  |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| application/x-www-form-urlencoded | Default. All characters are encoded before sent (spaces are converted to "+" symbols, and special characters are converted to ASCII HEX values)                                                              |
| multipart/form-data               | Used for sending large amounts of data, including files. Each field is sent as a separate block (with boundaries), preserving special characters and spaces without encoding. **Required for file uploads.** |
| text/plain                        | Sends data without any encoding at all. Not recommended                                                                                                                                                      |

- **Example HTTP Request:** username is John & Doe
- application/x-www-form-urlencoded:
<p style="color:#F0BB78;border-style:solid;border-color:#ff746c;margin:0;">
POST /submit HTTP/1.1 
<br>Host: example.com 
<br>Content-Type: application/x-www-form-urlencoded 
<br>Content-Length: 15 
<br>
<br>
username=John+%26+Doe
</p>
- multipart/form-data:
<p style="color:#F0BB78;border-style:solid;border-color:#ff746c;margin:0"> 
POST /upload HTTP/1.1
<br>
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryXYZ
<br>
<br>
------WebKitFormBoundaryXYZ
<br>
Content-Disposition: form-data; name="username"
<br>
<br>
John & Jane
<br>
------WebKitFormBoundaryXYZ--
</p>
- text/plain:
<p style="color:#F0BB78;border-style:solid;border-color:#ff746c;margin:0">
POST /submit HTTP/1.1 
<br>Host: example.com 
<br>Content-Type: text/plain 
<br>Content-Length: 15 
<br>
<br>
username=John & Doe
</p>

- We can handle multipart/form-data at backend in two ways:
	1. **Streaming**: We can read the data like a stream _chunk by chunk_  and _pipe_ it to storage to save it or to any other service. 
		- Can be done using: `fs.createWriteStream`, `busboy`or`multer` with streams.
		- Advantages are no need to store the file in memory, **efficient for large files** (doesn't block the event loop) and supports piping to cloud storage.
	2. **Converting into a string**: Buffers the entire thing in memory first and then converts it into a readable / usable format.
		- Can be done using `multer` in memory, `express.urlencoded()`
		- Best for processing **text files or small payloads.**
		- We should be careful cause large files handled like this may crash the system.