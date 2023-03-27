# Cucumber testing framework
## What is Cucumber?
Cucumber is a behavior-driven development (BDD) testing framework that allows software teams to create and run automated tests in a human-readable format.  
Originally developed in the Ruby programming language, but in the meanwhile it has been ported to other languages, such as Java and JavaScript.
Cucumber allows developers, testers, and business stakeholders to collaborate on defining the behavior of an application using plain-text feature files. 
These feature files describe the application's functionality in a way that is easily understandable by non-technical stakeholders, such as product owners or business analysts.  
By using Cucumber, teams can then generate automated test code from these feature files, which can be run against the application to verify its behavior. 
This helps to ensure that the application meets its intended functionality and requirements, while also providing a way to catch and fix bugs early in the development process.   
Cucumber has become a popular choice for teams looking to adopt a BDD approach to testing, as it promotes collaboration and communication between stakeholders, and helps to improve the overall quality of the software being developed.

## Key features
- Supports BDD: Cucumber is built on the principles of behavior-driven development(BDD), which focuses on defining the behavior of an application in a way that is easily understandable by non-technical stakeholders.
- Plain-text feature files: Cucumber uses plain-text feature files to describe the behavior of an application. These feature files can be written in a natural language format that is easily readable and understood by both technical and non-technical stakeholders.
- Integrates with other testing tools: Cucumber can be easily integrated with other testing tools, such as Selenium, to automate the testing process and provide better test coverage.
- Supports multiple programming languages: Cucumber was originally developed in Ruby, but has since been ported to other programming languages, such as Java and JavaScript, making it accessible to a wider range of developers.
- Test code generation: Cucumber can automatically generate test code from feature files, reducing the time and effort required to write and maintain test code.
- Step definitions: Cucumber allows developers to define reusable step definitions, which can be shared across multiple feature files and scenarios.
- Data tables and scenario outlines: Cucumber supports data tables and scenario outlines, which can be used to test different scenarios with different input values.
- Tagging and filtering: Cucumber allows scenarios to be tagged and filtered, making it easier to run specific tests or groups of tests.
- Parallel test execution: Cucumber supports parallel test execution, which can help to speed up the testing process and improve overall efficiency.

## Installation for Java
Cucumber is published in the central Maven repository. You can install it by adding dependencies to your project in your pom.xml file:
Make sure the Cucumber version is the same for all Cucumber dependencies.
```
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-java</artifactId>
    <version>7.11.2</version>
    <scope>test</scope>
</dependency>
```


## How does Cucumber work?
Cucumber reads executable specifications written in plain text and validates that the software does what those specifications say. 
The specifications consist of multiple examples, or scenarios. This all happens in the feature file.  
For example:

```Code Example
Scenario: Breaker guesses a word
Given the Maker has chosen a word
When the Breaker makes a guess
Then the Maker is asked to score
```

Each scenario is a list of steps for Cucumber to work through.  
In order for Cucumber to understand the scenarios, they must follow some basic syntax rules, called Gherkin.  
Gherkin is a set of grammar rules that makes plain text structured enough for Cucumber to understand.
Gherkin documents are stored in .feature text files.

After the feature files are written, it's time to define step definitions, which are blocks of code that define how each step in a scenario is implemented.  
Step definitions connect Gherkin steps to programming code, they carry out the action that should be performed by the step. So they hard-wire the specification to the implementation.
When Cucumber encounters a step without matching step definition, it will print a step definition snippet with a matching Cucumber expression.
You can use this as a good starting point for a new step definition.   
Note that step definitions aren't linked to a particular feature file or scenario. The only thing that matters is the step definition's expression.

Step definitions can be written in many programming languages.
Here is an example using JavaScript:
```Code Example
When("{maker} starts a game", function(maker) {
maker.startGameWithWord({ word: "whale" })
})
```

After the step definitions are defined, you can execute the feature file using a Cucumber runner, such as JUnit or TestNG.  
The runner reads the feature files and executes the step definitions in the correct order to simulate the behavior of the application.  
Once completed, Cucumber generates reports that provide detailed information on the results of the tests.  
This includes information such as the number of passed and failed scenarios, time taken to execute each scenario and any errors or exceptions encountered during testing.

## Advanced Cucumber Techniques:
## Scenario outlines
The Scenario Outline keyword can be used to run the same Scenario multiple times, with different combinations of values.  

Copying and pasting scenarios to use different values quickly becomes tedious and repetitive:
```
Scenario: eat 5 out of 12
  Given there are 12 cucumbers
  When I eat 5 cucumbers
  Then I should have 7 cucumbers

Scenario: eat 5 out of 20
  Given there are 20 cucumbers
  When I eat 5 cucumbers
  Then I should have 15 cucumbers
```
By using Scenario Outline, we can turn these Scenarios into one.
```
Scenario Outline: eating
  Given there are <start> cucumbers
  When I eat <eat> cucumbers
  Then I should have <left> cucumbers

  Examples:
    | start | eat | left |
    |    12 |   5 |    7 |
    |    20 |   5 |   15 |
```
Not only does it remove the duplicate Scenarios and Step Definitions, but it also makes it easy to add or remove test data.

## Cucumber Tags
Tags are a great way to organise your features and scenarios. They can be used for two purposes:
- Running a subset of scenarios
- Restricting hooks to a subset of scenarios

Any feature or scenario can have as many tags as you like. Separate them with spaces.
Tags can be placed above Features, Scenarios, Scenario Outlines and Examples.

```Code example
@SmokeTest @RegressionTest
Scenario: Successful Login
Given This is a blank test

@RegressionTest
Scenario: UnSuccessful Login
Given This is a blank test

Scenario Outline: Steps will run conditionally if tagged
  Given user is logged in
  When user clicks <link>
  Then user will be logged out

  @mobile
  Examples:
    | link                  |
    | logout link on mobile |

  @desktop
  Examples:
    | link                   |
    | logout link on desktop |
```
Note that you can use tags on different examples as showed above in the code.
It is not possible to place tags above Background or steps (Given, When, Then, And and But).

## Cucumber Hooks
Hooks are blocks of code that can run at various points in the Cucumber execution cycle.   
They are typically used for setup and teardown of the environment before and after each scenario.  
There are multiple types of hooks, for scenarios, steps and even global ones.

Scenario hooks: 
- Before : Runs before the first step of each scenario. Whatever happens in a Before hook is invisible to people who only read the features. You should consider using a background as a more explicit alternative, especially if the setup should be readable by non-technical people. Only use a Before hook for low-level logic such as starting a browser or deleting data from a database.
- After : Runs after the last step of each scenario, even when the step result is failed, undefined, pending or skipped.

```
@Before
public void doSomethingBefore() {
}
@After
public void doSomethingAfter(Scenario scenario){
    // Do something after after scenario
}
```

Step hooks:
- BeforeStep
- AfterStep
```
@BeforeStep
public void doSomethingBeforeStep(Scenario scenario){
}
@AfterStep
public void doSomethingAfterStep(Scenario scenario){
}
```

Global hooks: 
- BeforeAll : runs before any scenario is run.
- AfterAll : runs after all scenarios have been executed.

## Background
Occasionally you'll find yourself repeating the same Given steps in all the scenarios in a Feature.
Since it is repeated in every scenario, this is an indication that those steps are not essential to describe the scenarios.   
You can literally move such Given steps to the background, by grouping them under a Background section.  
A Background allows you to add some context to the scenarios that follow it. 
It can contain one or more Given steps, which are run before each scenario, but after any Before hook.

```Code example
Feature: Login functionality

Background:
    Given the user navigates to the login page

  Scenario: Valid login
    When the user enters valid credentials
    Then the user should be logged in

  Scenario: Invalid login
    When the user enters invalid credentials
    Then the user should see an error message
```
You can only have one set of Background steps per Feature.   
If you need different Background steps for different scenarios, consider breaking up your set of scenarios into more Rules or more Features.

## Benefits of Cucumber
- Improved collaboration between business and development teams: Cucumber's use of plain-text feature files helps to bridge the gap between technical and non-technical stakeholders, allowing them to collaborate more effectively in defining and testing the behavior of an application.
- Better test automation: Cucumber's ability to generate test code from feature files makes it easier to automate testing, reducing the time and effort required to write and maintain test code. Code can be reused if a similar step has to be performed.
- Higher code coverage: By providing a way to define the behavior of an application in a comprehensive and structured way, Cucumber helps to ensure that all aspects of the application are thoroughly tested and that there are no gaps in the testing coverage.
- More reliable tests: Cucumber's use of plain-text feature files and step definitions promotes consistency and reduces the risk of errors in test code, making tests more reliable and repeatable. If test data changes, the only changes needed to be made are the ones in the feature file.
- Improved quality: By catching and fixing bugs earlier in the development process, Cucumber helps to improve the overall quality of the software being developed, reducing the risk of defects and issues in production.
- Greater efficiency: Cucumber's support for parallel test execution and integration with other testing tools helps to improve the efficiency of the testing process, allowing teams to test more quickly and effectively.

## Best Practices
- Keep feature files simple and readable: Cucumber's strength is its use of plain-text feature files that can be easily understood by both technical and non-technical stakeholders. It's important to keep these files simple, concise, and easy to read.
- Use consistent and meaningful naming conventions: Use consistent and meaningful names for feature files, scenarios, and step definitions. This helps to improve clarity and makes it easier to maintain the codebase.
- Write atomic scenarios: Write atomic scenarios that focus on testing a single behavior or feature. This makes it easier to isolate and fix issues when they arise.
- Use tags effectively: Use tags to categorize scenarios and run specific subsets of tests. This can help to speed up the testing process and make it easier to focus on specific areas of the application.
- Write reusable step definitions: Write reusable step definitions that can be used across multiple feature files and scenarios. This reduces the amount of duplicated code and makes it easier to maintain the codebase.
- Use data tables and scenario outlines: Use data tables and scenario outlines to test different scenarios with different input values. This improves test coverage and helps to catch issues that may not be evident in a single test case.
- Use version control: Use version control, such as Git, to manage changes to the feature files and step definitions. This makes it easier to track changes and revert to previous versions if needed.
- Perform regular code reviews: Perform regular code reviews to ensure that the feature files and step definitions are consistent, maintainable, and adhere to best practices.

## Challenges and limitations
- Steep learning curve: Cucumber can have a steep learning curve, especially for non-technical stakeholders who are not familiar with testing and test automation.
- Maintenance overhead: While Cucumber's ability to generate test code from feature files reduces the time and effort required to write and maintain test code, there is still a maintenance overhead involved in keeping the feature files and step definitions up-to-date with changes in the application.
- Integration challenges: Cucumber can face integration challenges with other testing tools, especially when using custom frameworks or complex test environments.
- Slower execution times: Running tests through Cucumber can be slower than running them through other testing tools, which can be a limitation for teams working on large or complex applications.
- Limited reporting: Cucumber's reporting capabilities can be limited, which can make it difficult to analyze and interpret test results.
- Over-reliance on natural language: While Cucumber's use of natural language can make it easier for non-technical stakeholders to participate in testing, it can also lead to ambiguity and misinterpretation if the language used in the feature files is not precise enough
- Lack of support for some testing types: While Cucumber is great for functional and acceptance testing, it may not be suitable for other types of testing, such as load testing or security testing.

## Conclusion
In conclusion, Cucumber is a powerful tool for automated testing that promotes collaboration and communication between developers, testers and business stakeholders, leading to a better understanding of the requirements and behavior of the application being tested.   
Cucumber's key features such as the ability to write tests in plain text, the use of step definitions to define behavior and detailed reports make it an effective and efficient tool for testing software applications.  
While there are some challenges and limitations to using it, most of these can be overcome through careful planning, best practices and continuous improvement.

Overall, Cucumber can help to improve the quality, reliability and maintainability of applications, and should be considered as an option for any team looking to implement automated testing in their development process.

## Reference
Homepage: https://cucumber.io/

