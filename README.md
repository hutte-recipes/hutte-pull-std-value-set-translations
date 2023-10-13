# Hutte Recipe - Pull and Commit Standard value set translations

In this example, the pull/retrieval of `Standard Value Set Translations` through the Salesforce CLI cannot be successfully achieved through commands which uses source tracking, more information can be seen in the [known issue](https://issues.salesforce.com/issue/a028c00000qQ0VAAA0/unable-to-retrieve-the-standardvaluesettranslation-values-using-cli). This limitation impacts Hutte's logic for pull changes of this metadata type.

As a workaround, we can use Hutte custom buttons to automate this operation, by providing the custom logic for the pull and commit of this metadata type as a custom button in `hutte.yml` file.

![](./docs/example.jpg)

## Prerequisites

- a valid Sfdx Project
- a `hutte.yml` file (e.g. the default one shown in the `CONFIGURATION` tab)

## Steps

The following assumes that we use the `config` directory to store a configuration file for `sfdx-browserforce-plugin`.

### Step 1

- Edit the `hutte.yml` file in your default branch
- Add the following lines to the `custom_scripts`
- Push it to your main branch

```yaml
custom_scripts:
  # This scripts will be displayed on the scratch org's page
  scratch_org:
    'Pull Standard Value Set Translations':
      description: "Import data using Snowfakery"
      run: |
        sf project retrieve start --metadata 'StandardValueSetTranslation:*' --target-org "${SALESFORCE_USERNAME}"
        git add .
        git commit -m "chore: standard value set translations"
        git push
```

Note 1: If your project still uses `sfdx`, replace the `sf` command by `sfdx force:source:retrieve --metadata 'StandardValueSetTranslation:*' --targetusername "${SALESFORCE_USERNAME}"`
Note 2: This custom button can also be added to sandboxes page in Hutte, for that, add the button to `sandbox` instead of `scratch_org`

### Step 2

- Create a Scratch Org
- Pull the main changes of your development (any other change than value set translations)
- Use the `Pull Standard Value Set Translations` to pull and commit the standard value set translations
