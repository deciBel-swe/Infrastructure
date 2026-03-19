## Concepts and Termonology:
1. Github events: any event that happens in the github API or can be seen by the Github API (PR, Push, etc)
2. Workflow: Triggered by events, Are pipelines that contains Jobs
3. Jobs: can contain multiple steps
4. Runner: The actual server Environment that is being run, the same job is the same environment but across jobs different environment (take care of environment vars/ state/ passing data between jobs)

## Workflow Structure:
- Workflows form a Directed Acyclic Graph (DAG) by using "needs" to specify the structure

## Triggering events:
- Manually, Push to branch, Create/update PR, Cron Schedule, Tag, etc
- Can also specify specific files only (paths, paths-ignore), Example:
``` yaml
    paths:
    - 'src/**'       # Triggers on changes in the src directory
    - 'config/*.yml' # Triggers on changes to .yml files in the config directory
    paths-ignore:
    - 'docs/**'      # Ignores changes in the docs directory
    - 'README.md'    # Ignores changes to the README.md file
``` 
- Can also specify specific Branches (main, dev) or even (feat/*, etc)

## Environment Variables Scoping
- Env vars can be scoped to Workflows, Job, Or Step

## Passing Variables:
- Variables can be passed using: 
    - Step & job inputs/outputs
    - Environment Variables

- infra-job output -> input:
    1. Write to `$GITHUB_OUTPUT`          (`echo "my_var=value" >> $GITHUB_OUTPUT`)
    2. Refrence with \${{ }} syntax      (`${{ steps.step_id.outputs.my_var }}`)

- infra-job env var:
    1. Write to `$GITHUB_ENV` (`echo "MY_VAR=value" >> $GITHUB_ENV`)
    2. Refrence with `$VAR`
    
- Inter-job output -> input:
    1. Write to `$GITHUB_OUTPUT`
    2. Refrence in job "outputs"
    3. Set `needs`
    2. Refrence with \${{}} syntax       (`${{ needs.JobA.outputs.my_output_name }}`)

- for outputs:
    - "outputs" should be defined, Example:
        ``` yaml
        outputs:
            - foo: `${{ steps.step_id.outputs.var }}`
        ```

## Secrets and Variables:
- Can be stored in different levels
    - Organization level
    - Repository
    - Environment (e.g. staging/production)
- Accessed By
    ``` yaml
    ${{ secrets.SECRET_NAME }}
    ${{ vars.VAR_NAME }}
    ```

## Contexts:
- needs: outputs & results from dependent jobs
- steps: prior steps' outputs, outcome/conclusion
- secrets: secrets val
- vars:
- github: run and event metadata
- env: variables set at workflow/job/step scope (override rules: step > job > workflow)
- matrix: current matrix values (e.g. os, node)
- strategy: matrix strategy info (job-index, job-total, fail-fast, max-parallel)
- job: current job status container/service info
- runner: runner OS/arch/temp/tool_cache
- inputs: parameters to reusable/workflow_dispatch workflows

## Runner types
...