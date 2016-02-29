---
layout: post
title:  "Protractor Page Objects using ES2015"
date:   2016-02-29 19:40:16 +0200
categories: protractor
---

Currently Node supports most of the ES2015 syntax required to implement page objects in protractor using the new javascript classes.
The exception currently is exporting and importing classes using the new module syntax which can still be achieved using Babel.js(http://babeljs.io/).
The below code assumes  you are using `Node : 4.3.0 or higher`. Otherwise the dependencies are no different than a typical protractor set up.
Utilizing the ES2015 page object style is cleaner and lacks the excessive layers of `function() {}` you should be accustomed to.


  `pages/HomePage.js`

  {% highlight js %}

  'use strict';

  var HomePage = class HomePage {

    get searchBox() { return element(by.name("q")); }
    get submitSearchButton() { return element(by.css("input[value='Google Search']")); }

    go() {
      browser.get("http://google.com");
    }
    inputSearchBox(text) {
      searchBox.sendKeys(text);
    }
    clickSubmitSearchButton() {
      return submitSearchButton.click();
    }

  }
  module.exports = HomePage;

  {% endhighlight %}

  * * *


  `specs/HomePage-spec.js`

  {% highlight js %}

  'use strict'

  var HomePage = require('./pages/HomePage.js');

  describe('HomePage', () => {

    var homePage = new HomePage();

    beforeEach(() => {
      browser.ignoreSynchronization = true;
      homePage.go();
    });

    it("should include search term in the search results URL", () => {
      homePage.inputSearchBox("Debout Sur Le zinc");
      expect(browser.getCurrentUrl()).toContain("Debout")
        });
    });


        {% endhighlight %}
