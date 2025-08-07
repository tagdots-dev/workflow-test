# workflow-test

[![OpenSSF Best Practices](https://www.bestpractices.dev/projects/XXX/badge)](https://www.bestpractices.dev/projects/XXX)
[![CI](https://github.com/tagdots/delete-workflow-runs/actions/workflows/ci.yaml/badge.svg)](https://github.com/tagdots/delete-workflow-runs/actions/workflows/ci.yaml)
[![marketplace](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/tagdots/delete-workflow-runs/refs/heads/badges/badges/marketplace.json)](https://github.com/marketplace/actions/delete-workflow-runs-action)
[![coverage](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/tagdots/delete-workflow-runs/refs/heads/badges/badges/coverage.json)](https://github.com/tagdots/delete-workflow-runs/actions/workflows/cron-tasks.yaml)

<br>

### 😎 Why use delete-workflow-runs?
**delete-workflow-runs** was created because some of the _most popular_ actions for the same cause on the marketplace:
- are not regularly maintained.
- do not provide supportive information before a delete operation.
- do not identify orphan workflow runs to delete (parent workflow was deleted).

<br>

## ⭐ Why switch to delete-workflow-runs?
- we reduce your supply chain risks with `openssf best practices` in our software development and operations.
- we produce an estimate of API rate limit consumption in dry-run, allowing you to plan your delete operation properly.
- we identify orphan workflows to delete orphan workflow action runs.

<br>

## 🏃 Running _delete-workflow-runs_ on GitHub action
Please visit our GitHub action ([delete-workflow-runs-action](https://github.com/marketplace/actions/delete-workflow-runs-action)) on the GitHub Marketplace.

<br>

## 🖥 Running _delete-workflow-runs_ locally

#### Prerequisites
```
* Python (3.12+)
* GitHub fine-grained token (actions: write, contents: read)
```

<br>

#### Install _delete-workflow-runs_**
```
~/work/hello-world $ workon hello-world
(hello-world) ~/work/hello-world $ export GH_TOKEN=github_pat_xxxxxxxxxxxxx
(hello-world) ~/work/hello-world $ pip install -U delete-workflow-runs
```
<br>

#### 🔍 Example 1 - Run for help
```
(hello-world) ~/work/hello-world $ delete-workflow-runs --help
Usage: delete-workflow-runs [OPTIONS]

Options:
  --dry-run BOOLEAN   (optional) default: true
  --repo-url TEXT     e.g. https://github.com/{owner}/{repo}  [required]
  --min-runs INTEGER  (optional) min. no. of runs to keep in a workflow
  --max-days INTEGER  (optional) max. no. of days to keep the run in a workflow
  --version           Show the version and exit.
  --help              Show this message and exit.
```

<br>

#### 🔍 Example 2 - Perform a dry-run to keep 10 workflow runs for each workflow
**Summary**
- **API rate limit:** total, remaining, consumption after this dry-run, & consumption estimate without dry-run
- **Workflow runs:** all workflow runs grouped by workflow name and divided between orphan and active workflows
- **Mock Delete:** all workflow runs to be deleted

```
(hello-world) ~/work/hello-world $ delete-workflow-runs --min-runs 10 --dry-run true --repo-url https://github.com/tagdots/hello-world

🚀 Starting to Delete GitHub Action workflows (dry-run: True, min-runs: 10, max-days: None)

✅ Login successfully as: tagdots-owner

💥 Core API Rate Limit Info
API rate limit          : 5000
API rate limit remaining: 4993


💪 Gathering All Workflow Runs...
Processing data... ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00

Number of orphan workflow IDs : 5
Number of workflow runs       : 129
Number of orphan workflow runs: 30
Number of active workflow runs: 99


🔍 Orphan Workflow Runs
Number of oustanding orphan workflow run(s): 30

(MOCK TO DELETE): [15058092121, 15034888458, 15078596910, 15034475367, 15090026832, 15090030896, 15084245242, 15084303419, 15239417433, 15239382675, 15239408367, 15239397843, 15239358005, 15235079202, 15233659220,
16556825410, 16510306546, 16430838958, 16306842822, 16332759286, 16405553071, 16393954546, 16358571373, 16382491807, 16545099147, 16533675262, 16484292026, 16458028383, 16547233105, 16434758197]


🔍 Active Workflow Runs
Number of oustanding active workflow run(s): 99


🐑 Active Workflow Runs (grouped by Workflow Name)
name
cd                    19
ci                    20
cron-tasks            19
dependabot-updates    21
sidecar-pr-target     20
dtype: int64


🗑️ Deleting 9 workflow runs from cd
(MOCK TO DELETE): [15806064859, 16104512541, 16250807024, 16434583247, 16434645985, 16434754825, 16434785233, 16434820611, 16546884995]

🗑️ Deleting 10 workflow runs from ci
(MOCK TO DELETE): [15950739193, 16094600811, 16094619137, 16244732902, 16395541601, 16434567109, 16434627400, 16434740215, 16546738070, 16547528347]

🗑️ Deleting 9 workflow runs from cron-tasks
(MOCK TO DELETE): [16434793676, 16434825852, 16434852708, 16434896839, 16434934116, 16434996622, 16435041248, 16453916385, 16520026786]

🗑️ Deleting 11 workflow runs from dependabot-updates
(MOCK TO DELETE): [15950716794, 16094417348, 16094600635, 16094600796, 16244713166, 16244779653, 16395351569, 16395525528, 16546581526, 16546719181, 16547922603]

🗑️ Deleting 10 workflow runs from sidecar-pr-target
(MOCK TO DELETE): [15514064562, 15658883423, 15950739190, 16244732841, 16395541582, 16434566916, 16434627267, 16434740066, 16546738013, 16547528240]

💥 Core API Rate Limit Changes
API rate limit used     : 5
API rate limit remaining: 4988
API rate limit Reset At : 2025-08-07 21:31:26+00:00 (UTC)

************************** API Usage Estimate ******************************
This delete can consume 162 of your API limit.

Enough API limit to run this delete now? ✅ yes
****************************************************************************
```

<br>

#### 🔍 Example 3 - Delete workflow runs and keep up to the last 10 days for each workflow
**Summary**
- **API rate limit:** total, remaining, consumption after this dry-run, & consumption estimate without dry-run
- **Workflow runs:** all workflow runs grouped by workflow name and divided between orphan and active workflows
- **Mock Delete:** all workflow runs to be deleted

```
(hello-world) ~/work/hello-world $ delete-workflow-runs --max-days 10 --dry-run false --repo-url https://github.com/tagdots/hello-world


```

<br>

## 🔧 delete-workflow-runs command line options

| Input | Description | Default | Required | Notes |
|-------|-------------|----------|----------|----------|
| `repo-url` | Repository URL | `''` | Yes | e.g. https://github.com/{owner}/{repo} |
| `dry-run` | Dry-Run | `true` | No | - |
| `min-runs` | Min. no. of runs to keep in a workflow | `''` | No | - |
| `max-days` | Max. no. of days to keep run in a workflow | `''` | NO | - |

<br>

## ⚠️ Summary of GitHub rate limit for standard repository
```
* 1,000 requests per hour per repository
* No more than 100 concurrent requests are allowed
* No more than 900 points per minute are allowed for REST API endpoints
* No more than 90 seconds of CPU time per 60 seconds of real time is allowed
* Make too many requests that consume excessive compute resources in a short period of time.
```
<br>


<br>


## 😕  Troubleshooting

Open an [issue][issues]
<br>

## 🙏  Contributing

For pull requests to be accepted on this project, you should follow [PEP8][pep8] when creating/updating Python codes.

See [Contributing][contributing]
<br>

## 🙌 Appreciation
If you find this project helpful, please ⭐ star it.  **Thank you**.
<br>

## 📚 References

[GitHub API rate limit](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api?apiVersion=2022-11-28#primary-rate-limit-for-github_token-in-github-actions)

[How to fork a repo](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)
<br>

[contributing]: https://github.com/tagdots/delete-workflow-runs/blob/main/CONTRIBUTING.md
[issues]: https://github.com/tagdots/delete-workflow-runs/issues
[pep8]: https://google.github.io/styleguide/pyguide.html
