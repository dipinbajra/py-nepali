# nepali

[![PyPI version](https://badge.fury.io/py/nepali.svg)](https://badge.fury.io/py/nepali)
[![CI status](https://github.com/opensource-nepal/py-nepali/actions/workflows/python-package.yml/badge.svg?branch=master)](https://github.com/opensource-nepal/py-nepali/actions)
[![Downloads](https://img.shields.io/pypi/dm/nepali.svg?maxAge=180)](https://pypi.org/project/nepali/)
[![codecov](https://codecov.io/gh/opensource-nepal/py-nepali/branch/master/graph/badge.svg?token=PTUHYWCJ4I)](https://codecov.io/gh/opensource-nepal/py-nepali)

`nepali` is a python package containing features that will be useful for Nepali projects.

The major feature of this package is nepalidatetime, which is compatible with python's datetime feature. It helps nepali date to english, parsing nepali datetime, nepali timezone, and timedelta support in nepali datetime.

## Example

```python
import datetime
from nepali import phone_number
from nepali.datetime import nepalidate, parser

nepali_datetime = parser.parse('2079-02-15')
# 2079-02-15 00:00:00

# convert
date = datetime.date(2017, 3, 15)
nepali_date = nepalidate.from_date(date)
# 2073-12-02

phone_number.parse("+977-9845217789")
# {
# 	'type': 	'Mobile',
#	'number':	'9845217789',
#	'operator': <Operator: Nepal Telecom>
# }
```

## Requirements

    Python >= 3

## Installation

    pip install nepali

## Features

1. Nepali datetime
2. Phone number
3. Province / District / Municipality
4. Nepali Characters (Months, Days, etc)
5. Nepali/English numbers translation

## API Reference

### nepalidate

Represents nepali date, converts English date to nepali date and nepali date to english date

```python
from nepali.datetime import nepalidate
```

**Creating new object**

```python
# object with current date
np_date = nepalidate(year, month, day)

# object with today's date
np_date = nepalidate.today()

# parse date
np_date = nepalidate.strptime('2078-01-12', format='%Y-%m-%d')
```

**Object from python's `datetime.date`**

```python
import datetime

date = datetime.date.today()
np_date = nepalidate.from_date(date)
```

**Get python's `datetime` object**

```python
np_date.to_date() 		# datetime.date object
np_date.to_datetime()	# datetime.datetime object
```

### nepalidatetime

Represents nepali date time

```python
from nepali.datetime import nepalidatetime
```

**Creating new object**

```python
# object with specific datetime
np_datetime = nepalidatetime(year, month, day[, hour[, minute[, second]]]) # arguments must be nepali

# object with current datetime
np_datetime = nepalidatetime.now()

# parse datetime
np_datetime = nepalidatetime.strptime('2078-01-12 13:12', format='%Y-%m-%d %H:%M')
```

**Object from python's `datetime.datetime`**

```python
import datetime

dt = datetime.datetime.now()
np_datetime = nepalidatetime.from_datetime(dt)
```

**Get `nepalidate` and `time` and `datetime`**

```python
np_datetime.date()			# nepalidate object
np_datetime.time()			# nepalitime object
np_datetime.to_date()		# datetime.date object
np_datetime.to_datetime() 	# datetime.datetime object
```

**Date String Format**\
_Equivalent to python's datetime strftime format_

```python
npDateTime = nepalidatetime.now()
print(npDateTime.strftime('%a %A %w %d %b %B %m %y %Y %H %I %p %M %S'))
print(npDateTime.strftime_en('%a %A %w %d %b %B %m %y %Y %H %I %p %M %S'))
```

```
OUTPUT:
बिही बिहीबार ४ २३ असार असार ०३ ७९ २०७९ १२ १२ मध्यान्ह ४० १९
Thu Thursday 4 23 Ashad Ashad 03 79 2079 12 12 AM 40 19
```

**timedelta operations**

```python
ndt = nepalidatetime.now()
print(ndt + datetime.timedelta(hours=5))
print(ndt - datetime.timedelta(hours=5))
```

### parse

Parses datetime from a string.

_Note: `parse` method cost very high, so avoid this as much as you can. Use `strptime`_

```python
from nepali.datetime import parser as nepalidatetime_parser
ndt = nepalidatetime_parser.parse('29 Jestha, 2078, 1:30 PM')
```

### nepalihumanize

`nepalihumanize` converts `nepalidatetime` to nepali human readable form.

```python
from nepali.datetime import nepalihumanize
```

**Creating new object**

```python
# object from nepali datetime
ndt = nepalidatetime.now()
humanize_str = nepalihumanize(ndt)

# object from python datetime
dt = datetime.datetime.now()
humanize_str = nepalihumanize(dt)
```

**Humanize with threshold**\
returns date in nepali characters if more than threshold(in seconds) else returns humanize form

```python
humanize_str = nepalihumanize(ndt, threshold=60) # 60 seconds

# custom format after threshold
humanize_str = nepalihumanize(ndt, threshold=60, format='%Y-%m-%d') # 60 seconds
```

## For Django Template

Add `'nepali'` to your `INSTALLED_APPS` setting.

```python
INSTALLED_APPS = [
	...
	'nepali',
	...
]
```

IN your Template

```python
{% load nepalidatetime %}
```

```python
{% nepalinow %}
```

```python
{% nepalinow '%Y-%m-%d' %}
```

```python
{{ datetimeobj|nepalidate:"%Y-%m-%d" }}
{{ datetimeobj|nepalidate_en:"%Y-%m-%d" }}
```

```python
{{ datetimeobj|nepalihumanize }}
```
