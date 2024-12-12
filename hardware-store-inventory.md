---
title: "Building a Hardware Store Inventory Web App in Mendix"
linktitle: "Build a Hardware Store Inventory Web App in Mendix"
---

## 1 Introduction

This guide will walk you through creating a hardware store inventory app using Mendix Studio Pro. You will learn how to:

- Create a domain model
- Design a user interface using Mendix’s building blocks and widgets
- Implement custom logic using microflows

By the end of this tutorial, you will have a functional inventory management web application that allows you to view, add, update, and delete hardware items from the database.

## 2 Prerequisites

Before starting this tutorial, ensure the completion of these prerequisites:

- [**Mendix account**](https://signup.mendix.com/): It is free and only takes two minutes to create
- [**Mendix Studio Pro**](https://marketplace.mendix.com/link/studiopro): Download and install the software

## 3 Creating Your App

Mendix provides starter app templates to help users get started. Follow these steps to get started:

1. Open **Mendix Studio Pro** on your computer
2. Click **Create New App** to access the available starter templates
3. Select the **Blank Web App** template and click **Use This Starting Point**
4. In the **App Settings** form:
   1. Enter a descriptive name for your app
   2. Enable **Online Services** by selecting “Yes”
5. Click **Create App**. After some minutes, your app will be created and opened

Now, begin building the application.

## 4 Creating the Domain Model

The first step in building the app is creating a domain model. In Mendix, the [domain model](https://docs.mendix.com/refguide/domain-model/) is an object-relational mapping framework for storing data—it defines the app’s data structure.
For this inventory app, you will create a **Tools** [entity](https://docs.mendix.com/refguide/entities/) to manage hardware items, with [attributes](https://docs.mendix.com/refguide/attributes/) for the **Name** and **Code** of each tool. Follow these steps to create the domain model:

1. Open your app’s domain model by double-clicking the module in the **App Explorer**
2. Drag a new **Entity** from the Toolbox into the domain model canvas
3. Double-click the new entity and rename it to **Tools.** The entity will appear in blue, indicating it is persisted and will be stored in the database
4. Under the **Attributes** tab, click **New** to add the following attributes:
   1. **Name** (String)
   2. **Code** (String)
5. Click **OK** to save the configuration

Your domain model now has an entity for managing the hardware items.

### 4.1 Creating Overview and Detail Pages

Follow these steps to create an overview and detail pages for your entity automatically:

1. Right-click on the **Tools** entity and select **Generate Overview Pages**
2. Click **OK** to generate two pages:
   1. **Tools_NewEdit**: A form for adding and editing tools
   2. **Tools_Overview**: A page displaying a list of tools with options to edit or delete them

You will use these pages later, but first, create a landing page for the application.

## 5 Designing the User Interface (UI)

The UI determines how users interact with the application. Using Mendix’s [building blocks](https://docs.mendix.com/refguide/building-block/) and [widgets](https://docs.mendix.com/appstore/widgets/), you will create the following interfaces and pages:

1. The home page
2. The Tools page

### 5.1 Creating the Home Page

Using the Mendix **Blank Web App** template automatically creates the home page in the **Home_Web** page in the app. However, this is in its basic form. To transform it into your desired form, follow these steps:

1. Open the pre-created **Home_Web** page using the App Explorer
2. Delete the placeholder text widget containing the “getting started” instructions
3. Drag and drop a **Card Action** building block from the **Toolbox** onto the page
4. Rename the card to **Tools**
5. Right-click the button and select **Edit on click action**
6. Set the **On click action** to **Show a page**
7. Click **New** in the **Select web page** dialog to create a new page:
   1. Choose the **Responsive (web)** tab
   2. Select **Lists** and choose the **List** template.
   3. Name the page **Tools_Overview_Main** and click **OK**.

This new page will display a read-only list of tools. It’s distinct from the **Tools_Overview** page, which allows for viewing as well as editing and deleting.

### 5.2 Setting up the Tools page

1. Open the **Tools_Overview_Main** page and edit the “Page Header Title” to read "Tools"
2. Update the supporting text below the title to read: "View all tools available in the system"
3. Populate the list with the database data:
   1. Double-click the **List View** component to open a dialogue
   2. Switch to the **Data Source** tab and select the **Tools** entity created earlier
   3. Click **OK** and choose **Yes** when prompted to auto-fill the list

At this point, your view should look like this:
![](/images/tools-overview-main.png)

The view above shows that your data will be displayed in the following format: **Hammer18282992**, where "Hammer" is the tool's name and "18282992" is the associated code. This format may be confusing for users. To enhance clarity, customize the list view following the steps below:

1. Drag a **Layout Grid** widget with a 6,6 column configuration into the list view
2. Add two **Text** widgets, one in each column:
   1. Set the first to "Name" and the second to "Code"
   2. Use **Heading 5** as the render mode for both
3. Add another 6,6 layout grid below the above grid and drag the `{Name}` and `{Code}` attributes into the respective columns.

The list view should now display tools clearly, with separate columns for names and codes. Like so:

![](/images/updated-tools-overview-main.png)

Now, add a custom logic to validate inputs using microflow.

## 6 Adding a Validation Microflow

[Microflows](https://docs.mendix.com/refguide/microflows/) allow users to define app logic visually. For this app, you will create microflows to:

- Prevent adding empty items to the database.
- Notify users when an item is successfully added.

To get started, open the **Tools_NewEdit** page. This page will look like the image below:

![](/images/Tools_NewEdit-page.png)

1. Right-click the **Save** button and select **Generate Validation Microflow.** This generates two microflows: **ACT_Tools_NewEdit** and **VAL_Tools_NewEdit**
2. Open **ACT_Tools_NewEdit.** At this stage, this microflow will look like so:

![](/images/Tools_NewEdit-microflow.png)

1. Double-click the **Commit 'Tools'** action and enable **Refresh in Client** to ensure updates are visible immediately
2. Drag a **Show Message** action activity between the **Commit 'Tools'** and **Close Page** actions
3. Double-click the **Show Message** activity and configure it:
   1. Set the **Type** to "Information"
   2. Set the message to "Tool saved successfully"
4. Update the **End Event** to remove suggestions (e.g., `$IsValid`) and set the return type to **Nothing**

At this point, the updated microflow should look like the below:

![](/images/updated-Tools_NewEdit-microflow.png)

The next step in building this web application is to add navigation.

## 7 Finishing up

The [navigation](https://docs.mendix.com/refguide/navigation/) defines the app structure for the users. To connect all pages and provide seamless navigation:

1. Open the **Navigation** in the **App Explorer**
2. Add two menu items - “Tools” and “Tools Editor”
3. Configure each menu item: 1. Set the **On Click** action to **Show Page** and select the appropriate page 1. **Tools:** Links to the **Tools_Overview_Main** page 2. **Tools Editor:** Links to the **Tools_Overview** page 2. For **Close Pages**, set it to "All" to ensure a clean navigation experience.

![](/images/navigation.png)

Now, the app is ready to be tested and used.

## 8 Testing Your App

Run your app locally by pressing **F5** or clicking **Run Locally**. You can also publish the app on Mendix Cloud by clicking **Publish** from the Mendix Studio app.
Test the following features:

- Add a new hardware item
- View the list of existing items
- Edit or delete an item from the list

A visual flow of the inventory web application will look like this:

![](/images/app-visual-flow.gif)

Congratulations! You have successfully built a hardware store inventory app in Mendix.

## 9 Read More

Explore these additional resources to learn more about building with Mendix:

- [Customize styling](https://docs.mendix.com/howto/front-end/customize-styling-new/) in Mendix
- How to build a [responsive web app](https://docs.mendix.com/quickstarts/responsive-web-app/)
- Mendix Academy course on [becoming a Rapid Developer](https://academy.mendix.com/link/paths/31/Become-a-Rapid-Developer) for new Mendix users
- Mendix Academy’s [crash course](https://academy.mendix.com/link/paths/82/Crash-Course) for new Mendix users who are also experienced developers
- Mendix [development best practices](https://docs.mendix.com/refguide/dev-best-practices/) guide
