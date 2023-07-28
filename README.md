# Midnight's GitHub Actions

The base level repository for storing all my GitHub actions!

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
