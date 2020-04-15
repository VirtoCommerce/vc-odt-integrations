# Orders integration solution

## Solution description

Logic App implements a integration scenario with Virto Commerce Box and Mock for 3rd party ERP

Solution contains 4 projects:

1. **vc-odt-read-orders-dynamics** - Logic App for read orders from demo Dynamics ERP.

ARM parameters:

* *nestedLogicAppName* - name for nested LogicApp (default value - integration vc-odt-create-order);
* *organizationNameParam* - encoded Dynamics organization name (default value - org363c224b.crm). 


2. **vc-odt-read-orders-xls** - Mock Logic App for read orders from demo xlsx.

ARM parameters:

* *nestedLogicAppName* - name for nested LogicApp (default value - integration vc-odt-create-order);
* *excelPath* - path for dev, qa and demo environments demo excel file (default value - /Sales/ODT/odtorders_dev.xlsx). Demo Excel file should be placed in “http://virto365.sharepoint.com/sites/VirtoCommerce/Shared Documents” library.

3. **vc-odt-import-orders** - Logic App for parse orders list and create single order object ready for VirtoCommerce platform V3 orders creation API.

ARM parameters:

* *urlParam* - VirtoCommerce platform V3 url;
* *usernameParam* - user, who have access rights to create order;
* *passwordParam* - user password;
* *nestedCreateLogicAppName* - name for nested LogicApp (default value - vc-odt-create-order);
* *nestedUpdateLogicAppName* - name for nested LogicApp (default value - vc-odt-update-order);
* *storeIdParam* - storeId, mandatory field for order (default value - Electronics);
* *catalogIdParam* - catalogId, mandatory field for order item (default value - Electronics);

4. **vc-odt-create-order** - Logic App for single order creation in VirtoCommerce platform V3.

5. **vc-odt-update-order** - Logic App for single order update in VirtoCommerce platform V3.

## Deployment

All LogicApps should be created in the same resource group

For deploy vc-odt-create-order and vc-odt-read-orders-xls you have to specify ARM parameters.

Deploy order:

* **vc-odt-update-order**
* **vc-odt-create-order**
* **vc-odt-import-orders**
* **vc-odt-read-orders-xls**
