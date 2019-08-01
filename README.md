# Advanced Search on Twitter

These operators work on Web, Mobile, Tweetdeck.

Adapted from [TweetDeck Help](https://help.twitter.com/en/using-twitter/advanced-tweetdeck-features), @lucahammer [Guide](https://freshvanroot.com/blog/2019/twitter-search-guide-by-luca/), and Twitter / Tweetdeck itself. Contributions / tests, examples welcome!

Class | Operator | Finds Tweets…
-- | -- | --
Tweet content | love hatelove AND hate(love hate) | Containing both "love" and "hate"
  | love OR hate | At least one of either "love" or "hate"
  | -love | Excluding "love"
  | #tgif | A hashtag
  | $twtr | A cashtag, useful for following stock information
  | "love hate" | The complete phrase "love hate"
  | traffic ? | Question marks are matched
  | url:google.com | urls are tokenized and matched, works very well for subdomains and domains, not so well for long urls, depends on url. Youtube ids work well.
  |   |  
Users | from:user | Sent by a particular @username e.g. "#space from:NASA"
  | to:user | Replying to a particular @username
  | "+@user" | Mentioning a particular @username e.g. "+@NASA"
  | list:user/list-name | From members of this List e.g. list:NASA/space-tweets
  | filter:verified | From verified users
  |   |  
Geo | near:city | Geotagged in this place
  | within:radius | Within specific radius of the "near" operator, to apply a limit. Can use km or mi. e.g. "fire near:san-francisco within:10km"
  |   |  
Time  | since:yyyy-mm-dd | On or after a specified date
  | until:yyyy-mm-dd | On or before a specified date. Combine with the "since" operator for dates between.
  | max_id:tweet_id | Snowflake ID based for exact time search (milli_second_epoch - 1288834974657) << 22 
  | min_id:tweet_id | Does not work together with max_id
  |   |  
Tweet Type  | filter:nativeretweets | Retweets from users who have hit the retweet button.
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
  |   |  
App specific | source:client_name | Sent from a specified client e.g. source:tweetdeck (common clients are: tweetdeck, twitter_for_iphone, twitter_for_android, twitter_web_client)
  | card_domain:pscp.tv | Matches url in a Twitter Card. Maybe equivalent to `url:` for most links.
  | card_name:audio | Tweets with a Player Card (Links to Audio sources, Spotify, Soundcloud etc.)
  | card_name:animated_gif | Tweets With GIFs
  | card_name:player | Tweets with a Platyer Card
  | card_name:app | Tweets with links to an App Card
  | card_name:summary_large_image | Only large image Cards
  | card_name:summary | Only Small image summary cards


# Building Queries

Any "`filter:type`" can also be negated using the "`-`" symbol.

Example: I want Tweets from @Nasa with all types of media except images

`from:NASA filter:media -filter:images`

Combine complex queries together with booleans and parentheses to refine your results.

Example 1: I want mentions of either "puppy" or "kitten", with mentions of either "sweet" or "cute", excluding Retweets, with at least 10 likes.

`(puppy OR kitten) AND (sweet OR cute) -filter:nativeretweets min_faves:10`

Example 2: I want mentions of "space" and either "big" or "large" by members of the NASA astronauts List, sent from an iPhone or twitter.com, with images, excluding mentions of #asteroid, since 2011.

`space (big OR large) list:nasa/astronauts (source:twitter_for_iphone OR source:twitter_web_client) filter:images since:2011-01-01 -#asteroid`

# Note:

There are a bunch of operators that can be used in Web search that are either not implemented in the UI or are described in API docs instead of Search help pages, or are only documented for Tweetdeck, but work on the website and mobile too. This is a collection of all the operators I could find, as well as how to go about finding these in the first place.

Web, Mobile, Tweetdeck Search are one thing, Standard API Search is another, Premium Search and Enterprise Search is a third, separate thing based on Gnip products. API docs already exist for the API and Premium but i might add those separately later.

There are multiple systems, and not all of them support the same queries. 

### How did I find these in the first place?

Reading Twitter Documentation and help docs from as many sources as possible - eg: Developer Documentation, Help pages, Tool-specific help pages, eg: Tweetdeck help etc. Using Share feature on tweetdeck to copy the search string. Searching google and pastebin and github for rarely documented ones together to find other lists of operators others have compiled.

### Known Unknowns and Assumptions:

I have no idea how Twitter decides what should match `filter:news`, my guess is that it's based on a list of whitelisted domain names, as tweets from anyone can appear as long as they link to a news site, no idea if this list is public. No idea if or how this filter changed over time. But we can try to retrieve tweets and see. `lang:unk` will match most empty tweets or tweets with a single number or link. `filter:safe` presumably uses the User setting "Contains Sensitive Content" - but may also apply to specific tweets somehow.

It would be great to be able to reliably find Promoted tweets - this may be possible with some of the card searches.

I'd also like to search for Collections (Timelines) and Moments, but this seems to work ok with just `url:` searches. eg: `url:twitter.com/i/events` and `url:twitter.com/i/moments` (I think the difference is events are curated?) but `url:twitter.com url:timelines` has many false positives.


### Full List of supported languages:

From TweetDeck dropdown menu:

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

Searching for `lang:chr`, `lang:iu`, `lang:sk` seems to fail, as tweets matching the keywords are returned instead of the language. 
