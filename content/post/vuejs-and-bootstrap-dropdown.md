---
title: "Using bootstrap-dropdown with Vuejs2"
description: "How to make it work..."
date: "2017-04-15"
tags: [ "Development", "VueJS", "dropdown" ]
---

So while, still exploring/playing with Vuejs, came across another peculiar task - making bootstrap's dropdown work with Vuejs, and here is how I make it finally work! (well atleast for me)

Though it seems very straight-forward at first - making the dropdown show or hide based on one flag data attribute. With chrome inspector, I could find out the  required css class for same is: `open`. 

But, the challenge was to hide it (remove the `open` class) once user clicks away outside the dropdown menu. This part was really tricky, and while I was trying more and more to get it resolve, I found it's kind of really basic one and thus some-one would have actually fix it before. So on little google, found out this really great  tool - [vue-clickaway](https://github.com/simplesmiler/vue-clickaway) which exactly solves this problem!

## So how does it work?

Let's see some code first:

```html
<li class="dropdown" :class="{open: showUserDropDown}">
    <a class="dropdown-toggle" v-on-clickaway="clicked_away_ChangeUserDropdown" @click="toggleChangeUserDropdown">Select User <span class="caret"></span></a>
    <ul class="dropdown-menu">
    <li><a @click="user = 'user1'">User 1</a></li>
    <li><a @click="user = 'user2'">User 2</a></li>
    <li><a @click="user = 'user3'">User 3</a></li>
    </ul>
</li>
```

and in vuejs app:

```javascript
var vm = new Vue({

    el: '#vue-instance',

    data: {
        user: '',
        showUserDropDown: false,
    },

    mixins: [VueClickaway.mixin],

    methods: {
        toggleChangeUserDropdown() {
            if(this.showUserDropDown) {
                this.showUserDropDown = false
            }
            else {
                this.showUserDropDown = true
            }
        },
        
        clicked_away_ChangeUserDropdown() {
            this.showUserDropDown = false
        },
    }

})
```

As you can see, `showUserDropDown` is the data var which is the actual flag on which we either apply the required css `open` class on the dropdown. Clicking on dropdown, `toggleChangeUserDropdown` handler is called, which correctly toggles the flag and thus shows or hides the menu.

But the important part - of hiding the dropdown menu, once anything else is clicked on, is done by `v-on-clickaway` directive, which is provided by `VueClickaway` mixin.

Do remember, this `v-on-clickaway` is required for every dropdown with different handlers (making the corresponding flag to false).

That's it! Simple right? But only due to [vue-clickaway](https://github.com/simplesmiler/vue-clickaway)!

For complete working code, please refer to my vuejs demo [repo](https://github.com/abhijitmamarde/vuejs_demos).

Please leave the comments if you like this article, or any feedback so that I could improve my knowledge.
