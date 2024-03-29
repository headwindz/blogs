# `fixed` position

For a long time I have been believing that for an element with `position: fixed`, it will be positioned relative to the viewport. However, it's not always the case, as outlined in the docs: 

> It is positioned relative to the initial containing block established by the viewport, except when one of its ancestors has a transform, perspective, or filter property set to something other than none 

Simply put, if any of the closest ancestors has `transform`, `perspective` or `filter` property set, then the closest ancestors become the [containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block)

[Codesandbox](https://codesandbox.io/s/position-fiexed-r7idi)

## At what case can it be useful

When you use a third-party plugin/component with the usage of fixed position but you want it to be positioned relative to your specified container rather than the viewport. Given that you have no control over the third-party source code, wrapping the third-party plugin/component with a container (e.g. `div`) with `transform: translateX(0)` (or whatsoever that won't affect display) would simply make you happy.


## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.