# Note

## extension

[GitHub Actions](https://marketplace.visualstudio.com/items?itemName=me-dutour-mathieu.vscode-github-actions): 자동완성 기능을 제공

## Variables

[Variables](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables)

변수 선언하기

```yaml
name: Greeting on variable day

on: workflow_dispatch

env:
  DAY_OF_WEEK: Monday

jobs:
  greeting_job_en:
    runs-on: ubuntu-latest
    env:
      Greeting: Hello
    steps:
      - name: Greeting
        run: echo "$Greeting $DAY_OF_WEEK"
        env:
            Name : "John"
 greeting_job_kr:
    runs-on: ubuntu-latest
    env:
      Greeting: 안녕
    steps:
      - name: Greeting
        run: echo "$Greeting $DAY_OF_WEEK"
        env:
            Name : "Mona"
```

## Github Context

[Contexts](https://docs.github.com/ko/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs)

특정 정보에 액세스 하는 방법 (workflow, runner, var... 등)

- github
- env
- vars
- job
- jobs
- steps
- runner
- secrets
- strategy
- matrix
- needs
- inputs
