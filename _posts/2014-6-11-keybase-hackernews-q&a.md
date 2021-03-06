---
title: "Keybase Hacker News Q&A"
layout: main
comments: true
---

I started using [Keybase] sometime back in April. I've been interested
in doing crypto stuff for a long time, starting with using an SSH
keypair instead of a password to access the WWU CS department's
computer labs remotely.

[Keybase]: https://keybase.io

Learning more about GPG and actually figuring out how to send and
receive encrypted messages has been on my list of things to learn for
a while, but every time I had a couple spare minutes and tried to
approach it I was rebuffed by the impenetrability of the manuals for
gpg.

However, recently while thinking about who to give my remaining five
invites to, I decided to do some more research on what those more
security-savvy than I thought about Keybase.

The results were somewhat of a mixed bag. In no particular order, I
read:

- [A thoughtful critique](http://blog.lrdesign.com/2014/03/thoughts-on-keybase-io/)
- [A nice first look](http://www.tbray.org/ongoing/When/201x/2014/03/19/Keybase)
- [A response to the people decrying the upload private key feature](https://filippo.io/on-keybase-dot-io-and-encrypted-private-key-sharing/)
- [A brief look at the chain of trust in even getting Keybase](http://blog.lizdenys.com/2014/03/31/refusing-to-verify-myself/)

I also came across the discussion that occurred on Hacker News shortly
after Keybase first appeared. As so often happens when I stumble on to
a HN comment page, I ended up reading the whole thing. What I noticed
was that there seems to be some information from Max and Chris (the
founders of Keybase) that I hadn't run across on the Keybase website
itself.

So I volunteered myself for a public service. I extracted the parts of
the discussion that were interesting and relevant into a text
document. I tried to focus on the questions that got interesting
responses. I left out many little asides, and tangential discussions.

The founders of Keybase are malgorithms and maxtaco. Most of the
answers are by them, though in some cases I've preserved a chain of
replies, with only the first marked as a "question". Without further
ado:

>### General Announcement
>
>malgorithms 118 days ago
>
>There were multiple questions/comments below about this, so I felt I
>should clarify one detail about the keybase client's trust of the
>server. When the keybase client requests maria's key from the keybase
>server, it does not simply trust the public key because it trusts the
>server (or uses https - huh?).
>
>Rather, the server replies with links to tweets, gists, etc. --
>maria's public identity proofs. The keybase client does not trust that
>these are honest, so it scrapes them directly and makes sure they were
>signed by the same public key that the server provided. In other
>words, the server could reply with a different maria, and simply lie,
>but not with the real maria's github or twitter account.
>
>The server could also lie by omission, leaving out an identity. But it
>cannot invent ones that do not exist, without the client knowing.
>
>Again, the premise here is that maria is the sum of her online
>identities.
>
>The website itself is of course a different story. When you look up
>maria on keybase's website, you are trusting that keybase.io did not
>lie about her github account. Fortunately you can confirm by following
>the link to her gist, where she announced her keybase username and
>posted her key fingerprint.
>
>### Q:
>theboss 118 days ago
>
>I don't see why you don't just get the key once, allow you to verify
>it, and store it locally. It seems pointless to make all these extra
>requests to you.
>
>There's a reason that gpg does this..... Maria's twitter being hacked,
>Maria's github being hacked, Maria's Keystore being hacked....a lot
>can go wrong.
>
>There are still weaknesses like, you lie about a github and link to
>your own github, and lie about the public key. And...many others.
>
>### A:
>malgorithms 118 days ago
>
>yes, it does do this; once you're satisfied with maria's identity,
>that she's the person you want, you sign a statement to that effect,
>which you can store just locally or post back to the server. (or of
>course you can just sign her key in GPG!) The latter - posting back to
>the server - is for portability reasons. A keybase user will likely
>use keybase on multiple machines.
>
>### Q:
>riquito 117 days ago
>
>The first thing I thought about is a man in the middle attack with
>homoglyphs. I don't know if I'm paranoid, but look at this
>
>    > keybase id maria
>
>    pgp: C4B3 15B4 7154 5281 5100 1C58 C2A5 977B 0022 github:
>    mаria_leah ✓ https://gist.github.com/23423 twitter: mаria_h20 ✓
>    https://t.co/mаria_h20/523554 site: mаriah20.com ✓
>    https://mаriah20.com/X904F...
>
>I looked up for 'maria', all ascii. The answer, served by a malicious
>server, contains the first 'a' of maria in Cyrillic (check yourself,
>you'll see that 'mаria_leah' != 'maria_leah'). This would fool the
>user.
>
>Maybe the client should apply some logic as browsers do for IDN
>homograph attack to show characters not in your locale in a different
>way, or at least warn you.
>
>### A:
>malgorithms 117 days ago
>
>Hi riquito - this is a very legitimate concern, and it has to be
>reviewed individually for each type of proof keybase supports, in the
>client. With twitter, keybase, and github, you can't have a username
>containing any character other than an alphanumeric, dash, or
>underscore. Which means this kind of attack is impossible.
>
>But for future identity proofs (domains, for example, which we've yet
>to implement), this kind of attack is real. Our approach here will be
>that anything outside of normal ascii will be highlighted and
>addressed to the user, as a serious warning.
>
>### Q:
>sweis 118 days ago
>
>Even though there is a disclaimer, I think the "encrypt in your
>browser" feature (https://keybase.io/encrypt) undermines Keybase's
>security credibility.
>
>This form has essentially the same level of security as
>Hushmail. Anybody using it should consider the content exposed to
>Keybase or anyone compromising Keybase.
>
>### A:
>maxtaco 118 days ago
>
>I'm not an authority on hushmail, but it seems like they do crypto on
>the server, and the server is just trusted to throw away the keys and
>plaintext?
>
>In the keybase Web client, all crypto happens on the browser. The
>server knows no keys or data in plaintext. Of course, you'd have to
>audit the front-end JS code to believe that claim.
>
>But our intention is that the only way to compromise the Web-based
>tools would be to insert malicious JavaScript into the client's
>browser. A read-only compromise of the server yields only encrypted
>data, and the server never has access to the decryption keys.
>
>### Q:
>JustARandomGuy 118 days ago
>
>Hi Chris, a few comments:
>
>1. I like the site design, the story flow on the front page does a
>   great job of explaining what keybase is.
>
>2. I see (from the abovementioned story flow) that keys can be
>   verified by reviewing signed tweets/gists. Is this functionality
>   extendable to arbitrary links; i.e. verifying keys against personal
>   blogs, Tumblr, WordPress or does the third-party site need to
>   implement a recognized API?
>
>Again, thanks so much, and it looks like a terrific site so far.
>
>### A:
>malgorithms 118 days ago
>
>Good question! There will be no such thing as a general check, because
>-- for any identity -- the client software has to perform a check that
>a human would agree means something. For example, what does it mean
>that you own a certain blog? How would a person confirm it? Well, at
>first glance it might mean that you have the power to post a message
>there. But someone else could do that it in a comment, and so that
>wouldn't work with Keybase. So any given identity check has to match
>some human definition of what it means to have that identity. And it
>has to be publicly auditable.
>
>With twitter, it's the ability to post a tweet under a certain
>username. With owning a tumblr account, it might be something
>similar. With your known StackExchange profile it might mean posting a
>statement in a specific part of your profile. And so on.
>
>The common thread in each case is (1) that you post in a place where
>only your identity can, and (2) what you post is a signed statement
>claiming a connection among three things: (a) your keybase username,
>(b) your public key, and (3) the identity on that third party
>service. (The third one is necessary so it can't be moved elsewhere.)
>Note how twitter and github's are totally different, but achieving
>these three things.
>
>We will build out this list of identity checks, hopefully making all
>kinds of them easy to do. Everything from proving you own a domain to
>having a tumblr or reddit accoun. The definition of those checks will
>all be publicly reviewable, both in the spec and in the client, which
>is what checks them for you.
>
>### Q:
>ghayes 118 days ago
>
>Well, to follow up, could this be extended to ownership of a domain
>(via DNS txt record)? Could we use this as a means of authentication
>of a self-signed certificate for a domain?
>
>### A:
>maxtaco 118 days ago
>
>Yes to DNS, though we have to be careful here since DNS can be spoofed
>more easily than github or twitter proofs over https. I was thinking a
>slightly better way to prove ownership of foo.com would be to post a
>proof at https://foo.com/_keybase (or something similar). To spoof
>this, an attacker would have to spoof DNS and also the https
>certificate.
>
>Authenticating a self-signed domain certificate via keybase is a neat
>idea, but would probably need some browser support, unless there's a
>clever hack that I'm not thinking of.
>
>### Q:
>sneak 118 days ago
>
>I really, really want crypto, specifically, safe and secure-by-default
>crypto, to become much more usable.
>
>Despite this hope, I can't seem to help the fact that the first thing
>that popped into my head when I read their webpage is "oh, they're
>wrapping and abstracting important key authentication and critical key
>trust configuration to make it more user-friendly, and implementing it
>all in javascript. WHAT COULD POSSIBLY GO WRONG?"
>
>Even if I got whacked on the head one day and suddenly loved
>javascript, I would not use it for certain projects when I wanted to
>be taken seriously by, say, cryptographers.
>
>Then again, look at all the success cryptocat has had!
>
>### A:
>FiloSottile 118 days ago
>
>As long as the trust model is that the server is untrusted, it can be
>written in the language they prefer. As for the client, there can be
>as many implementations in as many languages as needed to make
>everyone happy IMHO (they call it reference client in the home).
>
>### Q:
>sneak 118 days ago
>
>If this talks to keybase's API over https and any large groups come to
>rely on this, we've then effectively replaced the decentralized safety
>of the Web of Trust used for authenticating PGP keys with the PKI
>that's used in browsers, which is completely and totally fucked.
>
>I cannot support a project that doesn't build and strengthen the
>underlying WoT. Getting https involved for authenticating unknown keys
>is a huge step backwards. Madness.
>
>### A:
>maxtaco 118 days ago
>
>We're not big fans of browser PKI either, but we're using it as
>scaffolding that hopefully one day can be torn down.
>
>`keybase-installer` needs an initial install over https from npm. We
>unfortunately saw no way around this.
>
>Assuming that install succeeds with integrity, then all future
>upgrades of the installer and client are verified with PGP keys stored
>locally on the client.
>
>Once the client is installed, it speaks HTTPS to the server, but we're
>not trusting the root CA. Rather, we sign with our own CA that we ship
>with the client.
>
>The proofs themselves, on twitter and github, all can be verified in
>the clear, as FiloSottile points out, but of course relying upon the
>HTTPS certificates of twitter and github to make sure the proofs
>weren't corrupted in transit between those services and the client.
>
>### Q:
>bqe 118 days ago
>
>I don't understand how using HTTPS for the API has any bearing
>whatsoever on the WoT built via PGP.
>
>You can still verify the keys with your client's cached copies, or
>using another PGP client.
>
>### A:
>FiloSottile 118 days ago
>
>Exactly, and moreover. If there is no trust in the server, everything
>can even go over unencrypted HTTP. CAs have no business here.
>
>### Q:
>IgorPartola 118 days ago
>
>OK, finally looking at it on a desktop...
>
>So my first question is this: if I know "maria" and I want to look her
>up to get her GPG key, how does keybase handle that? Does it just do
>an email address lookup, as in goes to, say, GitHub, grabs her email
>address, maria@example.com, then goes to a public key server and grabs
>the key that corresponds to maria@example.com?
>
>If that's the case, there is a security issue: what if Maria never
>published a GPG key, but Chloe did using Maria's email address?
>Moreover, what if Chloe has access to Maria's inbox and can read these
>messages I believe to be only readable by Maria?
>
>Edit: I see from responses below that various online presences of an
>identity tied to "maria" are checked. Is this not then susceptible to
>its own attack? For example, if Maria does not have a Twitter account
>and I create one, or compromise hers and post a different key, will I
>be able to at least introduce doubt into her identity, if not take it
>over outright?
>
>### A:
>maxtaco 118 days ago
>
>No, there are no proofs based on e-mail addresses, because such proofs
>are not publicly-auditable. We could ask that maria prove to the
>server that she controls a given gmail account, but there's no way for
>the server to prove that to you.
>
>We want the server to be untrusted, ideally just a dumb message
>router.
>
>If Chloe wants to impersonate maria, she'll need to get control of
>maria's twitter and github accounts. Just claiming maria's email
>address won't get her anywhere. (Note that GPG keyservers are
>susceptible to exactly the attack you describe).
>
>### A:
>IgorPartola 118 days ago
>
>Hold on. First, GPG servers are susceptible to the same type of
>attack, except they would never be used that way. You never look up a
>person by email, the send them an encrypted message using the key you
>get. Instead, you verify their key and email address out of band: you
>meet them, check their credentials, then sign the key. Keybase is
>trying to get rid of the in-person verification, an effort I applaud,
>but in favor of a much weaker check: whether a few centralized
>accounts had been compromised.
>
>The other part, where you check Maria's Twitter and GitHub accounts,
>means that a few things like Twitter, and GitHub are impervious to
>Chloe: a tall order and a centralized one at that.
>
>Once again, is the point here for me to get a tuple of (email address,
>public GPG key) so I can email Maria securely? If so, then someone
>somewhere has to prove that this tuple fetched from the public key
>servers is valid.
>
>If the point is to only communicate via keybase.io, then the service
>is centralized, and useless once actual sensitive info is exchanged,
>the US government takes notice and shuts it down at the DNS level.
>
>### A:
>maxtaco 118 days ago
>
>Cool, I agree no one should use PGP servers the way I described, but
>you never know what people are doing out there. To do things the
>proper way, as you described, is difficult in practice for lots of
>people.
>
>To answer the question, the point isn't to get an (email address,
>GPG-key) mapping. It's to get a (public-internet-identity, GPG-key)
>mapping. People sometimes do this today in an adhoc manner
>(e.g. tweeting your GPG fingerprint). We want it to be checkable by
>user-friendly software.
>
>### A:
>IgorPartola 118 days ago
>
>I see. That MO makes a bit more sense then, though is it not then
>limited to just Keybase.io and will no longer work if something
>happens to this service? Or more importantly, is there a way to make
>this distributed?
>
>### A:
>maxtaco 118 days ago
>
>If the site went away tomorrow, you'd still have keys in your GPG
>keychain. You'd also have a local cache of the server-side data
>relevant to you.
>
>All public server-side data is available as a dump
>(https://keybase.io/__/api-docs/1.0#call-dump-all). Private data like
>encrypted public keys and password hashes we of course will keep under
>wraps.
>
>We don't have immediate plans to make the system distributed, but if
>someone did it, we'd find it very cool. It's just too much for us to
>do right now.
>
>### Q:
>theboss 118 days ago
>
>Where are the security details published? I think that's what we all
>want to see...
>
>On top of this....I think this is cool in theory but bad in practice.
>
>The assumption that Root CA's are trustworthy is already hard enough
>to make, how do I know that Maria is actually Maria? How will you
>verify that ``Maria'' actually owns that twitter, github, gmail. Maybe
>it is possible to devise some type of scheme for those sites, but how
>about more obscure services?
>
>One mistake in one single account causes the entire thing to fall
>apart...
>
>### A:
>maxtaco 118 days ago
>
>As Chris said, we would like to publish everything, just haven't found
>the time yet. We have bits and pieces in wikis in our various github
>repositories (almost all of which are open source and public).
>
>The high bits are: all crypto is with GPG/RSA as per RFC4880. There
>are of course problems here, but we wanted backwards-compatibility and
>well-tested, well-used clients.
>
>We encrypt server-stored GPG private keys (if you choose to use that
>option) with TripleSec (see https://keybase.io/triplesec).
>
>Users use GPG to sign a series of JSON objects, of the form "I'm
>maxtaco on twitter", or "I checked Chris's proofs as of 2014/2/14 and
>they look good to me." All JSON objects that a user signs are chained
>together with SHA-2 hashes. So a user can sign the whole group of JSON
>statements by just signing the most recent one.
>
>Here's an example (click on "Show the Proof")
>https://keybase.io/max/sigs/ZnBizHMA8RKSB598TaDtjlPlLKSEu1Wu...
>
>There's a fair amount of engineering that went into the software
>distribution system. We rely first on npm to get the initial client
>out there, but after that, exclusively GPG for code-signing. That's
>documented here:
>
>https://github.com/keybase/node-installer/wiki/Update-Archit...
>
>We hope to have better documentation soon, and we value feedback, we
>just haven't had the time to put it together yet.
>
>### Q:
>caf 118 days ago
>
>The idea here isn't that you use keybase to find out Maria's twitter,
>github or gmail identities - it's the opposite. The idea is that you
>already know who Maria is on one or more of those services, so the
>fact that the account you know is Maria's at github has posted a
>signed message from that public key is supposed to testify to you that
>that is really your Maria's public key.
>
>You could of course manually review and verify Maria's github post
>that contains her public key - all that keybase is really doing here
>is providing an easy way of discovering that github post (or tweet, or
>whatever).
>
>### A:
>andrewaylett 117 days ago
>
>s/could/should/?
>
>### A:
>caf 116 days ago
>
>Only if you don't trust your copy of the keybase client (as opposed to
>the server, which you should not need to trust).
>
>### Q:
>theboss 118 days ago
>
>When I get someone's GPG key I can call them on the telephone or go to
>their house and make sure I got the right one.
>
>I add it and use it. When you use this, I'm assuming I get that key
>every time from the server. I can get it and verify it once, or twice,
>or three times, but what about the 1000th time? What happens when I am
>important enough that they return a public key that is not Maria's,
>and I am none the wiser.
>
>### A:
>malgorithms 117 days ago
>
>boss, I'm glad you answered this question. Because it explains the
>impetus for Keybase.
>
>I think what Keybase is addressing in the status quo is twofold: (1)
>sadly, almost no one does what you describe; in person meeting key
>exchanges and webs of trust may sadly be as unpopular in 20 years as
>they are now, and as they were 20 years ago. People who go to them are
>often confused, even programmers. I wish it were different.
>
>And (2) more important, in 2014, often the person you're dealing with
>is someone whose digital public identity is what matters, not their
>face in real life or phone number. If you know me online as
>github/malgorithms and twitter/malgorithms, to get my key, meeting
>someone in person or talking on the phone to someone who claims to be
>me is actually less compelling than a signed statement by malgorithms
>in all those places you know me.
>
>And if you do know me in real life, then I can tell you my keybase
>username and fingerprint, exactly as you're used to. So it's still as
>powerful for meeting in person. With the added benefit you can confirm
>my other identities, which you likely know to.
>
>In answer to your scenario about verifying: you only need to review
>the "maria" the server provides once, and then your private key signs
>a full summary of maria -- her key and proofs. Cases 2 through 1000 of
>performing a crypto action on maria involve you only trusting your own
>signature of what "maria" is. The client can query the server for
>changes to her identity, and this will be configurable; if maria adds
>a new proof, you might wish to know.
>
>### A:
>jonesetc 117 days ago
>
>This is what I assumed the answer would be, and at this point it just
>becomes a difference in opinion. I personally do not believe that the
>methods you describe are generally acceptable options in the modern
>age. My phone number and address are much more important to me than
>the off chance of someone capturing my https traffic, breaking it, and
>inserting a fake public key. There is a point where the absolute
>security of exchanging public keys written on pieces of paper in a
>park are called for, but it's not for everyone or even most.
>
>### Q:
>jonesetc 118 days ago
>
>Is there a way to remove an associated account?
>
>### A:
>maxtaco 118 days ago
>
>Yes, though it might be broken right now. Our plan is to allow this,
>for sure.
>
>### Q:
>diasp 117 days ago
>
>Interesting. Another approach is https://encrypt.to/ which loads the
>public key from key servers and encrypts client-side via JS.
>
>### A:
>malgorithms 117 days ago
>
>To clarify the difference, it seems encrypt.to is a service which does
>PGP crypto in the browser, based on keys pulled from keyservers. In
>contrast, Keybase is an identity-proving service, which proves key X
>belongs to person with twitter account Y, github account Z, etc. As a
>convenience, it also does encryption and other crypto actions for its
>users.
