# Lab #4: Working With APIs in Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.


## Lab Goals

## Acknowledgements

Elements of this lab procedure are adapted from:

- Eric Matthes, Chapter 17 "Working With APIs" from [*Python Crash Course*](https://nostarch.com/pythoncrashcourse2e) (No Starch Press, 2019): 359-375
- Charles Severance, Chapter 13 "Using Web Services" from [*Python for Informatics*](https://www.py4e.com/book.php) (2009): 155-168
- Wes McKinney, Chapter 6 "Data Loading, Storage, and File Formats" from [*Python for Data Analysis*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2018): 169-193
- Patrick Smyth, "Creating Web APIs with Python and Flask," *The Programming Historian* 7 (2018), https://doi.org/10.46430/phen0072.
- Ritvik Kharkar, ["Getting Census Data in 5 Easy Steps"](https://towardsdatascience.com/getting-census-data-in-5-easy-steps-a08eeb63995d) *Towards Data Science* (29 April 2019).

# Table of Contents

- [What are APIs and how do they work](#what-are-apis-and-how-do-they-work)
  * [API terminology](#api-terminology)
- [What can data from an API look like](#what-can-data-from-an-api-look-like)
- [Making an API call in Python](#making-an-api-call-in-python)
- [Working with the API response in Python](#working-with-the-api-response-in-python)
- [Writing an API response to a JSON file](#writing-an-api-response-to-a-json-file)
- [Example: U.S. Census Bureau Data](#example-us-census-bureau-data)
- [Project prompts](#project-prompts)
- [Lab notebook questions](#lab-notebook-questions)

# What are APIs and how do they work

Danielle Thé, ["API's Explained (with LEGO)"](https://youtu.be/qW1qhb8r8xI), *YouTube Video* (1 November 2016).

<p align="center"><a href="https://github.com/kwaldenphd/apis-python/blob/main/Figure_1.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/apis-python/blob/main/Figure_1.jpg?raw=true" /></a></p>

There are many different kinds of Application Programming Interfaces (APIs). In general, an API is the part of a computer program designed to be used or manipulated by another computer program. 

We can contrast this with interacting with a computer program via the user interface, which most often is a type of graphical user interface (GUI).

We're focusing on web APIs, which allow for information or functionality to be manipulated by other programs via the internet. 

For example, with Twitter’s web API, you can write a Python program to perform tasks such as favoriting tweets or collecting tweet metadata.

Other large-scale data repositories tend to make data available via an API.

For example, the U.S. National Archives and Records Administration (NARA) makes the National Archives catalog, Executive Orders, and photographic image content available [via an API](https://www.archives.gov/developer#toc--datasets).

The Smithsonian Institution allows open access to museum collection metadata [via an API](https://www.si.edu/openaccess/devtools).

The U.S. Library of Congress provides access to digital collection metadata, digitized historical newspaper images, public radio and television programs, and World Digital Library collections [via APIs](https://labs.loc.gov/lc-for-robots/). 

The U.S. Census Bureau makes as wide range of public data available [via API](https://www.census.gov/data/developers/data-sets.html).

APIs let us bring a live data connection into a programming environment and interact or work with it based on the format and protocols established as part of the API.

So we can imagine a scenario in which you wanted to build an application or visualization based on the location of [Iowa Department of Transportation's nearly 901 snow plows](https://iowadot.gov/tap.html).

Downloading a static dataset isn't going to work for this specific use case--you need a live data feed with up-to-date information.

Thus the beauty of APIs.
- [Link to IDOT's plow truck data](https://data.iowadot.gov/datasets/20a0c10c06a54240b5f2893e0187e22c_0?orderBy=OBJECTID&orderByAsc=false&page=6)

## API terminology

Some terminology that goes along with APIs.
- HTTP (HyperText Transfer Protocol): primary means of communicating or transferring data on the world wide web. 
- URL (Uniform Resource Locator): an address or location information for information on the web. A URL describes the location of a specific resource.
- JSON (JavaScript Object Notation): plain-text data storage format
- REST (REpresentational State Transfer): best practices for implementing APIs. APIs that follow these principles are called REST APIs.

# What can data from an API look like?

Navigate to https://api.github.com/search/repositories?q=language:python&sort=stars in a web browser.

We're looking at public projects hosted on GitHub that are written in the Python programming language.

Select the option to Expand All items, or click on the drop-down arrows to expand the data at this URL.
```JSON
{
  "total_count": 6343245,
  "incomplete_results": true,
  "items": [
    {
      "id": 123458551,
      "node_id": "MDEwOlJlcG9zaXRvcnkxMjM0NTg1NTE=",
      "name": "Python-100-Days",
      "full_name": "jackfrued/Python-100-Days",
      "private": false,
      "owner": {
        "login": "jackfrued",
        "id": 7474657,
        "node_id": "MDQ6VXNlcjc0NzQ2NTc=",
        "avatar_url": "https://avatars0.githubusercontent.com/u/7474657?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/jackfrued",
        "html_url": "https://github.com/jackfrued",
        "followers_url": "https://api.github.com/users/jackfrued/followers",
        "following_url": "https://api.github.com/users/jackfrued/following{/other_user}",
        "gists_url": "https://api.github.com/users/jackfrued/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/jackfrued/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/jackfrued/subscriptions",
        "organizations_url": "https://api.github.com/users/jackfrued/orgs",
        "repos_url": "https://api.github.com/users/jackfrued/repos",
        "events_url": "https://api.github.com/users/jackfrued/events{/privacy}",
        "received_events_url": "https://api.github.com/users/jackfrued/received_events",
        "type": "User",
        "site_admin": false
      },
      "html_url": "https://github.com/jackfrued/Python-100-Days",
      "description": "Python - 100天从新手到大师",
      "fork": false,
      "url": "https://api.github.com/repos/jackfrued/Python-100-Days",
      "forks_url": "https://api.github.com/repos/jackfrued/Python-100-Days/forks",
      "keys_url": "https://api.github.com/repos/jackfrued/Python-100-Days/keys{/key_id}",
      "collaborators_url": "https://api.github.com/repos/jackfrued/Python-100-Days/collaborators{/collaborator}",
      "teams_url": "https://api.github.com/repos/jackfrued/Python-100-Days/teams",
      "hooks_url": "https://api.github.com/repos/jackfrued/Python-100-Days/hooks",
      "issue_events_url": "https://api.github.com/repos/jackfrued/Python-100-Days/issues/events{/number}",
      "events_url": "https://api.github.com/repos/jackfrued/Python-100-Days/events",
      "assignees_url": "https://api.github.com/repos/jackfrued/Python-100-Days/assignees{/user}",
      "branches_url": "https://api.github.com/repos/jackfrued/Python-100-Days/branches{/branch}",
      "tags_url": "https://api.github.com/repos/jackfrued/Python-100-Days/tags",
      "blobs_url": "https://api.github.com/repos/jackfrued/Python-100-Days/git/blobs{/sha}",
      "git_tags_url": "https://api.github.com/repos/jackfrued/Python-100-Days/git/tags{/sha}",
      "git_refs_url": "https://api.github.com/repos/jackfrued/Python-100-Days/git/refs{/sha}",
      "trees_url": "https://api.github.com/repos/jackfrued/Python-100-Days/git/trees{/sha}",
      "statuses_url": "https://api.github.com/repos/jackfrued/Python-100-Days/statuses/{sha}",
      "languages_url": "https://api.github.com/repos/jackfrued/Python-100-Days/languages",
      "stargazers_url": "https://api.github.com/repos/jackfrued/Python-100-Days/stargazers",
      "contributors_url": "https://api.github.com/repos/jackfrued/Python-100-Days/contributors",
      "subscribers_url": "https://api.github.com/repos/jackfrued/Python-100-Days/subscribers",
      "subscription_url": "https://api.github.com/repos/jackfrued/Python-100-Days/subscription",
      "commits_url": "https://api.github.com/repos/jackfrued/Python-100-Days/commits{/sha}",
      "git_commits_url": "https://api.github.com/repos/jackfrued/Python-100-Days/git/commits{/sha}",
      "comments_url": "https://api.github.com/repos/jackfrued/Python-100-Days/comments{/number}",
      "issue_comment_url": "https://api.github.com/repos/jackfrued/Python-100-Days/issues/comments{/number}",
      "contents_url": "https://api.github.com/repos/jackfrued/Python-100-Days/contents/{+path}",
      "compare_url": "https://api.github.com/repos/jackfrued/Python-100-Days/compare/{base}...{head}",
      "merges_url": "https://api.github.com/repos/jackfrued/Python-100-Days/merges",
      "archive_url": "https://api.github.com/repos/jackfrued/Python-100-Days/{archive_format}{/ref}",
      "downloads_url": "https://api.github.com/repos/jackfrued/Python-100-Days/downloads",
      "issues_url": "https://api.github.com/repos/jackfrued/Python-100-Days/issues{/number}",
      "pulls_url": "https://api.github.com/repos/jackfrued/Python-100-Days/pulls{/number}",
      "milestones_url": "https://api.github.com/repos/jackfrued/Python-100-Days/milestones{/number}",
      "notifications_url": "https://api.github.com/repos/jackfrued/Python-100-Days/notifications{?since,all,participating}",
      "labels_url": "https://api.github.com/repos/jackfrued/Python-100-Days/labels{/name}",
      "releases_url": "https://api.github.com/repos/jackfrued/Python-100-Days/releases{/id}",
      "deployments_url": "https://api.github.com/repos/jackfrued/Python-100-Days/deployments",
      "created_at": "2018-03-01T16:05:52Z",
      "updated_at": "2020-12-16T16:09:01Z",
      "pushed_at": "2020-12-16T03:01:52Z",
      "git_url": "git://github.com/jackfrued/Python-100-Days.git",
      "ssh_url": "git@github.com:jackfrued/Python-100-Days.git",
      "clone_url": "https://github.com/jackfrued/Python-100-Days.git",
      "svn_url": "https://github.com/jackfrued/Python-100-Days",
      "homepage": "",
      "size": 272084,
      "stargazers_count": 97101,
      "watchers_count": 97101,
      "language": "Python",
      "has_issues": true,
      "has_projects": true,
      "has_downloads": true,
      "has_wiki": true,
      "has_pages": false,
      "forks_count": 38952,
      "mirror_url": null,
      "archived": false,
      "disabled": false,
      "open_issues_count": 513,
      "license": null,
      "forks": 38952,
      "open_issues": 513,
      "watchers": 97101,
      "default_branch": "master",
      "score": 1.0
    }
 ```

<blockquote>QX: What are we seeing in the browser or in this sample JSON? Some of the specific fields? What do we think this might look like when we bring it into Python, aka connect with JSON</blockquote>

# Making an API call in Python

First we use the `requests` module to send HTTP requests (i.e. request data via the world wide web) using Python.
- [Requests: HTTP for Humans documentation](https://requests.readthedocs.io/en/master/)
- [Python documentation on `requests` module](https://pypi.org/project/requests/)
- [Python Requests Module, W3 Schools](https://www.w3schools.com/python/module_requests.asp)

The HTTP request returns a response object that includes whatever data is returned as part of the API call.

The `requests` module includes a range of methods that let us interact with elements of the API.

Method | Description
--- | ---
`delete(url, args)` | Sends as DELETE request to the specified URL
`get(url, params, args)` | Sends a GET request to the specified URL
`head(url, args)` | Sends a HEAD request to the specified URL
`patch(url, data, args)` | Sends a PATCH request to the specified URL
`post(url, data, json, args)` | Sends a POST request to the specified URL
`put(url, data, args)` | Sends a PUT request to the specified URL
`request(method, url, args)` | Sends a request of the specified method to the specified URL

A very basic example of the `requests` module in action.
```Python
# import requests module
import requests

# store URL
x = requests.get('url')

# print URL  text
print(x.text)
```

This basic syntax should look familiar for those who took Elements I in the Fall 2020 semester.
```Python

import urllib
import urllib.request

url = 'https://en.wikisource.org/wiki/John_F._Kennedy%27s_Third_State_of_the_Union_Address'

response = urllib.request.urlopen(url)
webContent = response.read()

f = open('Kennedy_Third_SOTU.html', 'wb')
f.write(webContent)
f.close
```

Let's look at how we would call the GitHub API in Python.
```Python
# import requests module
import requests

# store API url
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'

# assign the headers
headers = {'Accept': 'application/vnd.github.v3+json'}

# assign the requests method
r = requests.get(url, headers=headers)

# print a status update for the requests command
print(f"Status code: {r.status_code}")

# store API response to variable
response_dict = r.json()

# process results
print(response_dict.keys())
```

The first `print()` statement returns a status message for the `requests.get()` API call.

Once we know the API call is working, we can go ahead and request the JSON data available via the API.

Then we can store that JSON data as a Python dictionary.

# Working With the API Response in Python

If our API call has been successful, we now have JSON or XML data in Python.

Now we can start to explore the data returned by the API in Python.

<blockquote>We can call this type of preliminary exploration and analysis EDA, or exploratory data analysis.</blockquote>

Let's say we wanted to know more about these Python repositories.
```Python
# import requests module
import requests

# store API url
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'

# assign the headers
headers = {'Accept': 'application/vnd.github.v3+json'}

# assign the requests method
r = requests.get(url, headers=headers)

# print a status update for the requests command
print(f"Status code: {r.status_code}")

# store API response to variable
response_dict = r.json()

# print the total number of repositories
print(f"Total repositories: {response_dict['total_count']}")

# store list of dictionaries with information about GitHub repositories
repo_dicts = response_dict['items']

# print length of repo_dicts
print(f"Repositories returned: {len(repo_dicts)}")

# select first repository using index
repo_dict = repo_dicts[0]

# print number of keys in individual repository dictionary
print(f"\nKeys: {len(repo_dict)}")

# print all keys in the dictionary using a for loop to iterate over keys in the dictionary
for key in sorted(repo_dict.keys()):
  print(key)
```

Now that we know what keys are in this dictionary, we can pull out the values for some of these keys.
```Python
# import requests module
import requests

# store API url
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'

# assign the headers
headers = {'Accept': 'application/vnd.github.v3+json'}

# assign the requests method
r = requests.get(url, headers=headers)

# print a status update for the requests command
print(f"Status code: {r.status_code}")

# store API response to variable
response_dict = r.json()

# print the total number of repositories
print(f"Total repositories: {response_dict['total_count']}")

# store list of dictionaries with information about GitHub repositories
repo_dicts = response_dict['items']

# print length of repo_dicts
print(f"Repositories returned: {len(repo_dicts)}")

# select first dictionary for first repository using index
repo_dict = repo_dicts[0]

# print header information before key values return
print("\nSelected information about first repository:")

# print repo name
print(f"Name: {repo_dict['name']}")

# print repository owner information
print(f"Owner: {repo_dict['owner']['login']}")

# print repository number of stars
print(f"Stars: {repo_dict['stargazers_count']}")

# print project URL
print(f"Repository URL: {repo_dict['html_url']}")

# print repository creation date
print(f"Created: {repo_dict['created_at']}")

# print repository last update time
print(f"Updated: {repo_dict['updated_at']}")

# print repository description
print(f"Description: {repo_dict['description']}")
```

Visit [GitHub's REST API documentation](https://docs.github.com/en/free-pro-team@latest/rest) to learn more about its API.

# From API to JSON file

In many situations, we would stay within a programming environment when working with the data returned by an API call.

But let's say you wanted to write the data returned by an API call to a JSON or XML file.

We can write the JSON data loaded as a Python dictionary to a JSON file using `json.dump()`.
```Python
# import json module
import json

# import request module
import request

# store API url
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'

# assign the headers
headers = {'Accept': 'application/vnd.github.v3+json'}

# assign the requests method
r = requests.get(url, headers=headers)

# print a status update for the requests command
print(f"Status code: {r.status_code}")

# store API response to variable
response_dict = r.json()

# print the total number of repositories
print(f"Total repositories: {response_dict['total_count']}")

# store list of dictionaries with information about GitHub repositories
repo_dicts = response_dict['items']

# print length of repo_dicts
print(f"Repositories returned: {len(repo_dicts)}")

# select first dictionary for first repository using index
repo_dict = repo_dicts[0]

# create new JSON file and write repo_dict object to file
with open('output.json', 'w') as json_file:
  json.dump(repo_dict, json_file)
```

# Example: U.S. Census Bureau Data

Let's say we want to work with the U.S. Census Bureau's API.

First step is to navigate to the Census Bureau's ["About"](https://www.census.gov/data/developers/about.html) page for developers and click the "Request a Key" icon.

Once you've completed the "Request a Key" form, you should receive an API key via email. This will look like a long string of letters and numbers.

Next step is to identify which Census Bureau dataset you want to work with.

Lots of options, ranging from the facets of data from the American Community Survey to data from the Decennial Census.

Explore the list of options (["Available APIs"](https://www.census.gov/data/developers/data-sets.html)) and decide on a specific dataset.

For the purposes of this example, I'm going to work with [five-year data from the American Community survey](https://www.census.gov/data/developers/data-sets/acs-5year.html) to find a recent population count for `46556`, Notre Dame's zip code.

First thing I want to do is explore the documentation available for this dataset so I know what might (or should) return from the API call.
- [Link to documentation for 2019 subset of this dataset](https://www.census.gov/data/developers/data-sets/acs-5year.html)

As you can see from the documentation, there are MANY variables and data points contained in this dataset. 

For this example, I'm going to stick to the original question of wanting a recent population count for Notre Dame's zip code.

From looking at the documentation, I know my API call will start with `https://api.census.gov/data/2017/acs/acs5?key=[YOUR_API_KEY]`

And from looking at the full list of variables, I know my variable name for "Total Population is `B01003_001E`.

So my API call for the "Total Population" variable will look like `https://api.census.gov/data/2017/acs/acs5?key=[YOUR_API_KEY]&get=B01003_001E`.

Execept this API call will return total population for all geographies in the ACS dataset, which is much more than I actually want.

I can modify the API call to only return "Total Population" for the zip code I am interested in.

The modified API call will look like `https://api.census.gov/data/2017/acs/acs5?key=[YOUR_API_KEY]&get=B01003_001E&for=zip%20code%20tabulation%20area:46556`.

You can paste this URL into your browser to check to make sure the API call is using the correct URL.

Check out the [Census Bureau's API documentation and sample queries](https://www.census.gov/data/developers/guidance/api-user-guide/query-examples.html) to learn more about how to customize your API call.

Now to bring this into Python.
```Python
# import requests module
import requests

# import json module
import json

# set census API key
apiKey = "YOUR_API_KEY"

# construct API
calledAPI = "https://api.census.gov/data/2017/acs/acs5?key=[YOUR_API_KEY]&get=B01003_001E&for=zip%20code%20tabulation%20area:46556"

# call the API and collect the response
response = requests.get(calledAPI)

# load response into JSON object, removing first element which is field labels
formattedResponse = json.loads(response.text)[1:]

# write that JSON object to a file
with open('output.json', 'w') as json_file:
  json.dump(formattedResponse, json_file)
```

# Project Prompts

Navigate to an open data portal and construct an API call.

For example, the U.S. National Archives and Records Administration (NARA) makes the National Archives catalog, Executive Orders, and photographic image content available [via an API](https://www.archives.gov/developer#toc--datasets).

The Smithsonian Institution allows open access to museum collection metadata [via an API](https://www.si.edu/openaccess/devtools).

The U.S. Library of Congress provides access to digital collection metadata, digitized historical newspaper images, public radio and television programs, and World Digital Library collections [via APIs](https://labs.loc.gov/lc-for-robots/). 

The U.S. Census Bureau makes as wide range of public data available [via API](https://www.census.gov/data/developers/data-sets.html).

SPORT DATA https://www.thesportsdb.com/api.php

View the API source in a browser. What are you seeing, how can we start to make sense of it What documentation is available, etc.

Construct an API call in Python

Interact with the data in Python to identify/pull out relevant key-value pairs

Take a subset of the data and write to a JSON file.

# Lab Notebook Questions
