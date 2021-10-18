# releaser-action

GitHub Action for licence scanner (JS).

_This README is intended for Turku.py-meetup Hacktoberfest 2021 workshop instructions. Unfortunately we do not yet take other PRs than the ones created in the workshop._

## Purpose

This action is intended for projects that are using the development-main-branching model. The idea of the two branches is that all of the pull requests are first merged to the development branch. The development branch may have a more extensive test suite than single feature branches. If the test suite succeeds for the development branch, the package can be released, and new code can be merged to the main branch.

The action aims to release the package (create tag to the repository) if two conditions are met:

1. There are such changes in the development branch that are missing from the main branch.
1. The acceptance tests for the development branch are passing.

Depending on the project, the release may be done on the fly, which means the release will take place after each PR is merged to the development branch. Or the action user may choose to use a schedule (weekly, nightly, etc.), which may add multiple PRs to the new release.

## Step-by-step Instructions

The plan is to create the GitHub action to this repository and then use it from another repositories: [hello-world-go](https://github.com/lauravuo/hello-world-go) and/or [hello-react-ts](https://github.com/lauravuo/hello-react-ts). We will improve the functionality incrementally, and test the changes along the way. Let's release a new version of the action each time a PR is merged so that changes can be tested from the test project.

1. **Fork this repository and create a new feature-branch**

1. **Create a skeleton for [composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) to your feature-branch.** You can use the placeholder texts from the instructions for now:

   1. [Create an action metadata file](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action#creating-an-action-metadata-file)
   1. Commit and push the changes.
   1. Create PR and ask the maintainer to release a new version of the action once the PR is merged.

   You should have added following files to the root of this repository:

   ```bash
   action.yml
   ```

1. **Fork the test repository [hello-react-ts](https://github.com/lauravuo/hello-react-ts) and create a new feature-branch**

1. **[Add new job](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action#testing-out-your-action-in-a-workflow) to the hello-react-ts project [do-release workflow](https://github.com/lauravuo/hello-react-ts/blob/dev/.github/workflows/do-release.yml) where you test the new GitHub Action you created in step 2.**
   1. Edit the file `do-release.yml`accordingly
   1. Commit and push changes.
   1. Create PR for the change.
   1. Once the PR is merged, see the logs for your added job in Actions tab and check does the job succeed. (_Note: if the maintainer has not released yet the action, it cannot be run._)
1. **Add actual releaser functionality.**
   1. Create new feature-branch for your _releaser-action_-fork.
   1. Copy content from [this gist](https://gist.github.com/lauravuo/4699716d47fcc858288f365582412c75):
      - Replace contents of `action.yml`
      - Create new file `release.sh`. Remember to make the script executable: `chmod a+x ./release.sh`.
   1. Create PR and ask the maintainer to release a new version of the action once the PR is merged.
1. **Update the action version in the test project**
   1. Create new feature-branch for your _hello-react-ts_-fork.
   1. Edit the new job that was added in step 4 so that it uses the latest version of the releaser action. Commit and push changes.
   1. Create PR.
   1. Once the PR is merged, see the logs for your added job in Actions tab and check does the job succeed. If script can successfully create a new tag to the repository, it should also trigger the on-release-workflow and eventually merge the dev branch to main.
1. **Add the releaser action to test project [hello-world-go](https://github.com/lauravuo/hello-world-go)**
   1. Add releaser action to another test project similarly as you did with hello-world-ts.
1. **Continue with further enhancements to the releaser action**:
   - Check [the action metadata](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions). Does it need enhancements?
   - Update README with usage instructions
   - Make branch names configurable, so that the action user could define the names of main and development branches.
   - How could we add a check to the release script, that the tag would be created only when a defined development branch test workflow is succeeded? Hint: check the use of BADGE-variable in the script.
   - How about adding some GitHub actions to this project? What kind of automated checks/tests would be beneficial for this GitHub Action project?
