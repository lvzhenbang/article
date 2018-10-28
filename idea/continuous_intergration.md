## 持续集成

不久前，我对持续集成(CI)有了初步的了解，我认为这似乎是一个额外的过程，迫使工程师在已经很大的项目上做额外的工作。我的团队开始在项目中实施CI，经过一些实践经验，我意识到它的巨大好处，不仅对公司，对我，一个工程师！在这篇文章中，我将描述使用CI的好处，以及如何免费、快速地实现它。

CI和持续交付(CD)通常一起讨论。在一篇文章中同时写 CI 和 CD 需要同时写和读很多东西，所以我在这里只讨论CI。也许我会在以后的文章里提到CD。😉

### CI是什么？

据我所知，持续集成是一种将测试、安全检查和开发实践结合在一起的编程模式，可以自信地将代码从开发分支不断地推到生产准备分支。

Microsoft Word就是CI的一个例子。单词被写入程序中，并根据拼写和语法算法进行检查，以确定文档的总体可读性和拼写能力。

### 为什么CI应该无处不在

我们已经对此进行了一些讨论，但我认为CI最大的好处是，它提高了工程师的工作效率，可以节省很多钱。具体来说，它提供了更快的反馈循环，更容易集成，并且减少了瓶颈。直接将CI与公司的节省成本联系起来是困难的，因为SaaS的成本随着用户基础的变化而扩大。因此，如果开发人员想将CI应用到业务，可以使用下面的公式。很好奇它能省多少钱吧？我的朋友David Inoa创建了下面的演示程序来帮助计算节省的成本。

[CI在更新依赖关系时需要进行大量的猜测，否则需要进行大量的调查和测试](https://res.cloudinary.com/css-tricks/image/upload/c_scale,w_900,f_auto,q_auto/v1540414981/no-ci-wont-work-flat_hk1mzt.jpg)

真正让人兴奋到尖叫的是CI可以让你和我作为开发者受益！

首先，CI将节省你的时间。多少时间？我们说的是每周几个小时。如何？哦，我要告诉你吗？CI自动测试你的代码，并让你知道是否可以将其合并到用于生产的分支中。你将花费大量的时间来测试你的代码，并与其他人一起为生产准备代码。

还有一种方法可以防止代码疲劳。它支持Greenkeeper等工具，它可以在代码检查后自动设置甚至合并请求。这使代码保持最新，并允许开发人员关注我们真正需要做的事情。比如写代码或者生活。包内的代码更新通常只需要对主要版本更新进行审查，因此不需要跟踪每一个需要操作的小版本。

### 不要找借口，使用CI！

当与开发人员交谈时，对话通常会以这样的方式结束:

“我会用CI，但……(插入借口)。”

对我来说，那是逃避！CI可以是免费的。这也很容易。的确，CI的好处伴随着一些成本，包括CircleCI或Greenkeeper等工具的月租费。但这对于它提供的长期储蓄来说只是九牛一毛。同样正确的是，需要时间来安排事情。但是值得指出的是，CI的力量可以在开源项目中免费使用。如果你需要或想保持你的代码的私密性，并且不想为CI工具付费，那么你真的可以使用一些优秀的npm包构建你自己的CI设置。

所以，别再找借口了，看看CI的力量吧！

### CI能解决哪些问题？

在进一步深入之前，我们应该介绍CI的用例。它解决了很多问题，在很多情况下都会派上用场:

当多个开发人员希望同时合并到生产分支时

在部署前没有发现错误或无法修复错误

当依赖项过时

当开发人员不得不等待更长的时间来合并代码时

当包依赖于其他包时

更新包时，必须在多个位置进行更改

Recommended CI tools
Let’s look at the high level parts used to create a CI feedback loop with some quick code bits to get CI setup for any open source project today. We’ll break this down into digestible chunks.

### Documentation

In order to get CI working for me right away, I usually set CI up to test my initial documentation for a project. Specifically, I use MarkdownLint and Write Good because they provide all the features and functionality I need to write tests for this part of the project.

The great news is that GitHub provides standard templates and there is a lot of content that can be copied to get documentation setup quickly. Read more about quickly setting up documentation and creating a documentation feedback loop.

I keep a package.json file at the root of the project and run a script command like this:

"grammar": "write-good *.md --no-passive",
"markdownlint": "markdownlint *.md"
Those two lines allow me to start using CI. That’s it！ I can now run CI to test grammar.

At this point, I can move onto setting up CircleCI and Greenkeeper to help me make sure that packages are up to date. We’ll get to that in just a bit.

### Unit testing
Unit tests are a method for testing small blocks (units) of code to ensure that the expected behavior of that block works as intended.

Unit tests provide a lot of help with CI. They define code quality and provide developers with feedback without having to push/merge/host code. Read more about unit tests and quickly setting a unit test feedback loop.

Here is an example of a very basic unit test without using a library:

const addsOne = (num) => num + 1 // We start with 1 as an initial value
  const numPlus1 = addsOne(3) // Function to add 3
  const stringNumPlus1 = addsOne('3') // Add the two functions, expect 4 as the value
    
  /**
    * console.assert
    * https://developer.mozilla.org/en-US/docs/Web/API/console/assert
    * @param test？
    * @param string
    * @returns string if the test fails
    **/
    
  console.assert(numPlus1 === 4, 'The variable `numPlus1` is not 4！')
  console.assert(stringNumPlus1 === 4, 'The variable `stringNumPlus1` is not 4！')
Over time, it is nice to use libraries like Jest to unit test code, but this example gives you an idea of what we’re looking at.

Here’s an example of the same test above using Jest:

const addsOne = (num) => num + 1

describe('addsOne', () => {
  it('adds a number', () => {
    const numPlus1 = addsOne(3)
    expect(numPlus1).toEqual(4)
  })
  it('will not add a string', () => {
    const stringNumPlus1 = addsOne('3')
    expect(stringNumPlus1 === 4).toBeFalsy();
  })
})
Using Jest, tests can be hooked up for CI with a command in a package.json like this:

"test:jest": "jest --coverage",
The flag --coverage configures Jest to report test coverage.

### Safety checks
Safety checks help communicate code and code quality. Documentation, document templates, linter, spell checkers, and type checker are all safety checks. These tools can be automated to run during commits, in development, during CI, or even in a code editor.

Safety checks fall into more than one category of CI: feedback loop and testing. I’ve compiled a list of the types of safety checked I typically bake into a project.

All of these checks may seem like another layer of code abstraction or learning, so be gentle on yourself and others if this feels overwhelming. These tools have helped my own team bridge experience gaps, define shareable team patterns, and assist developers when they're confused about what their code is doing.

Committing, merging, communicating: Tools like husky, commitizen, GitHub Templates, and Changelogs help keep CI running clean code and form a nice workflow for a collaborative team environment.
Defining code (type checkers): Tools like TypeScript define and communicate code interfaces — not only types！
Linting: This is the practice of ensuring that something matches defined standards and patterns. There’s a linter for nearly all programming languages and you’ve probably worked with common ones, like ESlint (JavaScript) and Stylelint (CSS) in other projects.
Writing and commenting: Write Good helps catch grammar errors in documentation. Tools like JSDoc, Doctrine, and TypeDoc assist in writing documentation and add useful hints in code editors. Both can compile into markdown documentation.
ESlint is a good example for how any of these types of tools are implemented in CI. For example, this is all that’s needed in package.json to lint JavaScript:

"eslint": "eslint ."
Obviously, there are many options that allow you to configure a linter to conform to you and your team’s coding standards, but you can see how practical it can be to set up.

### High level CI setup
Getting CI started for a repository often takes very little time, yet there are plenty of advanced configurations we can also put to use, if needed. Let’s look at a quick setup and then move into a more advanced configuration. Even the most basic setup is beneficial for saving time and code quality！

Two features that can save developers hours per week with simple CI are automatic dependency updates and build testing. Dependency updates are written about in more detail here.

Build testing refers to node_modules installation during CI by running an install — for example, (npm install where all node_modules install as expected. This is a simple task and does fail. Ensuring that node_modules installs as expected saves considerable time！

### Quick CI Setup
CI can be setup automatically for both CircleCI and Travis！ If a valid test command is already defined in the repository's package.json, then CI can be implemented without any more configuration.

In a CI tool, like CircleCI or Travis, the repository can be searched for after logging in or authentication. From there, follow the CI tool's UI to start testing.

For JavaScript, CircleCI will look at test within a repository's package.json to see if a valid test script is added. If it is, then CircleCI will begin running CI automatically！ Read more about setting up CircleCI automatically here.

Advanced configurations
If unit tests are unfinished, or if a more configuration is needed, a .yml file can be added for a CI tool (like CircleCI) where the execute runner scripts are made.

Below is how to set up a custom CircleCI configuration with JavaScript linting (again, using ESlint as an example) for a CircleCI.

First off, run this command:

mkdir .circleci && touch .circleci/config.yml
Then add the following to generated file:

defaults: &defaults
  working_directory: ~/code
  docker:
    - image: circleci/node:10
  environment:
  NPM_CONFIG_LOGLEVEL: error # make npm commands less noisy
  JOBS: max <h3>https://gist.github.com/ralphtheninja/f7c45bdee00784b41fed
    version: 2
    jobs:
    build:
      <<: *defaults
      steps:
        - checkout
        - run: npm i
        - run: npm run eslint:ci

After these steps are completed and after CircleCI has been configured in GitHub (more on that here), CircleCI will pick up .circleci/config.yml and lint JavaScript in a CI process when a pull request is submitted.

I created a folder with examples in this demo repository to show ideas for configuring CI with config.yml filesand you can reference it for your own project or use the files as a starting point.

The are more even more CI tools that can be setup to help save developers more time, like auto-merging, auto-updating, monitoring, and much more！

### Summary

We covered a lot here！ To sum things up, setting up CI is very doable and can even be free of cost. With additional tooling (both paid and open source), we can have more time to code, and more time to write more tests for CI — or enjoy more life away from the screen！

Here are some demo repositories to help developers get setup fast or learn. Please feel free to reach out within the repositories with questions, ideas or improvements.