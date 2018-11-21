# Using software categorization

## Introduction

Softwares categorization is a new feature introduced in 2.5.

It's goal is to create and attribute category to softwares depending on regular expressions or software name.

**`Note that software categories are calculated on computer inventory. Look at your prolog frequency to see how many time it will take to have all yours computers processed.`**

## Create a software category

To create a software categories, you need to go in the ```Manage > Software category``` menu.

Click on the ```New category``` tab.

![Create category](../../img/server/reports/software_categories_tab_create.png)

You have to fill the form to create your category, enter the name and if you want to make a sort by OS choose the name of OS (All OS, Windows, Unix or Android).

![Category form](../../img/server/reports/software_categories_category_form_create.png)

## Add regular expression / software name to a category

To add a regular expression to a category, you need to go in the ```Manage > Software category``` menu.

Click on the ```Add software``` tab.

![Add soft tab](../../img/server/reports/software_categories_tab_add_soft.png)

A form will be displayed, asking for two things :
* Category
* Software name / expression

![Add regular exp](../../img/server/reports/software_categories_add_regular_exp.png)

You can also add by name :

![Add by name](../../img/server/reports/software_categories_add_soft_name.png)

**`Note : form will autocomplete with all the software available in OCS Inventory database.`**

When you choose ```Advanced Settings``` you can add by version and publisher :

![Add by name](../../img/server/reports/software_categories_advance_settings.png)

## List software categories

To list software categories, you need to go in the ```Manage > Software category``` menu.
By default, there is no category.

![Empty list soft cat](../../img/server/reports/software_categories_list_empty.png)

When categories are available they are shown as is :

![List soft cat](../../img/server/reports/software_categories_list_categories.png)

## Set default category

In the case, no regular expression match, you need to be able to set a default category.

In the example, we will use a category named ```Default```.

To set a default software category, navigate to the ```Configuration > General configuration``` menu.

Click on the ```Inventory``` tab.

At the bottom of the page, you will see a configuration named ```DEFAULT_CATEGORY```.

![Default cat](../../img/server/reports/software_categories_default_config.png)

Change this value to the desired default category.
