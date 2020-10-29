# Salesforce HealthKit to Health Cloud - Org Customizations Project

> IMPORTANT: These are the customizations for Salesforce Health Cloud org (**min version 228**) which are required in order to integrate iOS Demo Mobile SDK Application + Apple HealthKit app wich can be found [here](https://github.com/ygavrylenko/healthkitdemo).

Following components will be deployed to your org:
- Platform Event Observation_Event__e
- Additional field ExternalId__c for standard Care Observation object
- Custom lightning flow "Create Care Observations from Platform Events"
- Connected App "HealthKitDemo Mobile App"
- Custom Permission Set "HealthKitDemoPS"

> IMPORTANT: Standard health cloud object CareObservation can not be accessed directly by community users (so we can not create observations from Mobile SDK app directly) that's why we are going to use custom platform event Observation_Event__c and process these events using lightning flow into CareObservation objects. This also gives us flexibility to pre-process events, check whether observations already exist (using ).

## Set up your environment

Follow the steps in the [Quick Start: Visual Studio Code for Salesforce Development](https://trailhead.salesforce.com/en/content/learn/projects/quickstart-vscode-salesforce) Trailhead project. The steps include:

- Install Salesforce CLI
- Install Visual Studio Code
- Install the Visual Studio Code Salesforce extensions, including the Lightning Web Components extensi

## How to Deploy Your Changes?

1. Clone this repository:

    ```
    git clone https://github.com/ygavrylenko/healthkitdemo-sfdx.git
    cd healthkitdemo-sfdx
    ```

1. Authorize your Health Cloud org using VSCode extension - Press CMD+Shift+P (For Mac) and Choose "SFDX: Authorize an Org". Alternatively execute following SFDX Command:

    ```
    sfdx force:auth:web:login
    ```

1. Deploy meta-data to your org either by clicking on force-app directory and clicking "Deploy Source to Org" or executing following command (replace username with valid one):

    ```
    sfdx force:source:deploy --sourcepath ./force-app -u username
    ```

1. Assign the **HealthKitDemoPS** permission set to the default user:

    ```
    sfdx force:user:permset:assign -n HealthKitDemoPS -u username
    ```

## Post deployments steps in your org:

1. Put CareObservation related list on corresponding patient's layout to visualize incoming observations

1. Create 2 new CodeSet objects in order to be able to create new Care Observations for HealthKit measurements
- click on New Care Observation, then on Code and add "New Code Set"
- Add one CodeSet with Code **"healthkit-weight"** and another with Code **"healthkit-heartrate"**, specify name you like, you can leave other fields like it is
- Units of measure with UnitCode **"lbs"** and **"bpm"** should already exist in your org, if not also create ones

> IMPORTANT: CodeSet objects are required when creating new Care Observations, mobile app transfers the CodeSet codes as a part of Observation_Event__e (together with UnitCode) and they are used by flow to retrieve corresponding CodeSet and UnitOfMeasure objects to create Care Observations.

1. Choose which patient you want to use for a demo (e.g. Charles Green as Part of Health Cloud - Makana Community) and assign **HealthKitPatientPS** Permission Set to this user! 

1. Go to connected app "HealthKitDemo Mobile App" save Consumer Key, you need to specify it later in the settings of mobille app (bootconfig.plist)

1. Go to overview of communities (in Setup) and copy url of your commnunity endpoint, you need to specify it later in the settings of mobille app (Info.plist)

1. Go now to the second part of setup for [Salesforce HealthKit Demo Mobile App](https://github.com/ygavrylenko/healthkitdemo)

## Read All About It

- [Salesforce Extensions Documentation](https://developer.salesforce.com/tools/vscode/)
- [Salesforce CLI Setup Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_intro.htm)
- [Salesforce DX Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_intro.htm)
- [Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference.htm)
