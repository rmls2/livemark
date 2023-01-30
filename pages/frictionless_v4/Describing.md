# Describing 

```python script
from frictionless import describe

package = describe("data/table.txt", type="package")
print(package.to_yaml())
```