# Resource Manager Error Codes

> **NOTE**
>
> This topic describes only module-specific error codes. For details about universal error codes, see [Universal Error Codes](errorcode-universal.md).

## 9001001 Invalid Resource ID

**Error Message**

The resId invalid.

**Description**

This error code is reported if the specified resId meets the type requirement but the **resId** does not exist.

**Possible Causes**

The specified **resId** does not exist.

**Solution**

Check whether the specified **resId** exists.

## 9001002 Matching Resource Not Found Based on the Specified Resource ID

**Error Message**

The resource not found by resId.

**Description**

This error code is reported if the specified **resId** meets the type requirement but no resource is found based on the **resId**.

**Possible Causes**

1. The specified **resId** is incorrect.

2. Resource parsing is incorrect.

**Solution**

Check whether the specified **resId** is correct.

## 9001003 Invalid Resource Name

**Error Message**

The resName invalid.

**Description**

This error code is reported if the specified **resName** meets the type requirement but the **resName** does not exist.

**Possible Causes**

The specified **resName** does not exist.

**Solution**

Check whether the specified **resName** is correct.

## 9001004 Matching Resource Not Found Based on the Specified Resource Name

**Error Message**

The resource not found by resName.

**Description**

This error code is reported if the specified resId meets the type requirement but no resource is found based on the **resId**.

**Possible Causes**

1. The specified **resName** is incorrect.

2. Resource parsing is incorrect.

**Solution**

Check whether the specified **resName** is correct.

## 9001005 Invalid Relative Path

**Error Message**

The resource not found by path.

**Description**

This error code is reported if no resource is found based on the specified relative path.

**Possible Causes**

The specified relative path is incorrect.

**Solution**

Check whether the specified relative path is correct.

## 9001006 Cyclic Reference

**Error Message**

the resource re-ref too much.

**Description**

This error code is reported if resources are referenced cyclically.

**Possible Causes**

Resources are referenced cyclically.

**Solution**

Check the reference of resource $, and remove the cyclic reference, if any.

## 9001007 Failed to Format the Resource Obtained Based on resId

**Error Message**

The resource obtained by resId formatting error.

**Description**

This error code is reported in the case of a failure to format the string resource obtained based on **resId**.

**Possible Causes**

1. The parameter type is not supported.

2. The numbers of parameters and placeholders are inconsistent.

3. The parameter does not match the placeholder type.

**Solution**

Check whether the number and type of **args** parameters are the same as those of placeholders.

## 9001008 Failed to Format the Resource Obtained Based on resName

**Error Message**

The resource obtained by resName formatting error.

**Description**

This error code is reported in the case of a failure to format the string resource obtained based on **resName**.

**Possible Causes**

1. The parameter type is not supported.

2. The numbers of parameters and placeholders are inconsistent.

3. The parameter does not match the placeholder type.

**Solution**

Check whether the number and type of **args** parameters are the same as those of placeholders.