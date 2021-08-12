My R Notes for Website Management
================
Congli (Claire) Zhang
Starting at: 7/25/2021

This is my notes of R code for setting up and maintaining a website.

## 1. Distill website

This is probably the simplest way to build a website with version
control. All you need is your RStudio and internet connecting you to
GitHub. I’m sure there’ll be similar instructions online but I had a
hard time to find it, so I explored on my own. It took me some trial and
error to get this approach sorted out.

I was taught two ways of version control: using third-party software
like GitKraken and the really simple Git commands/functions coming with
RStudio. The latter one is my all-time favorite as I’d like to limit the
number of software installed in my computer and more importantly, saving
time opening more software to do a simple task. Side note here, I use a
rather cheap Windows computer so consider saving your time and stopping
reading if you use Mac or other system.

Make sure you have RStudio and a browser open and let’s get started.
Note that all things below are the default version. Modifying anything
is not our topic today.

Step 1. Log onto GitHub and create a repo (don’t know how? [see
here](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-new-repository)).
DO NOT CHECK any of the three boxes under “Initialize this repository
with”. These three things, readme file, .gitignore, and license are
important but we have to leave them out just for now.

Step 2. Copy the url of your repo.

Step 3. Open your RStudio desktop and create a new project following
“new project” - “version control” - “Git”. You’ll be asked to put in the
url you just copied and give a name to the folder that hold your entire
project.

Step 4. Remember the project name and its path in your computer.

Step 5. In your RStudio, create a new project - distill website/blog.
You’ll be ask the name and path for the project, keep them the same with
those in step 3/4. Also, don’t forget to check the “Configur for GitHub
Pages” box. Wait for a few seconds to let RStudio create a automatic,
skeleton distill website/blog project for you.

Step 6. In your RStudio console, execute the code
“rmarkdown::render_site()” or click the “Build” button in your upper
right pane. They do the same job rendering all the RMD files in your
project root directory to HTML files (webpages you see on you
soon-to-come website). A default website should also show up in Viewer
in your bottom right pane, ready to be deployed.

Step 7. Click the “Git” button in your upper right pane and push your
work to GitHub (don’t know how? [see
here](https://happygitwithr.com/rstudio-git-github.html)).

Step 8. Publish your website on GitHub Page by simply going back to your
GitHub repository and finishing the configuration page in “Settings” -
“Pages”. When it’s done, you’ll be given a url for your website. Check
it out and start from there.
