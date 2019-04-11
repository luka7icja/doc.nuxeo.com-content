---
title: Configure Nuxeo Platform
description: This tutorial introduces you to a few important aspects of Nuxeo Studio so that you can take advantage of this great product as quickly as possible.
review:
  comment: ''
  date: '2019-02-18'
  status: ok
labels:
  - studio
  - customization
  - lts2017-ok
  - ktouchie
  - howto
toc: true
tree_item_index: 200
---

This tutorial introduces you to a few important aspects of **Nuxeo Studio** so that you can take advantage of this great product as quickly as possible.

## Introduction

BigCorp has a very simple workflow on a `BCContract` document type, called `BCContractCancel_WF`, allowing any BigCorp employee to cancel a contract. Once launched, the request will pass through a manager to confirm the cancellation.

BigCorp needs a new feature added to this workflow: have someone from Accounting specify the amount to refund to the client, with a full refund automatically granted to cancellations that occur within 10 days of signing.

In the steps below, we will show you how to add custom schemas, automation chains and workflow steps to fulfill this feature request.

## Setting Up

First, we'll set up the Nuxeo Platform so it's ready for you to configure.

### Nuxeo Platform

DOWNLOAD AND REGISTER THE NUXEO PLATFORM

1.  [Download Nuxeo Platform](https://www.nuxeo.com/downloads/) and unzip the archive.

1.  Start Nuxeo Server:

{{#> panel heading='**MAC OS** / **Linux**'}}

```bash
$ chmod +x ./bin/nuxeoctl
$ ./bin/nuxeoctl start
```

{{/panel}}

{{#> panel heading='**Windows**'}}

```bash
> .\bin\nuxeoctl.bat start
```

{{/panel}}

### Wizard

1.  Open Chrome or Firefox and navigate to `http://NUXEO_SERVER/nuxeo`.

1.  Follow the steps in Nuxeo Platform Wizard to complete the installation.

1.  Use your **Nuxeo Online Services** credentials to register your instance. If you don't have a **NOS** account yet, you can sign up for your 30-day free trial through the wizard.

1.  Be sure to select the addons needed for this Tutorial:

    - DAM
    - WEB UI

1.  The server will restart at the end. While you're waiting for your server to restart, install [Nuxeo Dev Tools]({{page version='' space='nxdoc' page='nuxeo-dev-tools-extension'}}) for Chrome or Firefox.

### Template

INSTALL THE GETTING STARTED TEMPLATE

1.  Log into Nuxeo Platform with the credentials _Administrator_ / _Administrator_.

1.  Open [Nuxeo Dev Tools]({{page version='' space='nxdoc' page='nuxeo-dev-tools-extension'}}) and under **Useful Links**, click on **Go To Studio Project**.

1.  Log into Studio with your **NOS** credentials.

1.  Under the **Configuration** menu in Studio, select **External Templates** and install the **Nuxeo Training: Getting Started** template by clicking on **Import this package**.

1.  From Nuxeo Platform, open [Nuxeo Dev Tools]({{page version='' space='nxdoc' page='nuxeo-dev-tools-extension'}}) again and click the **Hot Reload** button to update your instance with configurations from the template.
    {{#> callout type='warning' }}
    You must have Administrator access and Dev Mode should be activated to use the hot reload.
    {{/callout}}

### Users and Groups

CREATE SAMPLE USERS

1.  Click on **Administration** ![Administration]({{file space='nxdoc' page='web-ui-overview' name='administration.png'}} ?w=25), then **Users & Groups**.

1.  Click on **New** to create two new Users, **Jon Doe** and **Jane Smith**, setting passwords that you'll remember.

1.  Create a Group, **managers** and add Jane Smith to the group, then create a group **accounting** and add Jon Doe.

### Custom Documents

CREATE SAMPLE DOCUMENTS

Nuxeo Studio allows you to create your own document types and customize the metadata for each one. Let's take a look at one of the customized document types that we created for this tutorial.

1.  Click on **Browse** ![Browse]({{file space='nxdoc' page='web-ui-overview' name='browse.png'}} ?w=25) and select **Domain** > **Workspaces** in the drawer panel from the side menu.

1.  Click on the **Create** ![]({{file version='' space='nxdoc' page='web-ui-overview' name='create_button.png'}} ?w=25) button and create a workspace named **BigCorp**.

1.  Click on ![]({{file version='' space='nxdoc' page='web-ui-overview' name='create_button.png'}} ?w=25) again to create a BC Portfolio with the following metadata:

  <div class="table-scroll">
  <table class="hover">
    <tbody>
      <tr>
        <td>**Title**</td>
        <td>Generic Company</td>
      </tr>
      <tr>
        <td>**Company Size**</td>
        <td>(Select **Medium**)</td>
      </tr>
      <tr>
        <td>**Company Industry**</td>
        <td>(Select **Information Technology**)</td>
      </tr>
      <tr>
        <td>**First Name**</td>
        <td>Bob</td>
      </tr>
      <tr>
        <td>**Last Name**</td>
        <td>Jones</td>
      </tr>
      <tr>
        <td>**Tel**</td>
        <td>8765432</td>
      </tr>
      <tr>
        <td>**Email**</td>
        <td>bjones@company.com</td>
      </tr>
        <td>**Upload**</td>
        <td>Upload a logo of your choice!</td>
      </tr>
    </tbody>
  </table>
  </div>

1.  On the Permissions tab, give both groups the Read & Write permission on this document.

1.  From within your created portfolio, click ![]({{file version='' space='nxdoc' page='web-ui-overview' name='create_button.png'}} ?w=25) again. You'll notice that the only document we can create in a BC Portfolio is **BC Contract**.
    {{#> callout type='tip' }}
    To modify this constraint, under the **Configuration** menu in Studio, select **Content Model** > **Document Types** > **BC Portfolio**. You can then add other document types to the **Accepted Children Types** on the **Definition** tab.
    {{/callout}}

1.  Create a contract with the following metadata:

<div class="table-scroll">
  <table class="hover">
    <tbody>
      <tr>
        <td>**Title**</td>
        <td>GC Contract 1</td>
      </tr>
      <tr>
        <td>**Type**</td>
        <td>(Select **Fixed fee contract**)</td>
      </tr>
      <tr>
        <td>**Contract Owner**</td>
        <td>(Select **Jon Doe**)</td>
      </tr>
      <tr>
        <td>**Signature Date**</td>
        <td>February 28, 2017</td>
      </tr>
      <tr>
        <td>**Start Date**</td>
        <td>March 15, 2017</td>
      </tr>
      <tr>
        <td>**Expiration Date**</td>
        <td>March 15, 2018</td>
      </tr>
      <tr>
        <td>**Amount**</td>
        <td>25000</td>
      </tr>
    </tbody>
  </table>
</div>

You should have something that looks like this:

![BC Portfolio with BC Contract]({{file name='bcportfolio_with_contract.png'}} ?w=600)

Now we can tinker with our Workflow!

## Schemas

CONFIGURE A NEW FIELD ON A CUSTOM DOCUMENT

First we'll need to add a metadatum to our schema to represent the **amount to be refunded**.

1.  In Studio, under the **Configuration** menu, select **Content Model** > **Document Types** > **BCContract**, then click on the **Schema** tab.

1.  Add a new field to the schema: `refundAmount` as a Floating point.
    {{#> callout type='tip' }}
    This field is directly related to the **BC Contract** document type, but if you want to create more general     properties to be shared with other document types you can add them to a separate schema (**Configuration** >    **Content Model** > **Schemas**), or create your own.
    {{/callout}}

1.  Save your changes and click on the **Designer** button.
1.  Go to **Local Types** > **BCCONTRACT**.
1.  In the **view** layout, click **Edit**.
1.  Drag and drop the new property `refundAmount` from **Catalog** in the right menu to the Main View and then save.

{{#> callout type='tip' }}
Using the code editor ![]({{file name='code_editor.png'}} ?w=25), you can drag and drop the property into a Polymer `dom-if` template. The one we've created will only appear in the document view layout if the contract has been cancelled.
{{/callout}}

```html
<template is="dom-if" if="[[document.properties.bccontract:cancelled]]">
  <div role="widget">
    <label>Refund Amount</label>
    <div>[[document.properties.bccontract:refundAmount]]</div>
  </div>
  <div role="widget"><nuxeo-tag class="cancelled">Cancelled</nuxeo-tag></div>
  <div role="widget">
    <label>Cancellation Date</label>
    <div>[[formatDate(document.properties.bccontract:cancellationDate)]]</div>
  </div>
  <div role="widget">
    <label>Refund Amount</label>
    <div>€ [[document.properties.bccontract:refundAmount]]</div>
  </div>
</template>
```

## Automation Chains

CREATE A SIMPLE AUTOMATION CHAIN

Now let's create two simple automation chains to handle each refund case.

1.  In **Studio** under **Automation** > **Automation Chains**, click on **New** to create a new automation chain, `BCContract_CancelFullRefund_AC`.

1.  Drag and drop the **Document** > **Document.SetProperty** operation to the chain.

1.  Reference the property to be set using the XPath, `bccontract:refundAmount`.

1.  Reference the original contract amount (`@{Document.getProperty("bcsalescommon:amount")}`) as the value in order to perform a **full refund**.

1.  Save your automation chain, then create another, `BCContract_CancelPartialRefund_AC`. This time instead of the full amount, we'll reference a workflow variable, `@{WorkflowVariables["refundAmount"]}` which we'll declare in the next step.

## Workflows

CONFIGURE A WORKFLOW

1.  Under **Workflow** > **Process Definitions**, select **BCContractCancel_WF**.

1.  On the **Variables** tab, add `refundAmount` as a **Floating Point** value.

1.  On the **Graph** tab, under the **Automated tasks** menu, drag and drop a **Node** to the graph area, then click ![]({{file name='editor_area.gif' space='studio' page='studio-icons-index'}}) to edit it.

1.  Entitle the node, **Check Signature Date**.

1.  On the **Transitions** tab, add the transition `refundFull`. We only want to run this transition if the contract was signed 10 or fewer days ago, so we'll add an expression to evaluate that condition:

    ```java
    (CurrentDate.time - Fn.calendar(Document["bccontract:signatureDate"]).time) <= 864000000
    ```

1.  Select the `BCContract_CancelFullRefund_AC` automation chain for this transition.

1.  Create a second transition, `refundPartial` with a condition that checks for contracts more than 10 days ago, then save your node definition.

1.  From the **User Tasks** menu, create a **Simple Task** entitled **Enter Refund Amount**.

1.  BigCorp wants all partial refund amounts to be determined by someone on their accounting team, so we'll add `group:accounting` to the field **Assignees**.

1.  On the Variables tab, ensure that the refundAmount variable is editable in this task.

1.  On the Variables tab, ensure that the refundAmount variable is editable in this task.
    On the **Transitions** tab, add a `confirmRefund` button, then link the confirm button to the `BCContract_CancelPartialRefund_AC` automation chain.

1.  On the Variables tab, ensure that the refundAmount variable is editable in this task.
1.  Finally, reorganize the nodes and transitions to implement our new logic, and save. You should have something like this:

![workflow]({{file name='workflow-final.png'}} ?w=450)

### Task Layouts

CONFIGURE THE DIALOG FOR EACH TASK

A few final touches to customize user interaction with our workflow.

1.  In **Designer**, go to **UI** > **Layouts** to open up our workflow layouts.

1.  Click on the **Enter Refund Amount** task layout to configure it.

1.  From the catalog, select **task** > **variables** > **refundAmount : double** > **edit** to add the input widget, then save.

Return to Nuxeo Platform and perform another hot reload to deploy all your changes. Try creating documents with different signature dates and launching the **Cancel Contract** process from each one. You will have to log in as the other users you've created in order to complete all the tasks.

![Start Process]({{file name='start_process.png'}} ?w=600)

## Search

BigCorp wants to personalize a Search form in order to perform fulltext searches on contracts only and filter the results by size (amount).

### Page Providers

CREATE A PAGE PROVIDER FOR A CUSTOM DOCUMENT

1.  In Studio, select **Page Providers**, then **New**.

1.  We'll give our page provider the ID `contract`. To filter our custom document type `BCContract`, we'll add to the existing query filter:
    ```sql
    AND ecm:primaryType = 'BCContract'
    ```

    {{#> callout type='tip' }}
    You can add to the query filter to further tailor your search results. Try filtering out contracts that     haven't been cancelled!
    {{/callout}}

1.  Under **Predicates**, click **Add**. Click on **Edit binding** and select `schema:system` > `fulltext`.

1.  Next to **Aggregates**, click **Add** and select **Range** from the proposed Aggregate Types.

1.  Bind the `bcsalescommon:amount` field to the Aggregate, then add some ranges. For example:

  <div class="table-scroll">
    <table class="hover">
      <tbody>
        <tr>
          <th>Label</th>
          <th>From</th>
          <th>To</th>
        </tr>
        <tr>
          <td>Basic (up to €1,000)</td>
          <td>0</td>
          <td>1,000</td>
        </tr>
        <tr>
          <td>Silver (€1,001 - 5,000)</td>
          <td>1,001</td>
          <td>5,000</td>
        </tr>
        <tr>
          <td>Gold (€5,001 +)</td>
          <td>5,001</td>
          <td></td>
        </tr>
      </tbody>
    </table>
  </div>
1.  Save your Page Provider.

Once you are all set, click on **Configure layouts in Designer** at the top-right of your screen.
You are now in Studio Designer, where you can configure the layouts of your Search.

### Search Drawer

CUSTOMIZE A SEARCH

In the Studio Designer, you are now in the **UI** tab, under **Layouts** > **Page Providers**. You will find the page provider created previously.

1.  Open **form** to configure the form layout.

1.  Drag the search elements from the menu on the right into the form and modify the labels as needed.

1.  Then open **results** and drag columns from the menu on the right to customize your search results layout.

1.  From **Bind more elements**, select **Drawer menu search** and hover over ![]({{file version='' space='nxdoc' page='web-ui-overview' name='create_button.png'}} ?w=25) to select the Search drawer. Then fill in the form:

  <div class="table-scroll">
  <table class="hover">
    <tbody>
      <tr>
        <td>Name</td>
        <td>**Contract**</td>
      </tr>
      <tr>
        <td>Available</td>
        <td>***enabled***</td>
      </tr>
      <tr>
        <td>Label</td>
        <td>**Contract Search**</td>
      </tr>
      <tr>
        <td>Icon</td>
        <td>Select an icon of your choice.</td>
      </tr>
      <tr>
        <td>Order</td>
        <td>Enter a number to indicate the icon's position in the left menu.</td>
      </tr>
      <tr>
        <td>provider</td>
        <td>`contract`</td>
      </tr>
      <tr>
        <td>schemas</td>
        <td>`dublincore`, `bcsalescommon`, `bccontract`</td>
      </tr>
      <tr>
        <td>search-name</td>
        <td>**contract**</td>
      </tr>
      <tr>
        <td>auto</td>
        <td>`enabled`</td>
      </tr>
    </tbody>
  </table>
  </div>

1.  Finally, select **Translations** to modify the range labels. Following our previous example:

```javascript
{
  "label.ui.aggregate.from_0_to_1000":"Basic (up to €1,000)",
  "label.ui.aggregate.from_1001_to_5000":"Silver (€1,000 - 5,000)",
  "label.ui.aggregate.from_5001":"Gold (€5,000 +)"
}
```

Return to Nuxeo Platform, perform a hot reload and your customized search should be available. Create a few contracts and test your new Search.

{{#> callout type='tip' }}
Try customizing your search to filter contract types or owners, then try modifying the columns in your search results layout in order to display more relevant contract information.
{{/callout}}

## Branding

By now, I'm sure you've noticed our custom login page with the ever-conspicuous Big Corp logo. Not a fan? No problem! Let's modify the background image.

### Login Page

CUSTOMIZE THE LOGIN PAGE

1.  In Studio, select **Branding** from the **Configuration** menu, then **New**. Enter a name for your theme.

1.  On the tab **Login Page**, select a background image or a background colors that matches your company's color scheme.

1.  Select a logo to be displayed within the Login box.

1.  Save, hot reload your project and log out of your Nuxeo instance to see the changes.

{{#> callout type='tip' }}
Our teams are working round the clock to improve the themes and styling of Web UI so that you can apply your own branding. Check in again soon to see the improvements we've made and to further customize your Nuxeo application.
{{/callout}}