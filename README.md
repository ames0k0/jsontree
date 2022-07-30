Script to generate a json `tree` for `key` and dumping the `value` of the `key` by `tree`
```json
{"a": 1, "b": 2, "d": {"c": 3}}		# 'root -> d -> c'
{"a": 1, "b": 2, "d": [{"c": 3}]}	# 'root -> d -> [0] -> c'
```

#### Example (API)
```python3
# WORKDIR=src

import os

from config import FD, Key
from utils import FileIO
from run import PageDataTree


i_filepath = os.path.join(
	FD.ROOT_DIRECTORY, 'ranker_writer-ignore_me.json'
)

file_io = FileIO(i_filepath)
file_data = file_io.load()

pdt = PageDataTree(file_data)
pdt_tree = pdt.tree_by_key(key='user', result_to=Key.SAVE)

user_data = pdt.data_by_tree(next(pdt_tree))

o_filepath = os.path.join(
	FD.ROOT_DIRECTORY, 'ranker_writer_user_content-ignore_me.json'
)

file_io.dump(user_data, o_filepath)
```

#### Example (CLI)
```bash
# Search for `key` (-k), filter by `key` (-fk)
python src/__main__.py -i <input_filepath> -k id -fk user

# Search for `key` (-k), filter by `key` (-fk), print the value for `tree`
python src/__main__.py -i <input_filepath> -k id -fk user -o '*'

# Print the value for `tree` (NOTE: `-o <output_filepath>` - to dump a value)
python src/__main__.py -i <input_filepath> -t 'root -> props -> ... -> user' -o '*'
```

#### Dependencies
```bash
pip -V		# 22.1.1
python -V	# 3.10.5
pytest -V	# 6.2.5
```

#### Start
```bash
# no need to if `requirements.txt` is empty
python -m venv env && source env/bin/activate
pip install -r requirements.txt

# check functions if necessary
pytest .

# run script
python src/run.py
```

#### Script structure
```
# tree -I __pycache__ -I env
.
├── src
│   ├── __init__.py
│   ├── __main__.py
│   ├── config.py
│   ├── utils.py
│   └── run.py
├── tests
│   ├── __init__.py
│   └── test_page_data_tree.py
├── data
│   ├── ranker_writer-ignore_me.json
│   └── ranker_writer_user_content-ignore_me.json
├── LICENSE
├── README.md
├── UPDATES.txt
├── CONTRIBUTORS.md
└── requirements.txt
```

#### TODO
- [x] nested json
- [x] json in the list
- [x] check for multiple keys
	- [x] return multiple keys (iterable result)
	- [ ] unique multiple keys (not every single item in the list)
- [ ] check for keys by value
- [x] access to the data in the list
	- [x] add and get the index from `tree`
- [ ] handle errors on searching for a non string key
- [x] fix errors on reading and writing to the json file without filename
	- [x] no need to test for writing
	- [x] raising an error `FileNotFoundError` for not valid input filepath

#### CLI
- [x] Input
	- [ ] json file (validates: with or without '.json' extension)
- [ ] Key
	- [x] search: `str`
	- [x] filter `tree` by must have `key` (-fk)
	- [x] filter `tree` by must have `value` (-fv)
	- [x] result (config.py[Key]): `print`, `return`
- [ ] Limit (only `key`)
	- [x] `print`
	- [x] `return`
- [ ] Output
	- [x] dump a `value` to the file (`-o <output_filepath>`)
	- [ ] append to the dump file
	- [x] `print`: '\*'

Coding process: https://youtu.be/DkBAIKMN7x0

#### License

GNU GPL-v3
<br />
Copyright (c) 2022 Nodiru Gaji (@ames0k0)
