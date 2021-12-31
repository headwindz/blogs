# [Mixed Content](https://developer.mozilla.org/en-US/docs/Web/Security/Mixed_content)

> An HTTPS page that includes content fetched using cleartext HTTP is called a mixed content page. Pages like this are only partially encrypted, leaving the unencrypted content accessible to sniffers and man-in-the-middle attackers

There are two types of mixed content: `mixed passive/display content` and `mixed active content`

## Mixed passive/display content

> Mixed passive/display content is content served over HTTP that is included in an HTTPS webpage, but that cannot alter other portions of the webpage

* \<img\> (src attribute)
* \<audio\> (src attribute)
* \<video\> (src attribute)
* \<object\> subresources

## Mixed active content

> Mixed active content is content that has access to all or parts of the Document Object Model of the HTTPS page

* \<script\> (src attribute)
* \<link\> (href attribute)
* \<iframe\> (src attribute)
* fetch requests

## To avoid mixed content

* Always use `https` resources - Loading `https` resources on `http` page is valid while the reverse is not.
* Use relative protocol urls - requests the resource from whatever protocol the browser is viewing that current page through

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.
