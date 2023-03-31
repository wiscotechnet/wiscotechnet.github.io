# wiscotechnet.github.io

this repo uses github pages to serve the content to browsers. the default url is <https://wiscotechnet.github.io> and is secured using a github ssl certificate. no repo settings were changed from the defaults.

using several aws services this repo is also accessible via <https://www.wiscotech.net> and is secured using a aws ssl certificate.

## creating repo

using the official [documentation](https://pages.github.com/) a new repo was created named the same as the username of the account. further investigation reveals that you can use any repo name and enable this feature, but if the repo name is the same as the username then the pages feature is automatically enabled.

## aws

several aws services are required for this to work. most will be free for normal traffic, however you will need to register a domain with aws which has yearly fee's associated with it.

### register domain

using `route53` register a domain name (`wiscotech.net`). once the domain is available (could take some time) we can move on to the next steps (we will be returning to `route53` later).

### request wildcard ssl certificate

aws certificate manager (`acm`) provides free certificates. request a new one, and make sure the first domain name in the list is the root domain (`wiscotech.net`), and add an additional domain for wildcard support (`*.wiscotech.net`).

wait until the certificate is available before moving on, should only take a few minutes

### configure cloudfront distribution

create a new `cloudfront` distribution, and keep everything at the defaults except for the following:

* origin name:`wiscotechnet.github.io`
* origin domain:`wiscotechnet.github.io`
* alternate domain name:`www.wiscotech.net`
* ssl certificate:`select previously created certifcate from drop-down menu`
* viewer protocol policy:`redirect http to https`
* cache policy:`CachingOptimized`
* description:`serving wiscotechnet.github.io via www.wiscotech.net`

### create route53 record

create a new `A` record using `route53`, make sure to make it an `alias` and the use the drop-down menus to find and select the previously created `cloudfront` distribution.


## testing

once everything has propigated you should be able to access the repo using either <https://wiscotechnet.github.io/> or <https://www.wiscotech.net/>.

## notes

it looks like <https://wiscotech.net> also resolves to the repo. not sure why, but it's probably a browser feature.

using nslookup shows the following:

```shell
nslookup www.wiscotech.net
Server:  cdns01.comcast.net
Address:  2001:558:feed::1

Non-authoritative answer:
Name:    www.wiscotech.net
Addresses:13.35.116.16
          13.35.116.11
          13.35.116.6
          13.35.116.123
```

```shell
nslookup wiscotech.net
Server:  cdns01.comcast.net
Address:  2001:558:feed::1

Name:    wiscotech.net
```

## links

<https://medium.com/@bschandu67/host-your-website-using-github-pages-aws-route53-and-aws-cloudfront-7345493d2ea6>
<https://nimaeskandary.com/2018/02/11/setting-up-a-blog.html>