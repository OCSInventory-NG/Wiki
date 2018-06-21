# Mail Notifications

Mail Notifications have been added in release 2.5 of OCS Inventory. This feature allow you to configure and customize your report notification.

Click on _Config_ then _Notifications_.

## Configuration

The first page is the configuration's page.

* Activate or not notification mail.

![Notification follow](../../img/server/reports/notification_1.png)

* Set Administrator's email address and name.

![Notification follow](../../img/server/reports/notification_2.png)

* Set Reply's email address and name. (Optional)

![Notification follow](../../img/server/reports/notification_3.png)

* Configure SMTP : email send mode (_SMTP_ - _SMTP + SSL_ - _SMTP + TLS_), Host and Port.

![Notification follow](../../img/server/reports/notification_4.png)

* Configure SMTP optional : User identifier and password.

![Notification follow](../../img/server/reports/notification_5.png)

* Configure recurrence : reception time and day(s).

![Notification follow](../../img/server/reports/notification_6.png)

## Customization template

The second page allows you to customize your template.

By default the OCS Inventory template is selected.

![Notification follow](../../img/server/reports/notification_7.png)

If you choose _Customize template_, you can upload an html file with your custom template. It will be uploaded to the folder

    ocsreports/templates

![Notification follow](../../img/server/reports/notification_8.png)

In your OCS Inventory report you can use traduction files with 

    {{g.number_of_the_traduction}}
    Example : {{g.49}} = Name

At the moment the notification report only offers two choices :

    {{Report.Software}} - Number of softwares per software category
    {{Report.Asset}} - Number of machines per asset category
