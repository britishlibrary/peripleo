# Using with Recogito

You can use Peripleo to display data from images annotated with the [Recogito geo-annotation tool](https://recogito.pelagios.org/).

To do so, you need to register the Recogito annotation dataset in JSON-LD format as a `dataset` in the config file. Because Recogito's format is a slightly proprietary flavour of Linked Traces, you need to specify `RECOGITO_IMAGE` as the data format.

```json
{
  "initial_bounds": [-25.98, 63.0, -12.6, 66.9],
  "data": [{ 
    "name": "Islandia, Abraham Ortelius", 
    "format": "RECOGITO_IMAGE", 
    "src": "data/recogito-sample.jsonld" 
  }]
}
```

Please note that at the moment, you need to __download__ the data from Recogito, and publish the static JSON-LD file alongside your Peripleo installation. It is not currently possible to link to the download URL of a Recogito document directly. (This is because Recogito doesn't currently support [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) for download URLs.)

## To download the data from Recogito

To download the correct JSON-LD data file:
- go to the __Downloads__ tab of your document in Recogito
- in the __Annotations__ section, pick the __RDF__ > __JSON-LD__ download
