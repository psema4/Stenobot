STENOBOT
========
This system was developed to conduct voting and IRC meeting moderation
activities for the Pirate Party of Canada. It is made up of three parts,
which share a common database. It is currently in early beta, and is far from
feature-complete.

First, edit *./config.php* to match database and IRC server settings. This
affects all three parts of the program.

Next, create a new database for Stenobot, then run the following MySQL command:

    mysql -u[username] -p[password] [database name] < stenobot.sql

The program is divided into the following sections:


*./bot* - The IRC bot, which operates as a PHP shell script.

*./www* - The web interface for the bot.

*./vote* - The web interface for voting. This is mainly independent from the other two.


BOT
---
To run the bot, simply type:

	cd bot
	./stenobot.php

All other interaction can be done via the IRC interface. For details, use the
IRC command:

	/msg sb help

sb is the default helper nick, but it can be changed in the configuration file.
If needed, it is also possible to inject raw IRC and PHP commands into the
script at run time via the CLI. Any command preceded by 0 will be interpreted
as an IRC command to be run by the main bot, 1 for the helper bot. All other
commands will be run as PHP code.

The bot itself is mostly functional, though it is missing some features that
have been requested by the Pirate Party's Chair.


WWW
---
While it contains some half-functional code, the only page used by the bot is
login.php. This folder is designed to be operated at the root of a domain or
subdomain. It is fairly explanatory, and will be linked by Stenobot when needed
(before a meeting begins, and by PM whenever a new user joins). Only logged-in
users are able to speak and participate.


VOTE
----
Like www, this folder should have its own independent domain or subdomain. It 
is used exclusively for voting. Motions and votes are not presently added to
the database automatically, so this must be done by hand. The database
structure is fairly straightforward and should require little explanation.

Similarly, vote results can be pulled from the raw numbers in the database
using the following query:


    SELECT a.name, DATE_FORMAT(a.start, '%Y-%m-%d') AS date,
	    (SELECT COUNT(*) FROM votes AS b WHERE b.vid = a.vid AND b.vote = 'y') AS y,
	    (SELECT COUNT(*) FROM votes AS c WHERE c.vid = a.vid AND c.vote = 'n') AS n,
	    (SELECT COUNT(*) FROM votes AS d WHERE d.vid = a.vid AND d.vote = 'a') AS a,
	    IF(a.status = 'voting', 'yes', '') AS open
	    FROM motions AS a
	    ORDER BY start DESC


As this is used by an officially bilingual party, there is a significant amount
of hard-coded language translations. This will need to be customized to suit
other implementations.

This is the least complete part of the software.


This software will be posted to github.com/piratepartyca shortly.
