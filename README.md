![build](https://github.com/nlbao/pocket_stats/workflows/build/badge.svg)
[![codecov](https://codecov.io/gh/nlbao/pocket_stats/branch/master/graph/badge.svg)](https://codecov.io/gh/nlbao/pocket_stats)

# Pocket Stats

A tool to analyze your Pocket reading list (https://app.getpocket.com/).

- [Pocket Stats](#pocket-stats)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
    - [Pip](#pip)
    - [Git Clone](#git-clone)
  - [Usage](#usage)
    - [Data Querying](#data-querying)
      - [Fetch data from the Pocket server and cache it](#fetch-data-from-the-pocket-server-and-cache-it)
      - [Load cached data](#load-cached-data)
      - [Extract useful information](#extract-useful-information)
    - [Visualization](#visualization)
      - [Word Cloud](#word-cloud)
      - [Article Count timeseries](#article-count-timeseries)
      - [Word Count distribution](#word-count-distribution)
      - [Reading Speed & Reading Time](#reading-speed--reading-time)
      - [Top Domains](#top-domains)
      - [Language & Favorite](#language--favorite)
  - [Test](#test)
  - [Deployment](#deployment)
  - [Contribute](#contribute)
  - [Authors](#authors)
  - [License](#license)
  - [Acknowledgments](#acknowledgments)


## Prerequisites
* A [Pocket](https://app.getpocket.com/) account.
* Pocket [consumer key](https://getpocket.com/developer/apps/new).
* Pocket access token. You can use [fxneumann's OneClickPocket tool](http://reader.fxneumann.de/plugins/oneclickpocket/auth.php) to automate this.
* Python 3.6+.


## Installation
### Pip
```bash
    pip install pocket-stats
```

### Git Clone
```bash
    git clone https://github.com/nlbao/pocket_stats.git
    cd pocket_stats
    make setup
```


## Usage
Set necessary environment variables:
```bash
    export POCKET_STATS_CONSUMER_KEY='<your_pocket_consumer_key>'
    export POCKET_STATS_ACCESS_TOKEN='<your_pocket_access_token>'
```

### Data Querying
#### Fetch data from the Pocket server and cache it
```python
    from pocket_stats.data import fetch_data

    # fetch the first 100 items, won't overwrite cache
    data = fetch_data(offset=0, limit=100, overwrite_cache=False)

    # fetch ALL items, will overwrite cache
    data = fetch_data(ovewrite_cache=True)
```
The default location of cache file is `~/pocket-tools.cache`, you can change it using the `POCKET_STATS_CACHE_FILE` environment variable.

#### Load cached data
```python
    from pocket_stats.data import load_cache
    data = load_cache()
```

#### Extract useful information
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

- Number of aritlces per language:
```python
    >>> get_language_counts(data)
    Counter({'en': 17, 'unknown': 3})
```

- Number of favorite articles and its percent:
```python
    >>> get_favorite_count(data)
    {'count': 2, 'percent': 0.1}
```


### Visualization
Start the application (webserver) by running the commands below:
```bash
    cd </project/repo/dir/>
    python pocket_stats/app.py

    # You will see something like this:
    # Dash is running on http://127.0.0.1:8050/
```
Then enter http://127.0.0.1:8050/ in your web browser. The site contains multiple components:

#### Word Cloud
![word_cloud](docs/images/word_cloud.png)

#### Article Count timeseries
![article_count](docs/images/article_count.png)

#### Word Count distribution
![word_count](docs/images/word_count.png)

#### Reading Speed & Reading Time
![reading_time](docs/images/reading_time.png)

#### Top Domains
![top_domains](docs/images/top_domains.png)

#### Language & Favorite
![language_favorite](docs/images/language_favorite.png)

## Test
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
