Directory

Important Information
- [JSON Functions](#json-functions)
  - [$jsonParse](#jsonparseparsing)
  - [$jsonSetString](#jsonsetstringstringvalue)

- [/start](#start)
  - [Setting up your /start command](#setting-up-your-start-command)
- [Setting Up After /start](#setting-up-after-start)
  - [Version Checking](#version-checking)
  - [/update](#update) 

Guides
- [Economy and Items With Json](#economy-and-items-with-json)
  - [/balance](#balance)
  - [/inventory](#inventory)
  - [/buy & /sell](#sell--buy)
# BDFD Variables with JSON
Bot Designer for Discord is an excellent app but one issue you may encounter making your bot is the variable limit, this guide will introduce you to using JSON with variables to be able to save yourself a massive amount of variables.

This guide will explain how to setup JSON variables, update users variables, setup an economy system and a levels system + a little extra. I hope its easy to understand and that this guide helps you on your bot developing journey. 

# JSON Functions
### $jsonParse[parsing]
The first step to using Json in any way with BDSript2 is with `$jsonParse[]` this Function takes one input which is your JSON Code. But do not worry for you will not need to code any JSON while using this guide. <br> <br> in this guide we will be storing our JSON code in variables. which would look like this.
```
$jsonParse[$getVar[VARNAME;$authorID]]
```
For example if we have a variable called items and in it we have stored apples, oranges and pears then `$getVar[items;$authorID]]` would input the json string for these items into `$jsonParse` allowing us to modify them as we please with `$jsonSetString`. <br>
### $jsonSetString[string;value]
`$jsonSetString` will be the Function you use most in these examples and allows you to modify your values with no JSON knowledge at all! for example in the code shown below we will create a variable in the app called `items` set the value of this variable to `{}` by default for now. We will now add apples, oranges and pears and how many of each item the user has with the code shown below. <br>
(Slash Command - /test)
```
$jsonParse[$getVar[items;$authorID]]
$jsonSetString[apples;1]
$jsonSetString[oranges;2]
$jsonSetString[pears;3]
$setVar[items;$jsonStringify;$authorID]

variable set
```
*you may notice here that we use `$setVar[items;$jsonStringify;$authorID]` to save the JSON code, the Function `$jsonStringify` will output the parsed json code along with any changes that have been made to it since it was parsed, this is important to remember!* <br>

### $json[string]
Now that you have set your JSON code you can use `$json` to get whatever values you have stores, in this example we are going to use `$json` to get how many apples, oranges and pears we have. <br>
(Slash Command - /inventory)
```
$jsonParse[$getVar[items;$authorID]]

Inventory
$json[apples] Apples
$json[oranges] Oranges
$json[pears] Pears
```
![image](2023-06-08_172710848.png)
## /start

### Setting up your /start command

## Setting up After /start

### Version Checking

### /update

# Economy and Items With Json
In this example the variables you will need are as listed <br> `Name: cash, Value: 0` <br> `Name: items, Value: {}`

### /balance

### /inventory

### /sell & /buy
