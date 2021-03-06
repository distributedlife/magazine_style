---
layout: post
title: Improve This
subtitle:
permalink: /issue01/improve-this/
byline: The P2 Elves
category: issue01
authors:
    - name: by The P2 Elves
      avatar: pdp11-avatar.jpg
---
In improve this we take a look at a reader submitted test, user interface, story or block of code and we try and improve it, without context, explaining what we did as we went.

In this issue Anand has sent us a cucumber test that he’s asked us to improve.

<script src='https://gist.github.com/distributedlife/5692407.js'></script>

First up some incidental detail. “Given I or anyone else is, has or will log in” is the most ubiquitous waste of time that exists in the land of cucumber. Unless you're testing user authentication, delete it.

<div class='normal-gist'><script src='https://gist.github.com/distributedlife/5692415.js'></script></div>

The usernames and passwords are not required here. The roles are potentially useful but having buyer_1 and buyer_2 as unique values for a ‘role’ – that represents a classification – is a smell. Who they ‘bid on behalf of’ for is never mentioned again so that is also incidental detail.

<script src='https://gist.github.com/distributedlife/5692421.js'></script>

‘Created’ can be implied from the fact you are publishing it and as the automatic run assignment is never mentioned so it might be incidental too. I’m inclined to delete it.

Next.

<script src='https://gist.github.com/distributedlife/5692423.js'></script>

Ok. Although I would ignore how they get added to the Live Sale. And as listings seem to be of little consequence through the rest of the script I would delete this line entirely.

Next.

<script src='https://gist.github.com/distributedlife/5692452.js'></script>

The 'before Preview Start Time' refers to a state that a sale exists in. Implying state is bad because you end up referring to it in roundabout ways. Make it a first class part of your system. I'm not sure what this state is but it might be something like preparation. We also need to consider whether anyone “cannot” suspend the live sale or if only the admin cannot suspend the live sale. I suspect it’s anyone cannot suspend it. So let’s remove the admin reference.

<script src='https://gist.github.com/distributedlife/5692457.js'></script>

Next.

<script src='https://gist.github.com/distributedlife/5692489.js'></script>

Ok, now we can suspend the auction as it has started. So we should make this a separate test rather than a flow. Here we have an admin suspending an auction. Do we need tests for other roles not having the ability to suspend auctions?

<script src='https://gist.github.com/distributedlife/5692492.js'></script>

Next.

<div class='normal-gist'><script src='https://gist.github.com/distributedlife/5692495.js'></script></div>

Ok, so now we have some behavior where logged-in users are shown a message when a sale is suspended. It's not clear why buyer_2 does not see get notified. I can't see any buyer_1 steps earlier on, that don't include buyer_2.

I don't like implementation detail in tests. This test implies that only users online now, will be notified of the Live Sale suspension.

<script src='https://gist.github.com/distributedlife/5692506.js'></script>

We test that we notify users of the suspension but not how they are notified.

Next.

<div class='normal-gist'><script src='https://gist.github.com/distributedlife/5692513.js'></script></div>

Here we are testing for the absence of something. Do any users see the popup? If not, how can we test for something that was never built. Deleted.

Next.

<script src='https://gist.github.com/distributedlife/5692519.js'></script>

The auction starts with an initial bid. However it's not clear if we are testing that only an auctioneer can set the starting bid or, if setting the start bid starts the auction. I’m inclined to say the latter.

<script src='https://gist.github.com/distributedlife/5692521.js'></script>

Next.

<script src='https://gist.github.com/distributedlife/5692524.js'></script>

Ok, buyer 1 and 2 have both bid now. Then the auctioneer does a Sell and then a No Sale on the listing. What is a listing? It has only been mentioned once before on the second line of the script.

The last verification suggests that sales can be suspended but only if it is in the No Sale state. However there is no guarantee of this. I’m not sure what ‘No Sale’ means exactly. It’s a strange name for a state but i’ll let it pass but would no called it a ‘No Sale auction’ but a ‘No Sale’.

<script src='https://gist.github.com/distributedlife/5692528.js'></script>

Next.

<script src='https://gist.github.com/distributedlife/5692539.js'></script>

Listings are back and they can run. The important thing here is that the auctioneer ends the auction after the listings have run. Not that having the listings finish running automatically ends the auction. I see listings as not important.

<script src='https://gist.github.com/distributedlife/5692540.js'></script>

Done.

The main things we covered were around domain language and removing the implementation from the notifications. We removed lots of incidental detail and clarified some behaviour around state transition and who is authorised to do what.

We’ve made a lot of assumptions about how this system works and how the cucumber file is used. We’ve ignored how the cucumber file got to where it is today and followed a simple rule: if someone can’t understand how it works from reading it, then it needs improvement.

Do you have something you want improved? Send it to <a href='mailto:p2@thoughtworks.com'>p2@thoughtworks.com</a>.

<div class='byline'>All Gists brought to you by <a href='http://github.com/'>GitHub</a></div>
