## Brands

After creating a FIDEL API account and have access to the dashboard, the next step is to create a test **Brand** so you can create locations and add them to a Program in order to generate transaction data.

The **Brand** object is used to aggregate locations and keep a status of the brand consent. The consent is an signed authorization from the Brand that gives permission to FIDEL to monitor transactions in the Brand's locations. Once you go live you can request the Brand consent form from [developer@fidel.uk](mailto:developer@fidel.uk).

You can only create new locations on live environment from Brands that have signed the consent. On test environment, the consent status is automatically set to signed so you can continue testing.

When you're live you should send the signed brand consent forms to [developer@fidel.uk](mailto:developer@fidel.uk). Usually we approve and set the Brand to signed status in less that 24h.

# Create Brand
To create a new Brand, go to the **Brands** page on the dashboard, click the **+** icon and enter a name. Optionally, you can add a link to the brand logo.

<h5>Go to the Brands page on the dashboard to create a new brand.</h5>

![Create brand](https://docs.fidel.uk/assets/images/create-brand.png "Create brand")
