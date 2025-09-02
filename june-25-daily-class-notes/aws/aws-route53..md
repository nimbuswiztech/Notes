# AWS Route53.



So what is Route53, What is use of it? Why we use it?

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*hskjtrPfgFPqQek0LX8KZw.png" alt="" height="327" width="700"><figcaption></figcaption></figure>

Before we start with Route53, let’s understand what is **DNS(Domain Name System)**

We have two types of IP addresses IPv4 (32 bit field, apx 4 billion different addresses)& IPv6 (128bits).

if you want to know how DNS name resolves query, please check my below blog on same.

[How DNS (Domain Name System) Works and Queries Get ResolvedWhenever you type any domain name system for eg: http://whizlabs.com in your address bar of your browser. Browser will…levelup.gitconnected.com](https://levelup.gitconnected.com/how-does-dns-domain-name-system-query-gets-resolved-137a9e445ad8?source=post_page-----f3657b01ffa2---------------------------------------)

Let’s head over to our main topic i.e Route53.

> **In AWS, Route53 is global managed DNS (Domain Name System) & we already know DNS is a collection of rules and records which helps clients understand how to reach a server through URLs.**
>
> **DNS operates on port 53. Amazon decided to call it route 53 so that’s where the name comes from.**

It’s a global service. You need to buy a domain in order to work with Route53, Go to Route53 Service & Click on register domain. Enter the domain name & check availability, Add to cart & click on continue.

Route53 can use basically:

* Public domain names you own (or buy) or Private domain names that can be resolved by your instances in your VPCs.
* Route53 has many features such as **Load balancing, Health checks, Routing policy like Simple, Failover, Geolocation, Latency, Weighted, Multi value.**
* You pay $0.50 per month per hosted zone.

In AWS Route53, we have many types of records. Let’s talk about various records.

**1- SOA (Start of Authority Records)**

Basic SOA stores information about below things.

* Name of Server that supplied the data for zone.
* The administrator of that zone & current version of data file.

Eg:

> **ns-2048.awsdns-64.net. hostmaster.example.com. 1 7200 900 1209600 86400**\
> **Route53 Name server that create SOA record:** _ns-2048.awsdns-64.net._\
> **Email Address of Administrator:** hostmaster.example.com

**2- NS Record (Name Server Records)**

NS records is basically your name server records which are used by top level domain servers to direct traffic to content DNS server which contains the authoritative records.

> So whenever we create a hosted zone in Route53, Two types of records automatically created, one is SOA & second is NS.

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*DyQIQ4owP7QLyWHbX4RLXw.png" alt="" height="116" width="700"><figcaption><p>Hosted Zone(SOA &#x26; NS Records)</p></figcaption></figure>

Before we head over to important type of records used in AWS, let’s talk about one important concept TTL(Time to Live)

**TTL (Time to Live):**

TTL is mandatory for each DNS record. So TTL is length that a DNS records is cached on either the resolving server or user own Laptop. The Lower the TTL, the faster changes to DNS records. Whenever you created record set, you need to define TTL for it.

<figure><img src="https://miro.medium.com/v2/resize:fit:944/1*vdDeyVHsyM2WPH0a5HyUzw.png" alt="" height="192" width="472"><figcaption><p>TTL(Time to Live)</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*wEOc_Ehj4v4Q4brEeUHJeg.png" alt="" height="376" width="700"><figcaption><p>How Amazon Route 53 Routes Traffic for Your Domain (Source AWS Official)</p></figcaption></figure>

Let’s talk about the most common records in AWS.

When you create basic records, you specify the following values.

* Name
* Type
* Alias
* TTL (Time to Live)
* Value
* Routing Policy

**1- A Record ( URL to IPv4)**

The “A” record stands for Address record. The A record is used by computer to translate the name of the domain to an IP address.

Eg: (http://medium.com might point to [http://126.78.98.90)](http://126.78.98.90\)/)

**2- CNAME (Canonical Records- URL to URL)**

CNAME Points a URL to any other URL. (gaurav.gupta.com => gkg.example.com), We use it only for Non-Root Domain(aka. something.mydomain.com)

<figure><img src="https://miro.medium.com/v2/resize:fit:976/1*HZzp0pkBvlL2w-ujap-9XA.png" alt="" height="419" width="488"><figcaption><p>Resolving example.gaurav.com to newexample.gaurav.com</p></figcaption></figure>

**3- Alias Record:**

Alias record points a URL to an AWS Resource, Alias record are used to map resource record sets in your hosted zone to Elastic Load Balancer, CloudFront or S3 Buckets websites.

<figure><img src="https://miro.medium.com/v2/resize:fit:952/1*_8-LIsTbzeOsoBkm9XLVAg.png" alt="" height="598" width="476"><figcaption><p>Alias Record</p></figcaption></figure>

**4- AAAA: (URL to IPv6)**

An **AAAA record** maps a domain name to the IP address (Version 6) of the computer hosting the domain. An **AAAA record** is used to find the IP address of a computer connected to the internet from a name.

**5- MX Record (Main Exchange Record)**

A mail Exchanger **record** (**MX record**) specifies the mail server responsible for accepting email messages on behalf of a domain name. It is a resource **record** in the Domain Name System (**DNS**). It is possible to configure several **MX records**, typically pointing to an array of mail servers for load balancing and redundancy.

This is all about various type records, let’s talk about Routing policy. So what is routing policy.

> **When you create a record, you choose a routing policy, which determines how Amazon Route 53 responds to queries.**

<figure><img src="https://miro.medium.com/v2/resize:fit:872/1*nN-ACIbkow3c4Me0THDNsQ.png" alt="" height="109" width="436"><figcaption><p>Routing Policy</p></figcaption></figure>

There is total 6 types of routing policy in Route53, let’s talk about one by one.

**1- Simple Routing Policy:**

In case of simple routing policy, you can have only one record with multiple IP addresses. If you specify multiple values in record, Route53 returns all values in random order to the user.

Maps a domain to one URL, Use when you need to redirect to a single resource. You can’t attach health checks to simple routing policy. If multiple values are returned, a random one is chosen by the client.

<figure><img src="https://miro.medium.com/v2/resize:fit:952/1*JgGnY6fti3vhMVF8Wpuldw.png" alt="" height="414" width="476"><figcaption><p>Simple Routing Policy</p></figcaption></figure>

**2- Weighted Routing Policy**

Weighted Routing Policy controls the what percentage % of the requests that go to specific endpoint. It’s helpful to test 1% of traffic on new app version. It is also helpful to split traffic between two regions. We can associate Health checks with it.

Zoom image will be displayed

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*ouJuPCoanK8iNpDlSNUxGg.png" alt="" height="458" width="700"><figcaption><p>Weighted Policy</p></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:944/1*8vKPap8OovkpgKH3o3UiFA.png" alt="" height="713" width="472"><figcaption></figcaption></figure>

**3- Latency Routing Policy**

It allows you to route your traffic based on lowest network latency for your end user. It redirects to the server that has the least latency close to us also helpful when latency of users is a priority. Latency is evaluated in terms of user to designated AWS Region. For example: Germany may be directed to the US (if that’s the lowest latency)

<figure><img src="https://miro.medium.com/v2/resize:fit:902/1*OaLscyRsNhjM2jWaexSGTQ.png" alt="" height="719" width="451"><figcaption><p>Latency Routing Polciy</p></figcaption></figure>

**4- Failover Routing Policy**

**Failover routing** lets you **route** traffic to a resource when the resource is healthy or to a different resource when the first resource is unhealthy. The primary and secondary records can **route** traffic to anything from an Amazon S3 bucket that is configured as a website to a complex tree of records.

You can associate health check with this type of policy.

<figure><img src="https://miro.medium.com/v2/resize:fit:1234/1*9_j60Y0z0nF2ID8-FRUtaA.png" alt="" height="290" width="617"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:954/1*Aq9e8NyYnB7PMtXTwl1rCA.png" alt="" height="712" width="477"><figcaption><p>Failover Routing Policy</p></figcaption></figure>

**5- Geo Location Routing Policy**

**Geolocation routing** lets you choose the resources that serve your traffic based on the geographic location of your users, meaning the location that DNS queries originate from. For example, you might want all queries from Europe to be **routed** to an ELB load balancer in the Frankfurt region.

This is routing based on user location. Health check associated.

<figure><img src="https://miro.medium.com/v2/resize:fit:870/1*5FrIA7RjKg1tPjLJ33x7zQ.png" alt="" height="484" width="435"><figcaption><p>Geolocation Policy</p></figcaption></figure>

**6- Multi Value Routing Policy**

It helps distribute DNS responses across **multiple** resources. For example, use **multivalue** answer **routing** when you want to associate your **routing** records with a **Route** 53 health check.

Use multivalue answer routing when you need to return multiple values for a DNS query and route traffic to multiple IP addresses. Up to 8 healthy records are returned for each Multi Value query. Multi Value is not a substitute for having an ELB.

Zoom image will be displayed

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*Vlyqk1bkB0aDrM88lPSwIg.png" alt="" height="248" width="700"><figcaption></figcaption></figure>

<figure><img src="https://miro.medium.com/v2/resize:fit:922/1*6mYH2lr42K7JpXpEThMHuw.png" alt="" height="265" width="461"><figcaption><p>MultiValue Routing</p></figcaption></figure>

This is all about Route53 & it’s records & policy.

\
