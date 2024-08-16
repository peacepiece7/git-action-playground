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

### tl;dr

Github에서 액션 객체를 제공해준다.

[github-action 변수 출력](https://github.com/peacepiece7/git-action-playground/actions/runs/10414522785/job/28843610626),
[github context docs](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#job-context)

여기서 뭐가 있는지 보면 되고, matrix에 따라서 조금씩 다르다.

---

[Contexts](https://docs.github.com/ko/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs)

github에서 제공해주는 기능으로

특정 정보에 액세스 하는 방법이다.

아래처럼 쓸 수 있는 객체 애들이다.

```yaml
jobs:
  foo:
    :steps:
      - name: Checkout current commit (${{ github.sha }}) # github 요 부분이 github context
      - name: Set up Node.js ${{ matrix.node-version }} # matrix 요 부분이 github context
```

[다음 변수들이 있다](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#about-contexts)

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

위 컨택스트 들은 모두 사용할 수 있는 공간이 제한되어 있다.

([컨텍스트 가용성](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#context-availability))에서 확인할 수 있다.

matrix에 따라서 사용할 수 있는 컨텍스트가 달라지는데 다음과 같이 작성하면 github에서 객체에 어떤 속성이 있는지 확인할 수 있다.

```yml
name: GitHub Context
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    env:
      GREETING: Hello
    steps:
      - run: echo "print all github context info"
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        shell: bash
      - run: echo "print all env context info"
        env:
          ENV_CONTEXT: ${{ toJson(env) }}
        shell: bash
      - run: echo "print all vars context info"
        env:
          VARS_CONTEXT: ${{ toJson(vars) }}
        shell: bash
      - run: echo "print all job context info"
        env:
          JOB_CONTEXT: ${{ toJson(job) }}
        shell: bash
      #  - run: echo "print all jobs context info"
      #   env:
      #     GITHUB_CONTEXT: ${{ toJson(jobs) }}
      #   shell: bash
      - run: echo "print all steps context info"
        env:
          STEPS_CONTEXT: ${{ toJson(steps) }}
        shell: bash
      - run: echo "print all runner context info"
        env:
          RUNNER_CONTEXT: ${{ toJson(runner) }}
        shell: bash
      #   - run: echo "print all secrets context info"
      #     env:
      #       GITHUB_CONTEXT: ${{ toJson(secrets) }}
      #     shell: bash
      - run: echo "print all strategy context info"
        env:
          STRATEGY: ${{ toJson(strategy) }}
        shell: bash
      - run: echo "print all matrix context info"
        env:
          MATRIX_CONTEXT: ${{ toJson(matrix) }}
        shell: bash
      - run: echo "print all needs context info"
        env:
          NEEDS_CONTEXT: ${{ toJson(needs) }}
        shell: bash
      - run: echo "print all inputs context info"
        env:
          INPUTS_CONTEXT: ${{ toJson(inputs) }}
        shell: bash
```

### github

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

### env

workflow, job, step에서 사용할 수 있는 변수임 같은 내용이 위에 또 있지만 다시 적음

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

### vars

env에 있는 변수와 같은 역할이지만 github에서 설정하는 변수는 vars를 붙인다.

자매품으로 secrets가 있다.

```yml
on:
  workflow_dispatch:
env:
  # Setting an environment variable with the value of a configuration variable
  env_var: ${{ vars.ENV_CONTEXT_VAR }}

jobs:
  display-variables:
    name: ${{ vars.JOB_NAME }}
    # You can use configuration variables with the `vars` context for dynamic jobs
    if: ${{ vars.USE_VARIABLES == 'true' }}
    runs-on: ${{ vars.RUNNER }}
    environment: ${{ vars.ENVIRONMENT_STAGE }}
    steps:
      - name: Use variables
        run: |
          echo "repository variable : $REPOSITORY_VAR"
          echo "organization variable : $ORGANIZATION_VAR"
          echo "overridden variable : $OVERRIDE_VAR"
          echo "variable from shell environment : $env_var"
        env:
          REPOSITORY_VAR: ${{ vars.REPOSITORY_VAR }}
          ORGANIZATION_VAR: ${{ vars.ORGANIZATION_VAR }}
          OVERRIDE_VAR: ${{ vars.OVERRIDE_VAR }}

      - name: ${{ vars.HELLO_WORLD_STEP }}
        if: ${{ vars.HELLO_WORLD_ENABLED == 'true' }}
        uses: actions/hello-world-javascript-action@main
        with:
          who-to-greet: ${{ vars.GREET_NAME }}
```

secrets도 똑같다.

```yml
- name: Copy files to the personal server via SCP.
  env:
    SSHPASS: ${{ secrets.SERVER_PASSWORD }}
  run: |
    sshpass -e scp -o StrictHostKeyChecking=no build.zip ${{ secrets.SERVER_USERNAME }}@${{ secrets.SERVER_IP }}:${{ secrets.DEPLOY_PATH }}
```

### job

job에는 현재 실행 중인 작업에 대한 정보가 들어가 있다.

### steps

steps에는 현재 실행중인 단계에 대한 정보가 들어가 있다.

### runner

runner에는 현재 실행중인 runner에 대한 정보가 들어가 있다.

### strategy

matrix 실행 전략을 설정할 때 사용한다.

test-group 1, 2와 node 14, 16을 각각 조합해서 실행, 총 4번 실행된다.

```yml
name: Test strategy
on: push

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        test-group: [1, 2] # 이거 이름은 아무거나 해도 된다.
        node: [14, 16]
    steps:
      - run: echo "Mock test logs" > test-job-${{ strategy.job-index }}.txt
      - name: Upload logs
        uses: actions/upload-artifact@v4
        with:
          name: Build log for job ${{ strategy.job-index }}
          path: test-job-${{ strategy.job-index }}.txt
```

### matrix

strategy에서 정의한 행렬의 값이 들어간다.

```yml

```
