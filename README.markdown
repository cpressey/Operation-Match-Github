Operation Match, Github edition
===============================

`omg` is a command-line Python 2.7 script which uses the Github API to find
Github users with similar tastes, based on the repositories they've starred.

This code is in the public domain and comes with NO WARRANTY; see the file
UNLICENSE for more information.  By opening a pull request on this repository,
you agree to release your proposed changes into the public domain, under this
same UNLICENSE, as well.

Quick Start
-----------

Install the `requests` Python package, if you don't already have it installed,
possibly by running

    pip install --user requests

Clone the repo:

    git clone https://github.com/catseye/Operation-Match-Github
    cd Operation-Match-Github

Run the script:

    script/omg --login=yourgithublogin somegithubuser

Sample Run
----------

    % script/omg --login=catseye catseye
    Password:
    getting catseye's stars...
    getting BleuLlama/BLBasic's stargazers...
    getting BrianRamsay/Randomizer's stargazers...
    getting CRogers/Repent's stargazers...
    getting ChadMcKinney/Lich.js's stargazers...
    getting CirnoTheGenius/CallMeMaybe's stargazers...

> ...and it goes *on* like this.  Go mow the lawn or something

    getting whily/yalo's stargazers...
    getting workmajj/boogiewoogie's stargazers...
    getting xrchz/vml's stargazers...
    getting yaxu/Tidal's stargazers...
    getting yinwang0/pysonar2's stargazers...
    getting zephyrfalcon/nepenthe's stargazers...
    getting zephyrfalcon/xera's stargazers...
    
    -------------
    
    Based on starred repositories, the Github users with tastes
    most similar to catseye's are:
    
    https://github.com/8l
    with 33 stars in common:
    * https://github.com/aliclark/hasp
    * https://github.com/augustl/halt
    * https://github.com/carlini/rhoscript
    * https://github.com/clemahieu/mu
    * https://github.com/crcx/retro-language
    * https://github.com/cthulhuology/firth
    * https://github.com/darius/ichbins
    * https://github.com/digego/extempore
    * https://github.com/diiq/eight
    * https://github.com/emdash/goof
    * https://github.com/erlware/joxa
    * https://github.com/faylang/fay
    * https://github.com/grahamedgecombe/arc
    * https://github.com/griwes/ReaverOS
    * https://github.com/hellcoderz/schemec
    * https://github.com/jckarter/clay
    * https://github.com/krig/LISP
    * https://github.com/leepike/Copilot
    * https://github.com/libcpu/libcpu
    * https://github.com/matijapretnar/eff
    * https://github.com/msullivan/LazyK
    * https://github.com/nuisanceofcats/nyah
    * https://github.com/perl11/potion
    * https://github.com/pikatchu/LinearML
    * https://github.com/pikhq/clambda-demo
    * https://github.com/samrushing/irken-compiler
    * https://github.com/sanjoy/L
    * https://github.com/santoshrajan/lispyscript
    * https://github.com/smtlaissezfaire/bcompiler
    * https://github.com/vIiRuS/brainfuck-asm
    * https://github.com/viznut/IBNIZ
    * https://github.com/weiju/bootstrap-lisp
    * https://github.com/whily/yalo
        
    https://github.com/ghosthamlet
    with 29 stars in common:
    * https://github.com/Herzult/SimplePHPEasyPlus
    * https://github.com/JeffBezanson/femtolisp
    * https://github.com/Scriptor/pharen

> ...etc...

    https://github.com/emerge
    with 26 stars in common:
    * https://github.com/Constellation/escodegen
    * https://github.com/HackerFoo/peg
    * https://github.com/JohnEarnest/Mako

> ...etc...

    https://github.com/pablomarx
    with 24 stars in common:
    * https://github.com/JeffBezanson/femtolisp
    * https://github.com/Two9A/jsGB
    * https://github.com/antirez/load81

> ...etc etc etc

About the Script
----------------

`omg` came into existence as a weekend hack, playing with the Github API
(after using it for more serious purposes in (toolshelf)[https://github.com/catseye/toolshelf].)

"Operation Match" was the name of a major early computer dating project in
the USA.  According to 
[this 1965 article](http://www.thecrimson.com/article/1965/11/3/operation-match-pif-you-stop-to/),
it ran on a computer called the Avco 1790 (footnote 1).  While `omg` *could*
be used for computer dating, the author cannot in good conscience recommend
this use case.  Try just following them first.  This is OK because on Github,
following is not the same thing as stalking.

Evaluation
----------

The use of common stars, and only common stars, as a similarity metric
for "taste" (whatever that means) is obviously a crude one.  Still, it
seems to work "alright".  When I ran it against my own account, it found I
had 33 stars in common with @8l, 18 with @automata, 15 with @takeoutweight,
14 with @munificent, and 12 with @darius — all of whom I was already following.

But at the same time, it made it clear that there are some users on Github
who will star pretty much *anything*.  Even if we assume @hcilab is some sort
of bot, there are accounts who are clearly human who have 2K, 3K, or even 5K
starred repos!  (When are you ever going to find the time to try out, much less
contribute to, all those projects...?)

So one way that future work could improve the metric would be to give a user
a higher score if they have a similar number of starred repositories as the
target user.

Another way might be to check the users' contributions (issues and pull
requests) to repositories, and give them a (much?) higher score when they
have contributed-to repositories in common.

Caveats and Notes
-----------------

Since `omg` was a weekend hack, it is not very robust.  If the Github API
returns malformed JSON, it will try the request again in 5 seconds, but if
(for example) your wifi connection burps, the script may die and all the data
it has acquired up to that point will be lost.  Pull requests to make it more
robust are welcome.

If you don't pass `--login=yourgithublogin` on the command line, the script
will use the Github API anonymously.  This is not recommended, as it will
likely make more than 60 requests per hour.

If you run the script on a user with a lot of starred repositories, or who
has starred repositories that have been starred by a lot of users, it's
going to take a loooong time.  It took about twenty minutes to go through my
~280 stars.  It took about seventy minutes to go through @terryp's ~70 stars.
That's because I tend to star obscure little repos while Terry tends to star
very popular repos.

In fact, some users and repos have so many stars that this script skips them
completely when it comes across them:
    
*   `hcilab`, who has 44K starred repos
*   `equus12`, who has 21K starred repos
*   `vhf/free-programming-books`, which has 20K stars

Pull requests to add other star-heavy users or repos to this blacklist, or
to handle these cases in a nicer way, are welcome.

Users who have fewer than 3 stars in common with the given user are not
listed, because they tend to form a "long tail" which is not all that
interesting.  (In fact, even those with 3 stars in common tends to form a
fairly long tail.  Pull requests to make this more configurable, or otherwise
better presented, are welcome.  In the meantime, the use of `script`
or similar to capture the output is recommended.)

`omg` does not distinguish "vanity stars", where a user has starred one of
their own repos.  One can make an argument that these should be ignored.
Then again, one could make an argument that these should count, too.

Pull requests for improving the similarity metric will be considered, but
there's something to be said for the simplicity here too; if you have something
really sophisticated in mind, you might just want to write your own
script, stealing whatever you like from this one (it's in the public domain.)

Related Work
------------

I would be really surprised if this sort of hack against the Github API
hasn't been done by someone already, possibly in a different programming
language and possibly with different licensing terms, but if so, I'm not
aware of it.  If you are aware of one, feel free to open a pull request
and add a link to it here.

Footnotes
---------

1.  The Avco 1790 sounds like a lovely piece of antidiluvian hardware,
    doesn't it?  Unfortunately, all of my attempts thus far to find out
    more about it by searching the web have only led back to articles about
    Operation Match.
