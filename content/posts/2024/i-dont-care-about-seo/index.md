+++
title = "I Don't Care About Seo"
description = "I don't care about SEO and that's why quit it"
date = 2024-01-29T16:58:22-03:00
tags = ['Openweb']
draft = false
+++

I remember that when I first set up this site with Jekyll, I made sure to configure the site's metadata to make it easier for search engines to index it.

As much as I had reservations about this, I've always believed that scraping is an important way of disseminating knowledge. However, with this AI boom everywhere, and with a series of questions being raised about intellectual property, language model training, biases, and how some search engines scrape our content to provide answers for the user (often without giving credit or providing the source link), I see no reason to have my content indexed, leaving me to resort to other ways of disseminating content, such as my mastodon account, webring, blogrolls, etc.

I changed the `robots.txt` file on the website to

```yml
User-agent: *
Disallow: /
```

and changed the following meta tag on the page's `<head>` to

```html
<meta name='robots' content="noindex, nofollow, noimageindex, nosnippet">
```

I know, bots can ignore that, and technically there's nothing we can really do about it.

At the end of the day, I feel like this is more of a philosophical question than anything else. I mean, in the sense of how much we want tools like AI and "bad" algorithms to be an integral part of our lives.

The answers we give to questions like these dictate how much companies will want to keep developing or integrating these AI tools everywhere. In the search engine example, I just want a search engine, not an assistant who does everything for me (sometimes at the expense of other people's work).

Okay, maybe I went a little off topic or too far into it, but I think you get my idea.