- HTML Character entities are basically a set of characters (entities) which are used to represent reserved characters (by HTML) and some other characters which are not so easily accessible through keyboard.
-  **Commonly Used HTML Entities**

| Symbols | Description                  | Entity name  | Entity Number |
| ------- | ---------------------------- | ------------ | ------------- |
|         | non-breaking space           | `&nbsp;`     | `&#160;`      |
| <       | less than                    | `&lt;`       | `&#60;`       |
| ®       | registered trademark         | `&reg;`      | `&#174;`      |
| ©       | copyright                    | `&copy;`     | `&#169;`      |
| €       | euro                         | `&euro;`     | `&#8364;`     |
| ¥       | yen                          | `&yen;`      | `&#165;`      |
| £       | pound                        | `&pound;`    | `&#163;`      |
| ¢       | cent                         | `&cent;`     | `&#162;`      |
| “       | double quotation mark        | `&quot;`     | `&#34;`       |
| &       | ampersand                    | `&amp;`      | `&#38;`       |
| >       | greater than                 | `&gt;`       | `&#62;`       |
| ∂       | PARTIAL DIFFERENTIAL         | `&part;`     | `&#8706;`     |
| ∃       | THERE EXISTS                 | `&exist;`    | `&#8707;`     |
| ∅       | EMPTY SETS                   | `&empty;`    | `&#8709;`     |
| ∇       | NABLA                        | `&nabla;`    | `&#8711;`     |
| ∈       | ELEMENT OF                   | `&isin;`     | `&#8712;`     |
| ∉       | NOT AN ELEMENT OF            | `&notin;`    | `&#8713;`     |
| +       | PLUS SIGN                    | `&plus;`     | `&#43;`       |
| ∏       | N-ARY PRODUCT                | `&prod;`     | `&#8719;`     |
| ∑       | N-ARY SUMMATION              | `&sum;`      | `&#8721;`     |
| Α       | Alpha                        | `&Alpha;`    | `&#913;`      |
| Β       | Beta                         | `&Beta;`     | `&#914;`      |
| Γ       | Gamma                        | `&Gamma;`    | `&#915;`      |
| Δ       | delta                        | `&Delta;`    | `&#916;`      |
| Ε       | Epsilon                      | `&Epsilon;`  | `&#917;`      |
| Ζ       | Zeta                         | `&Zeta;`     | `&#918;`      |
| ♥       | BLACK HEART SUIT = valentine | `&hearts;`   | `&#9829;`     |
| ♣       | BLACK CLUB SUIT = shamrock   | `&clubs;`    | `&#9827;`     |
| ♠       | BLACK SPADE SUIT             | `&spades;`   | `&#9824;`     |
| ↓       | DOWNWARDS ARROW              | `&darr;`     | `&#8595;`     |
| →       | RIGHTWARDS ARROW             | `&rarr;`     | `&#8594;`     |
| ↑       | UPWARDS ARROW                | `&uarr;`     | `&#8593;`     |
| ←       | LEFTWARDS ARROW              | `&larr;`     | `&#8592;`     |
| ™       | TRADEMARK                    | `&trade;`    | `&#8482;`     |
| ♦       | BLACK DIAMOND SUIT           | ♦            | `&#9830;`     |
| °       | degree                       | `&deg;`      | `&#176;`      |
| ∞       | infinity                     | `&infin;`    | `&#8734;`     |
| ‰       | per-mille                    | `&permil;`   | `&#137;`      |
| ⋅       | dot operator                 | `&sdot;`     | `&#8901;`     |
| ±       | plus-minus                   | `&plusmn;`   | `&#177;`      |
| †       | hermitian                    | `&hercon;`   | `&#8889;`     |
| –       | minus sign                   | `&minus;`    | `&#8722;`     |
| ¬       | –                            | `&not;`      | `&#172;`      |
| %       | percent sign                 | `%percent;`  | `&#37;`       |
| f       | Function                     | `&fnof;`     | `&#402;`      |
| ∥       | parallel                     | `&parallel;` | `&#8741;`     |

- Here, reserved characters: `<, >, &, ", etc.
- **Best Practices for Using HTML Entities**
	- **Use Named Entities When Possible**: Named entities (`&lt;` instead of `&#60;`) are easier to remember and make your code more readable.
	- **Verify Entity Codes****: Always check your entity codes to ensure they display correctly across all browsers and platforms.
	- **Use Entities for Readability and Compliance**: Besides reserved characters, use entities to improve the readability of your code and to comply with HTML standards, especially when dealing with characters that may not be directly supported by your page’s charset.

Next connected topics: [[Diacritical marks]] , [[Non-breaking space]]