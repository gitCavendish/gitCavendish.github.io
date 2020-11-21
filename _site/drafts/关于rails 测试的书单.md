主题集中于 Rspec 专门针对 Minitest 的内容不多。

视频

https://www.pluralsight.com/courses/rspec-ruby-application-testing

书籍

---

术语解释

https://www.martinfowler.com/bliki/TestDouble.html


---

### Effective Testing with RSpec 3

https://pragprog.com/book/rspec3/effective-testing-with-rspec-3

#### 作者：

By Ian Dees, Myron Marston

Author
Myron Marston, a longtime Ruby programmer, has led the development of RSpec since 2012. He works as a senior software engineer at Moz in Seattle. Myron tweets as @myronmarston.

By day, Ian Dees slings code, tests, and puns at a Portland-area test equipment manufacturer. By night, he converts espresso into programming books, including Cucumber Recipes. Ian tweets as @undees.

#### 出版信息：

Release Date: August 2017

Pages: 356

#### 目录

1 Getting Started

- Getting Started With RSpec
  - Installing RSpec
  - Your First Spec
  - Understanding Failure
  - Sharing Setup (But Not Sandwiches)
  - Your Turn

- From Writing Specs to Running Them excerpt
  - Customizing Your Specs’ Output
  - Identifying Slow Examples
  - Running Just What You Need
  - Marking Work in Progress
  - Your Turn

- The RSpec Way
  - What Your Specs Are Doing for You
  - Comparing Costs and Benefits
  - Different Types of Specs
  - Guidelines

2 Building an App With RSpec 3

- Starting On the Outside: Acceptance Specs
  - First Steps
  - Deciding What to Test First
  - Checking the Response
  - Filling In the Response Body
  - Querying the Data
  - Saving Your Progress: Pending Specs
  - Your Turn
- Testing in Isolation: Unit Specs
  - From Acceptance Specs to Unit Specs
  - Filling In the First Spec
  - Handling Success
  - Refactoring
  - Handling Failure
  - Defining the Ledger
  - Your Turn
- Getting Real: Integration Specs
  - Hooking Up the Database
  - Testing Ledger Behavior
  - Testing the Invalid Case excerpt
  - Isolating Your Specs Using Database Transactions
  - Filling In the Behavior
  - Querying Expenses
  - Ensuring the Application Works for Real
  - Your Turn

3 RSpec Core
- Structuring Code Examples
  - Getting the Words Right
  - Sharing Common Logic
  - Sharing Example Groups
  - Your Turn
- Slicing and Dicing Specs with Metadata
  - Defining Metadata
  - Reading Metadata
  - Selecting Which Specs to Run
  - Sharing Code Conditionally
  - Changing How Your Specs Run
  - Your Turn
- Configuring RSpec
  - Command-Line Configuration
  - Setting Command-Line Defaults
  - Using a Custom Formatter
  - RSpec.configure
  - Your Turn

4 RSpec Expectations

- Exploring RSpec Expectations
  - Parts of an Expectation
  - How Matchers Work
  - Composing Matchers
  - Generated Example Descriptions
  - Your Turn
- Matchers Included in RSpec Expectations
  - Primitive Matchers
  - Higher-Order Matchers
  - Block Matchers
  - Your Turn
- Creating Custom Matchers
  - Delegating to Existing Matchers Using Helper Methods
  - Defining Matcher Aliases
  - Negating Matchers
  - Using the Matcher DSL
  - Defining a Matcher Class
  - Your Turn

5 RSpec Mocks

- Understanding Test Doubles
  - Types of Test Doubles
  - Usage Modes: Mocks, Stubs, and Spies
  - Origins: Pure, Partial, and Verifying Doubles excerpt
  - Your Turn
- Customizing Test Doubles
  - Configuring Responses
  - Setting Constraints
  - Your Turn
- Using Test Doubles Effectively
  - Constructing Your Test Environment
  - Stubject (Stubbing the Subject)
  - Using Partial Doubles Effectively
  - Connecting the Test Subject to Its Environment
  - The Risks of Mocking Third-Party Code
  - High-Fidelity Fakes
  - Faking I/O with StringIO
  - Wrapping a Third-Party Dependency
  - Your Turn

---

### Rails 5 Test Prescriptions
Build a Healthy Codebase

https://pragprog.com/book/nrtest3/rails-5-test-prescriptions

#### 作者：

作者


Noel Rappin is a Principal Software Engineer at Table XI, and is the author of multiple technical books. Before joining Table XI, Noel ran internal training at Groupon, and has a Ph.D. in educational technology and user-centered design from the GVU Lab at the Georgia Institute of Technology. Find Noel online at noelrappin.com.

#### 出版信息：

Publication date:	02/16/2018

PAGE 404

#### 目录

Acknowledgments

1 Preface
- Who You Are
- What’s in This Book
- What You’ll Need
- A Word About Tools, Best Practices, and Teaching TDD
- Changes in This Edition
- Online Resources

2 A Test-Driven Fable
- Testing First Drives Design
- What Is TDD Good For?
- When TDD Needs Some Help
- What You’ve Done

3 Test-Driven Development Basics
- Infrastructure
- The Requirements
- Installing RSpec
- Where to Start? excerpt
- Running the Test
- Making the Test Pass
- The Second Test
- Back on Task
- Adding Some Math
- The First Date
- Using the Time Data
- What You’ve Done

4 Test-Driven Rails
- Let’s Write Some Rails
- The Days Are Action-Packed
- Who Controls the Controller?
- A Test with a View
- Testing for Failure
- What You’ve Done

5 What Makes Great Tests
- The Big One
- Cost and Value
- SWIFT: The Five Qualities of Valuable Tests
- What You’ve Done

6 Testing Models excerpt
- What Can You Do in a Model Test?
- What Should You Test in a Model Test?
- Okay, Funny Man, What Makes a Good Set of Model Tests?
- Refactoring Models
- A Note on Assertions per Test
- Testing What Rails Gives You
- Testing ActiveRecord Finders
- Testing Shared Modules and ActiveSupport Concerns
- Writing Your Own RSpec Matchers
- What You’ve Done

7 Adding Data to Tests
- What’s the Problem?
- Fixtures
- Factories
- Dates and Times
- What You’ve Done

8 Using Test Doubles as Mocks and Stubs
- Test Doubles Defined
- Creating Stubs
- Mock Expectations
- Using Mocks to Simulate Database Failure
- Using Mocks to Specify Behavior
- More Expectation Annotations
- Mock Tips
- What You’ve Done

9 Integration Testing with Capybara and Cucumber
- A Field Guide to Integration and System Tests
- Setting Up Capybara
- Using Feature Tests to Build a Feature
- What to Test in an RSpec System Test
- Outside-in Testing
- Making the Capybara Test Pass
- Retrospective
- Setting Up Cucumber
- Writing Cucumber Features
- Writing Cucumber Steps
- Advanced Cucumber
- Is Cucumber Worth It?
- What You’ve Done

10 Testing JavaScript: Integration Testing
- Integration-Testing JavaScript with Capybara
- Let’s Talk About Drivers
- Making the Test Pass
- Webpack in Developer Mode
- What You’ve Done

11 Unit-Testing JavaScript
- Setting Up JavaScript Unit Tests
- Writing a Sample Test
- TDD in JavaScript
- Jasmine Matchers
- Testing Ajax Calls
- Using testdouble.js
- Connecting the JavaScript to the Server Code
- What You’ve Done

12 Testing Rails Display Elements
- Testing Routes
- Testing Helper Methods excerpt
- Testing Controllers and Requests
- Simulating Requests
- What to Expect in a Request Spec
- Older Rails Controller Tests
- Testing Mailers
- Testing Views and View Markup
- Using Presenters
- Testing Jobs and Cables
- What You’ve Done

13 Minitest
- Getting Started with Minitest
- Minitest Basics
- Running Minitest
- Minitest Setup
- Mocha
- System Tests and Others
- Minitest Helper Tests
- Minitest and Routing
- What You’ve Done

14 Testing for Security
- User Authentication and Authorization
- Adding Users and Roles
- Restricting Access
- More Access-Control Testing
- Using Roles
- Protection Against Form Modification
- Mass Assignment Testing
- Other Security Resources
- What You’ve Done

15 Testing External Services
- External Testing Strategy
- The Service Integration Test
- Introducing VCR
- Client Unit Tests
- Why an Adapter?
- Adapter Tests
- Testing for Error Cases
- Smoke Tests and VCR Options
- The World Is a Service
- What You’ve Done

16 Troubleshooting and Debugging
- General Principles
- The Humble Print Statement
- Git Bisect
- RSpec or Minitest Bisect
- Pry
- Common Rails Gotchas
- What You’ve Done

17 Running Tests Faster and Running Faster Tests
- Running Smaller Groups of Tests
- Running Rails in the Background
- Running Tests Automatically with Guard
- Writing Faster Tests by Bypassing Rails
- Recommendations for Faster Tests
- What You’ve Done

18 Testing Legacy Code
- What’s a Legacy?
- Set Expectations
- Getting Started with Legacy Code
- Test-Driven Exploration
- Dependency Removal
- Find the Seam
- Don’t Look Back
- What You’ve Done



---

### Testing Rails

https://gumroad.com/l/testing-rails

#### 作者：

Testing Rails is written by Josh Steiner and Joël Quenneville.

#### 出版信息：

2017年3月1日

#### 目录：


Table of Contents

1 Introduction
- Why test?
- Test Driven Development
- Characteristics of an Effective Test Suite
- Example Application
- RSpec

2 Types of Tests
- The Testing Pyramid
- Feature Specs
- Model Specs
- Request Specs
- View Specs
- Controller Specs
- Helper Specs
- Mailer Specs

3 Intermediate Testing
- Testing in isolation (doubles, stubs, spies, and mocks)
- External Services
- Page Objects
- JavaScript
- Continuous Integration
- Coverage Reports

4 Antipatterns

- Slow Tests
- Intermittent Failures
- Brittle Tests
- Duplication
- Testing Implementation Details
- Let, Subject, and Before
- Bloated Factories
- Using Factories Like Fixtures
- False Positives
- Stubbing the System Under Test
- Testing Behavior, Not Implementation
- Testing Code You Don't Own


---

### Everyday Rails Testing with RSpec

https://leanpub.com/everydayrailsrspec

A practical approach to test-driven development
Aaron Sumner

中文版：使用 RSpec 测试 Rails 应用

https://leanpub.com/everydayrailsrspec-cn


#### 作者：

Hi, I'm Aaron Sumner. I've been developing for the web since 1994, moving from static HTML to Applescript(!) to Perl to PHP and now to Ruby on Rails. In my blog, Everyday Rails, I show how I leverage tools from the Ruby ecosystem to be a productive developer, even when time and other resources are tight.

#### 出版信息：

2018-01-07

238 PAGES

47,061 WORDS


#### 目录

Table of Contents

Preface to this edition

Acknowledgements

1 Introduction
- Why RSpec?
- Who should read this book
- My testing philosophy
- How the book is organized
- Downloading the sample code
- Code conventions
- Discussion and errata
- A note about gem versions
- About the sample application

2 Setting up RSpec
- Gemfile
- Test database
- RSpec configuration
- Faster test suite start times with the rspec binstub
- Try it out!
- Generators
- Summary
- Questions
- Exercises

3 Model specs
- Anatomy of a model spec
- Creating a model spec
- The RSpec syntax
- Testing validations
- Testing instance methods
- Testing class methods and scopes
- Testing for failures
- More about matchers
- DRYer specs with describe, context, before and after
- Summary
- Question
- Exercise

4 Creating meaningful test data
- Factories versus fixtures
- Installing Factory Girl
- Adding factories to the application
- Generating unique data with sequences
- Associations in factories
- Avoiding duplication in factories
- Callbacks
- How to use factories safely
- Summary
- Exercises

5 Controller specs
- Basic controller specs
- Authenticated controller specs
- Testing user input
- Testing user input errors
- Handling non-HTML output
- Summary
- Question
- Exercises

6 Testing the user interface with feature specs
- Why feature specs?
- Additional dependencies
- A basic feature spec
- The Capybara DSL
- Debugging feature specs
- Testing JavaScript interactions
- Headless drivers
- Waiting for JavaScript
- Summary
- Exercises

7 Testing the API with request specs
- Request specs versus feature specs
- Testing GET requests
- Testing POST requests
- Replacing controller specs with request specs
- Summary
- Exercise

8 Keeping specs DRY
- Support modules
- Lazy-loading with let
- Shared contexts
- Custom matchers
- Aggregating failures
- Maintaining test readability
- Summary
- Exercise

9 Writing tests faster, and writing faster tests
- RSpec’s terse syntax
- Editor shortcuts
- Mocks and stubs
- Tags
- Remove unnecessary tests
- Run tests in parallel
- Take Rails out of the equation
- Summary
- Exercises

10 Testing the rest
- Testing file uploads
- Testing background workers
- Testing email delivery
- Testing web services
- Summary
- Exercises

11 Toward test-driven development
- Defining a feature
- From red to green
- Going outside-in
- The red-green-refactor cycle
- Summary
- Exercises

12 Parting advice
- Practice testing the small things
- Be aware of what you’re doing
- Short spikes are OK
- Write a little, test a little is also OK
- Try to write integration specs first
- Make time for testing
- Keep it simple
- Don’t revert to old habits!
- Use your tests to make your code better
- Sell others on the benefits of automated testing
- Keep practicing
- Goodbye, for now
- More testing resources for Rails
- RSpec
- Rails testing
- Ruby Tapas
- About Everyday Rails
- About the author
- Colophon
- Change log

---



https://gumroad.com/l/ruby-science

关于优化ruby代码的好书。

sample https://thoughtbot.com/ruby-science-sample.pdf

---

Rails 中嵌入地图相关功能的书

https://gumroad.com/l/geocoding-on-rails

samle https://thoughtbot.com/geocoding-sample.pdf
