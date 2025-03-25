+++
title = 'Styling My Rss Feed'
description = 'Describing how I styled my RSS Feed page'
date = 2024-01-14T16:58:22-03:00
tags = ['OpenWeb']
draft = false
+++

Recently, I came across a post by {{< url "Robb" "https://rknight.me/blog/styling-rss-and-atom-feeds/" "Robb Knight Website" >}} where he styled his RSS feed in a way that a person could read, instead of a machine readable `.xml` file.

Styling the RSS feed is possible with the help of XSLT (Extensible Stylesheet Language Transformations), which transforms our `.xml` file into other document types, such as HTML.

First, I added an `xml-stylesheet` processing instruction inside my `feed.xml` file. Right below the line containing the xml version.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet href="/feed.xsl" type="text/xsl"?>
```

Then, in the same directory as my `feed.xml` file, I created a file called **`feed.xsl`** and added the following content.

```xml
---
layout:
title: 'RSS Feed'
permalink: "/feed.xsl"
---

<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="3.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:atom="http://www.w3.org/2005/Atom" xmlns:dc="http://purl.org/dc/elements/1.1/">
  <xsl:output method="html" version="1.0" encoding="UTF-8" indent="yes"/>
  <xsl:template match="/">
    <html xmlns="http://www.w3.org/1999/xhtml" lang="en">
      <head>
        <title>{{ page.title }} - {{ site.title }}</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
        <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
        <meta name="color-scheme" content="dark light"/>
        {% include style.html %}
      </head>
      <body>
        {% include header.html %}
        <main>
            <h1>
                <!-- Feed-icon.svg: unnamed (Mozilla Foundation)User:Jahoe (GPL <http://www.gnu.org/licenses/gpl.html>, LGPL <http://www.gnu.org/licenses/lgpl.html>, MPL 1.1 <https://www.mozilla.org/MPL/1.1/>, MPL 1.1 <https://www.mozilla.org/MPL/1.1/>, GPL <http://www.gnu.org/licenses/gpl.html> or LGPL <http://www.gnu.org/licenses/lgpl.html>), via Wikimedia Commons -->
                <svg xmlns="http://www.w3.org/2000/svg" version="1.1" style="vertical-align: text-bottom; width: 1.2em; height: 1.2em;" id="RSSicon" viewBox="0 0 8 8">
                <title>RSS feed icon</title>
                <style type="text/css">
                  .button {stroke: none; fill: orange;}
                  .symbol {stroke: none; fill: white;}
                </style>
                <rect   class="button" width="8" height="8" rx="1.5" />
                <circle class="symbol" cx="2" cy="6" r="1" />
                <path   class="symbol" d="m 1,4 a 3,3 0 0 1 3,3 h 1 a 4,4 0 0 0 -4,-4 z" />
                <path   class="symbol" d="m 1,2 a 5,5 0 0 1 5,5 h 1 a 6,6 0 0 0 -6,-6 z" />
              </svg>
              Web Feed
            </h1>
          <p class="tip">
            <strong>This is a web feed,</strong> also known as an RSS feed. <strong>Subscribe</strong> by copying the URL <mark>https://abf.li/feed.xml</mark> into your newsreader.
          </p>
          <table>
            <thead>
              <tr>
                <th>Latest Posts</th>
                <th style="text-align: center;">Published</th>
              </tr>
            </thead>
            <tbody>
              <xsl:for-each select="/rss/channel/item">
                <tr>
                  <td>
                    <a target="_blank">
                      <xsl:attribute name="href">
                        <xsl:value-of select="link"/>
                      </xsl:attribute>
                      <xsl:value-of select="title"/>
                    </a>
                  </td>
                  <td style="text-align: center; white-space: nowrap; width: 50px;">
                    <xsl:value-of select="substring(pubDate, 0, 18)" />
                  </td>
                </tr>
              </xsl:for-each>
            </tbody>
          </table>
        </main>
        <footer>
          <p>
            {{ site.author.copy}} <a href="{{ site.author.mastodon }}" rel="me noopener" target="_blank">{{ site.author.name }}</a>
            ~ Content licensed under <a href="{{ site.license_url }}" rel="noopener noreferrer nofollow" target="_blank">{{ site.license }}</a>.<br />
            Last modified: {{ site.time | date: "%B %-d, %Y" }}.
          </p>
        </footer>
      </body>
    </html>
  </xsl:template>
</xsl:stylesheet>
```

This code was based on {{< url "Pretty Feed v3" "https://github.com/genmon/aboutfeeds/blob/main/tools/pretty-feed-v3.xsl" "Aboutfeeds - Pretty Feed" >}}, but I removed all the css that was adding unnecessary load, and then I simplified it so that the page shows just the site header, a banner about the web feed, and a table with the posts.

This is the before and after:

{{< img "Website Screenshot" "dd9cbdae-5073-49f8-8a77-ab9050367b06.webp" >}}
*Before*

{{< img "New Website Screenshot" "2f11be8e-98ce-4d35-9b15-17a9287e9a57.webp" >}}
*After*

These are nice people who have also styled their feed:

* {{< url "Darek Kay" "https://darekkay.com/atom.xml" "Darek Kay Feed" >}}
* {{< url "Simone Silvestroni" "https://minutestomidnight.co.uk/feed.xml" "Simone Silvestroni Feed" >}}
* {{< url "Robb Knight" "https://rknight.me/subscribe/posts/rss.xml" "Robb Knight Feed" >}}