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
    steps:
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

위 컨텍스트 들은 모두 사용할 수 있는 공간이 제한되어 있다.

([컨텍스트 가용성](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs#context-availability))에서 확인할 수 있다.

matrix에 따라서 사용할 수 있는 컨텍스트가 달라지는데 다음과 같이 작성하면 github contexts 어떤 속성이 있는지 확인할 수 있다.

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

![git-action-job](./git-action-1.png)

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

![git-action-job](./git-action-2.png)

```yml
name: Test matrix
on: push

jobs:
  matrix-test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        name: ['Mona', 'Lynn']
        hi: ['Hello', 'Hi']
    steps:
      - run: echo "${{ matrix.hi }} ${{ matrix.name }}"
```

### needs

[job간의 종속성을 설정할 때 사용](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/passing-information-between-jobs#example-defining-outputs-for-a-job)한다.

말로 설명하기 어려운데 job1의 job context를 job2에서 불러올 때 쓴다고 생각하면 된다.

```yml
jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      output_1: ${{ steps.gen_output.outputs.output_1 }}
      output_2: ${{ steps.gen_output.outputs.output_2 }}
      output_3: ${{ steps.gen_output.outputs.output_3 }}
    strategy:
      matrix:
        version: [1, 2, 3]
    steps:
      - name: Generate output
        id: gen_output
        run: |
          version="${{ matrix.version }}"
          echo "output_${version}=${version}" >> "$GITHUB_OUTPUT"
  job2:
    runs-on: ubuntu-latest
    needs: [job1]
    steps:
      # Will show
      # {
      #   "output_1": "1",
      #   "output_2": "2",
      #   "output_3": "3"
      # }
      - run: echo '${{ toJSON(needs.job1.outputs) }}'
```

job1이 끝나면 job2가 실행된다.

![needs](./git-action-3.png)

## working directory

[Setting a default shell and working directory](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/setting-a-default-shell-and-working-directory#setting-default-shell-and-working-directory)

git-action이 돌아가는 컴퓨터의 작업 디렉터리를 설정할 때 사용한다.

```yml
name: Test working directory
on: push

jobs:
  create-index-files:
    runs-on: ubuntu-latest
    defaults:
      run:
        shell: bash
    steps:
      # git pull로 저장소 가져오기
      - name: Checkout the repository.
        uses: actions/checkout@v3

      - name: check working directory
        run: |
          pwd
          echo "Contents of default working directory:"
          ls -l

      - name: Touch config file
        working-directory: packages/configuration
        run: |
          pwd
          echo "Contents of packages/configuration:"
          ls -l
          touch ./index.txt

      - name: Touch UI file
        working-directory: packages/ui
        run: |
          pwd
          echo "Contents of packages/ui:"
          ls -l
          touch ./index.txt
```

## Environment

[Using environments for deployment](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/using-environments-for-deployment)

prd, stg, dev 환경으로 배포할 때 마다 다른 환경 변수, 시크릿이 필요할 경우 사용한다.

`environment`에 환경 이름을 적어주면 된다.

만약에 변수 `vars.BACK_URL`이 `https://myapp.api.<prd|stg|dev>.com` 이라면

settings -> environment -> New environment 버튼을 클릭하고 다음과 같이 추가한다. (dev, stg, prd도 추가해보자)

![git-action-env2](./git-action-5.png)
![git-action-env](./git-action-4.png)

그리고 다음과 같이 작성한다.

```yml
name: Test environment
on: push

jobs:
  deploy-to-dev:
    runs-on: ubuntu-latest
    environment: dev # environment 이름을 적어준다
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Deploy to Development
        run: echo "${{ vars.BACK_URL }}"

  deploy-to-staging:
    runs-on: ubuntu-latest
    environment: stg
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Deploy to Staging
        run: echo "${{ vars.BACK_URL }}"

  deploy-to-production:
    runs-on: ubuntu-latest
    environment:
      name: prd
      url: https://myapp.com # 선택 사항으로 배포된 주소를 적어줄 수 있다. (git-action에 배포 주소가 표시되어서 편리하다.)
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Deploy to Production
        run: echo "${{ vars.BACK_URL }}"
```

## 더 알아볼 것들

캐싱, 병렬 실행, 워크플로우 트리거 이벤트, 워크플로우 구문 등 기본적인 사용방법 외에 더 알아볼 것들

## Concurrency

[concurrency](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/control-the-concurrency-of-workflows-and-jobs)

## cache dependency

[cache dependency](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/caching-dependencies-to-speed-up-workflows)

## Workflow trigger event

[workflow trigger event](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/triggering-a-workflow)

```yml
on:
  push:
    tags:
      - v1.**

on:
  push:
    paths:
      - '**.js'

on:
  push:
    branches:
      - 'releases/**'
    paths:
      - '**.js'
```

## workflow syntax

[workflow syntax](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#onpushbranchestagsbranches-ignoretags-ignore)
