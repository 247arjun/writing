# DEFCON 29 AppSec 101 Talk outline


## Start
Hey everybody, I hope you're all doing well and staying safe. Welcome to AppSec 101 at DEFCON 29. Over the next 30 minutes, we're going to take a Journey from Engineer to Hacker, as we gain empathy for both sides of the SecDevOps coin.

## Cyber Fried Chicken
Before we jump in though, I do want to call out that this is a 101-level talk, approachable to people who may be unfamiliar with Application Security. It's what I call "Cyber Fried Chicken". Why do I call it that?

Well, let's take a quick bite - just like you can eat a single chicken wing and say you've tasted and eaten chicken, it's still just a small part of the whole chicken. Similarly, this talk will give you a broad understanding of the field, without diving too deep or pretending to give you the whole bird.

Also, I love chicken wings and needed an excuse to pull that into this talk, so here we are. 

Clucking along... in this talk, we will learn what AppSec is and how it is applied to secure software engineering - we'll look at a fictional example of insecure code development, and contrast that with SecDevOps, so we may all leave here with a better appreciation for the role Application Security plays in modern software engineering.

But first, a word from our sponsor.

## Tatter
Have you ever felt like there were too many social networks to keep tabs on? Like there needed to be better consolidation of our feeds, so we can spend less time in front of our screens and more out in the real world?

Of course not!

Presenting, the newest anti-social network - Tatter. 

Tatter is launching soon on the web, mobile, AR/VR and everywhere else your WiFi network reaches. Sign up for an account, or multiple accounts, today. 

Tatter - it's where your friends aren't.

## Case Study
To be clear, Tatter is entirely made-up. However, let's use it as a case study to see how the Tatter team cut corners in software development, and the consequences of their actions. We generally only get this kind of hindsight with an Equifax or a Yahoo breach. Let's instead, learn from Tatter and their missteps.

Then, let's time-travel and fix things by ensuring Tatter follows AppSec practices. 


## BAD (OWASP Top 10)
Tatter started as an anti-social network from an Ivy League college, where a bunch of students got together and started writing code from their dorms. Let's call them Mark and Zuck. They were able to leverage their connections with VCs to get funding to hire more engineers to - as VCs would call it - increase velocity.

Freshly funded and on a full belly of VC money, Tatter hired engineers to add more features to Tatter. They added a Dislike button, photo uploads, advanced search and many other features.

In the race to launch publicly, many corners were cut - technical debt accrued. The focus was on velocity and not veracity. Mistakes were made, but not caught. Yet.

As Tatter grew, they added Marketing, Legal and eventually HR departments - limiting their engineering workforce to just developers. Things were looking good for a product launch soon. Mark was happy, Zuck was happy, the VCs were happy, the engineers were happy.

From a soft launch to an open launch, Tatter saw increasing customer usage and engagement. In their first months, they grew their user base, all the while focused on growth internally as well. 

One fine Friday evening at 4pm, news broke that Tatter was hacked. The news spread like wildfire, picked up by major news outlets. The Tatter team would have to pay up their technical debt it seemed.

## How were they hacked?
Let's spend a few minutes learning how Tatter was hacked. This will give us insight into what went wrong at Tatter.

Tatter had a front-end (a website and an application) and a back-end (a set of servers) and an API that linked the two.

Let's look at each of these and spot the vulnerabilities.

### The mobile apps
Tatter was meant as a mobile-first anti-social network. The focus of the engineering team was around the mobile user experience, with a lot of attention paid to the visual design more than the entire architectural design of the app. As a result, many shortcuts were taken to launch, and get the apps reviewed and published on time.

One such shortcut was that the API key the app used to communicate with the backend using the Tatter API was stored in the code of the app itself - as a string in the resources file. When hackers got hold of the app packages, they were able to extract the resources from reverse-engineering the app package to find the API key.

### API
When signing up for a Tatter account, a user was prompted to enter their email address as part of the sign up process. This is normal.

However, if an account already existed that used the same email address, Tatter would prompt the user to sign in instead - pre-populating the username of the account. This seems like a convenience until you realize that anyone can abuse the API and cycle through a bunch of email addresses and find the associated usernames. Hackers used the API key they found from the reverse-engineered apps to communicate with the API.

Additionally, the API didn't rate limit or lockout failed login attempts after a many attempts. This allowed automation to find many usernames, and crack a bunch of passwords (either using password spraying or credential stuffing).

### The website
A given user's profile on tatter.com could be navigated to using their username (so tatter.com/users/johndoe for example). From the list they obtained by abusing the API, hackers were able to scrape the profile pages of many users, including that of the site admins (their accounts were registered to admin@tatter.com, and they now had the usernames of that account). 

The admin accounts also used poor password choices, and were easily cracked. Hackers were able to sign in to the admin accounts using just the username/password, and view all profile metadata.

### Source Code
The engineers used a private GitHub repo for source control. They reused both the emails and passwords used for their Tatter accounts for GitHub as well, and again, no 2FA/MFA was used. Hackers were able to view source code for all of Tatter - product code, infrastructure code, everything. 

Here, hackers found DB connection strings, other secrets in code and more.

### Backend
Using the newly discovered credentials, hackers were ultimately able to breach, access and exfiltrate the cloud stored customer data. Tatter also stored customer passwords as MD5 hashes, so hackers were also able to crack a large number of customer passwords - that were invariably used in multiple other sites.

The keys to the kingdom were theirs, and Tatter didn't have a clue.


## Outcome
For trivia - This is an image of the wildfires in California from last year as seen from the International Space Station. This is what happens when a fire goes unchecked till its impact is massive. Now if you could anticipate that a certain set of actions was going to result in a massive wildfire, would you extinguish the flames when the match was lit, or the campfire got out of control or when millions of acres were burning? 


## SDLC
We're all aware of the Software Development Lifecycle, that decomposes the development process into stages. The SDLC has traditionally (in a waterfall model) comprised an initial requirements gathering phase, followed by a development phase, a testing phase and then release. In this traditional model, testing (especially security testing) happens only after the product has been built to some degree of maturity and usability.

## AppSec
Application Security is the process of finding, fixing, and preventing security vulnerabilities at the application level, as part of the software development processes. This manifests as multiple parallel processes that aim to find and address issues early in the SDLC. This is also called Shifting Left, since we are moving the security testing part further left in the SDLC.

## Time-travel
Now let's travel back in time to when Tatter began as an idea. Let's infuse AppSec into their engineering processes and follow the development and see where they end up.


### Security Requirements


### Threat Modeling
As part of the system design, Tatter developers and security engineers collaborated on a threat modeling exercise to identify the key assets that need to be protected, the trust boundaries between internal assets and external actors, and to ensure that appropriate controls existed to secure them against threats.

To keep the content approachable to all collaborators, a diagrammatic approach is used in the form of a Data Flow Diagram (DFD), where the flow of data from external users, through internal infrastructure is followed.

Threat modeling is used to ensure that controls exist to prevent Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service and Elevation of Privilege (STRIDE).

### Code Reviews
Code Reviews are an essential part of collaborative software development. When you have code you think is ready to be checked-in to the product, you seek review and approval from peers. This is an effective way to get an extra set of eyes on any new code added. You gain more value from this if the reviewers are well versed in security concepts, to catch issues before they're checked-in - so obvious issues like storing API keys in code, logic flaws etc.

### SAST
Once code has been committed to a product's codebase, additional sets of tools can be used to scan this code. 

Based on whether the scanning needs the code to be raw source or dynamically executed, we have Static AppSec Testing tools, and Dynamic AST tools.

SAST tools can find security issues that might've been missed in code reviews - so things like secrets in source (using RegEx and entropy), insecure query construction without input sanitization etc.

#### Demo of SAST (ShiftLeft on GitHub)
Let's see a quick example to drive this point home, using a security vendor's product that does this on GitHub repos. In my repo here, I run the tool against the source of a given project. Based on the tool's rules, it flags a whole host of security issues, based on criticality. I see some critical issues flagged for insecure query construction. I also see some secrets in code that were accidentally committed.

Incorporating this as part of your Continuous Integration pipeline will enable a per-commit monitoring of hygiene.


##### Secrets in code
We talked about secrets in code a few times now, but why is it such a big deal?

For many intermediate processes that any app does, security is a big part of those transactions - often this security depends on the usage of a secret. This could be an API key, an SSH key, OAuth tokens etc. Accidentally committing any of these into source is dangerous due to how easily they may be found - as we saw with Tatter.

To store these secrets securely, use a key vault or secret store - which allows programmatic access and retrieval of the stored secrets.

There is no such thing as a partial secret compromise. Any accidentally disclosed keys need to be changed.

##### Unsafe functions
Another example class of issues that a SAST is great at finding is unsafe functions - these may be missed in a human code review. For example, the C function `strcpy` (used to copy a string from a source to a destination) is unsafe because it doesn't check if the destination has enough space to store the source string. Using this can lead to a buffer overflow, which may be exploited. A SAST will flag the use of unsafe functions.

##### SQLi

### SCA
In today's complex interdependent software systems, where your code interacts closely with other code from dependent libraries, it is important to protect the full tree of dependencies. It's possible that your code is secure, but you are dependent on an insecure library.

Sofware Composition Analysis is used to better understand the dependencies that your product uses.

### Fuzzing
Let's understand fuzzing with a joke I read. 

A QA Engineer walks in to a bar, and orders 1 beer, 2 beers, 100 beers, -1 beers, 1 bear. 

An actual customer walks in to the bar and asks the bartender where the restroom is located. The bartender halts and catches fire.

Fuzzing is the process of feeding a system unexpected input to see if it can handle the unexpected input appropriately , and if needed, fails gracefully and safely.

### DAST
OWASP ZAP, BurpSuite

#### Demo of DAST (OWASP ZAP on Juice Shop)

## Outcome
In this version of reality, Tatter launched succesfully. Its customer data was kept secure, thanks to early identification and remediation of security issues. This was the result of a good Application Security program that worked with DevOps to prioritize product security and Shift Left succesfully.

Tatter was praised for its privacy and security, and over time gained enough users and revenue to allow its CEO to purchase a ticket to space in the near future. 

## Lessons and Resources
Tatter had a good run the second time around. They had a successful AppSec program that allowed them to find security issues early and fix them when the impact and cost were still low. 

## Wrap-up
Everyday, many insecure Tatter-esque apps are launched globally. It's our job to prevent them from ever seeing the light of day.

## Closing
I'm reminded of my first defcon 27, in Hacker Jeopardy, the answer Ring 0 kept coming up. Ring 0 represents the highest level of trust for any entity. We are all Ring 0. Let's each do our bit - pun intended. 

Thank you everybody for joining me today. I hope you have a better appreciation for Application Security, and I wish you an amazing defcon 29. Take care, stay safe and keep hacking!

If you'd like to learn more about Tatter, get access to the resources, a transcript of the talk - for translation or accessibility - check out tatter.dev.
