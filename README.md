# The Fellowship - CSE134B HW5

## Preface
Let us preface this by remarking on the inexperience of our group members in web development coming into this course. For most if not all of us, this is our first brush with client-side web technologies.

The spec of this assignment notes that for our documentation: "The form of this effort likely should be more than just a README.md file”. Surely, however, a well-formatted markdown file with a table of contents suffices.

## <a name="toc">Table of Contents</a>
* [Application Functionality](#app-func)
    * [Market Prices](#app-market)
    * [User Stack Tracking](#app-stack)
    * [Adding and Deleting Stack Items](#app-add)
    * [Settings](#app-settings)
    * [User Authentication](#app-user-auth)
* [Pages](#pages)
* [Development](#development)
    * [Development Bugs](#development-bugs)
* [Deployment](#deployment)
* [Technologies](#technologies)
* [Issues/Concerns](#issues)
* [Validation](#validation)
* [Browser Compatibility](#compatibility)

## <a name="app-func">Application Functionality</a> [&#8593;](#toc)
#### <a name="app-market">Market Prices</a> [&#8593;](#toc)
* Tracks prices (ask and bid) of each metal and charts their change over time.

#### <a name="app-stack">User Stack Tracking</a> [&#8593;](#toc)
* Tracks bullion value in total and for all metals individually.

#### <a name="app-add">Adding and Deleting Stack Items</a> [&#8593;](#toc)
* When adding a new stack item, click on placeholder coin image to upload a custom picture of that coin.
* Adding a coin can be done via the plus icon the specific metal page
* Deleting a coin can be done via the item detail page of the coin

#### <a name="app-settings">Settings</a> [&#8593;](#toc)
* Change account settings: name, username, email.
* Change account password.
* Change theme between dark and light.

#### <a name="app-user-auth">User Authentication</a> [&#8593;](#toc)
* Facebook login available from [here](http://thefellowship.parseapp.com)
* Canonical authentication via username and password available.

## <a name="pages">Pages</a> [&#8593;](#toc)
* ``index.html``:
* ``signup.html``:
* ``login.html``:
* ``home.html``:
* ``mymetal.html``:
* ``addnewitem.html``:
* ``itemdetail.html``:

## <a name="development">Development</a> [&#8593;](#toc)
* Our development philosophy is to tackle the primary functionalities first rather than completing features which may be bundled with a functionality
* The primary functionalities we identified were: 
* Create, Retrieve, Update, Delete (CRUD) operations for a user
* Coin or stack maintenance for a user’s coin collection
* User accounts with username/login authentication
* Graphical display (chart) of user’s and market’s metal history
* Initially all functions were kept inside a common file, main.js
* Effort was made to break out individual functions into their separate .js files as to avoid linking more than necessary javascript
* The libraries that we decided to use are Chart.js, jquery, parse, quandl,velocity
* The primary javascript files that were developed were: graph.js coin_list.js, item_detail.js, main.js, metal_info.js, parse-stackitem.js, 
* graph.js generates the chart that is displayed in the home.html, and mymetal.html
* graph.js gathers data of the user’s current coin stack and displays these values
* coin_list.js handles the gathering of all coins that are currently inside a user’s collection. 
* item_detail.js contains functions which call to our parse backend and gather coin information
* item_detail.js also handles uploading pictures to parse and displaying pictures of the coins on the user’s coin list
* metal_info.js contains functions that populate data and generates headers for each of the metals 
* parse-stackitem.js contains functions that will perform the transactions between our web app and the parse backend. 
* main.js acts as the controller of our application. It contains setup functions which will set-up our webpage such as navigation bars and transaction functions such as when a user saves a coin to the stack. 

#### <a name="development-bugs">Development Bugs</a> [&#8593;](#toc)
* 



## <a name="deployment">Deployment</a> [&#8593;](#toc)
* ``dist`` directory for production version of the app.
* Gulp as task runner to minify and concatenate JS and CSS files. This is detailed inside of the ``gulpfile.js`` file in the root directory. We first minify all of our css files, but do not concatenate them because theme switching is dependent on there being separate CSS files. We then minify all of our Javascript files and concatenate them into a single file, and place this file inside of the ``dist`` directory. Using ``gulp-useref``, Javascript import elements are collapsed to reference the concatenated ``all.js``. Finally all the assets and fonts are copied over to the ``dist`` directory.
* These static files constitute the ``public`` directory to host on Parse. Then we simply need to run ``parse deploy`` in the ``dist`` directory to host the static files.
* Unfortunately, some of the Javascript code to login/signup in the production version of the app is not properly being called.

## <a name="technologies">Technologies</a> [&#8593;](#toc)
* ADD RATIONALES AND FUNCTION
* Gulp: Gulp and Grunt were the two task runners that we surveyed for our application. In the end, we decided to use Gulp instead of Grunt as our team had more experience with Gulp from prior projects and Gulp was much easier to configure.
* Sass
* Parse: used as backend and user account management.
* ChartJS
* jQuery
* Quandl PERTH dataset used to provide metal bid/ask prices.

## <a name="issues">Issues/Concerns</a> [&#8593;](#toc)
* Google+ OAuth client-side Parse is not easy. Firebase came with simple API for doing client-side Google+ OAuth but unfortunately not Parse.
* Lacks capability to change/edit Coin Stack items.
* Lacks a strong database of coin types to properly display different coin types and its related attributes (% au, weight, etc)
* Lacked a reliable source to get current metal bid/ask prices.
  * Best data set had a monthly update in Quandl.
* Application will not fail gracefully.
* Parse lacks client-side implementation for social login functionalities other than facebook.
* Logging out from facebook before logging out from our app invalidates the session and requires manual logout
* Application was not developed to scale.
* Data precision is lacking. The shown +/- percentages are based on a monthly comparison.
* Graph lacks much functionality. Cannot analyze on a daily/hourly basis or change the range of months.
* Must only delete a coin that the user has in inventory, and not a “null” coin 

## <a name="validation">Validation</a> [&#8593;](#toc)
* The HTML all validates. Previously we had error because of empty table rows ``<tr>`` elements. This was fixed by removing the row entirely and deferring the task to the JS.
* For CSS, the lingering code from DreamTeam’s submission had validation errors mostly with the property ``fill``, but after extensive testing with different browsers: it seems to work properly.

## <a name="compatibility">Browser Compatibility</a> [&#8593;](#toc)
* Had an issue using the ``<dialog>`` HTML element when adding a new type on ``addnewitem.html``: non-Chromium browsers would always show it despite the fact that there was no open attribute. We fixed this by changing it to just a toggling div.
