# Customer Family

For create customer families

## Compatibility
* Thelia >= 2.1.0

## Installation

### Manually

* Copy the module into ```<thelia_root>/local/modules/CustomerFamily``` directory and be sure that the name of the module is CustomerFamily.
* Activate it in your thelia administration panel

### Composer

Add it in your main thelia composer.json file

```
composer require thelia/customer-family-module:~1.0
```

### Adding code in Thelia

The module needs some modifications il Thelia's code to work properly. These issues should be fixed in a future version of Thelia.

#### Registering customers

CustomerFamily uses Thelia's default register.html template and it does not work with the current version of this one. Hopefully there is a workaround to fix this problem. You have to add a `form` argument in the hook called "register.form-bottom" :

```
{hook name="register.form-bottom" form=$form }
```

There is also a bug while submitting the customer registration. To fix it, go to the Front\Controller\CustomerController PHP class, in the `createAction();` method. Replace this line :

```
$customerCreation = new CustomerCreateForm($this->getRequest());
```

by this line :

```
$customerCreation = $this->createForm('thelia.front.customer.create');
```

#### Updating accounts

There are almost the same problems while updating accounts.

First there is a missing `form` parameter in the default account-update.html template's account-update.form-bottom hook :

```
{hook name="account-update.form-bottom" form=$form }
```

There are also some form instantiations bugs which makes the submit ignore CustomerFamily's additional fields. To fix it, you have to use the `BaseController::createForm();` method instead of form contructors. Consequently you have to do those two modifications :

* In `CustomerController::viewAcion();`, replace "`$customerProfileUpdateForm = new CustomerProfileUpdateForm($this->getRequest(), 'form', $data);`" by "`$customerProfileUpdateForm = $this->createForm('thelia.front.customer.profile.update', 'form', $data);`".

* In `CustomerController::updateAcion();`, replace "`$customerProfileUpdateForm = new CustomerProfileUpdateForm($this->getRequest());`" by "`$customerProfileUpdateForm = $this->createForm('thelia.front.customer.profile.update');`".

## Usage

This module is visible in the BackOffice Customer Edit.

## Loop customer_family

This loop returns client families

### Input arguments

|Argument |Description |Version |
|---      |---         |--- |
|**id** | family id | 1.0
|**exclude_id** | exclude family id | 1.0

### Output values

|Argument |Description |Version |
|---      |---         |--- |
|**CUSTOMER_FAMILY_ID** | customer family id | 1.0
|**CODE** | customer family code | 1.0
|**TITLE_CUSTOMER_FAMILY** | customer family title | 1.0

### Exemple
```
{loop type="customer_family" name="customer_family_loop" current_product=$product_id limit="4"}
    {$CODE} ({$TITLE_CUSTOMER_FAMILY})
{/loop}
```

## Loop customer_customer_family

This loop returns customer famility for specific cutomer

### Input arguments

|Argument |Description |Version |
|---      |---         |--- |
|**customer_id** | customer id | 1.0
|**customer_family_id** | family id | 1.0

### Output values

|Argument |Description |Version |
|---      |---         |--- |
|**CUSTOMER_FAMILY_ID** | customer family id | 1.0
|**CUSTOMER_ID** | customer id | 1.0
|**SIRET** | siret number | 1.0
|**VAT** | vat number id | 1.0

### Exemple
```
{loop type="customer_customer_family" name="customer_customer_family_loop" customer_id="4"}
    {SIRET}
{/loop}
```

## Form customer_family_customer_create_form

This form extend customer_create_form

### Fields

|Name |Type |Required |Version |
|--- |--- |--- |--- |
|**customer_family_id** | integer | true | 1.0
|**siret** | string | false | 1.0
|**vat** | string | false | 1.0

## Default

By default, two families are created
* Particular
* Professional
