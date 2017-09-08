# Unit test with Jasmine

In this project I was given a web-based application that reads RSS feeds. The original developer of this application clearly saw the value in testing, they've already included [Jasmine](http://jasmine.github.io/) and even started writing their first test suite! Unfortunately, they decided to move on to start their own company and we're now left with an application with an incomplete test suite. That's where I came in.

## How to try this project
1. Go to [My Project](https://github.com/GustavoLastra/frontend-nanodegree-feedreader).
2. Download the project as a zip.
3. Decompress the folder and double click on the "index.html" file.

## Why this Project?

Testing is an important part of the development process and many organizations practice a standard of development known as "test-driven development". This is when developers write tests first, before they ever start developing their application. All the tests initially fail and then they start writing application code to make these tests pass.

Whether one work in an organization that uses test-driven development or in an organization that uses tests to make sure future feature development doesn't break existing features, it's an important skill to have!

## What did I learn?

How to use Jasmine to write a number of tests against a pre-existing application

## Code description

I defined the following test suites:

1. RSS Feeds
2. The menu
3. Initial Entries
4. New Feed Selection


The Unit Test is located in the "feedreader.js" file:
```
/* feedreader.js
 *
 * This is the spec file that Jasmine will read and contains
 * all of the tests that will be run against your application.
 *
 * I placed all of the tests within the $() function,
 * since some of these tests may require DOM elements.
 */
$(function() {
    describe('RSS Feeds', function() {
        it('are defined', function() {
            expect(allFeeds).toBeDefined();
            expect(allFeeds.length).not.toBe(0);
        });
        /* test that loops through each feed
         * in the allFeeds object and ensures it has a URL defined
         * and that the URL is not empty.
         */
         it('have a defined url', function(){
           for (var i=0; i<allFeeds.length; i++){
             expect(allFeeds[i].url).toBeDefined();
             expect(allFeeds[i].url).not.toBe("");
           }
         });
        /* test that loops through each feed
         * in the allFeeds object and ensures it has a name defined
         * and that the name is not empty.
         */
         it('have a defined name', function(){
           for (var i=0; i<allFeeds.length; i++){
             expect(allFeeds[i].name).toBeDefined();
             expect(allFeeds[i].name).not.toBe("");
           }
         });

    });
    /* test suite named "The menu" */
    describe('The menu', function() {
      /* test that ensures the menu element is
       * hidden by default. You'll have to analyze the HTML and
       * the CSS to determine how we're performing the
       * hiding/showing of the menu element.
       */
       it('menu element is hidden by default', function(){
         expect($('body').attr("class")).toBe("menu-hidden");
       });

       /* test that ensures the menu changes
        * visibility when the menu icon is clicked. This test
        * should have two expectations: does the menu display when
        * clicked and does it hide when clicked again.
        */
        it('menu changes visibility when the menu icon is clicked', function(){
          $('.menu-icon-link').click();
          expect($('body').attr("class")).not.toBe("menu-hidden");
          $('.menu-icon-link').click();
          expect($('body').attr("class")).toBe("menu-hidden");
        });
    });

    /* test suite named "Initial Entries" */
    describe('Initial Entries', function(){
      /* TODO: Write a test that ensures when the loadFeed
       * function is called and completes its work, there is at least
       * a single .entry element within the .feed container.
       * Remember, loadFeed() is asynchronous so this test will require
       * the use of Jasmine's beforeEach and asynchronous done() function.
       */
       beforeEach(function(done) {
         loadFeed(0,function(){
           done();
         });
       });

        it('when the loadFeed function is called and completes its work, there is at least a single .entry element within the .feed container.', function(done) {
            expect($('.feed:nth-child(0) .entry:nth-child(0)')).toBeDefined();
            done();
        });
    });

    /* test suite named "New Feed Selection" */
    describe('New Feed Selection', function(){
      /* test that ensures when a new feed is loaded
       * by the loadFeed function that the content actually changes.
       * Remember, loadFeed() is asynchronous.
       */
       $('.feed').append(Handlebars.compile($('.tpl-entry').html())("Hola entry"));

       var actual;
       var old;

       beforeEach(function(done) {
         loadFeed(0,function(){
           old = $('.feed').html();
           done();
         });
       });
       it('when a new feed is loaded by the loadFeed function that the content actually changes', function(done){
         loadFeed(3, function() {
                actual = $('.feed').html();
                expect(old).not.toEqual(actual);
                done();
            });
       });
    });
}());

```
