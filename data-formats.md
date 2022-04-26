# Who's Afraid of JSON?

* JSON ('Jason' with an invisible 'a', /*dʒeɪsən*/) is our friend.
* JSON's rather *un*-friendly full name is 'JavaScript Object Notation'.
* JSON can transform a set of data stored in a spreadsheet into a format which can not only be read and displayed by most mapping software, 
but which can also store simple or even very complex links between datasets.

## JSON is not complicated.

Consider this trivial spreadsheet:

| Site                            | Type          |
| ---                             | ---           |
| Banqueting House Whitehall      | Palace        |  
| Hampton Court                   | Palace        | 
| Hilsborough Castle and Gardens  | House         |
| Kensington Palace               | Palace        |
| Kew Palace                      | Palace        |
| Tower of London                 | Castle/Palace |

JSON might store the same information like this:

``` json
[
  {"site":"Banqueting House Whitehall"},
  {"site":"Hampton Court"},
  {"site":"Hilsborough Castle and Gardens"},
  {"site":"Kensington Palace"},
  {"site":"Kew Palace"},
  {"site":"Tower of London"}
]
```
Note that in the above example:

  * the outer square brackets indicate that the information is a list (an *array*),
  * each item in the list is enclosed in { curly blackets } (an *object*),
  * items are separated by commas,
  * each item is composed of 'properties' composed of a name and value, separated by a colon,
  * additional properties could be added within an item, separated by commas.

## This is where the 'fun' starts

We might now want to see the distribution of our dataset on a map, and so we have to introduce coordinate data 
(don't worry about how to find the coordinates, we can look at that later). In a spreadsheet, we might do it like this:

| Site                            | Longitude    | Latitude 
| ---                             | ---          | ---
| Banqueting House Whitehall      | -0.126405135 | 51.50397086
| Hampton Court                   | -0.337691804 | 51.40374037
| Hilsborough Castle and Gardens  | -6.093020031 | 54.46112567
| Kensington Palace               | -0.188374381 | 51.50494344
| Kew Palace                      | -0.291503292 | 51.48490788
| Tower of London                 | -0.07573787  | 51.50854694

There are many ways to store this information as JSON, but a standard known as 'GeoJSON' has emerged which can be read by mapping software, and in which each item might look something like this:

``` json
{
	"type": "Feature",
	"properties": {
		"title": "Banqueting House Whitehall"
	},
	"geometry": {
		"type": "Point",
		"coordinates": [-0.126405135, 51.50397086]
	}
}
```
Take a moment to digest that.

  * Every item is now defined as a "Feature",
  * the name of each feature is now stored within a sub-*object* called 'properties',
  * the coordinates of each feature are now stored within a sub-*object* called 'geometry',
  * in addition to coordinates, the 'geometry' *object* is also defined as a "Point": GeoJSON can also store geographical information as shapes and lines.

# What about Linking?

Suppose we now want to sort or filter our information based on some kind of categorisation. Our initial spreadsheet might now look like this:

| Site                            | Type          | Longitude    | Latitude 
| ---                             | ---           | ---          | ---
| Banqueting House Whitehall      | Palace        | -0.126405135 | 51.50397086
| Hampton Court                   | Palace        | -0.337691804 | 51.40374037
| Hilsborough Castle and Gardens  | House         | -6.093020031 | 54.46112567
| Kensington Palace               | Palace        | -0.188374381 | 51.50494344
| Kew Palace                      | Palace        | -0.291503292 | 51.48490788
| Tower of London                 | Castle/Palace | -0.07573787  | 51.50854694

Again, there are many ways in which this information might be stored as JSON, but another standard known as 'Linked Places Format' (LPF) has emerged 
which extends the GeoJSON format. 

``` json
{
	"type": "Feature",
	"properties": {
		"title": "Tower of London"
	},
	"geometry": {
		"type": "Point",
		"coordinates": [-0.07573787, 51.50854694]
	},
  "types": [
    { "label":"castle", "identifier":"https://www.wikidata.org/wiki/Q23413" },
    { "label":"palace", "identifier":"https://www.wikidata.org/wiki/Q16560" }
  ]
}
```

The *basic* information can still be interpreted by most mapping software, but in order to sort, filter, or display the additional categorisation information we need
something a bit more clever, such as *Peripleo*.
