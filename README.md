# Midnight's GitHub Actions

The base level repository for storing all my GitHub actions!

## Table of Contents

1. [Roadmap](#roadmap)
2. [GitHub Actions](#github-actions)
3. [Troubleshooting & Best Practices](#troubleshooting--best-practices)
4. [Actions: CI, CD, and releases](#actions-ci-cd-and-releases)

## Roadmap

- [ ] Automatically add README.md, CONTRIBUTING.md, and LICENSE files to new repo
- [ ] Automated releases when PR/MR is merged into `main` or `master`
- [ ] General purpose actions
  - [ ] Build action (`pip`/`poetry`)
  - [ ] Linting action (`pylint`)
  - [ ] Formatting action (`black`)
  - [ ] Unit test action (`poethepoet`, `pytest`, `coverage`)

## GitHub Actions

**Events** *trigger* **Workflows** which *use* **Actions**.

### Events

- GitHub triggered events:
  - `push`
  - `pull_request`
  - `public`
- Scheduled events: `schedule`
- Manually triggered: `workflow_dispatch` (external systems)
  - Best to add these to a workflow during the development process for debugging purposes.

### Workflows

- Workflows are like pipelines (e.g. Jenkins)
- Codify useful, customized processes
- `.yaml` syntax
- `.github/workflows` folder
- Workflows files glue together existing actions in sequence
  - Listen for particular events
  - Then run pre-existing actions
  - Or shell scripts
- Actions run in VMs
  - Linux, Win, or Mac
  - Or Docker on Linux VM
- Logs streaming & artifacts
- Secret store with each repository or organization
- Can reference actions from anywhere on GitHub
  - Example: `actions/setup-node@v1`
    - `actions` is the *user* or *organization*
    - `setup-node` is the *repo*
    - `v1` is a reference to the *release* version (can also reference tags)

### Actions

- Reusable units of code
  - `action.yml`
- Referencing vs authoring actions
- Administrative features
- Storing shared Actions
- Post your actions to the GitHub Marketplace
- Automatic linting of YAML actions file when using the GitHub Actions console
- Multiple jobs in a single action will run *in parallel*
- Reference other jobs in the same action as a *dependency*
  - For example, if you have a `test` job that can't run unless the `build` job is completed, you can add: `needs: [build]` to the `test` job to ensure it won't run until `build` is completed.

Types of Actions

| | **JavaScript action** | **Container action** |
| --- | --- | --- |
| **Virtual environment** | Linux, MacOS, Windows | Linux |
| **Language** | Anything that compiles to JavaScript | Any |
| **Speed** | ++ | + |
| **User experience** | ++ | + |

[GitHub Marketplace](https://github.com/marketplace?type=actions) where you can look for actions.

#### Monolithic action example

An example of a **big** action does the following:

- In one step:
  - Check out the code
  - Build and test
  - Release

However, since there are multiple actions in a single `step` it's difficult to troubleshoot/debug and lacks fine-tuned control for each action.

#### Chainable action example

- Create draft release
- Generate release notes
- Edit release, add notes

Each action is broken up into doing one specific thing and the behavior of each action can be tweaked more easily.

#### Starter workflows

- GitHub provided starter workflows
- Preconfigured for specific languages and frameworks
- Suggested workflows based on language and framework
- Create your own workflows

#### Nesting

There are three layers of nesting for a GitHub Action:

- Steps (tasks)
- Jobs
- Stages
  - In GitHub Actions, there is no such thing as "stages"

However, in GitHub, a "job" is roughly equivalent to a "stage". Jobs can be combined with other jobs, run in parallel, or serial, etc. This is a little different than other CI systems.

## Troubleshooting & Best Practices

### Troubleshooting Tools

- Workflow editor
- Action-debugging (setting **Secrets** that are used specifically by your actions for the purpose of debugging)
  - For example:
    - `ACTIONS_STEP_DEBUG=true`
      - Enables you to see log messages for at the step level
      - Gives you more information on what that action/step is doing
    - `ACTIONS_RUNNER_DEBUG=true`
      - Enables you to see the logs at the runner level
  - Remember to disable these when you're done debugging!
- VS Code extension
- Try locally: `nektos/act` project on GitHub
  - Allows you to run a specific action workflow on your local machine
  - Not very simple, should be last resort

### Best Practices

- Versioning
- Documentation
- Unit test coverage
- Maintain metadata
- Post to the marketplace

## Actions: CI, CD, and releases

### CI/CD Workflow with GitHub

#### A CI workflow [example]

- Single job with 4 steps
- OS the job runs on (Ubuntu, macOS, Windows)
- Strategy how job is executed (matrix build, etc.)
- Actions are composable separately
  - Checkout is separate
  - Setup for most languages in `github.com/actions`
  - `npm run` by shell
  - Artifact uploaded separately (to `public/`)

#### Super linter

- Ready to use for most languages
- `checkout` action
- `super-linter` container

```yaml
    - uses: actions/checkout@v2

    - name: Lint Code Base
      uses: docker://github/super-linter:v3
      env:
        VALIDATE_ALL_CODEBASE: false
        DEFAULT_BRANCH: master
```

### CD & Release Management

#### A CD workflow [example]

- GitHub Actions is a fully fledged CD

```yaml
  Build-Docker-Image:
    - name: create image and store image
      uses: thatsmidnight/actions/deploy_image@v1
      with:
        repo-token: ${{secrest.GITHUB_TOKEN}}
        image-name: ${{env.DOCKER_IMAGE_NAME}}

  Deploy-to-Azure:
    runs-on: ubuntu-latest
    needs: Build-Docker-Image
    name: Deploy app container to Azure
    steps:
      - name: "Login via Azure CLI"
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - uses: azure/docker-login@v1
        with:
            login-server: ${{env.IMAGE_REGISTRY_URL}}
            username: ${{ github.actor }}
            password: ${{ secrets.GITHUB_TOKEN }}

      - name: Deploy web app container
        uses: azure/webapps-container-deploy@v1
        with:
          app-name: $${{env.AZURE_WEBAPP_NAME}}
          images: ${{env.IMAGE_REGISTRY_URL}}/${{ github.actor }}
```
