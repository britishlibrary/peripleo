# Who's Afraid of JSON?

* JSON ('Jason' with an invisible 'a', /*dʒeɪsən*/) is our friend.
* JSON's rather *un*-friendly full name is '[JavaScript Object Notation](https://en.wikipedia.org/wiki/JSON)'.
* JSON can transform a set of data stored in a spreadsheet into a format which can not only be read and displayed by most mapping software, 
but which can also store simple or even very complex links between datasets.

## JSON is not complicated.

Consider this trivial spreadsheet (a list of UK Historic Royal Palaces):

| Site                            |
| ---                             |
| Banqueting House Whitehall      |
| Hampton Court                   |
| Hillsborough Castle and Gardens |
| Kensington Palace               |
| Kew Palace                      |
| Tower of London                 |

JSON might store the same information like this:

``` json
[
  {"site":"Banqueting House Whitehall"},
  {"site":"Hampton Court"},
  {"site":"Hillsborough Castle and Gardens"},
  {"site":"Kensington Palace"},
  {"site":"Kew Palace"},
  {"site":"Tower of London"}
]
```
Note that in the above example:

  * the outer square brackets indicate that the information is a list (an *array*),
  * each item in the list is enclosed in { curly brackets } (an *object*),
  * items are separated by commas,
  * each item is composed of "properties" composed of a name and value, separated by a colon,
  * additional properties could be added within an item, separated by commas,
  * otherwise the layout is unimportant - you could compress the whole thing onto one line.

## This is where the 'fun' starts

We might now want to see the distribution of our dataset on a map, and so we have to introduce coordinate data 
(don't worry about how to find the coordinates, we can look at that [later](#locating-and-linking)). In a spreadsheet, we might do it like this:

| Site                            | Longitude    | Latitude 
| ---                             | ---          | ---
| Banqueting House Whitehall      | -0.126405135 | 51.50397086
| Hampton Court                   | -0.337691804 | 51.40374037
| Hillsborough Castle and Gardens | -6.093020031 | 54.46112567
| Kensington Palace               | -0.188374381 | 51.50494344
| Kew Palace                      | -0.291503292 | 51.48490788
| Tower of London                 | -0.07573787  | 51.50854694

There are many ways to store this information as JSON, but a standard known as "GeoJSON" has emerged which can be read by mapping software, and in which each item might look something like this:

``` json
{
	"type": "Feature",
	"properties": {
		"title": "Tower of London"
	},
	"geometry": {
		"type": "Point",
		"coordinates": [-0.07573787, 51.50854694]
	}
}
```
Take a moment to digest that.

  * Every item is now defined as a "Feature",
  * the name of each feature is now stored (as a `title`) within a sub-*object* called `properties`,
  * the coordinates of each feature are now stored within a sub-*object* called `geometry`,
  * in addition to coordinates, the `geometry` *object* is also defined as a "Point": GeoJSON can also store geographical information as shapes and lines.

# What about Linking?

Suppose we now want to sort or filter our information based on some kind of categorisation. Our initial spreadsheet might now look like this:

| Site                            | Type          | Longitude    | Latitude 
| ---                             | ---           | ---          | ---
| Banqueting House Whitehall      | Palace        | -0.126405135 | 51.50397086
| Hampton Court                   | Palace        | -0.337691804 | 51.40374037
| Hillsborough Castle and Gardens | House         | -6.093020031 | 54.46112567
| Kensington Palace               | Palace        | -0.188374381 | 51.50494344
| Kew Palace                      | Palace        | -0.291503292 | 51.48490788
| Tower of London                 | Castle/Palace | -0.07573787  | 51.50854694

Again, there are many ways in which this information might be stored as JSON, but another standard known as "Linked Places Format" (LPF) has emerged 
which extends the GeoJSON format and introduces the notion of Linked Data. 

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
		{ "label": "castle", "identifier": "https://www.wikidata.org/wiki/Q23413" },
		{ "label": "palace", "identifier": "https://www.wikidata.org/wiki/Q16560" }
	]
}
```

Here we have added a property, `types`, which in this case is an *array* of two sub-*objects*:

  * Each sub-*object* has a `label` and an `identifier`,
  * each `identifier` is a URL *linking* the label to a definition within some web-based vocabulary (for example: https://www.wikidata.org/wiki/Q23413).

The primary advantages of tying our classifications to web-based vocabularies are:

  * machines can now recognise the items in our original list for what they actually represent (rather than just words), and can index them accordingly,
  * mapping software can be developed to spot relationships between items in multiple lists.

We can take the Linked Data concept further, *much* further, by linking features to relevant web-based `descriptions`, `depictions`, and related `links`. LPF handles such links like this:

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
		{ "label": "castle", "identifier": "https://www.wikidata.org/wiki/Q23413" },
		{ "label": "palace", "identifier": "https://www.wikidata.org/wiki/Q16560" }
	],
	"descriptions": [
		{ "@id": "https://en.wikipedia.org/wiki/White_Tower_%28Tower_of_London%29", "value": "The White Tower is a central tower, the old keep, at the Tower of London. It was built by William the Conqueror during the early 1080s, and subsequently extended. The White Tower was the castle's strongest point militarily, and provided accommodation for the king and his representatives, as well as (...)" },
		{ "@id": "https://en.wikipedia.org/wiki/Traitors%27_Gate", "value": "The Traitor's Gate is an entrance through which many prisoners of the Tudors arrived at the Tower of London. The gate was built by Edward I, to provide a water gate entrance to the Tower, part of St. Thomas's Tower, which was designed to provide additional accommodation for the royal family (...)" }
	],
	"depictions": [
		{ "@id": "https://www.geograph.org.uk/photo/4142029", "title": "TQ3380 : Poppies in the Moat, Tower of London, E1" },
		{ "@id": "https://www.geograph.org.uk/photo/2476055", "title": "TQ3380 : The Tower of London" }
	],
	"links": [
		{ "type": "primaryTopicOf", "identifier": "https://historicengland.org.uk/listing/the-list/list-entry/1002061" },
		{ "type": "primaryTopicOf", "identifier": "https://historicengland.org.uk/listing/the-list/list-entry/1000092" }
	]
}
```

Can you imagine trying to store all of this information (and yet more links) in a spreadsheet? **This is why JSON is our friend**.

The *basic* information can still be interpreted by most mapping software, but in order to sort, filter, or display the additional categorisation information we need
something a bit more clever, such as *Peripleo*, originally conceived within the [Pelagios Network](https://pelagios.org/) and developed by the British Library's [*Locating a National Collection*](https://britishlibrary.github.io/locating-a-national-collection/) (LaNC) project. A detailed description of LPF and its capabilities can be found [here](https://github.com/LinkedPasts/linked-places-format), while proposed extensions to this format can be found [here](https://github.com/docuracy/Locolligo/blob/main/schemas/LP.json).

*Peripleo* can, for example, filter and display any feature categorised as a "castle" (Wikidata Q23413) regardless of the identity of its present-day custodian; features might have additional filterable `types` identifying custodianship, and temporal information identifying relevant dates or historical periods (although *Peripleo* requires further development before it will interpret temporal data).

# Locating and Linking

LaNC has also promoted the prototype development of another piece of software, [*Locolligo*](https://github.com/docuracy/Locolligo/blob/main/README.md), which will offer a browser-based method for:

 * transforming spreadsheet data into JSON,
 * finding coordinates for features,
 * linking features to web-based vocabularies and other resources.

---

<b><big>{ "objective":"Be an Argonaut", "method":{ "Embrace":"JSON" } }</big></b>
