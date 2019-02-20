---
title: 'Creating consistently-styled checkboxes'
date: 'February 20, 2019'
author: 'Mark Youngman'
tags:
  - gds
  - checkboxes
  - css
---

# Creating consistently-styled checkboxes

When developing the new Hive IT website, we hit on the infamous problem of having consistent checkboxes across browsers and devices. To do this, we decided to emulate GDS checkboxes. In this article, we'll start with a basic input checkbox and label, and then with CSS move towards a working, accessible, consistently-styled checkbox.

GDS's approach to checkboxes is this:
- Make the actual checkbox input opaque.
- Draw a border directly over the top of checkbox input.
- Draw a smaller box inside that border, then make it look like a check mark by rotating it 45 degrees and then only drawing two sides.
- Only display the smaller box if the real, hidden checkbox input is checked.

We started with this setup: [gds-checkboxes.html version 1](https://github.com/hive-it-uk/consistently-styled-checkboxes/commit/f48232b287522f0b81aa6a9e91942b56174dacc8)

The first step is to hide the actual input checkbox, and then draw a box that will sit directly on top of the hidden input. To get the positioning correct, we set the containing div as `position: relative;` and all the child elements as `position: absolute;`. This does course the label text to move to the top left -- a problem we'll fix later. We add the box in the CSS using `::before` so that the HTML remains uncluttered.

[gds-checkboxes.html version 2](https://github.com/hive-it-uk/consistently-styled-checkboxes/commit/df4eb3061eef30b25158b14d2cc62393d5e013aa)

At this point, it isn't possible to click the input, and even if you could, there would be visual indication that it has been checked.

The second step is to create a tick, to visually indicate to the user whether the the input is checked or not. This done by creating a box with `::after`, rotating it 45 degrees, positioning and sizing it, and then only drawing two sides so it appears like a tick.

[gds-checkboxes.html version 3](https://github.com/hive-it-uk/consistently-styled-checkboxes/commit/77e76a1d12cf936fa6e8d235e824f5d201a6afe5)

Now we want to want the ability to check the input checkbox, and only display our new tick if the input is checked. Making the input checkable is as easy as setting `z-index: 1;`, allowing the input to capture a click event. To make the tick appear, we first make it `opacity: 0;` by default. Then we check whether any input is checked, and if it is we swing across to the tick, the `label::after`, and set `opacity: 1;`.

[gds-checkboxes.html version 4](https://github.com/hive-it-uk/consistently-styled-checkboxes/commit/55918a23e6078987a2cff0f26b37ffe2a3251577)

At this point, we're practically done. All that is required is to resize the text and move it into the correct position. Equally, we want add `cursor: pointer`

[gds-checkboxes.html version 5](https://github.com/hive-it-uk/consistently-styled-checkboxes/commit/bbd49abf47bea11b645ea3fdd6367d9f566e9838)

And we're done, right? Wrong! We haven't considered accessibility.

The first problem is that if the checkbox holds the focus, there is no indication that it holds it, making it difficult for people who rely on the keyboard to navigate the page. This is simply solved by adding an outline to the label if the associated input has the focus.

The second problem is a user might try to click label in order to check the checkbox. To solve this, we need to use javascript to add an event listener on the checkbox labels. We did this over using id and for attributes on the inputs and labels, as it doesn't work in Safari.

[gds-checkboxes.html version 6](https://github.com/hive-it-uk/consistently-styled-checkboxes/commit/6925d470fd335cae09a7d0a6c5d70d93e18442d9)

Finally, there was an IE11 bug to fix, where the sides of the checkbox tick that were set to width 0 would still be visible, which was fixed by also setting those sides to be transparent.

[gds-checkboxes.html final version](https://github.com/hive-it-uk/consistently-styled-checkboxes/commit/a4c8f3e0057f8396887ad1eb7d79f584241e28d8)

And then our work is done. 

