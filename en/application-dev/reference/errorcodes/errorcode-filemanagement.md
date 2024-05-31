# File Management Error Codes

> **NOTE**
>
> This topic describes only module-specific error codes. For details about universal error codes, see [Universal Error Codes](errorcode-universal.md).

### 13900001 Operation Not Permitted

**Error Message**

Operation not permitted

**Possible Causes**

The current operation on the file is not allowed.

**Solution**

Check the permission for the file.

### 13900002 File or Directory Not Exist

**Error Message**

No such file or directory

**Possible Causes**

The file or directory does not exist.

**Solution**

Check whether the file directory exists.

### 13900003 Process Not Exist

**Error Message**

No such process

**Possible Causes**

The process does not exist.

**Solution**

1. Check whether the process is killed unexpectedly.

2. Check whether the service related to the process has started.

### 13900004 System Call Interrupted

**Error Message**

Interrupted system call

**Possible Causes**

The system call is interrupted by another thread.

**Solution**

1. Check the multi-thread code logic.

2. Invoke the system call again.

### 13900005 I/O Error

**Error Message**

I/O error

**Possible Causes**

The I/O request is invalid.

**Solution**

Initiate the I/O request again.

### 13900006 Device or Address Not Exist

**Error Message**

No such device or address

**Possible Causes**

The device information or address is incorrect.

**Solution**

Check that the device information or address is correct.

### 13900007 Long Parameter List

**Error Message**

Arg list too long

**Possible Causes**

The parameter list is too long.

**Solution**

Reduce the number of parameters.

### 13900008 Invalid File Descriptor

**Error Message**

Bad file descriptor

**Possible Causes**

This file descriptor is closed.

**Solution**

Check whether the file descriptor is closed.

### 13900009 Child Process Not Exist

**Error Message**

No child processes

**Possible Causes**

The child process cannot be created.

**Solution**

Check the maximum number of processes in the system.

### 13900010 Resource Unavailable

**Error Message**

Try again

**Possible Causes**

The resources are blocked.

**Solution**

Request resources.

### 13900011 Memory Overflow

**Error Message**

Out of memory

**Possible Causes**

A memory overflow occurs.

**Solution**

1. Check the memory overhead.

2. Control the memory overhead.

### 13900012 Permission Denied

**Error Message**

Permission denied

**Possible Causes**

1. You do not have the permission to operate the file.

2. The file sandbox path is incorrect.

**Solution**

1. Check that the required permission is available.

2. Check that the file sandbox path is correct.

### 13900013 Incorrect Address

**Error Message**

Bad address

**Possible Causes**

The address is incorrect.

**Solution**

Check that the address is correct.

### 13900014 Device or Resource Not Available

**Error Message**

Device or resource busy

**Possible Causes**

The requested resource is unavailable.

**Solution**

Request the resource again.

### 13900015 File Already Exists

**Error Message**

File exists

**Possible Causes**

The file to be created already exists.

**Solution**

Check whether the file path is correct.

### 13900016 Invalid Cross-Device Link

**Error Message**

Cross-device link

**Possible Causes**

The link between devices is incorrect.

**Solution**

Check the devices and create the link correctly.

### 13900017 Device Not Exist

**Error Message**

No such device

**Possible Causes**

The device is not identified.

**Solution**

Check the connection to the target device.

### 13900018 Invalid Directory

**Error Message**

Not a directory

**Possible Causes**

The specified directory is invalid.

**Solution**

Check that the directory is correct.

### 13900019 The Specified Object Is a Directory

**Error Message**

Is a directory

**Possible Causes**

The specified object is a directory.

**Solution**

Check that the specified data is correct.

### 13900020 Invalid Parameter

**Error Message**

Invalid argument

**Possible Causes**

Invalid input parameter is detected.

**Solution**

Check that the input parameters are valid.

### 13900021 Too Many File Descriptors Opened

**Error Message**

File table overflow

**Possible Causes**

The number of file descriptors opened has reached the limit.

**Solution**

Close the file descriptors that are no longer required.

### 13900022 Too Many Files Opened

**Error Message**

Too many open files

**Possible Causes**

The number of files opened has reached the limit.

**Solution**

Close the files that are not required.

### 13900023 Text File Not Respond

**Error Message**

Text file busy

**Possible Causes**

The executable file of the program is in use.

**Solution**

Close the program that is being debugged.

### 13900024 File Oversized

**Error Message**

File too large

**Possible Causes**

The file size exceeds the limit.

**Solution**

Check whether the file size exceeds the limit.

### 13900025 Insufficient Space on the Device

**Error Message**

No space left on device

**Possible Causes**

The device storage space is insufficient.

**Solution**

Clear the space of the device.

### 13900026 Invalid Shift

**Error Message**

Illegal seek

**Possible Causes**

Seek is used in pipe or FIFO.

**Solution**

Check the use of **seek()**.

### 13900027 Read-Only File System

**Error Message**

Read-only file system

**Possible Causes**

The file system allows read operations only.

**Solution**

Check whether the file is read-only.

### 13900028 Links Reach the Limit

**Error Message**

Too many links

**Possible Causes**

The number of links to the file has reached the limit.

**Solution**

Delete the links that are no longer required.

### 13900029 Resource Deadlock

**Error Message**

Resource deadlock would occur

**Possible Causes**

Resource deadlock occurs.

**Solution**

Terminate the deadlock process.

### 13900030 Long File Name

**Error Message**

Filename too Long

**Possible Causes**

The length of the path or file name exceeds the limit.

**Solution**

Modify the path or file name.

### 13900031 Function Not Implemented

**Error Message**

Function not implemented

**Possible Causes**

The function is not supported by the system.

**Solution**

Check the system version and update the system if required.

### 13900032 Directory Not Empty

**Error Message**

Directory not empty

**Possible Causes**

The specified directory is not empty.

**Solution**

1. Check the directory.

2. Ensure that the directory is empty.

### 13900033 Too Many Symbol Links

**Error Message**

Too many symbolic links

**Possible Causes**

There are too many symbolic links.

**Solution**

Delete unnecessary symbol links.

### 13900034 Operation Blocked

**Error Message**

Operation would block

**Possible Causes**

The operation is blocked.

**Solution**

Perform the operation again.

### 13900035 Invalid File Descriptor

**Error Message**

Invalid request descriptor

**Possible Causes**

The requested file descriptor is invalid.

**Solution**

Check that the file descriptor is valid.

### 13900036 Target Is Not a Character Stream Device

**Error Message**

Device not a stream

**Possible Causes**

The device pointed to by the file descriptor is not a character stream device.

**Solution**

Check whether the file descriptor points to a stream.

### 13900037 No Data Available

**Error Message**

No data available

**Possible Causes**

The required data is not available.

**Solution**

Request the data again.

### 13900038 Value Mismatches the Data Type

**Error Message**

Value too large for defined data type

**Possible Causes**

The specified value is out of the range defined for the data type.

**Solution**

Modify the data type.

### 13900039 File Descriptor Corrupted

**Error Message**

File descriptor in bad state

**Possible Causes**

The file descriptor is corrupted.

**Solution**

Check that the file descriptor is valid.

### 13900040 System Call Interrupted

**Error Message**

Interrupted system call should be restarted

**Possible Causes**

The system call is interrupted.

**Solution**

Invoke the system call again.

### 13900041 Disk Quota Exceeded

**Error Message**

Quota exceeded

**Possible Causes**

The disk space is insufficient.

**Solution**

Clear disk space.

### 13900042 Unknown Error

**Error Message**

Unknown error

**Possible Causes**

The error is unidentified.

**Solution**

1. Call the API again.

2. Restart the service.
