# Note

## extension

[GitHub Actions](https://marketplace.visualstudio.com/items?itemName=me-dutour-mathieu.vscode-github-actions): 자동완성 기능을 제공

## Variables

[Variables](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables)

[변수 선언하기](https://github.com/peacepiece7/git-action-playground/actions/runs/10414361161/job/28843171795)

```yaml
name: Greeting on variable day
on: [push]

env:
  DAY_OF_WEEK: Monday

jobs:
  greeting_job_en:
    runs-on: ubuntu-latest
    env:
      Greeting: Hello
    steps:
      - name: Greeting
        run: echo "$Greeting $Name today is $DAY_OF_WEEK" # Hello John today is Monday
        env:
          Name: 'John'
      - name: Hey
        run: echo "Hi $Name" # Hi Mona
        env:
          Name: 'Mona'
  greeting_job_kr:
    runs-on: ubuntu-latest
    env:
      Greeting: 안녕
    steps:
      - name: Greeting
        run: echo "$Greeting $Name 오늘은 $DAY_OF_WEEK" # 안녕 Mona 오늘은 Monday
        env:
          Name: 'Mona'
      - name: Hey
        run: echo "안녕 $Name" # 안녕 John
        env:
          Name: 'John'
```

## Github Context

[Contexts](https://docs.github.com/ko/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs)

특정 정보에 액세스 하는 방법으로 아래처럼 쓸 수 있는 시스템 변수 같은 애들임

```yaml
jobs:
  foo:
    :steps:
      - name: Checkout current commit (${{ github.sha }})
      - name: Set up Node.js ${{ matrix.node-version }}
```

[다음 변수들이 있음](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#about-contexts)

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

하나씩 알아보자

1 github

작업중인 git-action에 대한 정보

```yml
name: Run CI
on: [push, pull_request]

jobs:
  normal_ci:
    runs-on: ubuntu-latest
    steps:
      - name: Run normal CI
        run: echo "Running normal CI"

  pull_request_ci:
    runs-on: ubuntu-latest
    if: ${{ github.event_name == 'pull_request' }}
    steps:
      - name: Run PR CI
        run: echo "Running PR only CI"
```
