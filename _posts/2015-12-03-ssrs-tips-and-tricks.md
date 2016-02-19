---
layout:     post
title:      SSRS Tips and Tricks
date:       2015-12-03
summary:    Random notes on my experiences using SSRS
categories: 
---

Notes from using Microsoft's SSRS to build complex pre-filled forms which where exported to PDF via the SOAP service

## Installing Toolset for Visual Studio

Download and install SQL Server Data Tools [SSDT](https://msdn.microsoft.com/en-us/library/mt204009.aspx). A BI Plug-In for Visual Studio.

## Report Data Window

You will need to use this window a lot to add datasets and params. Annoyingly it is not always visible as default. To open the window again you can use the hot key ctrl+alt+d or from the 'View Menu'. If you find it is absent from the menu altogether, try to close and re-open the RDL.

![report data window](http://burlistic.github.io/images/ssrs/report%20data%20window.jpg)

## Document Outline

Another handy window is the Document Outline. This make navigation a lot easier when constructing complex RDLS.

![document outline](http://burlistic.github.io/images/ssrs/document%20outline.jpg)

## Emtpy Tablix / Lists

Sometimes it can be handy to use the table layout of a list but there is no dataset. In this case simply add a dummy data set by using the following SQL <i>select 'emtpy'</i>

## Populating Drop Downs and Adding Default Options

When selecting options from a look up table a <i>UNION 'Please select'</i> can be used to add a default option.

## Forcing Through Changes Locally \ Issues after File Renamed

Delete the contents of the debug folder. If this doesn't work; hit the refresh on the UI when in preview mode

## Page Headers and Footers

Header and footers allow limited content - for example no page break or Tablixs. Likewise, page numbers cannot be accessed in the report body.

## Better Error Info for Debugging

Try running the sub reports individually first.

You get more detailed error messages running on the server. So if you are stuck you can deploy the RDL and run it via the Web UI.

Sometimes exporting to PDF will allow you see a partial error message for a sub report. This can be copy and pasted into Notepad so you can see it fully.

I have still yet to find where the logs are written locally or on the server as the methods above always worked for me.

## PDF Watermarks

It took a little bit of experimentation to get the watermarks generated. The size that ended up working was a bit odd (Width 25cm / Height 38cm) but does produce a nice watermark in the middle of the page.

Below are some of downloads for a 'draft' and 'copy' (included are the [GIMP](https://www.gimp.org) files if you want to amend - you will need to [rotate](https://docs.gimp.org/en/gimp-tool-rotate.html) and align again if you wish to modify the text). To add these images as a watermark include them as an embedded image in the background property. Set 'repeat Y' for multipage documents.

[Draft JPG]({{ site.url }}/downlaods/draft-25-38-cm.jpg)
[Copy JPG]({{ site.url }}/downlaods/copu-25-38-cm.jpg)
[Draft GIMP XCF]({{ site.url }}/downlaods/draft-25-38-cm.xcf)
[Copy GIMP XCF]({{ site.url }}/downlaods/copu-25-38-cm.xcf)

## Page Breaks Not Working When Exporting to PDF

If you have an empty Tablix row, without a empty textbox or other element, then any page breaks you have after will not work when exporting to PDF. A random issue I found as we where modifying the XML by hand. Via the UI Visual Studio re-inserts an empty text box to stop this happening.

## Use Rectangles to Insert Page Breaks Between Sub Reports

The page breaking on Tablix groups proved to be troublesome. Removing these and adding a rectangle at the end of each sub report with a page break proved to be a lot more reliable.

If the page break isn't working then you can add an empty Textbox to force it to render.

## Margins when Exporting to PDF

To force a left margin; position content slightly using the left property. The page margins seem to be ignored for some reason when exporting.

## Inconsitent Borders when Exporting to PDF

When using subreports the borders can be inconsistent. To remedy this increase the borders of the emelement in the subreports by 1pt. Do not add a border to the subreport placeholder if the content can grow across pages as this do not render well.

## Implementing Spacer Pages

A simple way we found to implement spacer pages when a sub report ends on odd page was to insert some place holder text then to process the PDF using a library such as iText to add the empty pages as required on the fly.

This requirement came about as we did not know if the user might print the generated PDF duplex or single sided and we need to ensure the start of each report (one PDF could contain several) was always face up.

## Load the reports UI without the top level navigation

Added in _SERVER to the URL and this will hide the navigation

## Deployment

Set the server path via the Project settings. The hitting run right click deploy on the project file to push your files to the server. Configuration settings can be used to stop deployment of Datasets.

It is recommend you clean down the environment and delete data sets and reports (RDLS) before deployment. Any easy way to do this is to add a parent folder, then you delete the folder in one hit and not need to delete each item individually.