---
layout: post
title: "Converting Yaml to JSON in JavaScript"
date: 2017-02-25 23:19
comments: true
categories: Development
keywords: Middleman, JSON, YAML, js-yaml
---
I use the Middleman [data files](https://middlemanapp.com/advanced/data-files) feature for my chess [site](http://www.gerardcondon.com/chess/).
These files are written in yaml. 
I could have used json directly for these which would have removed the need for this post but I find yaml is easier for manual editing - with json you constantly have to worry about matching up brackets. 
From the Middleman erb files, when I build locally I am able to access things like `data.fide.ratings`. 
However I would like to be able to load this data remotely and process it from my javascript code.

To load the remote file I just use the JQuery Ajax [get](https://api.jquery.com/jquery.get/) function and pass it the url of the file that I want. 
Then I use the [js-yaml](https://github.com/nodeca/js-yaml) library to parse the result and create a `data` json object from this. 
Now from my JavaScript code I am able to access the same data using the same notation.

```javascript
function loadYamlFromUrl() {
    $.get( "https://raw.githubusercontent.com/gerardcondon/chess/master/website/data/ratings.yaml", function( data ) {
        var data = jsyaml.load(data);
        // can now access data.fide etc
    });
}
```

This has worked well for me and for any data file that I was able to access via Middleman, I can now access the same data from JavaScript in the same format.
