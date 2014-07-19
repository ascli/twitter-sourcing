# Twitter-sourcing via the command line

Gathering authoritative sources using the Ruby command-line Twitter interface and CSVKit.


## Prerequisites

- Install Ruby 1.9.x or 2.x
- Install the [t gem](https://github.com/sferik/t), maintained by Erik Michaels-Ober
- Install Python 2.x or 3.x
- Install [CSVKit](https://github.com/onyxfish/csvkit), maintained by Christopher Groskopf 



## Steps

### Setup

To follow along:

- __Authenticate with Twitter__ by making a test app and getting an access token: The [t gem documentation can walk you through the configuration](https://github.com/sferik/t#configuration). You can create an app with __read-only__ access for now.

- __Make two sub-directories__ to keep things a little more organized, named `source-files` and `twitter-data`
    ```sh
    mkdir my-drafts
    mkdir my-data
    ```



## Hello `t` 

Let's try out the __t__ command-line utility's basic functionality. Typing in `t` will bring up a list of commands:

```
    t
```


```sh-output
Commands:
  t accounts                          # List accounts
  t authorize                         # Allows an application to request user...
  t block USER [USER...]              # Block users.
  t delete SUBCOMMAND ...ARGS         # Delete Tweets, Direct Messages, etc.
  t direct_messages                   # Returns the 20 most recent Direct Mes...
  t direct_messages_sent              # Returns the 20 most recent Direct Mes...
  t dm USER MESSAGE                   # Sends that person a Direct Message.
  ...
  t whois USER                        # Retrieves profile information for the...
```

The `whois` command, followed by a user name, will print out profile information:

```
    t whois neiltyson
```

```sh-output
    ID               19725644
    Since            Jan 29  2009 (5 years ago)
    Last update      Argue all you want about the physical world, but Nature is the ultimate arbiter: serving as judge, jury, & executioner. (10 hours ago)
    Screen name      @neiltyson
    Name (Verified)  Neil deGrasse Tyson
    Tweets           3,905
    Favorites        0
    Listed           29,805
    Following        47
    Followers        2,177,367
    Bio              Astrophysicist.  Author: Space Chronicles & Inexplicable Universe [Video]; Host: @StarTalkRadio & @COSMOSonTV
    Location         New York City
    URL              http://www.haydenplanetarium.org/tyson/
```


### Using the `--csv` flag

By default, `t` prints out a human-readable version of the user profile. By adding the `--csv` flag, we can specify that we want the data in a machine-parseable output, i.e. comma-separated-values:

```sh
  t whois --csv neiltyson
```

```sh-output
ID,Since,Last tweeted at,Tweets,Favorites,Listed,Following,Followers,Screen name,Name,Verified,Protected,Bio,Status,Location,URL
19725644,2009-01-29 18:40:26 +0000,2014-07-18 15:27:09 +0000,3905,0,29805,47,2177378,neiltyson,Neil deGrasse Tyson,true,false,Astrophysicist.  Author: Space Chronicles & Inexplicable Universe [Video]; Host: @StarTalkRadio & @COSMOSonTV,"Argue all you want about the physical world, but Nature is the ultimate arbiter: serving as judge, jury, &amp; executioner.",New York City,http://www.haydenplanetarium.org/tyson/
```


This CSV format is especially suited for spreadsheet work. [Check it out here](examples/whois-neiltyson.csv).

### Collecting a user's tweets

Let's try another `t` command: with `timeline`, we can get the specified user's latest tweets:

```sh
  t timeline neiltyson
```

```sh-output
   @neiltyson
   Argue all you want about the physical world, but Nature is the ultimate 
   arbiter: serving as judge, jury, & executioner.

   @neiltyson
   JUST POSTED: @StarTalkRadio's Cosmic Queries: "Space Probes” with guest host 
   @AmyMainzer. On @iTunes & http://t.co/2Jgkwzj5Hs

   @neiltyson
   @isoprene Thanks for the eagle eyes. Just fixed the typo.
```


Again, using the `--csv` flag, we can get the tweets in a spreadsheet-friendly format.

```sh
t timeline --csv neiltyson
```

```sh-output
ID,Posted at,Screen name,Text
490155821150375937,2014-07-18 15:27:09 +0000,neiltyson,"Argue all you want about the physical world, but Nature is the ultimate arbiter: serving as judge, jury, & executioner."
488745029263831040,2014-07-14 18:01:10 +0000,neiltyson,"JUST POSTED: @StarTalkRadio's Cosmic Queries: ""Space Probes” with guest host @AmyMainzer. On @iTunes & http://t.co/2Jgkwzj5Hs"
488551070520848384,2014-07-14 05:10:26 +0000,neiltyson,@isoprene Thanks for the eagle eyes.  Just fixed the typo.  
```


### Saving data to a file

Hopefully you're now wondering, _This is all nice, but how do we get this Twitter data into Excel?_ There's always the obvious way: open your text-editor, make a new blank file, highlight and copy the output that you see in the Terminal, paste it into the blank file, and then save as a CSV.

But since we're already at the command-line, let's do it the UNIX way by _redirecting_ the output of the `t` command into a text file:

```sh
t timeline --csv neiltyson > drafts/timeline-neiltyson.csv
```

(make sure you've already made the `drafts` subdirectory)

You shouldn't see anything printed to screen. But check out your `drafts` directory and you should see a file named [timeline-neiltyson.csv](drafts/timeline-neiltyson.csv)

Going forward, this is the data pipeline we're going to work with:

1. Get data from Twitter's API, using one of the `t` commands
2. Saving the output of that operation to a CSV file on our hard drive.
3. Re-opening and re-reading that file at a later time, perhaps using its data as fodder for more commands.

Saving files, re-opening files, re-saving them...It seems very clunky, like old-fashioned _manual_ labor, and not slick automated computing. But for the main task at hand &ndash; gathering a list of experts &ndash; we don't need anything faster. Maybe later, you'll see how the groundwork we lay now can be converted to a real-time web service. But that can be another project. 

For now, you'll hopefully find that the command-line is so fast that you'll have more than enough data and ways to slice and dice it to keep you occupied.


## Working with input files

## Parsing structured data

(and so forth)

