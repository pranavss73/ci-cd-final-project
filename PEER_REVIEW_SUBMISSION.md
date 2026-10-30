# Coursera Peer-Review Assignment Submission (Tasks 1 - 10)

## Project Title
**CI/CD Pipeline Automation with GitHub Actions, Tekton, and Red Hat OpenShift**

---

### Task 1: Provide the GitHub URL of the repository in the learner’s GitHub account (2 pts)
**URL:**
```text
https://github.com/pranavss73/ci-cd-final-project
```

---

### Task 2: Provide the GitHub URL of .github/workflows/workflow.yml showing the code snippet for either the Lint with flake8 or Eslint step (2 pts)
**URL:**
```text
https://github.com/pranavss73/ci-cd-final-project/blob/main/.github/workflows/workflow.yml
```

**Code Snippet:**
```yaml
      - name: Lint with flake8
        run: |
          flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
          flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
```

---

### Task 3: Provide the GitHub URL of .github/workflows/workflow.yml showing the code snippet for the Run unit tests with either nose or jest test step (2 pts)
**URL:**
```text
https://github.com/pranavss73/ci-cd-final-project/blob/main/.github/workflows/workflow.yml
```

**Code Snippet:**
```yaml
      - name: Run unit tests with nose
        run: nosetests -v --with-spec --spec-color --with-coverage --cover-package=app
```

---

### Task 4: Provide the GitHub URL of .tekton/tasks.yml showing the code snippet for the cleanup task (2 pts)
**URL:**
```text
https://github.com/pranavss73/ci-cd-final-project/blob/main/.tekton/tasks.yml
```

**Code Snippet:**
```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: cleanup
spec:
  description: This task will clean up a workspace by deleting all the files.
  workspaces:
    - name: source
  steps:
    - name: remove
      image: alpine:3
      env:
        - name: WORKSPACE_SOURCE_PATH
          value: $(workspaces.source.path)
      workingDir: $(workspaces.source.path)
      securityContext:
        runAsNonRoot: false
        runAsUser: 0
      script: |
        #!/usr/bin/env sh
        set -eu
        echo "Removing all files from ${WORKSPACE_SOURCE_PATH} ..."
        if [ -d "${WORKSPACE_SOURCE_PATH}" ] ; then
          rm -rf "${WORKSPACE_SOURCE_PATH:?}"/*
          rm -rf "${WORKSPACE_SOURCE_PATH}"/.[!.]*
          rm -rf "${WORKSPACE_SOURCE_PATH}"/..?*
        fi
```

---

### Task 5: Provide the GitHub URL of .tekton/tasks.yml showing the code snippet for the Run unit tests with either nose or jest test step (2 pts)
**URL:**
```text
https://github.com/pranavss73/ci-cd-final-project/blob/main/.tekton/tasks.yml
```

**Code Snippet:**
```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: nose
spec:
  description: This task will unit test with testing framework nose
  workspaces:
    - name: source
  params:
    - name: args
      description: Arguments to pass to nose
      type: string
      default: "-v"
  steps:
    - name: nosetests
      image: python:3.9-slim
      workingDir: $(workspaces.source.path)
      script: |
        #!/bin/bash
        set -e
        python -m pip install --upgrade pip wheel
        pip install -r requirements.txt
        nosetests $(params.args)
```

---

### Task 6: Provide the screenshot of the OpenShift PersistentVolumeClaim details in a file named oc-pipelines-console-pvc-details(.png/jpg) (2 pts)
- **File to Upload:** `screenshots/oc-pipelines-console-pvc-details.png` (or `.jpg`)
- **Direct GitHub File Link:** [screenshots/oc-pipelines-console-pvc-details.png](https://github.com/pranavss73/ci-cd-final-project/blob/main/screenshots/oc-pipelines-console-pvc-details.png)

---

### Task 7: Provide the screenshot showing GitHub actions running successfully in a file named cicd-github-validate(.png/jpg) (2 pts)
- **File to Upload:** `screenshots/cicd-github-validate.png` (or `.jpg`)
- **Direct GitHub File Link:** [screenshots/cicd-github-validate.png](https://github.com/pranavss73/ci-cd-final-project/blob/main/screenshots/cicd-github-validate.png)

---

### Task 8: Provide the screenshot showing details of the OpenShift Pipeline - oc-pipelines-oc-final(.png/jpg) (2 pts)
- **File to Upload:** `screenshots/oc-pipelines-oc-final.png` (or `.jpg`)
- **Direct GitHub File Link:** [screenshots/oc-pipelines-oc-final.png](https://github.com/pranavss73/ci-cd-final-project/blob/main/screenshots/oc-pipelines-oc-final.png)

---

### Task 9: Provide the screenshot showing details of the OpenShift Pipeline running successfully in a file named oc-pipelines-oc-green (.png/jpg) (2 pts)
- **File to Upload:** `screenshots/oc-pipelines-oc-green.png` (or `.jpg`)
- **Direct GitHub File Link:** [screenshots/oc-pipelines-oc-green.png](https://github.com/pranavss73/ci-cd-final-project/blob/main/screenshots/oc-pipelines-oc-green.png)

---

### Task 10: Provide the screenshot showing details of the OpenShift application logs in a file named oc-pipelines-app-logs (.png/jpg) (2 pts)
- **File to Upload:** `screenshots/oc-pipelines-app-logs.png` (or `.jpg`)
- **Direct GitHub File Link:** [screenshots/oc-pipelines-app-logs.png](https://github.com/pranavss73/ci-cd-final-project/blob/main/screenshots/oc-pipelines-app-logs.png)
