---
layout: lab
num: lab07b
labnum: lab07
ready: true
desc: "Spring Boot Skills"
assigned: 2019-11-08 17:00
due: 2019-11-16 23:59
github_org: "ucsb-cs56-f19"
org: "ucsb-cs56-f19"
gauchospace_url: "https://gauchospace.ucsb.edu/courses/mod/assign/view.php?id=TBD"
prev: lab07a
starter: 
---

<div style="display:none" >
Look here for formatted version: http://ucsb-cs56.github.io/f19/labWIP/lab07n
</div>

This lab builds on your work from {{page.prev}}.

# What if I didn't finish {{page.prev}}

If you were not successful in completing {{page.prev}}, you should go
back and complete any unfinished steps from {{page.prev}} first.  You
will get partial credit for this lab simply for doing the unfinished
steps from {{page.prev}}, even if you missed the deadline for
{{page.prev}}

# Individual lab

This is an **individual** lab on the topic of Java web apps on Heroku.

You may cooperate with one or more pair partners from your team to help in debugging and understanding the lab, but each person should complete the lab separately for themselves.


# Goals

See Goals section of  [lab07a](lab07a).

# Picking up from step 5 of [lab07a](lab07a).

Please return to your same repo:

* <tt>{{page.labnum}}-githubid</tt>.

We will work with this repo, and with the Heroku app you configured for {{lab.prev}}

   
# Step by step instructions


# Step 6: Set up repo for Travis-CI

We'll start this lab by setting up our repo on Travis-CI.

Setting up our repo on Travis-CI will set it up for "continuous integration"&mdash;that's what CI stands for.

Continuous integration means that we try to integrate new code into the code base early and often, and that we run all of the tests of our code base each time we do that.

With Travis-CI setup, each time you push code to GitHub, or do a pull request, a server in the cloud (at the Travis-CI.org website) will pull your repo, and run all of your JUnit tests.  You'll get an indication on the GitHub site for your repo whether the tests passed or not.

To set up your repo for Travis-CI, the first step is to copy the two line file `.travis.yml` from the starter code repo <{{page.starter}}> into the root of your repo.  (For this change, we'll make an exception and just do it directly on the master branch.)

So, please copy that file in to your repo, and do:

```
git add .travis.yml
git commit -m "xx - add .travis.yml for Travis-CI"
git push origin master
```

The next step is to visit the following website, and login with your GitHub account:

<https://travis-ci.org>

Once there, at the upper left hand corner of the dashboard, you should see a small plus sign next to the text "My Repositories".  You want to click this `+` sign as shown in this image:

![Travis Dashboard + sign](travis-dashboard-plus-sign-30.png)

That takes you to a page where you can add the <tt>{{page.org}}</tt> GitHub organization to your authorized organizations for Travis-CI.

You might have to scroll down the left column where the text says:

> MISSING AN ORGANIZATION? <br>
> <u>Review and add</u> your authorized organizations

On the page, the text "review and add" is a link; if you click it, you should be able to enable Travis-CI access for <tt>{{page.org}}</tt>.

Once you do that, you should be able to see the organization 
<tt>{{page.org}}</tt> in the left hand column.   If you click on it, you should then be able to see your repo, and enable it for Travis-CI by clicking the small button next to the repo name. 

It can be a bit confusing, but if you are patient with yourself and the site, you'll figure it out.  If after trying for a while, you are still having difficulty, ask a mentor, TA or the instructor for assistance.

Once you've got the repo enabled for Travis-CI, there will a web page specifically for your repo, with the url:

* <https://travis-ci.org/{{page.org}}/{{page.labnum}}-githubid>

where <tt>{{page.org}}/{{page.labnum}}-githubid</tt> is the name of your repo.   On that page, at the upper right, you should be able to find a button with the text "More Options". Click on this reveals the following menu:

![More Options Menu](travis-more-options-menu-30.png)

Clicking on the "Trigger Build" option will bring up this pop-up:

![Trigger Build](travis-trigger-build-30.png)

Here, you can trigger a build for any branch, with the master branch being the default.  Go ahead and trigger a build for your master branch.

You should be able to see on the Travis-Ci page for your repo that the branch build successfully, all the test cases pass, and that you end up "green on master".   (The color green indicates success, i.e. that all the tests passed.)

If not, try to determine what's wrong first by checking these things:
* When you type `mvn test` locally, do all the tests pass?
* Do you have a `.travis.yml` committed on the master branch?

If you aren't able to figure out what is wrong, seek out help from a mentor, TA, or instructor.

## Step 7: First feature branch: `xxSmallUIFixes`

### Step 7a: pull from master

Now, to be sure you have the latest code (in case you changed anything on another computer, or on github), do this in your terminal before proceeding:

```
git pull origin master
```

### Step 7b: create a feature branch

We will now create a feature branch. The first two letters should be your initials, e.g. `pc`, `ab`, etc.  

The rest should be `SmallUIFixes`.  So the branch name will be something like `pcSmallUIFixes` or `abSmallUIFixes`.

Type this (but not literally `xx` unless your first and last name both start with `x`)

```
git checkout -b xxSmallUIFixes
```

### Step 7c: Write a failing test

Now on this branch, the change we want to make is to change the
text for the "brand", which is the item at the upper left hand corner
of the web page (the thing you click to get home).  

* The current text is "My-Web-App".  
* We want to change that to "lab07"

So the first thing we do is write a failing test.  The test should:
* load the home page
* find that HTML element
* assert that the contents are `"lab07"`

When we run that test, it should say that it expected `lab07` but found `My-Web-App`.

Here's how an experienced developer would write this test:
1.  Locate a similar test that already exists in the code.

    In this case, the test `getHomePage_hasCorrectTitle()` in the file
    `src/test/java/hello/HomePageTest.java` is a good candidate.
    
2.  Find the XPath expression for the HTML element 
    containing `My-Web-App`.  We can do this by right 
    clicking in the browser 
    on the text `My-Web-App` and choose "Inspect".

    In Chrome, at least, this brings up a pane where you can 
    right click on the element, and there is a menu for "Copy".
    If you go to that menu, one of the options is "Copy XPath".

    The XPath for this element happens to be:
    `/html/body/div/nav/a`.

3.  We can now copy/paste the `getHomePage_hasCorrectTitle()` test,
    rename the copied test to `getHomePage_hasCorrectBrand()`.

    We can then change the code as follows:

    From:

    ```
    .andExpect(xpath("//title").exists())
    .andExpect(xpath("//title").string("CS56 Spring Boot Practice App"));
    ```

    To:

    ```
    .andExpect(xpath("/html/body/div/nav/a").exists())
    .andExpect(xpath("/html/body/div/nav/a").string("lab07"));
    ```

This test should fail if we run it, with something like:

```
java.lang.AssertionError: XPath /html/body/div/nav/a expected:<lab07> but was:<My-Web-App>
```

### Step 7d: Make the test pass

Then, we can make the test pass by replacing the text in that element.

That text lives inside the file `src/main/resources/templates/bootstrap/bootstrap_nav_header.html`, which is where all of the HTML code for the navigation header can be found.

We can change:

```
 <a class="navbar-brand" href="/">My-Web-App</a>
```

to 

```
 <a class="navbar-brand" href="/">lab07</a>
```

After this, the test should pass.

We can then commit both the change and the test together in a single commit.   *This is professional standard practice*.    

Commit this change.

### Step 7e: Do it again

Now, we want to change the text on the first link from "Page 1" to "Earthquakes".

Write a test that fails.  Then make that test pass.

Use the same technique.  

Once you have this test passing, make another commit on the `xxSmallUIFixes` branch.

### Step 7f: Make a pull request

Now make a pull request for this branch.

When you make the pull request, if Travis-Ci is working properly,
you should see a small yellow circle that eventually turns into a green check 
on the list of commits.    

When the pull request shows that the tests have passed, 
merge the pull request into master.

# Step 8: Next feature branch: `xxCreateForm`

We'll now create a second branch for creating a form.

Before we create the branch, we need to be sure we are working 
with a fresh copy of master.  So do:

```
git checkout master
git pull origin master
```

# Step 8a: Next feature branch: `xxCreateForm`

Then create a new branch called `xxCreateForm` (as always, `xx` should be your actual initials, not literally `xx`, unless your name
is, for example, "Xavier Xie".)

# Step 8a: Create the form

On this branch, we will create a simple HTML form using Thymeleaf.

The form will have two fields in it.  It will be a form that allows the user
to specify parameters for searching for earthquakes in the last 30 days.

The two parameters will be:

* a distance in kilometers from the UCSB campus (defined for our purposes as
  Latitude 34.4140° N, Longitude 119.8489° W)
* a minimum magnitude.  

Under `src/main/resources/templates` make a folder called `earthquakes` so that you have:
`src/main/resources/templates/earthquakes`.

Copy the file `page1.html` to a file under `src/main/resources/templates/earthquakes` called `search.html`.

In `search.html`, replace this line of code:

```html
    <title>Title of your page goes here</title>
```

with this:

```html
    <title>Earthquake Search</title>
```

Find the part of the page that reads like this:

```
<h1>Page 1</h1>

<p>This page is a placeholder.</p>
```

Replace it with this code, which is a heading and a Thymeleaf form:

```html
 <h1>Earthquake Search</h1>

 <form action="#" th:action="@{/earthquakes/results}" th:object="${eqSearch}" method="get">
            <table>
                <tr class="form-group">
                    <th><label for="distance" class="col-form-label">Distance (km)</label></th>
                    <td><input type="number" th:field="*{distance}" class="form-control" id="distance"></td>
                </tr>
                <tr class="form-group">
                    <th><label for="minmag" class="col-form-label">Minimum Magnitude</label></th>
                    <td><input type="number" th:field="*{minmag}" class="form-control" id="minmag"></td>
                </tr>
            </table>

            <input type="submit" class="btn btn-primary" value="Search">
        </form>
```

# Step 8b: Add a bean that corresponds to the form

Thymeleaf and Spring Boot work with Java Beans to move form information around.

So we need a Java Bean that corresponds to this form.

Create a Java class in the same directory as your other Java code called EqSearch.java.

It should be a plain old Java class with these private data members:

```java
 private int distance;
 private int minmag;
```

It should also have a no-arg constructor, and getters and setters for each of these fields.

That makes it a "Java Bean".

Note that the Java Bean naming standards are very strict. With names `distance` and `minmag`:

* The getters must be `public int` and named: `getDistance` and `getMinmag`
* The setters must be `public void` and named `setDistance` and `setMinmag`

If you vary from this, you'll have to be sure that the Thymeleaf code is changed accoringly, so be sure that 
you follow the conventions strictly.  Everything has to match, or it just won't work.

For example, don't be tempted to use: `getMinMag` unless you are prepared to make that change everywhere in 
all of the Thymeleaf and Java code, consistently.

Finally, be sure this `EqSearch.java` file is in the same package as the rest of your code.   That package is currently `hello`.


# Step 8c: Add a controller method for the form

In order to be able to see this form in the webapp, we need a controller method for it.

In the file `WebController.java`, add this code:

```java
 @GetMapping("/earthquakes/search")
    public String getEarthquakesSearch(Model model, OAuth2AuthenticationToken oAuth2AuthenticationToken,
            EqSearch eqSearch) {
        return "earthquakes/search";
    }
```

This code says
* we are going to have an endpoint in the application at the web address `/earthquakes/search`
* that endpoint will use the HTTP `GET` method (rather than `POST`, for example).  
  A `GET` request is the normal way of loading
  a web page from a URL or a link.
* That endpoint takes three parameters
   * The first two are needed to support the navigation bar and login/logout; we won't worry about those for now.
   * The third one is the Java Bean that is referred to in the form, `eqsearch`.  
   * The fields of that Bean must correspond to the fields in the form (`distance` and `minmag`).
* The return value corresponds to the HTML template that we defined, without the trailing `.html`, i.e. `earthquakes/search.html` inside `src/main/resources/templates/`.

Test this by running `mvn spring-boot:run` and by hand entering the web address <http://localhost:8080/earthquakes.search> and you should see the form.  Clicking on it won't work yet; making that work is a separate step.  One step at a time.

# Step 8d: Add a menu item that routes to the form.

To make it easier to get to this form, we'll add a link to it to our navigation bar.

In the file `src/main/resources/templates/bootstrap/bootstrap_nav_header.html` you should find this code:

```html
            <li class="nav-item active">
                <a class="nav-link" href="/">Home <span class="sr-only">(current)</span></a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="/page1">Page 1</a>
            </li>
```

This is two `<li>` elements (list items), each of which:
* starts with `<li>` (the `li` open tag)
* ends with `</li>` (the `li` close tag)

In case we haven't mentioned it before: it is important to understand that an HTML element starts with an open tag, ends with a close tag, and everything in between is the elements "content".   

What you'll be doing is adding a new `<li>` element in between those two, so that the code looks like this:

```html
            <li class="nav-item active">
                <a class="nav-link" href="/">Home <span class="sr-only">(current)</span></a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="/earthquakes/search">Earthquake Search</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="/page1">Page 1</a>
            </li>`
```

Run this, and you should see that there is now a link on the navigation bar that takes you to your page.


# Step 8e: Add a controller method for the form results.

Now we need a controller method for displaying the results.

That controller method will look like this:

```java
 @GetMapping("/earthquakes/results")
    public String getEarthquakesResults(Model model, OAuth2AuthenticationToken oAuth2AuthenticationToken,
            EqSearch eqSearch) {
        model.addAttribute("eqSearch", eqSearch);
        // TODO: Actually do the search here and add results to the model
        return "earthquakes/results";
    }
```

The additional step in this controller method is that we have this line of code:

```
        model.addAttribute("eqSearch", eqSearch);
```

This allows us to send information to the view (i.e. to the Thymeleaf HTML file) that we can display on the results page.

For now all we are doing is echoing back the information that the user entered.  But in a later step, we will replace this comment:

```        
// TODO: Actually do the search here and add results to the model

```

That comment will be replace with code that actually goes out to the web to get information on earthquakes.  We'll retreive that information and add it to the model.   That will make it available in the view.

# Step 8f: Add a view for the results page.

The view page `results.html` will be very similar to the page `search.html`.  Create it in the same directory,
i.e. `src/main/resources/templates/earthquakes`.   Start by copying all of the code from `search.html`. 

Then:
* Change the `title` element and the `h1` element to be `Earthquake Search Results`.
* Remove the `form` element entirely.
* Replace it with this `table` element:

```html
         <table>
            <tr>
                <th>Distance (km)</th>
                <td th:text="${eqSearch.distance}"></td>
            </tr>
            <tr>
                <th>Minimum Magnitude</th>
                <td th:text="${eqSearch.minmag}"></td>
            </tr>
        </table>
```

Now try running (you should restart), clicking on your `Earthquake Search` link and entering some numbers.

When you click the `Search` button, you should see the numbers echoed back to you.

If that works, we are ready to add some tests.

# Step 8g: Add tests

In this case, we wrote the code before we wrote the tests.

Ideally, you write the tests first.  But it isn't always feasible, especially when you are learning a new framework.

A thorough job of testing is a whole lab unto itself, so we'll just add a few small tests for now.   

Let me stress it again: the code here in this step is *an inadequate job of testing* the code that we've added in this step.
But it's at least a start.

First, let's add a test that makes sure that there is indeed a page at the address `/earthquakes/search` and that we can
retrieve that page without the server crashing.   To do that, we can use the following code, which we'll put into a file called `/src/test/java/hello/EarthquakeSearchTest.java`

```
package hello;

import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.xpath;


@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class EarthquakeSearchTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void getEarthquakeSearch() throws Exception {
        mvc.perform(MockMvcRequestBuilders.get("/earthquakes/search").accept(MediaType.TEXT_HTML))
                .andExpect(status().isOk())
                .andExpect(xpath("//title").exists())
                .andExpect(xpath("//title").string("Earthquakes Search"));
    }           
}
```

# Step 8h: Pull request

Do a pull request from your `xxCreateForm` branch into `master`.

You should then merge that pull request into master.

# Step 9: Next feature branch: `xxCallAPI`

In this step, we'll make yet another branch where we do something useful
with the information on the results page.  We'll make a call to an API
that provides information in JSON format.

In this step we'll only
echo that JSON information on the page; it won't yet be in a format
that is pleasing to an end user.   But we'll be able to see that we
are making progress.

### Step 9a: Create `xxCallAPI` branch.

To start, you need to checkout master and make a new branch.  As always, your initials, not `xx`.

```
git checkout master
git pull origin master
git checkout -b xxCallAPI
```

We have to checkout master and do `git pull origin master` to pick up the changes that were
made in the previous pull request for `xxCreateForm`.  

Do a `git log` command to be sure that you see all of those commits before proceeding.

### Step 9b: Add an `EarthquakeQueryService` that generate fake data for now.

For this step, we will use the idea that a commit can be more than just a way of organizing your
changes to a project.   Rather walk you through the changes you need to make,
I will refer you to a commit that shows you the changes needed.

That commit is here:
* <https://github.com/ucsb-cs56-f19/STAFF-lab07-dev-WIP/pull/3/commits/4581f91f90cd03ef76a95970b55f2e2fb78d6461>

In this commit, you see that:
* There is a new file called `src/main/java/hello/EarthquakeQueryService.java`.  You should 
  create a file like this one and add it to your code base.  This is a placeholder for the 
  code that will get the Earthquake data in JSON format.  (At a later step, we'll add in the
  code that retrieves the information.)
* There are also some changes to `src/main/java/hello/WebController.java`. These changes 
  call out to the new `EarthquakeService` object and retrieve the information in JSON 
  format.  We store that into an attribute in the model called `json`.
* Finally, we see that we've modified the `src/main/resources/templates/earthquakes/results.html` file
  by adding in a `<pre>` element with the `th:text` attribute.   The `th:text` attribute value
  of `"${json}"` will fill the `<pre>` element with the value of the `json` attribute in the model,
  replacing the `This is placeholder` text. 
  
Make all of these changes to your code, and then run the application.  You should see that
when you type in values in the search form, you now get a results page that shows the "fake json"
returned by the `EarthquakeQueryService` object.

Do a commit of these changes.  Use an appropriate commit message that explains what you did, and why.

Note: `"I made the changes the lab said to make"` is not what we are looking for here.

Something more like:
* `xx - Added placeholder EarthquakeQueryService and wired it up to results form`
* `xx - Added service that will eventually get earthquake data`

Describe the changes in whatever way makes sense to you, and would convey to a reader
what the purpose of the changes is.  Of course you have to understand the purpose of the
changes before you can do that.

### Step 9c: Make the `EarthquakeQueryService` retrieve real data.

The next step is to make the `EarthquakeQueryService` retrieve real data.

The way to do that is illustrated in this commit:

* <https://github.com/ucsb-cs56-f19/STAFF-lab07-dev-WIP/pull/3/commits/19029b2f00fd3a83262865b3904af5bee894f133>

Here we see that we are using the `RestTemplate` object that is built into Spring Boot to 
do an API call, and return the JSON string.    

If you would like more information on what is happening in this code, the following article
provides more details.  You are encouraged to look over that as/when you need to adapt
the code here for accessing other APIs. 

* <https://ucsb-cs56.github.io/topics/spring_boot_resttemplate/>

For now, though, just make the indicated changes.    You should see that with these changes,
when you run the application, it now retrieves JSON for earthquakes at the specified distance
and minimum magnitude, and displays that JSON in the results form.

Commit this code with an appropriate commit message that explains what change you made and why.
Again, it should be a message that reflects an understanding of the code:

* Not Good: `"xx - made step 9c changes"`
* Better: `"xx - added code to retrieve earthquake data from USGS web service"`

### Step 9d: Pull Request

Do a pull request for this branch to master.  In the description of the pull request,
enter a brief explanation of what these two commits, *as a group*, do to improve the code
and/or the working website.

Then accept the pull request.

# Step 10: Next feature branch: `xxJavaObjects`

In this final step, we'll learn how to transform that JSON string into
usable Java objects, and use those Java objects to put useful information
on the page.


### Step 10a: Create object for the top level GeoJSON returned

TODO

### Step 10b: Write method to convert JSON to Object

TODO

### Step 10c: Use object to display results

TODO

### Step 10d: Create objects for the next levels

TODO

### Step 10e: Use those objects to display results

TODO

### Step 10f: Pull Request

TODO

# Step 11: A small fix to `application.properties`

There is one final change to make (if you haven't done it already).

The GitHub login/logout is supposed to show your status in the GitHub organization <tt>{{page.org}}</tt>, either
as an `admin`, a `member` or a `guest`.   However, this doesn't work properly unless you add this line into your
`application.properties` file:

CORRECT: 
```
spring.security.oauth2.client.registration.github.scope: user,read:org
```

This should replace this incorrect line which may be there in the starter code:

INCORRECT:
```
spring.security.oauth2.client.registration.github.scope: "read:user", "read:org"
```


Add this in.  For this small change, you may just do a commit directly on the master branch.

If you'd like to understand more about what this change means, you can read more about OAuth Scopes here: <https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/>.

Before you do, you should accept your previous pull request(s), and then do:

```
git checkout master
git pull origin master
```

Test it, and make sure that when you logout and login, you see `member` by your username.

You can ask a TA, mentor or instructor to try your app as well. They should see `admin` when they login.

# Final Step: Submitting your work for grading

When you have a running web app, visit <{{page.gauchospace_url}}> and make a submission.

In the text area, enter something like this, substituting your repo name and your Heroku app name:

<div style="font-family:monospace;">
repo name: https://github.com/chrislee123/spring-boot-minimal-webapp<br>
on heroku: https://cs56-{{site.qxx}}-{{page.num}}-chrislee123.herokuapp.com<br>
</div>

Then, **and this is super important**, please make both of those URLs **clickable urls**.

The instructions for doing so are here: <https://ucsb-cs56.github.io/topics/gauchospace_clickable_urls/>


# Grading Rubric:

TBA


