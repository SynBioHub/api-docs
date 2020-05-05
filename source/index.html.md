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
What is SynBioHub?

SynBioHub includes two projects:

* An [open source software project](http://github.com/SynBioHub) providing a web interface for the storing and publishing of synthetic biology designs.
* A public instance of the aforementioned software project at [synbiohub.org](http://synbiohub.org), allowing users to upload and share designs.

For those familiar with the SBOL Stack, SynBioHub incorporates and extends its functionality.  An SBOL Stack installation is not required for SynBioHub.


What can SynBioHub be used for?

SynBioHub can be used to publish a library of synthetic parts and designs as a service, to share designs with collaborators, and to store designs of biological systems locally. Data in SynBioHub can be accessed via the HTTP API, Java API, or Python API where it can then be integrated into CAD tools for building genetic designs. SynBioHub contains an interface for users to upload new biological data to the database, to visualize DNA parts, to perform queries to access desired parts, and to download SBOL, GenBank, FASTA, etc.


### Contributors

SynBioHub

* [James Alastair McLaughlin](http://homepages.cs.ncl.ac.uk/j.a.mclaughlin)*
* [Prof. Chris J. Myers](http://www.async.ece.utah.edu/Myers)†
* [Dr Goksel Misirli](http://homepages.cs.ncl.ac.uk/goksel.misirli)*
* [Prof. Anil Wipat](http://homepages.cs.ncl.ac.uk/anil.wipat/)*
* [Zach Zundel](http://www.async.ece.utah.edu/people/students/zach-zundel/)†
* [James Scholz](https://www.async.ece.utah.edu/~scholz/)†

SBOL Stack

* [Dr Curtis Madsen](http://sites.bu.edu/ckmadsen/)§ 
* [James Alastair McLaughlin](http://homepages.cs.ncl.ac.uk/j.a.mclaughlin)* 
* [Dr Goksel Misirli](http://homepages.cs.ncl.ac.uk/goksel.misirli/)*
* [Dr Matthew Pocock](http://intbio.ncl.ac.uk/?people=matthew-pocock)‡
* [Dr Keith Flanagan](http://intbio.ncl.ac.uk/?people=dr-keith-flanagan)*
* [Dr Jennifer Hallinan](https://research.science.mq.edu.au/synthetic-biology/people/)Δ
* [Prof. Anil Wipat](http://homepages.cs.ncl.ac.uk/anil.wipat/)* 

Web Design

* [Antarctic Design](http://www.antarctic-design.co.uk/) 



##### *[Newcastle University](http://ncl.ac.uk) 
##### †[University of Utah](https://www.utah.edu/) 
##### ‡Turing Ate My Hamster Ltd
##### §[Boston University](http://www.bu.edu/) 
##### Δ[Macquarie University](https://www.mq.edu.au/) 

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

##### SendGrid email setup
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

`POST http://synbiohub.org/login ` 

This POST request requires email/password and returns a user token that should be passed in the X-authorization header to view private objects and submit new objects, etc. 


```plaintext
curl -X POST -H "Accept: text/plain" -d "email=<email>&password=<password>" https://synbiohub.org/login
```

```python
import requests

response = requests.post(
    'http://localhost:7777/login',
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
var url ='https://synbiohub.org/login';
var headers = {
    "Accept": "text/plain",
}

const params = new URLSearchParams();
params.append('email', 'test@user.synbiohub');
params.append('password', 'test');

fetch(url, { method: 'POST', headers: headers, body: params})
    .then(res => res.text())
    .then(body => console.log(body));
```

Parameter | Description
--------- | ------- | -----------
email | the e-mail address of the user to login with
password | the password of the user

## Logout

`POST http://synbiohub.org/logout `

This post request logs out the user specified in the X-authorization header.


```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" localhost:7777/logout
```

```python
import requests

url = 'https://synbiohub.org/logout'
myobj = {'email': 'test@user.synbiohub',
         'password' : 'test'}
headers ={'Accept' : 'text/plain'}

x = requests.post(url, data = myobj, headers = headers)

print(x.status_code)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
var url ='https://synbiohub.org/logout';
var headers = {
    "Accept": "text/plain",
}

const params = new URLSearchParams();
params.append('email', 'test@user.synbiohub');
params.append('password', 'test');

fetch(url, { method: 'POST', headers: headers, body: params})
    .then(res => res.text())
    .then(body => console.log(body));
```

## Reset Password

`POST <SynBioHub URL>/setNewPassword`

Resets the user's password.

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
const Url = 'https://synbiohub.org/search/objectType%3DComponentDefinition%26role%3D%3Chttp%3A%2F%2Fidentifiers.org%2Fso%2FSO%3A0000316%3E%26GFP/?offset=0&limit=50'
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
    'https://synbiohub.org/search/objectType%3DComponentDefinition%26role%3D%3Chttp%3A%2F%2Fidentifiers.org%2Fso%2FSO%3A0000316%3E%26GFP/?offset=0&limit=50',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" 'https://synbiohub.org/search/objectType%3DComponentDefinition%26role%3D%3Chttp%3A%2F%2Fidentifiers.org%2Fso%2FSO%3A0000316%3E%26GFP/?offset=0&limit=50

This endpoint returns JSON metadata of the form 
[
    {
        "uri":"http://synbiohub.org/public/igem/BBa_K1404008/1",
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

Finally, the URL can end with an offset and limit parameter.

## Count Search Results

`GET <SynBioHub URL>/searchCount/<key>=<value>&...&<search string>/?offset=#&limit=# `

Returns the number of items matching the search result.

```python
import requests

response = requests.get(
    'https://synbiohub.org/searchCount/objectType%3DComponentDefinition%26role%3D%3Chttp%3A%2F%2Fidentifiers.org%2Fso%2FSO%3A0000316%3E%26GFP',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/searchCount/objectType%3DComponentDefinition%26role%3D%3Chttp%3A%2F%2Fidentifiers.org%2Fso%2FSO%3A0000316%3E%26GFP

```

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/searchCount/objectType%3DComponentDefinition%26role%3D%3Chttp%3A%2F%2Fidentifiers.org%2Fso%2FSO%3A0000316%3E%26GFP'
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

## Search Root Collections

`GET <SynBioHub URL>/rootCollections
`

Returns all root collections.


```plaintext

curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/rootCollections

This endpoint returns JSON metadata of the form:

[
	...,
	{
	"uri":"https://synbiohub.org/public/igem/igem_collection/1",
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
    'https://synbiohub.org/public/igem/igem_collection/1/rootCollections',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)
```

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/igem_collection/1/rootCollections'
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
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/manage

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
    'https://synbiohub.org/manage',
    headers={'X-authorization': '<token>',
    'Accept': 'text/plain'}
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const url = 'https://synbiohub.org/manage'
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
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/shared

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
    'https://synbiohub.org/shared',
    headers={'X-authorization': '<token>',
    'Accept': 'text/plain'}
)

print(response.status_code)
print(response.content)

```

```javascript
const fetch = require("node-fetch");
const url = 'https://synbiohub.org/shared'
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
    'https://synbiohub.org/public/bsu/bsu_collection/1/subcollections',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/bsu/bsu_collection/1/subcollections

This endpoint returns JSON metadata of the form:
[
	{
		"uri":"https://synbiohub.org/public/bsu/SpaRK_collection/1",
		"name":"SpaRK","description":"SpaRK",
		"displayId":"SpaRK_collection",
		"version":"1"
	}

]
```

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/bsu/bsu_collection/1/subcollections'
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

`GET <SynBioHub URL>/twins`

Returns other components that have the same sequence.

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/bsu/BO_5629/1/twins',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/bsu/BO_5629/1/twins
```

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/bsu/BO_5629/1/twins'
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

`GET <SynBioHub URL>/similar`

Returns other components that have similar sequences.

Note that this endpoint only works if SBOLExplorer is activated.

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/bsu/BO_5629/1/similar',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/bsu/BO_5629/1/similar
```

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/bsu/BO_5629/1/similar'
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

`GET <SynBioHub URL>/uses`

Returns any other object that refers to this object, for example, if this is a component, it will return all other components that use this as a sub-component.

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/bsu/BO_5629/1/uses',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/bsu/BO_5629/1/uses

This endpoint returns JSON metadata of the form:

[
	{
		"type":"http://sbols.org/v2#Collection",
		"uri":"https://synbiohub.org/public/bsu/bsu_collection/1",
		"name":"Bacillus subtilis Collection",
		"description":"This collection includes information about promoters, operators, CDSs and proteins from Bacillus subtilis. Functional interactions such as transcriptional activation and repression, protein production and various protein-protein interactions are also included.",
		"displayId":"bsu_collection",
		"version":"1"
	}
]

```


```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/bsu/BO_5629/1/uses'
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




## Count Objects

`GET <SynBioHub URL>/<ObjectType>/count`

Returns the number of objects with a specified object type.

```plaintext
curl -X GET https://synbiohub.org/Collection/Count
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/Collection/Count',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)
```


```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/Collection/Count'
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

Note that you can replace `<ObjectType>` with any object type, such as `ComponentDefinition`, `SequenceAnnotation`, etc.

## SPARQL Query

`GET <SynBioHub URL>/sparql?query=<SPARQL query>`

Returns the results of the SPARQL query in JSON format. 

```plaintext
curl -X GET -H "Accept: application/json" https://synbiohub.org/sparql?query=select%20%3Fs%20%3Fp%20%3Fo%20where%20%7B%20%3Fs%20%3Fp%20%3Fo%20%7D

This endpoint returns JSON metadata of the form:

[...
	{ 
		"s": { "type": "uri", "value": "https://synbiohub.org/public/igem/BBa_K1732001/annotation2443059/range2443059/1" }	, 
		"p": { "type": "uri", "value": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type" }	, 
		"o": { "type": "uri", "value": "http://sbols.org/v2#Range" }
	}
...]
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/sparql?query=select%20%3Fs%20%3Fp%20%3Fo%20where%20%7B%20%3Fs%20%3Fp%20%3Fo%20%7D',
    params={'X-authorization': 'token'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
```


# Download Endpoints

The following endpoints are for downloading content from SynBioHub in various formats.

<aside class="success">Note that the X-authorization header is needed for downloading information about private objects.</aside>

## Download Attachment

`GET <URI>/download `

Returns the source for an attachment to the specified URI.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/test/attachment_00009TGVMsfEoCdMeRzHrU/1/download'
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
    'https://synbiohub.org/public/test/attachment_00009TGVMsfEoCdMeRzHrU/1/download',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)


```

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/test/attachment_00009TGVMsfEoCdMeRzHrU/1/download -O --compressed
```


## Download SBOL

`GET <URI>/sbol` OR
`GET <URI>`

Returns the object from the specified URI in SBOL format.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/sbol'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_F2620/1/sbol
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_F2620/1/sbol',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Download Non-Recursive SBOL 

`GET <URI>/sbolnr`

Returns the object from the specified URI in SBOL format non-recursively ( i.e. fetches the object without its children.)

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/sbolnr'
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


```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_F2620/1/sbolnr
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_F2620/1/sbolnr',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Download Metadata

`GET <URI>/metadata`

Returns the metadata for the object from the specified URI.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/metadata'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_K1479017/1/metadata
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_K1479017/1/metadata',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Download GenBank

`GET <URI>/gb`

Returns the object from the specified URI in GenBank format.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.gb'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.gb
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.gb',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Download FASTA

`GET <URI>/fasta`

Returns the object from the specified URI in FASTA format.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.fasta'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.fasta
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.fasta',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Download GFF3 

`GET <URI>/gff`

Returns the object from the specified URI in GFF3 format.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.gff'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.gff
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_K1479017/1/BBa_K1479017.gff',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Download Visualization

`GET <URI>/visualization`

Returns the visualization for the object from the specified URI.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/visualization'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_K1479017/1/visualization
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_K1479017/1/visualization',
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

`POST <SynBioHub URL>/submit `

Create a new collection including the elements within a file or add to a preexisting collection using the elements within a file.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -F id=<id> -F version=<version> -F name=<name> -F description=<description> -F citations=<citations> -F overwrite_merge=<overwrite_merge> -F file=@<filename> https://synbiohub.org/submit
```

```python
import requests

response = requests.post(
    'https://synbiohub.org/submit',
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
const url = 'https://synbiohub.org/submit'
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
curl -X POST -H "Accept: text/plain" -H "X-authorization:<>token" -d "id=bruh&version=1&name=&description=&citations=&tabState=new" https://synbiohub.org/user/testuser/bruh1/bruh1_collection/1/makePublic
```

```python
import requests

response = requests.post(
    'https://synbiohub.org/user/testuser/bruh1/bruh1_collection/1/makePublic',
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
const url = 'https://synbiohub.org/user/testuser/bruh2/bruh2_collection/1/makePublic'
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


## Remove Collection

`GET <URI>/removeCollection `

Removes the collection specified by the URI.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/removeCollection'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_F2620/1/removeCollection
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_F2620/1/removeCollection',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Remove Object

`GET <URI>/remove`

Remove the object specified by the URI, and the references to that object.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/remove'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_F2620/1/remove
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_F2620/1/remove',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Replace Object

`GET <URI>/replace `

Remove the object specified from URI, but leave references to the object.

```javascript
const fetch = require("node-fetch");
const Url = 'https://synbiohub.org/public/igem/BBa_K1479017/1/replace'
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

```plaintext
curl -X GET -H "Accept: text/plain" -H "X-authorization: <token>" https://synbiohub.org/public/igem/BBa_F2620/1/remove
```

```python
import requests

response = requests.get(
    'https://synbiohub.org/public/igem/BBa_F2620/1/replace',
    params={'X-authorization': 'token'},
    headers={'Accept': 'text/plain'},
)

print(response.status_code)
print(response.content)

```

## Update Collection Icon

`POST <URI>/icon`

Updates the collection's icon.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:febee2f0-ea67-4b1b-a588-45b837f1c161" -F "CollectionIcon=@/home/Desktop/icon.png" localhost:7777/public/testid0/testid0_collection/1/icon
```

```python
import requests

response = requests.post(
    'http://localhost:7777/user/testuser/bruh/bruh_collection/1/addOwner',
    headers={
        'X-authorization': '47f48533-0b4f-4aa1-b410-2aa1f7235eae',
        'Accept': 'text/plain'
    },
    data={
        'user': 'jvscholz',
        'uri' : 'http://localhost:7777/user/testuser/bruh/bruh_collection/1/addOwner'
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = 'https://synbiohub.org/user/testuser/bruh2/bruh2_collection/1/addOwner'
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
filename | The desired collection icon.


# Permission Endpoints


The following endpoints are for managing permissions on SynBioHub.

<aside class="success">Note that the X-authorization header is required for all permission endpoints.</aside>

## Add Owner

`POST <URI>/addOwner`

Adds an owner to an object specfied by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "user=<user>&uri=<uri>" https://synbiohub.org/user/testuser/bruh/bruh_collection/1/addOwner
```

```python
import requests

response = requests.post(
    'http://localhost:7777/user/testuser/bruh/bruh_collection/1/addOwner',
    headers={
        'X-authorization': '47f48533-0b4f-4aa1-b410-2aa1f7235eae',
        'Accept': 'text/plain'
    },
    data={
        'user': 'jvscholz',
        'uri' : 'http://localhost:7777/user/testuser/bruh/bruh_collection/1/addOwner'
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = 'https://synbiohub.org/user/testuser/bruh2/bruh2_collection/1/addOwner'
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
user | The user id of owner being added.
uri | The identity of the object to add owner to.

## Remove Owner

`POST <URI>/removeOwner/<username>`

Removes an owner from an object specified  by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<>" -d "userUri=<>" http://synbiohub.org/public/:collectionId/:displayId/:version/removeOwner/:username
```

```python
import requests

response = requests.post(
    'http://localhost:7777/user/testuser/bruh/bruh_collection/1/removeOwner/:username',
    headers={
        'X-authorization': '47f48533-0b4f-4aa1-b410-2aa1f7235eae',
        'Accept': 'text/plain'
    },
    data={
        'userUri': 'jvscholz',
        },
)

print(response.status_code)
print(response.content)
```

```javascript
const fetch = require("node-fetch");
const { URLSearchParams } = require('url');
const url = 'https://synbiohub.org/user/testuser/bruh2/bruh2_collection/1/removeOwner/:username'
var headers={
    "Accept" : "text/plain; charset=UTF-8",
    "X-authorization" : "<token>"
};

const params = new URLSearchParams();
params.append('userUri', '<id>');



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

`POST <URI>/updateMutableDescription `

Edit the mutable description of an object specified by the URI. 

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "uri=<uri>&value=<value>" http://synbiohub.org/updateMutableDescription
```

```python
import requests

response = requests.post(
    'https://synbiohub.org/updateMutableDescription',
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
const url = 'https://synbiohub.org/updateMutableDescription'
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

`POST <URI>/updateMutableNotes`

Edit the mutable notes of an object specified by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "uri=<uri>&value=<value>" http://synbiohub.org/updateMutableNotes
```

```python

import requests

response = requests.post(
    'https://synbiohub.org/updateMutableNotes',
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
const url = 'https://synbiohub.org/updateMutableNotes'
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

`POST <URI>/updateMutableSource`

Edit the mutable source of an object specified by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<>" -d "uri=<>&value=<>" http://synbiohub.org/updateMutableSource
```

```python

import requests

response = requests.post(
    'https://synbiohub.org/updateMutableSource',
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
const url = 'https://synbiohub.org/updateMutableSource'
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

`POST <URI>/updateCitations`

Edit the citations of an object specified by the URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "uri=<uri>&value=<value>" http://synbiohub.org/updateCitations
```

```python

import requests

response = requests.post(
    'https://synbiohub.org/updateCitations',
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
const url = 'https://synbiohub.org/updateCitations'
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
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -d "previous=<previous>&object=<test>"  synbiohub.org/edit/:field
```

```python

```

```javascript

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
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "object=<object>"  synbiohub.org/add/:field
```

```python

```

```javascript

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
curl -X POST -H "Accept: text/plain" -H "X-authorization:<token>" -d "object=<object>"  synbiohub.org/remove/:field
```

```python

```

```javascript

```
Parameter | Description
--------- | -----------
object | The value of the field to remove.

Possible fields to remove:
`role`
`type`

# Attachment Endpoints

The following endpoints are for creating attachments on SynBioHub.

<aside class="success">Note that the X-authorization header is required for all attachment endpoints.</aside>

## Attach File

`POST <URI>/attach `

Attach a specified file to a given URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -F 'file=@<filename>' https://synbiohub.org/user/MyUserName/test/test_collection/1/attach
```

```python

```

```javascript

```

## Attach URL

`POST <URI>/attachURL `

Attach a specified URL to a given URI.

```plaintext
curl -X POST -H "Accept: text/plain" -H "X-authorization: <token>" -d "url=<url>&name=<name>&type=<type>" https://synbiohub.org/public/bruh2/bruh2_collection/1/attachURL

```

```python
import requests

response = requests.post(
    'https://synbiohub.org/public/bruh2/bruh2_collection/1/attachURL',
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
const url = 'https://synbiohub.org/public/bruh2/bruh2_collection/1/attachURL'
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

# Administration Endpoints

The following endpoints are for users with administration privileges.

<aside class="success">Note that the X-authorization header is required for all administration endpoints.</aside>

## SPARQL Admin Query

`GET <SynBioHub URL>/admin/sparql?query=<SPARQL query>`

Returns the results of the SPARQL admin query in JSON format.

```plaintext
curl -X GET -H "Accept: application/json" 'https://synbiohub.org/admin/sparql?query=select%20%3Fs%20%3Fp%20%3Fo%20where%20%7B%20%3Fs%20%3Fp%20%3Fo%20%7D
```


```python
import requests

response = requests.get(
    'https://synbiohub.org/admin/sparql?query=select%20%3Fs%20%3Fp%20%3Fo%20where%20%7B%20%3Fs%20%3Fp%20%3Fo%20%7D',
    params={'X-authorization': 'token'},
    headers={'Accept': 'application/json'},
)

print(response.status_code)

print(response.json())
```

## Admin

`GET <SynBioHub URL>/admin`

not sure what this does

## View Graphs

`GET <SynBioHub URL>/admin/graphs`

Returns existing graphs and its number of triples.

## View Log

`GET <SynBioHub URL>/admin/log`

Returns the log.

## View Current Mail Settings

`GET <SynBioHub URL>/admin/mail`

Returns the current mail configuration.

## Update Mail Settings

`POST <SynBioHub URL>/admin/mail`

Update the mail configuration.

## View Plugins

`GET <SynBioHub URL>/admin/plugins`

View current plugins.

## Save Plugin

`POST <SynBioHub URL>/admin/savePlugin`

Save a new plugin.

## Delete Plugin

`POST <SynBioHub URL>/admin/deletePlugin`

Delete a plugin.

## View Registries

`GET <SynBioHub URL>/admin/registries`

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

Update registries from Web Of Registries.

## Federate

`POST <SynBioHub URL>/admin/federate`

Send request to join Web-of-Registries for a SynBioHub.

## View Remotes

`GET <SynBioHub URL>/admin/remotes`

View current remotes.

## Save Remote

`POST <SynBioHub URL>/admin/saveRemote`

Save a new remote.

## Delete Remote

`POST <SynBioHub URL>/admin/deleteRemote`

Delete a new remote.

## View SBOLExplorer

`GET <SynBioHub URL>/admin/explorer`

View current SBOLExplorer Endpoint

## Update SBOLExplorer

`POST <SynBioHub URL>/admin/explorer`

Update SBOLExplorer endpoint.

## Explorer Update Index

`POST <SynBioHub URL>/admin/explorerUpdateIndex`

don't know

## View Theme

`GET <SynBioHub URL>/admin/theme`

View the current theme.

## Update Theme

`POST <SynBioHub URL>/admin/theme`

Update the theme.

## View Users

`GET <SynBioHub URL>/admin/users`

View the current users.

## Update Users

`POST <SynBioHub URL>/admin/users`

Update the user's settings.

## New User

post and get

don't know

## Update User

`POST <SynBioHub URL>/admin/updateUser`

don't know

## Delete User

`POST <SynBioHub URL>/admin/deleteUser`

Delete a user. 

# Plugins

### Rationale 

It should be possible for someone running a SynBioHub instance to configure plugins to extend the functionality of SynBioHub without altering the main codebase. 


### Goals
SynBioHub administrators should be able to:
Add processing steps to the submission pipeline
Add rendering steps construct pages


### Implementation
Plugins and SynBioHub will communicate using HTTP. The end user’s browser will not communicate with the plugin server.

To execute a plugin, SynBioHub will send an HTTP POST request to the plugin’s execute endpoint. The body of the request will contain a JSON object containing:
complete_sbol: the single-use URL for the complete object to operate on
shallow_sbol: the single-use URL for a summarized or truncated view of the object
top_level: the top-level URL of the SBOL object
instanceUrl: the top-level URL of the SBOL object 
size: a number representing an estimate of the size of the object, probably triple count

These pages needn’t be ready immediately upon responding to the SynBioHub plugin request. It should respond with an HTTP 503 error and the Retry-After header set to the number of seconds SynBioHub should wait before retrying.

### Submission
Intake plugins should interpret the complete_data object, which may not be in SBOL, and return a page containing SBOL. This page needn’t be ready immediately upon responding to the SynBioHub plugin request.

### Curation
Intake plugins should interpret the complete_data object, which will be in SBOL, and return a page containing SBOL. This page needn’t be ready immediately upon responding to the SynBioHub plugin request.

### Download
The response of download plugins should be a URL where the representation of the part can be downloaded. This page needn’t be ready immediately upon responding to the SynBioHub plugin request.

### Rendering
The response of rendering plugins should be HTML which will be displayed on the top-level page. Rendering responses may be cached to improve performance.


### Plugin Specification
Plugins must provide two endpoints, /status and /run. 

### Status endpoint
The status endpoint should listen for HTTP GET requests and return an HTTP 200 OK and (optionally) a short message if the plugin service is ready to handle requests. Otherwise, an HTTP error code and short error message should be returned. 

### Run endpoint
The run endpoint should function as described above in the Implementation section. 
 


  
