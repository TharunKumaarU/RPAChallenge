# RPA Challenge – UiPath

A UiPath Studio project for the [RPA Challenge](https://rpachallenge.com/) "Input Forms" task: take the ten records in the challenge spreadsheet and enter them into the web form, whose fields change position after every submit.

## How it works

The spreadsheet holds ten people with seven fields each: First Name, Last Name, Company Name, Role in Company, Address, Email and Phone Number. The workflow loads these records into a DataTable and processes them one row at a time with a For Each Row loop.

The form is the difficult part, because the seven inputs are shuffled on every round, so their screen position and order cannot be relied on. Each input does carry a fixed `ng-reflect-name` attribute, and every selector in this project is built on that attribute. For example, the First Name field is always found with:

```xml
<webctrl tag='INPUT' ng-reflect-name='labelFirstName' />
```

| Spreadsheet column | `ng-reflect-name` |
|---|---|
| First Name | `labelFirstName` |
| Last Name | `labelLastName` |
| Company Name | `labelCompanyName` |
| Role in Company | `labelRole` |
| Address | `labelAddress` |
| Email | `labelEmail` |
| Phone Number | `labelPhone` |

## Workflows

**`Main.xaml`** reads the data and writes each record to the Output panel. It uses Read Text File to load `challenge.csv`, Generate Data Table From Text to build the `dtPeople` DataTable, and a For Each Row loop with a Log Message activity. It is the data-reading check that runs before any browser work.

**`FillForm.xaml`** does the form entry. After the same two data-reading steps, Use Application/Browser opens the challenge page in Chrome, a Click presses Start, and the For Each Row loop runs seven Type Into activities followed by a Click on Submit for each person.

## Status

`Main.xaml` runs successfully and logs all ten records. `FillForm.xaml` validates in Studio without errors; a complete end-to-end run against the live form is still pending.

## Data

`challenge.xlsx` is the original spreadsheet from the challenge site. `challenge.csv` is the same data exported as CSV, which is what the workflows read. This is a cross-platform project built in UiPath Studio on macOS, where the built-in activities read text files; reading the `.xlsx` directly would use Read Range Workbook from the UiPath.Excel.Activities package.

## Running it

1. Open the project folder in UiPath Studio (built with Studio 2026.0, cross-platform, VB expressions).
2. Let Studio restore the packages listed in `project.json`.
3. For `FillForm.xaml`, install the UiPath extension for Chrome from Studio's Tools page.
4. Open the workflow you want and press Run. Logged records appear in the Output panel.
