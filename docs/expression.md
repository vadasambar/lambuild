# Expression

`lambuild` uses the Expression Engine [antonmedv/expr](https://github.com/antonmedv/expr) to filter events and generate the buildspec dynamically.

The following parameters are passed to expressions.

.path | type | example | description
--- | --- | --- | ---
.event | [Event](#type-event) | |
.repo | [Repository](#type-repository) | |
.sha | string | |
.ref | string | |
.aws.Region | string | `us-east-1` |
.aws.AccountID | string | |
.aws.CodeBuild.ProjectName | string | |
.getCommit | `func() *github.Commit` | |
.getCommitMessage | `func() string` | |
.getPR | `func() *github.PullRequest` | | get an associated pull request
.getPRNumber | `func() int` | | get an associated pull request number
.getPRFiles | `func() []*github.CommitFile` | | get associated pull request files
.getPRFileNames | `func() []string` | | get associated pull request file paths
.getPRLabelNames | `func() []string` | | get associated pull request label names

Please see [go-github's document](https://pkg.go.dev/github.com/google/go-github/v35/github) too.

* [*github.Commit](https://pkg.go.dev/github.com/google/go-github/v35/github#Commit)
* [*github.PullRequest](https://pkg.go.dev/github.com/google/go-github/v35/github#PullRequest)
* [*github.CommitFile](https://pkg.go.dev/github.com/google/go-github/v35/github#CommitFile)

_Note that the above go-github version may be different from actual version which lambuild uses, because it is difficult to maintain the document properly. Please check [go.mod](../go.mod)._

To avoid unneeded GitHub API call, `lambuild` doesn't call API until the function is called at the expression, and the result is cached in the Lambda Function's request scope.

This is the reason why the type of parameters like `getPRFileNames` is function.

## Type: Event

.path | type | example | description
--- | --- | --- | ---
.Body | string | | GitHub Webhook's payload
.Headers | [Headers](#type-headers) | | [GitHub Webhook's Delivery headers](https://docs.github.com/en/developers/webhooks-and-events/webhook-events-and-payloads#delivery-headers)
.Payload | interface{} | | [*github.PullRequestEvent](https://pkg.go.dev/github.com/google/go-github/v35/github#PullRequestEvent) or [*github.PushEvent](https://pkg.go.dev/github.com/google/go-github/v35/github#PushEvent)

## Type: Headers

[GitHub Webhook's Delivery headers](https://docs.github.com/en/developers/webhooks-and-events/webhook-events-and-payloads#delivery-headers)

.path | type | example | description
--- | --- | --- | ---
.Event | string | `push` | `x-github-event`
.Delivery | string | | `x-github-delivery`
.Signature | string | | `x-hub-signature-256`

## Type: Repository

.path | type | example | description
--- | --- | --- | ---
.FullName | string | `suzuki-shunsuke/test-lambuild` |
.Owner | string | `suzuki-shunsuke` |
.Name | string | `test-lambuild` |
