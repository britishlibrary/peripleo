# Peripleo: Setup Guide

## What you need to get started

* Dataset(s) formatted as [Linked Places Format (LPF)](https://github.com/LinkedPasts/linked-places-format/blob/master/README.md) or GeoJSON. 
    * You can use any such dataset if it is accessible via a URL.
    * If your data is in a spreadsheet or delimited text (for example CSV), you will need to convert it using a tool such as [Locolligo](https://github.com/docuracy/Locolligo/blob/main/README.md).
* Somewhere to host and serve a simple HTML file, together with any datasets not hosted elsewhere. This guide begins with instructions for hosting your map on GitHub Pages: you might instead copy the files from the `docs` folder to another server.

## Setting up GitHub Pages

1. If you do not already have a GitHub account, go to https://github.com/signup and create one.
2. Sign in to GitHub.
3. Click on `Import Repository`.
4. Type `https://github.com/britishlibrary/peripleo` as your 'old repository's clone URL'.
5. Type a name for your new repository (perhaps related to the specifics of the map you are going to create), and decide whether to make it Public or Private.
6. Click on `Begin Import`, and wait for the process to complete.
7. Open your new repository either by clicking on the link shown when import completes, or by following the link to 'Your repositories' revealed by clicking on the icon top-right.
8. Click on `Settings`.
9. Click on `Pages`.
10. In the 'Source' section, click on 'None' and select `Main`; next to that, select folder `/docs` in the drop-down list.
11. Click on `Save`.
12. The system will then give you the URL on which your site is published. **Please note that every time you edit your site it may take a few minutes for it to be rebuilt before the changes are evident**.
13. You can now check that *Peripleo* is running correctly with the default example configuration, or you can go right ahead and start reconfiguring it to display your own dataset(s).

## Editing Configuration Files on GitHub

1. Navigate to your repository and click on `Code`, then select the `Docs` folder.
2. Configuration is done principally in `peripleo.config.json`, but (optionally) also in `index.html`.
3. Click on a filename to see its contents, and then on the pencil icon to begin editing.
4. Configuration settings are described in detail below. When you have finished, you need to type a *very* brief description of the changes you have made, and then click on `Commit changes`.
5. After a minute or so, you can check your modifications by going to your publication URL.

## Configuring your map

These are the configuration settings for the example map [here](https://descartes.emew.io/VCH/): 

```json
{
  "initial_bounds":[
    -5.5,
    49.5,
    2.2,
    55.8
  ],
  "map_style":"https://api.maptiler.com/maps/outdoor/style.json?key=kqQCjfFMEWhLGBLyMnwW",
  "layers":[
    {
      "name":"Ordnance Survey 6-inch (1888-1913)",
      "type":"raster",
      "tiles":[
        "https://nls-1.tileserver.com/fpsUZbULUtp1/{z}/{x}/{y}.png"
      ],
      "tileSize":256,
      "attribution":"Historical OS 6-inch Map Layer, 1888-1913, by National Library of Scotland",
      "minzoom":15,
      "maxzoom":22
    },
    {
      "name":"Ordnance Survey 1-inch (1885-1900)",
      "type":"raster",
      "tiles":[
        "https://geo.nls.uk/maps/os/1inch_2nd_ed/{z}/{x}/{y}.png"
      ],
      "tileSize":256,
      "attribution":"Historical OS 1-inch Map Layer, 1885-1900, by National Library of Scotland",
      "minzoom":0,
      "maxzoom":15
    }
  ],
  "data":[
    {
      "name":"VCH Places",
      "format":"LINKED_PLACES",
      "src":"https://docuracy.github.io/Locolligo/datasets/VCH-Places.lp.json"
    }
  ],
  "facets":[
    {
      "name":"County",
      "path":[
        "properties",
        "county"
      ]
    },
    {
      "name":"Schools",
      "path":[
        "properties",
        "schools"
      ]
    },
    "type"
  ],
  "link_icons":{
    "www.british-history.ac.uk":"https://raw.githubusercontent.com/britishlibrary/peripleo-lanc/main/logos/bho.png"
  }
}
```

* `initial_bounds`: Here you specify the coordinates (in degrees of longitude and latitude) of the bottom left and top right corners of your map, in the format `[bottom-left-longitude, bottom-left-latitude, top-right-longitude, top-right-latitude]`.
* `map_style` (optional): the URL to a vector basemap style, e.g. from [MapBox](https://docs.mapbox.com/api/maps/styles/) or [MapTiler](https://www.maptiler.com/cloud/). If omitted, *Peripleo* will load with an empty background.
* `layers` (optional): In this array you can configure additional base layers, enclosed in {curly brackets}. *Peripleo* currently supports GeoJSON and raster tile sources (more details [below](#about-additional-baselayers)).
* `data`: This is the array where your put information about each of your datasets, enclosed in {curly brackets}. You can use multiple datasets, separating them with a comma.
* `facets`: If you want your dataset to be filtered, this is where you specify how (more details [below](#about-facets)).
* `link_icons`: These are used to prettify external links in your dataset, and are defined by the link's domain name and a URL pointing to an icon (ideally 100px square).

____

# Further Configuration

## Additional Baselayers

In the `layers` array, *Peripleo* supports GeoJSON and raster tile sources. Each layer configuration object __must__
have a `name` field, and a `type` field with a value of either `geojson` or `raster`. For example:

```json
{ 
  "name": "SW Rivers", 
  "type": "geojson",
  "src": "layers/SW_rivers.geojson", 
  "color": "#5555ff" 
}
```

```json
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
```

## Facets

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
## Initial View

By default, *Peripleo* will open a map with preset bounds (described [above](#configuring-your-map)), with plain markers and without any facet(s) selected. You can change this default behaviour by adding parameters to the URL following the model given below.

         https:// `your-url` /#/ `zoom` / `longitude` / `latitude` /mode= `points|clusters|heatmap` +facet= `type`

   For example:

         https://britishlibrary.github.io/peripleo-lanc/leifuss/#/8.16/-3.3969/50.6397/mode=points+facet=type
   
   If you wish to set only the visualisation mode or facet selection, replace `zoom` / `longitude` / `latitude` with `?/?/?`. You cannot set only `zoom`, `longitude`, or `latitude`: you must provide values either for all three parameters or for none.

   *Peripleo* updates the window URL automatically whenever you change the visualisation mode, facet selection, or move the map, so you can simply copy the URL to share a particular map view. Note, however, that the updated URL is not visible when *Peripleo* is loaded in an iframe.
