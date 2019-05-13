# using_odo


### update to new version of pandas:
In the venv/ find `lib/python3.5/site-packages/odo/backends/pandas.py`, in the line 94 replace:
```python
@convert.register((pd.Timestamp, pd.Timedelta), (pd.tslib.NaTType, type(None)))
```
with:
```python
@convert.register((pd.Timestamp, pd.Timedelta), (type(pd.NaT), type(None)))
```

install:
```
pip install odo[sqlite]
pip install networkx==1.11
```

generate db:
```python
from odo import odo,discover,resource
import pandas as pd 
import sqlite3
file_path = ''#'my_path/'
csv_path = file_path + 'data.csv'
db_name = 'db.sqlite'
conn = sqlite3.connect(db_name)
# Use Odo to detect the shape and datatype of your csv:
data_shape = discover(resource(csv_path))
# print (data_shape)#"var * {'0': ?string, '1': ?string, '2': ?string, '3': ?string, '4': ?string}"
# Ready in csv to new table called 'data' within database 'data.sqlite' with table named 'nyc'
odo(pd.read_csv(csv_path), 'sqlite:///db.sqlite::nyc', dshape=data_shape)#"var * {'0': ?string, '1': ?string, '2': ?string, '3': ?string, '4': ?string}")
# Close database
conn.close()
```
