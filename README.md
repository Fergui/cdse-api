# CDSE API
Query the [Copernicus Data Space Ecosystem (CDSE) OpenSearch service](https://documentation.dataspace.copernicus.eu/APIs/OpenSearch.html) for available
products. To download the data an [account on CDSE](https://dataspace.copernicus.eu/) is required.

This is just a proof of concept, not finished, thoroughly tested or fully developed. You are still welcome to use it and also to submit pull requests fixing bugs you may find.

## Usage

Query and download Sentinel-1 products for a given time range

```python
import geojson
from datetime import datetime

from creodias_finder import query

results = query.query(
    'Sentinel1',
    start_date=datetime(2019, 1, 1),
    end_date=datetime(2019, 1, 2)
)
```

Returns

```python
[
    ...,
    {'geometry': {'coordinates': [[[-66.400017, -65.643265],
                               [-58.727936, -63.775444],
                               [-56.687397, -65.090927],
                               [-64.635124, -67.052101],
                               [-66.400017, -65.643265]]],
              'type': 'Polygon'},
     'id': '639595ae-da84-5eac-a96d-aa5e323ca0e9',
     'properties': {'centroid': {'coordinates': [-61.543707, -65.4137725],
                                 'type': 'Point'},
                    'cloudCover': -1,
                    'collection': 'Sentinel1',
                    'completionDate': '2019-01-03T00:00:18.941Z',
                    'description': None,
                    'gmlgeometry': '<gml:Polygon '
                                   'srsName="EPSG:4326"><gml:outerBoundaryIs><gml:LinearRing><gml:coordinates>-66.400017,-65.643265 '
                                   '-58.727936,-63.775444 -56.687397,-65.090927 '
                                   '-64.635124,-67.052101 '
                                   '-66.400017,-65.643265</gml:coordinates></gml:LinearRing></gml:outerBoundaryIs></gml:Polygon>',
                    'instrument': 'SAR',
                    'keywords': [{'href': 'https://finder.creodias.eu/resto/api/collections/Sentinel1/search.json?&lang=en&q=Antarctica',
                                  'id': 'ab3b4ea14403e2e',
                                  'name': 'Antarctica',
                                  'normalized': 'antarctica',
                                  'type': 'continent'},
                                 {'gcover': 0.25,
                                  'href': 'https://finder.creodias.eu/resto/api/collections/Sentinel1/search.json?&lang=en&q=Antarctica',
    ...
]
```

Download selected products

```python

from creodias_finder import download

ids = [result['id'] for result in results.values()]

CREDENTIALS = {
    "username": 'my-cdse-email',
    "password": 'my-cdse-password'
}

# download single product by product ID
download.download(ids[0], outfile='/home/andreas/data/file.zip', **CREDENTIALS)

# download a list of products, multithreaded
download.download_list(ids[1:11], threads=10, **CREDENTIALS)
```
