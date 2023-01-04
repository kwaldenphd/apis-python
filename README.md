# Working With APIs in Python

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>This tutorial was written by Katherine Walden and is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

## Lab Goals

This lab provides an overview of web APIs and how to construct an API call in Python. It also covers how to work with the results of an API call in the Python programming environment, and write that data to a JSON file.

By the end of this lab, students will be able to:
- Describe the basic components and functionality of a web API
- Write a basic API call in Python
- Work with the results of an API call in Python
- Write the results of an API call to a JSON file

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?pid=da5b8c14-e546-44fd-8f65-af36014781ce">Lecture/live coding playlist</a></td>
  </tr>
  </table>

## Acknowledgements

Elements of this lab procedure are adapted from:

- Eric Matthes, Chapter 17 "Working With APIs" from [*Python Crash Course*](https://nostarch.com/pythoncrashcourse2e) (No Starch Press, 2019): 359-375
- Charles Severance, Chapter 13 "Using Web Services" from [*Python for Informatics*](https://www.py4e.com/book.php) (2009): 155-168
- Wes McKinney, Chapter 6 "Data Loading, Storage, and File Formats" from [*Python for Data Analysis*](https://www.oreilly.com/library/view/python-for-data/9781491957653/) (O'Reilly, 2018): 169-193
- Patrick Smyth, "Creating Web APIs with Python and Flask," *The Programming Historian* 7 (2018), https://doi.org/10.46430/phen0072.
- Ritvik Kharkar, ["Getting Census Data in 5 Easy Steps"](https://towardsdatascience.com/getting-census-data-in-5-easy-steps-a08eeb63995d) *Towards Data Science* (29 April 2019).

# Table of Contents
- [Lecture & Live Coding](#lecture--live-coding)
- [Lab notebook template](#lab-notebook-template)
- [What are APIs and how do they work](#what-are-apis-and-how-do-they-work)
  * [API terminology](#api-terminology)
- [What can data from an API look like](#what-can-data-from-an-api-look-like)
- [Making an API call in Python](#making-an-api-call-in-python)
- [Working with the API response in Python](#working-with-the-api-response-in-python)
- [Writing an API response to a JSON file](#writing-an-api-response-to-a-json-file)
- [Example: U.S. Census Bureau Data](#example-us-census-bureau-data)
- [Additional lab notebook questions](#additional-lab-notebook-questions)
- [Lab notebook questions](#lab-notebook-questions)

[Click here](https://colab.research.google.com/drive/1BMhsfL6kJ9DvEM2SWfKrBsibvIUNkv9Q) to access the lab procedure as a Jupyter Notebook (Google Colab, ND Users)

# Lecture & Live Coding

Throughout this lab, you will see a Panopto icon at the start of select sections.

This icon indicates there is lecture/live coding asynchronous content that accompanies this section of the lab. 

You can click the link in the figure caption to access these materials (ND users only).

Example:

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?pid=9f78b0f4-242c-4d6e-8ea2-ae300137b529">Lecture/live coding playlist</a></td>
  </tr>
  </table>

# Lab notebook template

[Click here](https://colab.research.google.com/drive/1YF9TZcaZIE61I3TgyMm8jf1_AzWmaHGv?usp=sharing) to access the lab notebook template as a Jupyter Notebook (Google Colab, ND Users)

# What are APIs and how do they work

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=25c97f96-ac6b-45d4-90f6-ae300138a023">What are web APIs & how do they work</a></td>
  </tr>
  </table>

<p align="center"><a href="https://github.com/kwaldenphd/apis-python/blob/main/Figure_1.jpg?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/apis-python/blob/main/Figure_1.jpg?raw=true" /></a></p>

For a brief introduction to APIs, view Danielle Thé, ["API's Explained (with LEGO)"](https://youtu.be/qW1qhb8r8xI), *YouTube Video* (1 November 2016).

There are many different kinds of Application Programming Interfaces (APIs). In general, an API is the part of a computer program designed to be used or manipulated by another computer program. We can contrast this with interacting with a computer program via the user interface, which most often is a type of graphical user interface (GUI).

We're focusing on web APIs, which allow for information or functionality to be manipulated by other programs via the internet. For example, with [Twitter’s web API](https://developer.twitter.com/en/docs/twitter-api/getting-started/about-twitter-api), you can write a Python program to perform tasks such as favoriting tweets or collecting tweet metadata.

Other large-scale data repositories tend to make data available via an API.
- The U.S. National Archives and Records Administration (NARA) makes the National Archives catalog, Executive Orders, and photographic image content available [via an API](https://www.archives.gov/developer#toc--datasets).
- The Smithsonian Institution allows open access to museum collection metadata [via an API](https://www.si.edu/openaccess/devtools).
- The U.S. Library of Congress provides access to digital collection metadata, digitized historical newspaper images, public radio and television programs, and World Digital Library collections [via APIs](https://labs.loc.gov/lc-for-robots/). 
- The U.S. Census Bureau makes a wide range of public data available [via API](https://www.census.gov/data/developers/data-sets.html).

APIs let us bring a live data connection into a programming environment and interact or work with it based on the format and protocols established as part of the API.

We can imagine a scenario in which you wanted to build an application or visualization based on the location of [Iowa Department of Transportation's nearly 901 snow plows](https://public-iowadot.opendata.arcgis.com/pages/winter). Downloading a static dataset isn't going to work for this specific use case--you need a live data feed with up-to-date information. This is the beauty of APIs.
- [Link to IDOT's plow truck data](https://public-iowadot.opendata.arcgis.com/datasets/IowaDOT::snow-plow-truck-location-avl-iowa-dot/about)

## API terminology

Some terminology that goes along with APIs.
- ***HTTP (HyperText Transfer Protocol)***: primary means of communicating or transferring data on the world wide web. 
- ***URL (Uniform Resource Locator)***: an address or location information for information on the web. A URL describes the location of a specific resource.
- ***JSON (JavaScript Object Notation)***: plain-text data storage format
- ***REST (REpresentational State Transfer)***: best practices for implementing APIs. APIs that follow these principles are called REST APIs.

# What can data from an API look like?

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=56b8d9a2-beea-4fc7-8fdc-ae30013904f7">API Data</a></td>
  </tr>
  </table>

Navigate to https://api.github.com/search/repositories?q=language:python&sort=stars in a web browser. We're looking at public projects hosted on GitHub that are written in the Python programming language.

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

<blockquote>Q1: Describe what you see in this sample JSON. What are some of the name-value pairs? What can you tell about how the data is structured? What do we think this data might look like when we bring it into Python? You ARE NOT required to have code as part of this answer.</blockquote>

For more detail & background on JSON data structures: [Elements of Computing I "Structured Data" lab](https://github.com/kwaldenphd/python-structured-data#javascript-object-notation)

# Making an API call in Python

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=302b1945-2956-42be-b24b-ae300138993a">Making an API call</a></td>
  </tr>
  </table>

First, we use the `requests` module to send HTTP requests (i.e. request data via the world wide web) using Python. The HTTP request returns a response object that includes whatever data is returned as part of the API call.
- [Requests: HTTP for Humans documentation](https://requests.readthedocs.io/en/master/)
- [Python documentation on `requests` module](https://pypi.org/project/requests/)
- [Python Requests Module, W3 Schools](https://www.w3schools.com/python/module_requests.asp)

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

Let's look at how we would call the GitHub API in Python.

```Python
# import requests module
import requests

# import json module
import json

# store API url
url = 'https://api.github.com/search/repositories?q=language:python&sort=stars'

# assign the headers- not always necessary, but something we have to do with the GitHub API
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

The first `print()` statement returns a status message for the `requests.get()` API call. Once we know the API call is working, we can go ahead and request the JSON data available via the API. Then we can store that JSON data as a Python dictionary.

<blockquote>Q2: Describe how a web API works in your own words. What are the core components and how do they work together or interact?</blockquote>

# Working With the API Response in Python

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=921f41ea-ad5f-40d3-90f3-ae3001389583">Working with the API response</a></td>
  </tr>
  </table>

If our API call has been successful, we now have JSON or XML (extensible markup language) data in Python.

- For more on JSON: 
  * [general info](https://www.w3schools.com/whatis/whatis_json.asp)
  * [in Python](https://www.w3schools.com/python/python_json.asp)
  * [Elements of Computing I "Structured Data" lab](https://github.com/kwaldenphd/python-structured-data#javascript-object-notation)
- For more on XML: 
  * [general info](https://www.w3schools.com/xml/xml_whatis.asp)
  * [in Python](https://realpython.com/python-xml-parser/)

Now we can start to explore the data returned by the API in Python. Let's say we wanted to know more about these Python repositories.

```Python
# print the total number of repositories
print(f"Total repositories: {response_dict['total_count']}")

# store list of dictionaries with information about GitHub repositories
repo_dicts = response_dict['items']

# print length of repo_dicts
print(f"Repositories returned: {len(repo_dicts)}")
```

```Python
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

Visit [GitHub's REST API documentation](https://docs.github.com/en/free-pro-team@latest/rest) to learn more about its API. The example covered in the lab procedure focuses on using the GitHub API to access search results. [Link to more info](https://docs.github.com/en/search-github/searching-on-github/searching-for-repositories#search-by-topic) on that specific topic.

# From API to JSON file

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=a95a51c8-0991-4012-a353-ae30013891fe">From API Response to JSON File</a></td>
  </tr>
  </table>

In many situations, we would stay within a programming environment when working with the data returned by an API call. But let's say you wanted to write the data returned by an API call to a JSON or XML file. We can write the JSON data loaded as a Python dictionary to a JSON file using `json.dump()`.

```Python
# select first dictionary for first repository using index
repo_dict = repo_dicts[0]

# create new JSON file and write repo_dict object to file
with open('output.json', 'w') as json_file:
  json.dump(repo_dict, json_file)
```

If you're feeling rusty or need a refresher on file methods in Python: [Elements of Computing I "Structured Data" lab](https://github.com/kwaldenphd/python-structured-data#file-io--access-modes)

<blockquote>Q3: Build code + comments that accomplish the following tasks:
<ol type="a">
<li>Modify the GitHub search result URL and write an API call from Python</li>
<ul><li>A few options here- you could <a href="https://docs.github.com/en/search-github/searching-on-github/searching-topics">search by topics</a> and change the parameters for creation date, number of results, etc.</li>
<li>Sample code for this task is included in the previous section of the lab.</li></ul>
<li>Select a single repository from the API return</li>
<ul><li>Sample code for this task is included in the previous section of the lab.</li></ul>
<li>Write the API return to a JSON file</li>
<ul><li>Sample code for this task is included in the previous section of the lab.</li></ul>
</ol>
</blockquote>

# Example: U.S. Census Bureau Data

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=980b9a8e-29a5-4f07-900d-ae3001388e8a">U.S. Census Bureau Data & Q4</a></td>
  </tr>
  </table>

<p align="center"><a href="https://github.com/kwaldenphd/apis-python/blob/main/Figure_2.png?raw=true"><img class="aligncenter" src="https://github.com/kwaldenphd/apis-python/blob/main/Figure_2.png?raw=true" /></a></p>

Let's say we want to work with the U.S. Census Bureau's API. First step is to navigate to the Census Bureau's ["About"](https://www.census.gov/data/developers/about.html) page for developers and click the "Request a Key" icon. Once you've completed the "Request a Key" form, you should receive an API key via email. This will look like a long string of letters and numbers.

The next step is to identify which Census Bureau dataset you want to work with. A wide range of datasets are available via the Census Bureau's API, ranging from the facets of data from the American Community Survey to data from the Decennial Census. Explore the list of options (["Available APIs"](https://www.census.gov/data/developers/data-sets.html)) and decide on a specific dataset.

For the purposes of this example, let's work with [five-year data from the American Community survey](https://www.census.gov/data/developers/data-sets/acs-5year.html) to find a recent population count by state.

The first thing we want to do is explore the documentation available for this dataset so we know what might (or should) return from the API call.
- [Link to documentation for 2019 subset of this dataset](https://www.census.gov/data/developers/data-sets/acs-5year.html)

As you can see from the documentation, there are MANY variables and data points contained in this dataset. For this example, let's stick to the original question of wanting a recent population count by state. From looking at the documentation, we know the URL for the API call will start with `https://api.census.gov/data/2017/acs/acs5?key=[API_KEY]`

And from looking at the [full list of variables](https://api.census.gov/data/2019/acs/acs5/variables.html), we can tell the variable name for "Total Population is `B01003_001E`. So the API call for the "Total Population" variable will look like `https://api.census.gov/data/2017/acs/acs5?key=[API_KEY]&get=B01003_001E`. This API call will return total population for all geographies in the ACS dataset.

We can modify the API call to only return "Total Population" for the all states in the dataset. The modified API call will look like `https://api.census.gov/data/2017/acs/acs5?key=[API_KEY]&get=B01003_001E&for=state:*`. You can paste this URL into your browser (with your API key) to check to make sure the API call is using the correct URL.
  * No brackets around the API key.

Check out the [Census Bureau's API documentation and sample queries](https://www.census.gov/data/developers/guidance/api-user-guide/query-examples.html) to learn more about how to customize your API call.

Now to bring this into Python.

```Python
# import requests module
import requests

# import json module
import json

# set census API key
apiKey = "YOUR_API_KEY"

# construct API URL
calledAPI = 'https://api.census.gov/data/2017/acs/acs5?key=' + apiKey + '&get=NAME,B01003_001E&for=state:*'

# call the API and collect the response
response = requests.get(calledAPI)

# load response into JSON object, removing first element which is field labels
formattedResponse = json.loads(response.text)[1:]
# Note: If you receive an error like '''JSONDecodeError: Expecting value: line 2 column 1 (char 1)''', 
# toggle between double quotes (" ") and single quotes (' ') in calledAPI

# write that JSON object to a file
with open('output.json', 'w') as json_file:
  json.dump(formattedResponse, json_file)
```

<blockquote>Q4: Describe your experience working with the Census Bureau API. What was rewarding? What was challenging? How did you solve those challenges? What do we think this data might look like when we bring it into Python, or what are some things we could do with this data in Python?</blockquote>

<blockquote>OPTIONAL: Modify the API call to access another subset of data. Include code + comments.</blockquote>

# Additional Lab Notebook Questions

<table>
 <tr><td>
<img src="https://elearn.southampton.ac.uk/wp-content/blogs.dir/sites/64/2021/04/PanPan.png" alt="Panopto logo" width="50"/></td>
<td><a href="https://notredame.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=387c80d5-2327-4371-877b-ae3001388b66">Additional Lab Notebook Questions</a></td>
  </tr>
  </table>

Q5A: Select a specific dataset available via an API.

A few sources that might get you started:
- [GitHub repository with extensive inventory of free public APIs](https://github.com/public-apis/public-apis)
- [U.S. National Archives and Records Administration, NARA](https://www.archives.gov/developer#toc--datasets)
- [Smithsonian Institution](https://www.si.edu/openaccess/devtools)
- [Library of Congress](https://labs.loc.gov/lc-for-robots/)
- [U.S. Census Bureau](https://www.census.gov/data/developers/data-sets.html)
- [The Sports DB](https://www.thesportsdb.com/api.php)

Q5B: View the API source in a browser. Describe what you are seeing. How can we start to make sense of this data using available documentation?

Q6: Construct an API call in Python. Include code + comments.

Q7: Take a subset of the data and write to a JSON file. Include code + comments.

OPTIONAL: JSON's nested structure can't always be mapped onto a tabular (table-like) data structure. But `pandas` does include a `pd.read_json` function if you want to experiment with creating a `DataFrame` from JSON data.
- [`pandas` documentation for `pd.read_json`](https://pandas.pydata.org/docs/reference/api/pandas.read_json.html)
- [alternate method that converts JSON to dictionary and uses `pd.DataFrame.from_dict()` method](https://www.kite.com/python/answers/how-to-convert-a-json-string-to-a-pandas-dataframe-in-python)

# Lab Notebook Questions

[Click here](https://colab.research.google.com/drive/1YF9TZcaZIE61I3TgyMm8jf1_AzWmaHGv?usp=sharing) to access the lab notebook template as a Jupyter Notebook (Google Colab, ND Users)

Q1: Describe what you see in this sample JSON. What are some of the name-value pairs? What can you tell about how the data is structured? What do we think this data might look like when we bring it into Python? You ARE NOT required to have code as part of this answer.

Q2: Describe how a web API works in your own words. What are the core components and how do they work together or interact?

Q3: Build code + comments that accomplish the following tasks:
<ol type="a">
<li>Modify the GitHub search result URL and write an API call from Python</li>
<ul><li>A few options here- you could <a href="https://docs.github.com/en/search-github/searching-on-github/searching-topics">search by topics</a> and change the parameters for creation date, number of results, etc.</li>
<li>Sample code for this task is included in the previous section of the lab.</li></ul>
<li>Select a single repository from the API return</li>
<ul><li>Sample code for this task is included in the previous section of the lab.</li></ul>
<li>Write the API return to a JSON file</li>
<ul><li>Sample code for this task is included in the previous section of the lab.</li></ul>
</ol>

Q4: Describe your experience working with the Census Bureau API. What was rewarding? What was challenging? How did you solve those challenges? What do we think this data might look like when we bring it into Python, or what are some things we could do with this data in Python?

OPTIONAL: Modify the API call to access another subset of data. Include code + comments.

Q5A: Select a specific dataset available via an API.

A few sources that might get you started:
- [GitHub repository with extensive inventory of free public APIs](https://github.com/public-apis/public-apis)
- [U.S. National Archives and Records Administration, NARA](https://www.archives.gov/developer#toc--datasets)
- [Smithsonian Institution](https://www.si.edu/openaccess/devtools)
- [Library of Congress](https://labs.loc.gov/lc-for-robots/)
- [U.S. Census Bureau](https://www.census.gov/data/developers/data-sets.html)
- [The Sports DB](https://www.thesportsdb.com/api.php)

Q5B: View the API source in a browser. Describe what you are seeing. How can we start to make sense of this data using available documentation?

Q6: Construct an API call in Python. Include code + comments.

Q7: Take a subset of the data and write to a JSON file. Include code + comments.

OPTIONAL: JSON's nested structure can't always be mapped onto a tabular (table-like) data structure. But `pandas` does include a `pd.read_json` function if you want to experiment with creating a `DataFrame` from JSON data.
- [`pandas` documentation for `pd.read_json`](https://pandas.pydata.org/docs/reference/api/pandas.read_json.html)
- [alternate method that converts JSON to dictionary and uses `pd.DataFrame.from_dict()` method](https://www.kite.com/python/answers/how-to-convert-a-json-string-to-a-pandas-dataframe-in-python)
