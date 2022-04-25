# Code Repository Guidelines

This document aim to describe our general coding standards at Doberman.

## Code filosophy

As we have Daniel Renaissance Man Hägglund in our team, we do also follow the [12 Factor app](https://12factor.net/) filosophy, to various extent.

### [Environment variables](https://12factor.net/config)

Yes, we do all have secrets. And most hosting providers offers good interfaces to manage them too.

We often use `dotenv` for this, avaliable for multiple languages, here's the link to the [npm](https://www.npmjs.com/package/dotenv) module

It's highly recommended to have a `.env.template` in the repository to indicate to your fellow and future colleagues what env variables they should go get

```sh
# .env.template

CONTENTFUL_ACCESS_TOKEN=
CONTENTFUL_SPACE_ID=
```

## Code management

We're using Github as our primary repository host, for client work and internal work.

Internal work are often prefixed `dbrmn-` to indicate this.

We use tags for repos to enable better discovery between projects.

Not all projects are hosted within our organisation, some projects are migrated to client org before leaving the projects, some are hosted on a client provided organisation or service from start.

## Git workflow

Our filosophy is based on the gitflow model:

![gitflow model](https://miro.medium.com/max/2800/1*9yJY7fyscWFUVRqnx0BM6A.png)

- **Feature branch** every feature branch is based on `dev` branch, and merge to `dev` when approved
- **dev** is the staging branch
- **main** is the production branch

#### Releases are either done

1.  directly from `dev` to `main`

2.  Using tags

    - create a tag on `dev` from where you want to deploy
    - merge the tag into `main`
    - deploy `main` when ready

3.  Creating a release branch

    - create a branch from `dev` from where you want to deploy
    - merge the branch into `main`
    - deploy `main` when ready

### Git Pull request work flow

We encourage you to use the review tool to keep the feedback written and close to the code, with that said, longer discussions doesn't need to create a mini-twitter-metaverse of course, that's probably a Slack call with screen sharing or something else.

#### Usually we follow this:

- Author creates the pull request, highly recommended to use the description field to inform your reviewer of the scope of work.

- Add any adjacent tasks, like deploy tasks, env keys to be configured or similar to the description

- Add comments in the PR if there's specific things you want to have input on like, an implementation you're not too happy about, something you're worried about or happy with!

- Add a (preferably just one) reviewer to the PR on Github

- Reviewer will review the code, add any comments, hoorays or objections to the review.

- Once the PR is `Approved` author gets the noble ask to merge it!

### Testing and linting

As we do often work with different setups, the set up varies.

As a foundation for JS/TS:

- We use prettier as a standard. Because it's doing it's job very nicely. And a `.prettierrc.js` in every repo to be explicit about it.

- For TypeScript, VSCode is having a really good support for linting and highlighting error while coding.

- Husky pre-commit hook is very convenient

<details>
<summary>Click to expand Husky examples</summary>

```sh
# .husky/pre-commit

#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged

```

```sh
# .husky/\_/husky.sh

#!/bin/sh
if [ -z "$husky_skip_init" ]; then
  debug () {
    if [ "$HUSKY_DEBUG" = "1" ]; then
      echo "husky (debug) - $1"
    fi
  }

  readonly hook_name="$(basename "$0")"
  debug "starting $hook_name..."

  if [ "$HUSKY" = "0" ]; then
    debug "HUSKY env variable is set to 0, skipping hook"
    exit 0
  fi

  if [ -f ~/.huskyrc ]; then
    debug "sourcing ~/.huskyrc"
    . ~/.huskyrc
  fi

  export readonly husky_skip_init=1
  sh -e "$0" "$@"
  exitCode="$?"

  if [ $exitCode != 0 ]; then
    echo "husky - $hook_name hook exited with code $exitCode (error)"
  fi

  exit $exitCode
fi

```

```sh
# .husky/_/.gitignore

*
```

</details>
