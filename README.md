# The Fellowship - CSE134B HW5

## Preface
(Speak to inexperience of our group members in web development?). "The form of this effort likely should be more than just a README.md file.", but this suffices.

## <a name="toc">Table of Contents</a>
* [Application Functionality](#app-func)
* [Pages](#pages)
* [Development](#development)
* [Deployment](#deployment)
* [Technologies](#technologies)
* [Issues/Concerns](#issues)
* [Validation](#validation)
* [Browser Compatibility](#compatibility)

## <a name="app-func">Application Functionality</a> [&#8593;](#toc)
#### Market Prices
- Tracks prices (ask and bid) of each metal and charts their change over time.

#### User Stack Tracking
- Tracks bullion value in total and for all metals individually.

#### Adding and Deleting Stack Items
- When adding a new stack item, click on placeholder coin image to upload a custom picture of that coin.

#### Settings
- Change account settings: name, username, email.
- Change account password.
- Change theme between dark and light.

#### User Authentication
- Facebook login available from [here](http://thefellowship.parseapp.com)
- Canonical authentication via username and password available.

## <a name="pages">Pages</a> [&#8593;](#toc)
- ``index.html``: 
- ``home.html``:
- ``mymetal.html``:
- ``addnewitem.html``:
- ``itemdetail.html``:

## <a name="development">Development</a> [&#8593;](#toc)

## <a name="deployment">Deployment</a> [&#8593;](#toc)
- Gulp as task runner to minify and concatenate JS and CSS files.
- ``dist`` directory for production version of the app.

## <a name="technologies">Technologies</a> [&#8593;](#toc)
- ADD RATIONALES AND FUNCTION
- Gulp
- Sass
- Parse: used as backend and user account management.
- ChartJS
- jQuery
- Quandl PERTH dataset used to provide metal bid/ask prices.

## <a name="issues">Issues/Concerns</a> [&#8593;](#toc)
- Google+ OAuth client-side Parse is not easy. Firebase came with simple API for doing client-side Google+ OAuth but unfortunately not Parse.
- Lacks capability to change/edit Coin Stack items.
- Lacks a strong database of coin types to properly display different coin types and its related attributes (% au, weight, etc)
- Lacked a reliable source to get current metal bid/ask prices.
  - Best data set had a monthly update in Quandl.
- Application will not fail gracefully.
- Parse lacked easy implementation for social login functionalities other than facebook.
- Logging out from facebook before logging out from our app invalidates the session and requires manual logout
- Application was not developed to scale.
- Data precision is lacking. The shown +/- percentages are based on a monthly comparison.
- Graph lacks much functionality. Cannot analyze on a daily/hourly basis or change the range of months.

## <a name="validation">Validation</a> [&#8593;](#toc)

## <a name="compatibility">Browser Compatibility</a> [&#8593;](#toc)
