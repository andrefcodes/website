+++
title = 'Simple Jekyll Search'
description = 'A simple way to add a lightweight search to your static jekyll site'
date = 2022-06-09T16:20:47-03:00
tags = ['Jekyll', 'Static Site']
draft = false
+++

Since I created this website I missed the ability to search my posts on-site. With the help of {{< url "Simple Jekyll Search" "https://github.com/christian-fei/Simple-Jekyll-Search/" >}}, {{< url "this guide" "https://kevquirk.com/how-to-add-search-jekyll/" >}} written by Kev, and some minor tweaks, I've managed to add this feature.

I'm still getting used to have a personal blog, and this one doesn't have a lot of content as of today, what could justify not to add a search tool in it. But thinking long term, the content will eventually grow, and it'll come in hand, not only for the site's visitors, but myself.

With that in mind, I started looking for a tool capable of performing this functionality. I came across {{< url "Lunr" "https://lunrjs.com/guides/getting_started.html" >}}, Duckduck Go custom search engine, and Simple Jekyll Search.

For a number of reasons, it turned out that Simple Jekyll Search would fit my blog better, as long as I gave up the idea of not using JavaScript. 


It is worth mentioning that you will always have the option to access my posts in the [archive](/posts), which lists them chronologically

Kev's guide or the Simple jekyll search's repository are pretty straight forward if you want to go over the details on how to set it up yourself on your own website. I liked Kev's approach of specifying a search page, so we don't have to deal with `.js` in the whole site.

Some of the changes I've done differently from the original was adding the post description to the **searchResultTemplate**.

The **configuration** section should look like this:


```
<!-- Configuration -->
<script>
SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('results-container'),
    json: '/search.json',
    searchResultTemplate: '<li><h5 class="post-link"><a href="{url}">{title}</a></h5><p class="meta">{date}</p>{description}</li>'
})
</script>
```

Also, I've added the post's content to the **search.json** file, so  now we're able to also search for some specific content or paragraph that the `{title}` or the page `{description}` themselves doesn't have.

The **search.json** file should look like this:

```
---
---
[
  {% for post in site.posts %}
    {
      "title"    : "{{ post.title }}",
      "url"      : "{{ site.baseurl }}{{ post.url }}",
      "date"     : "{{ post.date | date: "%B %-d, %Y" }}",
      "description" : "{{ post.description | strip_html | strip_newlines | escape }}",
      "content" : "{{ post.content | strip_html | strip_newlines | escape }}"
    } {% unless forloop.last %},{% endunless %}
  {% endfor %}
]
```

and the **search.json** file after rendered, like this:

```
[
    {
      "title"    : "Andre Updates: May, 2022",
      "url"      : "/2022/06/01/updates/",
      "date"     : "June 1, 2022",
      "description" : "Everything that happened in the previous month and a few more updates",
      "content" : "This is an update of what happened in the previous month.    Photo by Markus Winkler on Unsplash📝 BloggingFor this month, I didn’t achieve my goal of writing a post per week, and I’m good with that.Even though I don’t keep any track about how many viewers my posts are hitting, I still have to deal with my mind that keeps telling me not to post anything that will be supposedly not interesting, nor has a supposedly good amount of words. It obviously goes against my main desire which is just to write about whatever I would like to.Below are the posts from the previous month:  Technology is evolving too fast  Remove metadata from photos on iphone  Is there a perfect keyboard?📖 ReadingI listed two articles that I found interesting and are, in my opinion, worth reading.Again there’s an article in Portuguese, I wouldn’t list the article if I didn’t like it, so if you feel like you want to read it, but don’t understand Portuguese, take this as an opportunity to learn, or just look for a translator.  The Movement to Ban Government Use of Face Recognition  Proteção nas redes: o que podemos fazer hoje pelos jovens - (en: Social media protection: What can we do today for the young people)🎵 Music and/or PodcastsToday, my recommendation is the album Back to Black (2006), by Amy Winehouse.His second album, Back to Black, has a strong influence of Soul and R&amp;amp;B, in addition to great songs. It manages to be sensitive and dancing at the same time.This is one of those discs you can’t find anything to say bad about.There’s no way you can listen to the album and not be delighted, even with sad songs, like rehab.  Back to Black by Amy Winehouse😎 OtherI’ve finished the Introduction to Computational Thinking and Data Science (MITx 6.00.2x) course that I mentioned last month.The course was insightful and showed lots of possibilities when working with data. Even with the high level of abstraction, due to this course be an introduction, I feel like I’m gaining more experience, and it keeps me motivated to keep learning.    ---    If you want to reply, have a suggestion or something else, feel free to contact me."
    } ,
  
    {
      "title"    : "Is there a perfect keyboard?",
      "url"      : "/2022/05/26/is-there-a-perfect-keyboard/",
      "date"     : "May 26, 2022",
      "description" : "Maybe the keyboard you were looking for is not the best keyboad. Actually, is there a perfect keyboard?",
      "content" : "Why are there so many keyboard sizes and layouts? Is it possible to find the perfect keyboard? Will we type better or faster with a “high-end” keyboard? Here’s my opinion:    75% Keyboard LayoutRecently, my wife needed a new keyboard because, for the hundredth time, some keys on her macbook started to fail. No news here about how Apple making laptops with more than horrible keyboards. I remember back in 2010 when I bought my first apple laptop, and it lasted for 10 years, while my 2018 has always shown some defect, be it in the battery, or in the keys. It’s a machine that I feel disgusted of using every day, but I’ll keep doing so until it finally takes its last breath, since money doesn’t grow on trees.Alright, back to my wife, she introduced me to keyboards that she found interesting, and they all had one trait in common. They were kind of fancy, had lots of lights, and she was recommended by one of those youtubers.I asked her to try out my keychron K2, and she liked really it. One of the things she appreciated most was it’s the width of her laptop’s keyboard. So I passed this keyboard on to her and bought another keyboard for me. It was her first contact with a mechanical keyboard, and it took her a few days to get used to it, but this is just an observation. I don’t want to lead you to believe that a mechanical keyboard is or isn’t better.Some people seem so obsessed with keyboards that they forget, in the end, what role this device will play in their day-by-day, be it an accountant, a programmer, a gamer, a student, and so on. Some companies put in some rgb lights on it, and voilà, this keyboard is perfect. In addition, I consider a disservice what some “content creators” do like making biased product reviews in exchange for some kind of compensation, mostly inducing people to buy products without considering other important aspects.Buying a keyboard, should be like buying an office chair. You’ve got to test it, find whether you like it or not, and only then buy it. Some questions should be asked, such as: do I suffer from a problem like tendinitis, or which layout best suits my use, or which keyboard will promote better ergonomics, or how many hours a day will I spend typing?Above I commented that I didn’t want to lead you to believe that a mechanical keyboard is better. For instance, if you have a problem with chronic hand pain due to repetitive strain, no mechanical keyboard will solve this problem. If you use the xpto switch, it won’t make you type faster or better. In the case of switches, for example, this is more related to the feeling on you fingers when typing and does not provide any significant improvement, unless if you are a gamer and need the actuation point to be faster. Probably this is one of the cases where you’ll might want to look for a keyboard that makes you pronate your hands less, or a ortholinear keyboard.In fact, most people suffer from using a keyboard due to bad habits and, perhaps, because there is a historical legacy linked to how the qwert layout was developed to optimize the working mechanism of the typewriters.    Keyboard Layout ComparisonWell, what are the most common keyboard layout types?The first is the old full size (or 100%). This keyboard derives from the IBM model M keyboard. The original consisted of a 101-keys keyboard that was divided into four basic groups:The first group were the alphabet and punctuation keys, followed by the middle group with the navigation keys and, for example, page up and page down, then the group of number keys, and finally the group of function keys located on the top.Basically, the full sized keyboard that we see today has 103 keys, where, in the 90s, with creation of windows 95, two super keys (or windows keys, as you will) were added, one on the left side, between the ctrl and alt, and one on the right.The second most popular keyboard layout is tenkeyless or tkl (80%).Already in the mid-90s, the use of computers for personal purposes or even for games, popularized the use of the mouse. Well, if you’ve ever used a full sized keyboard with a mouse, you must have already realized how uncomfortable it was to have to stretch your arm far away to use it. Unless you were a casual user, this could possibly have caused some pain or tension in your shoulder area.For this reason, and also because the number keys are so little used, there was a need to remove them. Hence, the name tenkeyless, which literally means “without ten keys”.The third type of layout, 75% keyboards, is perhaps the most popular type of keyboard out there, due to the fact that many people who have ever used a laptop, used one whose keys had this shape. For example, if you’ve ever used an apple macbook, you’ve used a keyboard 75%. Basically this keyboard is a tenkeyless keyboard but slightly reduced and with the navigation keys together the shift and ctrl keys on the right side.There are some laptops that bring the layout 100%, but generally this forces you to not be centered with the screen. For some people, this is not much big of deal, however I don’t particularly like it at all.One of the brands that became well known for offering keyboards in this format are, for example, the keychron with its k2 model (the same one that my wife uses at the moment). The keyboard model that I currently use is also a 75%, but in this case is the model 3084b from the brand akko. The reason I bought a different brand and not another keychron was the logistics and the price overall, because they’re quite similar in quality.Okay, let’s move on to the 65% model.This keyboard variant is very similar to the 75%, but the function keys have been removed. In this case, possibly this keyboard has a fn key in which you configure some shortcuts to have the same functionality as if had the missing keys. For me, since the function keys represent a full line of interesting shortcuts itself without having to configure any macros, I had a hard time adapting and went back to 75%.Well, from now onwards we start to get into a dark place. And you might also be wondering what the next step for a keyboard would be. The answer for me is that we’ve reached the limit, but some trailblazers have tried to reduce the model to 60% and have removed the navigation keys, as of the page up and page down keys as well.This model is, personally, the most confusing and requires a lot of mental effort to get used to, since it’s necessary to configure a series of shortcut functions to replace the missing keys, and I sincerely believe that this mental effort, especially when you use the keyboard for study or to work, does not pay off.I’ve commented in obscure places, so obviously I won’t go into the merits of keyboards that go beyond that, like the 55% or 40%.Alright, so after all, is it possible to get the perfect keyboard?Well, if you’ve read this far, you might have noticed that I really don’t believe that any keyboard layout you choose, be it a $10 plastic keyboard or fancy one with rgb lights and custom keycaps will make any keyboard perfect.In fact, most people’s problems are not the layout or the keyboard itself, but possibly never having trained to type using all their fingers.I myself remember being surprised to learn that those small bumps on the f and j keys serves to tactilely indicate where our index fingers, respectively, should be positioned.And here’s the moral of the story, always be seated in a comfortable position, grab the keyboard you like and just practice. This will make you type better, not the device necessarily. Basically our body tends to get used to repetitive movements - for example you hardly, unless you are speaking a foreign language, think to be able to speak. The same goes for when we type. If we type incorrectly, we end up encouraging our brain to continue a bad habit.There are even some very interesting sites to help you correct your typo. Some even have words in languages other than English:  Typing club  Monkey type  KeybrThat’s it for today…    ---    If you want to reply, have a suggestion or something else, feel free to contact me."
    }
]
```

Finally, this is a screenshot of a fully working lightweight searching tool.

{{< img "Website Screenshot" "/uploads/images/A6F3D3B5-BF82-46C5-B978-FBF241C1FA99.webp" >}}![](/assets)
*Website Screenshot*