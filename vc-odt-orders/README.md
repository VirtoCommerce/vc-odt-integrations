# Orders integration solution

## Solution description

LogicApp implements a integration scenario with Virto Commerce Box and Mock for 3rd party ERP

Solution contains 2 independent parts: export orders from VC and import orders to VC.

### Export orders

1. **vc-odt-export-customer-dynamics** - LogicApp to export VC members to Dynamics contacts. Checks if the contact exists for the specified member, then returns the accountid for it. If the contact is not found, a new one is creates and returns the accountid for it.

ARM parameters:

* *organizationNameParam* - encoded Dynamics organization name (default value - org363c224b.crm).

2. **vc-odt-export-order-dynamics** - LogicApp to export VC order to Dynamics order. Note: totatlamount not calculated now. Returns salesorderid for created order.
ARM parameters:

* *organizationNameParam* - encoded Dynamics organization name (default value - org363c224b.crm).

3. **vc-odt-export-orderproducts-dynamics** - LogicApp to export VC order items to Dynamics order products. Note: in Dynamics creates only OrderProducts, new products will not appear in the Dynamics catalog.

ARM parameters:

* *organizationNameParam* - encoded Dynamics organization name (default value - org363c224b.crm).

4. **vc-odt-export-orders** LogicApp read orders in VC with status Ready to export calls *vc-odt-export-customer-dynamics* *vc-odt-export-orders-dynamics* *vc-odt-export-orderproducts-dynamics* nested LogicApps and if export finished successfully change order status to Exported.

ARM parameters:

* *urlParam* - VirtoCommerce platform V3 url;
* *usernameParam* - user, who have access rights to create order;
* *passwordParam* - user password;
* *nestedExportCustomerLogicAppName* - name for nested LogicApp (default value - vc-odt-export-customer-dynamics);
* *nestedExportOrderLogicAppName* - name for nested LogicApp (default value - vc-odt-export-order-dynamics);
* *nestedExportOrderProductsLogicAppName* - name for nested LogicApp (default value - vc-odt-export-orderproducts-dynamics);

Deploy order:

* **vc-odt-export-customer-dynamics**
* **vc-odt-export-order-dynamics**
* **vc-odt-export-orderproducts-dynamics**
* **vc-odt-export-orders**

### Import orders

1. **vc-odt-read-orders-dynamics** - LogicApp to read orders from demo Dynamics ERP.

ARM parameters:

* *nestedLogicAppName* - name for nested LogicApp (default value - integration vc-odt-create-order);
* *organizationNameParam* - encoded Dynamics organization name (default value - org363c224b.crm).

2. **vc-odt-read-orders-xls** - Mock LogicApp for read orders from demo xlsx.

ARM parameters:

* *nestedLogicAppName* - name for nested LogicApp (default value - integration vc-odt-create-order);
* *excelPath* - path for dev, qa and demo environments demo excel file (default value - /Sales/ODT/odtorders_dev.xlsx). Demo Excel file should be placed in “http://virto365.sharepoint.com/sites/VirtoCommerce/Shared Documents” library.

3. **vc-odt-import-orders** - LogicApp for parse orders list and create single order object ready for VirtoCommerce platform V3 orders creation API.

ARM parameters:

* *urlParam* - VirtoCommerce platform V3 url;
* *usernameParam* - user, who have access rights to create order;
* *passwordParam* - user password;
* *nestedCreateLogicAppName* - name for nested LogicApp (default value - vc-odt-create-order);
* *nestedUpdateLogicAppName* - name for nested LogicApp (default value - vc-odt-update-order);
* *storeIdParam* - storeId, mandatory field for order (default value - Electronics);
* *catalogIdParam* - catalogId, mandatory field for order item (default value - Electronics);

4. **vc-odt-create-order** - LogicApp for single order creation in VirtoCommerce platform V3.

5. **vc-odt-update-order** - LogicApp for single order update in VirtoCommerce platform V3.

## Deployment

All LogicApps should be created in the same resource group

For deploy vc-odt-create-order and vc-odt-read-orders-xls you have to specify ARM parameters.

Deploy order:

* **vc-odt-update-order**
* **vc-odt-create-order**
* **vc-odt-import-orders**
* **vc-odt-read-orders-xls**
* **vc-odt-read-orders-dynamics**
