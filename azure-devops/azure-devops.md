# Azure DevOps

## Branch Name Conditions

We can set conditions on tasks that that they only run when a condition is true, we can use this to only run tasks when a branch is named as expected. 

### Setup a Variable

``` yml
variables:
  isRelease: ${{ startsWith(variables['Build.SourceBranch'], 'refs/heads/release') }}
  isHotfix: ${{ startsWith(variables['Build.SourceBranch'], 'refs/heads/hotfix') }}
  isFeature: ${{ startsWith(variables['Build.SourceBranch'], 'refs/heads/feature') }}
  isSupport: ${{ startsWith(variables['Build.SourceBranch'], 'refs/heads/support') }}
  isSpike: ${{ startsWith(variables['Build.SourceBranch'], 'refs/heads/spike') }}
```

### Using the variable in a task

``` yml
- task: PublishBuildArtifacts@1
  condition: and(succeeded(), eq(variables.isRelease, 'true'))
```