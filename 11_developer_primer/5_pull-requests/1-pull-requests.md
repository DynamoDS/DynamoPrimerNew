# Pull Requests

Dynamo depends on the creativity and commitment of its community, and the Dynamo team encourages contributors to explore possibilities, test ideas, and engage the community for feedback. While innovation is highly encouraged, changes will only be merged if they make Dynamo easier to use and satisfy the guidelines defined in this document. Changes with narrowly-defined benefits will not be merged.

#### Pull request expectations <a href="#pull-request-expectations" id="pull-request-expectations"></a>

The Dynamo team expects pull requests to follow a few guidelines:

* Adhere to our [coding standards](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards) and [node naming standards](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards)
* Include unit tests when adding new features
* When fixing a bug, add a unit test that highlights how the current behavior is broken
* Keep the discussion focused on one issue. Create a new issue if a new or related topic comes up.

And a few guidelines on what not to do:

* Surprising us with big pull requests. Instead, file an issue and start a discussion so we can agree on a direction before you invest a large amount of time.
* Commit code that you didn't write. If you find code that you think is a good fit to add to Dynamo, file an issue and start a discussion before proceeding.
* Submit PRs that alter licensing-related files or headers. If you believe there's a problem with them, file an issue and we'll be happy to discuss it.
* Make API additions without filing an issue and discussing it with us first.

#### Filling out the pull request template <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

When submitting a pull request, please use the [default PR template](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md). Before submitting your PR, ensure that the purpose is clearly described and all of the declarations can be claimed as true:

* The codebase is in a better state after this PR
* Is documented according to the [standards](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)
* The level of testing this PR includes is appropriate
* User facing strings, if any, are extracted into `*.resx`files
* All tests pass using the self-service CI
* Snapshot of UI changes, if any
* Changes to the API follow [Semantic Versioning](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions), and are documented in the [API Changes](https://github.com/DynamoDS/Dynamo/wiki/API-Changes) document.

An appropriate reviewer will be assigned to your pull request by the Dynamo Team.

#### Pull request review process <a href="#pull-request-review-process" id="pull-request-review-process"></a>

Once a pull request is submitted, you may need to stay involved during the review process. Please be aware of the following review criteria:

* The Dynamo team meets once per month to review pull requests from oldest to newest.
* If a reviewed pull request requires changes by the owner, the owner of the PR has 30 days to respond. If the PR has seen no activity by the next session, it will be either closed by the team or depending on its utility will be taken over by someone on the team.
* Pull requests should use Dynamo's default PR template
* Pull requests that do not have the Dynamo PR templates completely filled out with all declarations satisfied will not be reviewed.

#### Cherry-picking Dynamo Revit commits <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

Since there are multiple versions of Revit available on the market, you may be required to cherry-pick your changes into specific DynamoRevit Release branches so that different versions of Revit can pick up the new functionality. During the review process, contributors will be responsible for cherry-picking their reviewed commits to the other DynamoRevit branches as specified by the Dynamo team.
