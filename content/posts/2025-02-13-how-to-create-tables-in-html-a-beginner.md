---
title: "How to Create Tables in HTML A Beginner-Friendly Guide"
date: 2025-02-13T15:41:20.763Z
draft: false
type: posts
categories: 
- spin-up,html
---
# How to Create Tables in HTML A Beginner-Friendly Guide

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

A table is a set of data organized by rows and columns. Tables are useful for displaying connections between data types, such as products and their cost, employment and dates employed, or flights and departure times. In this tutorial, you will create a table using HTML, customize it by adding a desired amount of rows and columns, and add row and column headings to make your table easier to read.

[Prerequisites](#prerequisites)[](#prerequisites)
-------------------------------------------------

-   Familiarity with HTML. If you are not familiar with HTML or need a refresher, you can review the first three tutorials of our [How To Build a Website With HTML](/community/tutorial_series/how-to-build-a-website-with-html) tutorial series.
-   An `index.html` file to practice creating HTML tables. If you do not know how to create an `index.html` file, please follow the instructions in our brief tutorial [How To Set Up Your HTML Project](/community/tutorials/how-to-set-up-your-html-project).

[Fundamentals of HTML tables](#fundamentals-of-html-tables)[](#fundamentals-of-html-tables)
-------------------------------------------------------------------------------------------

An HTML table is created with an opening `<table>` tag and a closing `</table>` tag. Inside these tags, data is organized into rows and columns by using opening and closing table row `<tr>` tags and opening and closing table data `<td>` tags.

Table row `<tr>` tags are used to create a row of data. Inside opening and closing table `<tr>` tags, opening and closing table data `<td>` tags are used to organize data in columns.

As an example, here is a table that has two rows and three columns:

```
<table>
  <tr> 
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
  </tr>
  <tr> 
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
  </tr>
</table>
```

To explore how HTML tables works in practice, paste the code snippet above into the `index.html` file or other html file you are using for this tutorial.

Save and reload the file in the browser to check your results. (For instructions on loading the file in your browser, please visit [this step](/community/tutorials/html-elements#loading-an-html-file-in-your-browser) of our tutorial on HTML Elements.)

Your webpage should now have a table with three columns and two rows:

![3 columns, 2 rows table](https://assets.digitalocean.com/articles/new-learners/columns-and-rows.png)

To add an additional row, add the highlighted `<tr>` element to the bottom of your table:

```
<table>
  <tr> 
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
  </tr>
  <tr> 
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
  </tr>
  <tr>
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
  </tr>
</table> 
```

Save your results and check them in your browser. You should receive something like this:

![3 Columns and 3 Rows Table](https://assets.digitalocean.com/articles/new-learners/columns-and-rows-2.png)

To add another column, try adding an additional table data `<td>` element inside each of the table row `<tr>` elements:

```
<table>
  <tr> 
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
    <td>Column 4</td>
  </tr>
  <tr> 
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
    <td>Column 4 </td>
  </tr>
  <tr> 
    <td>Column 1</td>
    <td>Column 2</td>
    <td>Column 3</td>
    <td>Column 4</td>
  </tr>
</table>
```

Save your results and check them in your browser. Your webpage should display a table with three rows and four columns:

![Webpage displaying table with three rows and four columns](https://assets.digitalocean.com/articles/new-learners/columns-and-rows-3.png)

[Adding a Border To a Table](#adding-a-border-to-a-table)[](#adding-a-border-to-a-table)
----------------------------------------------------------------------------------------

In general, tables should be styled with [CSS](/community/tutorial_series/how-to-build-a-website-with-css#a-brief-introduction-to-css). If you do not know CSS, you can add some light styling using HTML by adding the attributes to the `<table>` element. For example, you can add a border to the table with the `border` attribute:

```
<table border="1">
  <tr> 
    <td>Row 1</td>
    <td>Row 2</td>
    <td>Row 3</td>
  </tr>
  <tr> 
    <td>Row 1</td>
    <td>Row 2</td>
    <td>Row 3</td>
 </tr>
</table>
```

Add the highlighted border attribute to your table and checking your results in the browser. (You can clear your `index.html` file and paste in the HTML code snippet above.) Save your file and load it in the browser. Your table should now have a border surrounding each of your rows and columns like this:

![Webpage displaying table with border](https://assets.digitalocean.com/articles/new-learners/border-with-table.png)

[Adding Headings To Rows and Columns](#adding-headings-to-rows-and-columns)[](#adding-headings-to-rows-and-columns)
-------------------------------------------------------------------------------------------------------------------

Headings can be added to rows and columns to make tables easier to read. Table headings are automatically styled with bold and centered text to visually distinguish them from table data. Headings also make tables more accessible as they help individuals using screen readers navigate table data.

Headings are added by using opening and closing `<th>` tags. To add _column_ headers, you must insert a new `<tr>` element at the top of your table, where you can add the column names using `<th>` tags.

Clear the `index.html` file and add a row of column headings with the following code snippet:

```
<table border="1">
  <tr> 
    <th></th>
    <th>Column Header 1</th>
    <th>Column Header 2</th>
    <th>Column Header 3</th>
  </tr>
</table>
```

Save the `index.html` file and reload it in your browser. You should receive something like this:

![Webpage displaying HTML column headings](https://assets.digitalocean.com/articles/new-learners/column-headings.png)

Your webpage should display a single row of column headers. Note that the first column header is empty. You may add a column header here if you like.

To add row headers, you must add opening and closing `<th>` tags as the first item in every table row `<tr>` element. Add the row headers and data by adding the highlighted code snippet below _between_ the closing `</tr>` tag and the closing `<table>` tag of the table in your `index.html` file:

```
<table border="1">
 <tr> 
    <th></th>
    <th>Column Header 1</th>
    <th>Column Header 2</th>
    <th>Column Header 3</th>
  </tr>
  <tr>
    <th>Row Header 1</th>
    <td>Data</td>
    <td>Data</td>
    <td>Data</td>
  </tr>
  <tr> 
    <th>Row Header 2</th>
    <td>Data</td>
    <td>Data</td>
    <td>Data</td>
 </tr>
 <tr> 
    <th>Row Header 3</th>
    <td>Data</td>
    <td>Data</td>
    <td>Data</td>
 </tr>
</table>
```

Save the `index.html` file and reload it in your browser. You should receive something like this:

![Webpage displaying table with column and row headings](https://assets.digitalocean.com/articles/new-learners/column-row-headings.png)

You should now have a table with with three column headings and three row headings.

[Responsive Table Techniques, Accessibility Best Practices, and Tags Used in HTML Tables](#responsive-table-techniques-accessibility-best-practices-and-tags-used-in-html-tables)[](#responsive-table-techniques-accessibility-best-practices-and-tags-used-in-html-tables)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### [Making Tables Responsive](#making-tables-responsive)[](#making-tables-responsive)

To make tables mobile-friendly, consider these techniques:

1.  **CSS Overflow for Scrollable Tables**
    
    ```
    .table-container {
       overflow-x: auto;
    }
    ```
    
    ```
    <div class="table-container">
    <table>
       <tr>
          <th>Column 1</th>
          <th>Column 2</th>
       </tr>
       <tr>
          <td>Data 1</td>
          <td>Data 2</td>
       </tr>
    </table>
    </div>
    ```
    
2.  **Bootstrap for Responsive Tables**
    
    ```
    <div class="table-responsive">
    <table class="table">
       <tr>
          <th>Header</th>
          <td>Data</td>
       </tr>
    </table>
    </div>
    ```
    

Bootstrap automatically makes tables responsive with `.table-responsive`.

### [Best Practices for Accessibility](#best-practices-for-accessibility)[](#best-practices-for-accessibility)

To improve accessibility:

-   **Use the `scope` attribute**
    
    ```
    <th scope="col">Column Name</th>
    ```
    
-   **Add `aria-labels` for screen readers**
    
    ```
    <table aria-describedby="table-description">
    ```
    
-   **Use `<caption>` for table descriptions**
    
    ```
    <caption>Product Pricing Table</caption>
    ```
    

For more styling and accessibility improvements, check [How to Style a Table with CSS](/community/tutorials/how-to-style-a-table-with-css).

### [Tags Used in HTML Tables](#tags-used-in-html-tables)[](#tags-used-in-html-tables)

Here are some commonly used table tags in HTML:

Tag

Description

`<table>`

Defines a table

`<tr>`

Defines a row

`<th>`

Defines a header cell

`<td>`

Defines a data cell

`<thead>`

Groups header content

`<tbody>`

Groups body content

`<tfoot>`

Groups footer content

`<caption>`

Adds a table caption

`<colspan>`

Merges columns

`<rowspan>`

Merges rows

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. How do you create a table in HTML?](#1-how-do-you-create-a-table-in-html)[](#1-how-do-you-create-a-table-in-html)

To create a table in HTML, use the `<table>` element along with `<tr>` (table row), `<th>` (table header), and `<td>` (table data) elements. Here’s a simple example:

```
<table border="1">
  <tr>
    <th>Header 1</th>
    <th>Header 2</th>
  </tr>
  <tr>
    <td>Data 1</td>
    <td>Data 2</td>
  </tr>
</table>
```

For more advanced styling options, check out [How to Style a Table with CSS](/community/tutorials/how-to-style-a-table-with-css).

### [2\. How to make a menu table in HTML?](#2-how-to-make-a-menu-table-in-html)[](#2-how-to-make-a-menu-table-in-html)

A menu table can be structured using the `<table>` element to display items and prices in a structured way:

```
<table border="1">
  <tr>
    <th>Item</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>Burger</td>
    <td>$5</td>
  </tr>
</table>
```

To make the menu visually appealing, you can apply CSS styles. Learn more in [How to Style HTML with CSS](/community/tutorial-series/how-to-style-html-with-css).

### [How to make a borderless table in HTML?](#how-to-make-a-borderless-table-in-html)[](#how-to-make-a-borderless-table-in-html)

You can remove table borders by setting `border="0"` in the `<table>` tag or using CSS:

```
table {
  border-collapse: collapse;
  border: none;
}
td, th {
  border: none;
}
```

You can learn more ways to style tables in [How to Build a Website with CSS](/community/tutorial-series/how-to-build-a-website-with-css).

### [4\. How do you create a row in an HTML table?](#4-how-do-you-create-a-row-in-an-html-table)[](#4-how-do-you-create-a-row-in-an-html-table)

To create a row, use the `<tr>` element inside the `<table>`:

```
<table border="1">
  <tr>
    <td>Row 1, Column 1</td>
    <td>Row 1, Column 2</td>
  </tr>
</table>
```

### [5\. How do I merge cells in an HTML table?](#5-how-do-i-merge-cells-in-an-html-table)[](#5-how-do-i-merge-cells-in-an-html-table)

Use the `colspan` or `rowspan` attributes in `<td>` or `<th>`:

```
<table border="1">
  <tr>
    <td colspan="2">Merged Cell</td>
  </tr>
  <tr>
    <td>Cell 1</td>
    <td>Cell 2</td>
  </tr>
</table>
```

### [6\. Can I make an HTML table responsive?](#6-can-i-make-an-html-table-responsive)[](#6-can-i-make-an-html-table-responsive)

Yes! You can use CSS or frameworks like Bootstrap to make tables responsive. For example, using CSS:

```
table {
  width: 100%;
  max-width: 100%;
  overflow-x: auto;
  display: block;
}
```

For a more comprehensive approach, check the **[Responsive Table Techniques](/community/tutorials/how-to-create-tables-in-html#responsive-table-techniques-accessibility-best-practices-and-tags-used-in-html-tables)** section above.

### [7\. What are the best practices for creating accessible tables?](#7-what-are-the-best-practices-for-creating-accessible-tables)[](#7-what-are-the-best-practices-for-creating-accessible-tables)

Use proper semantic elements like `<thead>`, `<tbody>`, and `<tfoot>`. Also, implement accessibility attributes like:

-   `<th scope="col">` for column headers.
-   `<th scope="row">` for row headers.
-   `aria-describedby` for assistive technologies.

Check the **[Accessibility Best Practices](/community/tutorials/how-to-create-tables-in-html#responsive-table-techniques-accessibility-best-practices-and-tags-used-in-html-tables)** section above for details.

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this tutorial, you have created an HTML table, added additional rows and columns, and created headings for rows and columns.

If you are interested in learning more about HTML, you can check our our tutorial series [How To Build a Website With HTML](/community/tutorial_series/how-to-build-a-website-with-html). To learn about how to use CSS to style HTML elements (including tables), please visit our tutorial series [How To Build a Website With CSS](/community/tutorial_series/how-to-build-a-website-with-css).

#### [Source](https://www.digitalocean.com/community/tutorials/how-to-create-tables-in-html)

<br/>
---
