# Ability Error Codes

> **NOTE**
>
> This topic describes only module-specific error codes. For details about universal error codes, see [Universal Error Codes](errorcode-universal.md).

## 16000001 Ability Name Does Not Exist

**Error Message**

Incorrect Ability name. The specified Ability name does not exist.

**Description**

This error code is reported when the specified ability name does not exist.

**Possible Causes**

The ability to query does not exist.

**Solution**

1. Check whether the bundle name is correct.
2. Check whether the ability name corresponding to the bundle name is correct.

## 16000011 Nonexistent Context

**Error Message**

The context does not exist.

**Description**

This error code is reported when the specified context does not exist.

**Possible Causes**

The context passed in the API does not exist.

**Solution**

Use the correct context.