# Encode URI

# **encodeURIComponent**
> The `**encodeURIComponent()**` function encodes a [URI](https://developer.mozilla.org/en-US/docs/Glossary/URI) by replacing each instance of certain characters by one, two, three, or four escape sequences representing the [UTF-8](https://developer.mozilla.org/en-US/docs/Glossary/UTF-8) encoding of the character (will only be four escape sequences for characters composed of two "surrogate" characters).



It's commonly applied to a quring string parameter with a link type. E.g. https://www.mysite.com/login?redirectUrl=https%3A%2F%2Fwww.other.com%3Fa%3Db%26c%3Dd&mysiteParam1=a&mysiteParam2=b


In the example link, mysite.com has three query params { `redirectUrl,` `mysiteParam1`, `mysiteParam2` } which are separated by `&`. The original `redirectUrl` link is https://www.other.com?a=b&c=d. The `=` and `&` in the original redirect link, if not encoded, will confuse with the query params of mysite.com.


# Versus encodeURI


`encodeURIComponent(value)` is mainly used to encode queryString parameter values, and it encodes every applicable character in `value`. `encodeURI` ignores protocol prefix (`http://`) and domain name. If you're encoding a string to put in a URL component (a query string parameter), you should use **encodeURIComponent**, and if you're encoding an existing URL, use **encodeURI**. It's simple


```javascript
const link = 'http://www.google.com/a b/c_d?a=b&c=d'

// http://www.google.com/a%20b/c_d?a=b&c=d
encodeURI(link); 
// http%3A%2F%2Fwww.google.com%2Fa%20b%2Fc_d%3Fa%3Db%26c%3Dd
encodeURIComponent(link);
```


