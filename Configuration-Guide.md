[<img title="Arts and Humanities Research Council" src="https://britishlibrary.github.io/locating-a-national-collection/graphics/UKRI-logo.png" height="50" align="right">](https://www.ukri.org/)
[<img title="Towards a National Collection" src="https://britishlibrary.github.io/locating-a-national-collection/graphics/TaNC-logo.png" height="50" align="right">](https://www.nationalcollection.org.uk/)
[<img title="The British Library" src="https://britishlibrary.github.io/locating-a-national-collection/graphics/BL.svg" height="50" align="right">](https://www.bl.uk/)
[<img title="Pelagios Network" src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="50" align="right">](https://pelagios.org/)
# *Peripleo* Configuration Guide
> This Guide assumes that you have already installed *Peripleo*, as outlined in the [Installation Guide](./README.md). It also assumed that you have created a compatible dataset, perhaps using *Locolligo* as outlined [here](https://github.com/docuracy/Locolligo/blob/main/User-Guide-Basic.md).

This is an example of the basic minimum configuration settings required in `peripleo.config.json`: 

```json
{
  "initial_bounds": [-7.9, 49.5, 2.2, 59.4],
  "map_style": "./map-style-OSM.json",
  "data": [
    {
      "format": "LINKED_PLACES",
      "src": "./data/VisitPlus-UK.lp.json"
    }
  ]
}
```

* `initial_bounds`: here you specify the coordinates (in degrees of longitude and latitude) of the bottom left and top right corners of your map, in the format `[bottom-left-longitude, bottom-left-latitude, top-right-longitude, top-right-latitude]`. *You can find the coordinates (as **latitude**,**longitude**) of any point on Earth by right-clicking on a [Google Map](https://www.google.com/maps/).* 
* `map_style` (optional): the URL to a vector basemap style. The basic supplied style employs Open Street Map tiles: if this line is deleted, *Peripleo* will load with an empty background. *See [below](#alternative-map-styles) for other options.*
* `data`: this is where you specify the URL of your dataset. The example points to a dataset in a folder named `data` *relative* to the address of `index.html`: it could instead point to an *absolute* address anywhere on the internet (one which starts with `http://` or `https://`). *See [below](#multiple-datasets) for an example using named, multiple datasets and HTML attributions.*

### **And that's it!**
- Point your browser to the URL of your `index.html` (perhaps something like **https://*username*.github.io/peripleo/** if you're using a default GitHub installation), and watch your map load. 
- *If you wish, you can change the page title and 'social previews' by editing the `<title>` and `<meta>` tags in your `index.html`.*
- There are many other ways to configure *Peripleo* to suit your data and your target audience: after looking at [Sharing your map](#-sharing-your-map), [Initial View](#-initial-view), and [Embedding your map](#-embedding-your-map), read on for [Advanced Configuration](#advanced-configuration).

---

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Sharing your Map

*Peripleo* updates the URL in your browser's search bar automatically whenever you change the visualisation mode, facet selection, or move the map, so you can simply **copy the URL** to share a particular map view. *Note, however, that the updated URL is not visible if your map is embedded (see [below](#embedding-your-map)).*
* *If you share your map on Twitter, please use the hashtags [`#BL_LaNC`](https://twitter.com/search?q=%23BL_LaNC&src=typed_query&f=live) [`#Peripleo`](https://twitter.com/search?q=%23Peripleo&src=typed_query&f=live)!*

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Initial View

By default, *Peripleo* will open a map with preset bounds (described [above](#peripleo-configuration-guide)), with plain markers and without any facet(s) selected. You can change this default behaviour by adding parameters to the URL following the model:

https://*your-url*/#/*zoom*/*longitude*/*latitude*/mode=*points|clusters|heatmap*+facet=*type*

   For example:

         https://britishlibrary.github.io/peripleo-lanc/leifuss/#/8.16/-3.3969/50.6397/mode=points+facet=type
   
   If you wish to set only the visualisation mode or facet selection, replace *zoom*/*longitude*/*latitude* with *?*/*?*/*?*. You cannot set only `zoom`, `longitude`, or `latitude`: you must provide values either for all three parameters or for none.

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Embedding your Map

Maps made using *Peripleo* can be embedded in other **web sites**, **wikis**, and **blogs**. For example:

* To embed in **WordPress** blogs, simply insert an HTML block something like this:

      <iframe src="https://britishlibrary.github.io/peripleo/#/5.88/-2.9080/53.1812/mode=heatmap+facet=type" title="VisitPlus" width="100%" height="800px" allowfullscreen="true"></iframe>
      
* To embed in a **Mediawiki** site, you will need to install the [HTMLTags extension](https://www.mediawiki.org/wiki/Extension:HTML_Tags), and include the following in your `LocalSettings.php` file:

      wfLoadExtension( 'HTMLTags' );
      $wgHTMLTagsAttributes['iframe'] = ['src','height','width','allowfullscreen','loading','name','referrerpolicy','srcdoc'];
      
   And on the page where you want the iframe to appear, something like:
      
      <htmltag tagname="iframe" src="https://britishlibrary.github.io/peripleo/#/5.88/-2.9080/53.1812/mode=heatmap+facet=type" width="100%" height="800px" allowfullscreen=true></htmltag>

____

# Advanced Configuration

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Alternative Map Styles
Instead of the default Open Street Map basemap, you could use one from [MapTiler](https://cloud.maptiler.com/maps/), but you will need to read the documentation and **get your own unique *key***. Alternatively, you might omit the map style altogether and use a tile baselayer instead: see [below](#-additional-baselayers).

Example using the [MapTiler Outdoor](https://cloud.maptiler.com/maps/outdoor/) style:
```json
"map_style": "https://api.maptiler.com/maps/outdoor/style.json?key=fc1c65e016c119b81e46c113b5cf8ebf8275b3b7"
```

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Baselayers
You can add any number of baselayers to your map, which might be GeoJSON (points, lines, or shapes) or raster tiles. All three types are combined in the example below (each enclosed in curly brackets and separated by commas):

```json
"layers": [
  { 
    "name": "Rivers & Canals", 
    "type": "geojson",
    "src": "./layers/waterways.geojson", 
    "color": "#5555ff" 
  },
  {
    "name": "A Google-Maps-style XYZ tile layer",
    "type": "raster",
    "tiles": [
      "https://www.example-tileserver.com/tiles/{z}/{x}/{y}.png"
    ],
    "tileSize": 256,
    "attribution": "Example tiles",
    "minzoom": 0,
    "maxzoom": 22
  }
]
```
* Note that you can have multiple tile layers which are visible at differing zoom levels.

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Additional Datasets
To combine multiple datasets on your map, simply add them to the `data` array like this:

```json
"data": [
    {
      "name": "VisitPlus",
      "format": "LINKED_PLACES",
      "src": "./data/VisitPlus-UK.lp.json",
      "attribution": "VisitPlus © <a href=\"https://britishlibrary.github.io/locating-a-national-collection/\" target=\"_blank\"><i>Locating a National Collection</i> Partners & Contributors</a>"
    },
    {
      "name": "VisitMinus",
      "format": "LINKED_PLACES",
      "src": "./data/VisitMinus.lp.json",
      "attribution": "VisitMinus © Fictitious Entity"
    }
]
```
* Each dataset should be given a distinctive `name`.
* The `attribution` (optional) will be shown in a collapsible bar at the bottom of the map. It can include HTML, but quotes need to be preceded by a backslash, as shown. 

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Link Icons
You can assign link icons to prettify external links in your dataset like this:

``` json
"link_icons": [
    { "pattern": "maps.google.com",  "img": "./logos/maps.google.com.png", "label": "Google Maps" },
    { "pattern": "www.geograph.org.uk", "img": "./logos/geograph.org.png", "label": "Geograph" },
    { "pattern": "en.wikipedia.org", "img": "./logos/en.wikipedia.org.png", "label": "Wikipedia" },
    { "pattern": "www.wikidata.org", "img": null, "label": "Wikidata" },
    { "pattern": "example.org/bad", "img": null, "label": "Bad Example" },
    { "pattern": "example.org/excellent", "img": "./logos/excellent.org.png", "label": "Excellent Example" },
    { "pattern": "example.org", "img": null, "label": "Terrible Example" },
    { "pattern": "www.geonames.org", "img": null, "label": "GeoNames" },
    { "pattern": "sws.geonames.org", "img": null, "label": "GeoNames" }
]
```
* Links matching a `pattern` will be grouped together, identified by the logo/icon referenced by `img` and by the associated `label`.
* Ideally, logo/icon images should be 100px square.
* Any link matching a `pattern` with an `img` set to `null` will be ignored and not shown in map popups.
* The first matching `pattern` is used (if any): in the example above, no `example.com` links will be shown except those beginning with `example.org/excellent`.

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Facets
A `facets` array is used to specify filtering of the loaded datasets. *If omitted, default facetting will be implemented based on dataset names, `feature.properties.type` values (or all values in each `feature.types` array), and the presence of an image link.*

``` json
  "facets": [
    { "name": "custodian", "path": ["relations", "label"] },
    { "name": "organisation", "path": ["properties", "organisation"] },
    { "name": "art type", "path": ["properties", "artworkType"] }
  ]
```
* Every custom facet configuration __must__ have both a `name` and a `path`.
* The `name` will be shown (capitalised) as a title in the filter legend.
* The `path` defines from which part of the record *Peripleo* will aggregate result counts, so for the example above:
  * `Custodian` will include the value of every `label` in each `feature.relations` array.
  * `Organisation` will include the value of `organisation` in each `feature.properties` object.
  * `Art Type` will include the value of `artworkType` in each `feature.properties` object.

*Peripleo* supports multi-value aggregation as well as paths with list structures, so if you set `path: [ 'types', 'label' ]`, *Peripleo* will be able to aggregate from any the following four data feature structures:

```json
{
  "types": {
    "label": "My Custom Type #1"
  }
}

{
  "types": [
    { "label": "My Custom Type #1" },
    { "label": "My Custom Type #2" }
  ]
}

{
  "types": {
    "label": [ "My Custom Type #1", "My Custom Type #2" ]
  }
}

{
  "types": [
    { "label": [ "My Custom Type #1", "My Custom Type #2" ] },
    { "label": [ "My Custom Type #3", "My Custom Type #4" ] }
  ]
}
```

Furthermore, you can add a `condition` that *Peripleo* will look for while aggregating results. So if you have facets configured like this...

```json
"facets": [
  { "name": "method", "path": ["relations", "label"], "condition": [ "relationType", "aat:300138082" ] }
]
```
... then in the following example *Peripleo* will include the first match, but not the second:

```json
{
  "types": [
    { "relationType": "aat:300138082", "label": "techniques (processes)" },
    { "relationType": "wd:Q3249551", "label": "process" }
  ]
}
```
And finally, facetting on dataset names and on all kinds of `type` (whether `feature.properties.type` values or values in a `feature.types` array) can be implemented with single words:
```json
"facets": [
  "dataset",
  "type"
]
```
## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Custom Welcome Screen

You can add a custom welcome message to the loading screen via the `welcome_message` config argument. The value of `welcome_message` must point to the (relative or absolute) URL of a markdown file. If a custom welcome message is configured, Peripleo will __not__ automatically switch to the map interface as soon as the data is loaded. Instead, an __Ok__ confirmation button will be displayed after loading is complete. The map interface will open after the user hits __Ok__.

Example:

```json
"welcome_message": "welcome.md"
```

In the above example, a file named `welcome.md` must be located on your webserver, in the same folder as the config file.

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Disabling the 'My Location' Button

Per default, Peripleo will show a 'My Location' button. Clicking this button will zoom the map to the users' current location, as reported by the browser via the [Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API). (On mobile devices, the button will make use of the GPS device.)

You can disable this feature, and hide the button by adding the following line to the config file:

```json
"disableMyLocation": true
```

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Enabling Google Analytics Tracking

Per default, Peripleo will __not__ perform any user activity tracking. Should you wish to track anonymized interactions, you can enable Peripleo's built-in Google Analytics support. To do so, simply add your Google Analytics tracking code to the config file:

```json
"ga_id": "G-XXXXXXXXXX"
```

If `ga_id` is present in the config, Peripleo will send events for the following user activities:

- Page load
- Search (event `search`, with the user query as `query` argument)
- Selection of a map marker (event `selection` with `id` as argument)
- Selection of a filter (event `filter` with `filter` and `filter_value` as arguments)
- Navigation to external source page (event `navigation` with `destination` as argument)

# Advanced Example
A fully-featured `peripleo.config.json` file might look like this:
```json
{
  "initial_bounds": [-7.9, 49.5, 2.2, 59.4],
  "map_style": "./map-style-OSM.json",
  "layers": [
    { 
      "name": "Rivers & Canals", 
      "type": "geojson",
      "src": "./layers/waterways.geojson", 
      "color": "#5555ff" 
    },
    {
      "name": "A Google-Maps-style XYZ tile layer",
      "type": "raster",
      "tiles": [
        "https://www.example-tileserver.com/tiles/{z}/{x}/{y}.png"
      ],
      "tileSize": 256,
      "attribution": "Example tiles",
      "minzoom": 0,
      "maxzoom": 22
    }
  ],
  "data": [
    {
      "name": "VisitPlus",
      "format": "LINKED_PLACES",
      "src": "./data/VisitPlus-UK.lp.json",
      "attribution": "VisitPlus © <a href=\"https://britishlibrary.github.io/locating-a-national-collection/\" target=\"_blank\"><i>Locating a National Collection</i> Partners & Contributors</a>"
    },
    {
      "name": "VisitMinus",
      "format": "LINKED_PLACES",
      "src": "./data/VisitMinus.lp.json",
      "attribution": "VisitMinus © Fictitious Entity"
    }
  ],
  "facets": [
    { "name": "method", "path": ["relations", "label"], "condition": [ "relationType", "aat:300138082" ] },
    { "name": "organisation", "path": ["properties", "organisation"] },
    { "name": "art type", "path": ["properties", "artworkType"] },
    "dataset",
    "type"
  ],
  "link_icons": [
    { "pattern": "maps.google.com",  "img": "./logos/maps.google.com.png", "label": "Google Maps" },
    { "pattern": "www.geograph.org.uk", "img": "./logos/geograph.org.png", "label": "Geograph" },
    { "pattern": "en.wikipedia.org", "img": "./logos/en.wikipedia.org.png", "label": "Wikipedia" },
    { "pattern": "www.wikidata.org", "img": null, "label": "Wikidata" },
    { "pattern": "example.org/bad", "img": null, "label": "Bad Example" },
    { "pattern": "example.org/excellent", "img": "./logos/excellent.org.png", "label": "Excellent Example" },
    { "pattern": "example.org", "img": null, "label": "Terrible Example" },
    { "pattern": "www.geonames.org", "img": null, "label": "GeoNames" },
    { "pattern": "sws.geonames.org", "img": null, "label": "GeoNames" }
  ]
}
```
