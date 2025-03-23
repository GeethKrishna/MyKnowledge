- This tag is used to embed an image into the page.
- It has two main attributes:
	- **src**: Specifies the path of the image.
	- **alt**: Specifies a text for the image to be displayed if the image for some reason cannot be displayed. As a good practice always try to include this in the element.
- Always specify the width and the height of an image. If they are not specified, the page might flicker while the image loads causing Cumulative Layout Shift (CLS).
- While making our page responsive, rendering a single image for all screen sizes may not be the correct choice. For that we can leverage the power of `srcset` attribute to give multiple sources to an image and let it render according to the media sizes.
- We can implement this in two ways:
1. **Using picture element:**
```
<picture>  
  <source media="(min-width:650px)" srcset="img_pink_flowers.jpg">  
  <source media="(min-width:465px)" srcset="img_white_flower.jpg">  
  <img src="img_orange_flowers.jpg" alt="Flowers" style="width:auto;">  
</picture>
```
- As we can see we can have multiple source elements for multiple media sizes with the `srcset` attribute which contains different URL's of the image to be shown for different media sizes.
- The last `img` tag is fallback image if any of the above `source` URL's fail.
- The `<picture>` element contains multiple `<source>` elements, each specifying:
    - A **media query** (e.g., `(min-width: 1024px)`) that determines when this source should be used.
    - A `srcset` attribute, which provides the image URL to be loaded if the media query matches.
- The browser selects the first `<source>` that matches the current viewport size.
- If **none of the `<source>` elements match**, the `<img>` tag acts as a **fallback** and loads `img_default.jpg`.
- We can use different photos of different formats too in each source element.

2. **Using `srcset` attribute directly in the `img` element:**
```
<img 
  src="img_default.jpg" 
  srcset="img_large.jpg 1024w, img_medium.jpg 768w, img_small.jpg 480w" 
  sizes="(max-width: 480px) 100vw, (max-width: 768px) 50vw, 33vw" 
  alt="A beautiful landscape"
>
```
- Instead of using the `<picture>` element, we can define multiple image sources directly inside the `img` tag using the `srcset` attribute. The `srcset` attribute allows us to specify different image files for different screen sizes or resolutions, ensuring optimal image loading based on the device.
- The `src` attribute provides a default fallback image (`img_default.jpg`), which is used if none of the `srcset` images are supported.
- The `srcset` attribute defines multiple image sources, each followed by a **descriptor**:
    - `img_large.jpg 1024w`: Used when the viewport width is large enough.
    - `img_medium.jpg 768w`: Used when the viewport width is medium.
    - `img_small.jpg 480w`: Used for smaller screens.
- The `sizes` attribute tells the browser **how much space the image will occupy** in different viewport sizes:
    - If the viewport width is up to **480px**, the image takes `100vw` (100% of the viewport width).
    - If the viewport width is up to **768px**, the image takes `50vw` (50% of the viewport width).
    - Otherwise, it takes **33vw** (33% of the viewport width).
- **Note: We can use only images of a single format here.**
- When to Use `srcset`?
	- Use `srcset` when you need **different image resolutions for different devices** to optimize loading time and performance.
	- Works well for **responsive images**, helping reduce **unnecessary bandwidth usage** by serving smaller images for mobile screens.


### **Comparison: `<picture>` vs `srcset` in `<img>`**

| Feature                                      | `<picture>` | `srcset` in `<img>` |
| -------------------------------------------- | ----------- | ------------------- |
| Supports different formats (e.g., WebP, PNG) | ✅ Yes       | ❌ No                |
| Uses media queries for different sizes       | ✅ Yes       | ✅ Yes               |
| Works for high-resolution displays           | ✅ Yes       | ✅ Yes               |
| Simpler syntax                               | ❌ No        | ✅ Yes               |
