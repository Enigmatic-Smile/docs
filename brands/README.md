# Brands

After creating an account and having access to the dashboard, the next step is to create a test **Brand** so you can create locations and add them to a Program in order to start tracking transactions.

The **Brand** (Merchant) object is used to aggregate locations and keep the status of the Brand consent. One of the requirements, when creating a Brand and before adding locations, is to obtain Brand consent from an authorized personnel (Brand User) and to provide the Brand with access to view the transactional data that is being shared.

You can only create new locations on live environment from Brands that you have invited to create an account, and that have authorised you to access their card linked transactions. On test environment, the consent status is automatically approved so you can continue testing.

## Create Brand

To create a new Brand, go to the **Brands** page on the dashboard, click the **+** icon and enter a name. Optionally, you can add a link to the brand logo.

To automatically request Brand consent, enter the name and email of the Brand User and will send an email inviting the Brand User to create a Brand account at [clo.fidel.uk](https://clo.fidel.uk). By creating an account the Brand User provides consent and will have access to transactions made by cardholders at their participating locations.

You can monitor consent status on the dashboard and also set up a `brand.consent` webhook to be notified when the status changes.

##### Go to the Brands page on the dashboard to create a new brand.

![Create brand](https://docs.fidel.uk/assets/images/create-brand.png "Create brand")
