# Extracting Data

"voltei recife foi a saudade que me trouxe pelo braço"

```python script

from frictionless import Resource, extract
from pprint import pprint

resource = Resource('data/capital-3.csv')
resource.infer()
# as an example, in the next line we will append the schema
resource.schema.missing_values.append('3') # will interpret 3 as a missing value
resource.path = 'capital-3.csv'
resource.to_yaml('data/capital.resource-test.yaml') # use resource.to_json for JSON format

rows = extract('data/capital.resource-test.yaml')
pprint(rows)

```