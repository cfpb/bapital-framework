The bf-icon component provides SVG icons for Capital Framework.
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

**New in February 2018.** bf-icons now provides each icon as an individual SVG,
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
@bf-icon-height: 1.1875em;
```


### SVG icon basics

We subscribe to the guidance offered by Chris Coyier in his article,
["A Pretty Good SVG Icon System"](https://css-tricks.com/pretty-good-svg-icon-system/),
in which he concludes, "Just include the icons inline."

As a simple example, you could create one of our icon links like so:

<a class="a-link a-link__icon" href="#">
    <span class="a-link_text">Download the info sheet</span>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 651.7 1200" class="bf-icon-svg"><path d="M507.1 692.8c-15.6-15.6-40.9-15.6-56.6 0l-85.1 85.1V466.6c0-22.1-17.9-40-40-40s-40 17.9-40 40V778l-85.1-85.1c-15.6-15.6-40.9-15.6-56.6 0-15.6 15.6-15.6 40.9 0 56.6L297 902.9c7.5 7.5 17.7 11.7 28.3 11.7s20.8-4.2 28.3-11.7l153.3-153.4c15.8-15.7 15.8-41 .2-56.7z"/><path d="M30 161c-16.5 0-30 13.5-30 30v827.8c0 16.5 13.5 30 30 30h591.7c16.5 0 30-13.5 30-30V343.7L469 161H30zm389.6 60v134.8c0 19.9 16.3 36.2 36.2 36.2h135.9V988.8H60V221h359.6z"/></svg>
</a>

```
<a class="a-link a-link__icon" href="#">
    <span class="a-link_text">Download the info sheet</span>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 651.7 1200" class="bf-icon-svg"><path d="M507.1 692.8c-15.6-15.6-40.9-15.6-56.6 0l-85.1 85.1V466.6c0-22.1-17.9-40-40-40s-40 17.9-40 40V778l-85.1-85.1c-15.6-15.6-40.9-15.6-56.6 0-15.6 15.6-15.6 40.9 0 56.6L297 902.9c7.5 7.5 17.7 11.7 28.3 11.7s20.8-4.2 28.3-11.7l153.3-153.4c15.8-15.7 15.8-41 .2-56.7z"/><path d="M30 161c-16.5 0-30 13.5-30 30v827.8c0 16.5 13.5 30 30 30h591.7c16.5 0 30-13.5 30-30V343.7L469 161H30zm389.6 60v134.8c0 19.9 16.3 36.2 36.2 36.2h135.9V988.8H60V221h359.6z"/></svg>
</a>
```

The SVG code in this example was copied from `bf-icons/src/icons/download.svg`.

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

The filenames of the SVGs included with bf-icons
match the names in [the tables below](#the-icons).
There are duplicate SVG files for each alias, as well.

_Note to contributors: If any icon is ever updated,
you must be sure to also update each of the alias SVGs._

{% comment %}
TODO: Provide better contributing docs for SVG icons,
including how to run the Node script for processing the source SVGs.
{% endcomment %}

#### What the Less is doing

If you look in `bf-icons.less`, below the afore-mentioned sizing variable,
you'll see this simple rule:

```
.bf-icon-svg {
    height: @bf-icon-height;
    vertical-align: text-top;
    fill: currentColor;
}
```

Referring back to the example above, you can see that we have encoded
`class="bf-icon-svg"` in the root element of each of our SVG icons.
As a result, the Less rule gets applied to all of the SVGs on the page,
just like any other HTML element.

We start by limiting the size of the SVG to a proportion of the text height,
using the `@bf-icon-height` variable's em value.
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

The bf-icon component provides the custom icon font for Capital Framework.
This component can be used by itself, but is designed to work with Capital
Framework.

> NOTE: If you use `bf-icons.less` directly,
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

Color variables referenced in comments are from [bf-core bf-brand-colors.less](https://github.com/cfpb/capital-framework/blob/master/src/bf-core/src/bf-brand-colors.less).


```
@bf-icon-border-color:          @gray;
```

### Icon generation variables

```
@bf-icon-prefix:                bf-icon;
@bf-icon-path:                  'fonts';
```

### The basics

The class defined by the @bf-icon-prefix variable applies all shared icon
styles including the font family. By default, this class will be named
`bf-icon` but it can be changed in the settings. All icons must use three
classes, one for the base class, one to select the desired icon, and one
for the chosen pseudo-element. For example:

```
<span class="bf-icon
             bf-icon-money
             bf-icon__before"></span>
```

It's preferred to combine the icon classes with an existing element, but if
it's necessary to use an empty element, please use the `span` element instead
of the `i` element. This avoids font family cascading conflicts when using an
italic webfont on `i` elements and then another font for the icons.
_Note that this issue only pops up in older versions of Internet Explorer._


### Helpers

#### Icon sizes


##### Large icon size

<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__lg"></span>

```
<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__lg"></span>
```

##### 2x icon size

<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__2x"></span>

```
<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__2x"></span>
```

##### 3x icon size

<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__3x"></span>

```
<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__3x"></span>
```

##### 4x icon size

<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__4x"></span>

```
<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__4x"></span>
```

##### 5x icon size

<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__5x"></span>

```
<span class="bf-icon
             bf-icon-money
             bf-icon__before
             bf-icon__5x"></span>
```

#### Mixins

MIT Licensed by Font Awesome

##### Icon rotation mixin

```
.@{bf-icon-prefix}__rotate-90  { .bf-icon__rotate(90deg, 1);  }
```

- First parameter is `@degrees`.
- Second parameter is `@rotation`.

##### Icon flip mixin

```
.@{bf-icon-prefix}__flip-horizontal { .bf-icon__flip(-1, 1, 0); }
.@{bf-icon-prefix}__flip-vertical   { .bf-icon__flip(1, -1, 2); }
```

- First parameter is for number of horizontal flips.
- Second parameter is for number of vertical flips.
- Third parameter is for rotation.

#### Modified icons

MIT Licensed by Font Awesome

##### Bordered icons

<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__border"></span>
```
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__border"></span>
```

Border color set by `@bf-icon-border-color`

##### Rotated icons

<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__rotate-90"></span>
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__rotate-180"></span>
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__rotate-270"></span>
```
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__rotate-90"></span>
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__rotate-180"></span>
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__rotate-270"></span>
```

##### Flipped icons

<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__flip-horizontal"></span>
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__flip-vertical"></span>

```
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__flip-horizontal"></span>
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__flip-vertical"></span>
```

#### Animated icons

MIT Licensed by Font Awesome

##### Spinning icons

<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__spin"></span>


```
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__spin"></span>

```

##### Pulsing icons

<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__pulse"></span>

```
<span class="bf-icon
             bf-icon-update
             bf-icon__before
             bf-icon__pulse"></span>
```


### Icons

#### Navigation icons

##### Left

<span class="bf-icon
             bf-icon-left
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-left-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-left
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-left-round
             bf-icon__before"></span>
```

##### Right

<span class="bf-icon
             bf-icon-right
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-right-round
             bf-icon__before"></span>


```
<span class="bf-icon
             bf-icon-right
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-right-round
             bf-icon__before"></span>
```

##### Up

<span class="bf-icon
             bf-icon-up
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-up-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-up
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-up-round
             bf-icon__before"></span>
```

##### Down

<span class="bf-icon
             bf-icon-down
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-down-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-down
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-down-round
             bf-icon__before"></span>
```

##### Arrow left

<span class="bf-icon
             bf-icon-arrow-left
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-left-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-arrow-left
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-left-round
             bf-icon__before"></span>
```

##### Arrow right

<span class="bf-icon
             bf-icon-arrow-right
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-right-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-arrow-right
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-right-round
             bf-icon__before"></span>
```

##### Arrow up

<span class="bf-icon
             bf-icon-arrow-up
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-up-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-arrow-up
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-up-round
             bf-icon__before"></span>
```

##### Arrow down

<span class="bf-icon
             bf-icon-arrow-down
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-down-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-arrow-down
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-arrow-down-round
             bf-icon__before"></span>
```

#### Status icons

##### Approved

<span class="bf-icon
             bf-icon-approved
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-approved-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-approved
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-approved-round
             bf-icon__before"></span>
```

##### Error

<span class="bf-icon
             bf-icon-error
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-error-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-error
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-error-round
             bf-icon__before"></span>
```

##### Help

<span class="bf-icon
             bf-icon-help
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-help-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-help
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-help-round
             bf-icon__before"></span>
```

##### Delete

<span class="bf-icon
             bf-icon-delete
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-delete-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-delete
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-delete-round
             bf-icon__before"></span>
```

##### Plus

<span class="bf-icon
             bf-icon-plus
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-plus-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-plus
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-plus-round
             bf-icon__before"></span>
```

##### Minus

<span class="bf-icon
             bf-icon-minus
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-minus-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-minus
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-minus-round
             bf-icon__before"></span>
```

##### Update

<span class="bf-icon
             bf-icon-update
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-update-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-update
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-update-round
             bf-icon__before"></span>
```

#### Social icons

##### YouTube

<span class="bf-icon
             bf-icon-youtube
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-youtube-square
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-youtube
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-youtube-square
             bf-icon__before"></span>
```

##### Linkedin

<span class="bf-icon
             bf-icon-linkedin
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-linkedin-square
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-linkedin
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-linkedin-square
             bf-icon__before"></span>
```

##### Facebook

<span class="bf-icon
             bf-icon-facebook
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-facebook-square
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-facebook
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-facebook-square
             bf-icon__before"></span>
```

##### Flickr

<span class="bf-icon
             bf-icon-flickr
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-flickr-square
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-flickr
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-flickr-square
             bf-icon__before"></span>
```

##### Twitter

<span class="bf-icon
             bf-icon-twitter
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-twitter-square
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-twitter
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-twitter-square
             bf-icon__before"></span>
```

##### GitHub

<span class="bf-icon
             bf-icon-github
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-github-square
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-github
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-github-square
             bf-icon__before"></span>
```

##### Email

<span class="bf-icon
             bf-icon-email-social
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-email-social-square
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-email-social
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-email-social-square
             bf-icon__before"></span>
```

#### Document icons

##### Document

<span class="bf-icon
             bf-icon-document
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-document-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-document
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-document-round
             bf-icon__before"></span>
```

##### PDF

<span class="bf-icon
             bf-icon-pdf
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-pdf-round
             bf-icon__before"></span>
```
<span class="bf-icon
             bf-icon-pdf
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-pdf-round
             bf-icon__before"></span>
```

##### Upload

<span class="bf-icon
             bf-icon-upload
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-upload-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-upload
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-upload-round
             bf-icon__before"></span>
```

##### Download

<span class="bf-icon
             bf-icon-download
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-download-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-download
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-download-round
             bf-icon__before"></span>
```

##### Copy

<span class="bf-icon
             bf-icon-copy
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-copy-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-copy
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-copy-round
             bf-icon__before"></span>
```

##### Edit

<span class="bf-icon
             bf-icon-edit
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-edit-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-edit
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-edit-round
             bf-icon__before"></span>
```

##### Attach

<span class="bf-icon
             bf-icon-attach
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-attach-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-attach
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-attach-round
             bf-icon__before"></span>
```

##### Print

<span class="bf-icon
             bf-icon-print
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-print-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-print
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-print-round
             bf-icon__before"></span>
```

##### Save

<span class="bf-icon
             bf-icon-save
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-save-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-save
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-save-round
             bf-icon__before"></span>
```

##### Appendix

<span class="bf-icon
             bf-icon-appendix
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-appendix-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-appendix
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-appendix-round
             bf-icon__before"></span>
```

##### Supplement

<span class="bf-icon
             bf-icon-supplement
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-supplement-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-supplement
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-supplement-round
             bf-icon__before"></span>
```

##### RSS

<span class="bf-icon
             bf-icon-rss
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-rss-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-rss
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-rss-round
             bf-icon__before"></span>
```

#### Financial product icons

##### Bank account

<span class="bf-icon
             bf-icon-bank-account
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-bank-account-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-bank-account
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-bank-account-round
             bf-icon__before"></span>
```

##### Credit card

<span class="bf-icon
             bf-icon-credit-card
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-credit-card-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-credit-card
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-credit-card-round
             bf-icon__before"></span>
```

##### Loan

<span class="bf-icon
             bf-icon-loan
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-loan-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-loan
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-loan-round
             bf-icon__before"></span>
```

##### Money transfer

<span class="bf-icon
             bf-icon-money-transfer
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-money-transfer-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-money-transfer
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-money-transfer-round
             bf-icon__before"></span>
```

##### Mortgage

<span class="bf-icon
             bf-icon-mortgage
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-mortgage-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-mortgage
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-mortgage-round
             bf-icon__before"></span>
```

##### Debt collection

<span class="bf-icon
             bf-icon-debt-collection
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-debt-collection-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-debt-collection
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-debt-collection-round
             bf-icon__before"></span>
```

##### Credit report

<span class="bf-icon
             bf-icon-credit-report
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-credit-report-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-credit-report
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-credit-report-round
             bf-icon__before"></span>
```

##### Money

<span class="bf-icon
             bf-icon-money
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-money-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-money
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-money-round
             bf-icon__before"></span>
```

##### Quick cash

<span class="bf-icon
             bf-icon-quick-cash
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-quick-cash-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-quick-cash
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-quick-cash-round
             bf-icon__before"></span>
```

##### Contract

<span class="bf-icon
             bf-icon-contract
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-contract-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-contract
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-contract-round
             bf-icon__before"></span>
```

##### Complaint

<span class="bf-icon
             bf-icon-complaint
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-complaint-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-complaint
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-complaint-round
             bf-icon__before"></span>
```

##### Getting a credit card

<span class="bf-icon
             bf-icon-getting-credit-card
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-getting-credit-card-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-getting-credit-card
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-getting-credit-card-round
             bf-icon__before"></span>
```

##### Buying a car

<span class="bf-icon
             bf-icon-buying-car
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-buying-car-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-buying-car
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-buying-car-round
             bf-icon__before"></span>
```

##### Paying for college

<span class="bf-icon
             bf-icon-paying-college
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-paying-college-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-paying-college
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-paying-college-round
             bf-icon__before"></span>
```

##### Owning a home

<span class="bf-icon
             bf-icon-owning-home
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-owning-home-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-owning-home
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-owning-home-round
             bf-icon__before"></span>
```

##### Debt

<span class="bf-icon
             bf-icon-debt
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-debt-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-debt
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-debt-round
             bf-icon__before"></span>
```

##### Building credit

<span class="bf-icon
             bf-icon-building-credit
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-building-credit-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-building-credit
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-building-credit-round
             bf-icon__before"></span>
```

##### Prepaid cards

<span class="bf-icon
             bf-icon-prepaid-cards
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-prepaid-cards-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-prepaid-cards
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-prepaid-cards-round
             bf-icon__before"></span>
```

##### Payday loan

<span class="bf-icon
             bf-icon-payday-loan
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-payday-loan-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-payday-loan
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-payday-loan-round
             bf-icon__before"></span>
```

##### Retirement

<span class="bf-icon
             bf-icon-retirement
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-retirement-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-retirement
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-retirement-round
             bf-icon__before"></span>
```


#### Web icons

##### User

<span class="bf-icon
             bf-icon-user
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-user-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-user
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-user-round
             bf-icon__before"></span>
```

##### WiFi

<span class="bf-icon
             bf-icon-wifi
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-wifi-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-wifi
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-wifi-round
             bf-icon__before"></span>
```

##### Search

<span class="bf-icon
             bf-icon-search
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-search-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-search
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-search-round
             bf-icon__before"></span>
```

##### Share

<span class="bf-icon
             bf-icon-share
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-share-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-share
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-share-round
             bf-icon__before"></span>
```

##### Link

<span class="bf-icon
             bf-icon-link
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-link-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-link
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-link-round
             bf-icon__before"></span>
```

##### External link

<span class="bf-icon
             bf-icon-external-link
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-external-link-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-external-link
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-external-link-round
             bf-icon__before"></span>
```

##### Audio mute

<span class="bf-icon
             bf-icon-audio-mute
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-mute-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-audio-mute
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-mute-round
             bf-icon__before"></span>
```

##### Audio low

<span class="bf-icon
             bf-icon-audio-low
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-low-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-audio-low
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-low-round
             bf-icon__before"></span>
```

##### Audio medium

<span class="bf-icon
             bf-icon-audio-medium
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-medium-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-audio-medium
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-medium-round
             bf-icon__before"></span>
```

##### Audio max

<span class="bf-icon
             bf-icon-audio-max
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-max-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-audio-max
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-audio-max-round
             bf-icon__before"></span>
```

##### Favorite

<span class="bf-icon
             bf-icon-favorite
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-favorite-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-favorite
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-favorite-round
             bf-icon__before"></span>
```

##### Unfavorite

<span class="bf-icon
             bf-icon-unfavorite
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-unfavorite-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-unfavorite
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-unfavorite-round
             bf-icon__before"></span>
```

##### Bookmark

<span class="bf-icon
             bf-icon-bookmark
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-bookmark-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-bookmark
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-bookmark-round
             bf-icon__before"></span>
```

##### Unbookmark

<span class="bf-icon
             bf-icon-unbookmark
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-unbookmark-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-unbookmark
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-unbookmark-round
             bf-icon__before"></span>
```

##### Settings

<span class="bf-icon
             bf-icon-settings
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-settings-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-settings
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-settings-round
             bf-icon__before"></span>
```

##### Menu

<span class="bf-icon
             bf-icon-menu
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-menu-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-menu
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-menu-round
             bf-icon__before"></span>
```

##### Lock

<span class="bf-icon
             bf-icon-lock
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-lock-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-lock
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-lock-round
             bf-icon__before"></span>
```

##### Unlock

<span class="bf-icon
             bf-icon-unlock
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-unlock-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-unlock
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-unlock-round
             bf-icon__before"></span>
```

##### Clock

<span class="bf-icon
             bf-icon-clock
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-clock-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-clock
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-clock-round
             bf-icon__before"></span>
```

##### Chart

<span class="bf-icon
             bf-icon-chart
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-chart-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-chart
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-chart-round
             bf-icon__before"></span>
```

##### Play

<span class="bf-icon
             bf-icon-play
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-play-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-play
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-play-round
             bf-icon__before"></span>
```

##### History

<span class="bf-icon
             bf-icon-history
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-history-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-history
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-history-round
             bf-icon__before"></span>
```

##### Table of contents

<span class="bf-icon
             bf-icon-table-of-contents
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-table-of-contents-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-table-of-contents
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-table-of-contents-round
             bf-icon__before"></span>
```

##### Newspaper

<span class="bf-icon
             bf-icon-newspaper
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-newspaper-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-newspaper
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-newspaper-round
             bf-icon__before"></span>
```

##### Microphone

<span class="bf-icon
             bf-icon-microphone
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-microphone-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-microphone
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-microphone-round
             bf-icon__before"></span>
```

##### Bullhorn

<span class="bf-icon
             bf-icon-bullhorn
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-bullhorn-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-bullhorn
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-bullhorn-round
             bf-icon__before"></span>
```

##### Double quote

<span class="bf-icon
             bf-icon-double-quote
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-double-quote-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-double-quote
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-double-quote-round
             bf-icon__before"></span>
```

##### Speech bubble

<span class="bf-icon
             bf-icon-speech-bubble
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-speech-bubble-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-speech-bubble
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-speech-bubble-round
             bf-icon__before"></span>
```

##### Information

<span class="bf-icon
             bf-icon-information
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-information-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-information
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-information-round
             bf-icon__before"></span>
```

##### Lightbulb

<span class="bf-icon
             bf-icon-lightbulb
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-lightbulb-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-lightbulb
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-lightbulb-round
             bf-icon__before"></span>
```

##### Dialogue

<span class="bf-icon
             bf-icon-dialogue
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-dialogue-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-dialogue
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-dialogue-round
             bf-icon__before"></span>
```

##### Date

<span class="bf-icon
             bf-icon-date
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-date-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-date
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-date-round
             bf-icon__before"></span>
```

##### Closing quote

<span class="bf-icon
             bf-icon-closing-quote
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-closing-quote-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-closing-quote
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-closing-quote-round
             bf-icon__before"></span>
```

##### Livestream

<span class="bf-icon
             bf-icon-livestream
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-livestream-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-livestream
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-livestream-round
             bf-icon__before"></span>
```

##### Parents

<span class="bf-icon
             bf-icon-parents
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-parents-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-parents
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-parents-round
             bf-icon__before"></span>
```

##### Servicemembers

<span class="bf-icon
             bf-icon-servicemembers
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-servicemembers-round
             bf-icon__before"></span>

```
<span class="bf-icon
             bf-icon-servicemembers
             bf-icon__before"></span>
<span class="bf-icon
             bf-icon-servicemembers-round
             bf-icon__before"></span>
```


### Icon character variables

Inspired by Font Awesome, we've added Less variables for adding icons in
Less files.

```less
.download-icon:after {
     .bf-icon();
     display: inline-block;
     content: @bf-icon-download;
     width: 1em;
     text-align: right;
}
```
