---
category : blog
tags : [MOTODEV, Android, i18n]
title: MOTODEV Studio localization feature and the Google Translate API
hidden: true
---
**NOTE:** This blog post was originally hosted on the **Motorola Developers (MOTODEV)**. site. That site is no longer online. I've made every attempt to preserve the original content with only formatting changes to fit this site.

---

Hello everyone,

Earlier this week, Google announced that they were deprecating the
Google Translate APIs, effective immediately. I want to take a moment to
explain what this means to MOTODEV Studio users.

The announcement is short on details about how the deprecation will
occur. It mentions that there will be a limitation on the number of
translations per day, but it isn't clear if this limitation is "per IP
address", "per API key", or some other factor. No mention is made of
what the limit entails. We're doing some investigations to test what
these conditions are. If it is the former, then you will need to
exercise some restraint on the system, but the impact will be local to
each user.

If the limitation is "per API key", then some measures will have to be
taken because all MOTODEV Studio users share the same API key, which is
stored within MOTODEV Studio itself. We were already planning to
refactor this data value out so you could use your own API keys, and
this change will find it's way into an update in the near future.
Regardless, this is a short-term fix, as the translate API will go
completely offline later this year.

When we created the localization files editor, we always knew we wanted
to have multiple translation providers and this is evident by looking in
the translate dialog. There is a popup that could be filled with other
providers because this functionality is encapsulated into its' own
plugin. We have started the process of evaluating other providers and if
we can locate a good alternative, it will be included in a release later
in the year.

For the short term, keep using the translate feature as it exists. If
you run into a problem with it, please let us know by responding to this
thread. That way we can work together for a better experience for all.

Thanks (again) for using MOTODEV Studio.

-E

http://code.google.com/apis/language/translate/overview.html
