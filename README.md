# BDFD Variables with JSON
Bot Designer for Discord is an excellent app but one issue you may encounter making your bot is the variable limit, this guide will introduce you to using JSON with variables to be able to save yourself a massive amount of variables.

This guide will explain how to setup JSON variables, update users variables, setup an economy system and a levels system + a little extra. I hope its easy to understand and that this guide helps you on your bot developing journey. 

- **Important Information**
  - [JSON Functions](#json-functions) 
    > [$jsonParse](#jsonparseparsing) <br>
    > [$jsonSetString](#jsonsetstringstringvalue) <br>
    > [$json](#jsonstring) <br>
  - [/start & setting json variables](#start-and-update)
    > [Setting up your /start command](#setting-up-your-start-command) <br>
  - [Setting Up After /start](#setting-up-after-start) <br>
    > [Version Checking](#version-checking) <br>
    > [/update](#update) <br>
- **Basic Guides.**
  - [Adding to JSON Strings](#adding-to-json-strings)
    > [$calculate](#calculate) <br>
    > [/add-cash](#add-cash) <br>
    > [/remove-cash](#remove-cash) <br>
    > [Boosters](#boosters) <br>
  - [Economy and Items With Json](#economy-system)
    > [/balance](#balance) <br>
    > [/work](#work) <br>
    > [/inventory](#inventory) <br>
    > [/buy & /sell](#sell--buy) <br>

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
![image](image_2023-06-08_172710848.png) <br>
*The two blocks of code will output these results.*

# /start and /update
Now that you understand the basics i will explain an important part of JSON variables, which is a /start command and a Version Checker (explained later). The purpose of a `/start` command is to set the users variables with the correct json code before they can use any other command. this ensures no errors occur later down the line as for example a command may add an +1 apple to the user, but if `$json[apple]` doesnt return a number then the code can break. <br>

### Setting up your /start command
To get started make a variable named `started` and set the value to `false`, the /start command we are going to make below will include the items, stats and other json information we will use in the guides below. If you later for example a new item to your bot after you set this command up then it can cause errors unless you have a Version Checker which will be explained later. <br>
(Slash Command - /start) <br>
(Variables: <br>
`Name: stats, Value: {}` <br>
`Name: items, Value: {}`) <br>
```
$if[$getVar[started;$authorID]==true]
$title[you have already used /start]
$else
$jsonParse[$getVar[stats;$authorID]]
$jsonSetString[cash;0]
$jsonSetString[xp;0]
$jsonSetString[lvl;0]
$jsonSetString[lvlR;100]
$setVar[stats;$jsonStringify;$authorID]

$jsonParse[$getVar[items;$authorID]]
$jsonSetString[apples;0]
$jsonSetString[oranges;0]
$jsonSetString[pears;0]
$setVar[items;$jsonStringify;$authorID]

$title[/start success]
$endif
```
## Setting up After /start
Now lets say you want to add a new fruit to your bot but lots of people have already started, you cant make them all use /start again or there stuff will get reset and you feel like you have lost all hope to update your bot and its all because of json. Have no fear for Version Checking is here a system i myself have designed and use that makes updating users json variables easier than ever before! <br>
### Version Checking
The first step is to actually setup a Version Checker this can be done in a JSON Variable so make a new one with the name `versions` and set the value as `{}` now add this $if block to every command you make.
```
$jsonParse[$getVar[versions;$authorID]]
$if[$or[$json[0.1]==false]]
$title[This bot has updated since you last used it!]
$description[use /update to fix this up :)]
$else
// Your commands code
$endif
```
Now every time you update your bot you will have to go to each of your commands and add that new version to the checker, for example if i release another version `0.2` i update `$if[$or[$json[0.1]==false]]` to this `$if[$or[$json[0.1]==false;$json[0.2]==false]]` and if i make a `0.2.1` for example i then make it `$if[$or[$json[0.1]==false;$json[0.2]==false;$json[0.2.1]==false]]` and so on. <br>
### /update
Now once you have setup your Version Checker lets say you want to add a new fruit for example Bananas. You will make a new command `/update` and start it of like this.
```
$if[$getVar[started:$authorID]==true]
$jsonParse[$getVar[versions;$authorID]]
$if[$and[$json[0.1]==true]
$title[Your already up to date!]
$else
// UPDATE CODE
$endif
$else
$title[you havent started yet use /start]
$endif
```
<br> Now you need to set up the actual variable update, this can be done pretty easily with `$jsonSetString` and `$jsonParse` inside of `$if` blocks, this example below will add Bananas to the `items` variable for users who have already used `/start`
```
$if[$getVar[started:$authorID]==true]
$jsonParse[$getVar[versiosn;$authorID]]
$if[$and[$json[0.1]==true]
$title[Your already up to date!]
$else
// UPDATER START
$var[version;$jsonStringify] // Store the version json code this is if you have multiple versions made so that its able to correctly update multiple versions at the same time.
$if[$json[0.1]==false]
$jsonSetString[0.1;true]
$var[version;$jsonStringify] // Saves update for later.
$jsonParse[$getVar[items;$authorID]] // Parse our items variable.
$jsonSetString[bananas;0] // Add Bananas
$setVar[items;$jsonStringify;$authorID] // Save new item
$endif
// if adding another version add below the last one for example if i was to make a v0.2 i would place it in this line.
$setVar[versions;$var[version];$authorID]]
// UPDATER END
$title[Update Successfull]
$else
$title[you havent started yet use /start]
$endif
```
<br> If you ever plan to make another update to lets say add Lettuce then just use this code below and add the new updates to the area indicated. place this code below the $endif of the last version you made.
```
$jsonParse[$var[version]]
$if[$json[0.1]==false]
$jsonSetString[0.1;true]
$var[version;$jsonStringify] // Saves update for later.
// updates added in version (new items, stats, tools etc etc.)
$endif
```
*remember to add new versions to all your commands Version Checkers and to `$if[$and[$json[0.1]==true]` in the /update command*

# Adding to JSON Strings

### $calculate

### /add-cash

### /remove-cash

### Boosters

# Economy System

### /balance

### /work

### /inventory

### /sell & /buy
