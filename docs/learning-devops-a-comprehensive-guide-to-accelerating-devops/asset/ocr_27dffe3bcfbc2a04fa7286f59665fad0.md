```
command: test
projects: '**/*/Tests/*.csproj'
arguments: '--configuration Release --collect "Code coverage"'
```

In this file, we have three sections: `trigger`, `pool`, and `steps`. In the `trigger` section, we indicate the branch of the code that triggers the pipeline, so here, any commit on the `master` branch will execute the pipeline.

Then, the `pool` section indicates the agent pool to be used to execute the pipeline. Finally, in the `steps` section, we write all tasks that will be executed during the pipeline. Here, in our lab, we have declared four tasks, as follows:

• The restore of the NuGet packages of the project
• The compilation of the project
• The execution of the tests
• The publication of the code coverage of the executed tests

+   2. We commit and push this file into the root of the repository, as follows:

![](bbox=[0.186, 0.485, 0.786, 0.708])

Figure 7.48 – Azure Pipelines: YAML pipeline