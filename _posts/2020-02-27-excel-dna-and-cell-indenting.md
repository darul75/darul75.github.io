---
layout: post
title: ExcelDNA and cell indenting
---

Most of excel add-in developers choose [ExcelDNA](https://github.com/Excel-DNA) rather than VSTO and they are making the right decision as it performs faster relying on a C layer.

Excel API is vast and you might not need to wrap all existing methods but only the necessary required for your application to provide interactions it needs with Excel.

On this thread I will just expose a tiny piece of code and feature you may need to apply indenting on a cell.

## Cell indenting overview

You don't need me to see all different options to get cell content aligned so I propose you take a look at this link to see various way of doing it from the ui.

[Cell indenting options](https://excel.tips.net/T003270_Understanding_Cell_Indenting.html)

The only option we are interested in for that case is the `Indent` you can specify but not well documented. 

![Git repository example with source tree](/images/posts/exceldna4.png)

To get such result in the Excel grid:

![Git repository example with source tree](/images/posts/exceldna5.jpg)

## Documentation

There is a reference document you will need to learn about all available methods and how to use them with ExcelDNA.

[Excel 4 Macro Reference](/assets/download/Excel4MacroReference.pdf)

Inside it there is a section called `Alignment` covering the options but not clearly explaining how to set indent4.

![Git repository example with source tree](/images/posts/exceldna1.png)
![Git repository example with source tree](/images/posts/exceldna2.png)
![Git repository example with source tree](/images/posts/exceldna3.png)

You see at the end, you can specify a boolean argument `Add_indent` but it is not clear what it does and to be honest I can't remember what it was doing exactly.

## Solution

I had to play a bit to find it, I was just tempted to make it works so we do not need to use VSTO for just a single piece of missing feature.

I found the solution by looking and thinking on how ExcelDNA works in general and how arguments are passed to method where there is a need to pass exactly what the API is expecting underneath.

So I tried to add one more parameter.....and it worked ;) and it saved us some precious time.

Default usage or interpretation of this documentation will drive you to this kind of statement only.

```javascript
// example by looking at documentation
XlCall.Excel(XlCall.xlcAlignment, 2, false, 3, 0, true);
```

And here is how it should be to get indentation working with ExcelDNA.

```javascript
// and here the magic to get cell indent working with this extra seventh parameter
XlCall.Excel(XlCall.xlcAlignment, 2, false, 3, 0, true, 4); // indent of 4
XlCall.Excel(XlCall.xlcAlignment, 2, false, 3, 0, true, 7); // indent of 7 etc...
```

Tested with Excel 2010 to 2016.

## Conclusion

Do not give up to early as it may pay off later.

## References

[ExcelDNA](https://github.com/Excel-DNA)

[Excel 4 Macro Reference](/assets/download/Excel4MacroReference.pdf)


----------

Tags: *ExcelDNA* *c#*
