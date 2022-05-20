[<img title="Arts and Humanities Research Council" src="https://britishlibrary.github.io/locating-a-national-collection/graphics/UKRI-logo.png" height="50" align="right">](https://www.ukri.org/)
[<img title="Towards a National Collection" src="https://britishlibrary.github.io/locating-a-national-collection/graphics/TaNC-logo.png" height="50" align="right">](https://www.nationalcollection.org.uk/)
[<img title="The British Library" src="https://britishlibrary.github.io/locating-a-national-collection/graphics/BL.svg" height="50" align="right">](https://www.bl.uk/)
[<img title="Pelagios Network" src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="50" align="right">](https://pelagios.org/)
# *Peripleo* Configuration Guide
> This Guide assumes that you have already installed *Peripleo*, as outlined in the [Installation Guide](./README.md).

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
  ],
  "link_icons": [
  ]
}
```

* `initial_bounds`: here you specify the coordinates (in degrees of longitude and latitude) of the bottom left and top right corners of your map, in the format `[bottom-left-longitude, bottom-left-latitude, top-right-longitude, top-right-latitude]`. *You can find the coordinates (as **latitude**,**longitude**) of any point on Earth by right-clicking on a [Google Map](https://www.google.com/maps/).* 
* `map_style` (optional): the URL to a vector basemap style. The basic supplied style employs Open Street Map tiles: if this line is deleted, *Peripleo* will load with an empty background. *See [below](#alternative-map-styles) for other options.*
* `data`: this is where you specify the URL of your dataset. The example points to a dataset in a folder named `data` *relative* to the address of `index.html`: it could instead point to an *absolute* address anywhere on the internet (one which starts with `http://` or `https://`). *See [below](#multiple-datasets) for an example using named, multiple datasets and HTML attributions.*

### **And that's it!**
- Point your browser to the URL of your `index.html` (perhaps something like **https://*username*.github.io/peripleo/** if you're using a default GitHub installation), and watch your map load. 
- *If you wish, you can change the page title and 'social previews' by editing the `<title>` and `<meta>` tags in your `index.html`.*
- There are many other ways to configure *Peripleo* to suit your data and your target audience: after looking at [Sharing your map](#-sharing-your-map) and [Embedding your map](#-embedding-your-map), read on for [Advanced Configuration](#advanced-configuration).

---

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Sharing your map

*Peripleo* updates the URL in your browser's search bar automatically whenever you change the visualisation mode, facet selection, or move the map, so you can simply **copy the URL** to share a particular map view. *Note, however, that the updated URL is not visible if your map is embedded (see [below](#embedding-your-map)).*

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Embedding your map

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

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Additional Baselayers
You can add any number of baselayers to your map, which might be GeoJSON (points, lines, or shapes), a georeferenced map (GeoTIFF), or raster tiles. All three types are combined in the example below:

```json
[
  { 
    "name": "Rivers & Canals", 
    "type": "geojson",
    "src": "./layers/waterways.geojson", 
    "color": "#5555ff" 
  },
  {
    "name": "Warped Map of Europe",
    "type": "raster",
    "src": "./layers/europe.geotiff",
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

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Additional Datasets

```json
"data": [
    {
      "name": "VisitPlus",
      "format": "LINKED_PLACES",
      "src": "./data/VisitPlus-UK.lp.json",
      "attribution": "VisitPlus © <a href=\"https://britishlibrary.github.io/locating-a-national-collection/\" target=\"_blank\"><i>Locating a National Collection</i> Partners & Contributors</a>"
    }
]
```
* The `attribution` can include HTML, but quotes need to be preceded by a backslash, as shown. 

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Link Icons

``` json
"link_icons": [
    { "pattern": "maps.google.com",  "img": "./logos/maps.google.com.png", "label": "Google Maps" },
    { "pattern": "www.geograph.org.uk", "img": "./logos/geograph.org.png", "label": "Geograph" },
    { "pattern": "en.wikipedia.org", "img": "./logos/en.wikipedia.org.png", "label": "Wikipedia" },
    { "pattern": "www.wikidata.org", "img": null, "label": "Wikidata" },
    { "pattern": "www.geonames.org", "img": null, "label": "GeoNames" },
    { "pattern": "sws.geonames.org", "img": null, "label": "GeoNames" }
]
```
* `link_icons`: These are used to prettify external links in your dataset, and are defined by the link's domain name and a URL pointing to an icon (ideally 100px square). In map pop-ups, links are sorted in the order in which they appear in this list, but omitted if the `img` attribute is set to `null`.

## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Facets

* `facets`: If you want your dataset to be filtered, this is where you specify how (more details [below](#about-facets)).
* 
``` json
  "facets": [
    "type"
  ],
```

Every custom facet configuration __must__ have a `name` and a `path` field. The name will be shown (capitalized)
as a title in the filter legend. The `path` defines from which part of the record *Peripleo* will aggregate 
result counts.

For example, if you set `path: 'category'`, *Peripleo* will aggregate the values found in the `category` field at 
the top of each data record. If you want to aggregate the values found in `properties.category` of each record,
set the path to `path: [ 'properties', 'category' ]`.

*Peripleo* supports multi-value aggregation as well as paths with list structures. I.e. if you set 
`path: [ 'types', 'label' ]`, *Peripleo* will be able to aggregate from the following record data structures:

```json
{
  "types": {
    "label": "My Custom Type #1"
  }
}
```

```json
{
  "types": [{
    "label": "My Custom Type #1"
  },{
    "label": "My Custom Type #2"
  }]
}
```


```json
{
  "types": {
    "label": [ "My Custom Type #1", "My Custom Type #2" ]
  }
}
```

```json
{
  "types": [{
    "label": [ "My Custom Type #1", "My Custom Type #2" ]
  }, {
    "label": [ "My Custom Type #3", "My Custom Type #4" ]
  }]
}
```

Additionally, you can add a `condition` that *Peripleo* will look for while aggregating results. For example,
if you set the condition `[ "relationType", "aat:300138082" ]`, Peripleo will count the first match, but not the second.

```json
{
  "types": [{
    "relationType": "aat:300138082",
    "label": "My Custom Type #1"
  },{
    "relationType": "someOtherType",
    "label": "My Custom Type #2"
  }]
}
```
## [<img src="https://github.com/britishlibrary/peripleo-lanc/blob/5e65ec35bfb0389bdc790d235898459c13a3abda/logos/pelagios.svg" height="20">](#) Initial View

By default, *Peripleo* will open a map with preset bounds (described [above](#peripleo-configuration-guide)), with plain markers and without any facet(s) selected. You can change this default behaviour by adding parameters to the URL following the model given below.

         https:// `your-url` /#/ `zoom` / `longitude` / `latitude` /mode= `points|clusters|heatmap` +facet= `type`

   For example:

         https://britishlibrary.github.io/peripleo-lanc/leifuss/#/8.16/-3.3969/50.6397/mode=points+facet=type
   
   If you wish to set only the visualisation mode or facet selection, replace `zoom` / `longitude` / `latitude` with `?/?/?`. You cannot set only `zoom`, `longitude`, or `latitude`: you must provide values either for all three parameters or for none.
