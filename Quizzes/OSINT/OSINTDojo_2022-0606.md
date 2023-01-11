# OSINTDojo_20220606_Challenge
### Challenge
This week’s [#OSINT](https://twitter.com/hashtag/OSINT?src=hashtag_click) challenge will test your web3 research skills. Try to answer the following questions about the attached image. 
1) What ETH wallet owns this NFT? 
2) What is this wallet known for?
3) Which threat actor is this wallet attributed to? 
4) What mixing service did the actor use?

![FUkkKTnUYAAVX2F](https://user-images.githubusercontent.com/101227395/172295558-7c328e4d-5718-473c-9ceb-d09f67fb0459.jpg)

Challenge presented by: @OSINTDojo on Twitter.  [Check them out!](https://twitter.com/OSINTDojo)

---
#### Step 1
First off, I want to say that I have almost no experience with Web3, cryptocurrency, Non-Fungible Tokens.  This will be a _huge_ learning experience for me.

Okay, now with the disclaimer out of the way, let's break down the provided image:

![FUkkKTnUYAAVX2F 1](https://user-images.githubusercontent.com/101227395/172295577-4b2b1164-13b7-41f3-ad91-f85c7c89cf2a.jpg)

1) This symbol looks like the Etherum logo
2) Listing for the NFT
3) The creator of the NFT
4) The description of the NFT

These stick out and will likely be unique identifiers.  But now I have to find out where this is hosted.  Are there any sites that buy/sell NFTs?

---
#### Step 2
I did a search for the creator of the NFT, Religious0ne's lemonade stand.  That search pulled up their collection on OpenSea: https://opensea.io/collection/religious0ne-collection

Looking through their collection, I saw a whole bunch of random dumb BS.  I know this may be a super hot take, but I find NFT Artwork to be moronic and an absolute scam.  

___
#### Step 3
I filtered the activity results to look at All Time and scrolled down to find the first entry, then work my way backwards.  I found a couple of different listings, but only a few that matched the item listing and ignored the NFTs with the 'Minted' status.

![Pasted image 20220606224638](https://user-images.githubusercontent.com/101227395/172295651-93f930a9-9229-4991-88c4-24520d8dd575.png)

From this, I now have 2 potential accounts that answers the first question: `Based_dom`, `098B71`

---
#### Step 4
Moving to inspect the 2 accounts further, I find that `098B71` returns a 404 error that the user can't be found.  Hmmm...  might have to do more some more digging on that.  I'll put that on hold for now.

`Based_dom` appears to be just a handle, as beneath their profile is some additional account information.

![Pasted image 20220606225115](https://user-images.githubusercontent.com/101227395/172295673-229dccee-8589-469e-8c5a-b627b43de4b5.png)

Looking through the history of `Based_dom`/`sashagreystoilet.eth`, this account seems to be legitimate.  No associations are listed with this indicating any malicious actions.  I'll pin this one and go back to look at the other account.

---
#### Step 5
If the page no longer exists _now_, maybe it exists somewhere in the past?  Time to head over to [Wayback Machine](http://web.archive.org/) and see if I can dig anything up.

I take the URL for the [`098B71` account that no longer seems to exist](https://opensea.io/0x098B716B8Aaf21512996dC57EB0615e2383E2f96) and throw it into the search bar.  

Surprisingly, I get 2 hits, both in March 2022.  There are 2 snapshots recorded on 31 March 2022.  

![Pasted image 20220606225833](https://user-images.githubusercontent.com/101227395/172295699-58960177-4789-44bc-bd30-54205527046b.png)

I open up the snapshot captured at 13:10:13 (GMT) and [see the user's account page on OpenSea](http://web.archive.org/web/20220331131013/https://opensea.io/0x098B716B8Aaf21512996dC57EB0615e2383E2f96).  

![Pasted image 20220606230008](https://user-images.githubusercontent.com/101227395/172295717-f3a2b93f-261c-4015-9f62-fa05e186a639.png)

Here I am able to copy the full address for the account: `0x098B716B8Aaf21512996dC57EB0615e2383E2f96`

---
#### Step 6

I throw the newly found address into a search engine and hope that this continues to lead me down the right track.  

And boy howdy, the results looking daming (but also looks like I am on the right track!).  One of the results is from the US Department of Treasury, associating the address with the world's favorite adversarial nation, **North Korea**!

![giphy-2115959046](https://user-images.githubusercontent.com/101227395/172295909-eb6cbc47-c677-443a-81a0-7cd2b58fff9c.gif)

And like the icing on the cake, one of its known associations is one my favorite threat actors of interest, **Lazarus Group**.  

---
#### Step 7
Starting to hit a wall because I've been in classes all day since before the sun came over the horizon, I do one final search to see if I can answer question 4.  I use the same address found with the Wayback Machine and add 'mixer' as a term to the search.  

This gives me a mixture of results, but one looks more promising the rest: https://www.trmlabs.com/post/north-koreas-lazarus-group-moves-funds-through-tornado-cash

On that site, they list that Tornado Cash was the mixer used in recent times.

![626b652a9025486cd245e6c7_0F05E989-E1F0-4AFA-8D39-758310A2A7A2](https://user-images.githubusercontent.com/101227395/172296217-6d46f54f-461a-493d-9008-e4ac9395fa4d.jpeg)

---
#### Conclusion
1) What ETH wallet owns this NFT?   `0x098B716B8Aaf21512996dC57EB0615e2383E2f96`
2) What is this wallet known for? Most recently was the Ronin Bridge Exploit.  These bad dudes have enjoyed stealing lots of crypto over the last few years.  
3) Which threat actor is this wallet attributed to? Lazarus Group, who also likes to target [defense contractors with malicious Word Documents](https://blog.knowbe4.com/lazarus-group-continues-targeting-defense-contractors) and possibly even [trying to get hired by Web3 developers](https://twitter.com/jonwu_/status/1520072367069876224)
4) What mixing service did the actor use? Tornado Cash

And as a bonus question, how did they accomplish all of this?  Well...
![image](https://user-images.githubusercontent.com/101227395/172296045-e816f0da-90ab-4adb-97e1-a631a359060d.png)
