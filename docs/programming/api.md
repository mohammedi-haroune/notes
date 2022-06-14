# REST APIs
kk
### Defintions
#### REST
REST is a well-defnied structural patterns for designing APIs that allows software to talk to each others, it doens't dictate any implementation details even the protocol but the most common is HTTP

REST is the name that has been given to the architectural style of HTTP itself, described by one of the
leading authors of the HTTP specifications. HTTP is the reality—REST is a set of design ideas that shaped
it.

REST is Data Oriented, it focuses on the underlying entities rather than the functions that manipulate them, being Data Oriented minimize the learning time because users can guess things easily especially if we are following standards, ex if the user know that there is a resource `/items` to get the list of items, the'll guess it also accepts a `POST` request with a new item and most probably individual items are accessible (GET, POST, PATCH, DELETE) at `/items/{id}`

#### Web API
a Web API is the set of representations (data), URLs, requests and responses (method, URL, headers and body) that allow the users to interact with our system.

#### RESTfull
RESTfull isn't well-defined and it most probably refers to the APIs that adhers the most to the REST patterns

#### HTTP
- HTTP is the most common protocol used for REST APIs, an HTTP reqeuest constitued of a 
  - Method: GET, POST, PATCH, PUT, DELETE ...
  - URL: where to the request
  - Headers: Content-Type, Authorization, we can define custom headers if you want
  - Body: what's in the request

#### Notes
- It's much more practicle to follow standard ways when we craft REST APIs, it help our experienced users to use their expertise of REST APIs and use our new APIs much more easily and it help new commers to learn something standard that'd befinical for them when dealing with other APIs

- Put yourself in the shoe of the user

- Use JSON, it's the most common representation used in REST APIs even though it only supports basic data types: int, string ... 

## API Design Elements
The following aspects of API design are all important, and together they define your API:
- **The representations of your resources:** this includes the definition of the fields in the resources
(assuming your resource representations are structured, rather than streams of bytes or characters),
and the links to related resources.
- **The use of standard** (and occasionally custom) HTTP headers.
- **The URLs and URI templates** that define the query interface of your API for locating resources based
on their data.
- **Required behaviors by clients:** for example, DNS caching behaviors, retry behaviors, tolerance of
fields that were not previously present in resources, and so on.


### Designing Representations
- Use JSON, keep it simple and human readable
- Include links in the resposne, this idea is sometimes called Hypermedia As The Engine Of
Application State, or HATEOAS
  - It allow a navigation experience like navigating in a browser
  - It's much more discoverable than having to hit the Documentation, understand it, guess how to construct the URL using the returned data and then write code (and debug it) that do it.

Two ways of include links, we can make them read-only or read-write, if they are read-only, it'll make our live easier, if they are read-write we have to deal with them appropritely 
