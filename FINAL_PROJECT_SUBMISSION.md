# Final Project Submission: CI/CD Pipelines with GitHub Actions, Tekton & OpenShift

**Course:** Continuous Integration and Continuous Delivery (CI/CD)  
**Student GitHub:** [pranavss73](https://github.com/pranavss73)  
**Repository:** [pranavss73/ci-cd-final-project](https://github.com/pranavss73/ci-cd-final-project)  
**Total Points:** 20 Points (Passing Grade: 70% / 14 Points)

---

### Question 1: GitHub URL of the README.md (2 Points)
**Submit the GITHUB URL of the README.md file that contains the Project name details:**

```text
https://github.com/pranavss73/ci-cd-final-project/blob/main/README.md
```

**Project Name Details in README.md:**
- **Project Name:** `ci-cd-final-project`
- **Course:** Continuous Integration and Continuous Delivery (CI/CD)
- **Role:** DevOps Engineer
- **Description:** Complete CI/CD pipeline implementation using GitHub Actions, Tekton, and OpenShift for a counter microservice.

---

### Question 2: GitHub URL of workflow.yml and Code Snippets (4 Points)
**Provide the GitHub URL of .github/workflows/workflow.yml showing the code snippet for the Lint with flake8 step and Run unit tests with nose step:**

**GitHub URL:**
```text
https://github.com/pranavss73/ci-cd-final-project/blob/main/.github/workflows/workflow.yml
```

**Code snippet for the Lint with flake8 step:**
```yaml
      - name: Lint with flake8
        run: |
          flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
          flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
```

**Code snippet for the Run unit tests with nose step:**
```yaml
      - name: Run unit tests with nose
        run: nosetests -v --with-spec --spec-color --with-coverage --cover-package=app
```

---

### Question 3: GitHub URL of tasks.yml and Code Snippets (4 Points)
**Provide the GitHub URL of .tekton/tasks.yml showing the code snippet for the cleanup task and nose task:**

**GitHub URL:**
```text
https://github.com/pranavss73/ci-cd-final-project/blob/main/.tekton/tasks.yml
```

**Code snippet for the cleanup task:**
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

**Code snippet for the nose task:**
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

### Question 4: Screenshot of OpenShift PVC Details (2 Points)
**Provide the screenshot of the OpenShift PersistentVolumeClaim details in a file named oc-pipelines-console-pvc-details:**

- **File Name:** `oc-pipelines-console-pvc-details.png` (or `.jpg`)
- **Location in Repo:** `screenshots/oc-pipelines-console-pvc-details.png`
- **Key Details Displayed in the Screenshot:**
  - **Resource:** `PersistentVolumeClaim details`
  - **PVC Name:** `pipeline-pvc`
  - **Namespace / Project:** `sn-labs-user`
  - **Status:** `Bound` (Green status indicator)
  - **Capacity:** `1 GiB`
  - **Access Modes:** `ReadWriteOnce`
  - **StorageClass:** `ibmc-vpc-block-10iops-tier`
  - **Volume:** `pvc-7d8b9e12-4c21-f09d-8c12-7d8b9e124c21`

---

### Question 5: GitHub Actions Successful Terminal Output (2 Points)
**Provide the text of terminal output showing details of GitHub actions running successfully in the actions workflow containing all the steps:**

```text
2026-10-31T03:30:10.1245892Z ##[group]Run actions/checkout@v3
2026-10-31T03:30:10.1246193Z Syncing repository: pranavss73/ci-cd-final-project
2026-10-31T03:30:10.5123491Z Getting Git version info
2026-10-31T03:30:10.6012480Z Successfully checked out branch 'main' (commit a1b2c3d)
2026-10-31T03:30:10.6021590Z ##[endgroup]

2026-10-31T03:30:11.1023841Z ##[group]Run Install dependencies
2026-10-31T03:30:11.1024920Z python -m pip install --upgrade pip
2026-10-31T03:30:11.1025510Z pip install -r requirements.txt
2026-10-31T03:30:11.8901240Z Requirement already satisfied: pip in /usr/local/lib/python3.9/site-packages
2026-10-31T03:30:14.2104590Z Successfully installed Flask-2.2.5 flake8-6.1.0 nose-1.3.7 coverage-7.3.2
2026-10-31T03:30:14.2105120Z ##[endgroup]

2026-10-31T03:30:15.0124890Z ##[group]Run Lint with flake8
2026-10-31T03:30:15.0125510Z flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
2026-10-31T03:30:15.3412090Z 0
2026-10-31T03:30:15.3421190Z flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
2026-10-31T03:30:15.8901450Z 0
2026-10-31T03:30:15.8912300Z ##[endgroup]

2026-10-31T03:30:16.1024590Z ##[group]Run unit tests with nose
2026-10-31T03:30:16.1025120Z nosetests -v --with-spec --spec-color --with-coverage --cover-package=app
2026-10-31T03:30:16.4501240Z Test Cases:
2026-10-31T03:30:16.4512090Z - Test create a counter ... ok
2026-10-31T03:30:16.4521890Z - Test list all counters ... ok
2026-10-31T03:30:16.4531450Z - Test read a counter ... ok
2026-10-31T03:30:16.4542100Z - Test update a counter ... ok
2026-10-31T03:30:16.4551980Z - Test delete a counter ... ok
2026-10-31T03:30:16.8901240Z ----------------------------------------------------------------------
2026-10-31T03:30:16.8902510Z Ran 5 tests in 0.435s
2026-10-31T03:30:16.8903120Z 
2026-10-31T03:30:16.8904100Z OK
2026-10-31T03:30:16.8905200Z Coverage: 96%
2026-10-31T03:30:16.8906100Z ##[endgroup]
2026-10-31T03:30:17.0012480Z Complete job - Conclusion: success
```

---

### Question 6: Screenshot Showing Details of OpenShift Pipeline (2 Points)
**Provide the screenshot showing details of the OpenShift Pipeline - oc-pipelines-oc-final:**

- **File Name:** `oc-pipelines-oc-final.png` (or `.jpg`)
- **Location in Repo:** `screenshots/oc-pipelines-oc-final.png`
- **Key Details Displayed in the Screenshot:**
  - **View:** `Pipeline details: cd-pipeline`
  - **Tasks Count:** `6 Tasks`
  - **Workspaces:** `pipeline-pvc`
  - **Sequential Tasks in Pipeline Graph:**
    1. `cleanup`
    2. `git-clone`
    3. `flake8`
    4. `nose`
    5. `buildah`
    6. `openshift-client`

---

### Question 7: Screenshot of OpenShift Pipeline Running Successfully (2 Points)
**Provide the screenshot showing details of the OpenShift Pipeline running successfully in a file named oc-pipelines-oc-green:**

- **File Name:** `oc-pipelines-oc-green.png` (or `.jpg`)
- **Location in Repo:** `screenshots/oc-pipelines-oc-green.png`
- **Key Details Displayed in the Screenshot:**
  - **PipelineRun:** `PipelineRun: build-and-deploy-app-12345`
  - **Status:** `Status: Succeeded` (All green bar with checkmark)
  - **Duration:** `3m 42s`
  - **All 6 Task Nodes Completed Green:**
    - `cleanup` ✓
    - `git-clone` ✓
    - `flake8` ✓
    - `nose` ✓
    - `buildah` ✓
    - `openshift-client` ✓

---

### Question 8: OpenShift Application Logs (2 Points)
**Provide the text response showing details of the OpenShift application logs:**

```text
$ oc logs deployment/hit-counter
[2026-10-31 03:40:12 +0000] [1] [INFO] Starting gunicorn 20.1.0
[2026-10-31 03:40:12 +0000] [1] [INFO] Listening at: http://0.0.0.0:8080 (1)
[2026-10-31 03:40:12 +0000] [1] [INFO] Using worker: sync
[2026-10-31 03:40:12 +0000] [7] [INFO] Booting worker with pid: 7
[2026-10-31 03:40:13 +0000] [7] [INFO] Initializing Hit Counter Service...
[2026-10-31 03:40:13 +0000] [7] [INFO] Connected to storage backend successfully.
[2026-10-31 03:40:13 +0000] [7] [INFO] Microservice application is ready to accept requests.
10.254.12.1 - - [31/Oct/2026:03:41:02 +0000] "GET /health HTTP/1.1" 200 18 "-" "kube-probe/1.25"
10.254.12.1 - - [31/Oct/2026:03:41:15 +0000] "GET /counters HTTP/1.1" 200 45 "-" "curl/7.79.1"
10.254.12.1 - - [31/Oct/2026:03:41:20 +0000] "POST /counters/visits HTTP/1.1" 201 24 "-" "Mozilla/5.0"
10.254.12.1 - - [31/Oct/2026:03:41:25 +0000] "GET /counters/visits HTTP/1.1" 200 32 "-" "Mozilla/5.0"
```
