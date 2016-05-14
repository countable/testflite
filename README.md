# AbAdmin

AbAdmin is a simple plugin which enables Ab split testing for NodeJS apps. 

This package is developed because I couldn't find a nice one to include in my app without changing too much code. For this to work, only two lines of code are needed, and I've also created a simple interface to add/view/delete tests.

The requirements are: 
 * [Express](expressjs.com)
 * [CookieParser](https://github.com/expressjs/cookie-parser)
 * [BodyParser](https://github.com/expressjs/body-parser)
 * Any templating engine (such as [Jade](jade-lang.com)) which follows the Express convention (render html with ```res.render(template, data[,callback])```)

### Setting up
To get started with AbAdmin, you only need to include it in your main JS file, after including CookieParser and BodyParser. 

First, navigate to your folder and create an express app using

```express [name of the project]```

Then install AbAdmin

```npm install abadmin```

Require AbAdmin using 

```var abadmin = require('abadmin');```

and initiate it with ```app``` parameter, after those ```app.use(/**/)``` statements

```javascript
// uncomment after placing your favicon in /public
//app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

//initiate ab admin
abadmin(app);

```

Thats it, you are good to go!

### Ab test introduction

Take a following example: you own a web shop, and you are thinking about changing the design. Now, instead of changing it, you are smart, and want to test multiple designs for the products page first. 

What do you do?

Lets say you change the buy button color from green to red, and make it slightly bigger. Now you want to display the old page, and then the new page, and then the old page, and so on. Of course, if a user has visited you website and have seen the old page, you want to display only the old page to him. 

Both pages lead to the third page, which is the checkout page. You want to test which one will have better conversion rate. If both pages have the same amount of unique visitors, you want to check how many have bought the product by coming from the old page, and how many from the new page.

Terminology:
* Page A is the old page
* Page B is the new page
* Page C is the checkout page (in the previous example)
* Weight is the ratio, e.g. for each 1 user of the old page, send 2 users to the new page
* Hits is a number of unique users that have visited the page
* Returns is the number of users that have visited the destination page through that page

### Usage
To use the package, go to ```/abadmin```, which will open a nice interface to allow you to add, view stats and remove tests. With the given explanation, there should be no problem on usage.

Before creating the tests, create the templates and put them in the ```views``` folder.
Then create the test and save it. Changes will take place immidiately. To test it, open the page you are testing with one browser, and then with some other, and you will see the changes.

### Persistence
The tests are saved in a file named ```abadmin.tests.json``` and have the following format:

```javascript
[
  {
  "id": "auto-generated",
  "name": "[name of the test]",
  "destination": "[Page C of the test]",
  "dynamicWeight": false,
  "page": {
  	"template": "[Page A template]",
    "weight": 1,
    "hits": 0,
    "returns": 0
  },
  "variations": [
  	{
    	"template": "[Variation B template]",
        "weight": 2,
        "hits": 0,
        "returns": 0
      }
      ...
    ]
  }
]
```

This file can be edited manually and then server should be restarted, or visit the page ```/abadmin/reloaddb``` which will read from the file. 

Notice: this file is overwritten each time someone visits any page that can trigger tests, so it can have different content then what you've written when you reload the database. Using the web interface is recommended.

MIT License, 

Pavlović Dž Filip
