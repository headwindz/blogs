Frontend developers are all familiar with [the anchor element <a>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a). I was recently struggling with a bug which took me a while to figure out. The root cause is an unexpected use of `href` attribute.

> href: Contains a URL or a URL fragment that the hyperlink points to. URLs are not restricted to Web (HTTP)-based documents, but can use any protocol supported by the browser.

# Use case

I build a multi-level menus with the following requirements:

* Only the leaf menu item is an active link
* Clicking other non-leaf menu items should expand the next level

## What I did

It comes in handy for making each menu item with `<a>`. The only difference between leaf menu items and non-leaf menu items is that leaf menu items should have valid `href` which indicates the redirection destination. Therefore, I build a `MenuItem` component. E.g.

```javascript

// Note, the example is in React
function MenuItem({link, title, onClick = () => {}}) {
  return <a href={link} onClick={onClick}> {title} </a>
}
```

## The Problem

### Leaf menu items

Leaf menu items work fine as the source `{ link: '/aaa', title: 'abc'}` would be rendered as `<a href='/aaa'> abc </a>`


### Non-leaf menu items

For non-leaf menu items, the source `{ link: '', title: 'abc', onClick: console.log }` would be rendered as `<a href> abc </a>`. Clicking such an anchor link will reload the page. The reason is that the current page is assumed to be the value of `href` attribute if the `href` attribute exists but no value is set for it. i.e.

```html
<a href>aa</a>
```

is equivalent to 

```html
<a href={location.href}>aa</a>
```

## Solution

### Sol.1 - don't set href if link is exceptional

```javascript
function MenuItem({link, title, onClick = () => {}}) {
  if(link) {
    return <a href={link} onClick={onClick}> {title} </a>
  }
  return <a onClick={onClick}> {title} </a>
}
```

### Sol.2 - reset link if link is exceptional

```javascript
function MenuItem({link, title, onClick = () => {}}) {
  if (!link) {
    link = '#'
  }
  return <a href={link} onClick={onClick}> {title} </a>
}
```

# Conclusion

To conclude, an anchor element with `href` attribute present but no value set for it will make it an active link to the current page. Having this knowledge in mind may save you time when you get into a similar track :-)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.