# OUISwebsite

This rep contains all files necessary to build the OUIS website https://oxforditaliansociety.org/.

If you're exploring this, you're probably somehow responsible for the management of the website. This guide should help you navigating through the maze of files so that you know what to write where for the the website to show the info you want. I'll try to keep it as low-level as possible, so that even if you don't have any previous experience in programming/website management, you should still gain enough knowledge to do what needs to be done. Nonetheless, if you _do_ have previous knowledge about gitHub, the command line, and programming at large, things will get oh so much easier.

The guide is (planned to be) organised as follows:
1. A list of things to do/know to get set-up and running
2. How to make shallow changes to the website (ie, adding events, changing committee members, ...)
3. How to make deep changes to the website (ie, changing layout, adding pages, ...)
4. How do websites work
5. Why did I make this so complicated rather than just using damn wix

__Section 1__ is necessary to do anything, and most of your work should be revolving around __Section 2__. __Section 3__ starts getting tricky, and for that I would strongly advice that you knew what you're doing before attempting following anything of what's there. __Section 4__ can also be helpful in case you want to change host/expand your website's capabilities. __Section 5__ is to try and convince you that this setup makes sense and you shouldn't hate me (which will undoubtedly happen when things'll get complicated and technical down along this guide).

As a disclaimer: most of the information in this guide is self-taught and stems from a tedious trial-and-error combined with some googling. When I approached this my experience in websites was close to nil, although I was fairly confortable with programming in general. This is to say, what's in this guide _does the trick_ but lacks a strong background: if you already know your way around websites and stumble upon this, don't judge me too harshily. On the other hand, this proves that even if you have close to nil prior experience, you can get a website up and running quite easily: ain't that amazing?
____


## How to get set-up and running

You'll need a couple of programs before you can start playing around with the website: namely, [git](https://git-scm.com/ "Git website") and [Hugo](https://gohugo.io/ "Hugo website"). If you haven't them already, make sure to install them in your machine. On ubuntu and macOS, it should be as easy as writing a couple of lines in your terminal: the guides online for [git](https://git-scm.com/downloads "git install") and [Hugo](https://gohugo.io/getting-started/quick-start/#step-1-install-hugo "Hugo install") will tell you everything you need to know on how to do it.

Chances are you've already stumbled across git if you've done any programming: it's a tool used for version control. In a nutshell, it allows to keep track of the modifications done over time to a set of files in a folder or, more accurately, a _repository_. GitHub provides a nice online interface for git: if you're reading this, you are currently exploring the gitHub repository containing all files related to the OUIS website. If you're managing the website, you're going to have to keep this repository up to date, so you've got to learn how to do stuff like _cloning_, _committing_ and _pushing_. If these terms are familiar to you, that's it for the gitHub part; if not, you should spend some time [looking them up](https://help.github.com/en/articles/github-glossary "git glossary"). As a first step, [clone](https://help.github.com/en/articles/cloning-a-repository "how to: clone") the repository on a folder in your local machine. You'll use gitHub to keep track of the modifications to your files somewhere safe, so that in case something goes horribly wrong and you mess up the website terribly, it should be straightfoward to _revert_ back to a valid version of it.

Hugo, on the other hand, is a website generator. It takes a bunch of files in a specific structure and it organises them in a format that's readable by a host, so that it can show your shiny website properly. You'll mostly use Hugo to check that you're modifying the website correctly, before you put the updated version online. This way you can be sure that what you're doing is right, without the whole internet laughing at you because you horribly broke the layout of the page.




## How to make __shallow__ changes to the website

The term has changed and new events are coming up. After the elections, the members of the committee have changed. Prices for membership / classes have raised due to inflation. These actions are examples of what I define _shallow_ changes: as opposed to _deep_ changes, the former just amount to straightforwardly modify some text files in this repository. No need to know html, javascript, php, or any programming at all for what matters. The idea was to make the website flexible enough so that it could easily accommodate for these day to day (term to term?) changes. This section deals with how to do this.

Hopefully you've already gone through __Section 1__: Hugo is installed and you've successfully cloned the OUIS website repository in a folder on your machine. Now, open a terminal, [`cd`](https://www.computerhope.com/unix/ucd.htm) into that folder and write
```shell
hugo server -D
```
Leave the terminal be: instead, open your favourite browser, and visit the page http://localhost:1313. If everything went well, you should be able to see a twin version of the actual OUIS website. The only difference is that this version lives locally: your laptop has been turned into a host for this local version. For as long as hugo is running and you don't close or otherwise interrupt the process in the terminal, http://localhost:1313 will show how the website would look like if you were to put (_deploy_) it online. Even better, it would do so in real-time: if you modify the folder locally, that would change almost instantaneously the website on http://localhost:1313, without touching the actual online version, though, and this is very important (I'll talk about how to deploy things online later).

Before actually starting to change things, it's good to spend a couple of words on the structure of the repository itself, so that you know how to look for the files you need to modify. For shallow changes, you'll have to focus on the folders content and data, as well as (possibly) on the file `config.toml`.

### /content/
Here is where you need to go to change/add events. The content of this folder mimics the page structure of the website: there's a `/classes/` page because there's a `/classes/` folder, and so on. Hugo takes care of creating pages accordingly, but includes them in the website only if there is a `.md` file somewhere inside the folder. That's why you see loads of almost empty `.md` files inside there. Every event has a page associated to it, which is why you'll have to create an `.md` file for every event that gets organised. Go to the events subfolder and you'll see it's already populated with some of the events held so far. Give them a meaningful title, as this will name the corresponding page in the website (the url will be `/events/<name of the .md file>`). The format `yyyy-mm-dd-name-intervalled-by-iphens.md` seems reasonable to me, and _it would be nice_ if it was kept consistent. What _needs_ to be kept consistent, instead (lest visualisation won't work properly) is the format of the `.md` file itself. Open one of the `.md` files with your favourite text editor, and you'll see the following format:
```yaml
---
types : speaker
image : images/events/roberto-d-agostino.jpg
week : 2
old: false
startDate : 2019-05-10T17:30:00Z
endDate : 2019-05-10T18:30:00Z
place : New College, LR6
title : Roberto d'Agostino
---
We are delighted to be hosting ...
```

The section beginning and ending with `---` identifies a block containing a bunch of variables, and this followed by a human-readable description of the event. Here's what each variable means/does:
- `types: tag1 tag2 ...` This assigns some tags to each event, so that it can be recovered easily using the filter buttons in the main page (clicking on a filter only shows events with that tag). For now the tags available are `speaker`, `cinema` and `social` (although more can be added by changing the corresponding `.yml` file in `/data/`). Any subset of these tags can be added to types (just interval them with a simple space), and if an event has more tags, it will be identified by more filters. Simple as that.
- `image: path-to-image` This assigns a main image to the event. The image file should be put in the folder `/images/events/`.
- `week: number` The term week number the event is held in.
- `old: false/true` Whether the event is old (`true`), and hence should be shown only on the events page, or if it is instead new (`false`) and should be shown in the main page. When an event turns old, please modify the corresponding file.
- `startDate : YYYY-MM-DDTHH:MM:SSZ` Date and time of the beginning of the event. Notice __the obnoxious format needs to be preserved__! It is used internally by Hugo to handle it properly as an actual date and to show it nicely. If you mess up with it, you'll probably start seeing weird numbers instead of the correct year, for example.
- `endDate : YYYY-MM-DDTHH:MM:SSZ` Same as above, but for the end of the event.
- `place : wherever` Info on the place of the event. This is going to be read as is by humans, so you can write anything meaningful, really.
- `title : fancy title` A title for the event. Same as above.
- `whatever comes after the "---"` A description of the event: this will also be shown as is, in the corresponding event page.

That should be it regarding events. You can add as many as you want: all those identified as old will be shown in the `/events/` page, while all those identified as not old will be shown in the main page. In both case, they will be nicely organised in a grid. I've tried to automate the process as much as possible: for example, the images will be automatically centered and resized to preserve a fixed aspect-ratio, and the small dark grey box will be resized to accomodate for longer titles. Certain things, however, can't be automated: if you put an image where the relevant bit is on the top right corner, you'll se nothing relevant; if the title is ridiculously long, it will spill over the maximum size of the element, and it will look bad. Check every time you modify things to make sure the website still looks nice.


### /data/
Here is where you need to go to modify pretty much everything else. You'll see a bunch of `.yml` files in here, all referring to specific sections in the website: for example, `contact.yml` contains info on how to fill the "get in touch" section at the bottom of the main page. There're too many to describe them all in detail, but it should be straightforward to reverse-engineer what is modified by changing what: the names should help, for once, and make sure you're always checking http://localhost:1313 when you re-write something, so that you can look for changes. As an example, let's take `about.yml`, which dictates what goes in the "About Us" section in the main page
```yaml
enable : true
heading : About
headingSpan : Us
aboutItem : 
- icon : tf-ion-ios-wineglass
  title : Events
  content : "Every term we commit to organise a number of events to keep our members engaged. These range from social aperitivos to talks with 	invited speakers: keep an eye on our term card to know what's up next"	
  url: events	

- icon : tf-ion-ios-book
  title : Teaching
  content : "To promote Italian culture, we offer a variety of language courses in Italian at very attractive prices for our members. The classes 	are open to all levels: just pick the one you're most confortable with"	
  url: classes	

- icon : tf-ion-ios-people
  title : Community
  content : "Our society is composed by an ever-growing community of Oxford students, alumni, and non-, all united by their love for the Italian 	culture. The Italian Society is the best way to keep in touch with them"
  url: committee
```
again, a bunch of variables with some values assigned to them. `enable` for example, decides whether the section is shown at all, `heading` and `headingSpan` define the title and the colored part of it appearing at the top of the section, respectively: change these and you'll see the site change accordingly in http://localhost:1313. Something worth pointing out are lists: these are identified by putting a `-` before an element. For example, `aboutItem` is a list containing three elements. Each element has the fields `icon`, `title`, `content`, and `url`. This reflects the fact that there are three dark boxes in the About Us section, titled respectively Events, Teaching and Community, as reflected by the `title` field. Guess what: if you were to add another element, with the correct fields, a new box would appear automatically in the website. This to show that the website has some flexibility, provided you respect the original format. Similar ideas apply to, for example, the language classes offered, the membership plans offered, the members of the committee: it's up to you to fish the correct file containing that info and modify it accordingly, but with what you've learnt so far it shouldn't be too hard.

### /config.toml:
This contains the last few parameters, slightly higher-level than the rest (like items on the navigation menu, and the sort). Also data regarding small sections (the Join Us section, for example, and the counters for members and other things) is held here. I don't expect you'll have to play around with this file too much, though, apart from maybe updating the counters from time to time.




## Deploying the website

You've made your changes, you've checked them locally on http://localhost:1313, you're satisfied with the way they look, and now you want to show the whole world the updated info on the website: it's time to deploy your changes online. This is done in two steps.

1- Committing: First of all, you want your changes to be stored on the online repository. This step is not necessary, but since you've made some changes, it makes sense to store them somewhere safer than your local machine (and somewhere that's easily accessible to everyone in the future, too). This is what gitHub is for: you can create a "screenshot" of how your files look like at this point in time by writing the following commands on terminal:
```shell
git add .
```
note the `.` at the end, which states that you should add all files. This is to tell git to consider all the modified files up until now.
```shell
git commit -m "<MESSAGE>"
```
this is to create the actual screenshot of your files, ie, to save a version of your files. You can put anything you want in `<MESSAGE>`: if in the future you realise you made some horrible mistakes and you want to revert back to some previous version, you can identify the correct one using this message. For example, something like `"Updated list of events for TT19"` seems like a reasonable message to include.
```shell
git push
```
This saves (pushes) the version of the repository online, so that it's stored somewhere safe.

These commands are not necessary, in the sense that you can still deploy an updated website even without going through this), but it's a very clever thing to do: whoever'll take care of the website in the future will be grateful for this.


2- Deploying: This is the big deal. This gets things live. It's not like there's no turning back (remember what we said about reverting to a previous commit?), but still: your mistakes will be shown. So double-check everything, and once you're sure, write on the terminal
```shell
./deploy.sh
```
Simple enough. This is a shell command that runs a series of actions for you automatically. If you're interested in the details, what happens is:
- It runs Hugo to create the actual website. The necessary data is then saved in the `/public/` folder
- The content in `/public/` is added, committed and pushed to _another_ repository, whose content is looked at by the host to show the actual website online, and which should never be touched in any other way.
Changes might take a while to actually become live on https://oxforditaliansociety.org/, so don't panic if you don't see any change straight away, and give it a few hours.


Congratulations! You've made the first few changes to the website. Kudos to that!







## How to make __deep__ changes to the website

You decided you don't like the layout of the website anymore. The colors are ugly and that section is organised terribly. A whole new page needs to be addded to the website, because we decided to offer new things in the meantime. These are examples of what I define _deep_ changes: as opposed to _shallow_ changes, in order to implement these we need to start tampering with the actual template of the website, and for this we require some knowledge of [css](https://www.w3schools.com/css/ "css tutorial") and [html](https://www.w3schools.com/htmL/html_intro.asp "html tutorial") (at the least). This section is __not__ about _learning_ css or html: if you want to change the layout of the website, I hope you know how to do it already (although it's not that hard: I myself knew nothing when I started tampering around). This section is about _where_ to change things in the repository to achieve what you want to achieve: after that, you're on your own.

In __Section 2__ we dealt with tiny every day changes to the website. The first thing I'd recommend, is to check that section again and play around with the files there for a bit longer: there're actually quite a few things you can do even without turning to html, so chances are you don't need to go through the bother, and you can make it do just by changing an option value in one of the `.yml` files instead. 

If you still insist that no, you really need to mess up the website, then fine, let's do it! To deal with the layout, I started with the [meghna theme](https://themes.gohugo.io/meghna-hugo/ "meghna"). A theme dictates how the pages look like (from low-level stuff like colors used, up to how content is organised and what happens if I click on certain parts). All theme-related file are inside the `/themes/meghna/` folder: you'll have to modify stuff in there to apply the changes. In particular:


### /themes/meghna/assets/css/style.css

This file contains info regarding the style of all the _basic blocks_ of your website. Stuff like titles, boxes, buttons, even mouse-hovering behaviour, and so on, are all styled here: a class identifying each building block is defined, and there you can define font type and size, colors, size, and the sort. I've tried to organise classes according to the sections of the website in which they're used, but I've been a bit messy here and there, so don't rely on this 100%. 


### /themes/meghna/layouts/

As the name suggests, this folder contains info on the actual layout of the pages. Notice the folder structure mimics (to some extent) the one in `/content/`, so that a layout is specified for each of the pages of the website by putting a `.html` file inside the correspondingly named folder. Info on the layouts of each of the sections appearing in the main page is instead located in the `/partials/` folder.
If you wish to create a new page in the website, let's say `photo-gallery`, you need to proceed as follows:
- create a folder named `photo-gallery` in `/content/`
- put an empty `_index.md` file in `/content/photo-gallery/`: this way, Hugo knows it needs to create another page
- create a folder equally named `photo-gallery` in `/themes/meghna/layouts/`
- put a `.html` file in there (the name is not too important: call it `list`)
- fill `list.html` with info on the layout of the specific page: this way, Hugo knows what to show in that page






