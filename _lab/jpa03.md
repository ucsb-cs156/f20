---
desc: "Deploying full stack app with Auth0 and Database"
assigned: 2020-10-22 14:00
due: 2020-10-29 23:59
gauchospace_url: https://gauchospace.ucsb.edu/courses/mod/assign/view.php?id=5169550
github_org: ucsb-cs156-f20
layout: lab
num: jpa03
ready: true
starter: https://github.com/ucsb-cs156-f20/STARTER-jpa03
---

{% include drop_down_style.html %}

<div style="display:none" >
Look here for formatted version: http://ucsb-cs156.github.io/f20/lab/jpa03
</div>

This is an **individual** lab on the topic of deploying
Java web apps that use OAuth and Databases, using Heroku.

You may cooperate with one or more pair partners from your team to help in debugging and understanding the lab, but each person should complete the lab separately for themselves.

# Preliminaries

We assume that you already created a Heroku account for `jpa02`.

We also assume that you are familiar with the material in `jpa02`
concerning the following items; consult jpa02 if you find
while doing the lab that you need a refresher on any of these:

* Running a Spring Boot app on localhost on port 8080
* Connecting to a server on `localhost:8080` and dealing
  with the issues that arise if that server is running on CSIL
* Creating a new app on Heroku
* Connecting an app on Heroku to a GitHub repo, and deploying
  a branch of that repo on Heroku
* Configuring *Secrets* in a Github repo.

Also, if you are working on your own machine, you need the same
software that was needed for `jpa02`, namely:

* Java 11
* Maven
* git
* Heroku CLI

See guides for installing these on your machine at the links shown:

* Windows: <https://ucsb-cs156.github.io/topics/windows/>
* MacOS: <https://ucsb-cs156.github.io/topics/macos/>

# Step 1: Understanding what we are trying to do

## What are we trying to accomplish in this lab?

This lab, similar to `jpa02` has no programming; just like
 `jpa02` the task is simply to deploy an app on localhost
 and on Heroku.  This time, however, instead of a simple
`Hello World` type app, the app you are deploying is a full-stack
web app with:

* A front-end built in React (under the directory `./javascript`) as in `jspa01`
* A back-end built in Spring Boot (the code for this is under the directory `./src`, plus the `pom.xml` at the top level
* Auth0 integration; this allows the app to have a "login/logout" feature based on Google Accounts (e.g. your UCSB Google Account)
* An SQL database, which runs using H2 (an in-memory database) on localhost, and using Postgres when running on Heroku.

The reason we are doing this lab is that before you can
work on a full-stack legacy code webapp, you need to know how to 
deploy such an app, i.e. get it running.

There are quite a few configuration steps that are needed,
and we want to get you used to those before we start
introducing the coding challenges as well.

# Step 2: Create your repo

You should already have a repo under the course organization 
<tt>{{page.github_org}}</tt> called 
<tt>{{page.num}}-<i>githubid</i></tt>
created for you by the staff, where <tt><i>github</i></tt> 
is your github id.

If not, create one for yourself following that naming convention;
it should initially be private, and empty (no `README`, license or
`.gitignore`.)

Clone that repo somewhere and cd into it.

Then add this remote:

<tt>git remote add starter {{page.starter}}</tt>

Then do:

```
git checkout -b main
git pull starter main
git push origin main
```

# Step 3: Configure your app as documented in the README.md

The next step is to read through the [README.md](https://github.com/ucsb-cs156-f20/STARTER-jpa03/blob/main/README.md)
and configure your app as indicated there.

This includes configuration for Auth0, and GitHub Actions
(the `CODECOV_TOKEN`).

The Auth0 configuration step includes copying some files
with `.SAMPLE` to files that are of the same name *without*
the `.SAMPLE` extension. Those files (the ones without the `.SAMPLE` extension) contain *secrets* so
they should *not* be committed to GitHub. 

The instruction to configure this app for OAuth are linked to from
the `README.md` in the starter repo itself, so will not repeat
them here.  Follow those instructions (which may, in turn, refer
you to instruction inside the `doc` folder of the repo.)

These instructions include setting up an account with `Auth0`; you
should be able to do so on a "free plan"; at no time should
you need to enter a credit card for activities needed in this
course.

## About Auth0

Auth0 is a third-party service that helps application
developers add OAuth support to their application.

OAuth is a protocol that allows you to delegate the login/logout
functionality (user authentication) to a third party such as
Google, Facebook, GitHub, Twitter, etc.  If you've ever used
a website that allows you to "Login with Google" or "Login With Facebook", then chances are good that app was built using OAuth.

Indeed, you've already encountered two examples of GitHub OAuth earlier in the course:
* when you used your GitHub account to log into the <https://ucsb-cs-github-linker.herokuapp.com>
* when you used your GitHub account to log into <https://codecov.io>

## Follow the instructions in the README

The instructions in the README include instructions
for deploying to both localhost, and to Heroku.  Follow those
instructions, consulting the material below and the
material in jpa02 as needed, until you have the application
working on both localhost and Heroku.

## Reminder about running your webapp on `localhost`

To get a spring boot app running on `localhost`, generally
we need to do the following:

* Perform any necessary configuration of secrets, e.g.
  in the `loc
* First, use `mvn compile` to make sure that the code compiles.

  * If you get the error about `JAVA_HOME` not being defined
    correctly, you may need this command:
    ```
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk
    ```
* Next, try `mvn test` to be sure that the test cases pass.
* Then, try `mvn spring-boot:run`.  This should start up a web
  server on port 8080 running on `localhost`

  The `mvn spring-boot:run` command is a shortcut that is provided for us to be able to run the jar file.  It does pretty much the same thing as 
  if we ran the `.jar` file and specified the class containing our `main` on the command line.


When the app is up and running, try logging in with your
UCSB Google Account.  You should then be able to create, edit
and delete todo items.

Note that at this point, each time you start up the app,
the database is recreated *from scratch* in memory.  We use
an in-memory database called H2 when running on localhost;
this saves you a lot of trouble with having to configure
a database on CSIL or on your own machine.

When we migrate to Heroku in the later steps of this lab,
we'll be using a "real" database, one where what is stored
remains even when the app is shut down.

# Step 6: Create a new Heroku App using the Heroku CLI

In this step, we'll deploy our Spring Boot application to the public internet using Heroku.

Deploying this app to Heroku should automatically provision a Postgres database for the app; if that doesn't happen, we'll add instructions
to the lab on how to provision this manually.

Logged into CSIL (or one of the machines in the CSTL, i.e. Phelps 3525), use this command to login to Heroku at the command line:

```
heroku login
```

**NOTES**: 

* If you are ssh'ing in to CSIL, you may need to use `heroku login -i` which allows you to login without having to go to a browser.

* If the `heroku login` command doesn't work, you can instead create the Heroku App at the Heroku Dashboard by
  visiting <https://dashboard.heroku.com/apps>, clicking (at upper right):  "New&nbsp;=>&nbsp;Create New App" and
  then creating an app with the name <tt>heroku create cs56-{{site.qxx}}-<i>githubid</i>-{{page.num}}</tt> as explained in 
  the instructions below.

Then, use this command to create a new web app running on heroku.  Substitute your github id in place of `githubid`.  
Note that you should convert your githubid to all lowercase; heroku web-app names do not permit uppercase letters.

<tt>heroku create cs156-{{site.qxx}}-<i>githubid</i>-{{page.num}}</tt>

Notes:
* A reminder that this is an individual lab, 
  so you should complete it for yourself, i.e. there is only one github id in the name, not a pair of github ids.   
* Please do not literally put the letters <tt><i>githubid</i></tt> 
  in your app name; you are meant to substitute your own github id there.
* If Heroku indicates that the name is too long, you may truncate any
  part of it.   

# Step 7: Login to the Heroku Dashboard

Login to <https://dashboard.heroku.com/apps> and look for the <tt>create cs56-{{site.qxx}}-<i>githubid</i>-{{page.num}}</tt> app that you created.

You should find a place where you can connect your App to Github.  

Click on this, and select your repo to connect the Github Repo to Heroku.

Then, click on "deploy branch".


# What if it doesn't work?

If it doesn't work, consult the troubleshooting steps in `jpa02` and try them before asking a mentor, TA, or instructor for help.


# Step 11: Adding links to repo in the README.md

Edit your README.md.  Just as in `jpa02`, you'll find some TODO items inside indicating what edits you need to make.

All quarter long, we want you to develop the habit of adjusting the 
README.md in your repo to include a link to your repo.

The link to your repo may seem redundant, but it helps your mentors, TAs and instructors; when you submit your work for grading to either Gradescope or Gauchospace, having those links handy really helps us navigate through your assignments quickly to evaluate them and assign grades.

# Step 12: Submitting your work for grading

When you have a running web app, visit <{{page.gauchospace_url}}> and make a submission.

In the text area, enter something like this, substituting your repo name and your Heroku app name:

<div style="font-family:monospace;">
repo name: https://github.com/{{page.org}}/{{page.num}}-chrislee123<br><br>
on heroku: https://cs156-{{site.qxx}}-chrislee123-{{page.num}}.herokuapp.com<br>
</div>

Then, **and this is super important**, please make both of those URLs **clickable urls**.

The instructions for doing so are here: <https://ucsb-cs156.github.io/topics/gauchospace_clickable_urls/>


# Grading Rubric:

* (10 pts) Having a repo with the correct name in the correct organization
* (30 pts) Having a running web app at <tt>https://cs56-{{site.qxx}}-<i>githubid</i>-{{page.num}}.herokuapp.com</tt>
* (30 pts) Running web app has the ability to login through a Google Account, and create, edit and delete TODOs.
* (10 pts) There is a post on Gauchospace that has the correct content
* (10 pts) The links on Gauchospace are clickable links (to make it easier to test your app)
* (10 pts) README has a link to your repo.


