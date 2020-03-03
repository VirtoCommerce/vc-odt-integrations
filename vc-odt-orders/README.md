# Orders integration solution

## Solution description

Solution contain 3 projects:

1. **vc-odt-read-orders-xlsx** - Logic App for read orders from xlsx.
Test file with orders placed in SharePoint\VirtoCommerce\Training\ODT\odtorders.xlsx

2. **vc-odt-import-orders** - Logic App for parse orders list and create single order object ready for VirtoCommerce platform V3 orders creation API.

3. **vc-odt-create-order** - Logic App for single order creation in VirtoCommerce platform V3.

Logic App calls 'api/order/customerOrders API method for new order creation.

For deploy vc-odt-create-order you have to specify ARM parameters:

* urlParam - VirtoCommerce platform V3;
* usernameParam - user, who have access rigts to create order;
* passwordParam - user password.
