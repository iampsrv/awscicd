# 📘 What is buildspec.yml?

The `buildspec.yml` is a YAML file that defines the build commands and settings used by AWS CodeBuild. It tells CodeBuild what to do at each phase of the build lifecycle — such as installing dependencies, compiling code, running tests, and packaging artifacts.

- Must be named `buildspec.yml` by default (can be overridden in the CodeBuild project settings).
- Must be placed in the root directory of your source code repository (or a specified path).

---

# 📄 Basic Structure of buildspec.yml

\`\`\`yaml
version: 0.2

phases:
  install:
    commands:
      - echo Installing dependencies
  pre_build:
    commands:
      - echo Logging in to Amazon ECR
  build:
    commands:
      - echo Building the application
  post_build:
    commands:
      - echo Build completed on `date`

artifacts:
  files:
    - '**/*'
\`\`\`

---

# 🧩 File Sections Explained

## 1. version
- Defines the buildspec version.
- Supported: `0.1` (legacy), `0.2` (recommended).

## 2. env (Environment Variables)

Define variables used during the build.

\`\`\`yaml
env:
  variables:
    NODE_ENV: production
  parameter-store:
    DB_PASSWORD: /my/db/password
  secrets-manager:
    API_KEY: mySecret:api_key
\`\`\`

- Plaintext: under `variables`
- Secure values: from Systems Manager Parameter Store or Secrets Manager

## 3. phases

Specifies commands in the build lifecycle:

| Phase       | Purpose                                   |
|-------------|-------------------------------------------|
| install     | Set up tools and runtime environments     |
| pre_build   | Authentication, environment prep, linting |
| build       | Compilation, bundling, test execution     |
| post_build  | Cleanup, report generation, notifications |

Each phase can include:
- `commands`: A list of shell commands.
- `runtime-versions`: Language versions (e.g., Node.js, Python).

### Example with runtime version:
\`\`\`yaml
install:
  runtime-versions:
    nodejs: 18
  commands:
    - npm install
\`\`\`

## 4. artifacts

Specifies which files to archive after build.

\`\`\`yaml
artifacts:
  files:
    - 'dist/**/*'
  base-directory: 'dist'
  name: 'my-artifact.zip'
  discard-paths: yes
\`\`\`

- `files`: Wildcard patterns allowed.
- `base-directory`: Optional, changes root for artifacts.
- `discard-paths`: If true, files are flattened.

## 5. reports

Used for test results (e.g., JUnit, Cucumber).

\`\`\`yaml
reports:
  my-report:
    files:
      - '**/test-results.xml'
    file-format: JUNITXML
    base-directory: reports/
\`\`\`

## 6. cache

Used to speed up builds by caching dependencies.

\`\`\`yaml
cache:
  paths:
    - 'node_modules/**/*'
    - '/root/.cache/pip'
\`\`\`

- Automatically stored in S3 between builds.

## 7. timeout-in-minutes

Sets maximum build time (default: 60 minutes).

\`\`\`yaml
timeout-in-minutes: 30
\`\`\`

## 8. logs

Optional logging config (usually defined in the project itself).

\`\`\`yaml
logs:
  cloudwatch:
    enabled: true
    group_name: my-log-group
    stream_name: build-log
\`\`\`

---

# ✅ Best Practices

- Use `runtime-versions` for consistent environments.
- Always include a `post_build` section for reporting or cleanup.
- Enable caching for dependency-heavy builds.
- Use environment variables to manage secrets securely.
- Test your `buildspec.yml` locally with a Docker container or using CodeBuild's local agent.
- Separate build and deploy responsibilities clearly.

---

# 📦 Sample buildspec.yml for a Node.js App

\`\`\`yaml
version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 18
    commands:
      - echo Installing dependencies...
      - npm ci
  build:
    commands:
      - echo Building the app...
      - npm run build
  post_build:
    commands:
      - echo Build completed

artifacts:
  files:
    - 'dist/**/*'
    - 'package.json'
    - 'package-lock.json'
  base-directory: dist/
  discard-paths: yes

cache:
  paths:
    - 'node_modules/**/*'
\`\`\`

---

# 🧪 Sample with Test Reports

\`\`\`yaml
version: 0.2

phases:
  build:
    commands:
      - npm install
      - npm test -- --reporters=junit
artifacts:
  files:
    - '**/*'
reports:
  unit-test-results:
    files:
      - 'test-results.xml'
    file-format: JUNITXML
\`\`\`

---

# 🛠️ Troubleshooting Tips

- **File not found?** Make sure `buildspec.yml` is at the repo root or set explicitly in the project config.
- **Secrets not working?** Verify permissions for Parameter Store or Secrets Manager.
- **Build fails with exit code 255?** Usually indicates permission or syntax errors.
- **Artifacts empty?** Confirm correct base-directory and file patterns.

---

# 🔗 Helpful Resources

- [AWS buildspec Reference](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
- [Sample buildspec files](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-example.html)
- [CodeBuild Environment Variables](https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-env-vars.html)
- [Testing Locally with CodeBuild Agent](https://docs.aws.amazon.com/codebuild/latest/userguide/use-codebuild-agent.html)