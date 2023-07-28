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

Types of Actions

| | **JavaScript action** | **Container action** |
| --- | --- | --- |
| **Virtual environment** | Linux, MacOS, Windows | Linux |
| **Language** | Anything that compiles to JavaScript | Any |
| **Speed** | ++ | + |
| **User experience** | ++ | + |

[GitHub Marketplace](https://github.com/marketplace?type=actions) where you can look for actions.
