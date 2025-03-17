![Python >= 3.9](https://img.shields.io/badge/python->=3.9-red.svg) [![](https://badgen.net/github/release/deedy5/duckduckgo_search)](https://github.com/deedy5/duckduckgo_search/releases) [![](https://badge.fury.io/py/duckduckgo-search.svg)](https://pypi.org/project/duckduckgo-search)
# Duckduckgo_search<a name="TOP"></a>

AI chat and search for text, news, images and videos using the DuckDuckGo.com search engine.

## Table of Contents
* [Install](#install)
* [CLI version](#cli-version)
* [Duckduckgo search operators](#duckduckgo-search-operators)
* [Regions](#regions)
* [DDGS class](#ddgs-class)
* [Proxy](#proxy)
* [Exceptions](#exceptions)
* [1. chat() - AI chat](#1-chat---ai-chat)
* [2. text() - text search](#2-text---text-search-by-duckduckgocom)
* [3. images() - image search](#3-images---image-search-by-duckduckgocom)
* [4. videos() - video search](#4-videos---video-search-by-duckduckgocom)
* [5. news() - news search](#5-news---news-search-by-duckduckgocom)
* [Disclaimer](#disclaimer)

## Install
```python
pip install -U duckduckgo_search
```

## CLI version

```python3
ddgs --help
```
CLI examples:
```python3
# AI chat
ddgs chat
# text search
ddgs text -k "Assyrian siege of Jerusalem"
# find and download pdf files via proxy
ddgs text -k "Economics in one lesson filetype:pdf" -r wt-wt -m 50 -p https://1.2.3.4:1234 -d -dd economics_reading
# using Tor Browser as a proxy (`tb` is an alias for `socks5://127.0.0.1:9150`)
ddgs text -k "'The history of the Standard Oil Company' filetype:doc" -m 50 -d -p tb
# find and save to csv
ddgs text -k "'neuroscience exploring the brain' filetype:pdf" -m 70 -o neuroscience_list.csv
# don't verify SSL when making the request
ddgs text -k "Mississippi Burning" -v false
# find and download images
ddgs images -k "beware of false prophets" -r wt-wt -type photo -m 500 -d
# get news for the last day and save to json
ddgs news -k "sanctions" -m 100 -t d -o json
```
[Go To TOP](#TOP)

## Duckduckgo search operators

| Keywords example |	Result|
| ---     | ---   |
| cats dogs |	Results about cats or dogs |
| "cats and dogs" |	Results for exact term "cats and dogs". If no results are found, related results are shown. |
| cats -dogs |	Fewer dogs in results |
| cats +dogs |	More dogs in results |
| cats filetype:pdf |	PDFs about cats. Supported file types: pdf, doc(x), xls(x), ppt(x), html |
| dogs site:example.com  |	Pages about dogs from example.com |
| cats -site:example.com |	Pages about cats, excluding example.com |
| intitle:dogs |	Page title includes the word "dogs" |
| inurl:cats  |	Page url includes the word "cats" |

[Go To TOP](#TOP)

## Regions
<details>
  <summary>expand</summary>

    xa-ar for Arabia
    xa-en for Arabia (en)
    ar-es for Argentina
    au-en for Australia
    at-de for Austria
    be-fr for Belgium (fr)
    be-nl for Belgium (nl)
    br-pt for Brazil
    bg-bg for Bulgaria
    ca-en for Canada
    ca-fr for Canada (fr)
    ct-ca for Catalan
    cl-es for Chile
    cn-zh for China
    co-es for Colombia
    hr-hr for Croatia
    cz-cs for Czech Republic
    dk-da for Denmark
    ee-et for Estonia
    fi-fi for Finland
    fr-fr for France
    de-de for Germany
    gr-el for Greece
    hk-tzh for Hong Kong
    hu-hu for Hungary
    in-en for India
    id-id for Indonesia
    id-en for Indonesia (en)
    ie-en for Ireland
    il-he for Israel
    it-it for Italy
    jp-jp for Japan
    kr-kr for Korea
    lv-lv for Latvia
    lt-lt for Lithuania
    xl-es for Latin America
    my-ms for Malaysia
    my-en for Malaysia (en)
    mx-es for Mexico
    nl-nl for Netherlands
    nz-en for New Zealand
    no-no for Norway
    pe-es for Peru
    ph-en for Philippines
    ph-tl for Philippines (tl)
    pl-pl for Poland
    pt-pt for Portugal
    ro-ro for Romania
    ru-ru for Russia
    sg-en for Singapore
    sk-sk for Slovak Republic
    sl-sl for Slovenia
    za-en for South Africa
    es-es for Spain
    se-sv for Sweden
    ch-de for Switzerland (de)
    ch-fr for Switzerland (fr)
    ch-it for Switzerland (it)
    tw-tzh for Taiwan
    th-th for Thailand
    tr-tr for Turkey
    ua-uk for Ukraine
    uk-en for United Kingdom
    us-en for United States
    ue-es for United States (es)
    ve-es for Venezuela
    vn-vi for Vietnam
    wt-wt for No region
___
</details>

[Go To TOP](#TOP)


## DDGS class

The DDGS classes is used to retrieve search results from DuckDuckGo.com.
```python3
class DDGS:
    """DuckDuckgo_search class to get search results from duckduckgo.com

    Args:
        headers (dict, optional): Dictionary of headers for the HTTP client. Defaults to None.
        proxy (str, optional): proxy for the HTTP client, supports http/https/socks5 protocols.
            example: "http://user:pass@example.com:3128". Defaults to None.
        timeout (int, optional): Timeout value for the HTTP client. Defaults to 10.
        verify (bool): SSL verification when making the request. Defaults to True.
    """
```

Here is an example of initializing the DDGS class.
```python3
from duckduckgo_search import DDGS

results = DDGS().text("python programming", max_results=5)
print(results)
```

[Go To TOP](#TOP)

## Proxy

Package supports http/https/socks proxies. Example: `http://user:pass@example.com:3128`.
Use a rotating proxy. Otherwise, use a new proxy with each DDGS class initialization.

*1. The easiest way. Launch the Tor Browser*
```python3
ddgs = DDGS(proxy="tb", timeout=20)  # "tb" is an alias for "socks5://127.0.0.1:9150"
results = ddgs.text("something you need", max_results=50)
```
*2. Use any proxy server* (*example with [iproyal rotating residential proxies](https://iproyal.com?r=residential_proxies)*)
```python3
ddgs = DDGS(proxy="socks5h://user:password@geo.iproyal.com:32325", timeout=20)
results = ddgs.text("something you need", max_results=50)
```
*3. The proxy can also be set using the `DDGS_PROXY` environment variable.*
```python3
export DDGS_PROXY="socks5h://user:password@geo.iproyal.com:32325"
```

[Go To TOP](#TOP)

## Exceptions

```python
from duckduckgo_search.exceptions import (
    ConversationLimitException,
    DuckDuckGoSearchException,
    RatelimitException,
    TimeoutException,
)
```

Exceptions:
- `DuckDuckGoSearchException`: Base exception for duckduckgo_search errors.
- `RatelimitException`: Inherits from DuckDuckGoSearchException, raised for exceeding API request rate limits.
- `TimeoutException`: Inherits from DuckDuckGoSearchException, raised for API request timeouts.
- `ConversationLimitException`: Inherits from DuckDuckGoSearchException, raised for conversation limit during API requests to AI endpoint.

[Go To TOP](#TOP)

## 1. chat() - AI chat

```python
def chat(self, keywords: str, model: str = "gpt-4o-mini", timeout: int = 30) -> str:
    """Initiates a chat session with DuckDuckGo AI.

    Args:
        keywords (str): The initial message or question to send to the AI.
        model (str): The model to use: "gpt-4o-mini", "llama-3.3-70b", "claude-3-haiku",
            "o3-mini", "mistral-small-3". Defaults to "gpt-4o-mini".
        timeout (int): Timeout value for the HTTP client. Defaults to 30.

    Returns:
        str: The response from the AI.
    """
```
***Example***
```python
results = DDGS().chat("summarize Daniel Defoe's The Consolidator", model='claude-3-haiku')

# There is also `chat_yield` generator which yields chunks while a response is being processed:
for x in DDGS().chat_yield("How Do Airplanes Fly", model='llama-3.3-70b'):
    print(x)
```

[Go To TOP](#TOP)

## 2. text() - text search by duckduckgo.com

```python
def text(
    keywords: str,
    region: str = "wt-wt",
    safesearch: str = "moderate",
    timelimit: str | None = None,
    backend: str = "auto",
    max_results: int | None = None,
) -> list[dict[str, str]]:
    """DuckDuckGo text search generator. Query params: https://duckduckgo.com/params.

    Args:
        keywords: keywords for query.
        region: wt-wt, us-en, uk-en, ru-ru, etc. Defaults to "wt-wt".
        safesearch: on, moderate, off. Defaults to "moderate".
        timelimit: d, w, m, y. Defaults to None.
        backend: auto, html, lite. Defaults to auto.
            auto - try all backends in random order,
            html - collect data from https://html.duckduckgo.com,
            lite - collect data from https://lite.duckduckgo.com.
        max_results: max number of results. If None, returns results only from the first response. Defaults to None.

    Returns:
        List of dictionaries with search results.
    """
```
***Example***
```python
results = DDGS().text('live free or die', region='wt-wt', safesearch='off', timelimit='y', max_results=10)
# Searching for pdf files
results = DDGS().text('russia filetype:pdf', region='wt-wt', safesearch='off', timelimit='y', max_results=10)
print(results)
[
    {
        "title": "News, sport, celebrities and gossip | The Sun",
        "href": "https://www.thesun.co.uk/",
        "body": "Get the latest news, exclusives, sport, celebrities, showbiz, politics, business and lifestyle from The Sun",
    }, ...
]
```

[Go To TOP](#TOP)

## 3. images() - image search by duckduckgo.com

```python
def images(
    keywords: str,
    region: str = "wt-wt",
    safesearch: str = "moderate",
    timelimit: str | None = None,
    size: str | None = None,
    color: str | None = None,
    type_image: str | None = None,
    layout: str | None = None,
    license_image: str | None = None,
    max_results: int | None = None,
) -> list[dict[str, str]]:
    """DuckDuckGo images search. Query params: https://duckduckgo.com/params.

    Args:
        keywords: keywords for query.
        region: wt-wt, us-en, uk-en, ru-ru, etc. Defaults to "wt-wt".
        safesearch: on, moderate, off. Defaults to "moderate".
        timelimit: Day, Week, Month, Year. Defaults to None.
        size: Small, Medium, Large, Wallpaper. Defaults to None.
        color: color, Monochrome, Red, Orange, Yellow, Green, Blue,
            Purple, Pink, Brown, Black, Gray, Teal, White. Defaults to None.
        type_image: photo, clipart, gif, transparent, line.
            Defaults to None.
        layout: Square, Tall, Wide. Defaults to None.
        license_image: any (All Creative Commons), Public (PublicDomain),
            Share (Free to Share and Use), ShareCommercially (Free to Share and Use Commercially),
            Modify (Free to Modify, Share, and Use), ModifyCommercially (Free to Modify, Share, and
            Use Commercially). Defaults to None.
        max_results: max number of results. If None, returns results only from the first response. Defaults to None.

    Returns:
        List of dictionaries with images search results.
    """
```
***Example***
```python
results = DDGS().images(
    keywords="butterfly",
    region="wt-wt",
    safesearch="off",
    size=None,
    color="Monochrome",
    type_image=None,
    layout=None,
    license_image=None,
    max_results=100,
)
print(images)
[
    {
        "title": "File:The Sun by the Atmospheric Imaging Assembly of NASA's Solar ...",
        "image": "https://upload.wikimedia.org/wikipedia/commons/b/b4/The_Sun_by_the_Atmospheric_Imaging_Assembly_of_NASA's_Solar_Dynamics_Observatory_-_20100819.jpg",
        "thumbnail": "https://tse4.mm.bing.net/th?id=OIP.lNgpqGl16U0ft3rS8TdFcgEsEe&pid=Api",
        "url": "https://en.wikipedia.org/wiki/File:The_Sun_by_the_Atmospheric_Imaging_Assembly_of_NASA's_Solar_Dynamics_Observatory_-_20100819.jpg",
        "height": 3860,
        "width": 4044,
        "source": "Bing",
    }, ...
]
```

[Go To TOP](#TOP)

## 4. videos() - video search by duckduckgo.com

```python
def videos(
    keywords: str,
    region: str = "wt-wt",
    safesearch: str = "moderate",
    timelimit: str | None = None,
    resolution: str | None = None,
    duration: str | None = None,
    license_videos: str | None = None,
    max_results: int | None = None,
) -> list[dict[str, str]]:
    """DuckDuckGo videos search. Query params: https://duckduckgo.com/params.

    Args:
        keywords: keywords for query.
        region: wt-wt, us-en, uk-en, ru-ru, etc. Defaults to "wt-wt".
        safesearch: on, moderate, off. Defaults to "moderate".
        timelimit: d, w, m. Defaults to None.
        resolution: high, standart. Defaults to None.
        duration: short, medium, long. Defaults to None.
        license_videos: creativeCommon, youtube. Defaults to None.
        max_results: max number of results. If None, returns results only from the first response. Defaults to None.

    Returns:
        List of dictionaries with videos search results.
    """
```
***Example***
```python
results = DDGS().videos(
    keywords="cars",
    region="wt-wt",
    safesearch="off",
    timelimit="w",
    resolution="high",
    duration="medium",
    max_results=100,
)
print(results)
[
    {
        "content": "https://www.youtube.com/watch?v=6901-C73P3g",
        "description": "Watch the Best Scenes of popular Tamil Serial #Meena that airs on Sun TV. Watch all Sun TV serials immediately after the TV telecast on Sun NXT app. *Free for Indian Users only Download here: Android - http://bit.ly/SunNxtAdroid iOS: India - http://bit.ly/sunNXT Watch on the web - https://www.sunnxt.com/ Two close friends, Chidambaram ...",
        "duration": "8:22",
        "embed_html": '<iframe width="1280" height="720" src="https://www.youtube.com/embed/6901-C73P3g?autoplay=1" frameborder="0" allowfullscreen></iframe>',
        "embed_url": "https://www.youtube.com/embed/6901-C73P3g?autoplay=1",
        "image_token": "6c070b5f0e24e5972e360d02ddeb69856202f97718ea6c5d5710e4e472310fa3",
        "images": {
            "large": "https://tse4.mm.bing.net/th?id=OVF.JWBFKm1u%2fHd%2bz2e1GitsQw&pid=Api",
            "medium": "https://tse4.mm.bing.net/th?id=OVF.JWBFKm1u%2fHd%2bz2e1GitsQw&pid=Api",
            "motion": "",
            "small": "https://tse4.mm.bing.net/th?id=OVF.JWBFKm1u%2fHd%2bz2e1GitsQw&pid=Api",
        },
        "provider": "Bing",
        "published": "2024-07-03T05:30:03.0000000",
        "publisher": "YouTube",
        "statistics": {"viewCount": 29059},
        "title": "Meena - Best Scenes | 02 July 2024 | Tamil Serial | Sun TV",
        "uploader": "Sun TV",
    }, ...
]
```

[Go To TOP](#TOP)

## 5. news() - news search by duckduckgo.com

```python
def news(
    keywords: str,
    region: str = "wt-wt",
    safesearch: str = "moderate",
    timelimit: str | None = None,
    max_results: int | None = None,
) -> list[dict[str, str]]:
    """DuckDuckGo news search. Query params: https://duckduckgo.com/params.

    Args:
        keywords: keywords for query.
        region: wt-wt, us-en, uk-en, ru-ru, etc. Defaults to "wt-wt".
        safesearch: on, moderate, off. Defaults to "moderate".
        timelimit: d, w, m. Defaults to None.
        max_results: max number of results. If None, returns results only from the first response. Defaults to None.

    Returns:
        List of dictionaries with news search results.
    """
```
***Example***
```python
results = DDGS().news(keywords="sun", region="wt-wt", safesearch="off", timelimit="m", max_results=20)
print(results)
[
    {
        "date": "2024-07-03T16:25:22+00:00",
        "title": "Murdoch's Sun Endorses Starmer's Labour Day Before UK Vote",
        "body": "Rupert Murdoch's Sun newspaper endorsed Keir Starmer and his opposition Labour Party to win the UK general election, a dramatic move in the British media landscape that illustrates the country's shifting political sands.",
        "url": "https://www.msn.com/en-us/money/other/murdoch-s-sun-endorses-starmer-s-labour-day-before-uk-vote/ar-BB1plQwl",
        "image": "https://img-s-msn-com.akamaized.net/tenant/amp/entityid/BB1plZil.img?w=2000&h=1333&m=4&q=79",
        "source": "Bloomberg on MSN.com",
    }, ...
]
```

[Go To TOP](#TOP)

## Disclaimer

This library is not affiliated with DuckDuckGo and is for educational purposes only. It is not intended for commercial use or any purpose that violates DuckDuckGo's Terms of Service. By using this library, you acknowledge that you will not use it in a way that infringes on DuckDuckGo's terms. The official DuckDuckGo website can be found at https://duckduckgo.com.
