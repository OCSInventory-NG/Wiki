# Prerequisites on OCS deployment

This chapter will try to list all the requirement needed by the server and agents.

## Server configuration

Having deployment configured properly on the server is not really complicated, you'll have to keep two things in mind : 
* By default deployment is not enabled
* SSL / HTTPS is a mandatory, OCS doesn't send deployment package if the server is not configured with HTTPS

To enable OCS Inventory deployment feature, login with a sadmin user and navigate to the `Configuration -> General configuration` menu.

On the left of the page, click on the `Deployment` tab and set the `DOWNLOAD` settings to ON. 
Don't forget to click on the update button at the bottom of the page otherwise your settings won't be saved.



## Agent configuration


