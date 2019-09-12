# Getting to grips with NGINX

This learning material is an introduction to working with NGINX.

Current status: **EVOLVING**

Contributions welcome!

## Pre-requisites and tips

* We'll be working with Docker from the command-line, you'll need to have Docker Desktop installed and be happy working with CLI tools.
* Exercises usually invlove making some NGNIX changes and testing their effect on web content that is served. Occasionally, you may find that browsers are helpfully unhelpful, e.g. automatically fixing urls and handling redirects, making it hard to reason about what is happening. In this case _curl_ is you friend - get used to ...

```
curl -i http://localhost:8080/
```

## Contents

1. [Getting started](/md/getting-started.md)
2. [Configuration](/md/config-files.md)
    1. [Enable request logging](/md/exercise-enable-request-logging.md)
    1. [Add error logging](/md/exercise-add-error-logging.md)
    1. [Change root web page](/md/exercise-change-root-web-page.md)
3. [Contexts](/md/contexts.md)
    1. [Locations and routing](/md/exercise-locations-and-routing.md)
    1. [Location matching](/md/exercise-location-matching.md)

_An Immediate Media Dev-Day Production_
