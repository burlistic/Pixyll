---
layout:     post
title:      SSRS Tips and Tricks
date:       2015-12-03
summary:    Random notes on my experiences using SSRS
categories: 
---

Notes from using Microsoft's SSRS 2014 to build complex pre-filled forms which where exported to PDF via the SOAP service

## Page Breaks Not Working When Exporting to PDF

If you have an empty Tablix row, without a empty textbox or other element, then any page breaks you have after will not work when exporting to PDF. A random issue I found as we where modifying the XML by hand. Via the UI Visual Studio re-inserts an empty text box to stop this happening.

## Use Rectangles to Insert Page Breaks Between Sub Reports

The page breaking on Tablix groups proved to be troublesome. Removing these and adding a rectangle at the end of each sub report with a page break proved to be a lot more reliable.

## Page Headers and Footers

Page breaks do not working in headers and footers - which makes sense in hindsight. Likewise, page numbers cannot be accessed in the report body.

## Implementing Space Pages

A simple way we found to implement spacer pages when a sub report ends on odd page was to insert some place holder text then to process the PDF using a library such as iText to add the empty pages as required on the fly.

## Populating Drop Downs and Adding Default Options

When selecting options from a look up table a UNION 'Please select' can be used to add a default option.

How to sort to make sure this appears first in the list?

## Installing Toolset for Visual Studio 2013

BI Plug-in can be found online

## Forcing Through Changes \ Renames

Delete the contents of the debug folder. If this doesn't work; hit the refresh on the UI when in preview mode

## One Click Deploy

Set the server path via the Project settings. The hitting run right click deploy on the project file to push your files to the server. Configuration settings can be used to stop deployment of Datasets.