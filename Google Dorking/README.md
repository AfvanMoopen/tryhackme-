# Google Dorking

Explaining how Search Engines work and leveraging them into finding hidden content!

[Google Dorking](https://tryhackme.com/room/googledorking)

## Topic's

* OSINT

## Ye Ol' Search Engine

Google is arguably the most famous example of “Search Engines”, I mean who remembers Ask Jeeves? shudders

Now it might be rather patronising explaining how these “Search Engines” work, but there’s a lot more going on behind the scenes then what we see. More importantly, we can leverage this to our advantage to find all sorts of things that a wordlist wouldn’t. Researching as a whole - especially in the context of Cybersecurity encapsulates almost everything you do as a pentester. [MuirlandOracle](https://tryhackme.com/p/MuirlandOracle) has created a [fantastic room](https://tryhackme.com/room/introtoresearch) on learning the attitudes towards how to research, and what information you can gain from it exactly.

"Search Engines" such as Google are huge indexers – specifically, indexers of content spread across the World Wide Web.

These essentials in surfing the internet use “Crawlers” or “Spiders” to search for this content across the World Wide Web, which I will discuss in the next task.

1. Roger dodger!

`No answer needed`

## Let's Learn About Crawlers

**What are Crawlers and how do They Work?**

These crawlers discover content through various means. One being by pure discovery, where a URL is visited by the crawler and information regarding the content type of the website is returned to the search engine. In fact, there are lots of information modern crawlers scrape – but we will discuss how this is used later. Another method crawlers use to discover content is by following any and all URLs found from previously crawled websites. Much like a virus in the sense that it will want to traverse/spread to everything it can.

**Let's Visualise Some Things...**

The diagram below is a high-level abstraction of how these web crawlers work. Once a web crawler discovers a domain such as **mywebsite.com**, it will index the entire contents of the domain, looking for keywords and other miscellaneous information - but I will discuss this miscellaneous information later.

![](https://i.imgur.com/4nrDDa0.png)

In the diagram above, "**mywebsite.com**" has been scraped as having the keywords as “Apple” “Banana" and “Pear”. These keywords are stored in a dictionary by the crawler, who then returns these to the search engine i.e. Google. Because of this persistence, Google now knows that the domain “**mywebsite.com**” has the keywords “Apple", “Banana” and “Pear”. As only one website has been crawled, if a user was to search for “Apple”...“**mywebsite.com**” would appear. This would result in the same behaviour if the user was to search for “Banana”. As the indexed contents from the crawler report the domain as having “Banana”, it will be displayed to the user.

As illustrated below, a user submits a query to the search engine of “Pears". Because the search engine only has the contents of one website that has been crawled with the keyword of “Pears” it will be the only domain that is presented to the user. 

![](https://i.imgur.com/nbbsAp4.png)

However, as we previously mentioned, **crawlers attempt to traverse, termed as crawling, every URL and file that they can find!** Say if “**mywebsite.com**” had the same keywords as before (“Apple", “Banana” and “Pear”), but also had a URL to another website “**anotherwebsite.com**”, the crawler will then attempt to traverse everything on that URL (**anotherwebsite.com**) and retrieve the contents of everything within that domain respectively.

This is illustrated in the diagram below. The crawler initially finds “**mywebsite.com**”, where it crawls the contents of the website - finding the same keywords (“Apple", “Banana” and “Pear”) as before, but it has additionally found an external URL. Once the crawler is complete on “**mywebsite.com**”, it'll proceed to crawl the contents of the website “anotherwebsite.com”, where the keywords ("Tomatoes", “Strawberries” and “Pineapples”) are found on it. The crawler's dictionary now contains the contents of both “**mywebsite.com**” and “**anotherwebsite.com**”, which is then stored and saved within the search engine.

![](https://i.imgur.com/CIM2c6N.png)

**Recapping**

So to recap, the search engine now has knowledge of two domains that have been crawled:

1. mywebsite.com
2. anotherwebsite.com

Although note that “**anotherwebsite.com**” was only crawled because it was referenced by the first domain “**mywebsite.com**”. Because of this reference, the search engine knows the following about the two domains:


| **Domain Name** | **Keyword** |
|---|---|
| mywebsite.com | Apples |
| mywebsite.com | Bananas |
| mywebsite.com | Pears |
| anotherwebsite.com | Tomatoes |
| anotherwebsite.com | Strawberries |
| anotherwebsite.com | Pineapples |

Or as illustrated below:

![](https://i.imgur.com/BJeI451.png)

Now that the search engine has some knowledge about keywords, say if a user was to search for “Pears” the domain “**mywebsite.com**” will be displayed - as it is the only crawled domain containing "Pears":

![](https://i.imgur.com/lBD6FPD.png)

Likewise, say in this case the user now searches for "Strawberries". The domain "**anotherwebsite.com**" will be displayed, as it is the only domain that has been crawled by the search engine that contains the keyword "Strawberries":

![](https://i.imgur.com/1LGoslC.png)

This is great...But imagine if a website had multiple external URL's (as they often do!) That'll require a lot of crawling to take place. There's always the chance that another website might have similar information as of that another website crawled - right? So how does the "Search Engine" decide on the hierarchy of the domains that are displayed to the user?

In the diagram below in this instance, if the user was to search for a keyword such as "Tomatoes" (which websites 1-3 contain) who decides what website gets displayed in what order?

![](https://i.imgur.com/OG2Fgsx.png)

A logical presumption would be that website 1 -> 3 would be displayed...But that's not how real-world domains work and/or are named.

So, who (or what) decides the hierarchy? Well...

1. Name the key term of what a "Crawler" is used to do

`Index`

2. What is the name of the technique that "Search Engines" use to retrieve this information about websites?

`Crawling`

3. What is an example of the type of contents that could be gathered from a website?

`Keywords`

## Enter: Search Engine Optimisation

**Search Engine Optimisation**

Search Engine Optimisation or SEO is a prevalent and lucrative topic in modern-day search engines. In fact, so much so, that entire businesses capitalise on improving a domains SEO “ranking”. At an abstract view, search engines will “prioritise” those domains that are easier to index. There are many factors in how “optimal” a domain is - resulting in something similar to a point-scoring system.

To highlight a few influences on how these points are scored, factors such as:

* How responsive your website is to the different browser types I.e. Google Chrome, Firefox and Internet Explorer - this includes Mobile phones!
* How easy it is to crawl your website (or if crawling is even allowed ...but we'll come to this later) through the use of "Sitemaps"
* What kind of keywords your website has (i.e. In our examples, if the user was to search for a query like “Colours” no domain will be returned - as the search engine has not (yet) crawled a domain that has any keywords to do with “Colours”

There is a lot of complexity in how the various search engines individually "point-score" or rank these domains - including vast algorithms. Naturally, the companies running these search engines such as Google don't share exactly how the hierarchic view of domains ultimately ends up. Although, as these are businesses at the end of the day, you can pay to advertise/boost the order of which your domain is displayed.

**Find a good example of how websites pay to boost their domains in the search listings**

There are various online tools - sometimes provided by the search engine providers themselves that will show you just how optimised your domain is. For example, let's use [SEO Site Checkup](https://seositecheckup.com/) to check the rating of [TryHackMe](https://tryhackme.com/):

![](https://i.imgur.com/Ge1sR6r.png)

According to this tool, TryHackMe is rated as **62/100** (as of 31/03/2020). That's not too bad and it'll show the justifications as to how this score was calculated below on the page.

**But...Who or What Regulates these "Crawlers"?**

Aside from the search engines who provide these "Crawlers", website/web-server owners themselves ultimately stipulate what content "Crawlers" can scrape. Search engines will want to retrieve **everything** from a website - but there are a few cases where we wouldn't want **all** of the contents of our website to be indexed! Can you think of any...? How about a secret administrator login page? We don't want **everyone** to be able to find that directory - especially through a google search.

Introducing Robots.txt... 

1. Using the SEO Site Checkup tool on "tryhackme.com", does TryHackMe pass the “Meta Title Test”? (Yea / Nay)

`Yea`

2. Does "tryhackme.com" pass the “Keywords Usage Test?” (Yea / Nay)

`Nay`

3. Use [https://neilpatel.com/seo-analyzer/](https://neilpatel.com/seo-analyzer/) to analyse [http://googledorking.cmnatic.co.uk](http://googledorking.cmnatic.co.uk/):

`No answer needed`

4. With the same tool and domain in Question #3 (previous): How many pages use “flash”

`0`

5. From a "rating score" perspective alone, what website would list first? tryhackme.com or googledorking.cmnatic.co.uk Use tryhackme.com's score of 62/100 as of 31/03/2020 for this question.

`googledorking.cmnatic.co.uk`

## Beepboop - Robots.txt

**Robots.txt**

Similar to "Sitemaps" which we will later discuss, this file is the first thing indexed by "Crawlers" when visiting a website.

**But what is it?**

This file must be served at the root directory - specified by the webserver itself. Looking at this files extension of **.txt**, its fairly safe to assume that it is a text file.

The text file defines the permissions the "Crawler" has to the website. For example, what type of "Crawler" is allowed (I.e. You only want Google's "Crawler" to index your site and not MSN's). Moreover, Robots.txt can specify what files and directories that we do or don't want to be indexed by the "Crawler".

A very basic markup of a Robots.txt is like the following:

![](https://i.imgur.com/wZ3lo4B.png)

Here we have a few keywords...

| Keyword | Function |
|---|---|
| User-agent | Specify the type of "Crawler" that can index your site (the asterisk being a wildcard, allowing all "User-agents" |
| Allow | Specify the directories or file(s) that the "Crawler" can index |
| Disallow | Specify the directories or file(s) that the "Crawler" cannot index |
| Sitemap | Provide a reference to where the sitemap is located (improves SEO as previously discussed, we'll come to sitemaps in the next task) |


In this case:

1. Any "Crawler" can index the site
2. The "Crawler" is allowed to index the entire contents of the site
3. The "Sitemap" is located at http://mywebsite.com/sitemap.xml

Say we wanted to hide directories or files from a "Crawler"? Robots.txt works on a "blacklisting" basis. Essentially, **unless told otherwise**, the Crawler will index whatever it can find.

![](https://i.imgur.com/audlFn8.png)

In this case:

1. Any "Crawler" can index the site
2. The "Crawler" can index every other content that isn't contained within "/super-secret-directory/".

Crawlers also know the differences between sub-directories, directories and files. Such as in the case of the second "Disallow:" ("/not-a-secret/but-this-is/")

The "Crawler" will index all the contents within "**/not-a-secret/**", but will not index anything contained within the sub-directory "**/but-this-is/**".

3. The "Sitemap" is located at http://mywebsite.com/sitemap.xml

**What if we Only Wanted Certain "Crawlers" to Index our Site?**

We can stipulate so, such as in the picture below:

![](https://i.imgur.com/LxitBJs.png)

In this case:

1. The "Crawler" "Googlebot" is allowed to index the entire site ("Allow: /")
2. The "Crawler" "msnbot" is not allowed to index the site (Disallow: /")

**How about Preventing Files From Being Indexed?**

Whilst you can make manual entries for every file extension that you don't want to be indexed, you will have to provide the directory it is within, as well as the full filename. Imagine if you had a huge site! What a pain...Here's where we can use a bit of [regexing](https://www.rexegg.com/regex-quickstart.html).

![](https://i.imgur.com/mzDqFVY.png)

In this case:

1. Any "Crawler" can index the site
2. However, the "Crawler" cannot index any file that has the extension of .ini within any directory/sub-directory using ("$") of the site.
3. The "Sitemap" is located at [http://mywebsite.com/sitemap.xml](http://mywebsite.com/sitemap.xml)

Why would you want to hide a **.ini** file for example? Well, files like this contain sensitive configuration details. Can you think of any other file formats that might contain sensitive information?

1. Where would "robots.txt" be located on the domain "ablog.com"

`ablog.com/robots.txt`

2. If a website was to have a sitemap, where would that be located?

`/sitemap.xml`

3. How would we only allow "Bingbot" to index the website?

`User-agent: Bingbot`

4. How would we prevent a "Crawler" from indexing the directory "/dont-index-me/"?

`Disallow: /dont-index-me/`

5. What is the extension of a Unix/Linux system configuration file that we might want to hide from "Crawlers"?

`.conf`

## Sitemaps

Comparable to geographical maps in real life, “Sitemaps” are just that - but for websites!

“Sitemaps” are indicative resources that are helpful for crawlers, as they specify the necessary routes to find content on the domain. The below illustration is a good example of the structure of a website, and how it may look on a "Sitemap":

![](https://i.imgur.com/L5WqJU4.png)

The blue rectangles represent the **route** to nested-content, similar to a directory I.e. “Products” for a store. Whereas, the green rounded-rectangles represent an actual page. However, this is for illustration purposes only - “Sitemaps” don't look like this in the real world. They look something much more similar to this:

![](https://i.imgur.com/12Yxcn5.png)

“Sitemaps” are XML formatted. I won't explain the structure of this file-formatting as the room [XXE](https://tryhackme.com/room/xxe) created by [falconfeast](https://tryhackme.com/p/falconfeast) does a mighty fine job of this.

The presence of "Sitemaps" holds a fair amount of weight in influencing the "optimisation" and favorability of a website. As we discussed in the "Search Engine Optimisation" task, these maps make the traversal of content much easier for the crawler!

**Why are "Sitemaps" so Favourable for Search Engines?**

Search engines are lazy! Well, better yet - search engines have a lot of data to process. The efficiency of how this data is collected is paramount. Resources like "Sitemaps" are extremely helpful for "Crawlers" as the necessary routes to content are already provided! All the crawler has to do is scrape this content - rather than going through the process of manually finding and scraping. Think of it as using a wordlist to find files instead of randomly guessing their names!

The easier a website is to "Crawl", the more optimised it is for the "Search Engine"

1. What is the typical file structure of a "Sitemap"?

`XML`

2. What real life example can "Sitemaps" be compared to?

`Map`

3. Name the keyword for the path taken for content on a website

`Route`

## What is Google Dorking?

**Using Google for Advanced Searching**

As we have previously discussed, Google has a lot of websites crawled and indexed. Your average Joe uses Google to look up Cat pictures (I'm more of a Dog person myself...). Whilst Google will have many Cat pictures indexed ready to serve to Joe, this is a rather trivial use of the search engine in comparison to what it can be used for.
For example, we can add operators such as that from programming languages to either increase or decrease our search results - or perform actions such as arithmetic!

![](https://i.imgur.com/hrfWM6i.png)

Say if we wanted to narrow down our search query, we can use quotation marks. Google will interpret everything in between these quotation marks as exact and only return the results of the exact phrase provided...Rather useful to filter through the rubbish that we don't need as we have done so below:

![](https://i.imgur.com/pJSW4ou.png)

**Refining our Queries**

We can use terms such as “**site**” (such as bbc.co.uk) and a query (such as "gchq news") to search the specified site for the keyword we have provided to filter out content that may be harder to find otherwise. For example, using the “site” and "query" of "bbc" and "gchq", we have modified the order of which Google returns the results.

In the screenshot below, searching for “gchq news” returns approximately 1,060,000 results from Google. The website that we want is ranked behind GCHQ's actual website:

![](https://i.imgur.com/b4duG6S.png)

But we don't want that...We wanted “**bbc.co.uk**” first, so let's refine our search using the “site” term. Notice how in the screenshot below, Google returns with much fewer results? Additionally, the page that we didn't want has disappeared, leaving the site that we did actually want!

![](https://i.imgur.com/dG3e64O.png)

Of course, in this case, GCHQ is quite a topic of discussion - so there'll be a load of results regardless.

**So What Makes "Google Dorking" so Appealing?**

First of all - and the important part - it's legal! It's all indexed, publicly available information. However, what you do with this is where the question of legality comes in to play...

A few common terms we can search and combine include:

| Term | Action |
|---|---|
| filetype: | Search for a file by its extension (e.g. PDF) |
| cache: | View Google's Cached version of a specified URL |
| intitle: | The specified phrase MUST appear in the title of the page |

For example, let's say we wanted to use Google to search for all PDFs on bbc.co.uk:

`site:bbc.co.uk filetype:pdf`

![](https://i.imgur.com/xoDtXnA.png)

Great, now we've refined our search for Google to query for all publically accessible PDFs on "**bbc.co.uk**" - You wouldn't have found files like this "Freedom of Information Request Act" file from a wordlist!

Here we used the extension PDF, but can you think of any other file formats of sensitive nature that **may** be publically accessible? (Often unintentionally!!) Again, what you do with any results that you find is where the legality comes into play - this is why "Google Dorking" is so great/dangerous.

Here is simple directory traversal.

**I have blanked out a lot of the below to cover you, me, THM and the owners of the domains:**

![](https://i.imgur.com/24OH1Kk.png)

![](https://i.imgur.com/o0Cnm1P.png)

1. What would be the format used to query the site bbc.co.uk about flood defences

`site: bbc.co.uk flood defences`

2. What term would you use to search by file type?

`filetype:`

3. What term can we use to look for login pages?

`intitle: login`
