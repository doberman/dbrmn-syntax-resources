# Code Repository Guidelines

This document aim to describe our general coding standards at Doberman.

## Code philosophy

As we have Daniel Renaissance Man Hägglund in our team, we do also follow the [12 Factor app](https://12factor.net/) philosophy, to various extent.

### Readme contains full instructions on how to get started

A good rule of thumb is that any colleague should _ideally_ be able to read the readme, and from there be able to set up their own local environment. This is very very helpful when inviting new developers to a project, and can even be used as a test for the instructions.

Make sure to update it when scripts or tools changes along the project.

### Environment variables

Yes, we do all have secrets. We follow the [12 Factor app config](https://12factor.net/config) principle. And most hosting providers offers good interfaces to manage them too.

We often use `dotenv` for this, avaliable for multiple languages, here's the link to the [npm](https://www.npmjs.com/package/dotenv) module

It's highly recommended to have a `.env.template` in the repository to indicate to your fellow and future colleagues what env variables they should go get

```sh
# .env.template

CONTENTFUL_ACCESS_TOKEN=
CONTENTFUL_SPACE_ID=
```

## Code management

We're using Github as our primary repository host, for client work and internal work.

Internal work are ideally prefixed `dbrmn-` to indicate this.

We use topics for repos to enable better discovery between projects, so developers can find projects with a certain framework or tool.

<img src="https://user-images.githubusercontent.com/4181277/166698904-0419aaf4-65cc-462e-bb20-00c30c12dcc0.png" alt="Example of searching by topic" width="50%"/>

Not all projects are hosted within our organisation, some projects are migrated to client org before leaving the projects, some are hosted on a client provided organisation or service from start. When migrating a project outside of our organisation and have the legal option to, it's nice to upload a read-only copy of the repository to our organisation, so we can still use the repo as inspiration and for learning purposes.

## Git workflow

Our philosophy is based on the gitflow model, but with a twist, link to it [here](https://docs.google.com/presentation/d/1BnyasC1WQQIRx7s9ar9xoyXlHRxLdyt_PEOkr3Z53Mc/edit):

|                                          Without external stake holders                                           |                                            With external stake holders                                            |
| :---------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------: |
| ![ci model](https://user-images.githubusercontent.com/4181277/166697547-7e113d0e-f3dc-43c9-acb7-0a55e0a11676.png) | ![ci model](https://user-images.githubusercontent.com/4181277/166697568-56deb472-136b-4eca-b3b7-5447089dcbf5.png) |

- **Feature branch** every feature branch is based on `dev` branch, and merge to `dev` when approved. Usually prefixed `feaure/{name-of-the-feature}`
- **Chore branch** every feature branch is based on `dev` branch, and merge to `dev` when approved. Usually prefixed `chore/{name-of-the-chore}`.
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

- Author creates the pull request, use the description field to inform your reviewer of the scope of work.

- Add any adjacent tasks, like deploy tasks, env keys to be configured or similar to the description too.

- Add your comments in the PR if there's specific things you want to have input on like, an implementation you're not too happy about, something you're worried about or happy with!

- Add a (preferably just one) reviewer to the PR on Github

- Reviewer will review the code, add any comments, hoorays or objections to the review.

- We merge with squashing the commits, since the history still lives with the branch and pull request (at least on Github), then full history is still accessible if needed.

- Once the PR is `Approved` the reviewer gets the noble ask to merge it, unless other setup is agreed. The reasoning why the reviewer will merge, is to emphasise on the shared the responsibility of the code.

- Delete the branch, when it's merged and accepted.

### Testing and linting

As we do often work with different tech stacks, and therefore the setup varies, for good and bad.

As a foundation for JS/TS:

- We use prettier as a standard. Because it's doing it's job very nicely. And a `.prettierrc.js` in every repo to be explicit about it.

- For TypeScript, VSCode is having a really good support for linting and highlighting error while coding.

- Husky is very handy to define git hook related business, like linting pre-commit
