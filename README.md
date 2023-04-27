# TTAP WITH CODE COVERAGE AND QUALITY

## Front End Testing 

End-to-end testing using Cypress which allows developers to test their code, helps to catch bugs and errors.

## Installation

Use the [link](https://docs.cypress.io/guides/getting-started/installing-cypress#Direct-download) to install Cypress zip file. Cypress will run without needing to install any dependencies.

## Setup

On opening the cypress, Select the project HMI folder then choose the E2E testing type.

After launching the browser, Select the browser type to run the tests.

![Screenshot (45)](https://user-images.githubusercontent.com/84472191/234762874-c0e56002-c913-47ae-9d32-ea39ca63e20c.png)


## Writing test file

Create new spec.cy.ts file.

![Screenshot (50)](https://user-images.githubusercontent.com/84472191/234762634-f6a0b793-6604-452e-9106-ea953201bea5.png)

```
describe('Register module', () => {
//Register for new user
it('Register' , () => {
 cy.visit('http://localhost:4200/login')
 cy.contains('Register').click()
 }) 
})
```
Save the file.

## Result

Open selected browser it will automatically run the test. Along with this run the HMI Client and TTAP Server seperately.

![Screenshot (49)](https://user-images.githubusercontent.com/84472191/234762664-a2f18d2e-0038-43fd-b6c2-bdd53c215f52.png)

______________________________________________________________


# Unit Testing on Backend

Unit testing using xUnit which allows developers to test their code.

## Installation

Install [xUnit.net.TestGenerator2022](https://marketplace.visualstudio.com/items?itemName=YowkoTsai.xUnitnetTestGenerator) extenstion which allows to write unit tests. 

After that install [xUnit](https://www.nuget.org/packages/xunit) NuGet package in *.[Tt]est project.

## How to write test file
 
``` 
[Fact()]

public void Sampletest(){

    //Arrange
    Calculate cal = new Calculate();

    //Act
    int result = cal.addtwonum(2,3);

    //Assert
    Assert.Equal(5,result);

}
```

## How to run test file

Right click on the test method and then click on the run tests.

## Result

We can see whether the test case has passed or failed.

<img width="314" alt="testcase" src="https://user-images.githubusercontent.com/84472191/234547514-783cacb4-6ae9-4b64-9b20-6278b4681924.PNG">

______________________________________________________________

# Code coverage

Code coverage is the measure of how much code is covered during testing phase.

## Installation of coverlet

Coverlet is a tool which support for line, branch and method coverage.

Install extension [RunCoverletReportVs2022](https://marketplace.visualstudio.com/items?itemName=ChrisDexter.RunCoverletReportVs2022) to generate report. 

Install tool using [coverlet.collector](https://www.nuget.org/packages/coverlet.collector).

## How to generate code coverage report

In visual studio, Go to "tools" -> "run code coverage". 

* Report will be generated automatically based on test cases.

We can see the line coverage and branch coverage percentage in the report.

<img width="444" alt="MicrosoftTeams-image (11)" src="https://user-images.githubusercontent.com/84472191/234776037-0b7c060f-10b8-47aa-b332-871bd57bf0a6.png">


______________________________________________________________


# Code Quality

Code Quality is the measure how well the code is.

## SonarQube 

SonarQube helps to detect code smells, bugs, vulnerabilities, and security vulnerabilities in source code.

## How to download

By docker we can download sonarqube image by this command

```
docker pull sonarqube
```

## SonarQube Integration with DevOps

Install the SonarQube extension for Azure DevOps. 

Go to Azure DevOps "Marketplace" and search for [SonarQube](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarqube). 

## Configure the SonarQube Server 

* Login to your SonarQube server and go to the "Administration" tab.

* Click on "Configuration" and then select "DevOps platform integrations". 

* Add configure with the following details:
   * Name: Azure DevOps
   * URL: https://dev.azure.com/{your organization name}/

* Save the configuration.

## Connection between sonarqube and devOps

In order for Azure DevOps to communicate with SonarQube, you need to generate a SonarQube token. 

Login to your SonarQube server and go to the "My Account" tab. 

Click on "Security" and then "Generate Tokens". Give the token a name and click "Generate". Note down the token value.

After that create a Service Connection in Azure DevOps. Go to your Azure DevOps project and click on "Project Settings". Click on "Service Connections" and then "New Service Connection". 

Select "SonarQube" and enter the following details:

Connection Name: SonarQube
Server URL: https://{your SonarQube server URL}

## Pipeline

Add the SonarQube Build Task to Your Pipeline. Edit your pipeline and add a new task.

Select "SonarQube" from the task list and enter the following details:

SonarQube Endpoint: Select the service connection you created.
Project Key: The unique key for your SonarQube project
Project Name: The name of your SonarQube project
Project Version: The version of your SonarQube project
Additional Properties: Additional properties for the SonarQube analysis

Save the task and run your pipeline. SonarQube analysis will now be integrated into your Azure DevOps pipeline.

## Pipeline YAML code

```
 - task: SonarQubePrepare@5
 displayName: Prepare analysis on SonarQube
 inputs:
 SonarQube: 8cbdbfcd-554d-405b-9466-3f66187d845c
 projectKey: TTAP_Src_TTAP_Src_AYcnpkee2Z48pf9n33Fj
 projectName: TTAP_Src
 extraProperties: "# Additional properties that will be passed to the scanner, \n# Put one key=value per line, example:\n# sonar.exclusions=**/*.bin\nsonar.cs.opencover.reportsPaths=\"$(Agent.TempDirectory)/**/coverage.opencover.xml\""
 - task: DotNetCoreCLI@2
 displayName: Restore
 inputs:
 command: restore
 projects: $(BuildParameters.RestoreBuildProjects)
 - task: DotNetCoreCLI@2
 displayName: Build
 inputs:
 projects: $(BuildParameters.RestoreBuildProjects)
 arguments: --configuration $(BuildConfiguration)
 - task: DotNetCoreCLI@2
 displayName: Test
 inputs:
 command: test
 projects: '**/*[Tt]est/*.csproj'
 arguments: --no-build --configuration $(BuildConfiguration) --collect:"XPlat Code Coverage" -- DataCollectionRunSettings.DataCollectors.DataCollector.Configuration.Format=opencover
 - task: SonarQubeAnalyze@5
 displayName: Run Code Analysis
 - task: SonarQubePublish@5
 displayName: Publish Quality Gate Result
```

## Result

After successfull build the results will be uploaded in sonarqube dashboard page. 

______________________________________________________________


# SonarLint Extension
SonarLint extension provide developers with real-time feedback on code quality issues, including bugs, vulnerabilities, and code smells, and suggest improvements based on the rules defined in SonarQube.

## Installation

Install the SonarLint Extension. You can do this by opening Visual Studio and navigating to the "Extensions" menu. Click on "Manage Extensions" and search for [SonarLint](https://marketplace.visualstudio.com/items?itemName=SonarSource.SonarLintforVisualStudio2022) 

## Configuration

Open Visual Studio and navigate to "Extensions" -> "SonarLint" -> "Connected Mode" -> "Bind to sonarqube" 

Enter the following details:

SonarQube Server URL: The URL of your SonarQube server
Username/token: Login username of your SonarQube server
Password: Login Password of your SonarQube server

Save the settings.

## Result

SonarLint will now analyze your code. You can view the issues like bugs, errors, code smells in the "SonarLint Results" window.


