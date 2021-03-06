
# Project Guidelines
> While developing a new project is like rolling on a green field for you, maintaining it is a potential dark twisted nightmare for someone else.
Here's a list of guidelines we've found, written and gathered that (we think) works really well with most JavaScript projects here at [hive](http://wearehive.co.uk).
If you want to share a best practice, or think one of these guidelines  should be removed, [feel free to share it with us](http://makeapullrequest.com).
- [Git](#git)
- [Documentation](#documentation)
- [Environments](#environments)
- [Dependencies](#dependencies)
- [Testing](#testing)
- [Structure and Naming](#structure-and-naming)
- [Code style](#code-style)
- [Logging](#logging)
- [API Design](#api-design)
- [Licensing](#licensing)

## 1. Git
### 1.1 Some Git Rules
There are a set of rules to keep in mind:
* Perform work in a feature branch.
    
    _why:_
    >Because this way all work is done in isolation on a dedicated branch rather than the main branch. It allows you to submit multiple pull requests without confusion. You can iterate without polluting the master branch with potentially unstable, unfinished code. [read more...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
* Branch out from `develop`
    
    _why:_
    >This way, you can make sure that code in master will almost always build without problems, and can be mostly used directly for releases (this might be overkill for some projects).

* Never push into `develop` or `master` branch. Make a Pull Request.
    
    _why:_
    > It notifies team members that they have completed a feature. It also enables easy peer-review of the code and dedicates forum for discussing the proposed feature

* Update your local `develop` branch and do a interactive rebase before pushing your feature and making a Pull Request

    _why:_
    > Rebasing will merge in the requested branch (`master` or `develop`) and apply the commits that you have made locally to the top of the history without creating a merge commit (assuming there were no conflicts). Resulting in a nice and clean history. [read more ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

* Resolve potential conflicts while rebasing and before making a Pull Request
* Delete local and remote feature branches after merging.
    
    _why:_
    > It will clutter up your list of branches with dead branches.It insures you only ever merge the branch back into (`master` or `develop`) once. Feature branches should only exist while the work is still in progress.

* Before making a Pull Request, make sure your feature branch builds successfully and passes all tests (including code style checks).
    
    _why:_
    > You are about to add your code to a stable branch. If your feature-branch tests fail, there is a high chance that your destination branch build will fail too. Additionaly you need to apply code style check before making a Pull Request. It aids readability and reduces the chance of formatting fixes being mingled in with actual changes.

* Use [this .gitignore file](./.gitignore).
    
    _why:_
    > It already has a list of system files that should not be sent with your code into remote repository. In addition, it excludes setting folders and files for mostly used editors, as well as most common dependency folders.

* Protect your `develop` and `master` branch .
  
    _why:_
    > It protects your production-ready branches from reciving unexpected and irreversible changes. read more... [Github](https://help.github.com/articles/about-protected-branches/) and [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html)

### 1.2 Git Workflow
Because of most of the reasons above, we use [Feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) with [Interactive Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) and some elements of [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (naming and having a develop branch). The main steps are as follow:

* Checkout a new feature/bug-fix branch
    ```sh
    git checkout -b <branchname>
    ```
* Make Changes
    ```sh
    git add
    git commit -a
    ```
    _why:_
    > `git commit -a` will start an editor which lets your separate the subject from the body. Read more about it in *section 1.3*.

* Sync with remote to get changes you’ve missed
    ```sh
    git checkout develop
    git pull
    ```
    
    _why:_
    > This will give you a chance to deal with conflicts on your machine while rebasing(later) rather than creating a Pull Request that contains conflicts.
    
* Update your feature branch with latest changes from develop by interactive rebase
    ```sh
    git checkout <branchname>
    git rebase -i --autosquash develop
    ```
    
    _why:_
    > You can use --autosquash to squash all your commits to a single commit. Nobody wants many commits for a single feature in develop branch [read more...](https://robots.thoughtbot.com/autosquashing-git-commits)
    
* If you don’t have conflict skip this step. If you have conflicts, [resolve them](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)  and continue rebase
    ```sh
	git add <file1> <file2> ...
    git rebase --continue
    ```
* Push your branch. Rebase will change history, so you'll have to use `-f` to force changes into the remote branch. If someone else is working on your branch, use the less destructive `--force-with-lease`.
    ```sh
    git push -f
    ```
    
    _why:_
    > When you do a rebase, you are changing the history on your feature branch. As a result, Git will reject normal `git push`. Instead, you'll need to use the -f or --force flag. [read more...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
    
    
* Make a Pull Request.
* Pull request will be accepted, merged and close by reviewer.
* Remove your local feature branch if you're done.

### 1.3 Writing good commit messages

Having a good guideline for creating commits and sticking to it makes working with Git and collaborating with others a lot easier. Here are some rules of thumb ([source](https://chris.beams.io/posts/git-commit/#seven-rules)):

 * **Separate the subject from the body with a newline between the two**

    _why:_
    > Having a `body` section lets you explain the context
that's useful for a code reviewer. if you can link to an associated Jira ticket, GitHub issue, Basecamp to-do, etc. Also most desktop Git clients have clear separation between message line and body in their GUI.

 * Limit the subject line to 50 characters
 * Capitalize the subject line
 * Do not end the subject line with a period
 * Use [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) in the subject line
 * Wrap the body at 72 characters
 * Use the body to explain **what** and **why** as opposed to **how**

## 2. Documentation 
* Use this [template](./README.sample.md) for `README.md`, Feel free to add uncovered sections.
* For projects with more than one repository, provide links to them in their respective `README.md` files.
* Keep `README.md` updated as project evolves.
* Comment your code. Try to make it as clear as possible what you are intending with each major section.
* If there is an open discussion on github or stackoverflow about the code or approach you're using, include the link in your comment, 
* Don't use commenting as an excuse for a bad code. Keep your code clean.
* Don't use clean code as an excuse to not comment at all.
* Keep comments relevant as your code evolves.

## 3. Environments
* Depending on project size, define separate `development`, `test` and `production` environments.

    _why:_
    > Different data, tokens, APIs, ports etc... may be needed on different environments. You may want an isolated `development` mode that calls fake API which returns predictable data, making both automated and manually testing much easier. Or you may want to enable Google Analytics only on `production` and so on. [read more...](https://stackoverflow.com/questions/8332333/node-js-setting-up-environment-specific-configs-to-be-used-with-everyauth)


* Load your deployment specific configurations from environment variables and never add them to the codebase as constants, [look at this sample](./samples/config.sample.js).

    _why:_
    > You have tokens, passwords and other valuable information in there. Your config should be correctly separated from the app internals as if the codebase could be made public at any moment.

    _How:_
    >Use `.env` files to store your variables and add them to `.gitignore` to be excluded. Instead commit a `.env.example`  which serves as a guide for developers. For production you should still set your environment variables in the standard way.
    [read more](https://medium.com/@rafaelvidaurre/managing-environment-variables-in-node-js-2cb45a55195f)

* It’s recommended to validate environment variables before your app starts.  [Look at this sample](./samples/configWithTest.sample.js) using `joi` to validate provided values.
    
    _why:_
    > It may save others from hours of troubleshooting.

### 3.1 Consistent dev environments:
* Set your node version in `engines` in `package.json`
    
    _why:_
    > It lets others know the version of node the project works on. [read more...](https://docs.npmjs.com/files/package.json#engines)

* Additionally, use `nvm` and create a  `.nvmrc`  in your project root. Don't forget to mention it in the documentation

    _why:_
    > Any one who uses `nvm` can simply use `nvm use` to switch to the suitable node version. [read more...](https://github.com/creationix/nvm)

* You can also use a `preinstall` script that checks node and npm versions

    _why:_
    > Some dependencies may fail when used by newer versions of node.
    
* Use Docker images provided it doesn't make things more complicated

    _why:_
    > It can give you a consistent environment across the entire workflow. Without much neeed to fiddle with libs, dependencies or configs. [read more...](https://hackernoon.com/how-to-dockerize-a-node-js-application-4fbab45a0c19)

* Use local modules instead of using globally installed modules

    _why:_
    > Lets you share your tooling with your colleague instead of expecting them to have it on their systems.

## 4. Dependencies 
Before using a package, check its GitHub. Look for the number of open issues, daily downloads and number of contributors as well as the date the package was last updated.

* If less known dependency is needed, discuss it with the team before using it.
* Keep track of your currently available packages: e.g., `npm ls --depth=0`. [read more...](https://docs.npmjs.com/cli/ls)
* See if any of your packages have become unused or irrelevant: `depcheck`. [read more...](https://www.npmjs.com/package/depcheck)
    
    _why:_
    > The is a potential risk that you import this unused library in your bundle and ship it for production and as the result increase your bundle size. Find them, get rid of them.

* Check download statistics to see if the dependency is heavily used by the community: `npm-stat`. [read more...](https://npm-stat.com/)
    
    _why:_
    > More usage mostly means more contributors, which usually means better maintenance which results in bugs get discovered and fixed way quicker.

* Check to see if the dependency has a good, mature version release frequency with a large number of maintainers: e.g., `npm view async`. [read more...](https://docs.npmjs.com/cli/view)

    _why:_
    > Having loads of contributors wont be as effective, if maintainers dont merge those fixes and patches quickly enough.

* Always make sure your app works with the latest versions of dependencies without breaking: `npm outdated`. [read more...](https://docs.npmjs.com/cli/outdated)

    _why:_
    > Dependency updates sometimes have breaking changes, and you should be aware of that as quick as possible. Always check their release notes. Update your dependencies one by one, that makes troubleshooting easier (If breaking changes happens). Use cool tools such as [npm-check-updates](https://github.com/tjunnone/npm-check-updates).

* Check to see if the package has known security vulnerabilities with, e.g., [Snyk](https://snyk.io/test?utm_source=risingstack_blog).


### 4.1 Consistent dependencies:

* Make sure your team member gets the exact same dependencies as you

    _why:_
    > Because you want the code to behave as expected and identical in any development machine [read more...](https://medium.com/@kentcdodds/why-semver-ranges-are-literally-the-worst-817cdcb09277)

    _how:_
    > Use `package-lock.json` on `npm@5` or higher

    _I don't have npm@5:_
    > Alternatively you can use `Yarn` and make sure to mention it in `README.md`. Your lock file and `package.json` should have the same versions after each dependency update. [read more...](https://yarnpkg.com/en/)

    _I don't like the name `Yarn`:_
    > Too bad. For older versions of `npm`, use `—save --save-exact` when installing a new dependency and create `npm-shrinkwrap.json` before publishing. [read more...](https://docs.npmjs.com/files/package-locks)


## 5. Testing 

* Have a `test` mode environment if needed.

    _why:_
    > Some believe for unit testing `development` mode and for end to end testing  `production` mode will be enough. While this is partly true, there are some exceptions. One example is you may not want to enable analytical information on a 'production' mode and pollute someone's dashboard with test data. Or your API may apply rate limits in `production` and block your many test calls. 

* Place your test files next to the tested modules using `*.test.js` or `*.spec.js` naming convention, like `moduleName.spec.js`

    _why:_
    > You don't want to dig through a folder structure every time you have to find a unit test. Additionally, this naming convention is standard now and gets picked up by most JavaScript testing frameworks.
    

* Put your additional test files into a separate test folder to avoid confusion.

    _why:_
    > Some test files don't particularly relate to any specific implementation file. You have to put it in a folder that is most likely to be found by other developers: `__test__` folder. This name: `__test__`  is also standard now and gets picked up by most JavaScript testing frameworks.

* Write testable code, avoid side effects, extract side effects, write pure functions

    _why:_
    > You want to test a business logic as a separate units. You have to "minimize the impact of randomness and non-deterministic processes on the reliability of your code". [read more...](https://medium.com/javascript-scene/tdd-the-rite-way-53c9b46f45e3)


* Don’t write tests to check types, instead use a static type checker

    _why:_
    > Sometimes you may need a Static type checker. It brings a certain level of reliability to your code. [read more...](https://medium.freecodecamp.org/why-use-static-types-in-javascript-part-1-8382da1e0adb)


* Run tests locally before making any pull requests to `develop`.

     _why:_
    > Because you don't want to be the one who caused production-ready branch build to fail.

* Document your tests, with instructions.

     _why:_
    > Other developers or DevOps experts may get desperate to know where these stuff are and how to run them.

## 6. Structure and Naming 
* Organize your files around product features / pages / components, not roles. Also, place your test files next to their implementation.



    **Bad**

    ```
    .
    ├── controllers
    |   ├── product.js
    |   └── user.js
    ├── models
    |   ├── product.js
    |   └── user.js
    ```

    **Good**

    ```
    .
    ├── product
    |   ├── index.js
    |   ├── product.js
    |   └── product.test.js
    ├── user
    |   ├── index.js
    |   ├── user.js
    |   └── user.test.js
    ```

    _why:_
    > Instead of a long list of files You will create small modules that encapsulate one responsibility including its test and so on. It gets much easier to navigate through and things can be found at a glance 
    
* Put your additional test files to a separate test folder to avoid confusion.

    _why:_
    > Other developers or DevOps experts may get desperate to know where these stuff are and how to run them.

* Use a `./config` folder and don't make different config files for different environments.

    _why:_
    >When you break down a config file for different purposes (database, API and so on); putting them in a folder with a very recoginzable name such as `config` makes sense. Just remember not to make different config files for different environments. It doesn't scale cleanly. As more deploys of the app are created, new environment names are necessary.
    Values to be used in config files should provided by environment variables. [read more...](https://medium.com/@fedorHK/no-config-b3f1171eecd5)
    

* Put your scripts in a `./scripts` folder. This includes `bash` and `node` scripts.

    _why:_
    >It's very likely you may end up with more than one script. Production build, Development build, database synchronisation and so on.
    

* Place your build output in a `./build` folder. Add `build/` to `.gitignore`.

    _why:_
    >Name it what you like, `dist` is also cool. But make sure that keep it consistent with your team. Whet gets in there is most likely generated  (bundled, compiled, transpiled) or moved there. What you can generate, your teammates should be able to generate too, so there is no point commiting them into your remote repository. 

* Use `PascalCase' 'camelCase` for filenames and directory names. Use  `PascalCase`  only for Components.

* `CheckBox/index.js` should have the `CheckBox` component, as could `CheckBox.js`, but **not** `CheckBox/CheckBox.js` or `checkbox/CheckBox.js` which are redundant.

* Ideally the directory name should match the name of the default export of `index.js`.

    _why:_
    > Then you can expect what component or module you will receive by simply just importing its parent folder.   

## 7. Code style 
* Use stage-2 and higher JavaScript (modern) syntax for new projects. For old project stay consistent with existing syntax unless you intend to modernise the project.

    _why:_
    > This is all up to you. We use transpilers to use advantages of new syntax. stage-2 is more likely to eventually become part of the spec with only minor revisions. 

* Include code style check in your build process.

    _why:_
    > Breaking your build is one way of enforcing code style to your code. It prevents you from taking it less seriously. Do it for both client and server-side code. [read more...](https://www.robinwieruch.de/react-eslint-webpack-babel/)

* Use [ESLint - Pluggable JavaScript linter](http://eslint.org/) to enforce code style.

    _Why:_
    > We simply prefer `eslint`, you don't have to. It has more number of rules supported, ability to configure the rules, and ability to add custom rules.

* We use [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) for JavaScript, [Read more](https://www.gitbook.com/book/duk/airbnb-javascript-guidelines/details). Use the javascript style guide required by the project or your team.

* We use [Flow type style check rules for ESLint.](https://github.com/gajus/eslint-plugin-flowtype) when using [FlowType](https://flow.org/).

    _Why:_
    > Flow introduces few syntaxes that also need to follow certain code style and be checked.

* Use `.eslintignore` to exclude file or folders from code style check.

    _Why:_
    > You don't have to pollute your code with `eslint-disable` comments whenever you need to exclude couple of files from style checking.

* Remove any of your `eslint` disable comments before making a Pull Request.

    _Why:_
    > It's normal to disable style check while working on a code block to focus more on the logic. Just remember to remove those `eslint-disable` comments and follow the rules.

* Always use  `//TODO:`  comments to remind yourself and others about an unfinished job.
* Always comment and keep them relevant as code changes.
* Remove commented block of code when possible.
* Avoid js alerts in production.
* Avoid irrelevant or funny comments, logs or naming (source code may get handed over to another company/client and they may not share the same banter).
* Write testable code, avoid side effect, extract side effects, write pure functions.
* Make your names search-able with meaningful distinctions avoid shortened names. For functions Use long, descriptive names. A function name should be a verb or a verb phrase, and it needs to communicate its intention.
* Organize your functions in a file according to the step-down rule. Higher level functions should be on top and lower levels below. It makes it more natural to read the source code.

## 8. Logging 
* Avoid client-side console logs in production
* Produce readable production logging. Ideally use logging libraries to be used in production mode (such as [winston](https://github.com/winstonjs/winston) or
[node-bunyan](https://github.com/trentm/node-bunyan)).

## 9 API design 
Follow resource-oriented design. This has three main factors: resources, collection, and URLs.
* A resource has data, relationships to other resources, and methods that operate against it
* A group of resources is called a collection.
* URL identifies the online location of a resource.


### 9.1 API Naming

#### 9.1.1 Naming URLs
* `/users` a collection of users (plural nouns).
* `/users/id` a resource with information about a specific user.
* A resource always should be plural in the URL. Keep verbs out of your resource URLs.
* Use verbs for non-resources. In this case, your API doesn't return any resources. Instead, you execute an operation and return the result to the client. Hence, you should use verbs instead of nouns in your URL to distinguish clearly the non-resource responses from the resource-related responses.

GET 	`/translate?text=Hallo`

#### 9.1.2 Naming fields
* The request body or response type is JSON then please follow `camelCase` to maintain the consistency.
* Expose Resources, not your database schema details. You don't have to use your `table_name` for a resource name as well. Same with resource properties, they shouldn't be the same as your column names.
* Only use nouns in your URL naming and don’t try to explain their functionality and only explain the resources (elegantly).


### 9.2 Operating on resources

#### 9.2.1 Use HTTP methods
Only use nouns in your resource URLs, avoid endpoints like `/addNewUser` or `/updateUser` .  Also avoid sending resource operations as a parameter. Instead explain the functionalities using HTTP methods:

* **GET**		Used to retrieve a representation of a resource.
* **POST**	Used to create new resources and sub-resources
* **PUT**		Used to update existing resources
* **PATCH**	Used to update existing resources.  PATCH only updates the fields that were supplied, leaving the others alone
* **DELETE**	Used to delete existing resources

### 9.3 Use sub-resources
Sub resources are used to link one resource with another, so use sub resources to represent the relation.
An API is supposed to be an interface for developers and this is a natural way to make resources explorable.
If there is a relation between resources like  employee to a company, use `id` in the URL:

* **GET**		`/schools/2/students	`	Should get the list of all students from school 2
* **GET**		`/schools/2/students/31`	Should get the details of student 31, which belongs to school 2
* **DELETE**	`/schools/2/students/31`	Should delete student 31, which belongs to school 2
* **PUT**		`/schools/2/students/31`	Should update info of student 31, Use PUT on resource-URL only, not collection
* **POST**	`/schools `					Should create a new school and return the details of the new school created. Use POST on collection-URLs

### 9.4 API Versioning
When your APIs are public for other third parties, upgrading the APIs with some breaking change would also lead to breaking the existing products or services using your APIs. Using versions in your URL can prevent that from happening:
`http://api.domain.com/v1/schools/3/students	`

### 9.5 Send feedbacks
#### 9.5.1 Errors
Response messages must be self descriptive. A good error message response might look something like this:
```json
{
"code": 1234,
"message" : "Something bad happened",
"description" : "More details"
}
```
or for validation errors:

```json
{
  "code" : 2314,
  "message" : "Validation Failed",
  "errors" : [
    {
      "code" : 1233,
      "field" : "email",
      "message" : "Invalid email"
    },
    {
       "code" : 1234,
       "field" : "password",
       "message" : "No password provided"
    }
  ]
}
```
Note: Keep security exception messages as generic as possible. For instance, Instead of saying ‘incorrect password’, you can reply back saying ‘invalid username or password’ so that we don’t unknowingly inform user that username was indeed correct and only password was incorrect.

#### 9.5.2 Align your feedback with HTTP codes.
##### The client and API worked (success – 2xx response code)  
* `200 OK` This HTTP response represents success for `GET`, `PUT` or `POST` requests.
* `201 Created` This status code should be returned whenever a new instance is created. E.g on creating a new instance, using `POST` method, should always return `201` status code.
* `204 No Content` represents the request was successfully processed, but has not returned any content. `DELETE` can be a good example of this. If there is any error, then the response code would be not be of 2xx Success Category but around 4xx Client Error category.

##### The client application behaved incorrectly (client error – 4xx response code)
* `400 Bad Request` indicates that the request by the client was not processed, as the server could not understand what the client is asking for.
* `401 Unauthorized` indicates that the request lacks valid credentials needed to access the needed resources, and the client should re-request with the required credentials.
* `403 Forbidden` indicates that the request is valid and the client is authenticated, but the client is not allowed access the page or resource for any reason.
* `404 Not Found` indicates that the requested resource was not found. 
* `406 Not Acceptable` A response matching the list of acceptable values defined in Accept-Charset and Accept-Language headers cannot be served.
* `410 Gone` indicates that the requested resource is no longer available and has been intentionally and permanently moved.

##### The API behaved incorrectly (server error – 5xx response code)
* `500 Internal Server Error` indicates that the request is valid, but the server could not fulfill it due to some unexpected condition.
* `503 Service Unavailable` indicates that the server is down or unavailable to receive and process the request. Mostly if the server is undergoing maintenance or facing a temporary overload.


### 9.6 Resource parameters and metadata
* Provide total numbers of resources in your response
* The amount of data the resource exposes should also be taken into account. The API consumer doesn't always need the full representation of a resource.Use a fields query parameter that takes a comma separated list of fields to include:
    ```
    GET /student?fields=id,name,age,class
    ```
* Pagination, filtering and sorting don’t need to be supported by default for all resources. Document those resources that offer filtering and sorting.


### 9.7 API security
#### 9.7.1 TLS
To secure your web API authentication, all authentications should use SSL. OAuth2 requires the authorization server and access token credentials to use TLS.
Switching between HTTP and HTTPS introduces security weaknesses and best practice is to use TLS by default for all communication. Throw an error for non-secure access to API URLs.

#### 9.7.2 Rate limiting
If your API is public or have high number of users, any client may be able to call your API thousands of times per hour. You should consider implementing rate limit early on.

#### 9.7.3 Input Validation
It's difficult to perform most attacks if the allowed values are limited.
* Validate required fields, field types (e.g. string, integer, boolean, etc), and format requirements. Return 400 Bad Request with details about any errors from bad or missing data.

* Escape parameters that will become part of the SQL statement to protect from SQL injection attacks

* As also mentioned before, don't expose your database scheme when naming your resources and defining your responses

#### 9.7.4 URL validations
Attackers can tamper with any part of an HTTP request, including the URL, query string,

#### 9.7.5 Validate incoming content-types.
The server should never assume the Content-Type. A lack of Content-Type header or an unexpected Content-Type header should result in the server rejecting the content with a `406` Not Acceptable response.

#### 9.7.6 JSON encoding
A key concern with JSON encoders is preventing arbitrary JavaScript remote code execution within the browser or node.js, on the server. Use a JSON serialiser to entered data to prevent the execution of user input on the browser/server.


### 9.8 API documentation
* Fill the `API Reference` section in [README.md template](./README.sample.md) for API.
* Describe API authentication methods with a code sample
* Explaining The URL Structure (path only, no root URL) including The request type (Method)

For each endpoint explain:
* URL Params If URL Params exist, specify them in accordance with name mentioned in URL section
```
Required: id=[integer]
Optional: photo_id=[alphanumeric]
```
* If the request type is POST, provide a working examples. URL Params rules apply here too. Separate the section into Optional and Required.
* Success Response, What should be the status code and is there any return data? This is useful when people need to know what their callbacks should expect!
    ```
    Code: 200
    Content: { id : 12 }
    ```
* Error Response, Most endpoints have many ways to fail. From unauthorised access, to wrongful parameters etc. All of those should be listed here. It might seem repetitive, but it helps prevent assumptions from being made. For example
    ```json
    {
        "code": 403,
        "message" : "Authentication failed",
        "description" : "Invalid username or password"
    }   
    ```


#### 9.8.1 API design tools
There are lots of open source tools for good documentation such as [API Blueprint](https://apiblueprint.org/) and [Swagger](https://swagger.io/).

## 10. Licensing 
Make sure you use resources that you have the rights to use. If you use libraries, remember to look for MIT, Apache or BSD but if you modify them, then take a look into licence details. Copyrighted images and videos may cause legal problems.


---
Sources:
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript)
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials)
