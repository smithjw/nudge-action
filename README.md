# nudge-actions
Reusable Workflow and stand-alone Python script for updating a Nudge osVersionRequirements array using Apple's gdfm service (https://gdmf.apple.com/v2/pmv).

## Reusable Workflow (in progress)
To call the workflow from your own repo, create a GitHub Actions Workflow file with the following `jobs` block:

``` yaml
name: "Test workflow_call"

on:
  workflow_dispatch:

jobs:
  call_workflow:
    uses: smithjw/nudge-actions/.github/workflows/update-nudge-version.yml@main
    with:
      unos_debug: true
      unos_test_mode: true
```

If you would like to specify the minimum os version, add `unos_min_major_os_version` to your Workflow
If you would like to specify location (relative to the working directory of your repo) of the nudge.json file, add `unos_nudge_json_file` to your Workflow

## Python Script
This has been written and tested on Python 3.11. The script can be run independtly of the GitHub Action and takes the following input:

- `--debug` OR `-d` OR `UNOS_DEBUG` (environment variable)
  - Produces verbose output to the console
- `--test-mode` OR `-t` OR `UNOS_TEST_MODE` (environment variable)
  - Will enable verbose logging and prevent writing anything to disk
- `--version` OR `-v` OR `UNOS_MIN_MAJOR_OS_VERSION` (environment variable)
  - Sets the minimum major OS version supported in your environment
- `--file` OR `-f` OR `UNOS_NUDGE_JSON_FILE` (environment variable)
  - Location of your json file containing the `osVersionRequirements` array. If none is specified the file `nudge.json` is written to the current working directory.

## Running the standalone script

``` shell
# Clone Repo
gh repo clone smithjw/nudge-actions
cd nudge-actions

# Create Python Virtual Environment
python -m venv .venv
source .venv/bin/activate

# Install Requirements
pip install -r app/requirements.txt

# Run Script
python app/update_nudge_osVersionRequirements.py --test-mode
```

### Notes

If https://gdmf.apple.com/v2/pmv changes format in the future or can no longer be used, an alternative could be https://jamf-patch.jamfcloud.com/v1/software/
