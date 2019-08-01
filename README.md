# Advanced Search on Twitter

These operators work on Web, Mobile, Tweetdeck.

Adapted from [TweetDeck Help](https://help.twitter.com/en/using-twitter/advanced-tweetdeck-features), @lucahammer [Guide](https://freshvanroot.com/blog/2019/twitter-search-guide-by-luca/), and Twitter / Tweetdeck itself.

These are all the documented tweetdeck operators:

Class | Operator | Finds Tweets…
-- | -- | --
Tweet content | love hatelove AND hate(love hate) | Containing both "love" and "hate"
  | love OR hate | At least one of either "love" or "hate"
  | -love | Excluding "love"
  | #tgif | A hashtag
  | $twtr | A cashtag, useful for following stock information
  | "love hate" | The complete phrase "love hate"
  | filter:news | Containing link to a news story. Combine with a list operator to narrow the user set down further.
  | filter:safe | Excluding NSFW content.
  |   |  
Users | from:user | Sent by a particular @username e.g. "#space from:NASA"
  | to:user | Replying to a particular @username
  | "+@user" | Mentioning a particular @username e.g. "+@NASA"
  | list:user/list-name | From members of this List e.g. list:NASA/space-tweets
  | filter:verified | From verified users
  |   |  
Tweet info | near:city | Geotagged in this place
  | within:radius | Within specific radius of the "near" operator, to apply a limit. Can use km or mi. e.g. "fire near:san-francisco within:10km"
  | since:yyyy-mm-dd | On or after a specified date
  | until:yyyy-mm-dd | On or before a specified date. Combine with the "since" operator for dates between.
  | filter:nativeretweets | Retweets from users who have hit the retweet button.
  | filter:retweets | Old style retweets ("RT") + quoted tweets.
  | filter:replies | Tweet is a reply to another Tweet.
  | min_retweets:5 | A minimum number of Retweets
  | min_faves:10 | A minimum number of Likes
  | min_replies:100 | A minimum number of replies
  | source:client_name | Sent from a specified client e.g. source:tweetdeck (common clients are: tweetdeck, twitter_for_iphone, twitter_for_android, web)
  |   |  
Media | filter:media | All media types.
  | filter:twimg | Native Twitter images (pic.twitter.com links)
  | filter:images | All images.
  | filter:video | All video types, including native Twitter video and external sources such as Youtube.
  | filter:periscope | Periscopes
  | filter:native_video | All Twitter-owned video types (native video, vine, periscope)
  | filter:consumer_video | Twitter native video only
  | filter:pro_video | Twitter pro video (Amplify) only

# Note:

There are a bunch of operators that can be used in Web search that are either not implemented in the UI or are described in API docs instead of Search help pages, or are only documented for Tweetdeck, but work on the website and mobile too. This is a collection of all the operators I could find, as well as how to go about finding these in the first place.

Web, Mobile, Tweetdeck Search are one thing, Standard API Search is another, Premium Search and Enterprise Search is a third, separate thing based on Gnip products. API docs already exist for the API and Premium but i might add those separately later.

There are multiple systems, and not all of them support the same queries. 

### How did I find these in the first place?

Reading Twitter Documentation and help docs from as many sources as possible - eg: Developer Documentation, Help pages, Tool-specific help pages, eg: Tweetdeck help etc. Using Share feature on tweetdeck to copy the search string. Searching google and pastebin and github for rarely documented ones together to find other lists of operators others have compiled.

### Known Unknowns and Assumptions:

I have no idea how Twitter decides what should match `filter:news`, or how, if at all - it changed over time. But we can try to retrieve tweets and see if there's commonality. `lang:unk` will match most empty tweets or tweets with a single number or link. `filter:safe` presumably uses the User setting "Contains Sensitive Content" - but may also apply to specific tweets somehow.

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
