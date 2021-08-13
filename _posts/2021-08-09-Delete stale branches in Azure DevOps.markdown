---
title:  "Delete stale branches in Azure DevOps"
date:   2021-08-09 18:25:48
categories: [devops]
tags: [azure, devops]	
---
<h3>Introduction</h3>
<p>
In my experience, there are two main reasons stale branches exist in Azure DevOps (or any source code repo):
<ol>
<li>Branches are not deleted after completing pull request. Although Azure DevOps gives you option to delete branch after merging given the right permissions are applied. But this option is not used most of the time</li>
<li>Developers create temp branches for proof of concepts, resolve merging conflicts or any other reason and then forget to delete them</li>
</ol>
In this exercise, we will delete all the branches left as result of two cases.
</p>

<h3>Prerequisite</h3>
a. Install the <a href="https://docs.microsoft.com/en-us/cli/azure/install-azure-cli" target="_blank">Azure Cli</a><br>
b. Install the Azure Cli DevOps extension through PowerShell by running following command
<pre><code>
<span>$</span> az extension add --name azure-devops
</code></pre>

<h3>Write deletion scripts</h3>

<p>Start PowerShell and login to Azure</p>
<pre><code>
<span>$</span> az login
</code></pre>

<p>a. Script to delete branches that have completed the pull request (delete-pull-request-completed-branches.ps1)</p>
<pre><code>
$project = "{projectName}"
$repository = "{repoName}"
if (-not (Test-Path env:IS_DRY_RUN)) { $env:IS_DRY_RUN = $true }
Write-Host ("is dry run: {0}" -f $env:IS_DRY_RUN)

$prsCompleted = az repos pr list `
    --project $project `
    --repository $repository `
    --target-branch develop `
    --status completed `
    --query "[].sourceRefName" |
ConvertFrom-Json;

if ($prsCompleted.count -eq 0) {
    Write-Host "No merged pull request"
    return;
}

$refs = az repos ref list --project $project --repository $repository --filter heads | ConvertFrom-Json
$refs |
Where-Object { $prsCompleted.Contains( $_.name ) } |
ForEach-Object {
    Write-Host ("deleting merged branch: {0} - {1}" -f $_.name, $_.objectId)
    if (![System.Convert]::ToBoolean($env:IS_DRY_RUN)) {
        $result = az repos ref delete `
            --name $_.name `
            --object-id $_.objectId `
            --project $project `
            --repository $repository |
        ConvertFrom-Json
        Write-Host ("success message: {0}" –f $result.updateStatus)
    }
}
</code></pre>
<p>
Notes:
<ul>
  <li>Replace {projectName} and {repoName} with your own project and repository name</li>
  <li>Use IS_DRY_RUN environment variable to list the branches without deleting them.  It can be changed manually when running on local machine or can be setup as <a href="https://colinsalmcorner.com/azure-pipeline-variables/" target="_blank">Azure DevOps pipeline build variable</a>
  </li>
</ul>
</p>

<p>b. Script to delete old, staled branches (delete-stale-branches.ps1)</p>
<pre><code>
$project = "projectName"
$repository = "repoName"
$excludeBranches = @("develop", "master")
$daysDeleteBefore = -30
$dateTimeNow = [DateTime]::Now
$dateTimeBeforeToDelete = $dateTimeNow.AddDays( $daysDeleteBefore)
if (-not (Test-Path env:IS_DRY_RUN)) { $env:IS_DRY_RUN = $true }
Write-Host ("is dry run: {0}" -f $env:IS_DRY_RUN)
Write-Host ("datetime now: {0}" -f $dateTimeNow)
Write-Host ("delete branches before {0}" -f (get-date $dateTimeBeforeToDelete))

$refs = az repos ref list --project $project --repository $repository --filter heads | ConvertFrom-Json

$toDeleteBranches = @()
 
foreach ($ref in $refs) {

    if ($ref.name -replace "refs/heads/" -in $excludeBranches) {
        continue;
    }

    $objectId = $ref.objectId
 
    # fetch individual commit details
    $commit = az devops invoke `
        --area git `
        --resource commits `
        --route-parameters `
        project=$project `
        repositoryId=$repository `
        commitId=$objectId |
    ConvertFrom-Json
 
    $toDelete = [PSCustomObject]@{ 
        objectId     = $objectId
        name         = $ref.name
        creator      = $ref.creator.uniqueName
        lastAuthor   = $commit.committer.email
        lastModified = $commit.push.date
    }
    $toDeleteBranches += , $toDelete
}

$toDeleteBranches = $toDeleteBranches | Where-Object { (get-date $_.lastModified) -lt (get-date $dateTimeBeforeToDelete) }

if ($toDeleteBranches.count -eq 0) {
    Write-Host "No stale branches to delete"
    return;
}

$toDeleteBranches |
ForEach-Object {
    Write-Host ("deleting staled branch: name={0} - id={1} - lastModified={2}" -f $_.name, $_.objectId, $_.lastModified)
    if (![System.Convert]::ToBoolean($env:IS_DRY_RUN)) {
        $result = az repos ref delete `
            --name $_.name `
            --object-id $_.objectId `
            --project $project `
            --repository $repository |
        ConvertFrom-Json
        Write-Host ("success message: {0}" -f $result.updateStatus)
    }
}
</code></pre>
<p>
Notes:
<ul>
  <li>Use $excludeBranches for excluding branches you do not want to delete</li>
  <li>Use $daysDeleteBefore for indicating the number of days before to delete branches</li>
</ul>
</p>

<h3>Define the Azure DevOps pipeline (azure-pipelines.yml)</h3>
<pre><code>
name: Delete Branches
 
schedules:
  # run at midnight every day
  - cron: "0 0 * * *"
    displayName: Delete branches
    branches:
      include:
        - develop
pool:
  vmImage: ubuntu-latest

steps:
- script: |
    az extension add -n azure-devops
  displayName: Install Azure DevOps CLI

- checkout: self
  clean: true

- script: |
    echo $(ACCESS_TOKEN) | az devops login
  displayName: Login
  env:
    ADO_PAT_TOKEN: $(ACCESS_TOKEN)

- pwsh: .\delete-pull-request-completed-branches.ps1
  displayName: 'Delete merged branches'
  
  
- pwsh: .\delete-stale-branches.ps1
  displayName: 'Delete stale branches'

</code></pre>
<p>
Notes:
<ul>
  <li>Schedule is defined to run the job at midnight</li>
  <li>ubuntu-latest is used as build machine</li>
  <li>First step install the Azure DevOps extension</li>
  <li>Second step login to Azure DevOps using <a href="https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page " target="_blank">PAT token</a>. ACCESS_TOKEN should be defined as as <a href="https://colinsalmcorner.com/azure-pipeline-variables/" target="_blank">Azure DevOps pipeline build variable</a></li>
  <li>Third step run the script to delete merged pull requests</li>
  <li>Fourth step run the script to delete stale branches</li>
</ul>
</p>

<h3>Resources:</h3>
<p>
<ul>
  <li><a href="https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema" target="_blank">Azure DevOps YAML schema reference</a></li>
  <li><a href="https://colinsalmcorner.com/azure-pipeline-variables/" target="_blank">Azure Pipeline Variables</a></li>
  <li><a href="https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=preview-page " target="_blank">Creating Personal Access Token (PAT) in Azure DevOps</a></li>
  <li><a href="https://azuredevopslabs.com/labs/azuredevops/yaml/ " target="_blank">Setting up Azure DevOps pipeline using YAML </a></li>
</ul>
</p>

