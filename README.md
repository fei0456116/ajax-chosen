# Ajax-Chosen

This project is an addition to the excellent [Chosen jQuery plugin](https://github.com/harvesthq/chosen) that makes HTML input forms more friendly.  Chosen adds search boxes to `select` HTML elements, so I felt it could use the addition of ajax autocomplete for awesomely dynamic forms.

This script bootstraps the existing Chosen plugin without making any modifications to the original code. Eventually I would love to see this functionality built-in to the library, but until then, this seems to work pretty well.

## How to Use

This plugin exposes a new jQuery function named `ajaxChosen` that we call on a `select` element. The first argument are the options passed to the jQuery $.ajax function. The `data` parameter is optional, and the `success` callback is also optional.

The second argument is a callback that tells the plugin what HTML `option` elements to make. It is passed the data returned from the ajax call, and you have to return an object where the key is the HTML `option` value attribute and the value is the text to display. In other words:

	terms[3] = "Ohio";
	
becomes:
	
	<option value="3">Ohio</option>

or for grouping:

	terms[3] = {
	  group : "true",
	  text : "City",
	  items : {}
        }
        terms[3].items[10] = 'Stockholm'
        terms[3].items[23] = 'Moskow'
          
becomes:

        <optgroup label="City">
            <option value='10'>Stockholm</option>
            <option value='23'>Moskow</option>
        </optgroup>

Note: 

In case of a bug in chosen, it is necessary to change choosen.css.

Add 

	display: list-item;

to 

	.chzn-container .chzn-results .group-result {

class

### Options

There are some additional ajax-chosen specific options you can pass into the first argument to control it's behavior.

* minTermLength: minimum number of characters that must be typed before an ajax call is fired
* afterTypeDelay: how many milliseconds to wait after typing stops to fire the ajax call
* jsonTermKey: the ajax request key to use for the search query (defaults to `term`)

## Example Code

``` js
$("#example-input").ajaxChosen({
	type: 'GET',
	url: '/ajax-chosen/data.php',
	dataType: 'json'
}, function (data) {
	var terms = {};
			
	$.each(data, function (i, val) {
		terms[i] = val;
	});
			
	return terms;
});
```
To use optgroup tag result 'terms' item should be changed to return list of group objects:

``` js
$("#example-input").ajaxChosen({
	type: 'GET',
	url: '/ajax-chosen/grouped.php',
	dataType: 'json'
}, function (data) {
	var terms = {};
	$.each(data, function (i, val) {
		terms[i] = {
			group: true,
			text: val.name,
			items: {}
		};
		$.each(val.Values, function (i1, val1) {
			terms[i].items[val1.Id] = val1.name;
		});
	});
	return terms;
});

```

## Developing ajax-chosen

ajax-chosen is written in Coffeescript, so there is a Cakefile provided that will perform all necessary tasks for you. Simply run `cake` to see all available commands.
