<p align="center"><img src="resources/img/entry-loader.svg" width="381" height="148" alt="Entries Loader And Filter plugin"></p>

# Entries Loader And Filter plugin for Craft CMS 4.x

This plugin will give you an option to load more entries using ajax.

You have options for filters based on fields and categories.

You have the sorting options available.

Number fields are converted to `range` input type, you can apply CSS as per your need.

You can control the options from the plugin setting page or while calling the `render()`. Just watch below animation for available backend options.

You can pass your custom query as well to get the entries as per your need.

You have the option to create `Multiple Load More` instances on different templates with different settings.

If you have already shown some entries (`desc` order) on a template, you can skip the number of entries by adjusting the offset in the Setting page or while calling the `render()`.We will skip those entries then.

# Setting Page

![Screenshot](resources/img/craft-entry-loader-settingpage.gif)

## Requirements

This plugin requires `Craft CMS 4.X` or later.

For Older versions please go to [Craft V3](https://plugins.craftcms.com/search?q=ajaxinate&tab=plugins&craft3)

## Installation.

To install the plugin, follow the below instructions.

1.  Open your terminal and go to your Craft project:

        cd /path/to/project

2.  Then tell Composer to load the plugin:

        composer require hestabit/craft-ajaxinate

3.  In the Control Panel, go to Settings → Plugins and click the “Install” button for Entries Loader And Filter.

### :warning: Please select these `Supported Field` type only in the plugin settings page or passing in `render()` call.

- Number
- Radio
- Checkbox
- Lightswitch

## Steps to add Load More functionality.

- Add `csrf token` to the template on which you want the Load More functionality. Scroll down for the example.

- Add the below code to the template (on which you added the `csrf token`). This code will add the `Load More button`.


```twig

{{ craft.craftAjaxinate.loadMoreVariable() }}

```

- Add this class **"ajaxDataDump"** to any existing element or create a new empty `<div class="ajaxDataDump"></div>`on your template or,
-  Use `containerClass`  within `render()`to create your own custom class that will be used to dump the loaded content. 

- **Rendering Template** : Create a new **separate template**. In this template, you have access to `{{ entries }}` object. This object has all the entries based on settings. `Don't put any extra markup here like header or footer`. See the example below.

* Select **Rendering Template** in the plugin’s Setting page or while calling `render()`, that you just created in the above step.

Sample code for calling the **Rendering Template**

```twig
        {{ craft.craftAjaxinate.render({
                template: 'ajax/stories.twig',
        }) }} 
```

* You are free to apply CSS and define HTML as per your need, on the entries in **Rendering Template**.

- **Options** Available options for **Load More** button:
  - btnWrapperClass : Class to be added on `<div>`.
  - loadMoreName : String to be used for **Load More** button. Default **Load More**

## Load More button example with options

    {{ craft.craftAjaxinate.loadMoreVariable({
          btnWrapperClass:'ajaxBtn',
          loadMoreName: 'Load More'})
    }}

## Steps to add sorting and filters

- All of the above steps should be done.
- To render sorting you need to add the below code in your template on which your Load More button is available:

```twig

       {{ craft.craftAjaxinate.render() }}
```

- Adjust settings as per your needs from the plugin’s Setting page or pass the available options while calling `render()`.

* **Options for filters and sorting** Pass below options to change the settings of the plugin. Available options are:

  - template : Pass the **Rendering Template** path.
  - limit : Pass the limit.
  - offset : Entries to skip and load on page load.
  - initLoad : To show entries on page load like `initLoad:true` or `initLoad:false`
  - resetBtnState : To show reset button like `resetBtnState:true` or `resetBtnState:false`
  - extraFilters : To show filters pass fields handle like `extraFilters: ['price', 'featuredEntry', 'anyOtherhandleName']`
  - sortingFilters : To show sorting options `( date and price handle only )` like
                      `sortingFilters:{
                        'date':'dateHandleName',
                        'price':'priceHandleName'
                      },`
  - section : Pass the sections name like `['news', 'services']` or select in plugin settings.
  - catGroup : Array of categories handle like `['cms','craftcms']` Required to show the categories filter.
  - catGroupLimit : Number of categories child to show, `default is 10`.
  - tagGroup : Name of tag group handle like `'blogtag'`.
  - query : Advanced query options, just pass the parameters in craft format. Scroll down for the examples.
  
  
* **Options for adding class** Pass below-mentioned options to add the class names on different input controls.

  - selectClass : Class to be added on `<select>`.
  - optionClass : Class to be added on `<option>`.
  - ulClass : Class to be added on `<ul>`
  - liClass : Class to be added on `<li>`
  - resetWrapperClass : Class to be added on `<div>` of reset button
  - catWrapperClass : Class to be added on `<div>` of category option
  - checkFieldDiv : Class to be added on `<div>` of checkbox fields
  - sortingWrapperClass : Class to be added on `<div>` of sorting option
  - messageClass : Class in which error/success messges will be shown.(pass in `render`)

 
* **Sorting control options** Pass below-mentioned options change the default strings.
 
  - nFirstName : String to be used for **Newest First**. Default is **Newest First**
  - oFirstName : String to be used for **Oldest First**. Default is **Oldest First**
  - lPriceName : String to be used for **Low To High**. Default is **Low To High**
  - hPriceName : String to be used for **High To Low**. Default is **High To Low**
  - dSortName : String to be used for **Default Sorting**. Default is **Default Sorting**

 
* **Options for changing the message**
  - noMoreDataMsg : Message to show when no entries are found as per the **above settings** or **user input**.

 
* **Options for onscroll events (pass parameters in `loadMoreVariable`)**
  - scrollActive : To activate pass `true` else `false`.You can set the default in CP as well.
  - pagesToLoad : Number of pages to load on each scroll.
  - bottomOffset : Number of pixels from the bottom when ajax will be triggered.
  - loaderTemplate : To override default loader pass your loader template path.


 ## Load more with scroll parameters 
 ```twig
  {{ craft.craftAjaxinate.loadMoreVariable({
      loadMoreName: 'Load More',
      scrollActive: true,
      pagesToLoad: 2,
      bottomOffset: 300
      }) 
  }}
```

 ## Sorting Example with options

```twig
    {{ craft.craftAjaxinate.render({
      selectClass: 'selectClassWrapper',
      optionClass: 'optionClassWrapper',
      sortingWrapperClass: 'sortingWrapperClasss'
    }) }}
  ```

Load 3 entries on page load and show filters of `featuredEntry (lightswitch), price (number)`.Also, show all child categories of `mensClothing and shoes` and tags of `technology`.
```twig
    {{ craft.craftAjaxinate.render({
      template: 'ajax/stories.twig',
      offset: 3,
      initLoad: true,
      resetBtnState: 1,
      section: ['news'], # or select in plugin settings
      extraFilters: ['featuredEntry', 'price'],
      catGroup: ['mensClothing', 'shoes'],
      sortingFilters:{
        'date': 'eventDate',
        'price': 'price',
      },
      tagGroup: ['technology'],      
    }) }}
```

Load 3 entries on page load and `shortDescription` should not be empty. As the limit is not passed CP settings will be used.
```twig
    {{ craft.craftAjaxinate.render({
      template: 'ajax/stories.twig',
      offset: 3,
      initLoad: true,
      resetBtnState: 1,
      section: ['news'], # or select in plugin settings
      query:{
        'shortDescription':':notempty:',
      },
      
    }) }} 
```


Load 3 entries on page load and `postDate` before `2019-07-31`.

```twig
    {{ craft.craftAjaxinate.render({
      template: 'ajax/stories.twig',
      offset: 3,
      initLoad: true,
      resetBtnState: 1,
      section: ['news'], # or select in plugin settings
      query:{
        'before': '2019-07-26'
      },
      
    }) }} 
```

Load 4 entries on page load and `postDate` before `2019-07-31` and `shortDescription` should not be empty. Each line in `query` has `and` relation between them.

```twig
    {{ craft.craftAjaxinate.render({
      template: 'ajax/stories.twig',
      offset: 4,
      limit: 5,
      initLoad: true,
      section: ['news'], # or select in plugin settings
      resetBtnState: 1,
      query:{
        'shortDescription':':notempty:',
        'before': '2019-07-26'
      }      
    }) }}
```

`title` either `foo` or `bar`. As `template` is not passed CP settings will be used.

```twig
    {{ craft.craftAjaxinate.render({
      limit: 5,
      resetBtnState: 1,
      section: ['news'], # or select in plugin settings
      query:{
        'where': ['or', ['like','title','foo'], ['like','title','bar']],
      }      
    }) }}
```


`title` either `foo` or `bar` or `field_featuredEntry` (`lightswitch`) is active.

Append **field_** before handleName.

```twig
    {{ craft.craftAjaxinate.render({
      limit: 5,
      resetBtnState: 1,
      section: ['news'], # or select in plugin settings
      query:{
        'where': ['or', ['like','title','foo'], ['like','title','bar']],
        'orWhere': ['and', ['=','field_featuredEntry',1]],
      }      
    }) }}
```

Between `2019-07-12` and `2019-07-31` dates.

Append **field_** before handleName.

```twig
    {{ craft.craftAjaxinate.render({
      limit: 5,
      resetBtnState: 1,
      section: ['news'], # or select in plugin settings
      query:{
        'postDate': ['and', '>= 2019-07-12', '<= 2019-07-31'],
        'orWhere': ['and', ['=','field_featuredEntry',1]],
      }      
    }) }}
```


<details>

## <summary> Example of csrf  ( There is no need to declare csrf if already declared)</summary>

```twig
  # Example of csrf  ( There is no need to declare csrf if its already declared in your site)
  {% set csrfToken = {
    csrfTokenName: craft.app.config.general.csrfTokenName,
    csrfTokenValue: craft.app.request.csrfToken,
  } %}

  {% js %}
    window.Craft = {{ csrfToken | json_encode | raw }};
  {% endjs %}

```

</details>

---

<details>
<summary>Example of rendering template (You have access to {{entries}} in this template, You have to iterate on this object)</summary>

```twig

{# Access all the fields in the iteration. #}

{% for item in entries %}
  <a href="{{item.url}}">{{ item.title }}</a>
  .....
{% endfor %}
```

</details>

---

## Entries Loader And Filter Roadmap

Future development plans for the plugin (if any suggestion then do create a feature request over GitHub):

- [x] Load more entries (Particular template )
- [x] Option to select the default template in backend
- [x] Sorting
- [x] Filters
- [x] Multiple Load More
- [x] Custom Queries `new`
- [x] Option to load entries on onload
- [x] Filter based on future entries
- [x] Onscroll `new`
- [ ] Search

## Support

Found any issue :confused: , [Create a Github Issue](https://github.com/Hestabit/craft-ajaxinate/issues/new)

## Credits

- Developed by [Saurabh Ranjan](https://github.com/insaurabh)
- Boilerplate by [pluginfactory](https://pluginfactory.io)

Brought to you by [HestaBit](https://github.com/Hestabit)
