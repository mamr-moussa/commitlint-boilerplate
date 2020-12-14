# CommitLint Boilerplate
This project is a simple template for better development standards and environment, which will ensure that all your submitted code is linted, tested and includes expressive commit messages.

**What to expect?**

 - Validating your commit messages to follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) standards.
 - Terminate and deliver readable errors for the invalid commit messages.
 - Lint all JS files before each commit.
 - Terminate commits if the code includes syntax errors.
 - Run Unit Testing before each commit.
 - Terminate commit if the unit test doesn't pass.
 - Generate/Update CHANGELOG based on commits history.
 - Generate semantic versions based on commits types.
 - Validate the build health for each version.
 - Validate pull requests based on the build health.
 - Validate pull requests based on the commit message.
 
 **Used Packages**
 - [ESLint](https://eslint.org/) syntax linting
 - [Jest](https://jestjs.io/) testing
 - [Husky](https://typicode.github.io/husky) git hooks
 - [Lint Staged](https://www.npmjs.com/package/lint-staged) auto lint for staged files
 - [CommitLint](https://commitlint.js.org/#/) commits validating
 - [CommitLint/Conventional Config](https://commitlint.js.org/#/) conventional commits standards
 - [CommitLint/Travis CLI](https://www.npmjs.com/package/@commitlint/travis-cli/v/11.0.0) for pull requests validation by Travis
 - [Github CommitLint Actions](https://github.com/marketplace/actions/commitlint-cli) pull requests checks by Github Actions
 - [Standard Version](https://www.npmjs.com/package/standard-version) Uses commits history to generate CHANELOG and Semantic Versioning
 
 ## Get Started
 This boilerplate includes the whole setup and configurations set out of the box, so if you need to test it out, just run the following
 `npm install`
 `npm start`
But first let's find out what is this setup capable to do, and how to do so.

***1 - Linting***
We're using ESLint for linting purposed, so we added 2 scripts in our `package.json`
`npm run lint` to test linting.
`npm run lint:fix` to fix the possible lint issues.

***2 - Testing***
We're adding a little testing configuration as a proof of concept, only one script was added to our `package.json` to `npm run test` or `yarn test` which basically runs Jest testing.
But still this test results are capable to prevent your commit if it doesn't pass.

We added a little demo for `sum.js` file which only includes a sum function.
Then we added `sum.test.js` to test this function, try to give it invalid expectations and commit this code and it will not be accepted.

## Commits Basics and Pipeline
**1 - Conventional Commits**

Conventional Commits provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of.

Schema:
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```
Type:
The commit contains the following structural elements, to communicate intent to the consumers of your library:

1.  **fix:**  a commit of the  _type_  `fix`  patches a bug in your codebase (this correlates with  [`PATCH`](http://semver.org/#summary)  in semantic versioning).
2.  **feat:**  a commit of the  _type_  `feat`  introduces a new feature to the codebase (this correlates with  [`MINOR`](http://semver.org/#summary)  in semantic versioning).
3.  **BREAKING CHANGE:**  a commit that has a footer  `BREAKING CHANGE:`, or appends a  `!`  after the type/scope, introduces a breaking API change (correlating with  [`MAJOR`](http://semver.org/#summary)  in semantic versioning). A BREAKING CHANGE can be part of commits of any  _type_.
4.  _types_  other than  `fix:`  and  `feat:`  are allowed, for example  [@commitlint/config-conventional](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional)  (based on the  [the Angular convention](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)) recommends  `build:`,  `chore:`,  `ci:`,  `docs:`,  `style:`,  `refactor:`,  `perf:`,  `test:`, and others.
5.  _footers_  other than  `BREAKING CHANGE: <description>`  may be provided and follow a convention similar to  [git trailer format](https://git-scm.com/docs/git-interpret-trailers).

**Examples**
```
fix: correct minor typos in code

see the issue for details

on typos fixed.

Reviewed-by: Z
Refs [#JIRA-123](https://jira.com/jira-123)
```
OR
```
feat([#JIRA-123](https://jira.com/jira-123)): add signup form

-add a signup form
-Call the api and handle results

Reviewed-by: Z
```
A good practice is to leave the ticket number referenced by a link so you can relate the commit to your issue tracker, you can do it in Footer, Subject or even the Scope.
The first commit will look like this

fix: correct minor typos in code

see the issue for details

on typos fixed.

Reviewed-by: Z
Refs [#JIRA-123](https://jira.com/jira-123)

The second will be very close but the ticket number will appear in the CHANGELOG as they only includes commits subjects to look like this in the CHANGELOG.
**[#JIRA-123](https://jira.com/jira-123)**: add signup form

No Body or Footer commits
```
feat(UI): add signup form
```
Also scope is optional by default
```
feat: add signup form
```
BREAKING CHANGES
```
feat(authentication)!: change the response attributes
```
Breaking Changes can also be mentioned in the footer to describe it like so.
```
feat(authentication)!: change the response attributes

BREAKING CHANGE: moved token from response body to response headers
```
This will add a highlighted section in your CHANGELOG files so other teams will be take care if they are consuming this modules.

***Check our  [CHANGELOG](https://github.com/mamr-moussa/commitlint-boilerplate/blob/main/CHANGELOG.md#300-2020-12-14) to see how it looks***


**2 - Husky + Lint Staged**

- Husky is a git hooks invoker, simply it executes commands on certain hooks
**eg:**  `pre-commit`: which will be invoked before each commit, and only when this commands runs without errors it  it will move to the next trick to commit.
- Lint Staged is a linting invoker, it invokes certain files extensions using your custom rules
   - eg: in `package.json` add a new section `lint-staged`
   ```
   "lint-staged": {
    "*.js": [
      "npm run lint:fix",
      "git add"
    ]
  }
   ``` 
   So this will apply on each js file a linting process and then will add it to staged files.
 - We can use this along with Husky to lint all files before commits and also validate our linting status, so if it is not fine it will be automatically rejected.
 - This alone is capable to prevent any syntax errors or messy code to be uploaded to our repos.
	 - eg: Husky + lint-staged, in `package.json` create new section for `husky`
	 ` "husky": { "hooks": {  "pre-commit": "lint-staged" } }`
  
**3 - CommitLint + Conventional Commits**

Simply CommitLint will ensure that your commits match the previous rules, if not it will prevent messy commit messages to be sent; this will you later to generate more meaningful CHANGELOGs and versions.
However; CommitLint has options to customize your rules, they already have a nice [documentation](https://github.com/conventional-changelog/commitlint/blob/master/docs/reference-rules.md) showing how to create your custom group of rules.

CommitLint also supports plugins and their ecosystem includes many useful plugins for easier extensions and quicker configurations; such as ***jira*** plugin which validates having a JIRA ticket id, and many more.

commitLint configurations goes in `commitlint.config.js`, you can check ours in this repo as a [quick example](https://github.com/mamr-moussa/commitlint-boilerplate/blob/main/commitlint.config.js). 

##### CommitLint + Husky
CommitLint validations need to take place under a certain git hook, so once we commit it should validate the commit message, so now we can update `husky` hooks to include another new hook that will react for commitLint
**eg:**
```
"husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
      "pre-commit": "lint-staged"
    }
  }
  ```
  Now if you try to submit a messy commit message it will be rejected.
  
**4 - Jest Testing + Lint Staged**
It's very common to updated some piece of code that will affect others, and therefor unit testing and E2E testing, unlike us, this modules will not forget, so usually when it happens its testing frameworks that detects this errors.
Lint Staged and Jest together will ensure that every time you will commit, you wont be able to do it unless your unit test passes, this will reduce a great amount of issues before it comes.

**eg:**
under lint staged section in `package.json`
```
"lint-staged": {
    "*.js": [
      "npm run lint:fix",
      "jest --bail --findRelatedTests",
      "git add"
    ]
  }
```
this will add a new chain to be invoked by husky pre-commit hook, so now it will:

 1. Fix possible lint issues
 2. Debug lint issues
 3. Run your test
 4. ensures all of them are ok
 5. stage all of them
 
 
**5 - CommitLint + Travis CI**
Travis CI is a hosted continuous integration service used to build and test software projects hosted at GitHub and Bitbucket.
Having Travis in our back will:
 1. Build every release (version) and validate the build health.
 2. Validate commit messages before pull requests
 3. Validate the build health before pull requests
 
##### Install and Configure Travis + CommitLint

First we need to install `commitlint/travis-cli` as a dev dependency.
`npm i @commitlint/travis-cli -D`
Then we need to create `.travis.yml` file to tell Travis more about our project.

Check [our configurations](https://github.com/mamr-moussa/commitlint-boilerplate/blob/main/.travis.yml) here or just copy the following initially.
```
language: node_js
node_js:
  - node
script:
  - commitlint-travis
before_install:
  - npm install -g yarn --cache-min 999999999
install:
  - yarn
```
Also you will need to register to Travis and enable the access to your repo, you can watch your builds, testing, pull requests status using their web app or Github.


**6 - CommitLint + Github Actions**

If you're using Github, and you need to just check pull requests commit messages, there is an even easier way to do it using Github Actions + CommitLint

all you have to do is to create this file starting from your `root` directory
`/.github/workflows/commitlint.yml` 
then copy paste the content inside [our configurations](https://github.com/mamr-moussa/commitlint-boilerplate/blob/main/.github/workflows/commitlint.yml) and you're all set!

You can also copy the following instead
```
name: Lint Commit Messages
on: [pull_request]

jobs:
  commitlint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - uses: wagoid/commitlint-github-action@v2
```

### CommitLint, Travis, and Github in action.
[This Pull Request](https://github.com/mamr-moussa/commitlint-boilerplate/pull/4) is invalid for both Travis, and Github Actions, they perform 3 checks before telling whether it's valid or not.
and this one failed in all of them
The 3 checks are: 
1. Travis: Branch Build Health
2. Travis: CommitLint
3. Github: CommitLint

Reasons why it fails:
1. commit message does not match the conventional commit standard
2. The unit test does not pass
 
Travis also leave you a full logs of why this checks was failed

On the other hand
[This Pull Request](https://github.com/mamr-moussa/commitlint-boilerplate/pull/5) is valid for both, if you check, the commit message is following the conventional commits standards and the unit test passes.


**7 - Standard Version + Conventional Commits**

Standard Version is CLI tool you can install it globally or you can add it to your dev dependencies, if you're following the conventional commits standards which will not be optional if you're using `commitlint`; Standard Version will do the following:

1. Generate a `CHANGELOG.md` file for you on version releases based on your commits.
2. Update your package version based on your commits types.

***Install and usage:***
`npm i standard-version -D`
Then add a `release` script to your `package.json` scripts
```
"scripts": {
  ...
  "release": "standard-version"
 }
  ```
now you are all set, just write a nice commits and when it comes to a new release just use the script you've just added and push
`yarn release` or `npm run release` then push your code and enjoy the updates!


# Conclusion 
This pipeline is starting from:
1. code writing to ensure a better code quality by linting;
2. then prevents any syntax errors or messy code to be committed;
3. then it makes sure that no forgotten code are left before commits;
4. then it makes sure that every test case you wrote are satisfied before committing;
5. then it prevents any meaning less commit messaged to be delivered;
6. then it references the teams work to the issue tracker;
7. then it tests the build health for you;
8. then it validates the pull requests before you take action;
9. then they update and well document you change logs;
10. then they correctly update your version based on your work.

10 Big features for a better development environment.