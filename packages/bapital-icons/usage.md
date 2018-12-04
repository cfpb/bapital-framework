<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>bapital-icons</title>
  <link rel="stylesheet" href="bapital-icons.css">
</head>
<body>

The bapital-icon component provides SVG icons for Capital Framework.
This component can be used by itself, but is designed to work with Capital
Framework.

## Table of contents

- [**New!** SVG icons](#svg-icons)
  - [Variables](#variables)
    - [Size variables](#size-variables)
  - [SVG icon basics](#svg-icon-basics)
  - [Rotating icons and other special features](#rotating-icons-and-other-special-features)
  - [The icons](#the-icons)
    - [Navigation icons](#navigation-icons)
    - [Status icons](#status-icons)
    - [Social/sharing icons](#social-sharing-icons)
    - [Communications icons](#communications-icons)
    - [Document icons](#document-icons)
    - [Financial products, services, and concepts](#financial-products-services-and-concepts)
    - [Web application icons](#web-application-icons)
- [**To be deprecated!** Icon font](#icon-font)




## SVG icons

**New in February 2018.** bapital-icons now provides each icon as an individual SVG,
in accordance with current industry best practices for implementing icons.

The [original icon font implementation](#icon-font) will remain available until
the release of Capital Framework version 5.0. (Timeline TBD.)


### Variables

Theme variables for setting the color and sizes throughout the project.
Overwrite them in your own project by duplicating the variable `@key: value`.

#### Size variables

The standard icon height matches the 19px rendered canvas of text
set in Avenir Next sized at 16px ( 19/16 = 1.1875 ).

```
@bapital-icon-height: 1.1875em;
```


### SVG icon basics

We subscribe to the guidance offered by Chris Coyier in his article,
["A Pretty Good SVG Icon System"](https://css-tricks.com/pretty-good-svg-icon-system/),
in which he concludes, "Just include the icons inline."

As a simple example, you could create one of our icon links like so:

<a class="a-link a-link__icon" href="#">
    <span class="a-link_text">Download the info sheet</span>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 651.7 1200" class="bapital-icon-svg"><path d="M507.1 692.8c-15.6-15.6-40.9-15.6-56.6 0l-85.1 85.1V466.6c0-22.1-17.9-40-40-40s-40 17.9-40 40V778l-85.1-85.1c-15.6-15.6-40.9-15.6-56.6 0-15.6 15.6-15.6 40.9 0 56.6L297 902.9c7.5 7.5 17.7 11.7 28.3 11.7s20.8-4.2 28.3-11.7l153.3-153.4c15.8-15.7 15.8-41 .2-56.7z"/><path d="M30 161c-16.5 0-30 13.5-30 30v827.8c0 16.5 13.5 30 30 30h591.7c16.5 0 30-13.5 30-30V343.7L469 161H30zm389.6 60v134.8c0 19.9 16.3 36.2 36.2 36.2h135.9V988.8H60V221h359.6z"/></svg>
</a>

```
<a class="a-link a-link__icon" href="#">
    <span class="a-link_text">Download the info sheet</span>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 651.7 1200" class="bapital-icon-svg"><path d="M507.1 692.8c-15.6-15.6-40.9-15.6-56.6 0l-85.1 85.1V466.6c0-22.1-17.9-40-40-40s-40 17.9-40 40V778l-85.1-85.1c-15.6-15.6-40.9-15.6-56.6 0-15.6 15.6-15.6 40.9 0 56.6L297 902.9c7.5 7.5 17.7 11.7 28.3 11.7s20.8-4.2 28.3-11.7l153.3-153.4c15.8-15.7 15.8-41 .2-56.7z"/><path d="M30 161c-16.5 0-30 13.5-30 30v827.8c0 16.5 13.5 30 30 30h591.7c16.5 0 30-13.5 30-30V343.7L469 161H30zm389.6 60v134.8c0 19.9 16.3 36.2 36.2 36.2h135.9V988.8H60V221h359.6z"/></svg>
</a>
```

The SVG code in this example was copied from `bapital-icons/src/icons/download.svg`.

Because including raw SVG code is not necessarily pretty or user-friendly,
we encourage using your templating system to include them by reference.
For example, here is how we include them in the Liquid templates of this site:

<a class="a-link a-link__icon" href="#">
    <span class="a-link_text">Download the info sheet</span>
    {% include icons/download.svg %}
</a>

```
<a class="a-link a-link__icon" href="#">
    <span class="a-link_text">Download the info sheet</span>
    {% raw %}{% include icons/download.svg %}{% endraw %}
</a>
```

_Note: Jinja2, the templating language that cfgov-refresh uses,
has a near-identical syntax for includes, but it requires that the path
be enclosed in quotation marks, like so:
`{% raw %}{% include 'icons/download.svg' %}{% endraw %}`._

The filenames of the SVGs included with bapital-icons
match the names in [the tables below](#the-icons).
There are duplicate SVG files for each alias, as well.

_Note to contributors: If any icon is ever updated,
you must be sure to also update each of the alias SVGs._

{% comment %}
TODO: Provide better contributing docs for SVG icons,
including how to run the Node script for processing the source SVGs.
{% endcomment %}

#### What the Less is doing

If you look in `bapital-icons.less`, below the afore-mentioned sizing variable,
you'll see this simple rule:

```
.bapital-icon-svg {
    height: @bapital-icon-height;
    vertical-align: text-top;
    fill: currentColor;
}
```

Referring back to the example above, you can see that we have encoded
`class="bapital-icon-svg"` in the root element of each of our SVG icons.
As a result, the Less rule gets applied to all of the SVGs on the page,
just like any other HTML element.

We start by limiting the size of the SVG to a proportion of the text height,
using the `@bapital-icon-height` variable's em value.
To align the canvas of the icon with the canvas of neighboring text,
we set `vertical-align: text-top;`.
Finally, setting `fill: currentColor;` tells the SVG to set its path's fill
color to match the `color` value of its parent element.

#### Caveats

There are two modifications based on restrictions in Internet Explorer 8 and 9.

First, IE8 does not support `fill: currentColor`. Typically the fallback would
be to use a png image, but due to the inability to know what the background or
text color of it's surroundings are, we found it better to fall back to the
paired text with no icon.

Second, IE9 displays svgs as full width by default (not the paths, just the
SVG container). To eliminate this issue we've set the width of the SVGs to
match the height. The whitespace to the left or right may not be quite
accurate, but we determined this is an acceptable difference for a legacy
browser like IE9.


### Rotating icons and other special features

In our original icon font implementation, we included a
[`spin` modifier](#spinning-icons) that took an icon
and animated it rotating around its center point.
This has not yet been implemented in the SVG icon system,
but we will do so before deprecating the icon font.

There were other features in the icon font system, as well,
[which you can see below](#helpers), but other than rotation,
none of them were ever used.
We have no plans to implement them in the SVG system,
unless a future need requires it.


### The icons

Each icon has a circled variant shown in the second column
(or square, in the case of the social media icons)
that can be accessed by appending `-round` (or `-square`) to
the cancanonical name or any of its aliases.

#### Navigation icons

| icon | icon-round | canonical name | aliases |
| ---- | ---------- | -------------- | ------- |
| {% include icons/up.svg %} | {% include icons/up-round.svg %} | up | chevron-up |
| {% include icons/right.svg %} | {% include icons/right-round.svg %} | right | chevron-right |
| {% include icons/down.svg %} | {% include icons/down-round.svg %} | down | chevron-down |
| {% include icons/left.svg %} | {% include icons/left-round.svg %} | left | chevron-left |
| {% include icons/arrow-up.svg %} | {% include icons/arrow-up-round.svg %} | arrow-up |  |
| {% include icons/arrow-right.svg %} | {% include icons/arrow-right-round.svg %} | arrow-right |  |
| {% include icons/arrow-down.svg %} | {% include icons/arrow-down-round.svg %} | arrow-down |  |
| {% include icons/arrow-left.svg %} | {% include icons/arrow-left-round.svg %} | arrow-left |  |
{: class="icon-table"}

#### Status icons

| icon | icon-round | canonical name | aliases |
| ---- | ---------- | -------------- | ------- |
| {% include icons/approved.svg %} | {% include icons/approved-round.svg %} | approved | check, checkmark, success |
| {% include icons/error.svg %} | {% include icons/error-round.svg %} | error | delete, close, remove, multiply, multiplication, x |
| {% include icons/warning.svg %} | {% include icons/warning-round.svg %} | warning | alert, exclamation-mark |
| {% include icons/help.svg %} | {% include icons/help-round.svg %} | help | question, question-mark |
| {% include icons/update.svg %} | {% include icons/update-round.svg %} | update | updating _(used for animated state)_ |
| {% include icons/dollar.svg %} | {% include icons/dollar-round.svg %} | dollar |  |
| {% include icons/plus.svg %} | {% include icons/plus-round.svg %} | plus | add, addition, expand |
| {% include icons/minus.svg %} | {% include icons/minus-round.svg %} | minus | subtract, subtraction, collapse |
| {% include icons/divide.svg %} | {% include icons/divide-round.svg %} | divide | division |
| {% include icons/equal.svg %} | {% include icons/equal-round.svg %} | equal | equals |
| {% include icons/percentage.svg %} | {% include icons/percentage-round.svg %} | percentage | percent |
{: class="icon-table"}

#### Social/sharing icons

| icon | icon-square | canonical name | aliases |
| ---- | ---------- | -------------- | ------- |
| {% include icons/email.svg %} | {% include icons/email-square.svg %} | email | envelope, envelope-back |
| {% include icons/facebook.svg %} | {% include icons/facebook-square.svg %} | facebook |  |
| {% include icons/flickr.svg %} | {% include icons/flickr-square.svg %} | flickr |  |
| {% include icons/github.svg %} | {% include icons/github-square.svg %} | github |  |
| {% include icons/linkedin.svg %} | {% include icons/linkedin-square.svg %} | linkedin |  |
| {% include icons/twitter.svg %} | {% include icons/twitter-square.svg %} | twitter |  |
| {% include icons/youtube.svg %} | {% include icons/youtube-square.svg %} | youtube |  |
{: class="icon-table"}

#### Communications icons

| icon | icon-round | canonical name | aliases |
| ---- | ---------- | -------------- | ------- |
| {% include icons/email.svg %} | {% include icons/email-round.svg %} | email | envelope-back |
| {% include icons/fax.svg %} | {% include icons/fax-round.svg %} | fax | fax-machine |
| {% include icons/mail.svg %} | {% include icons/mail-round.svg %} | mail | envelope-front |
| {% include icons/phone.svg %} | {% include icons/phone-round.svg %} | phone | telephone, handset |
| {% include icons/technology.svg %} | {% include icons/technology-round.svg %} | technology | cellphone, tablet |
| {% include icons/web.svg %} | {% include icons/web-round.svg %} | web | globe, world |
{: class="icon-table"}

#### Document icons

| icon | icon-round | canonical name | aliases |
| ---- | ---------- | -------------- | ------- |
| {% include icons/appendix.svg %} | {% include icons/appendix-round.svg %} | appendix |  |
| {% include icons/paper-clip.svg %} | {% include icons/paper-clip-round.svg %} | paper-clip | attach, attachment |
| {% include icons/copy.svg %} | {% include icons/copy-round.svg %} | copy | duplicate |
| {% include icons/document.svg %} | {% include icons/document-round.svg %} | document | doc, pdf |
| {% include icons/download.svg %} | {% include icons/download-round.svg %} | download |  |
| {% include icons/upload.svg %} | {% include icons/upload-round.svg %} | upload |  |
| {% include icons/edit.svg %} | {% include icons/edit-round.svg %} | edit | pencil |
| {% include icons/print.svg %} | {% include icons/print-round.svg %} | print | printer |
| {% include icons/rss.svg %} | {% include icons/rss-round.svg %} | rss | feed |
| {% include icons/save.svg %} | {% include icons/save-round.svg %} | save | disk |
| {% include icons/supplement.svg %} | {% include icons/supplement-round.svg %} | supplement |  |
{: class="icon-table"}

#### Financial products, services, and concepts

| icon | icon-round | canonical name | aliases |
| ---- | ---------- | -------------- | ------- |
| {% include icons/bank.svg %} | {% include icons/bank-round.svg %} | bank | bank-account |
| {% include icons/building-credit.svg %} | {% include icons/building-credit-round.svg %} | building-credit | |
| {% include icons/car.svg %} | {% include icons/car-round.svg %} | car | car-loan, auto, auto-loan |
| {% include icons/complaint.svg %} | {% include icons/complaint-round.svg %} | complaint | |
| {% include icons/fountain-pen.svg %} | {% include icons/fountain-pen-round.svg %} | fountain-pen | contract |
| {% include icons/credit-card.svg %} | {% include icons/credit-card-round.svg %} | credit-card | |
| {% include icons/credit-report.svg %} | {% include icons/credit-report-round.svg %} | credit-report |  |
| {% include icons/debt-collection.svg %} | {% include icons/debt-collection-round.svg %} | debt-collection |  |
| {% include icons/debt.svg %} | {% include icons/debt-round.svg %} | debt |  |
| {% include icons/getting-a-credit-card.svg %} | {% include icons/getting-a-credit-card-round.svg %} | getting-a-credit-card | credit-card-contract |
| {% include icons/loan.svg %} | {% include icons/loan-round.svg %} | loan |  |
| {% include icons/money.svg %} | {% include icons/money-round.svg %} | money | dollar-bill |
| {% include icons/money-transfer.svg %} | {% include icons/money-transfer-round.svg %} | money-transfer |  |
| {% include icons/mortgage.svg %} | {% include icons/mortgage-round.svg %} | mortgage | sold |
| {% include icons/house.svg %} | {% include icons/house-round.svg %} | house | buying-a-house, owning-a-home, home |
| {% include icons/payday-loan.svg %} | {% include icons/payday-loan-round.svg %} | payday-loan |  |
| {% include icons/college.svg %} | {% include icons/college-round.svg %} | college | paying-for-college, grad-cap, mortarboard |
| {% include icons/prepaid-cards.svg %} | {% include icons/prepaid-cards-round.svg %} | prepaid-cards | prepaid |
| {% include icons/quick-cash.svg %} | {% include icons/quick-cash-round.svg %} | quick-cash |  |
| {% include icons/piggy-bank.svg %} | {% include icons/piggy-bank-round.svg %} | piggy-bank | retirement |
{: class="icon-table"}

#### Web application icons

| icon | icon-round | canonical name | aliases |
| ---- | ---------- | -------------- | ------- |
| {% include icons/audio-max.svg %} | {% include icons/audio-max-round.svg %} | audio-max | audio-high |
| {% include icons/audio-medium.svg %} | {% include icons/audio-medium-round.svg %} | audio-medium |  |
| {% include icons/audio-low.svg %} | {% include icons/audio-low-round.svg %} | audio-low |  |
| {% include icons/audio-mute.svg %} | {% include icons/audio-mute-round.svg %} | audio-mute | mute, audio-off |
| {% include icons/bookmark.svg %} | {% include icons/bookmark-round.svg %} | bookmark |  |
| {% include icons/unbookmark.svg %} | {% include icons/unbookmark-round.svg %} | unbookmark |  |
| {% include icons/broadcast.svg %} | {% include icons/broadcast-round.svg %} | broadcast | antenna, radio |
| {% include icons/bullhorn.svg %} | {% include icons/bullhorn-round.svg %} | bullhorn | megaphone |
| {% include icons/chart.svg %} | {% include icons/chart-round.svg %} | chart | graph |
| {% include icons/clock.svg %} | {% include icons/clock-round.svg %} | clock | time |
| {% include icons/date.svg %} | {% include icons/date-round.svg %} | date | calendar |
| {% include icons/dialogue.svg %} | {% include icons/dialogue-round.svg %} | dialogue | discussion |
| {% include icons/disabled.svg %} | {% include icons/disabled-round.svg %} | disabled | no, disallowed |
| {% include icons/external-link.svg %} | {% include icons/external-link-round.svg %} | external-link |  |
| {% include icons/favorite.svg %} | {% include icons/favorite-round.svg %} | favorite | star, starred, fav, fave |
| {% include icons/unfavorite.svg %} | {% include icons/unfavorite-round.svg %} | unfavorite | unstar, unstarred, unfav, unfave |
| {% include icons/information.svg %} | {% include icons/information-round.svg %} | information | info, i |
| {% include icons/lightbulb.svg %} | {% include icons/lightbulb-round.svg %} | lightbulb | idea |
| {% include icons/link.svg %} | {% include icons/link-round.svg %} | link |  |
| {% include icons/list.svg %} | {% include icons/list-round.svg %} | list |  |
| {% include icons/lock.svg %} | {% include icons/lock-round.svg %} | lock | locked |
| {% include icons/unlock.svg %} | {% include icons/unlock-round.svg %} | unlock | unlocked |
| {% include icons/menu.svg %} | {% include icons/menu-round.svg %} | menu | hamburger |
| {% include icons/microphone.svg %} | {% include icons/microphone-round.svg %} | microphone | mic |
| {% include icons/newspaper.svg %} | {% include icons/newspaper-round.svg %} | newspaper | news |
| {% include icons/parent.svg %} | {% include icons/parent-round.svg %} | parent | family |
| {% include icons/play.svg %} | {% include icons/play-round.svg %} | play |  |
| {% include icons/open-quote.svg %} | {% include icons/open-quote-round.svg %} | open-quote |  |
| {% include icons/close-quote.svg %} | {% include icons/close-quote-round.svg %} | close-quote |  |
| {% include icons/search.svg %} | {% include icons/search-round.svg %} | search | zoom, magnifying-glass |
| {% include icons/settings.svg %} | {% include icons/settings-round.svg %} | settings | preferences, gear, cog |
| {% include icons/share.svg %} | {% include icons/share-round.svg %} | share |  |
| {% include icons/speech-bubble.svg %} | {% include icons/speech-bubble-round.svg %} | speech-bubble |  |
| {% include icons/user.svg %} | {% include icons/user-round.svg %} | user | person |
| {% include icons/wifi.svg %} | {% include icons/wifi-round.svg %} | wifi | wi-fi, wireless |
{: class="icon-table"}




## Icon font

**To be deprecated in CFv5.** Everything from here down is the documentation
for the icon font implementation that will soon be deprecated.
It's here if you need it in the meantime, but please use the above
[SVG icon guidance](#svg-icons) for any new implementations.

---

The bapital-icon component provides the custom icon font for Capital Framework.
This component can be used by itself, but is designed to work with Capital
Framework.

> NOTE: If you use `bapital-icons.less` directly,
  be sure to run the file through
  [Autoprefixer](https://github.com/postcss/autoprefixer),
  or your compiled Capital Framework CSS will
  not work perfectly in older browsers.


### Table of contents

- [Variables](#variables-1)
    - [Color variables](#color-variables)
    - [Icon generation variables](#icon-generation-variables)
- [The basics](#the-basics)
- [Helpers](#helpers)
    - [Icon sizes](#icon-sizes)
    - [Mixins](#mixins)
    - [Modified icons](#modified-icons)
    - [Animated icons](#animated-icons)
- [Icons](#icons)
    - [Navigation icons](#navigation-icons-1)
    - [Status icons](#status-icons-1)
    - [Social icons](#social-icons)
    - [Document icons](#document-icons-1)
    - [Financial product icons](#financial-product-icons)
    - [Web icons](#web-icons)
- [Icon character variables](#icon-character-variables)


### Variables

Theme variables for setting the color and sizes throughout the project.
Overwrite them in your own project by duplicating the variable `@key: value`.

#### Color variables

Color variables referenced in comments are from [bapital-core bapital-brand-colors.less](https://github.com/cfpb/capital-framework/blob/master/src/bapital-core/src/bapital-brand-colors.less).


```
@bapital-icon-border-color:          @gray;
```

### Icon generation variables

```
@bapital-icon-prefix:                bapital-icon;
@bapital-icon-path:                  'fonts';
```

### The basics

The class defined by the @bapital-icon-prefix variable applies all shared icon
styles including the font family. By default, this class will be named
`bapital-icon` but it can be changed in the settings. All icons must use three
classes, one for the base class, one to select the desired icon, and one
for the chosen pseudo-element. For example:

```
<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before"></span>
```

It's preferred to combine the icon classes with an existing element, but if
it's necessary to use an empty element, please use the `span` element instead
of the `i` element. This avoids font family cascading conflicts when using an
italic webfont on `i` elements and then another font for the icons.
_Note that this issue only pops up in older versions of Internet Explorer._


### Helpers

#### Icon sizes


##### Large icon size

<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__lg"></span>

```
<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__lg"></span>
```

##### 2x icon size

<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__2x"></span>

```
<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__2x"></span>
```

##### 3x icon size

<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__3x"></span>

```
<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__3x"></span>
```

##### 4x icon size

<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__4x"></span>

```
<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__4x"></span>
```

##### 5x icon size

<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__5x"></span>

```
<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before
             bapital-icon__5x"></span>
```

#### Mixins

MIT Licensed by Font Awesome

##### Icon rotation mixin

```
.@{bapital-icon-prefix}__rotate-90  { .bapital-icon__rotate(90deg, 1);  }
```

- First parameter is `@degrees`.
- Second parameter is `@rotation`.

##### Icon flip mixin

```
.@{bapital-icon-prefix}__flip-horizontal { .bapital-icon__flip(-1, 1, 0); }
.@{bapital-icon-prefix}__flip-vertical   { .bapital-icon__flip(1, -1, 2); }
```

- First parameter is for number of horizontal flips.
- Second parameter is for number of vertical flips.
- Third parameter is for rotation.

#### Modified icons

MIT Licensed by Font Awesome

##### Bordered icons

<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__border"></span>
```
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__border"></span>
```

Border color set by `@bapital-icon-border-color`

##### Rotated icons

<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__rotate-90"></span>
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__rotate-180"></span>
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__rotate-270"></span>
```
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__rotate-90"></span>
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__rotate-180"></span>
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__rotate-270"></span>
```

##### Flipped icons

<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__flip-horizontal"></span>
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__flip-vertical"></span>

```
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__flip-horizontal"></span>
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__flip-vertical"></span>
```

#### Animated icons

MIT Licensed by Font Awesome

##### Spinning icons

<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__spin"></span>


```
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__spin"></span>

```

##### Pulsing icons

<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__pulse"></span>

```
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before
             bapital-icon__pulse"></span>
```


### Icons

#### Navigation icons

##### Left

<span class="bapital-icon
             bapital-icon-left
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-left-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-left
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-left-round
             bapital-icon__before"></span>
```

##### Right

<span class="bapital-icon
             bapital-icon-right
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-right-round
             bapital-icon__before"></span>


```
<span class="bapital-icon
             bapital-icon-right
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-right-round
             bapital-icon__before"></span>
```

##### Up

<span class="bapital-icon
             bapital-icon-up
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-up-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-up
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-up-round
             bapital-icon__before"></span>
```

##### Down

<span class="bapital-icon
             bapital-icon-down
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-down-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-down
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-down-round
             bapital-icon__before"></span>
```

##### Arrow left

<span class="bapital-icon
             bapital-icon-arrow-left
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-left-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-arrow-left
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-left-round
             bapital-icon__before"></span>
```

##### Arrow right

<span class="bapital-icon
             bapital-icon-arrow-right
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-right-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-arrow-right
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-right-round
             bapital-icon__before"></span>
```

##### Arrow up

<span class="bapital-icon
             bapital-icon-arrow-up
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-up-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-arrow-up
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-up-round
             bapital-icon__before"></span>
```

##### Arrow down

<span class="bapital-icon
             bapital-icon-arrow-down
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-down-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-arrow-down
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-arrow-down-round
             bapital-icon__before"></span>
```

#### Status icons

##### Approved

<span class="bapital-icon
             bapital-icon-approved
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-approved-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-approved
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-approved-round
             bapital-icon__before"></span>
```

##### Error

<span class="bapital-icon
             bapital-icon-error
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-error-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-error
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-error-round
             bapital-icon__before"></span>
```

##### Help

<span class="bapital-icon
             bapital-icon-help
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-help-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-help
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-help-round
             bapital-icon__before"></span>
```

##### Delete

<span class="bapital-icon
             bapital-icon-delete
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-delete-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-delete
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-delete-round
             bapital-icon__before"></span>
```

##### Plus

<span class="bapital-icon
             bapital-icon-plus
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-plus-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-plus
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-plus-round
             bapital-icon__before"></span>
```

##### Minus

<span class="bapital-icon
             bapital-icon-minus
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-minus-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-minus
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-minus-round
             bapital-icon__before"></span>
```

##### Update

<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-update-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-update
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-update-round
             bapital-icon__before"></span>
```

#### Social icons

##### YouTube

<span class="bapital-icon
             bapital-icon-youtube
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-youtube-square
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-youtube
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-youtube-square
             bapital-icon__before"></span>
```

##### Linkedin

<span class="bapital-icon
             bapital-icon-linkedin
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-linkedin-square
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-linkedin
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-linkedin-square
             bapital-icon__before"></span>
```

##### Facebook

<span class="bapital-icon
             bapital-icon-facebook
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-facebook-square
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-facebook
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-facebook-square
             bapital-icon__before"></span>
```

##### Flickr

<span class="bapital-icon
             bapital-icon-flickr
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-flickr-square
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-flickr
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-flickr-square
             bapital-icon__before"></span>
```

##### Twitter

<span class="bapital-icon
             bapital-icon-twitter
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-twitter-square
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-twitter
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-twitter-square
             bapital-icon__before"></span>
```

##### GitHub

<span class="bapital-icon
             bapital-icon-github
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-github-square
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-github
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-github-square
             bapital-icon__before"></span>
```

##### Email

<span class="bapital-icon
             bapital-icon-email-social
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-email-social-square
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-email-social
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-email-social-square
             bapital-icon__before"></span>
```

#### Document icons

##### Document

<span class="bapital-icon
             bapital-icon-document
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-document-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-document
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-document-round
             bapital-icon__before"></span>
```

##### PDF

<span class="bapital-icon
             bapital-icon-pdf
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-pdf-round
             bapital-icon__before"></span>
```
<span class="bapital-icon
             bapital-icon-pdf
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-pdf-round
             bapital-icon__before"></span>
```

##### Upload

<span class="bapital-icon
             bapital-icon-upload
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-upload-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-upload
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-upload-round
             bapital-icon__before"></span>
```

##### Download

<span class="bapital-icon
             bapital-icon-download
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-download-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-download
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-download-round
             bapital-icon__before"></span>
```

##### Copy

<span class="bapital-icon
             bapital-icon-copy
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-copy-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-copy
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-copy-round
             bapital-icon__before"></span>
```

##### Edit

<span class="bapital-icon
             bapital-icon-edit
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-edit-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-edit
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-edit-round
             bapital-icon__before"></span>
```

##### Attach

<span class="bapital-icon
             bapital-icon-attach
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-attach-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-attach
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-attach-round
             bapital-icon__before"></span>
```

##### Print

<span class="bapital-icon
             bapital-icon-print
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-print-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-print
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-print-round
             bapital-icon__before"></span>
```

##### Save

<span class="bapital-icon
             bapital-icon-save
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-save-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-save
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-save-round
             bapital-icon__before"></span>
```

##### Appendix

<span class="bapital-icon
             bapital-icon-appendix
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-appendix-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-appendix
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-appendix-round
             bapital-icon__before"></span>
```

##### Supplement

<span class="bapital-icon
             bapital-icon-supplement
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-supplement-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-supplement
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-supplement-round
             bapital-icon__before"></span>
```

##### RSS

<span class="bapital-icon
             bapital-icon-rss
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-rss-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-rss
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-rss-round
             bapital-icon__before"></span>
```

#### Financial product icons

##### Bank account

<span class="bapital-icon
             bapital-icon-bank-account
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-bank-account-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-bank-account
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-bank-account-round
             bapital-icon__before"></span>
```

##### Credit card

<span class="bapital-icon
             bapital-icon-credit-card
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-credit-card-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-credit-card
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-credit-card-round
             bapital-icon__before"></span>
```

##### Loan

<span class="bapital-icon
             bapital-icon-loan
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-loan-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-loan
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-loan-round
             bapital-icon__before"></span>
```

##### Money transfer

<span class="bapital-icon
             bapital-icon-money-transfer
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-money-transfer-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-money-transfer
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-money-transfer-round
             bapital-icon__before"></span>
```

##### Mortgage

<span class="bapital-icon
             bapital-icon-mortgage
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-mortgage-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-mortgage
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-mortgage-round
             bapital-icon__before"></span>
```

##### Debt collection

<span class="bapital-icon
             bapital-icon-debt-collection
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-debt-collection-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-debt-collection
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-debt-collection-round
             bapital-icon__before"></span>
```

##### Credit report

<span class="bapital-icon
             bapital-icon-credit-report
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-credit-report-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-credit-report
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-credit-report-round
             bapital-icon__before"></span>
```

##### Money

<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-money-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-money
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-money-round
             bapital-icon__before"></span>
```

##### Quick cash

<span class="bapital-icon
             bapital-icon-quick-cash
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-quick-cash-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-quick-cash
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-quick-cash-round
             bapital-icon__before"></span>
```

##### Contract

<span class="bapital-icon
             bapital-icon-contract
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-contract-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-contract
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-contract-round
             bapital-icon__before"></span>
```

##### Complaint

<span class="bapital-icon
             bapital-icon-complaint
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-complaint-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-complaint
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-complaint-round
             bapital-icon__before"></span>
```

##### Getting a credit card

<span class="bapital-icon
             bapital-icon-getting-credit-card
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-getting-credit-card-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-getting-credit-card
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-getting-credit-card-round
             bapital-icon__before"></span>
```

##### Buying a car

<span class="bapital-icon
             bapital-icon-buying-car
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-buying-car-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-buying-car
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-buying-car-round
             bapital-icon__before"></span>
```

##### Paying for college

<span class="bapital-icon
             bapital-icon-paying-college
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-paying-college-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-paying-college
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-paying-college-round
             bapital-icon__before"></span>
```

##### Owning a home

<span class="bapital-icon
             bapital-icon-owning-home
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-owning-home-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-owning-home
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-owning-home-round
             bapital-icon__before"></span>
```

##### Debt

<span class="bapital-icon
             bapital-icon-debt
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-debt-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-debt
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-debt-round
             bapital-icon__before"></span>
```

##### Building credit

<span class="bapital-icon
             bapital-icon-building-credit
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-building-credit-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-building-credit
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-building-credit-round
             bapital-icon__before"></span>
```

##### Prepaid cards

<span class="bapital-icon
             bapital-icon-prepaid-cards
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-prepaid-cards-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-prepaid-cards
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-prepaid-cards-round
             bapital-icon__before"></span>
```

##### Payday loan

<span class="bapital-icon
             bapital-icon-payday-loan
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-payday-loan-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-payday-loan
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-payday-loan-round
             bapital-icon__before"></span>
```

##### Retirement

<span class="bapital-icon
             bapital-icon-retirement
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-retirement-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-retirement
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-retirement-round
             bapital-icon__before"></span>
```


#### Web icons

##### User

<span class="bapital-icon
             bapital-icon-user
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-user-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-user
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-user-round
             bapital-icon__before"></span>
```

##### WiFi

<span class="bapital-icon
             bapital-icon-wifi
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-wifi-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-wifi
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-wifi-round
             bapital-icon__before"></span>
```

##### Search

<span class="bapital-icon
             bapital-icon-search
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-search-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-search
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-search-round
             bapital-icon__before"></span>
```

##### Share

<span class="bapital-icon
             bapital-icon-share
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-share-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-share
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-share-round
             bapital-icon__before"></span>
```

##### Link

<span class="bapital-icon
             bapital-icon-link
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-link-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-link
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-link-round
             bapital-icon__before"></span>
```

##### External link

<span class="bapital-icon
             bapital-icon-external-link
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-external-link-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-external-link
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-external-link-round
             bapital-icon__before"></span>
```

##### Audio mute

<span class="bapital-icon
             bapital-icon-audio-mute
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-mute-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-audio-mute
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-mute-round
             bapital-icon__before"></span>
```

##### Audio low

<span class="bapital-icon
             bapital-icon-audio-low
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-low-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-audio-low
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-low-round
             bapital-icon__before"></span>
```

##### Audio medium

<span class="bapital-icon
             bapital-icon-audio-medium
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-medium-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-audio-medium
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-medium-round
             bapital-icon__before"></span>
```

##### Audio max

<span class="bapital-icon
             bapital-icon-audio-max
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-max-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-audio-max
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-audio-max-round
             bapital-icon__before"></span>
```

##### Favorite

<span class="bapital-icon
             bapital-icon-favorite
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-favorite-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-favorite
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-favorite-round
             bapital-icon__before"></span>
```

##### Unfavorite

<span class="bapital-icon
             bapital-icon-unfavorite
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-unfavorite-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-unfavorite
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-unfavorite-round
             bapital-icon__before"></span>
```

##### Bookmark

<span class="bapital-icon
             bapital-icon-bookmark
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-bookmark-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-bookmark
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-bookmark-round
             bapital-icon__before"></span>
```

##### Unbookmark

<span class="bapital-icon
             bapital-icon-unbookmark
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-unbookmark-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-unbookmark
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-unbookmark-round
             bapital-icon__before"></span>
```

##### Settings

<span class="bapital-icon
             bapital-icon-settings
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-settings-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-settings
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-settings-round
             bapital-icon__before"></span>
```

##### Menu

<span class="bapital-icon
             bapital-icon-menu
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-menu-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-menu
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-menu-round
             bapital-icon__before"></span>
```

##### Lock

<span class="bapital-icon
             bapital-icon-lock
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-lock-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-lock
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-lock-round
             bapital-icon__before"></span>
```

##### Unlock

<span class="bapital-icon
             bapital-icon-unlock
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-unlock-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-unlock
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-unlock-round
             bapital-icon__before"></span>
```

##### Clock

<span class="bapital-icon
             bapital-icon-clock
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-clock-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-clock
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-clock-round
             bapital-icon__before"></span>
```

##### Chart

<span class="bapital-icon
             bapital-icon-chart
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-chart-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-chart
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-chart-round
             bapital-icon__before"></span>
```

##### Play

<span class="bapital-icon
             bapital-icon-play
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-play-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-play
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-play-round
             bapital-icon__before"></span>
```

##### History

<span class="bapital-icon
             bapital-icon-history
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-history-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-history
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-history-round
             bapital-icon__before"></span>
```

##### Table of contents

<span class="bapital-icon
             bapital-icon-table-of-contents
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-table-of-contents-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-table-of-contents
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-table-of-contents-round
             bapital-icon__before"></span>
```

##### Newspaper

<span class="bapital-icon
             bapital-icon-newspaper
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-newspaper-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-newspaper
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-newspaper-round
             bapital-icon__before"></span>
```

##### Microphone

<span class="bapital-icon
             bapital-icon-microphone
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-microphone-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-microphone
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-microphone-round
             bapital-icon__before"></span>
```

##### Bullhorn

<span class="bapital-icon
             bapital-icon-bullhorn
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-bullhorn-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-bullhorn
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-bullhorn-round
             bapital-icon__before"></span>
```

##### Double quote

<span class="bapital-icon
             bapital-icon-double-quote
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-double-quote-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-double-quote
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-double-quote-round
             bapital-icon__before"></span>
```

##### Speech bubble

<span class="bapital-icon
             bapital-icon-speech-bubble
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-speech-bubble-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-speech-bubble
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-speech-bubble-round
             bapital-icon__before"></span>
```

##### Information

<span class="bapital-icon
             bapital-icon-information
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-information-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-information
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-information-round
             bapital-icon__before"></span>
```

##### Lightbulb

<span class="bapital-icon
             bapital-icon-lightbulb
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-lightbulb-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-lightbulb
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-lightbulb-round
             bapital-icon__before"></span>
```

##### Dialogue

<span class="bapital-icon
             bapital-icon-dialogue
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-dialogue-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-dialogue
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-dialogue-round
             bapital-icon__before"></span>
```

##### Date

<span class="bapital-icon
             bapital-icon-date
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-date-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-date
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-date-round
             bapital-icon__before"></span>
```

##### Closing quote

<span class="bapital-icon
             bapital-icon-closing-quote
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-closing-quote-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-closing-quote
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-closing-quote-round
             bapital-icon__before"></span>
```

##### Livestream

<span class="bapital-icon
             bapital-icon-livestream
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-livestream-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-livestream
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-livestream-round
             bapital-icon__before"></span>
```

##### Parents

<span class="bapital-icon
             bapital-icon-parents
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-parents-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-parents
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-parents-round
             bapital-icon__before"></span>
```

##### Servicemembers

<span class="bapital-icon
             bapital-icon-servicemembers
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-servicemembers-round
             bapital-icon__before"></span>

```
<span class="bapital-icon
             bapital-icon-servicemembers
             bapital-icon__before"></span>
<span class="bapital-icon
             bapital-icon-servicemembers-round
             bapital-icon__before"></span>
```


### Icon character variables

Inspired by Font Awesome, we've added Less variables for adding icons in
Less files.

```less
.download-icon:after {
     .bapital-icon();
     display: inline-block;
     content: @bapital-icon-download;
     width: 1em;
     text-align: right;
}
```

</body>
</html>
