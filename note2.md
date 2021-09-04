My R Notes for Website Management
================
Congli (Claire) Zhang
Starting at: 7/25/2021

This is my notes of R code for setting up and maintaining a website for
ONLY BEGINNERS. Experts may say differently about everything. I’m just a
learner wanting to share some strategies that work for me.

## 1. Distill website

This is probably the simplest way to build a website with version
control. All you need is your RStudio and internet connecting you to
GitHub. I’m sure there’ll be similar instructions online but I had a
hard time to find it, so I explored on my own. It took me some trial and
error to get this approach sorted out.

I was taught two ways of version control: using third-party software
like GitKraken and Git commands/functions coming with RStudio (of course
you can use shell if you’re using a mac). The latter one is my all-time
favorite as I’d like to limit the number of software installed in my
computer and more importantly, saving time opening more software to do a
simple task. Side note here, I use a rather cheap Windows computer so
consider saving your time and stopping reading if you use Mac or other
system.

Make sure you have RStudio and a browser open and let’s get started.
Note that all things below are the default version. Modifying anything
is not our topic today.

Step 1. Log onto GitHub and create a repo (don’t know how? [see
here](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-new-repository)).
DO NOT CHECK any of the three boxes under “Initialize this repository
with”. These three things, README file, .gitignore, and license are
important but we have to leave them out just for now.

Issues you’ll have if checking README: it’ll create a branch named
“main” for your future commit activities as well as a README.md file,
but the distill package (who builds static website using html files)
doesn’t recognize this file - it’ll give you an error saying no
readme.html and stop building your website in step 6.

Similar issues with checking .gitignore and license will occur

Copy the url of your repo.

Step 2. Open your RStudio and create a new project following “new
project” - “version control” - “Git”. You’ll be asked to put in the url
you just copied and give a name to the folder that hold your entire
project.

Note that you have other options to version-control your project - using
a 3rd software such as GitKraken, or using the shell built in RStudio if
you are using a Mac, or other things I don’t know yet.

Remember the project name and its path in your computer.

Step 3. In your RStudio, create a new project for distill website/blog.
You’ll be ask the name and path for the project, keep them the same with
those in step 2. Also, don’t forget to check the “Configure for GitHub
Pages” box. Wait for a few seconds to let RStudio create a automatic,
skeleton distill website/blog project for you.

Step 4. In your RStudio console, execute the code
“rmarkdown::render_site()” or click the “Build” button in your upper
right pane. They do the same job rendering the RMD files in your project
**root directory** to HTML files and store them in a folder called
“docs”. Also a nice function of distill is giving you the instant view
of your website, examine it in the new window to see if every page is
what you want - if somewhere has not been updated, my best guess is that
the Rmd file is not in the root directory so you have to render it
manually.

Note that the htmls in the docs folder (you can change this if you want
just don’t be impatient with the consequences) are the web pages you see
on you soon-to-come website so whenever you want to make any chance to
your website, you want to make sure to update these htmls.

Step 5. Click the “Git” button in your upper right pane and push your
work to GitHub (don’t know how? [see
here](https://happygitwithr.com/rstudio-git-github.html)).

Note that when you pull, RStudio will tell you there’s no branch yet and
when you push, a branch called “master” will be automatically created
for you. It’s important you remember this branch for your future
commits.

Step 6. Publish your website on GitHub Page. Going back to your GitHub
repository and finishing the configuration page in “Settings” - “Pages”.
When it’s done, you’ll be given a url for your website. Check it out and
start from there.

Note that during the configuration, make sure to publish the pages under
the master branch then docs folder.
