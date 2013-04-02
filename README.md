contao-responsive-tables
========================

Enriching data tables in Contao with attributes so that you can linearize them for small screens like on mobile phones.

Use still need to adjust your stylesheet, though.

# Making data tables readable on mobile phones

There are several approaches to format tables on small screens.

I'm focussing on the easiest one here: Linearizing everything so that all cells stack on eachother in vertical.

A properly structured data table has a _table header_ with _header cells_ that work as titles for the _data cells_ in the same column. This is achievable with Contao's content element *table* by using the header option:

Name   | Year   | Age
-------|--------|----
Andy   | 1984   | 28
Simon  | 1989   | 24

Linearizing this table would result in one stack of _header cells_ `th` on the top, followed by stacks of _data cells_ `td`:

> **Name**  
> **Year of birth**  
> **Age**
>
> Andy  
> 1984  
> 28
> 
> Simon  
> 1989  
> 24  

Since not all data explains itself without a header (or title), one solution is to hide the header and add the title to each _data cell_, which makes reading the data sets possible again:

> **Name** Andy  
> **Year** 1984  
> **Age** 28
> 
> **Name** Simon  
> **Year** 1989  
> **Age** 24 

## How it works

This extension looks for a corresponding _header cell_ for each _data cell_ and adds an `title`-attribute to each.

Using _CSS pseudo elements_, you can add those attributes above each cell:

    td[title]:before {
      display: block;
      font-weight: bold;
      content: attr(title);
    }

## The complete CSS

Using progressive enhancement, the basic CSS could look something like this:

    table, thead, tfoot, tbody, tr, th, td {
      display: block;
    }
    
    tr {
      margin: .6em 0;
    }
    
    thead {
      height: 0; /* we show titles for each cell, not in the header */
      overflow: hidden;
    }
    
    /* show the title attribute above each data cell */
    td[title]:before {
      display: block;
      font-weight: bold;
      content: attr(title); 
    }
    
    @media (min-width: 30em) { /* assume our reset table fits in there */ 
      table { display: table; }
      thead { display: table-header-group; }
      tfoot { display: table-footer-group; }
      tbody { display: table-row-group; }
      tr { display: table-row; }
      td, th { display: table-cell; }
      td[title]:before { content: none; }
    }      

## IE and progressive enhancement

Since there are some serious issues with table formatting in IE <= 8, you should avoid formatting tablets at all and keep the browsers's default rules.

IE <= 8 is used mainly on the desktop, so either work with `max-width` mediaqueries to linearize the table, or hide the table-stylesheet to this browser.
