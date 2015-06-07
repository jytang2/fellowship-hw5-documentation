# The Fellowship - CSE134B HW5

## Preface
Let us preface this by remarking on the inexperience, coming into this course, of our group members in web development. For most if not all of us, this is our first brush with client-side web technologies. Therefore, it is with great effort and many hours of wallowing around in the depths and extents of thick tomes of documentation that we fit together this web app.

## Preliminary Notes
* The production version of the app is located in the ``dist/public`` directory. Development version is the root directory excluding the ``dist`` directory.
* Our app should be tested from this [link](http://thefellowship.parseapp.com) in order for facebook login to work (clear cache if still getting behaviour from HW4). This is just the hosted static files of the production version of the app.
* The spec of this assignment notes that for our documentation: "[t]he form of this effort likely should be more than just a README.md file”. Surely, however, a well-formatted markdown file with a table of contents suffices.

## <a name="toc">Table of Contents</a>
* [Application Functionality](#app-func)
* [Pages](#pages)
* [Development](#development)
    * [Obstacles and Lessons Learned](#dev-obstacles)
* [Deployment](#deployment)
* [Technologies](#technologies)
* [Issues/Concerns](#issues)
* [Validation](#validation)
* [Browser Compatibility](#compatibility)

## <a name="app-func">Application Functionality</a> [&#8593;](#toc)
#### Market Prices
* Tracks prices (ask and bid) of each metal.
* Graphs the change over time of 1 troy oz of each metal.
* Graph can switch between monthly and daily view by clicking on the toggle button labelled "Month" or "Day".
* Ask and bid prices are pulled from Quandl PERTH dataset, which updates only monthly. Graph prices are pulled from Quandl WSJ dataset.
* The market prices in the graph and the ask/bid prices on the left of the graph are mismatched because Quandl WSJ data does not give ask/bid price and instead daily 1 troy oz values, while Quandl PERTH data gives ask/bid price but not daily prices.

#### User Stack Tracking
* Tracks bullion value in total and for all metals individually.
* The tracking is based upon the *current time* at which the stack items were added or deleted from the system, not according to the items’ *purchase dates*. This was done to avoid the confusing interaction of adding in the item with a **purchase date** of a particular month/day and deleting the same item in a different month/day. In this situation, it is ambiguous whether such a deletion should affect the stack total of the purchase date or of the current date. Therefore, we settled on the user’s total values changing based on the time at which they added/deleted items.

#### Adding and Deleting Stack Items
* When adding a new stack item, click on placeholder coin image to upload a custom picture of that coin.
* Adding a coin can be done via the plus icon the specific metal page
* Deleting a coin can be done via the item detail page of the coin
* Add a new type of coin by selecting the appropriate item in the dropdown for Type. Input fields should display to be filled out accordingly.

#### Settings
* Accessible via the cog icon in the top nav bar.
* Change account settings: name, username, email.
* Change account password.
* Change theme between dark and light.

#### User Authentication
* Facebook login available through [here](http://thefellowship.parseapp.com)
* Canonical authentication via username and password available.
* Usernames must be unique.

## <a name="pages">Pages</a> [&#8593;](#toc)
* ``index.html``: main page from which to login with an existing account or sign up.
* ``signup.html``: page on which the user either creates an account or uses Facebook to authenticate.
* ``login.html``: page on which the user either logs in with an existing account or authenticates via Facebook.
* ``home.html``: homescreen of the application, with information on the user’s total stack value as well as market prices and changes. Both types of information are plotted on the graph.
* ``mymetal.html``: page that displays metal-specific information: user’s stack value, market prices, graph, and list of that metal in the user’s stack
* ``addnewitem.html``: page to create a new stack item, for which a new type can be be optionally made and associated with the item
* ``itemdetail.html``: page to view the selected stack item and provides an option to delete said item.
* ``settings.html``: can change user information, password, and app theme here

## <a name="development">Development</a> [&#8593;](#toc)
* Our development philosophy is to tackle the primary functionalities first rather than completing features which may be bundled with other functionality
* The primary functionalities we identified were: 
  * Create, Retrieve, Update, Delete (CRUD) operations for a user
  * Coin or stack maintenance for a user’s coin collection
  * User accounts with username/login authentication
  * Graphical display (chart) of user’s and market’s metal history
* Initially all functions were kept inside a common file, main.js
* Effort was made to break out individual functions into their separate .js files as to avoid linking more than necessary javascript
* Files are bundled based on their technology. The HTML files are located within the root directory of our project, the javascript files are placed in the ./js directory and the stylesheets are placed in the ./sass and ./style directories
* The libraries included in the app are jQuery, Parse, Chart.js, and Velocity.
* The primary javascript files that were developed were: graph.js coin_list.js, item_detail.js, main.js, metal_info.js, parse-stackitem.js, 
* graph.js generates the chart that is displayed in the home.html, and mymetal.html
* graph.js gathers data of the user’s current coin stack and displays these values
* coin_list.js handles the gathering of all coins that are currently inside a user’s collection. 
* item_detail.js contains functions which call to our parse backend and gather coin information
* item_detail.js also handles uploading pictures to parse and displaying pictures of the coins on the user’s coin list
* metal_info.js contains functions that populate data and generates headers for each of the metals 
* parse-stackitem.js contains functions that will perform the transactions between our web app and the parse backend. 
* main.js acts as the controller of our application. It contains setup functions which will set-up our webpage such as navigation bars and transaction functions such as when a user saves a coin to the stack. 

#### <a name="dev-obstacles">Obstacles and Lessons Learned</a> [&#8593;](#toc)
* Main debugging technique was to use the javascript console on the browser
* Parse object queries sometimes required the ``include()`` function to encompass a specific property. ``object.query.include(“type”)`` was needed to pull the specific coin’s type from parse backend
* Functions which makes transactions between parse backend and our web app such as ``deleteStackItem()`` are designed to be asynchronous, there were times when our web app errored out even when the transaction was successful
* Must only delete a coin that the user has in inventory, and not a “null” coin.
* Production version of the app did not work at one point. This was because of unforeseen JS interdependencies, the order of which mattered (e.g. parse.js had to be included before using the Parse object).
* Dealing with callback functions were difficult. Time constraints prevented us from developing a proper style to handle callbacks as situations arose with multi-chained callbacks.
* Timestamps and dates are horrible! Keeping consistency between months, days and the proper indexing for arrays is incredibly difficult without a consistent standard. Proper design and structure would have made this easier.
* Design, Design, Design!

## <a name="deployment">Deployment</a> [&#8593;](#toc)
* ``dist`` directory for production version of the app.
* Gulp as task runner to minify and concatenate JS and CSS files. This is detailed inside of the ``gulpfile.js`` file in the root directory. Running the main task ``gulp all``: we first minify all of our css files, but do not concatenate them because theme switching is dependent on there being separate CSS files. We then minify all of our Javascript files and concatenate them into a single file, and place this file inside of the ``dist/public`` directory. Using ``gulp-useref``, Javascript import elements are collapsed to reference the concatenated ``all.js``. Finally all the assets and fonts are copied over to the ``dist/public`` directory.
* These static files constitute the ``public`` directory to host on Parse. Then we simply need to run ``parse deploy`` in the ``dist`` directory to host the static files.
* The ``gulp validate`` task can be used to validate all of the html. The output validation files are in html format and enumerate out all the warnings, errors, and notes that the W3C validator issues.

## <a name="technologies">Technologies</a> [&#8593;](#toc)
* Gulp: Gulp and Grunt were the two task runners that we surveyed for our application. In the end, we decided to use Gulp instead of Grunt as our team had more experience with Gulp from prior projects and Gulp was much easier to configure.
* Sass: Syntactically awesome stylesheets, simple and easy css extensions along with compatibility with the css libraries we were already familiarizing ourselves with. Kept from previous group in the case that we wanted to modify styles via Sass.
* Parse: Stores the user login information and the coins in a user’s collection. Allows for Facebook authentication but does not support easy connection with Google+ OAuth. Upon researching Firebase we discovered that it had a simple API for doing Google+ OAuth. Unfortunately, we already implemented our login and coin storage system using Parse. With the deadline approaching, we decided to keep using Parse because we did not want to risk not finishing the project.
* Chart.js: used to display the graph for market prices and stack totals
* jQuery: used seldomly in favour of vanilla Javascript
* Quandl PERTH and WSJ datasets used to provide metal bid/ask prices.

## <a name="issues">Issues/Concerns</a> [&#8593;](#toc)
* Google+ OAuth client-side Parse is not easy. Firebase came with simple API for doing client-side Google+ OAuth but unfortunately not Parse. Having researched extensively, it still seems that the most straightforward way is to use Parse cloud code, but that steps out of the bounds of the client-side nature of this assignment.
* Lacks capability to change/edit Coin Stack items: although we have an update method available, we did not have the time to create new UI to link it up with.
* Lacked a reliable source to get current metal bid/ask prices with frequent updates.
  * Best available dataset had a monthly update in Quandl.
* Application does not fail gracefully in all cases.
* Parse lacks client-side implementation for social login functionalities other than Facebook.
* Logging out from Facebook before logging out from our app invalidates the session and requires manual logout
* Application was not developed to scale.
* Data precision is lacking. The shown +/- percentages are based on a monthly comparison.
* Graph lacks some desired functionality. Cannot analyze on other bases (hourly, yearly, etc.).
* Graph displays negative values for the prices
* Graph is not intelligent. It is only able to fill holes in provided data from datasets from previous values. If a previous value does not exist, it inputs an undefined.
* Graph performance is slow, although we may toggle back and forth between by-month or by-day displays, the render of the graph took a performance hit for this feature. This can be ameliorated with a loading indication to the user.
* Graph is not shown responsively, i.e. resizing the browser window hides the graph until the month/day toggle is clicked on.
* When on the daily view of the graph, the percentages on the left of the graph go a bit haywire. This is a known bug.

## <a name="validation">Validation</a> [&#8593;](#toc)
* The HTML all validates. Previously we had errors because of empty table rows ``<tr>`` elements. This was fixed by removing the row entirely and deferring the task to the JS.
* For CSS, the lingering code from DreamTeam’s submission had validation errors mostly with the property ``fill``, but after extensive testing with different browsers: it seems to behave properly.

## <a name="compatibility">Browser Compatibility</a> [&#8593;](#toc)
* Had an issue using the ``<dialog>`` HTML element when adding a new type on ``addnewitem.html``: non-Chromium browsers would always show it despite the fact that there was no open attribute. We fixed this by changing it to just a toggling div.
* Differing Javascript implementations across browser vendors meant that using non-standard Date constructors resulted in inconsistent behaviour in some browsers. We addressed this by abiding by what was available cross-platform.

Fin. [&#8593;](#toc)
