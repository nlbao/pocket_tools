[![PyPI version](https://badge.fury.io/py/pocket-stats.svg)](https://badge.fury.io/py/pocket-stats)
![build](https://github.com/nlbao/pocket_stats/workflows/build/badge.svg)
[![codecov](https://codecov.io/gh/nlbao/pocket_stats/branch/master/graph/badge.svg)](https://codecov.io/gh/nlbao/pocket_stats)
[![Total alerts](https://img.shields.io/lgtm/alerts/g/nlbao/pocket_stats.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/nlbao/pocket_stats/alerts/)
[![Language grade: Python](https://img.shields.io/lgtm/grade/python/g/nlbao/pocket_stats.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/nlbao/pocket_stats/context:python)

# Pocket Stats

A tool to analyze your [Pocket](https://app.getpocket.com/) reading list.

Deployed at https://pocket-stats.nlbao.page .

- [Pocket Stats](#pocket-stats)
  - [Features](#features)
    - [Word Cloud](#word-cloud)
    - [Article Count timeseries](#article-count-timeseries)
    - [Word Count distribution](#word-count-distribution)
    - [Reading Speed & Reading Time](#reading-speed--reading-time)
    - [Top Domains](#top-domains)
    - [Language & Favorite](#language--favorite)
  - [Installation](#installation)
  - [Run web app locally](#run-web-app-locally)
  - [Data querying](#data-querying)
  - [Testing](#testing)
  - [Deployment](#deployment)
  - [Contribute](#contribute)
  - [Authors](#authors)
  - [License](#license)
  - [Acknowledgments](#acknowledgments)


## Features
### Word Cloud
![word_cloud](https://user-images.githubusercontent.com/4289177/87027187-cca22180-c1aa-11ea-89cb-ae1b91493934.png)

### Article Count timeseries
![article_count](https://user-images.githubusercontent.com/4289177/87027476-38848a00-c1ab-11ea-9115-925dc0660815.png)

### Word Count distribution
Stacked histograms
![word_count](https://user-images.githubusercontent.com/4289177/87027494-3de1d480-c1ab-11ea-9bfd-ded18cb4025b.png)

### Reading Speed & Reading Time
Stacked histograms
![reading_time](https://user-images.githubusercontent.com/4289177/87027500-3fab9800-c1ab-11ea-8a57-be4cb7eaf168.png)

### Top Domains
Stacked bar charts.
![top_domains](https://user-images.githubusercontent.com/4289177/87027511-420df200-c1ab-11ea-85c9-08f08f546caf.png)

### Language & Favorite
![language_favorite](https://user-images.githubusercontent.com/4289177/87027522-463a0f80-c1ab-11ea-8436-352adc72266b.png)


## Installation

Prerequisites:
* A [Pocket](https://app.getpocket.com/) account.
* Pocket [consumer key](https://getpocket.com/developer/apps/new).
* Pocket access token. You can use [fxneumann's OneClickPocket tool](http://reader.fxneumann.de/plugins/oneclickpocket/auth.php) to automate this.
* Python 3.6+.

You can install using Pip:
```bash
    pip3 install pocket-stats
```

Or clone this repo:
```bash
    git clone https://github.com/nlbao/pocket_stats.git
    cd pocket_stats
    make setup
```


## Run web app locally

```bash
    # Set necessary environment variables:
    export POCKET_STATS_CONSUMER_KEY='<your_pocket_consumer_key>'
    export POCKET_STATS_ACCESS_TOKEN='<your_pocket_access_token>'
       
    # Start the webserver
    gunicorn --workers 2 'pocket_stats.app:server' -b :8050 --reload
    
    # You will see something like this:
    # Dash is running on http://127.0.0.1:8050/
```
Go to http://127.0.0.1:8050/ from your web browser.


## Data querying

``` python
    from pocket_stats.data import <your_function_names>
```

- Count word in all the titles:
```python
    >>> count_words_in_title(data)
    Counter({'-': 5, '|': 5, 'python': 3, 'problem': 2, 'strace': 1, 'wow': 1, 'much': 1, 'syscall': 1, 'martin': 1, 'heinz': 1, 'personal': 1, 'website': 1, '&': 1, 'blog': 1, 'call': 1, 'programmer,': 1})
```

- Number of words in each article:
```python
    >>> get_word_counts(data)
    [2207, 0, 5449, 4721, 3245, 805, 1849, 4087, 0, 538, 5054, 21, 866, 266, 1146, 213, 823, 3551, 787, 0]
```

- Reading time of each article:
```python
    >>> get_reading_time(data)
    [9.80888888888889, 24.217777777777776, 20.982222222222223, 14.422222222222222, 3.577777777777778, 8.217777777777778, 18.164444444444445, 2.391111111111111, 22.462222222222223, 0.09333333333333334, 3.848888888888889, 1.1822222222222223, 5.093333333333334, 0.9466666666666667, 3.6577777777777776, 15.782222222222222, 3.497777777777778]
```

- Number of newly added articles per day:
```python
    >>> get_added_time_series(data) 
                All articles
    2020-07-04             5
    2020-07-03             8
    2020-07-02             2
    2020-07-01             5
```

- Number of newly archived articles per day:
```python
    >>> get_archived_time_series(data)
                Archived articles
    2020-07-04             2
```

- Number of articles per domain:
```python
    >>> get_domain_counts(data)
    Counter({'kalzumeus.com': 3, 'bogleheads.org': 2, 'github.io': 2, 'brendangregg.com': 1, 'martinheinz.dev': 1, 'awealthofcommonsense.com': 1, 'jlcollinsnh.com': 1, 'callan.com': 1, 'engineerseekingfire.com': 1, 'arxiv.org': 1, 'popularmechanics.com': 1, 'dolpages.com': 1, 'economist.com': 1, 'romantomjak.com': 1, 'digitalocean.com': 1, 'deepnote.com': 1})
```

- Number of articles per language:
```python
    >>> get_language_counts(data)
    Counter({'en': 17, 'unknown': 3})
```

- Number of favorite articles and its percent:
```python
    >>> get_favorite_count(data)
    {'count': 2, 'percent': 0.1}
```

## Testing
```bash
    make check
```

## Deployment
You can deploy the `app.py` as a webserver.

Example: https://dash.plotly.com/deployment.


## Contribute
Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Authors
- [Bao Nguyen](https://github.com/nlbao).
- [contributors](https://github.com/nlbao/pocket_stats/contributors) who participated in this project.


## License
MIT License - see the [LICENSE.md](LICENSE) file for details.


## Acknowledgments
- Pocket Python client: https://github.com/rakanalh/pocket-api
- fxneumann's OneClickPocket tool: http://reader.fxneumann.de/plugins/oneclickpocket/auth.php
