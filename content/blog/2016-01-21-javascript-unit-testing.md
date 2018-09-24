---
id: 1288
title: JavaScript Unit Testing and Code Coverage
date: 2016-01-21T10:24:56+00:00
author: Phong Huynh
layout: post
guid: http://www.phonghuynh.ca/?p=1288
permalink: /javascript-unit-testing/
dsq_thread_id:
  - "4510869590"
categories:
  - Learning
  - Programming
  - Web Development
---
[<img class="alignnone size-full wp-image-1290" src="/wp-content/uploads/2016/01/b1cda5a7a4e429cbb4df3f5c0d27c2b9.jpg" alt="b1cda5a7a4e429cbb4df3f5c0d27c2b9" width="3000" height="2000" />](/wp-content/uploads/2016/01/b1cda5a7a4e429cbb4df3f5c0d27c2b9.jpg)

In my last blog post <a href="http://www.phonghuynh.ca/transitioning-school-workplace/" target="_blank">A Web Developer&#8217;s Transition From School to the Workplace</a> I wrote that as a web developer, you should never stop learning and growing. I joined a new company a bit over a year ago and in the past year I have learned a lot, specifically about unit testing code. This is because on our projects, unit tests are required for a user story to be closed.

At previous companies I have worked at, unit tests were never enforced for JavaScript code. According to <a href="http://ashleynolan.co.uk/blog/frontend-tooling-survey-2015-results#q7-javascript-testing" target="_blank">this recent survey</a>, 59.66% of respondents do not use a tool to test their JavaScript.

A unit test primarily focuses on a single unit of code. For example, a function can be a unit of code that can be tested. Unit tests should be small and easy to run. They should also be independent from any dependencies which means no network or database access. Mock data objects are instead used for any code that requires data.

## Importance of Unit Testing

  * Unit tests enables you and your team to make changes to the code without breaking the existing application because the tests fail if something is broken.
  * Saves a lot of time since you will know your code is broken when the tests fail instead of manually testing the application.
  * Encourages modular code and reduces code complexity because it enforces you to write smaller and testable blocks of code.
  * Easier in the long run when you or a new developer makes changes or refactors the code in the future.
  * Helps you better understand the design of the code that is being tested.
  * Reduces the need for documentation and long comments because you are able to see all of the conditions and results expected from that unit of code.
  * Gives a nice visual feedback (green for pass and red for fail) which tells your brain whether you are doing something right or wrong.

## Examples

### AngularJS Unit Testing

<pre>describe('Home Page', function () {
    var controller;

    beforeEach(module('app.home'));

    beforeEach(inject(function ($controller) {
        controller = $controller;

        controller = $controller('HomeController', {});
    }));

    it('should set someVar to blue', function() {
        expect(controller.someVar).toEqual('blue');
    });

    it('should set someList to one, two, three', function() {
        expect(controller.someList).toEqual(['one', 'two', 'three']);
    });

    describe('Click Button', function () {
        it('should show alert message when clicked', function() {
            spyOn(window, 'alert');
            controller.someFunctionUsedByTheHomePage();
            expect(window.alert).toHaveBeenCalledWith('Congratulations');
        });
    });
});</pre>

### Sails.js (Node.js) Unit Testing

<pre>var HelloController = require('../../api/controllers/HelloWorldController'),
    assert = require('chai').assert,
    request = require('supertest'),
    url = 'http://localhost:1337';

describe('The Hello Controller', function() {
    var Sails = require('sails'),
        sails;

    before(function(done) {
      Sails.lift({}, function(err, server) {
        sails = server;
        if (err) return done(err);
            done(err, sails);
        });
    });

    after(function(done) {
        Sails.lower(done);
    });

    describe('GET /helloworld', function() {
        it('should return hello world message', function(done) {
            request(url)
                .get('/HelloWorld')
                .end(function(err, res) {
                    if (err) {
                        throw err;
                    }

                    assert(res.status, 200);
                    assert(res.text, 'Hello World');
                    done();
                });
        });
    });
});</pre>

## Code Coverage

A way to measure the amount of code that is actually being tested is to use a code coverage tool, a nice tool to use is <a href="https://github.com/gotwarlost/istanbul" target="_blank">Istanbul</a>. It is a good approach to identify if you have properly tested a line of code.

Using a coverage tool provides you a friendly visual report.

<img class="size-large wp-image-1308 aligncenter" src="/wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-1024x361.png" alt="Screen Shot 2016-01-21 at 10.05.51 AM" width="650" height="229" srcset="/wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-1024x361.png 1024w, /wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-300x106.png 300w, /wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-150x53.png 150w, /wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-1300x458.png 1300w, /wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-720x254.png 720w, /wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-400x141.png 400w, /wp-content/uploads/2016/01/Screen-Shot-2016-01-21-at-10.05.51-AM-800x282.png 800w" sizes="(max-width: 650px) 100vw, 650px" />
