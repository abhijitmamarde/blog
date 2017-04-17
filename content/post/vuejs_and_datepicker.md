---
title: "Using datepicker with Vuejs2"
description: "How to make it work..."
date: "2017-04-13"
tags: [ "Development", "VueJS", "datepicker" ]
---

# Using datepicker with Vuejs2

Ok, so I am learning/exploring [Vuejs](https://vuejs.org/) now a days. It's a great `MVVM` framework which is light-weight and simple to learn. Simple enough to start **without any background** with other similar Javascript frameworks.

After going through initial code samples and tutorials, I wanted to go for more. At around same time, working on one side project, I needed to integrate [bootstrap-datepicker](https://github.com/uxsolutions/bootstrap-datepicker) with Vuejs. Did some search online, but couldn't find any working solution along with Vuejs2. So this is how I managed to do it.

First let's see the code:

```javascript
Vue.component('datepicker', {

template: `
    <div class="input-group date">
    <input type="text" class="form-control">
    <span class="input-group-addon">
        <i class="glyphicon glyphicon-calendar"></i>
    </span>
    </div>
    `,

props: ['updater', 'default'],

mounted: function(){

    //!important, as `this` is not available in the context below
    var t = this;
    var dp = $(this.$el.children[0]);

    if(t.default) {
        store.commit(t.updater, t.default)
        $(this.$el.children[0]).val(t.default)
    }

    dp.datepicker({
        format: date_format.toLowerCase(),  
        autoclose: true
    }).on('changeDate', function(e) {
        store.commit(t.updater, moment(e.date).format(date_format))
    });

    t.$on('reset', function(date_reset) {
      // console.log('reset called for:' + t.updater + " date:" + date_reset)
      store.commit(t.updater, date_reset)

      //to set the datepicker (date in textbox + last set valdate)
      dp.val(date_reset)
      dp.data({date: date_reset})
      dp.datepicker('update')
    });

},

});
```

## Details

The whole idea is, using vuejs, create a custom `component` so that, we can define any number of `datepicker` controls. For ex: the code below:

```html
<datepicker ref="startDate" updater="setStartDate" default="2016-12-01">
</datepicker>
```

Would simply create the datepicker control with default date populated as mentioned and would set the value in `startDate` data varaible. The `updater` mentions which store mutation/method would be called when the date is changed (by clicking in the datepicker event).

Here, an additional event `reset` is handled, which when emited, would call the require store mutation + setting the text value in date input field.

Here, `jquery` is used for initializing the datepicker control (*Remember:* bootstrap-datepicker requires it!) as well as to set the date value in datepicker from vuejs. Say on clicking a button (ex: setting to today's date)

One more point to note here, for selecting the input field in our template, we have used:

```jquery
$(this.$el.children[0])
```

Which means, the actual `input` field which we are initializing as datepicker, is the first children from our template's root element:

```html
<!-- root element of template -->
<div class="input-group date">
    <!-- input: first child under the root element -->
    <input type="text" class="form-control">
    ...
</div>
```

Thus, this would obviously change if the template is changed. *Keep a note of that!*

So, that's all about it. I have tried to pen down my first experience with vuejs!

Please do visit to my [vuejs-demo](https://github.com/abhijitmamarde/vuejs_demos) repo for the complete working code (and others).

