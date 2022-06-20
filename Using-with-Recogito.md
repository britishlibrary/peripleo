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

You can also point Peripleo to the live JSON-LD endpoint of the Recogito document directly, using its absolute URL. (Make sure the document is set to __public visibility__ in Recogito!

```json
{
  "initial_bounds": [-25.98, 63.0, -12.6, 66.9],
  "data": [{ 
    "name": "Islandia, Abraham Ortelius", 
    "format": "RECOGITO_IMAGE", 
    "src": "https://recogito.pelagios.org/document/p9csuaqia9zz2e/downloads/annotations/jsonld" 
  }]
}
```
