# Mail Notifications

Mail Notifications have been added in release 2.5 of OCS Inventory. This feature allow you to configure and customize your report notification.

Click on _Config_ then _Notifications_.

![Notification follow](../../img/server/reports/notification_9.png)

## Configuration

The first page is the configuration page.

* Activate or not notification mail.

![Notification follow](../../img/server/reports/notification_1.png)

* Set Administrator's email address and name.

![Notification Admin email](../../img/server/reports/notification_2.png)

* Set reply email address and name. (Optional)

![Notification Admin name](../../img/server/reports/notification_3.png)

* Configure SMTP : email send mode (_SMTP_ - _SMTP + SSL_ - _SMTP + TLS_), Host and Port.

![Notification SMTP](../../img/server/reports/notification_4.png)

* Configure SMTP : User identifier and password. (Optional)

![Notification SMTP user](../../img/server/reports/notification_5.png)

* Configure recurrence : reception time and day(s).

![Notification recurrence](../../img/server/reports/notification_6.png)

## Customization template

The second page allows you to customize your template.

By default the OCS Inventory template is selected.

![Notification default](../../img/server/reports/notification_7.png)

If you choose _Customize template_, you can upload an html file with your custom template. It will be uploaded to the folder

    ocsreports/templates

![Notification customize](../../img/server/reports/notification_8.png)

In order to have the OCS Inventory report values in your notification, put in your html code the values proposed below :

At the moment the notification report only offers two choices :

    {{Report.Software}} - Number of softwares per software category
    {{Report.Asset}} - Number of machines per asset category

In your OCS Inventory report you can use traduction files with

    {{g.number_of_the_traduction}}
    Example : {{g.49}} = Name

## Change default notification language

The default notification language is English but you can modify this value in

    ocsreports/var.php

Change the default value by your

    /**
     * Default GUI language
     */
    define('DEFAULT_LANGUAGE', 'your_language');

## Configure CronTab

In your server, configure a CronTab to call the file cron_mailer.php in

    ocsreports/tools/cron_mailer.php

**`Note`**`: It's recommended to set the frequency to every minute`
