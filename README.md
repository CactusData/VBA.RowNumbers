# VBA.RowNumbers

## Enumeration of Records and Rows in Queries and Forms in *Microsoft Access*

Version 1.1.4

*(c) Gustav Brock, Cactus Data ApS, CPH*

![General](https://raw.githubusercontent.com/GustavBrock/VBA.RowNumbers/master/images/EE%20RowNumbers.png)

### Enumeration of records and rows
Set of functions to:

- Add a sequential *record number* to each row in a form
- Add a sequential *row number* to each record of a query
- Add a sequential user maintained *priority number* to each record in a form
- Add a random *row number* to each record of a query

No third-party tools used, only a single code module that accepts both 32- and 64-bit Microsoft Access. 

Further, these *traditional* and  *code-less methods* are demonstrated:

- Add a sequential *row counter* to each record of a query or form
- Add a random *row number* to each record of a query or form

### How it works

Enumeration can be performed in several ways, but have one thing in common:

	A number is assigned each row or record.

The difference is *how* and *when*, and - if the records are supposed to be updated, appended, or deleted - by *who* or *what*. The numbers can be sequential or random. 

The functions listed here offer four methods, each having some characteristic advantages and disadvantages:

#### 1. Record Numbers
These are similar to the *Record Number* displayed in the *Navigation Bar* of a form (left-bottom, in the status bar of the form).

***Advantages***

> Will always be sequential from top to bottom of the form, no matter how records are ordered, filtered, edited, deleted, or inserted.
> 
> For a form, the source can be a table; a query is not required.

***Disadvantages***
> Will not, for the individual record, be static ("sticky").
> 
> Belongs to the form, not the recordset.
> 
> May update slowly when browsing the form.
> 
> For forms only, not queries.

#### 2. Row Numbers
These are created in a query, as a separate field of the resulting recordset.

***Advantages***
> The numbers will not update if records are deleted, and new records will be assigned the next higher number(s) as long as the query is not requeried.
> 
> The numbers will be defined by the ordering of the query.
>
> If a form is bound to the query, the numbers will always stick to the individual record, no matter how the form is ordered or filtered, or (if the query or form is not updated/requeried) if records are added or deleted.
>
>Generating the numbers takes *one table scan* only, thus browsing the query, or a form bound to it, is very fast.
>
>As the numbers are static ("sticky"), they are well suited for export or for use in append queries.

***Disadvantages***
>If records are added or deleted, the assigned numbers may change after a requery to regain sequentiality.
>
>If used in a form, and different filtering or sorting is applied, there is no method to reqain sequentiality other than to revert to the original ordering and remove filtering.

#### 3. Priority Numbers
These are stored in a separate numeric field of the table. Thus, they are persistent, and primarily intended to be maintained by a user having the records listed in a form.

***Advantages***
>The numbers will be persistent, no matter how the records are filtered or ordered.
>
>When maintained in a form, a new record will automatically be assigned the next higher number (lowest priority).
>
>In the form, records can be inserted or deleted without breaking the sequentiality of the numbers.
>
>Can easily, for example by a button-click, be reset to match the current sorting of the form.
>
>Will not degrade browsing in any way.

***Disadvantages***
>If records can be inserted or deleted from sources not maintaining the sequentiality of the numbers, this will be broken. However, sequentiality can be restored by a call to the function *AlignPriority*.
 
##### 4. Random Row Numbers
These are pseudo-random numbers assigned each record in a query, as a separate field of the resulting recordset. The typical purpose is to sort on these, selecting a specific (small) count of random records from a (large) recordset. 

For this purpose, a query must be used to generate the numbers; if the numbers are generated from an expression bound to the ControlSource of a control in a form, the form will not be able to sort on that field, which in most cases would defeat its purpose.

***Advantages***
>Allows what appears to be a random selection of records.
>
>Allows what appears to be a random ordering of the records.
>
>Very fast. Will not degrade browsing in any way.
>
>In a form, the numbers will not change if sorting or filtering is changed, or the form is requeried.
>
>Can easily, for example by a button-click, be shuffled.

***Disadvantages***
>Must be implemented in a query to be useful.

#### 5. No-Code Row Counter
This is a simple counter using an expression with *DCount* that - for each record - counts the records having a unique key equal to the current key or lower.

***Advantages***
>The sequence of the numbers will always be aligned with the ascending order of the unique key referenced.
>
>Requires zero code, only a TextBox having an expression as ControlSource.
>
>In a form, the numbers will not change if sorting or filtering is changed.

***Disadvantages***
>As the number is calculated for each record visible in the form, browsing can be slow, indeed for large recordset.

#### 6. No-Code Random Number
This is a simple pseudo-random number generator using an expression with *Rnd* that assigns a pseudo-random number to each record. However, these are *delicate* and will be regenerated whenever the form or query is requeried, or sorting or filtering is changed.

***Advantages***
>Requires zero code, only a TextBox having an expression as ControlSource.
>
>Can be copy-pasted to, say, Excel.

***Disadvantages***
>Very fragile. Any sorting, filtering, or a requery will rebuild the numbers.
>
>Cannot be sorted in a form.

### Code

All code is contained in one module: *RowEnumeration*

In those cases where a form is used for display of the records, some additional code may be required in the code-behind module of the form. This is, however, kept at a minimum, and is documented in the in-line comments of the functions.


### Demo

A separate module contains some functions to demonstrate how to call the helper functions for resetting or requerying the number series.

A set of queries and forms are included in the *Microsoft Access 2016* project to demonstrate typical implementations of the functions.


### Documentation

Detailed documentation is in-line. 

Articles on the topic can be found here:

![EE Logo](https://raw.githubusercontent.com/GustavBrock/VBA.RowNumbers/master/images/EE%20Logo.png)

[Random Rows in Microsoft Access](https://www.experts-exchange.com/articles/33030/Random-Rows-in-Microsoft-Access.html)