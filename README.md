# 🔍 Advanced Search on Twitter

[![👀](https://repository-images.githubusercontent.com/200083171/7d2f7d80-b492-11e9-8f1b-4a5863429dca)](https://twitter.com/search-advanced)

These operators work on [Web](https://twitter.com/search-advanced), [Mobile](https://mobile.twitter.com/search-advanced), [Tweetdeck](https://tweetdeck.twitter.com/).

There is some overlap, but largely these will **not work** for [v1.1 Search](https://developer.twitter.com/en/docs/twitter-api/v1/tweets/search/overview), [Premium Search](https://developer.twitter.com/en/docs/twitter-api/premium/search-api/overview), or [v2 Search](https://developer.twitter.com/en/docs/twitter-api/tweets/search/introduction) APIs.

Adapted from [TweetDeck Help](https://help.twitter.com/en/using-twitter/advanced-tweetdeck-features), @lucahammer [Guide](https://freshvanroot.com/blog/2019/twitter-search-guide-by-luca/), @eevee [Twitter Manual](https://eev.ee/blog/2016/02/20/twitters-missing-manual/), @pushshift and Twitter / Tweetdeck itself. Contributions / tests, examples welcome!

Class | Operator | Finds Tweets… | Eg:
-- | -- | -- | --
Tweet content | `nasa esa` <br> `(nasa esa)` | Containing both "`nasa`" and "`esa`". Spaces are implicit AND. Brackets can be used to group individual words if using other operators. | [🔗](https://twitter.com/search?q=esa%20nasa&src=typed_query&f=live)
&nbsp; | `nasa OR esa` | Either "`nasa`" or "`esa`". OR must be in uppercase. | [🔗](https://twitter.com/search?q=nasa%20OR%20esa&src=typed_query&f=live)
&nbsp; | `"state of the art"` | The complete phrase "`state of the art`". Will also match "`state-of-the-art`". Also use quotes to prevent spelling correction. | [🔗](https://twitter.com/search?q=%22state%20of%20the%20art%22&src=typed_query&f=live)
&nbsp; | `"this is the * time this week"` | A complete phrase with a wildcard. ` * ` does not work outside of a quoted phrase or without spaces. | [🔗](https://twitter.com/search?q=%22this%20is%20the%20*%20time%20this%20week%22&src=typed_query&f=live)
&nbsp; | `+radiooooo` | Force a term to be included as-is. Useful to prevent spelling correction. | [🔗](https://twitter.com/search?q=%2Bradiooooo&src=typed_query&f=live)
&nbsp; | `-love` <br> `-"live laugh love"` | `-` is used for excluding "`love`". Also applies to quoted phrases and other operators. | [🔗](https://twitter.com/search?q=bears%20-chicagobears&src=typed_query&f=live)
&nbsp; | `#tgif` | A hashtag | [🔗](https://twitter.com/search?q=%23tgif&src=typed_query&f=live)
&nbsp; | `$TWTR` | A cashtag, like hashtags but for stock symbols | [🔗](https://twitter.com/search?q=%24TWTR%20OR%20%24FB%20OR%20%24AMZN%20OR%20%24AAPL%20OR%20%24NFLX%20OR%20%24GOOG&src=typed_query&f=live)
&nbsp; | `What ?` | Question marks are matched | [🔗](https://twitter.com/search?q=(Who%20OR%20What%20OR%20When%20OR%20Where%20OR%20Why%20OR%20How)%20%3F&src=typed_query&f=live)
&nbsp; | `:) OR :(` | Some emoticons are matched, positive `:) :-) :P :D` or negative `:-( :(` | [🔗](https://twitter.com/search?q=%3A%29%20OR%20%3A-%29%20OR%20%3AP%20OR%20%3AD%20OR%20%3A%28%20OR%20%3A-%28&src=typed_query&f=live)
&nbsp; | 👀 | Emoji searches are also matched. Usually needs another operator to work. | [🔗](https://twitter.com/search?q=%F0%9F%91%80%20lang%3Aen&src=typed_query&f=live) 
&nbsp; | `url:google.com` | urls are tokenized and matched, works very well for subdomains and domains, not so well for long urls, depends on url. Youtube ids work well. Works for both shortened and canonical urls, eg: `gu.com` shortener for `theguardian.com`. When searching for Domains with hyphens in it, you have to replace the hyphen by an underscore (like `url:t_mobile.com`) but underscores `_` are also tokenized out, and may not match | [🔗](https://twitter.com/search?q=url%3Agu.com&src=typed_query&f=live) 
&nbsp; | `lang:en` | Search for tweets in specified language, not always accurate, see the full [list](#supported-languages) and special `lang` codes below. | [🔗](https://twitter.com/search?q=lang%3Aen&src=typed_query&f=live)
&nbsp; | | | 
Users | `from:user` | Sent by a particular `@username` e.g. `"dogs from:NASA"` | [🔗](https://twitter.com/search?q=dogs%20from%3Anasa&src=typed_query&f=live)
&nbsp; | `to:user` | Replying to a particular `@username` | [🔗](https://twitter.com/search?q=%23MoonTunes%20to%3Anasa&src=typed_query&f=live)
&nbsp; | `@user` | Mentioning a particular `@username`. Combine with `-from:username` to get only mentions | [🔗](https://twitter.com/search?q=%40cern%20-from%3Acern&src=typed_query&f=live)
&nbsp; | `list:108534289` <br> `list:user/list-slug` | Tweets from members of this public list. Use the list ID from the API or with urls like `https://twitter.com/i/lists/4143216`. List slug is for old list urls like `http://twitter.com/nasa/lists/astronauts`. Cannot be negated, so you can't search for "not on list". | [🔗](https://twitter.com/search?q=list%3A4143216&src=typed_query&f=live)
&nbsp; | `filter:verified` | From verified users | [🔗](https://twitter.com/search?q=filter%3Averified&src=typed_query&f=live)
&nbsp; | `filter:follows` | Only from accounts you follow | [🔗](https://twitter.com/search?q=filter%3Afollows%20lang%3Aen&src=typed_query&f=live)
&nbsp; | `filter:social` <br> `filter:trusted` | Only from algorithmically expanded network of accounts based your own follows and activities. Works on "`Top`" results not "`Latest`" | [🔗](https://twitter.com/search?q=kitten%20filter%3Asocial&src=typed_query)
&nbsp; | | | 
Geo | `near:city` | Geotagged in this place. Also supports Phrases, eg: `near:"The Hague"` | [🔗](https://twitter.com/search?q=near%3A%22The%20Hague%22&src=typed_query&f=live)
&nbsp; | `near:me` | Near where twitter thinks you are | [🔗](https://twitter.com/search?q=near%3Ame&src=typed_query&f=live)
&nbsp; | `within:radius` | Within specific radius of the "near" operator, to apply a limit. Can use km or mi. e.g. `fire near:san-francisco within:10km` | [🔗](https://twitter.com/search?q=fire%20near%3Asan-francisco%20within%3A10km&src=typed_query&f=live)
&nbsp; | `geocode:lat,long,radius` | E.g., to get tweets 10km around twitters hq, use `geocode:37.7764685,-122.4172004,10km` | [🔗](https://twitter.com/search?q=geocode%3A37.7764685%2C-122.4172004%2C10km&src=typed_query&f=live)
&nbsp; | `place:96683cc9126741d1` | Search tweets by [Place Object](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/geo-objects.html#place) ID eg: USA Place ID is `96683cc9126741d1` | [🔗](https://twitter.com/search?q=place%3A96683cc9126741d1&src=typed_query&f=live)
&nbsp; | | | 
Time | `since:yyyy-mm-dd` | On or after (inclusive) a specified date | [🔗](https://twitter.com/search?q=since%3A2019-06-12%20until%3A2019-06-28%20%23nasamoontunes&src=typed_query&f=live)
&nbsp; | `until:yyyy-mm-dd` | Before (NOT inclusive) a specified date. Combine with a "since" operator for dates between. | [🔗](https://twitter.com/search?q=since%3A2019-06-12%20until%3A2019-06-28%20%23nasamoontunes&src=typed_query&f=live)
&nbsp; | `since_time:1142974200` | On or after a specified unix timestamp in seconds. Combine with the "until" operator for dates between. Maybe easier to use than `since_id` below. | [🔗](https://twitter.com/search?q=since_time%3A1561720321%20until_time%3A1562198400%20%23nasamoontunes&src=typed_query&f=live)
&nbsp; | `until_time:1142974215` | Before a specified unix timestamp in seconds. Combine with a "since" operator for dates between. Maybe easier to use than `max_id` below. | [🔗](https://twitter.com/search?q=since_time%3A1561720321%20until_time%3A1562198400%20%23nasamoontunes&src=typed_query&f=live)
&nbsp; | `since_id:tweet_id` | After (NOT inclusive) a specified Snowflake ID | [🔗](https://twitter.com/search?q=since_id%3A1138872932887924737%20max_id%3A1144730280353247233%20%23nasamoontunes&src=typed_query&f=live)
&nbsp; | `max_id:tweet_id` | At or before (inclusive) a specified Snowflake ID (see [Note](#snowflake-ids) below) | [🔗](https://twitter.com/search?q=since_id%3A1138872932887924737%20max_id%3A1144730280353247233%20%23nasamoontunes&src=typed_query&f=live)
&nbsp; | `within_time:2d` <br> `within_time:3h` <br> `within_time:5m` <br> `within_time:30s` | Search within the last number of days, hours, minutes, or seconds | [🔗](https://twitter.com/search?q=nasa%20within_time%3A30s&src=typed_query&f=live)
&nbsp; | | | 
Tweet Type | `filter:nativeretweets` | Only retweets created using the retweet button. Works well combined with `from:` to show only retweets. | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Anativeretweets&src=typed_query&f=live)
&nbsp; | `include:nativeretweets` | Native retweets are excluded by default. This shows them. In contrast to `filter:`, which shows only retweets, this includes retweets in addition to other tweets | [🔗](https://twitter.com/search?q=from%3Anasa%20include%3Anativeretweets%20&src=typed_query&f=live)
&nbsp; | `filter:retweets` | Old style retweets ("RT") + quoted tweets. | [🔗](https://twitter.com/search?q=filter%3Aretweets%20from%3Atwitter%20until%3A2009-11-06%09&src=typed_query&f=live)
&nbsp; | `filter:replies` | Tweet is a reply to another Tweet. good for finding conversations, or threads if you add or remove `to:user` | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Areplies%20-to%3Anasa&src=typed_query&f=live)
&nbsp; | `conversation_id:tweet_id` | Tweets that are part of a thread (direct replies and other replies) | [🔗](https://twitter.com/search?q=conversation_id%3A1140437409710116865%20lang%3Aen&src=typed_query&f=live)
&nbsp; | `filter:quote` | Contain Quote Tweets | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Aquote&src=typed_query&f=live)
&nbsp; | `quoted_tweet_id:tweet_id` | Search for quotes of a specific tweet | [🔗](https://twitter.com/search?q=quoted_tweet_id%3A1138631847783608321&src=typed_query&f=live)
&nbsp; | `quoted_user_id:user_id` | Search for all quotes of a specific user | [🔗](https://twitter.com/search?q=quoted_user_id%3A11348282&src=typed_query&f=live)
&nbsp; | `card_name:poll2choice_text_only` <br> `card_name:poll3choice_text_only` <br> `card_name:poll4choice_text_only` <br> `card_name:poll2choice_image` <br> `card_name:poll3choice_image` <br> `card_name:poll4choice_image`| Tweets containing polls. For polls containing 2, 3, 4 choices, or image Polls. | [🔗](https://twitter.com/search?q=lang%3Aen%20card_name%3Apoll4choice_text_only%20OR%20card_name%3Apoll3choice_text_only%20OR%20card_name%3Apoll2choice_text_only&src=typed_query&f=live)
&nbsp; | | | 
Engagement | `filter:has_engagement` | Has some engagement (replies, likes, retweets). Can be negated to find tweets with no engagement. | [🔗](https://twitter.com/search?q=breaking%20filter%3Anews%20-filter%3Ahas_engagement&src=typed_query&f=live)
&nbsp; | `min_retweets:5` | A minimum number of Retweets. Counts seem to be approximate for larger (1000+) values. | [🔗](https://twitter.com/search?q=min_retweets%3A5000%20nasa&src=typed_query&f=live)
&nbsp; | `min_faves:10` | A minimum number of Likes | [🔗](https://twitter.com/search?q=min_faves%3A10000%20nasa&src=typed_query&f=live)
&nbsp; | `min_replies:100` | A minimum number of replies | [🔗](https://twitter.com/search?q=min_replies%3A1000%20nasa&src=typed_query&f=live)
&nbsp; | `min_retweets:500` | A maximum number of Retweets | [🔗](https://twitter.com/search?q=-min_retweets%3A500%20nasa&src=typed_query&f=live)
&nbsp; | `min_faves:500` | A maximum number of Likes | [🔗](https://twitter.com/search?q=-min_faves%3A500%20nasa&src=typed_query&f=live)
&nbsp; | `min_replies:100` | A maximum number of replies | [🔗](https://twitter.com/search?q=-min_replies%3A100%20nasa&src=typed_query&f=live)
&nbsp; | | | 
Media | `filter:media` | All media types. | [🔗](https://twitter.com/search?q=filter%3Amedia%20cat&src=typed_query&f=live)
&nbsp; | `filter:twimg` | Native Twitter images (`pic.twitter.com` links) | [🔗](https://twitter.com/search?q=filter%3Atwimg%20cat&src=typed_query&f=live)
&nbsp; | `filter:images` | All images. | [🔗](https://twitter.com/search?q=filter%3Aimages%20cat&src=typed_query&f=live)
&nbsp; | `filter:videos` | All video types, including native Twitter video and external sources such as Youtube. | [🔗](https://twitter.com/search?q=filter%3Avideos%20cat&src=typed_query&f=live)
&nbsp; | `filter:periscope` | Periscopes | [🔗](https://twitter.com/search?q=filter%3Aperiscope%20cat&src=typed_query&f=live)
&nbsp; | `filter:native_video` | All Twitter-owned video types (native video, vine, periscope) | [🔗](https://twitter.com/search?q=filter%3Anative_video%20cat&src=typed_query&f=live)
&nbsp; | `filter:vine` | Vines (RIP) | [🔗](https://twitter.com/search?q=filter%3Avine%20cat&src=typed_query&f=live)
&nbsp; | `filter:consumer_video` | Twitter native video only | [🔗](https://twitter.com/search?q=filter%3Aconsumer_video%20cat&src=typed_query&f=live)
&nbsp; | `filter:pro_video` | Twitter pro video (Amplify) only | [🔗](https://twitter.com/search?q=filter%3Apro_video%20cat&src=typed_query&f=live)
&nbsp; | `filter:spaces` | Twitter Spaces only | [🔗](https://twitter.com/search?q=filter%3Aspaces&src=typed_query&f=live)
&nbsp; | | | 
More Filters | `filter:links` | Only containing some URL, includes media. use `-filter:media` for urls that aren't media | [🔗](https://twitter.com/search?q=filter%3Afollows%20filter%3Alinks%20-filter%3Amedia&src=typed_query&f=live)
&nbsp; | `filter:mentions` | Containing any sort of `@mentions` | [🔗](https://twitter.com/search?q=filter%3Amentions%20from%3Atwitter%20-filter%3Areplies&src=typed_query&f=live)
&nbsp; | `filter:news` | Containing link to a news story. Combine with a list operator to narrow the user set down further. | [🔗](https://twitter.com/search?q=filter%3Anews%20lang%3Aen&src=typed_query&f=live)
&nbsp; | `filter:safe` | Excluding NSFW content. Excludes content that users have marked as "Potentially Sensitive". Doesn't always guarantee SFW results. | [🔗](https://twitter.com/search?q=filter%3Asafe%20%23followfriday&src=typed_query&f=live)
&nbsp; | `filter:hashtags` | Only Tweets with Hashtags. | [🔗](https://twitter.com/search?q=from%3Anasa%20filter%3Ahashtags&src=typed_query&f=live)
&nbsp; | | | 
App specific | `source:client_name` | Sent from a specified client e.g. source:tweetdeck (See [Note](#common-clients) for common ones) eg: `twitter_ads` doesn't work on it's own, but does with another operator. | [🔗](https://twitter.com/search?q=source%3A%22GUCCI%20SmartToilet%E2%84%A2%22%20lang%3Aen&src=typed_query&f=live)
&nbsp; | `card_domain:pscp.tv` | Matches domain name in a Twitter Card. Mostly equivalent to `url:` operator. | [🔗](https://twitter.com/search?q=card_domain%3Apscp.tv&src=typed_query&f=live)
&nbsp; | `card_url:pscp.tv` | Matches domain name in a Card, but with different results to `card_domain`. | [🔗](https://twitter.com/search?q=card_url%3Apscp.tv&src=typed_query&f=live)
&nbsp; | `card_name:audio` | Tweets with a Player Card (Links to Audio sources, Spotify, Soundcloud etc.) | [🔗](https://twitter.com/search?q=card_name%3Aaudio&src=typed_query&f=live)
&nbsp; | `card_name:animated_gif` | Tweets With GIFs | [🔗](https://twitter.com/search?q=card_name%3Aanimated_gif&src=typed_query&f=live)
&nbsp; | `card_name:player` | Tweets with a Player Card | [🔗](https://twitter.com/search?q=card_name%3Aplayer&src=typed_query&f=live)
&nbsp; | `card_name:app` <br> `card_name:promo_image_app` | Tweets with links to an App Card. `promo_app` does not work, `promo_image_app` is for an app link with a large image, usually posted in Ads. | [🔗](https://twitter.com/search?q=card_name%3Aapp%20OR%20card_name%3Apromo_image_app&src=typed_query&f=live)
&nbsp; | `card_name:summary` | Only Small image summary cards | [🔗](https://twitter.com/search?q=card_name%3Asummary&src=typed_query&f=live)
&nbsp; | `card_name:summary_large_image` | Only large image Cards | [🔗](https://twitter.com/search?q=card_name%3Asummary_large_image&src=typed_query&f=live)
&nbsp; | `card_name:promo_website` | Larger than `summary_large_image`, usually posted via Ads | [🔗](https://twitter.com/search?q=card_name%3Apromo_website%20lang%3Aen&src=typed_query&f=live)
&nbsp; | `card_name:promo_image_convo` <br> `card_name:promo_video_convo` | Finds [Conversational Ads](https://business.twitter.com/en/help/campaign-setup/conversational-ad-formats.html) cards. | [🔗](https://twitter.com/search?q=carp%20card_name%3Apromo_image_convo&src=typed_query&f=live)
&nbsp; | `card_name:3260518932:moment` | Finds Moments cards. `3260518932` is the user ID of `@TwitterMoments`, but the search finds moments for everyone, not that specific user. | [🔗](https://twitter.com/search?q=card_name%3A3260518932%3Amoment&src=typed_query&f=live)


## Matching:

On web and mobile, keyword operators can match on: The user's name, the @ screen name, tweet text, and shortened, as well as expanded url text (eg, `url:trib.al` finds accounts that use that shortener, even though the full url is displayed).

By default "Top" results are shown, where "Top" means tweets with some engagements (replies, RTs, likes). "Latest" has most recent tweets. People search will match on descriptions, but not all operators work. "Photos" and "Videos" are presumably equivalent to `filter:images` and `filter:videos`.

Exact Tokenization is not known, but it's most likely a custom one to preserve entities. URLs are also tokenized. Spelling correction appears sometimes, and also plurals are also matched, eg: `bears` will also match tweets with `bear`. `-` not preceeding an operator are removed, so "state-of-the-art" is the same as "state of the art".

Private accounts are not included in the search index, and their tweets do no appear in results. Locked and suspended accounts are also hidden from results. There are other situations where tweets may not appear: [anti-spam measures](https://help.twitter.com/en/rules-and-policies/enforcement-options), or tweets simply have not been indexed due to server issues. 

Twitter is using some words as signal words. E.g. when you search for “photo”, Twitter assumes you’re looking for Tweets with attached photos. If you want to search for Tweets which literally contain the word “photo”, you have to wrap it in double quotes `"photo"`.


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

For some queries you may want to use parameters with hyphens or spaces in it, e.g. `site:t-mobile.com` or `app:Twitter for iOS`. Twitter doesn’t accept hyphens or spaces in parameters and won’t display any tweets for this query. You can still search for those parameters by replacing all hyphens and spaces with underscores, e.g. `site:t_mobile.com` or `app:Twitter_for_iOS`.

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

To convert a Twitter ID to millisecond epoch:

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

Language is specified as [2 letter ISO codes](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes). Language is tagged automatically from the tweet text, nad not always accurate, see [here](https://blog.twitter.com/engineering/en_us/a/2015/evaluating-language-identification-performance.html) for notes on accuracy. The list from TweetDeck dropdown menu has all of them:

<details><summary>All Languages</summary>
<p>

```txt
lang:am Amharic (አማርኛ)
lang:ar Arabic (العربية)
lang:bg Bulgarian (Български)
lang:bn Bengali (বাংলা)
lang:bo Tibetan (བོད་སྐད)
lang:ca Catalan (Català)
lang:ch` Cherokee (ᏣᎳᎩ)
lang:cs Czech (čeština)
lang:da Danish (Dansk)
lang:de German (Deutsch)
lang:dv Maldivian (ދިވެހި)
lang:el Greek (Ελληνικά)
lang:en English (English)
lang:es Spanish (Español)
lang:et Estonian (eesti)
lang:fa Persian (فارسی)
lang:fi Finnish (Suomi)
lang:fr French (Français)
lang:gu Gujarati (ગુજરાતી)
lang:hi Hindi (हिंदी)
lang:ht Haitian Creole (Kreyòl ayisyen)
lang:hu Hungarian (Magyar)
lang:hy Armenian (Հայերեն)
lang:in Indonesian (Bahasa Indonesia)
lang:is Icelandic (Íslenska)
lang:it Italian (Italiano)
lang:iu Inuktitut (ᐃᓄᒃᑎᑐᑦ)
lang:iw Hebrew (עברית)
lang:ja Japanese (日本語)
lang:ka Georgian (ქართული)
lang:km Khmer (ខ្មែរ)
lang:kn Kannada (ಕನ್ನಡ)
lang:ko Korean (한국어)
lang:lo Lao (ລາວ)
lang:lt Lithuanian (Lietuvių)
lang:lv Latvian (latviešu valoda)
lang:ml Malayalam (മലയാളം)
lang:my Myanmar (မြန်မာဘာသာ)
lang:ne Nepali (नेपाली)
lang:nl Dutch (Nederlands)
lang:no Norwegian (Norsk)
lang:or Oriya (ଓଡ଼ିଆ)
lang:pa Panjabi (ਪੰਜਾਬੀ)
lang:pl Polish (Polski)
lang:pt Portuguese (Português)
lang:ro Romanian (limba română)
lang:ru Russian (Русский)
lang:si Sinhala (සිංහල)
lang:sk Slovak (slovenčina)
lang:sl Slovene (slovenski jezik)
lang:sv Swedish (Svenska)
lang:ta Tamil (தமிழ்)
lang:te Telugu (తెలుగు)
lang:th Thai (ไทย)
lang:tl Tagalog (Tagalog)
lang:tr Turkish (Türkçe)
lang:uk Ukrainian (українська мова)
lang:ur Urdu (ﺍﺭﺩﻭ)
lang:vi Vietnamese (Tiếng Việt)
lang:zh Chinese (中文)
```

</p>
</details>

Searching for `lang:chr`, `lang:iu`, `lang:sk` seems to fail, as tweets matching the keywords are returned instead of the language. 

There are also some special language codes that work. For example:

* [`lang:und`](https://twitter.com/search?q=lang%3Aund&src=typed_query&f=live) for undefined language
* [`lang:qam`](https://twitter.com/search?q=lang%3Aqam&src=typed_query&f=live) for tweets with mentions only (works for tweets since `2022-06-14`)
* [`lang:qct`](https://twitter.com/search?q=lang%3Aqct&src=typed_query&f=live) for tweets with cashtags only (works for tweets since `2022-06-14`)
* [`lang:qht`](https://twitter.com/search?q=lang%3Aqht&src=typed_query&f=live) for tweets with hashtags only (works for tweets since `2022-06-14`)
* [`lang:qme`](https://twitter.com/search?q=lang%3Aqme&src=typed_query&f=live) for tweets with media links (works for tweets since `2022-06-14`)
* [`lang:qst`](https://twitter.com/search?q=lang%3Aqst&src=typed_query&f=live) for tweets with a very short text (works for tweets since `2022-06-14`)
* [`lang:zxx`](https://twitter.com/search?q=lang%3Azxx&src=typed_query&f=live) for tweets with either media or Twitter Card only, without any additional text (works for tweets since `2022-06-14`)


### Common clients:

`source:` should work for any API client, try putting the client name in quotes or replace spaces with underscores. This is the App name field that you can alter in the [developer app configuration page](https://developer.twitter.com/en/apps), so anyone can set anything here and appear to tweet from a made up client.

You cannot copy an existing name. This operator needs to be combined with something else to work, eg: `lang:en` These are some common ones:

<details><summary>Official Twitter Clients:</summary>
<p>

```
twitter_web_client
twitter_web_app
twitter_for_iphone
twitter_for_ipad
twitter_for_mac
twitter_for_android
twitter_ads
tweetdeck
tweetdeck_web_app
twitter_for_advertisers
twitter_media_studio
cloudhopper (tweets via sms service)
```

</p>
</details>

<details><summary>Very Common 3rd Party Clients:</summary>
<p>

```
facebook
instagram
twitterfeed
tweetbot.net
IFTTT
```

</p>
</details>

<details><summary>notable, weird and wonderful ones:</summary>
<p>

```
"LG Smart Refrigerator"
"GUCCI SmartToilet™"
```

</p>
</details>
