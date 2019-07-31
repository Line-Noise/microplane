# microplane

A CLI tool to make git changes across many repos, especially useful with Microservices.
You can learn more about microplane in this [introductory blogpost](https://medium.com/always-a-student/mo-repos-mo-problems-how-we-make-changes-across-many-git-repositories-293ad7d418f0).

![microplane](https://cdn.pixabay.com/photo/2013/07/12/14/16/lemon-148119_640.png)

_"the lemon is Git{Hub,Lab}"_

## Setup

### Download

You can download a pre-built version of Microplane from the [Github releases](https://github.com/Clever/microplane/releases).

### Build Locally

Microplane is a Golang project. It uses [`dep`](https://github.com/golang/dep) for vendoring.

First, clone the repo into your `GOPATH`. Next, run `make install_deps` (this calls `dep`). To build, run `make build`. You should now have a working build of Microplane in `./bin/mp`.

## Usage

### GitHub setup

The `GITHUB_API_TOKEN` environment variable must be set for Github. This should be a [GitHub Token](https://github.com/settings/tokens) with `repo` scope.

Optionally: The `GITHUB_URL` environment variable can be set to use a Github Enterprise setup, otherwise it will use https://github.com.
The `GITHUB_URL` **must** be a valid URL for the API endpoint including a trailing slash: e.g. `https://git.yourcompany.com/api/v3/`

### GitLab setup

The `GITLAB_API_TOKEN` environment variable must be set for Gitlab. This should be a [GitLab access token](https://gitlab.com/profile/personal_access_tokens)

Optionally: The `GITLAB_URL` environment variable can be set to use a Gitlab on-premise setup, otherwise it will use https://gitlab.com.

### Using Microplane

Microplane has an opinionated workflow for how you should manage git changes across many repos.
To make a change, use the following series of commands.

1. [Init](docs/mp_init.md) - target the repos you want to change using a [Github "Advanced Search" Query](https://github.com/search/advanced)
2. [Clone](docs/mp_clone.md) - clone the repos you just targeted
3. [Plan](docs/mp_plan.md) - run a script against each of the repos and preview the diff
4. [Push](docs/mp_push.md) - commit, push, and open a Pull Request
5. [Merge](docs/mp_merge.md) - merge the PRs

For an in-depth example, check out the [introductory blogpost](https://medium.com/always-a-student/mo-repos-mo-problems-how-we-make-changes-across-many-git-repositories-293ad7d418f0).

## Implementation

Microplane parallelizes various git commands and API calls.

At each step in the Microplane workflow, a repo only moves forward if the previous step for that repo was successful.

We persist the progress of a Microplane run in the following local file structure.

```
mp/
  init.json
  repo1/
    clone/
      clone.json
      <git-repo>
    plan/
      plan.json
      <git-repo-with-commit>
    push/
      push.json
    merge/
      merge.json
  repo2/
    ...
```
