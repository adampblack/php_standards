# My PHP standards

## My background
I have worked as a developer for the past 5 years. My main interests recently have been creating common code to follow the DRY principle more and to make code more readable. After collaborating with many clients and colleagues I have noticed that everyone uses slightly different standards which is irritating from an application development and analysis point of view as it reduces the ability to create common code and it requires a lot more checking to create queries as you are never quite sure the table/column names someone has used.

I should emphasis that there is **no right or wrong way** - the most important lesson you should take away is being *consistent* with whatever standard you use. 

## Benefit

The development of standards for PHP is to allow multiple programmers to work on the same code base and to have similar looking code.

[Naming convention (Programming) - Wikipedia](https://en.wikipedia.org/wiki/Naming_convention_(programming))

The benefit of this is to improve interoperability between PHP applications.

## Object oriented (OOP) vs procedural 

Where possible, use OOP.

Use inheritance between classes to avoid duplicate code (DRY principle).

## Variable names
PHP uses typeless variables. It prefixed with a "$" sign and holding any data type.

Prepend the variable with the datatype of the variable with a three letter abbreviation below and then use title case to distinguish the words.

Preference is to use long descriptive variable names which are self-documenting the code. Use an IDE which does auto-complete to help use. The order of the words should be human-readable (logic sequence of words).

e.g. $strTemporaryPassword not $strPasswordTemporary

Avoid using abbreviations - where reasonable. This is because different people will abbreviate differently.

For abbreviations, only capitalise the first letter of the abbreviation when used.

e.g. intId or strUrl 

 

Exception is for class properties (see below).

Class properties
All lowercase.

This makes the class properties the same as the table column names.

e.g. column name

$objModel->id

and


e.g. object property

public $organisationid;

$objModel->organisationid

** Avoids case sensitive bugs**.

### Variable type

** Three letter abbreviation **

Example

#### Array

arr

arrUserModels


#### Boolean

bit

bitDownload


#### Float

flt

fltSum


#### Integer

int

intCount

#### Object

obj

objModel


#### String

str

strSql


## Spacing

Put one space between associative arrays and string concatenations.

### Associative array

$arrOptions =
[
    'projectid' => $intProjectId,
    'reportid' => $intReportId,
];
 
### String concatenation

$strExample = "Hello" . "World";

## Arrays
 

### Indexed arrays
 
All on one line.

$arrPrimeNumbers = [1, 3, 5];

### Associative arrays

Each key value pair should be on its own row.

Not like this:

arrExample = 
[
    'objModel' => $objModel, 'intCounter' => $intCounter
];
 

Like this - 1 row per key / value pair.

arrExample = 
[
    'objModel' => $objModel,
    'intCounter' => $intCounter
];
 

*Exception will be if there is only one key pair.*

e.g.

$objDataProvider->sort->defaultOrder = ['createdon' => SORT_DESC];

we won’t go

$objDataProvider->sort->defaultOrder = 
[
     'createdon' => SORT_DESC
];


## Strings and quotes
If you don’t need the string to include variables use **single quote** as a preference.

PHP won't use additional processing to interpret what is inside the single quote. 

When you use double quotes PHP has to parse to check if there are any variables within the string. 

A single quoted string does not have variables within it interpreted. 

A double quoted string does.

[Difference between single and double quotes - Stackoverflow](https://stackoverflow.com/questions/3446216/what-is-the-difference-between-single-quoted-and-double-quoted-strings-in-php)

### Strings over multiple lines
For single line strings keep the variable assignment and string on same line.

For multiple lines (like SQL) put it so that it is clear from indenting where the strings starts and finishes. 

$strQuery =
'
    SELECT *
    FROM example
    WHERE id = 123
';

## Blank lines
Use between methods top and bottom in classes.

Use in code where grouping lines of code logical. Include comments, as required, to describe blocks of code.

Use between control structures (if, for, while, foreach, etc). If there is no space between them it is difficult to read.


## Ordering of properties and methods
 

**Alphabetical order for class properties**. Blank line top and bottom of this block of properties.

e.g.

	public $aexample;
	public $bexample;
	public $cexample;
 

**Alphabetical order for custom class methods.

N.B. **Custom** - Prefer to keep standard methods of framework up the top as they are likely used more frequently.

 

The standard framework methods should stay up the top of the class in the default order.

## Identing

We use **Allman style**:

[Indenting styles - Wikipedia](https://en.wikipedia.org/wiki/Indentation_style)

 

Example of class:

class TestController extends TestController
{

}

## Method names


### Controller

#### Framework

Keep same.

#### Custom

Use **sentence case** (not title case).

e.g. actionPrintreport not actionPrintReport

Best to avoid case sensitive issues.


** Why **
(applicable from my Yii2 experience)

In the controller if you had title case in some frameworks will require a dash between each word captalised in the URL.

I think it also required some manual entries into the the url manager rules.

### AJAX 

For AJAX endpoints, prepend the name of the action with **Ajax** (e.g. actionAjaxchangestatus).

This helps identify the ajax endpoints rather then the view-render-type endpoints and other endpoints (custom functions or partial renders e.g export csv).

 
### Model
 

#### Framework

Keep same.

For magic setters and getters:

Use **sentence case** (not title case).


Exceptions (not setter or getters and not apart of framework):

Use all lower case.


#### Custom validators

There are some cases which don’t use magic setters and getters - such as custom validators. 

e.g. validatesql

2. Custom model functions 

These should probably exist in the controller not the model.

e.g updateuser

This allows us to run the methods all as lowercase for custom functions and using magic getter.

objModel->fullname

Custom model function

objModel::updateuser(1, 2);

**Avoids case sensitive issues**.

## Loops
Preference is to use foreach.

Be explicit with what the key and the value are.

Don’t do this:

foreach ($arrUserModels as $key => $value) 
{
}

Either do this if you don’t need the index:

foreach ($arrUserModels as $objUserModel) 
{
}

or this if you do need the index.

foreach ($arrUserModels as $intIndex => $objUserModel) 
{
}

## If statements
Always use the curly brackets around if statements (except for ternary expression).

This has caused errors in code in the past and is difficult to read sometimes (where indenting done incorrectly or inconsistently). 

More efficient just to include the curly braces to avoid bugs. 

## Search model with table joins

Make sure all andFilterWhere conditions include the explicit table name.

For the table with the current model:

$this::tableName() . '.id' => $this->id,

For other models which are joined to this base model/table.

Example::tableName() . '.id' => $this->exampleid,
 
## Search model with dates 

We include partial dates so that users can filter on years, months, or days.

// Partial startdate
$objQuery->andWhere("CASE
	WHEN '{$this->startdate}'::text != '' THEN TO_CHAR(startdate::date, 'YYYY-MM-DD') LIKE '%{$this->startdate}%' ELSE true
	END");
 
Excluding createdon and modifiedon

unless used in the index view.

## Sql
All reserved words put in capital letters.

Layout one statement per line.

SELECT *
FROM example
WHHERE del = false;


For all text searches use ILIKE to make it case insensitive.

->andFilterWhere(['ILIKE', 'client', $this->client])

Not

->andFilterWhere(['LIKE', 'LOWER(client)', strtolower($this->client)])
 

For **data types** all lowercase.

->andFilterWhere(['ILIKE', 'CAST(status AS text)', $this->status])

For **database engine functions** (such as TO_CHAR) **all uppercase**.

## Miscellaneous

### DRY principle

Do not copy and paste the same code from one place to another - use inheritance or import the function.

### Length of a method/function

If possible, keep to one screen worth of code.

Try to refactor code longer than this (multiple methods or use iteration).
