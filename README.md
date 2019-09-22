# Advanced Search on Twitter

![👀](https://repository-images.githubusercontent.com/200083171/7d2f7d80-b492-11e9-8f1b-4a5863429dca)

These operators work on Web, Mobile, Tweetdeck.

Adapted from [TweetDeck Help](https://help.twitter.com/en/using-twitter/advanced-tweetdeck-features), @lucahammer [Guide](https://freshvanroot.com/blog/2019/twitter-search-guide-by-luca/), @eevee [Twitter Manual](https://eev.ee/blog/2016/02/20/twitters-missing-manual/), @pushshift and Twitter / Tweetdeck itself. Contributions / tests, examples welcome!

Class | Operator | Finds Tweets… | Eg:
-- | -- | -- | --
Tweet content | `nasa esa` <br> `(nasa esa)` | Containing both "nasa" and "esa". Spaces are implicit AND. Brackets can be used to group individual words if using other operators. | [🔗](https://twitter.com/search?q=esa%20nasa&src=typed_query)
  | `nasa OR esa` | Either "nasa" or "esa". OR must be in uppercase. | [🔗](https://twitter.com/search?q=nasa%20OR%20esa&src=typed_query)
  | `"state of the art"` | The complete phrase "state of the art". Will also match "state-of-the-art" | [🔗](https://twitter.com/search?q=%22state%20of%20the%20art%22&src=typed_query)
  | `-love` <br> `-"live laugh love"` | `-` is used for excluding "love". Also applies to quoted phrases and other operators. | [🔗](https://twitter.com/search?q=bears%20-chicagobears&src=typed_query)
  | `#tgif` | A hashtag | [🔗](https://twitter.com/search?q=%23tgif&src=typed_query)
  | `$TWTR` | A cashtag, like hashtags but for stock symbols | [🔗](https://twitter.com/search?q=%24TWTR%20OR%20%24FB%20OR%20%24AMZN%20OR%20%24AAPL%20OR%20%24NFLX%20OR%20%24GOOG&src=typed_query)
  | `What ?` | Question marks are matched | [🔗](https://twitter.com/search?q=(Who%20OR%20What%20OR%20When%20OR%20Where%20OR%20Why%20OR%20How)%20%3F&src=typed_query)
  | `:) OR :(` | Some emoticons are matched, positive `:) :-) :P :D` or negative `:-( :(` | [🔗](https://twitter.com/search?q=%3A%29%20OR%20%3A-%29%20OR%20%3AP%20OR%20%3AD%20OR%20%3A%28%20OR%20%3A-%28&src=typed_query)
  | 👀 | Emoji searches are also matched. Usually needs another operator to work. | [🔗](https://twitter.com/search?q=%F0%9F%91%80%20lang%3Aen&src=typed_query) 
  | `url:google.com` | urls are tokenized and matched, works very well for subdomains and domains, not so well for long urls, depends on url. Youtube ids work well. Works for both shortened and canonical urls, eg: gu.com shortener for theguardian.com | [🔗](https://twitter.com/search?q=url%3Agu.com&src=typed_query) 
  | `lang:en` | Search for tweets in specified language, see [list](#supported-languages) below. | [🔗](https://twitter.com/search?q=lang%3Aen&src=typed_query)
  |   |  
Users | `from:user` | Sent by a particular `@username` e.g. `"dogs from:NASA"` | [🔗](https://twitter.com/search?q=dogs%20from%3Anasa&src=typed_query)
  | `to:user` | Replying to a particular `@username` | [🔗](https://twitter.com/search?q=%23MoonTunes%20to%3Anasa&src=typed_query)
  | `@user` | Mentioning a particular `@username`. Combine with `-from:username` to get only mentions | [🔗](https://twitter.com/search?q=%40cern%20-from%3Acern&src=typed_query)
  | `list:user/list-slug` | From members of this public List e.g. list:NASA/astronauts. Use the list slug (url part after `/lists/`) | [🔗](https://twitter.com/search?q=list%3ANASA%2Fastronauts&src=typed_query)
  | `filter:verified` | From verified users | [🔗](https://twitter.com/search?q=filter%3Averified&src=typed_query)
  |   |  
Geo | `near:city` | Geotagged in this place. Also supports Phrases, eg: "The Hague" | [🔗](https://twitter.com/search?q=near%3A%22The%20Hague%22&src=typed_query)
  | `near:me` | Near where twitter thinks you are | [🔗](https://twitter.com/search?q=near%3Ame&src=typed_query)
  | `within:radius` | Within specific radius of the "near" operator, to apply a limit. Can use km or mi. e.g. `fire near:san-francisco within:10km` | [🔗](https://twitter.com/search?q=fire%20near%3Asan-francisco%20within%3A10km&src=typed_query)
  | `geocode:lat,long,radius` | E.g., to get tweets 10km around twitters hq, use `geocode:37.7764685,-122.4172004,10km` | [🔗](https://twitter.com/search?q=geocode%3A37.7764685%2C-122.4172004%2C10km&src=typed_query)
  | `place:96683cc9126741d1` | Search tweets by [Place Object](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/geo-objects.html#place) ID eg: USA Place ID is `96683cc9126741d1` | [🔗](https://twitter.com/search?q=place%3A96683cc9126741d1&src=typed_query)
  |   |  
Time  | `since:yyyy-mm-dd` | On or after a specified date | [🔗](https://twitter.com/search?q=since%3A2019-06-12%20until%3A2019-06-28%20%23nasamoontunes&src=typed_query)
  | `until:yyyy-mm-dd` | On or before a specified date. Combine with the "since" operator for dates between. | [🔗](https://twitter.com/search?q=since%3A2019-06-12%20until%3A2019-06-28%20%23nasamoontunes&src=typed_query)
  | `max_id:tweet_id` | Snowflake ID based for exact time search (see [Note](#snowflake-ids) below) | [🔗](https://twitter.com/search?q=since_id%3A1138872932887924737%20max_id%3A1144730280353247233%20%23nasamoontunes&src=typed_query)
  | `since_id:tweet_id` | Works together with `max_id` and another operator | [🔗](https://twitter.com/search?q=since_id%3A1138872932887924737%20max_id%3A1144730280353247233%20%23nasamoontunes&src=typed_query)
  |   |  
Tweet Type  | `filter:nativeretweets` | Only retweets created using the retweet button. Works well combined with `from:` to show only retweets. | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Anativeretweets&src=typed_query)
  | `include:nativeretweets` | Native retweets are excluded by default. This shows them. In contrast to `filter:`, which shows only retweets, this includes retweets in addition to other tweets | [🔗](https://twitter.com/search?q=from%3Anasa%20include%3Anativeretweets%20&src=typed_query)
  | `filter:retweets` | Old style retweets ("RT") + quoted tweets. | [🔗](https://twitter.com/search?q=filter%3Aretweets%20from%3Atwitter%20until%3A2009-11-06%09&src=typed_query&f=live)
  | `filter:replies` | Tweet is a reply to another Tweet. good for finding conversations, or threads if you add or remove `to:user` | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Areplies%20-to%3Anasa&src=typed_query)
  | `filter:quote` | Contain Quote Tweets | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Aquote&src=typed_query)
  | `card_name:poll2choice_text_only` | Tweets containing polls. `poll2choice_text_only` for polls containing two choices, `poll3choice_text_only` for polls with three choices and `poll4choice_text_only` for polls with four choices. | [🔗](https://twitter.com/search?q=card_name%3Apoll4choice_text_only&src=typed_query&f=live)
  |   |  
Engagement  | `min_retweets:5` | A minimum number of Retweets. Counts seem to be approximate for larger (1000+) values. | [🔗](https://twitter.com/search?q=min_retweets%3A5000%20nasa&src=typed_query)
  | `min_faves:10` | A minimum number of Likes | [🔗](https://twitter.com/search?q=min_faves%3A10000%20nasa&src=typed_query)
  | `min_replies:100` | A minimum number of replies | [🔗](https://twitter.com/search?q=min_replies%3A1000%20nasa&src=typed_query)
  | `-min_retweets:500` | A maximum number of Retweets | [🔗](https://twitter.com/search?q=-min_retweets%3A500%20nasa&src=typed_query)
  | `-min_faves:500` | A maximum number of Likes | [🔗](https://twitter.com/search?q=-min_faves%3A500%20nasa&src=typed_query)
  | `-min_replies:100` | A maximum number of replies | [🔗](https://twitter.com/search?q=-min_replies%3A100%20nasa&src=typed_query)
  |   |  
Media | `filter:media` | All media types. | [🔗](https://twitter.com/search?q=filter%3Amedia%20cat&src=typed_query)
  | `filter:twimg` | Native Twitter images (pic.twitter.com links) | [🔗](https://twitter.com/search?q=filter%3Atwimg%20cat&src=typed_query)
  | `filter:images` | All images. | [🔗](https://twitter.com/search?q=filter%3Aimages%20cat&src=typed_query)
  | `filter:videos` | All video types, including native Twitter video and external sources such as Youtube. | [🔗](https://twitter.com/search?q=filter%3Avideos%20cat&src=typed_query)
  | `filter:periscope` | Periscopes | [🔗](https://twitter.com/search?q=filter%3Aperiscope%20cat&src=typed_query)
  | `filter:native_video` | All Twitter-owned video types (native video, vine, periscope) | [🔗](https://twitter.com/search?q=filter%3Anative_video%20cat&src=typed_query)
  | `filter:vine` | Vines (RIP) | [🔗](https://twitter.com/search?q=filter%3Avine%20cat&src=typed_query)
  | `filter:consumer_video` | Twitter native video only | [🔗](https://twitter.com/search?q=filter%3Aconsumer_video%20cat&src=typed_query)
  | `filter:pro_video` | Twitter pro video (Amplify) only | [🔗](https://twitter.com/search?q=filter%3Apro_video%20cat&src=typed_query)
  |   |  
More Filters | `filter:follows` | Only from accounts you follow | [🔗](https://twitter.com/search?q=filter%3Afollows%20lang%3Aen&src=typed_query)
  | `filter:links` | Only containing some URL, includes media. use -filter:media for urls that aren't media | [🔗](https://twitter.com/search?q=filter%3Afollows%20filter%3Alinks%20-filter%3Amedia&src=typed_query)
  | `filter:mentions` | Containing any sort of `@mentions` | [🔗](https://twitter.com/search?q=filter%3Amentions%20from%3Atwitter%20-filter%3Areplies&src=typed_query)
  | `filter:news` | Containing link to a news story. Combine with a list operator to narrow the user set down further. | [🔗](https://twitter.com/search?q=filter%3Anews%20lang%3Aen&src=typed_query)
  | `filter:safe` | Excluding NSFW content. Excludes content that users have marked as "Potentially Sensitive". Doesn't always guarantee SFW results. | [🔗](https://twitter.com/search?q=filter%3Asafe%20%23followfriday&src=typed_query)
  | `filter:hashtags` | Only Tweets with Hashtags. | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Ahashtags&src=typed_query)
  |   |  
App specific | `source:client_name` | Sent from a specified client e.g. source:tweetdeck (See [Note](#common-clients) for common ones) eg: `twitter_ads` doesn't work on it's own, but does with another operator. | [🔗](https://twitter.com/search?q=source%3A%22GUCCI%20SmartToilet%E2%84%A2%22%20lang%3Aen&src=typed_query)
  | `card_domain:pscp.tv` | Matches url in a Twitter Card. Maybe equivalent to `url:` for most links. | [🔗](https://twitter.com/search?q=card_domain%3Apscp.tv&src=typed_query)
  | `card_name:audio` | Tweets with a Player Card (Links to Audio sources, Spotify, Soundcloud etc.) | [🔗](https://twitter.com/search?q=card_name%3Aaudio&src=typed_query)
  | `card_name:animated_gif` | Tweets With GIFs | [🔗](https://twitter.com/search?q=card_name%3Aanimated_gif&src=typed_query)
  | `card_name:player` | Tweets with a Player Card | [🔗](https://twitter.com/search?q=card_name%3Aplayer&src=typed_query)
  | `card_name:app` | Tweets with links to an App Card | [🔗](https://twitter.com/search?q=card_name%3Aapp&src=typed_query)
  | `card_name:summary_large_image` | Only large image Cards | [🔗](https://twitter.com/search?q=card_name%3Asummary_large_image&src=typed_query)
  | `card_name:summary` | Only Small image summary cards | [🔗](https://twitter.com/search?q=card_name%3Asummary&src=typed_query)

## Matching:

On web and mobile, keyword operators can match on: The user's name, the @ screen name, tweet text, and shortened, as well as expanded url text (eg, `url:trib.al` finds accounts that use that shortener, even though the full url is displayed).

By default "Top" results are shown, where "Top" means tweets with some engagements (replies, RTs, likes). "Latest" has most recent tweets. People search will match on descriptions, but not all operators work. "Photos" and "Videos" are presumably equivalent to `filter:images` and `filter:videos`.

Exact Tokenization is not known, but it's most likely a custom one to preserve entities. URLs are also tokenized. Spelling correction appears sometimes, and also plurals are also matched, eg: `bears` will also match tweets with `bear`. `-` not preceeding an operator are removed, so "state-of-the-art" is the same as "state of the art".

Private accounts are not included in the search index, and their tweets do no appear in results. Locked and suspended accounts are also hidden from results. There are other situations where tweets may not appear: [anti-spam measures](https://help.twitter.com/en/rules-and-policies/enforcement-options), or tweets simply have not been indexed due to server issues. 

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

### Limitations:

Known limitations: `card_name:` only works for the last 7-8 days.

The maximum number of operators seems to be about 22 or 23.

### Tweetdeck Equivalents:

Tweetdeck options for columns have equivalents you can use on web search:

- Tweets with Images: `filter:images` 
- Videos: `filter:videos`
- Tweets with GIFs: `card_name:animated_gif` 
- "Tweets with broadcasts": `(card_domain:pscp.tv OR card_domain:periscope.tv OR "twitter.com/i/broadcasts/")`
- "Any Media" `(filter:images OR filter:videos)` 
- "Any Links (includes media)": `filter:links` 

## Notes:

Web, Mobile, Tweetdeck Search runs on one type of system (as far as i can tell), Standard API Search is a different index, Premium Search and Enterprise Search is another separate thing based on Gnip products. API docs already exist for the API and Premium but i might add guides for those separately.

### Snowflake IDs:

All user, tweet, DM, and some other object IDs are snowflake IDs on twitter since `2010-06-01` and `2013-01-22` for user IDs. In short, each ID embeds a timestamp in it.

To use these with `since_id` / `max_id` as time delimiters, either pick a tweet ID that roughly has a `created_at` time you need, remembering that all times on twitter are UTC, or use the following (This works for all tweets after Snowflake was implemented):

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

From a technical perspective Quote-Tweets are Tweets with a URL of another Tweet. It's possible to find Tweets that quote a specific Tweet by searching for the URL of that Tweet. Any parameters need to be removed or only Tweets that contain the parameter as well are found. Twitter appends a Client-parameter when copying Tweet URLs through the sharing menu. Eg. `?s=20` for the Web App and `?s=09` for the Android app. Example: `twitter.com/jack/status/20/ -from:jack`

To find all Tweets that quote a specific user, you search for the first part of the Tweet-URL and exclude Tweets from the user: `twitter.com/jack/status/ -from:jack`.

### Geo Searches

Very few tweets have exact geo coordinates. Exact Geo coordinates are phased out for normal tweets, but will remain for photos: https://twitter.com/TwitterSupport/status/1141039841993355264

Tweets instead can be tagged by [Place](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/geo-objects.html#place)

### How did I find these in the first place?

Reading Twitter Documentation and help docs from as many sources as possible - eg: Developer Documentation, Help pages, Tool-specific help pages, eg: Tweetdeck help etc. Using Share feature on tweetdeck to copy the search string. Searching google and pastebin and github for rarely documented ones together to find other lists of operators others have compiled.

### Known Unknowns and Assumptions:

I have no idea how Twitter decides what should match `filter:news`, my guess is that it's based on a list of whitelisted domain names, as tweets from anyone can appear as long as they link to a news site, no idea if this list is public. No idea if or how this filter changed over time. But we can try to retrieve tweets and see. `lang:und` will match most empty tweets or tweets with a single number or link. `filter:safe` presumably uses the User setting "Contains Sensitive Content" - but may also apply to specific tweets somehow.

It would be great to be able to reliably find Promoted tweets - this may be possible with some of the card searches. Tweets composed in Twitter Ads are available with `source:twitter_ads` but other promoted tweets may not have been created with that app.

I'd also like to search for Collections (Timelines) and Moments, but this seems to work ok with just `url:` searches. eg: `url:twitter.com/i/events` and `url:twitter.com/i/moments` (I think the difference is events are curated?) but `url:twitter.com url:timelines` has many false positives.

In Search Settings, "Hide Sensitive Content" equivalent is `filter:safe` - is there an equivalent to "Remove Blocked and Muted Accounts"?

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

`source:` should work for any API client, try putting the client name in quotes or replace spaces with underscores. This is the App name field that you can alter in the [developer app configuration page](https://developer.twitter.com/en/apps), so anyone can set anything here and appear to tweet from a made up client. You cannot copy an existing name. This operator needs to be combined with something else to work, eg: `lang:en` These are some common ones:

`twitter_web_client`, 
`twitter_for_iphone`, 
`twitter_for_android`, 
`twitter_ads`
`tweetdeck`, 
`facebook`, 
`instagram`, 
`twitterfeed`, 
`cloudhopper` (tweets via sms service), 
`tweetbot.net`, 
`IFTTT` 

These are some notable, weird and wonderful ones:

`source:"LG Smart Refrigerator"`
`source:"GUCCI SmartToilet™"`
