# Using with Recogito

You can use Peripleo to display image annotations created with the [Recogito geo-annotation tool](https://recogito.pelagios.org/).

To do so, you need to download the annotations from Recogito in JSON-LD format, host the JSON-LD file at a public location (preferably where you host your copy of Peripleo), and set up your `peripleo.config.json` file accordingly. Example:

```json
{
  "initial_bounds": [-25.98, 63.0, -12.6, 66.9],
  "map_style": "map-style-OSM.json",
  "data": [{ 
    "name": "Islandia, Abraham Ortelius", 
    "format": "RECOGITO_IMAGE", 
    "src": "data/recogito-sample.jsonld" 
  }]
}
```

Please note that at the moment, it is strictly necessary to __download__ the data from Recogito. You cannot currently link to the download URL of a Recogito document directly. (This is because Recogito doesn't support [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) on download URLs.)

## To download the data from Recogito

To download the correct JSON-LD data file:
- go to the __Downloads__ tab of your document in Recogito
- in the __Annotations__ section, pick the __RDF__ > __JSON-LD__ download
