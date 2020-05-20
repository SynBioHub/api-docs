---
title: SynBioHub API

language_tabs: # must be one of https://git.io/vQNgJ
  - plaintext: shell
  - python
  - javascript


toc_footers:
  - <a href='https://github.com/SynBioHub/synbiohub'>SynBioHub Github</a>
  - <a href='https://groups.google.com/forum/#!forum/synbiohub-users'>Join the SynBioHub Users mailing list</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# About SynBioHub
### What is SynBioHub?

SynBioHub includes two projects:

* An [open source software project](https://github.com/SynBioHub/synbiohub) providing a web interface for the storing and publishing of synthetic biology designs.
* A public instance of the aforementioned software project at [synbiohub.org](http://synbiohub.org), allowing users to upload and share designs.


### What can SynBioHub be used for?

SynBioHub can be used to publish a library of synthetic parts and designs as a service, to share designs with collaborators, and to store designs of biological systems locally. Data in SynBioHub can be accessed via the HTTP API, Java API, or Python API where it can then be integrated into CAD tools for building genetic designs. SynBioHub contains an interface for users to upload new biological data to the database, to visualize DNA parts, to perform queries to access desired parts, and to download SBOL, GenBank, FASTA, etc.

### Publications

* *McLaughlin, James Alastair, et al. ["SynBioHub: a standards-enabled design repository for synthetic biology."]((https://pubs.acs.org/doi/abs/10.1021/acssynbio.7b00403)) ACS synthetic biology 7.2 (2018): 682-688.*
* *Mante, Jeanet, Zach Zundel, and Chris J. Myers. ["Extending SynBioHub's Functionality with Plugins."]((https://pubs.acs.org/doi/abs/10.1021/acssynbio.0c00056)) ACS Synthetic Biology (2020).*
* *Zhang, Michael, Zach Zundel, and Chris J. Myers. ["SBOLExplorer: Data Infrastructure and Data Mining for Genetic Design Repositories."]((https://pubs.acs.org/doi/abs/10.1021/acssynbio.9b00089)) ACS synthetic biology 8.10 (2019): 2287-2294.*

### Contributors

* [Dr. James Alastair McLaughlin](http://homepages.cs.ncl.ac.uk/j.a.mclaughlin) (Newcastle University)
* [Prof. Chris J. Myers](http://www.async.ece.utah.edu/Myers) (University of Utah)
* [Dr. Goksel Misirli](http://homepages.cs.ncl.ac.uk/goksel.misirli) (Newcastle University)
* [Prof. Anil Wipat](http://homepages.cs.ncl.ac.uk/anil.wipat/) (Newcastle University)
* [Zach Zundel](http://www.async.ece.utah.edu/people/students/zach-zundel/) (University of Utah)
* [James Scholz](https://www.async.ece.utah.edu/~scholz/) (University of Utah)

An earlier version of SynBioHub known as SBOL Stack was developed by the following people:

* [Dr. Curtis Madsen](http://sites.bu.edu/ckmadsen/) (Boston University)
* [James Alastair McLaughlin](http://homepages.cs.ncl.ac.uk/j.a.mclaughlin) (Newcastle University) 
* [Dr. Goksel Misirli](http://homepages.cs.ncl.ac.uk/goksel.misirli/) (Newcastle University)
* [Dr. Matthew Pocock](http://intbio.ncl.ac.uk/?people=matthew-pocock) (Turing Ate My Hamster Ltd.)
* [Dr. Keith Flanagan](http://intbio.ncl.ac.uk/?people=dr-keith-flanagan) (Newcastle University) 
* [Dr. Jennifer Hallinan](https://research.science.mq.edu.au/synthetic-biology/people/) (Macquarie University)
* [Prof. Anil Wipat](http://homepages.cs.ncl.ac.uk/anil.wipat/) (Newcastle University) 

Web Design

* [Antarctic Design](http://www.antarctic-design.co.uk/) 

# Installation

## From Prebuilt Image
### Install Docker

First, install [Docker](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/).

Ensure that the Docker daemon is running on your system before continuing. On Mac and Windows, a small whale icon in the system tray indicates Docker is running.

On Ubuntu, start docker using: 

```systemctl start docker```

### Starting a SynBioHub Instance

First, pull the docker-compose configuration:: 

```git clone https://github.com/synbiohub/synbiohub-docker```

Then, start SynBioHub with: 

```docker-compose --file ./synbiohub-docker/docker-compose.yml up```

 If you would like to start SynBioHub with [SBOLExplorer](https://github.com/SynBioDex/SBOLExplorer) use the following commands instead: 


```sysctl -w vm.max_map_count=262144```

```docker-compose --file ./synbiohub-docker/docker-compose.yml --file ./synbiohub-docker/docker-compose.explorer.yml up``

### Configuring

In a web browser, visit 

``` http://localhost:7777/ ```

On the first startup, you will be taken to the SynBioHub Setup Page, which enables basic setup of the site. After the first setup, the Admin Portal will allow admin users to update their site configuration. 

### SendGrid email setup
In order to enable SynBioHub to send account-related emails, you need a [SendGrid](https://sendgrid.com/) account and API key. Once you have created your account, you should click "Settings" in the left bar, then "API Keys". On the resulting page, click the "Create API Key" button in the upper-right corner, and give your new API key a name. You should see the key on the next page. Copy the key and paste it into the "SendGrid API Key" in the Mail page on the SynBioHub admin dashboard. Save the API key in SynBioHub and you are ready to begin sending email. 

### Updating

To update a container, pull the new version and start it. 

```cd synbiohub-docker```

``` git pull ```

``` cd .. ```

``` ## if you are not running SBOL Explorer ```

``` docker-compose --file synbiohub-docker/docker-compose.yml pull synbiohub ```

```docker-compose --file synbiohub-docker/docker-compose.yml up ```

``` ## If you are running SBOL Explorer: ```

``` docker-compose --file synbiohub-docker/docker-compose.yml --file synbiohub-docker/docker-compose.explorer.yml pull synbiohub ```

``` docker-compose --file synbiohub-docker/docker-compose.yml --file synbiohub-docker/docker-compose.explorer.yml up ```

###### PERSISTENT LOGS UPDATE

If you are updating from a version earlier than 1.5.2, then you must execute the following command to setup persistent log files: 

```docker exec -it synbiohub-docker_synbiohub_1 mkdir /mnt/data/logs ```

#### If you are updating from a version of SynBioHub earlier than 1.3.0 to 1.3.0 or later, you must execute the following steps to persist your data between instances! ####

``` docker exec synbiohub cp /opt/synbiohub/synbiohub.sqlite /mnt/data/synbiohub.sqlite ```

``` docker exec synbiohub cp -R /opt/synbiohub/uploads /mnt/data/uploads ```

``` docker exec synbiohub chown synbiohub /mnt/data/synbiohub.sqlite ```

``` docker exec synbiohub chgrp synbiohub /mnt/data/synbiohub.sqlite ```

``` docker exec synbiohub chown -R synbiohub /mnt/data/uploads ```

``` docker exec synbiohub chgrp -R synbiohub /mnt/data/uploads ```

To update to the latest version of SynBioHub, first stop and remove the container: 

``` docker stop synbiohub ```

``` docker rm synbiohub ```

Then pull the latest version: 

``` docker pull synbiohub/synbiohub:1.3.0 ```

Finally, run a new container with the latest image: 

``` docker run -v synbiohub:/mnt -p 7777:7777 --name synbiohub -d synbiohub/synbiohub:1.3.0 ```

## From Source

Follow the instructions on the [GitHub README](https://github.com/synbiohub/synbiohub) to install SynBioHub locally on your system. If you would like SynBioHub to run as a service, you can enable Virtuoso using systemd or open a virtual terminal using tmux or GNU screen and run 

``` sudo /usr/local/bin/virtuoso-t +configfile $YOUR_CONFIG_FILE ```

 You should also run SynBioHub as a system service or using a virtual terminal and the command

``` npm start ```

If you are doing development work, you can start SynBioHub with the command 

``` npm run-script dev ```

which will restart the application with any change to the JavaScript source. 

## NGINX configuration

Instructions for managing nginx server blocks can be found [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-server-blocks-virtual-hosts-on-ubuntu-16-04#step-three-create-server-block-files-for-each-domain).

The server block for a SynBioHub installation listening on port 7777 is to the right 

```
client_max_body_size 800m;
proxy_connect_timeout 6000;
proxy_send_timeout 6000;
proxy_read_timeout 6000;
send_timeout 6000;
server {
        listen 80;
        server_name _;

        location / {
                proxy_set_header X-Real-IP  $remote_addr;
                proxy_set_header X-Forwarded-Server $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $host;
                proxy_pass http://127.0.0.1:7777$request_uri;
        }
}
```
This is most useful when you would like to host SynBioHub on a subdomain alongside other content (using nginx as an HTTP proxy) or using HTTPS. 

# User Endpoints

Endpoints that control user related functions

## Login

`POST <SynBioHub URL>/login ` 

This POST request requires email/password and returns a user token that should be passed in the X-authorization header to view private objects and submit new objects, etc. 


```plaintext
curl -X POST -H "Accept: text/plain" -d "email=<email>&password=<password>" <SynBioHub URL>/login
```

```python
import requests

response = requests.post(
    '<SynBioHub URL>/login',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'email': 'test@user.synbiohub',
        'password' : 'test',
        },
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
var url ='<SynBioHub URL>/login';
var headers = {
    "Accept": "text/plain",
}

const params = new URLSearchParams();
params.append('email', '<email>');
params.append('password', '<password>');

fetch(url, { method: 'POST', headers: headers, body: params})
    .then(res => res.text())
    .then(body => console.log(body));
```

Parameter | Description
--------- | ------- | -----------
email | The e-mail address of the user to login with.
password | The password of the user.

## Logout

`POST <SynBioHub URL>/logout `

This post request logs out the user specified in the X-authorization header.


```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/logout
```

```python
import requests

url = '<SynBioHub URL>/logout'

headers ={'Accept' : 'text/plain'}

x = requests.post(url, headers = headers)

print(x.status_code)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
var url ='<SynBioHub URL>/logout';
var headers = {
    "Accept": "text/plain",
}

fetch(url, { method: 'POST', headers: headers})
    .then(res => res.text())
    .then(body => console.log(body));
```

## Register

`POST <SynBioHub URL>/register`

Register a new user on SynBioHub.

## Request Reset Password Token

`POST <SynBioHub URL>/resetPassword`

Request a reset password token.

## Set New Password

`POST <SynBioHub URL>/setNewPassword`

--->takes token and new password

Resets the user's password with the provided token and two copies of the new password.

## View Profile

`GET <SynBioHub URL>/profile`

View the user's profile.



## Update Profile

`POST <SynBioHub URL>/profile`

Update the user's profile.

# Search Endpoints

The following endpoints are used to search within SynBioHub.

<aside class="success">Note that the X-authorization header is not needed, but if specified, search will return information about both public and private objects.</aside>

## Search Metadata

`GET <SynBioHub URL>/search/<key>=<value>&...&<search string>/?offset=#&limit=#`

```javascript
const fetch = require("node-fetch");
const Url = '<SynBioHub URL>/search/<key>=<value>&...&<search string>/?offset=#&limit=#'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/search/<key>=<value>&...&<search string>/?offset=#&limit=#',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" '<SynBioHub URL>/search/objectType%3DComponentDefinition%26role%3D%3Chttp%3A%2F%2Fidentifiers.org%2Fso%2FSO%3A0000316%3E%26GFP/?offset=0&limit=50'

This endpoint returns JSON metadata of the form 
[
    {
        "uri":"<SynBioHub URL>/public/igem/BBa_K1404008/1",
        "name":"BBa_K1404008",
        "description":"p70-CsgA-His*2, double His-tagged curli generator",
        "displayId":"BBa_K1404008",
        "version":"1"
    },

    ...
]
```

Returns the metadata for the object from the specified search query. The search query is composed of a series of key value pairs as described below:

Key/value pair | Description
--------- | -----------
objectType=value | The type of object to search for ( objectType=ComponentDefinition)
sbolTag=value |  A tag in the SBOL namespace to search for ( role=<http://identifiers.org/so/SO:0000316>)
collection=value | Matches objects that are members of the specified collection (collection=<http://synbiohub.org/public/igem/igem_collection>)
dcterms:tag=value | A tag in the dcterms namespace to search for ( dcterms:title='pLac'&) - note this requires an exact match
namespace/tag=value | A full namespace with tag separated by appropriate delimiter ( <http://sbols.org/v2#role>=<http://identifiers.org/so/SO:0000316>)

After the key/value pairs, an optional search string can be provided that will be used to search for partial matches in the displayId, name, or description fields.

Finally, the URL can end with an offset (where you want to start) and limit parameter (how many results you want to get).

## Count Search Results

`GET <SynBioHub URL>/searchCount/<key>=<value>&...&<search string>/?offset=#&limit=# `

Returns the number of items matching the search result.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" '<SynBioHub URL>/searchCount/<key>=<value>&...&<search string>/?offset=#&limit=#'
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/searchCount/<key>=<value>&...&<search string>/?offset=#&limit=#',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const Url = '<SynBioHub URL>/searchCount/<key>=<value>&...&<search string>/?offset=#&limit=#'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Key/value pair | Description
--------- | -----------
objectType=value | The type of object to search for ( objectType=ComponentDefinition)
sbolTag=value |  A tag in the SBOL namespace to search for ( role=<http://identifiers.org/so/SO:0000316>)
collection=value | Matches objects that are members of the specified collection (collection=<http://synbiohub.org/public/igem/igem_collection>)
dcterms:tag=value | A tag in the dcterms namespace to search for ( dcterms:title='pLac'&) - note this requires an exact match
namespace/tag=value | A full namespace with tag separated by appropriate delimiter ( <http://sbols.org/v2#role>=<http://identifiers.org/so/SO:0000316>)

After the key/value pairs, an optional search string can be provided that will be used to search for partial matches in the displayId, name, or description fields.


## Search Root Collections

`GET <SynBioHub URL>/rootCollections`

Returns all root collections.

```plaintext

curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/rootCollections

This endpoint returns JSON metadata of the form:

[
	...,
	{
	"uri":"<SynBioHub URL>/public/igem/igem_collection/1",
	"name":"iGEM Parts Registry",
	"description":"The iGEM Registry is a growing collection of genetic parts that can be mixed and matched to build synthetic biology devices and systems.  As part of the synthetic biology community's efforts to make biology easier to engineer, it provides a source of genetic parts to iGEM teams and academic labs.",
	"displayId":"igem_collection",
	"version":"1"
	},
	...
]

```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/rootCollections',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)
```

```javascript
const fetch = require("node-fetch");
const Url = '<SynBioHub URL>/rootCollections'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Search Submissions

`GET <SynBioHub URL>/manage`

Returns the meta data on all submissions for the user specified by the X-authorization token.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/manage

This endpoint returns JSON metadata of the form:

[...
	{
	"displayId":"bruh_collection",
	"version":"1",
	"name":"bruh",
	"description":"testbruh",
	"type":"http://sbols.org/v2#Collection",
	"uri":"http://localhost:7777/user/testuser/bruh/bruh_collection/1",
	"typeName":"Collection",
	"url":"/user/testuser/bruh/bruh_collection/1",
	"triplestore":"private",
	"prefix":""
	}
...]
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/manage',
    headers={'X-authorization': '<token>',
    'Accept': 'text/plain'}
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/manage'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Search Shared Objects

`GET <SynBioHub URL>/shared`

Returns the meta data on objects that other users have shared with the user specified by the X-authorization token. 

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/shared

This endpoint returns JSON metadata of the form:

[...
	{
	"displayId":"bruh_collection",
	"version":"1",
	"name":"bruh",
	"description":"testbruh",
	"type":"http://sbols.org/v2#Collection",
	"uri":"http://localhost:7777/user/testuser/bruh/bruh_collection/1",
	"typeName":"Collection",
	"url":"/user/testuser/bruh/bruh_collection/1",
	"triplestore":"private",
	"prefix":""
	}
...]
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/shared',
    headers={'X-authorization': '<token>',
    'Accept': 'text/plain'}
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/shared'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Search Sub-Collections

`GET <URI>/subCollections`

Returns the collections that are members of another collection.

```python
import requests

response = requests.get(
    '<URI>/subcollections',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/subcollections

This endpoint returns JSON metadata of the form:
[
	{
		"uri":"<SynBioHub URL>/public/bsu/SpaRK_collection/1",
		"name":"SpaRK","description":"SpaRK",
		"displayId":"SpaRK_collection",
		"version":"1"
	}

]
```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/subcollections'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```


## Search Twins

`GET <URI>/twins`

Returns other components that have the same sequence.

```python
import requests

response = requests.get(
    '<URI>/twins',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/twins
```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/twins'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Search Similar 

`GET <URI>/similar`

Returns other components that have similar sequences.

Note that this endpoint only works if SBOLExplorer is activated.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/similar
```

```python
import requests

response = requests.get(
    '<URI>/similar',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/similar'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```
## Search Uses

`GET <URI>/uses`

Returns any other object that refers to this object, for example, if this is a component, it will return all other components that use this as a sub-component.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/uses

This endpoint returns JSON metadata of the form:

[
	{
		"type":"http://sbols.org/v2#Collection",
		"uri":"<SynBioHub URL>/public/bsu/bsu_collection/1",
		"name":"Bacillus subtilis Collection",
		"description":"This collection includes information about promoters, operators, CDSs and proteins from Bacillus subtilis. Functional interactions such as transcriptional activation and repression, protein production and various protein-protein interactions are also included.",
		"displayId":"bsu_collection",
		"version":"1"
	}
]

```

```python
import requests

response = requests.get(
    '<URI>/uses',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const Url = '<SynBioHub URL>/public/bsu/BO_5629/1/uses'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Count Objects by Type

`GET <SynBioHub URL>/<ObjectType>/count`

Returns the number of objects with a specified object type.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/<ObjectType>/Count
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/<ObjectType>/Count',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```


```javascript
const fetch = require("node-fetch");
const Url = '<SynBioHub URL>/<ObjectType>/Count'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Note that you can replace `<ObjectType>` with any SBOL object type, such as `ComponentDefinition`, `SequenceAnnotation`, etc. See [here](https://sbolstandard.org/data-model-specification/) for more object types

## SPARQL Query

`GET <SynBioHub URL>/sparql?query=<SPARQL query>`

Returns the results of the SPARQL query in JSON format. 

```plaintext
curl -X GET -H "Accept: application/json" <SynBioHub URL>/sparql?query=<SPARQL query>

This endpoint returns JSON metadata of the form:

[...
	{ 
		"s": { "type": "uri", "value": "<SynBioHub URL>/public/igem/BBa_K1732001/annotation2443059/range2443059/1" }	, 
		"p": { "type": "uri", "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type" }	, 
		"o": { "type": "uri", "value": "http://sbols.org/v2#Range" }
	}
...]
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/sparql?query=<SPARQL query>',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
```


```javascript
const fetch = require("node-fetch");
const Url = '<SynBioHub URL>/sparql?query=<SPARQL query>'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```


# Download Endpoints

The following endpoints are for downloading content from SynBioHub in various formats.

<aside class="success">Note that the X-authorization header is needed for downloading information about private objects.</aside>

## Download SBOL

`GET <URI>/sbol` OR `GET <URI>`

Returns the object from the specified URI in SBOL format.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/sbol
```

```python
import requests

response = requests.get(
    '<URI>/sbol',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/sbol'
const otherPram={
    headers:{
	"content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
.then(res => res.buffer()).then(buf => console.log(buf.toString()))
.catch (error=>console.log(error))
```

## Download Non-Recursive SBOL 

`GET <URI>/sbolnr`

Returns the object from the specified URI in SBOL format non-recursively ( i.e. fetches the object without its children.)

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/sbolnr
```

```python
import requests

response = requests.get(
    '<URI>/sbolnr',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/sbolnr'
const otherPram={
    headers:{
	"content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
.then(res => res.buffer()).then(buf => console.log(buf.toString()))
.catch (error=>console.log(error))
```


## Download Metadata

`GET <URI>/metadata`

Returns the metadata for the object from the specified URI.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/metadata
```

```python
import requests

response = requests.get(
    '<URI>/metadata',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/metadata'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Download GenBank

`GET <URI>/gb`

Returns the object from the specified URI in GenBank format.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/gb
```

```python
import requests

response = requests.get(
    '<URI>/gb',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/gb'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Download FASTA

`GET <URI>/fasta`

Returns the object from the specified URI in FASTA format.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/fasta
```

```python
import requests

response = requests.get(
    '<URI>/fasta',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/fasta'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Download GFF3 

`GET <URI>/gff`

Returns the object from the specified URI in GFF3 format.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/gff
```

```python
import requests

response = requests.get(
    '<URI>/gff',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/gff'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Download Attachment

`GET <URI>/download `

Returns the source for an attachment to the specified URI.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/download -O --compressed
```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/download'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

```python
import requests

response = requests.get(
    '<URI>/download',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

# Submission Endpoints

The following endpoints are for managing submissions on SynBioHub.

<aside class="success">Note that the X-authorization header is required for all submission endpoints.</aside>

## Submit

`POST <SynBioHub URL>/submit`

Create a new collection including the elements within a file or add to a preexisting collection using the elements within a file.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -F id=<id> -F version=<version> -F name=<name> -F description=<description> -F citations=<citations> -F overwrite_merge=<overwrite_merge> -F file=@<filename> <SynBioHub URL>/submit
```

```python
import requests

response = requests.post(
    '<SynBioHub URL>/submit',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    files={
	'files': open('<file.txt>','rb'),
	},
    data={
        'id': '<id>',
        'version' : '<version>',
        'name' : '<name>',
        'description' : '<description>',
        'citations' : '<citations>',
        'overwrite_merge' : '<tabState>'
    },

)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const FormData = require('form-data');
const { createReadStream } = require('fs');
const url = '<SynBioHub URL>/submit'
const stream = createReadStream('input.txt');
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const form = new FormData();
form.append('id', '<id>');
form.append('version', '<version>');
form.append('name', '<name>');
form.append('description', '<description>');
form.append('citations', '<citations>');
form.append('overwrite_merge', '<overwrite_merge>');
form.append('file', stream);


fetch(url, { method: 'POST', headers: headers, body:form})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | -----------
id | a user-defined string identifier for the submission; alphanumeric and underscores only,  (ex.`BBa_R0010`)
version |  the version string to associate with the submission,  (ex. `1`)
name |  the name string to assign to the submission
description | the description string to assign to the submission
citations | a list of comma separated pubmed IDs of citations to store with the submission
overwrite_merge | '0' prevent if submission exists, '1' overwrite if submission exists, '2' to merge and prevent if submission exists, '3' to merge and overwrite matching URIs
file | contents of an SBOL2, SBOL1, GenBank, FASTA, GFF3, ZIP, or COMBINE Archive file
rootCollections | the URI of the collection to be submitted into

If creating a collection, provide the id, version, name, description, citations, and optionally a file. In this case, overwrite_merge should be 0 or 1. If submitting the contents into an existing collection, otherwise, only provide a URI for the rootCollections that you are submitting into and the file that you are submitting.

## Make Public Collection

`POST <URI>/makePublic`

Makes the collection specified by the URI public.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "id=<id>&version=<version>&name=<name>&description=<description>&citations=<citations>&tabState=<tabState>" URI/makePublic
```

```python
import requests

response = requests.post(
    '<URI>/makePublic',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'id': '<id>',
        'version' : '<version>',
        'name' : '<name>',
        'description' : '<description>',
        'citations' : '<citations>',
        'tabState' : '<tabState>'
        },
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<URI>/makePublic'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('id', '<id>');
params.append('version', '<version>');
params.append('name', '<name>');
params.append('description', '<description>');
params.append('citations', '<citations>');
params.append('tabState', '<tabState>');

fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | ------- | -----------
id | The id for the new collection.
version | The version for the new collection.
name | The name for the new collection (optional: default is existing name).
description | The description for the new collection (optional: default is existing description).
citations | The comma-separated listed of PubMed ids (optional: default is existing citations).
tabState | Use "new" for moving to a new public collection, and "existing" if moving into an existing public collection.
collections| If moving into an existing collection, collections is the URI of the collection


## Remove Collection

`GET <URI>/removeCollection `

Removes the collection specified by the URI.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/removeCollection
```

```python
import requests

response = requests.get(
    '<URI>/removeCollection',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/removeCollection'
const otherPram={
    headers:{
	"content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
.then(res => res.buffer()).then(buf => console.log(buf.toString()))
.catch (error=>console.log(error))
```

## Remove Object

`GET <URI>/remove`

Remove the object specified by the URI, and the references to that object.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/remove
```

```python
import requests

response = requests.get(
    '<URI>/remove',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```


```javascript
const fetch = require("node-fetch");
const Url = '<URI>/remove'
const otherPram={
    headers:{
	"content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
.then(res => res.buffer()).then(buf => console.log(buf.toString()))
.catch (error=>console.log(error))
```

## Replace Object

`GET <URI>/replace `

Remove the object specified from URI, but leave references to the object.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/remove
```

```python
import requests

response = requests.get(
    '<URI>/replace',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/replace'
const otherPram={
    headers:{
	"content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
.then(res => res.buffer()).then(buf => console.log(buf.toString()))
.catch (error=>console.log(error))
```

## Update Collection Icon

`POST <URI>/icon`

Updates the collection's icon.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -F "collectionIcon=<icon>" <URI>/icon
```

```python
import requests

response = requests.post(
    'localhost:7777/public/testid0/testid0_collection/1/icon',
    headers={
        'X-authorization': '88472249-ee84-49c6-8f81-0b7a6b9dce64',
        'Accept': 'text/plain'
    },
    files={
        'collectionIcon': open('icon.png', 'rb'),
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<URI>/addOwner'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('user', '<id>');
params.append('uri', '<version>');


fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | ------- | -----------
collectionIcon | The desired collection icon.


# Permission Endpoints


The following endpoints are for managing permissions on SynBioHub.

<aside class="success">Note that the X-authorization header is required for all permission endpoints.</aside>

## Add Owner

`POST <URI>/addOwner`

Adds an owner to an object specfied by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "user=<user>&uri=<uri>" <URI>/addOwner
```

```python
import requests

response = requests.post(
    '<URI>/addOwner',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'user': '<user>',
        'uri' : '<uri>'
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<URI>/addOwner'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('user', '<user>');
params.append('uri', '<uri>');


fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | ------- | -----------
user | The user id of owner being added.
uri | The identity of the object to add owner to.

## Remove Owner

`POST <URI>/removeOwner/<username>`

Removes an owner from an object specified  by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "userUri=<userUri>" <URI>/removeOwner/<username>
```

```python
import requests

response = requests.post(
    '<URI>/removeOwner/<username>',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'userUri': '<userUri>',
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<URI>/removeOwner/<username>'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('userUri', '<userUri>');



fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | ------- | -----------
userUri | The user id of the user being removed.

# Edit Endpoints

These endpoints allow you to edit various fields within each object. 

<aside class="success">Note that the X-authorization header is required for all edit endpoints.</aside>

## Edit Mutable Descriptions

`POST <SynBioHub URL>/updateMutableDescription `

Edit the mutable description of an object specified by the URI. 

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "uri=<uri>&value=<value>" <SynBioHub URL>/updateMutableDescription
```

```python
import requests

response = requests.post(
    '<SynBioHub URL>/updateMutableDescription',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'uri': '<uri>',
        'value' : '<value>'
        },
)

print(response.status_code)
print(response.content)

```

```javascript

const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<SynBioHub URL>/updateMutableDescription'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};


const params = new URLSearchParams();
params.append('uri', '<uri>');
params.append('value', '<value>');


fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))


```

Parameter | Description
--------- | -----------
uri | The identity of the object to update.
value | The new value for the mutable description


## Edit Mutable Notes

`POST <SynBioHub URL>/updateMutableNotes`

Edit the mutable notes of an object specified by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "uri=<uri>&value=<value>" <SynBioHub URL>/updateMutableNotes
```

```python

import requests

response = requests.post(
    '<SynBioHub URL>/updateMutableNotes',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'uri': '<uri>',
        'value' : '<value>'
        },
)

print(response.status_code)
print(response.content)

```

```javascript

const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<SynBioHub URL>/updateMutableNotes'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};


const params = new URLSearchParams();
params.append('uri', '<uri>');
params.append('value', '<value>');


fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))


```

Parameter | Description
--------- | -----------
uri | The identity of the object to update.
value | The new value for the mutable notes.

## Edit Mutable Source

`POST <SynBioHub URL>/updateMutableSource`

Edit the mutable source of an object specified by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "uri=<uri>&value=<value>" <SynBioHub URL>/updateMutableSource
```

```python

import requests

response = requests.post(
    '<SynBioHub URL>/updateMutableSource',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'uri': '<uri>',
        'value' : '<value>'
        },
)

print(response.status_code)
print(response.content)

```

```javascript

const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<SynBioHub URL>/updateMutableSource'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};


const params = new URLSearchParams();
params.append('uri', '<uri>');
params.append('value', '<value>');


fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))


```

Parameter | Description
--------- | -----------
uri | The identity of the object to update.
value | The new value for the mutable source.

## Edit Citations

`POST <SynBioHub URL>/updateCitations`

Edit the citations of an object specified by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "uri=<uri>&value=<value>" <SynBioHub URL>/updateCitations
```

```python

import requests

response = requests.post(
    '<SynBioHub URL>/updateCitations',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'uri': '<uri>',
        'value' : '<value>'
        },
)

print(response.status_code)
print(response.content)

```

```javascript

const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<SynBioHub URL>/updateCitations'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};


const params = new URLSearchParams();
params.append('uri', '<uri>');
params.append('value', '<value>');


fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))


```

Parameter | Description
--------- | -----------
uri | The identity of the object to update.
value | The new value for the citation.

## Edit Field

`POST <URI>/edit/<field>`

Edit field of an object.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -d "previous=<previous>&object=<test>"  <URI>/edit/<field>
```

```python
import requests

response = requests.post(
    '<URI>/edit/<field>',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'previous': '<previous>',
        'object' : '<object>',
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<URI>/edit/<field>'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('previous', '<previous>');
params.append('object', '<object>');

fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | -----------
previous | The previous value of the field.
object | The new value of the field.

Possible fields to edit:
`title`
`description`
`role`
`wasDerivedFrom`
`type`

## Add Field

`POST <URI>/add/<field>`

Add field to an object.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "object=<object>"  <URI>/add/<field>
```

```python
import requests

response = requests.post(
    '<URI>/add/<field>',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'object' : '<object>',
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<SynBioHub URL>/add/<field>'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('object', '<object>');

fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```
Parameter | Description
--------- | -----------
object | The new value of the field.

Possible fields to add:
`role`
`type`

## Remove Field

`POST <URI>/remove/<field>`

Remove field from an object.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "object=<object>"  <URI>/remove/<field>
```

```python
import requests

response = requests.post(
    '<URI>/remove/<field>',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'object' : '<object>',
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<URI>/remove/<field>'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('object', '<object>');

fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```
Parameter | Description
--------- | -----------
object | The value of the field to remove.

Possible fields to remove:
`role`
`type`

## Add to Collection

## Remove Membership

# Attachment Endpoints

The following endpoints are for creating attachments on SynBioHub.

<aside class="success">Note that the X-authorization header is required for all attachment endpoints.</aside>

## Attach File

`POST <URI>/attach `

Attach a specified file to a given URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -F 'file=@<filename>' <URI>/attach
```

```python
import requests

response = requests.post(
    '<URI>/attach',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    files={
	'file': open('<file>','rb'),
	},

)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const FormData = require('form-data');
const { createReadStream } = require('fs');
const url = '<URI>/attach'
const stream = createReadStream('<file>');
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const form = new FormData();
form.append('file', stream);


fetch(url, { method: 'POST', headers: headers, body:form})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | -----------
file | The name of the file to attach.

## Attach URL

`POST <URI>/attachURL `

Attach a specified URL to a given URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -d "url=<url>&name=<name>&type=<type>" <URI>/attachURL
```

```python
import requests

response = requests.post(
    '<URI>/attachURL',
    headers={
        'X-authorization': '<token>',
        'Accept': 'text/plain'
    },
    data={
        'url': '<url>',
        'name' : '<name>',
        'type' : '<type>'
        },
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = '<URI>/attachURL'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('url', '<url>');
params.append('name', '<name>');
params.append('type', '<type>');


fetch(url, { method: 'POST', headers: headers, body:params})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

Parameter | Description
--------- | -----------
url | The URL to attach.
name | The name of the attachment.
type | The format type of the object at the URL.

## Download Attachment

`GET <URI>/download `

Returns the source for an attachment to the specified URI.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <URI>/download -O --compressed
```

```javascript
const fetch = require("node-fetch");
const Url = '<URI>/download'
const otherPram={
    headers:{
        "content-type" : "text/plain; charset=UTF-8"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

```python
import requests

response = requests.get(
    '<URI>/download',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)


```


# Administration Endpoints

The following endpoints are for users with administration privileges.

<aside class="success">Note that the X-authorization header is required for all administration endpoints.</aside>

## SPARQL Admin Query

`GET <SynBioHub URL>/admin/sparql?query=<SPARQL query>`

Returns the results of the SPARQL admin query in JSON format.

```plaintext
curl -X GET -H "Accept: application/json" '<SynBioHub URL>/admin/sparql?query=<SPARQL query>
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/sparql?query=<SPARQL query>',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
```

```javascript
const fetch = require("node-fetch");
const Url = '<SynBioHub URL>/admin/sparql?query=<SPARQL query>'
const otherPram={
    headers:{
        "content-type" : "application/json; charset=UTF-8",
	"X-authorization" : "<token>"
    },
    method:"GET"
};
fetch(Url,otherPram)
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Get Status

`GET <SynBioHub URL>/admin`

Returns configuration options for SynBioHub.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## View Graphs

`GET <SynBioHub URL>/admin/graphs`

Returns existing graphs and its number of triples.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/graphs
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/graphs',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/graphs'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## View Log

`GET <SynBioHub URL>/admin/log`

Returns the SynBioHub log file.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/log
```
```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/log',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/log'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```


## View Current Mail Settings

`GET <SynBioHub URL>/admin/mail`

Returns the current mail configuration.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/mail
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/mail',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/mail'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```


## Update Mail Settings

`POST <SynBioHub URL>/admin/mail`

Update the mail configuration.

## View Plugins

`GET <SynBioHub URL>/admin/plugins`

View current plugins.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/plugins
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/plugins',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/plugins'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Save Plugin

`POST <SynBioHub URL>/admin/savePlugin`

Save a new plugin.

## Delete Plugin

`POST <SynBioHub URL>/admin/deletePlugin`

Delete a plugin.

## View Registries

`GET <SynBioHub URL>/admin/registries`

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/registries
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/registries',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/registries'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

View current registries.

## Save Registry

`POST <SynBioHub URL>/admin/saveRegistry`

Save a new registry.

## Delete Registry

`POST <SynBioHub URL>/admin/deleteRegistry`

Delete a registry.

## Set Administrator Email

`POST <SynBioHub URL>/admin/setAdministratorEmail`

Update Web Of Registries administrator email.

## Retrieve From Web Of Registries

`POST <SynBioHub URL>/admin/retrieveFromWebOfRegistries`

Update registries from Web-of-Registries.

## Federate

`POST <SynBioHub URL>/admin/federate`

Send request to join Web-of-Registries for a SynBioHub.

## View Remotes

`GET <SynBioHub URL>/admin/remotes`

View current remotes.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/remotes
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/remotes',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/remotes'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Save Remote

`POST <SynBioHub URL>/admin/saveRemote`

Save a new remote.

## Delete Remote

`POST <SynBioHub URL>/admin/deleteRemote`

Delete a remote.

## View SBOLExplorer

`GET <SynBioHub URL>/admin/explorer`

View current SBOLExplorer Endpoint

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/explorer
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/explorer',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/explorer'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Update SBOLExplorer

`POST <SynBioHub URL>/admin/explorer`

Update SBOLExplorer endpoint.

## Update SBOLExplorer Index.

`POST <SynBioHub URL>/admin/explorerUpdateIndex`

Updates SBOLExplorer index.

## View Theme

`GET <SynBioHub URL>/admin/theme`

View the current theme.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/theme
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/theme',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/theme'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Update Theme

`POST <SynBioHub URL>/admin/theme`

Update the theme.

## View Users

`GET <SynBioHub URL>/admin/users`

View the current users.

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" <SynBioHub URL>/admin/users
```

```python
import requests

response = requests.get(
    '<SynBioHub URL>/admin/users',
    params={'X-authorization': '<token>'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
`
```

```javascript
const fetch = require("node-fetch");
const url = '<SynBioHub URL>/admin/users'
const headers={
        "Accept" : "text/plain; charset=UTF-8",
	"X-authorization" : "<token>"
};
fetch(url, { method: 'GET', headers: headers})
    .then(res => res.buffer()).then(buf => console.log(buf.toString()))
    .catch (error=>console.log(error))
```

## Update Users

`POST <SynBioHub URL>/admin/users`

Update the user's settings.

## New User

`POST <SynBioHub URL>/admin/newUser`

Create a new user.

## Update User

`POST <SynBioHub URL>/admin/updateUser`

Update a user's settings.

## Delete User

`POST <SynBioHub URL>/admin/deleteUser`

Delete a user. 

# Plugins

### Plugin Specification
Plugins must provide three endpoints, /status, /evaluate, and /run. 

*Status endpoint*
The status endpoint should listen for HTTP GET requests and return an HTTP 200 OK and (optionally) a short message if the plugin service is ready to handle requests. Otherwise, an HTTP error code and short error message should be returned. 

*Evaluate endpoint*
The evaluate endpoint should listen for HTTP POST requests, and return an HTTP 200 OK if the request can be handled by the run endpoint. 
The body of evaluate requests sent to each plugin type is described below.

*Run endpoint*
The run endpoint should listen for HTTP POST requests, and return an HTTP 200 OK with the result of the plugin operation. 
The body of run requests sent to each plugin type is described below.

### Implementation
Plugins and SynBioHub will communicate using HTTP. The end user’s browser will not communicate with the plugin server.

#### Submission
For both the `evaluate` and `run` endpoints, SynBioHub will send a JSON object structured as follows:
` { manifest: { files: [ ... ] } } `

Each file in `files` will have the following parameters:

    1. `filename`: The name of the file

    2. `type`: the Content-Type of the file

    3. `url`: A URL where the file can be accessed

*Evaluate endpoint*

SynBioHub will send an HTTP POST request containing the manifest to the submit plugin's evaluate endpoint.
The plugin should respond with HTTP 200 OK if the plugin can handle the submission.

*Run endpoint*

SynBioHub will send an HTTP POST request containing the manifest to the submit plugin's run endpoint.
It will also send the `instanceUrl` parameter.
The plugin should respond with an HTTP response and file attachment, which represents the submission.

#### Download
*Evaluate endpoint*

SynBioHub will send an HTTP POST request to the download plugin's evaluate endpoint. The body of the request will contain a **JSON** containing:

    1. `type`: The RDF type of the top-level object

The plugin should respond with an HTTP 200 OK if the request can be handled.

*Run endpoint*

SynBioHub will send an HTTP POST request to the dwonload plugin’s run endpoint. The body of the request will contain a **JSON** object containing:

	1. `complete_sbol`: the single-use URL for the complete object to operate on

	2. `shallow_sbol`: the single-use URL for a summarized or truncated view of the object

	3. `top_level`: the top-level URL of the SBOL object

	4. `instanceUrl`: the top-level URL of the SBOL object 

	5. `size`: a number representing an estimate of the size of the object, probably triple count

The plugin should respond with an HTTP request and file attachment which represents the object.

#### Visualization
The response of visualization plugins should be **HTML** which will be displayed on the top-level page. Rendering responses may be cached to improve performance.

*Evaluate endpoint*

SynBioHub will send an HTTP POST request to the visualization plugin's evaluate endpoint. The body of the request will contain a **JSON** containing:

    1. `type`: The RDF type of the top-level object

*Run endpoint*

SynBioHub will send an HTTP POST request to the visualization plugin’s run endpoint. The body of the request will contain a **JSON** object containing:

	1. `complete_sbol`: the single-use URL for the complete object to operate on

	2. `shallow_sbol`: the single-use URL for a summarized or truncated view of the object

	3. `top_level`: the top-level URL of the SBOL object

	4. `instanceUrl`: the top-level URL of the SBOL object 

	5. `size`: a number representing an estimate of the size of the object, probably triple count

The plugin should respond with an HTML page to be rendered in-frame on the corresponding SynBioHub page. 
