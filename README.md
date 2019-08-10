# Advanced Search on Twitter

![👀](https://repository-images.githubusercontent.com/200083171/7d2f7d80-b492-11e9-8f1b-4a5863429dca)

These operators work on Web, Mobile, Tweetdeck.

Adapted from [TweetDeck Help](https://help.twitter.com/en/using-twitter/advanced-tweetdeck-features), @lucahammer [Guide](https://freshvanroot.com/blog/2019/twitter-search-guide-by-luca/), @eevee [Twitter Manual](https://eev.ee/blog/2016/02/20/twitters-missing-manual/), @pushshift and Twitter / Tweetdeck itself. Contributions / tests, examples welcome!

Class | Operator | Finds Tweets…
-- | -- | --
Tweet content | love hatelove AND hate(love hate) | Containing both "love" and "hate"
  | love OR hate | At least one of either "love" or "hate"
  | -love | Excluding "love"
  | #tgif | A hashtag
  | $twtr | A cashtag, useful for following stock information
  | "love hate" | The complete phrase "love hate"
  | traffic ? | Question marks are matched
  | :) OR :( | Emoticons are matched, positive `:) :-) :P :D` or negative `:-( :(`
  | 👀 | Emoji searches are also matched
  | url:google.com | urls are tokenized and matched, works very well for subdomains and domains, not so well for long urls, depends on url. Youtube ids work well.
  | lang:en | Search for tweets in specified language, see [list](#supported-languages) below.
  |   |  
Users | from:user | Sent by a particular @username e.g. "#space from:NASA"
  | to:user | Replying to a particular @username
  | "+@user" | Mentioning a particular @username e.g. "+@NASA"
  | list:user/list-name | From members of this List e.g. list:NASA/space-tweets
  | filter:verified | From verified users
  |   |  
Geo | near:city | Geotagged in this place
  | near:me | Near where twitter thinks you are
  | within:radius | Within specific radius of the "near" operator, to apply a limit. Can use km or mi. e.g. "fire near:san-francisco within:10km"
  | geocode:lat,long,radius | E.g., to get tweets 10km around twitters hq, use geocode:37.7764685,-122.4172004,10km
  | place:96683cc9126741d1 | Search tweets by [Place Object](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/geo-objects.html#place) ID 
  |   |  
Time  | since:yyyy-mm-dd | On or after a specified date
  | until:yyyy-mm-dd | On or before a specified date. Combine with the "since" operator for dates between.
  | max_id:tweet_id | Snowflake ID based for exact time search (see [Note](#snowflake-ids) below) 
  | min_id:tweet_id | Does not work together with max_id
  |   |  
Tweet Type  | filter:nativeretweets | Retweets from users who have hit the retweet button
  | include:nativeretweets | Native retweets are excluded per default. This shows them.  
  | filter:retweets | Old style retweets ("RT") + quoted tweets.
  | filter:replies | Tweet is a reply to another Tweet.
  | filter:quote | Contain Quote Tweets 
  |   |  
Engagement  | min_retweets:5 | A minimum number of Retweets
  | min_faves:10 | A minimum number of Likes
  | min_replies:100 | A minimum number of replies
  | -min_retweets:500 | A maximum number of Retweets
  | -min_faves:1000 | A maximum number of Likes
  | -min_replies:1000 | A maximum number of replies
  |   |  
Media | filter:media | All media types.
  | filter:twimg | Native Twitter images (pic.twitter.com links)
  | filter:images | All images.
  | filter:video | All video types, including native Twitter video and external sources such as Youtube.
  | filter:periscope | Periscopes
  | filter:native_video | All Twitter-owned video types (native video, vine, periscope)
  | filter:vine | Vines (RIP)
  | filter:consumer_video | Twitter native video only
  | filter:pro_video | Twitter pro video (Amplify) only
  |   |  
More Filters | filter:follows | Only from accounts you follow
  | filter:links | Only containing some URL
  | filter:mentions | Containing `@mentions`
  | filter:news | Containing link to a news story. Combine with a list operator to narrow the user set down further.
  | filter:safe | Excluding NSFW content.
  | filter:hashtags | Only Tweets with Hashtags.
  |   |  
App specific | source:client_name | Sent from a specified client e.g. source:tweetdeck (See [Note](#common-clients) for common ones) `twitter_ads` doesn't work on it's own, but does with another operator.
  | card_domain:pscp.tv | Matches url in a Twitter Card. Maybe equivalent to `url:` for most links.
  | card_name:audio | Tweets with a Player Card (Links to Audio sources, Spotify, Soundcloud etc.)
  | card_name:animated_gif | Tweets With GIFs
  | card_name:player | Tweets with a Platyer Card
  | card_name:app | Tweets with links to an App Card
  | card_name:summary_large_image | Only large image Cards
  | card_name:summary | Only Small image summary cards

## Building Queries:

Any "`filter:type`" can also be negated using the "`-`" symbol. `exclude:links` is the same as `-filter:links`

Example: I want Tweets from @Nasa with all types of media except images

`from:NASA filter:media -filter:images`

Combine complex queries together with booleans and parentheses to refine your results.

Example 1: I want mentions of either "puppy" or "kitten", with mentions of either "sweet" or "cute", excluding Retweets, with at least 10 likes.

`(puppy OR kitten) AND (sweet OR cute) -filter:nativeretweets min_faves:10`

Example 2: I want mentions of "space" and either "big" or "large" by members of the NASA astronauts List, sent from an iPhone or twitter.com, with images, excluding mentions of #asteroid, since 2011.

`space (big OR large) list:nasa/astronauts (source:twitter_for_iphone OR source:twitter_web_client) filter:images since:2011-01-01 -#asteroid`

To find any quote tweets, search for the tweet permalink, or the tweet ID with `url` eg: `https://twitter.com/NASA/status/1138631847783608321` or `url:1138631847783608321`, see [note](#quote-tweets) for more.

On Web and mobile, keyword operators can match on: The user's name, the @ screen name, tweet text, and shortened, as well as expanded url text (eg, url:trib.al finds accounts that use that shortener, even though the full url is displayed).

Private accounts are not included in the search index, and their tweets do no appear in results. Locked and suspended accounts are also hidden from results. There are other situations where tweets may not appear: [anti-spam measures](https://help.twitter.com/en/rules-and-policies/enforcement-options), or tweets simply have not been indexed due to server issues. 

## Notes:

Web, Mobile, Tweetdeck Search runs on one type of system (as far as i can tell), Standard API Search is a different index, Premium Search and Enterprise Search is another separate thing based on Gnip products. API docs already exist for the API and Premium but i might add guides for those separately.

### Snowflake IDs:

All user, tweet, DM, and some other object IDs are snowflake IDs on twitter since `2010-06-01` and `2013-01-22` for user IDs. In short, each ID embeds a timestamp in it.

To use these with `min_id` / `max_id` as time delimiters, either pick a tweet ID that roughly has a `created_at` time you need, remembering that all times on twitter are UTC, or use the following (This works for all tweets after Snowflake was implemented):

To convert a Twitter ID to microsecond epoch:

`(tweet_id >> 22) + 1288834974657` -- This gives the millisecond epoch of when the tweet or user was created.

Convert from epoch back to a tweet id:

`(millisecond_epoch - 1288834974657) << 22 = tweet id`

Here's a use case:

You want to start gathering all tweets for specific search terms starting at a specific time. Let's say this time in `August 4, 2019 09:00:00 UTC`. You can use the `max_id` parameter by first converting the millisecond epoch time to a tweet id. You can use https://www.epochconverter.com.

`August 4, 2019 09:00:00 UTC` = `1564909200000` (epoch milliseconds)

`(1564909200000 - 1288834974657) << 22 = 1157939227653046272` (tweet id)

So if you set max_id to `1157939227653046272`, you will start collecting tweets earlier than that datetime. This can be extremely helpful when you need to get a very specific portion of the timeline.

Here's a quick Python function:

```python
def convert_milliepoch_to_tweet_id(milliepoch):
    if milliepoch <= 1288834974657:
        raise ValueError("Date is too early (before snowflake implementation)")
    return (milliepoch - 1288834974657) << 22
```

Unfortunately, remember that JavaScript does not support 64bit integers, so these calculations and other operations on IDs often fail in unexpected ways.

More details on snowflake can be found in @pushshift document [here](https://docs.google.com/document/d/1xVrPoNutyqTdQ04DXBEZW4ZW4A5RAQW2he7qIpTmG-M/).

### Quote-Tweets

From a technical perspective Quote-Tweets are Tweets with a URL of another Tweet. It's possible to find Tweets that quote a specific Tweet by searching for the URL of that Tweet. Any parameters need to be removed or only Tweets that contain the parameter as well are found. Twitter appends a Client-parameter when copying Tweet URLs through the sharing menu. Eg. ```?s=20``` for the Web App and ```?s=09``` for the Android app. Example: ```twitter.com/jack/status/20/ -from:jack```

To find all Tweets that quote a specific user, you search for the first part of the Tweet-URL and exclude Tweets from the user: ```twitter.com/jack/status/ -from:jack```.

### Geo Searches

Very few tweets have exact geo coordinates. Exact Geo coordinates are phased out for normal tweets, but will remain for photos: https://twitter.com/TwitterSupport/status/1141039841993355264

Tweets instead can be tagged by [Place](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/geo-objects.html#place)

### How did I find these in the first place?

Reading Twitter Documentation and help docs from as many sources as possible - eg: Developer Documentation, Help pages, Tool-specific help pages, eg: Tweetdeck help etc. Using Share feature on tweetdeck to copy the search string. Searching google and pastebin and github for rarely documented ones together to find other lists of operators others have compiled.

### Known Unknowns and Assumptions:

I have no idea how Twitter decides what should match `filter:news`, my guess is that it's based on a list of whitelisted domain names, as tweets from anyone can appear as long as they link to a news site, no idea if this list is public. No idea if or how this filter changed over time. But we can try to retrieve tweets and see. `lang:und` will match most empty tweets or tweets with a single number or link. `filter:safe` presumably uses the User setting "Contains Sensitive Content" - but may also apply to specific tweets somehow.

It would be great to be able to reliably find Promoted tweets - this may be possible with some of the card searches. Tweets composed in Twitter Ads are available with `source:twitter_ads` but other promoted tweets may not have been created with that app.

I'd also like to search for Collections (Timelines) and Moments, but this seems to work ok with just `url:` searches. eg: `url:twitter.com/i/events` and `url:twitter.com/i/moments` (I think the difference is events are curated?) but `url:twitter.com url:timelines` has many false positives.

### Supported Languages:

From TweetDeck dropdown menu:

<details><summary>All Languages</summary>
<p>

`lang:am` Amharic (አማርኛ)
`lang:ar` Arabic (العربية)
`lang:bg` Bulgarian (Български)
`lang:bn` Bengali (বাংলা)
`lang:bo` Tibetan (བོད་སྐད)
`lang:ca` Catalan (Català)
`lang:chr` Cherokee (ᏣᎳᎩ)
`lang:cs` Czech (čeština)
`lang:da` Danish (Dansk)
`lang:de` German (Deutsch)
`lang:dv` Maldivian (ދިވެހި)
`lang:el` Greek (Ελληνικά)
`lang:en` English (English)
`lang:es` Spanish (Español)
`lang:et` Estonian (eesti)
`lang:fa` Persian (فارسی)
`lang:fi` Finnish (Suomi)
`lang:fr` French (Français)
`lang:gu` Gujarati (ગુજરાતી)
`lang:hi` Hindi (हिंदी)
`lang:ht` Haitian Creole (Kreyòl ayisyen)
`lang:hu` Hungarian (Magyar)
`lang:hy` Armenian (Հայերեն)
`lang:in` Indonesian (Bahasa Indonesia)
`lang:is` Icelandic (Íslenska)
`lang:it` Italian (Italiano)
`lang:iu` Inuktitut (ᐃᓄᒃᑎᑐᑦ)
`lang:iw` Hebrew (עברית)
`lang:ja` Japanese (日本語)
`lang:ka` Georgian (ქართული)
`lang:km` Khmer (ខ្មែរ)
`lang:kn` Kannada (ಕನ್ನಡ)
`lang:ko` Korean (한국어)
`lang:lo` Lao (ລາວ)
`lang:lt` Lithuanian (Lietuvių)
`lang:lv` Latvian (latviešu valoda)
`lang:ml` Malayalam (മലയാളം)
`lang:my` Myanmar (မြန်မာဘာသာ)
`lang:ne` Nepali (नेपाली)
`lang:nl` Dutch (Nederlands)
`lang:no` Norwegian (Norsk)
`lang:or` Oriya (ଓଡ଼ିଆ)
`lang:pa` Panjabi (ਪੰਜਾਬੀ)
`lang:pl` Polish (Polski)
`lang:pt` Portuguese (Português)
`lang:ro` Romanian (limba română)
`lang:ru` Russian (Русский)
`lang:si` Sinhala (සිංහල)
`lang:sk` Slovak (slovenčina)
`lang:sl` Slovene (slovenski jezik)
`lang:sv` Swedish (Svenska)
`lang:ta` Tamil (தமிழ்)
`lang:te` Telugu (తెలుగు)
`lang:th` Thai (ไทย)
`lang:tl` Tagalog (Tagalog)
`lang:tr` Turkish (Türkçe)
`lang:uk` Ukrainian (українська мова)
`lang:ur` Urdu (ﺍﺭﺩﻭ)
`lang:vi` Vietnamese (Tiếng Việt)
`lang:zh` Chinese (中文)

</p>
</details>

Searching for `lang:chr`, `lang:iu`, `lang:sk` seems to fail, as tweets matching the keywords are returned instead of the language. 

### Common clients:
`source:` doesn't seem to work for any API client, these are the valid, indexed ones i could find:

`twitter_web_client`,
`twitter_for_iphone`,
`twitter_for_android`,
`tweetdeck`, 
`facebook`, 
`instagram`,
`twitterfeed`,
`cloudhopper` (tweets via sms service)
`tweetbot.net`
`IFTTT`
