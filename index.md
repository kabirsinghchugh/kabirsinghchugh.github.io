---
layout: default
---

[Kant Essay](docs/critique-practical-reason)

# Tutorial

1. Standard markdown links(cmd K in iaWriter) `[Name](folder/doc)`

### Preview Page Types

[Listing Pages](samples/listings-page)

Text can be **bold**, _italic_, or ~~strikethrough~~.
Text can be `**bold**`, `_italic_`, or `~~strikethrough~~`.


# [](#header-1)Header 1

Header link as `[](#header-1)`


## [](#header-2)Header 2

> This is a blockquote following a header.


### [](#header-3)Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### [](#header-4)Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### [](#header-5)Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### [](#header-6)Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Image

`![](https://assets-cdn.github.com/images/icons/emoji/octocat.png)`

![](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

### Code block no language

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```



#### Configuration variables

Dracula will respect the following variables, if set in your site's `_config.yml`:

```yml
title: [The title of your site]
description: [A short description of your site's purpose]
icon: [path to file including extension]

```

Additionally, you may choose to set the following optional variables:


#### Stylesheet

If you'd like to add your own custom styles:

1. Create a file called `/assets/css/style.scss` in your site
2. Add the following content to the top of the file, exactly as shown:
    ```scss
    ---
    ---

    @import "{{ site.theme }}";
    ```
3. Add any custom CSS (or Sass, including imports) you'd like immediately after the `@import` line

*Note: If you'd like to change the theme's Sass variables, you must set new values before the `@import` line in your stylesheet.*

#### Layouts

If you'd like to change the theme's HTML layout:

1. [Copy the original template](https://github.com/dracula/gh-pages/blob/master/_layouts/default.html) from the theme's repository<br />(*Pro-tip: click "raw" to make copying easier*)
2. Create a file called `/_layouts/default.html` in your site
3. Paste the default layout content copied in the first step
4. Customize the layout as you'd like


Specify the URL that you'd like the template to use in your site's `_config.yml`. For example, if the variable was `site.github.url`, you'd add the following:

    ```yml
    github:
      zip_url: http://example.com/download.zip
      another_url: another value
    ```

> *Note: You must remove the `site.` prefix, and each variable name (after the `github.`) should be indent with two space below `github:`.*
