# @ohos.url (URL String Parsing)

> **NOTE**
>
> The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```
import Url from '@ohos.url' 
```
## URLParams<sup>9+</sup>

### constructor<sup>9+</sup>

constructor(init?: string[][] | Record&lt;string, string&gt; | string | URLSearchParams)

A constructor used to create a **URLParams** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| init | string[][] \| Record&lt;string, string&gt; \| string \| URLSearchParams | No| Input parameter objects, which include the following:<br>- **string[][]**: two-dimensional string array<br>- **Record&lt;string, string&gt;**: list of objects<br>- **string**: string<br>- **URLSearchParams**: object<br>The default value is **null**.|

**Example**

```js
let objectParams = new Url.URLParams([ ['user1', 'abc1'], ['query2', 'first2'], ['query3', 'second3'] ]);
let objectParams1 = new Url.URLParams({"fod" : '1' , "bard" : '2'});
let objectParams2 = new Url.URLParams('?fod=1&bard=2');
let urlObject = Url.URL.parseURL('https://developer.mozilla.org/?fod=1&bard=2');
let params = new Url.URLParams(urlObject.search);
```


### append<sup>9+</sup>

append(name: string, value: string): void

Appends a key-value pair into the query string.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Key of the key-value pair to append.|
| value | string | Yes| Value of the key-value pair to append.|

**Example**

```js
let urlObject = Url.URL.parseURL('https://developer.exampleUrl/?fod=1&bard=2');
let paramsObject = new Url.URLParams(urlObject.search.slice(1));
paramsObject.append('fod', '3');
```


### delete<sup>9+</sup>

delete(name: string): void

Deletes key-value pairs of the specified key.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Key of the key-value pairs to delete.|

**Example**

```js
let urlObject = Url.URL.parseURL('https://developer.exampleUrl/?fod=1&bard=2');
let paramsObject = new Url.URLParams(urlObject.search.slice(1));
paramsObject.delete('fod');
```


### getAll<sup>9+</sup>

getAll(name: string): string[]

Obtains all the values based on the specified key.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Target key.|

**Return value**

| Type| Description|
| -------- | -------- |
| string[] | All the values obtained.|

**Example**

```js
let urlObject = Url.URL.parseURL('https://developer.exampleUrl/?fod=1&bard=2');
let params = new Url.URLParams(urlObject.search.slice(1));
params.append('fod', '3'); // Add a second value for the fod parameter.
console.log(params.getAll('fod').toString()) // Output ["1","3"].
```


### entries<sup>9+</sup>

entries(): IterableIterator<[string, string]>

Obtains an ES6 iterator. Each item of the iterator is a JavaScript array, and the first and second fields of each array are the key and value respectively.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| IterableIterator&lt;[string, string]&gt; | ES6 iterator.|

**Example**

```js
let searchParamsObject = new Url.URLParams("keyName1=valueName1&keyName2=valueName2"); 
for (var pair of searchParamsObject.entries()) { // Show keyName/valueName pairs
    console.log(pair[0]+ ', '+ pair[1]);
}
```


### forEach<sup>9+</sup>

forEach(callbackFn: (value: string, key: string, searchParams: this) => void, thisArg?: Object): void

Traverses the key-value pairs in the **URLSearchParams** instance by using a callback.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callbackFn | function | Yes| Callback invoked to traverse the key-value pairs in the **URLSearchParams** instance.|
| thisArg | Object | No| Value of **this** to use when **callbackFn** is invoked. The default value is this object.|

**Table 1** callbackFn parameter description

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | string | Yes| Value that is currently traversed.|
| key | string | Yes| Key that is currently traversed.|
| searchParams | Object | Yes| Instance that invokes the **forEach** method.|

**Example**

```js
const myURLObject = Url.URL.parseURL('https://developer.exampleUrl/?fod=1&bard=2'); 
myURLObject.params.forEach((value, name, searchParams) => {  
    console.log(name, value, myURLObject.params === searchParams);
});
```


### get<sup>9+</sup>

get(name: string): string | null

Obtains the value of the first key-value pair based on the specified key.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Key specified to obtain the value.|

**Return value**

| Type| Description|
| -------- | -------- |
| string | Returns the value of the first key-value pair if obtained.|
| null | Returns **null** if no value is obtained.|

**Example**

```js
let paramsObject = new Url.URLParams('name=Jonathan&age=18'); 
let name = paramsObject.get("name"); // is the string "Jonathan" 
let age = parseInt(paramsObject.get("age"), 10); // is the number 18
```


### has<sup>9+</sup>

has(name: string): boolean

Checks whether a key has a value.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Key specified to search for its value.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the value exists; returns **false** otherwise.|

**Example**

```js
let urlObject = Url.URL.parseURL('https://developer.exampleUrl/?fod=1&bard=2');
let paramsObject = new Url.URLParams(urlObject.search.slice(1)); 
let result = paramsObject.has('bard');
```


### set<sup>9+</sup>

set(name: string, value: string): void

Sets the value for a key. If key-value pairs matching the specified key exist, the value of the first key-value pair will be set to the specified value and other key-value pairs will be deleted. Otherwise, the key-value pair will be appended to the query string.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Key of the value to set.|
| value | string | Yes| Value to set.|

**Example**

```js
let urlObject = Url.URL.parseURL('https://developer.exampleUrl/?fod=1&bard=2');
let paramsObject = new Url.URLParams(urlObject.search.slice(1));
paramsObject.set('baz', '3'); // Add a third parameter.
```


### sort<sup>9+</sup>

sort(): void

Sorts all key-value pairs contained in this object based on the Unicode code points of the keys and returns undefined.  This method uses a stable sorting algorithm, that is, the relative order between key-value pairs with equal keys is retained.

**System capability**: SystemCapability.Utils.Lang

**Example**

```js
let searchParamsObject = new Url.URLParams("c=3&a=9&b=4&d=2"); // Create a test URLSearchParams object
searchParamsObject.sort(); // Sort the key/value pairs
console.log(searchParamsObject.toString()); // Display the sorted query string // Output a=9&b=2&c=3&d=4
```


### keys<sup>9+</sup>

keys(): IterableIterator&lt;string&gt;

Obtains an ES6 iterator that contains the keys of all the key-value pairs.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| IterableIterator&lt;string&gt; | ES6 iterator that contains the keys of all the key-value pairs.|

**Example**

```js
let searchParamsObject = new Url.URLParams("key1=value1&key2=value2"); // Create a URLSearchParamsObject object for testing
for (var key of searchParamsObject .keys()) { // Output key-value pairs
    console.log(key);
}
```


### values<sup>9+</sup>

values(): IterableIterator&lt;string&gt;

Obtains an ES6 iterator that contains the values of all the key-value pairs.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| IterableIterator&lt;string&gt; | ES6 iterator that contains the values of all the key-value pairs.|

**Example**

```js
let searchParams = new Url.URLParams("key1=value1&key2=value2"); // Create a URLSearchParamsObject object for testing
for (var value of searchParams.values()) {
    console.log(value);
}
```


### [Symbol.iterator]<sup>9+</sup>

[Symbol.iterator]\(): IterableIterator&lt;[string, string]&gt;

Obtains an ES6 iterator. Each item of the iterator is a JavaScript array, and the first and second fields of each array are the key and value respectively.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| IterableIterator&lt;[string, string]&gt; | ES6 iterator.|

**Example**

```js
const paramsObject = new Url.URLParams('fod=bay&edg=bap');
for (const [name, value] of paramsObject[Symbol.iterator]()) {
    console.log(name, value); 
} 
```


### tostring<sup>9+</sup>

toString(): string

Obtains search parameters that are serialized as a string and, if necessary, percent-encodes the characters in the string.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| string | String of serialized search parameters, which is percent-encoded if necessary.|

**Example**

```js
let url = Url.URL.parseURL('https://developer.exampleUrl/?fod=1&bard=2');
let params = new Url.URLParams(url.search.slice(1)); 
params.append('fod', '3');
console.log(params.toString());
```

## URL

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| hash | string | Yes| Yes| String that contains a harsh mark (#) followed by the fragment identifier of a URL.|
| host | string | Yes| Yes| Host information in a URL.|
| hostname | string | Yes| Yes| Hostname (without the port) in a URL.|
| href | string | Yes| Yes| String that contains the whole URL.|
| origin | string | Yes| No| Read-only string that contains the Unicode serialization of the origin of the represented URL.|
| password | string | Yes| Yes| Password in a URL.|
| pathname | string | Yes| Yes| Path in a URL.|
| port | string | Yes| Yes| Port in a URL.|
| protocol | string | Yes| Yes| Protocol in a URL.|
| search | string | Yes| Yes| Serialized query string in a URL.|
| params<sup>9+</sup> | [URLParams](#urlparams9) | Yes| No| **URLParams** object allowing access to the query parameters in a URL.|
| username | string | Yes| Yes| Username in a URL.|

### constructor<sup>9+</sup>

constructor()

A no-argument constructor used to create a URL. It returns a **URL** object after **parseURL** is called. It is not used independently.

**System capability**: SystemCapability.Utils.Lang

### parseURL<sup>9+</sup>

static parseURL(url : string, base?: string | URL): URL

Parses a URL.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| url | string | Yes| Input object.|
| base | string \| URL | No| Input parameter, which can be any of the following:<br>- **string**: string<br>- **URL**: string or object<br>The default value is an empty string or an empty object.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200002 | Invalid url string. |


**Example**

```js
let mm = 'https://username:password@host:8080';
let url = Url.URL.parseURL(mm); 
let result = url.toString(); // Output 'https://username:password@host:8080/'
```

### tostring

toString(): string

Converts the parsed URL into a string.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| string | Website address in a serialized string.|

**Example**

```js
const url = Url.URL.parseURL('https://username:password@host:8080/directory/file?query=pppppp#qwer=da');
let result = url.toString();
```

### toJSON

toJSON(): string

Converts the parsed URL into a JSON string.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| string | Website address in a serialized string.|

**Example**
```js
const url = Url.URL.parseURL('https://username:password@host:8080/directory/file?query=pppppp#qwer=da');
let result = url.toJSON();
```
