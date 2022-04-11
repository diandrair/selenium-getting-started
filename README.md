# Selenium-getting-started

When starting out as a Test Engineer you might overwhelmed with the amount of terminology that you are getting exposed to as a beginner. Initially, it all seems fun, but once you are getting to the automation part this is where the things might get “scary”. At some point, you even feel like “Is it for me? Can I handle all that?”.

The main problem is to get started quickly without going too much into the details (to avoid losing motivation) and have it all as a single article so that it is easy to digest. Also, it’s a good idea to show how to create test cases as a maintainable structure rather than just a sequence of commands.

For the purpose of this article the environment that we are going to use will be:

- IDE: IntelliJ IDEA
- Programming language: Java
- Browser and OS: Chrome on Mac
- Automation Tool: WebDriver + chromedriver
- Framework : Spring boot + Selenium + Cucumber

This article has you covered for all those points. Basic knowledge of Java and OOP concepts would be helpful to understand the logic of our tests.

Shall we start?

-----------------------------------------------------------------------------------------------------------------

## The Basics

Selenium WebDriver is a tool to automate the execution of manual Web-Browser workflows. It might be either a simple form that you are tired of filling out multiple times or a complex system that verifies hundreds of Web pages.

The idea here is that we are loading a specific webpage(s), performing actions with it, and comparing if results were those that we expected or not. Of course, there are some variations in terms of setting up the environment depending on which platform do we use (e.g Windows, Mac, or Linux).

The typical syntax looks like this:

<img width="589" alt="image" src="https://user-images.githubusercontent.com/77116793/162722968-d33db328-6108-48e6-b3c7-fa2f6caffb3c.png">

Our plan of action on a webpage comes down to this scenario most of the time:

1. Load a webpage (specific section of the website)
2. Find an element that we want to perform an action on. There are multiple ways to identify an element on a webpage. Those element identifiers are called “locators”. We are going to be examining “XPath” and “id” locators in this article
3. Perform an action with it. Most of the time it involves sending a specific key sequence and/or clicking the element
4. Validate the state. This is where the power of Unit-testing frameworks (e.g TestNG, JUnit) comes handy. We are making sure that the actual result matches the expected result

