---
layout: "selectel"
page_title: "Selectel: selectel_iam_serviceuser_v1"
sidebar_current: "docs-selectel-resource-iam-serviceuser-v1"
description: |-
  Creates and manages a service user for Selectel products using public API v1.
---

# selectel\_iam\_serviceuser\_v1

Creates and manages a service user using public API v1. Selectel products support Identity and Access Management (IAM). For more information about service users, see the official Selectel documentation.

The selectel_iam_serviceuser_v1 resource replaces deprecated `selectel_vpc_user_v2` and `selectel_vpc_role_v2` resources. For additional information, see the [Upgrading Terraform Selectel Provider to version 5.0.0](https://registry.terraform.io/providers/selectel/selectel/latest/docs/guides/migrating_to_iam_serviceuser) guide.

~> **Note:** The password of the service user is stored as raw data in a plain-text file. Learn more about sensitive data in state.

## Example Usage

```hcl
resource "selectel_iam_serviceuser_v1" "serviceuser_1" {
  name          = "username"
  password    = "password"
  role {
    role_name = "member"
    scope         = "account"
  }
  role {
    role_name = "iam_admin"
    scope         = "account"
  }
}
```

## Argument Reference

* `name` - (Required) Name of the service user.

* `password` - (Required, Sensitive) Password of the service user.

* `role` - (Optional) Manages service user roles. You can add multiple roles – each role in a separate block. For more information about roles, see the [Selectel documentation](ссылка на таблицу).

* `role_name` - (Required) Role name. Available role names are `iam_admin`, `member`, `reader`, `billing`, `object_storage:admin`, and `object_storage_user`.

* `scope` - (Required) Scope of the role. Available scopes are `account` and `project`. If `scope` is `project`, the `project_id` argument is required.

* `project_id` - (Optional) Unique identifier of the associated Cloud Platform project. Changing this creates a new service user.  If `scope` is `project`, the `project_id` argument is required. Retrieved from the selectel_vpc_project_v2 resource. Learn more about Cloud Platform projects.

* `enabled` - (Optional) Specifies if you can create a Cloud Platform Keystone token for the service user. Boolean flag, the default value is `true`. Learn more about Cloud Platform Keystone tokens.

### Roles

To assign roles, use the following values for `scope` and `role_name`:

* Account administrator - `scope` is `account`, `role_name` is `member`;
* Billing administrator - `scope` is `account`, `role_name` is `billing`;
* User administrator - `scope` is `account`, `role_name` is `iam_admin`;
* Project administrator - `scope` is `project`, `role_name` is `member`;
* Account viewer - `scope` is `account`, `role_name` is `reader`;
* Project viewer - `scope` is `project`, `role_name` is `reader`;
* Object storage admin - `scope` is `project`, `role_name` is `object_storage:admin`;
* Object storage user - `scope` is `project`, `role_name` is `object_storage_user`.

## Import

You can import a service user:

```shell
export OS_DOMAIN_NAME=<account_id>
export OS_USERNAME=<username>
export OS_PASSWORD=<password>
terraform import selectel_iam_serviceuser_v1.serviceuser_1 <user_id>
```

where:

* `<account_id>` - Selectel account ID. The account ID is in the top right corner of the [Control panel](https://my.selectel.ru/). Learn more about [Registration](https://docs.selectel.ru/control-panel-actions/account/registration/).

* `<username>` - Name of the service user. To get the name, in the top right corner of the [Control panel](https://my.selectel.ru/profile/users_management/users?type=service), go to the account menu ⟶ **Profile and Settings** ⟶ **User management** ⟶ the **Service users** tab ⟶ copy the name of the user. Learn more about [service users](https://docs.selectel.ru/control-panel-actions/users-and-roles/user-types-and-roles/).

* `<password>` - Password of the service user.

* `<user_id>` - Unique identifier of the service user to import, for example, `abc1bb378ac84e1234b869b77aadd2ab`. To get the ID, in the top right corner of the [Control panel](https://my.selectel.ru/), go to the account menu ⟶ **Profile and Settings** ⟶ **User management** ⟶ the **Service users** tab ⟶ copy the ID under the user name.
