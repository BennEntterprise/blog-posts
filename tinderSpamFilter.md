Date (5/20)

# If I was at Tinder, what would I want to work on....

As a woman Tinder can be like shooting fish in a barrel. As soon as you swipe right a few times, you've got several matches. The prettier the woman, the higher the percentage of matches. I won't get into the incredibly subjective field of what is "pretty" but in general women can be more selective than men on this platform. (This is also assuming a cis-hetero norm because that's the only experience I have. If you have other expereiences write to me! I'd love to have you co-author this with me! Especially if we can generalize across gender and orientation).

For men it's a bit harder. At one point I was counting that approximately every 100 swipes I would get _maybe_ 3-5 likes. Of these I'd be lucky if there was one that wasn't trying to sell me a service (like nudes, or a subscription to a porn site) that is clearly a scam. Porn has it's time and place, but when I'm trying to make a human connection...it's not really what I need.

Lately (May 2020 Covid Season) I've been swipping more. Looking for someone who wouldn't mind having a glass of wine over Skype or doing a socially distant walk in the park. I'm in Pittsburgh, an area that has Covid under control and is starting to open back up soon (as of this writing anyway). After reporting about 10 accounts, making work for someone else, I couldn't help but wonder... "If I worked at Tinder, could I fix this, and stop those damn spam tickets from showing up?"

## Learning about The Scam

ok! Imagination time! I've been hired to fix Tinder's spam problem. How do I do it?

To fix a problem you need to learn about it. So that meant that instead of seeking matches, I needed to start seeking Spam. I would swipe, intentionally looking for profiles that felt "scammy" (here's a good link I used as an initial compass, then I just used intuition). Most of the time these profiles had a fairly attractive woman, with betweeen 1 and 3 pictures, one of which being a swimsuit or other slightly nude photo. None of the photos were exceptionally explicit, meaning they didn't set of Tinder's exisiting porn filter, but they were still what one could label as seductive or a tease. These profiles usually had a SnapChat Code. Then I would copy/paste that code into Snapchat.

Now I know you're not supposed to follow spam links, but copy/pasting to Snap allows a sort of sandbox. I'm just viewing a user profile, I'm not actually visiting the site. Amazingly these profiles are public, so when you search for their name you can see one photo on the Daily/Pinned story. I did this experiment for about 20 minutes last night and found 5 of these fake profiles, all of which included a link to a site called `blueberrypancakes.website`. Hell of a name to sell subscription based porn but who am I to judge right?

_You have to imagine my shock when I saw the SAME spam web site pop up on all 5 accounts!_

I'd imagine that Tinder is fighting Spam everyday so it makes sense that some will get through (no one has a perfect email inbox right?) but to see 5 of the same thing is quite interesting... Which led me to think: "I can automate this".

## Lets Review: Steps for identifying the spam.

1. View the profile of a potential match (the current card)
2. If there is a snapchat code, copy it.
3. Paste snapchat code into snapchat.
4. Select the first picture
5. Discern if the first picture has a link to to another site (most likely trying to sell something on _blueberrypancakes.website_)
6. Report as spam, swipe left (don't swipe right on spam, it will boost it's credibility)

Ok So how to do this?
It's against the Tinder Terms of Service to data mine (ask me how I know) but since I'm supposing I have been hired by Tinder that means I get to write a psudoscript that can actually access the business logic, perisitance and storage layers of the achitecture. Great! I'm not breeaking any rules! And since every time I found spam as a user, I reported it, even my preliminary work was within the ToS; I was helping Tinder to become a better platform via the tools they provided to users.

Essentially the Psudo code will look like this.

1. Grab a set of user profiles from the database. (Use SQL, of NoSql type languages Lately I've been learning LINQ so maybe it will come in handy.)

-   Let's go by account_create_date, so we can stop any new spam from coming on. Also make sure the account hadn't already been reviewed in the past month/week or whatever time frame. We'll have to tune the system once we can see the loads.
-   We also likely have daily/weekly enrollment numbers so there is predicatability in our sample size.
-   Predicatbility in sample size on "non-mission critical" functions means the script, when finished can run on aws spot instances for cheap.
-   Cheap budgets means I'm earning my engineer salary while provide good value to the end user. :)

2. For each of those users scan the profile for a snapchat code.

-   Will probably need a regex to find snapchat names which are alphanumeric with no spaces and have an upper character limit.
-   My inital study found these at the end of a profile so I would parse right-to-left, giving higher weight to rightmost matches.
-   Rule out any valid words, if human review is needed, they'll only have to discern from a few "potentialSnapChatNames"

3. Using the snapchat api, search for the usernames collected.

-   If the username exists, save it as one for review.

4. Review Stage

-   Visit each profile in our "review" list and take a screenshot.
-   Pass that screenshot into a photo ML nural net. (this net would be trained to look for the size/font at or near the bottom of the image, where links appear in the Snapchat platform.)
-   I have some friends in image recognition at CMU so I'd probably query their experience for a good library/system to use.
-   If nural net rings positive, that user account and email are banned on Tinder under the Terms of Service not to sell products/services unless explicitly using the Tinder Advertising API.
-   Update the server side Tinder profile with a new time stamp for "lastReview". Don't want to scan the same account everyday!
-   Provide a dispute link so that any false positives can be properly remediated
-   I'd volunteer to handle those tickets so that I have a proper incentive to make sure the net is well trained before deployment.

5. Refine the algorithm.

-   This logic of this service should be reviewed _at least_ monthly to be sure it is functioning using best practices when it comes to sorting/processing complexity,maintaining encryption in transit and at rest, and using the cheapest AWS instances( during off peak hours with proper horizontal scaling out AND in).

6. Advocate for Data structure reform.

-   If users could fill out a field for snapchat/instagram/fb etc, they would be self identifying the usernames that is being parsed in step 2. String parsing (especially in thousands of localized languages) is incredibly complex so this update to the data structure would allow this algorithm to run more efficiently could also be potentially expanded to include a audit of other social site.

# In Closing

I don't work at Tinder, but the fact that not all the accounts I swipe past are scam means that they are doing good work. I've found an edge case and conducted a gedanken as a mental exercise. No part of this is meant to comprimise any Tinder services or products. I'll probably still continue to use the service, but hopefully there's someone employed already to be working on this problem.

If you have anything you would like to add please write to me! I'd love to hear from you and refine this procedure futher.

-Kyle
