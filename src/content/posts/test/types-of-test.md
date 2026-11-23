---
title: "Types of Testing"
author: "Christopher Chang"
description: "Simply introduce the different types of test in software development."
pubDatetime: 2026-11-23T22:42:00+08:00
featured: false
draft: false
tags: ["test", "software development"]
---


## Manual Testing and Automated Testing

It is important to distinguish between manual and automated testing. Manual testing is performed by hand, involving clicking through the application or using appropriate tools to interact with the software and APIs. This approach is very costly, as it requires personnel to set up the environment and execute tests themselves, and it is prone to human error, since testers may make typos in test scripts or omit steps.

On the other hand, automated testing is performed by computers executing pre-written test scripts. The complexity of these tests can vary widely, ranging from verifying a single method within a class to ensuring that a series of complex operations in the UI produce consistent results. While more robust and reliable than manual testing, the quality of automated tests depends on how well the test scripts are written.

Automated testing is a key component of continuous integration and continuous delivery, and it’s also a great way to scale your QA process when adding new features to your application. However, as we’ll see in this guide, there’s still value in performing some manual testing using so‑called exploratory testing.

### Different Types of Tests

### 1. Unit Testing 

Unit tests operate at a very low level, close to the application’s source code. They test individual methods and functions of the classes, components, or modules used by the software. Typically, unit tests have relatively low automation costs and can be run quickly by a continuous integration server.

### 2. Integration Testing

Integration tests verify whether the different modules or services used by your application can work well together. For example, they can test interactions with a database or ensure that microservices collaborate as expected. These types of tests are more expensive to run because they require multiple parts of the application to be up and running.

### 3. Functional Testing

Functional tests focus on the application’s business requirements. They verify only the operation’s output, without checking the system’s intermediate state during execution.

Confusion sometimes arises between integration testing and functional testing, as both require multiple components to interact. The distinction is that integration testing may simply verify that you can query the database, whereas functional testing expects to retrieve specific values from the database as defined by the product requirements.

### 4. End-to-End Testing

End-to-end testing replicates user behavior within the complete application environment. It verifies whether various user workflows operate as expected, ranging from simple tasks like loading a web page or logging in to more complex scenarios such as validating email notifications or online payments...

End-to-end testing is very useful, but it is expensive to execute and can be difficult to maintain when automated. It is recommended to perform a few critical end-to-end tests and rely more on lower-level test types (unit and integration tests) to quickly identify major changes.

### 5. Acceptance Testing

Acceptance testing is formal testing to verify that the system meets its business requirements. It requires the entire application to be up and running during testing and focuses on replicating user behavior. However, it can also go further by measuring system performance and rejecting changes if certain objectives are not met.

### 6. Performance Testing

Performance testing evaluates an application’s performance under specific workloads. These tests help measure reliability, speed, scalability, and responsiveness. For example, they can monitor response times when handling a high volume of requests or observe system behavior when processing large amounts of data. They can determine whether the application meets performance requirements, identify bottlenecks, assess stability during peak traffic, and more.

### 7. Smoke Test

Smoke tests are basic tests that verify the fundamental functionality of an application. They are designed to be executed quickly, with the goal of ensuring that the system’s core features operate as expected.

Smoke tests can be used immediately after a new version is created to determine whether more expensive tests can be run, or right after deployment to ensure the application functions properly in the newly deployed environment.

### How to perform testing automatically

To automate testing, you first need to write tests programmatically using a testing framework suitable for your application. [PHPUnit](https://phpunit.de/), [Mocha](https://mochajs.org/), and [RSpec](http://rspec.info/) are examples of testing frameworks that can be used for PHP, JavaScript, and Ruby, respectively. Each language has [many options](https://en.wikipedia.org/wiki/List_of_unit_testing_frameworks), so you may need to do some research and consult the developer community to identify the framework best suited to your needs.

When your tests can be run via scripts from the terminal, you can have a continuous integration server (such as Bamboo) execute them automatically, or use a cloud service like Bitbucket Pipelines. These tools monitor your repository and run your test suite whenever new changes are pushed to the main repository.

### Exploratory Testing

The more features and improvements your code includes, the more tests you’ll need to run to ensure everything works as expected. Furthermore, for each bug you fix, it’s wise to verify that it doesn’t reappear in newer versions. Automation is key to achieving this, and writing tests will inevitably become part of your development workflow.

So the question is, is manual testing still worthwhile? The short answer is yes; it’s best to conduct exploratory testing to uncover subtle bugs.

Exploratory testing sessions should not exceed two hours and should have a clear scope to help testers focus on specific areas of the software. Once all testers have received the briefing, they should use a variety of actions to verify the system’s functionality.