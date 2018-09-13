---
title: Control over rasterization in Aspose.CAD.
layout: post
tags: csharp, java, cad, export
---

<a href="https://products.aspose.com/cad">Aspose.CAD</a> allows some control over rasterization of CAD files to raster images and PDF files. Let's see what's possible with them.

Rasterization itself, in essence, is controlled by an instance of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions">CadRasterizationOptions</a> class, which is assigned to <a href="https://apireference.aspose.com/net/cad/aspose.cad/imageoptionsbase/properties/vectorrasterizationoptions">VectorRasterizationOptions</a> property of instance of output format-specific descendant of <a href="https://apireference.aspose.com/net/cad/aspose.cad/imageoptionsbase">ImageOptionsBase</a>, i.e. <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pdfoptions">PdfOptions</a>, <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pngoptions">PngOptions</a> etc. Consequently, when you pass the output options instance to <a href="https://apireference.aspose.com/net/cad/aspose.cad/image">Image</a>'s <a href="https://apireference.aspose.com/net/cad/aspose.cad.image/save/methods/3">Save</a> method, it's the CadRasterizationOptions property which specifies what will be rendered, and output options themselves only set up the file that contains the rendered image. Here we will focus on the CadRasterizationOptions instead of format-specific options.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/pageheight">PageHeight</a> and <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/pagewidth">PageWidth</a> properties: obviously, sets output image's height and width. In case of raster image, these define image's dimensions in pixels, in case of PDF, as these are vector images, it is mostly relative and only used for defining output image's aspect ratio, unless <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/unittype">UnitType</a> is specified, then these will define actual physical dimensions of PDF page(s), and content will be scaled to fit it.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/pagesize">PageSize</a> is essentially the same, just accepts <a href="https://apireference.aspose.com/net/cad/aspose.cad/sizef">SizeF</a> which defines height and width at once.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/borderx">BorderX</a> and <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/bordery">BorderY</a> define margins around actual image area, i.e. the final image will have dimensions of  PageHeight+BorderX*2 and height of PageWidth+BorderY*2.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/centerdrawing">CenterDrawing</a> option if set to true will determine geometric center of displayed objects and pin it to image's center. When exporting 3D entities it will automatically be treated as enabled.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/backgroundcolor">BackgroundColor</a>, obviously, sets background color, as CAD systems typically imply there's no background, but you do need one in raster images. Default is white.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/drawcolor">DrawColor</a> - you may override entities' color defined in a CAD document and substitute your own by setting this property to color you want and <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/drawtype">DrawType property to UseDrawColor. By default, DrawColor is black.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/drawtype">DrawType</a> - indicates wether to render entities used colors specified in document, or use DrawColor. Default is UseDrawColor.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/unittype">UnitType</a> - specifies dimension unit of output image. If you're exporting to PDF and have specified output image dimensions, then PDF page will have dimensions of the specified PageHeight and PageWidth in the specified units. If you're exporting to PDF with specified UnitType and without specified dimensions, then PDF's page will have dimensions corresponding to CAD document content, in the specified units. I.e. if an document contains a 100 inch rod, with dimensions specified in inches, and you specify centimeters as UnitType, then output PDF will have an 254 cm rod, with dimensions specified in cm.

In case of raster images, UnitType only matters when output dimensions are not specified. Then one pixel represents 1x1 unit square. So, if you have a feature that's 100 inches wide in document, and you set UnitType to Centimeter and export to raster without specifying dimensions, the feature will have width of 254 pixels. If the CAD image is unitless, image's units are treated as inches.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/contentasbitmap">ContentAsBitmap</a> - only applicable when exporting to PDF, if it is set to true, the rendered image will be rasterized and embedded into PDF as raster image, instead of a regular vector one. Default is false.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/graphicsoptions">GraphicsOptions</a> - allows to set antialiasing level, interpolation quality and text rendering mode for raster images (including embedded in PDF)

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/zoom">Zoom</a> - another way to specify margins. Value of 1 corresponds to exact fit, value below 1 allows to create margins, value above 1 allows to scale drawing up.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/penoptions">PenOptions</a> - allows to set pen options for entities with no defined pen options. Currently only allows setting start and end line caps.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/observerpoint">ObserverPoint</a> - only matters for IFC images, as these do not have defined observation point. By default, observation point is looking "down" along Z axis.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/fields/renderresult">RenderResult</a> - a handler that will receive CadRenderResult when render will be completed. Allows to see list of errors that have happened during render.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/typeofentities">TypeOfEntities</a> - Specifies either 2D or 3D entities to render for AutoCAD files. Most entities are rendered either way, some are 2D only, some are 3D only, more detailed explaination is in <a href="https://asposecad.github.io/Clarifying-3d-export-aspose-cad/">Clarifying export of 3D entities with Aspose.CAD</a> article.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/layers">Layers</a> - gets or sets layers to export for AutoCAD documents. 

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/layouts">Layouts</a> - gets or sets layout names to export for AutoCAD documents. If null, Model layout will be exported.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/pdfproductlocation">PdfProductLocation</a> - AutoCAD files may have underlays that refer to PDF files. Aspose.CAD can't render PDF's by itself, so this option is used to specify location of Aspose.PDF DLL file that can read PDF files. License file is expected to be in the same folder.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/automaticlayoutsscaling">AutomaticLayoutScaling</a> - scales CAD document content to output image size if set to true, default is true.

### <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/noscaling">NoScaling</a> - defines wether to scale CAD content to match output image. This is overridden by AutomaticLayoutScaling - if that is set to true, NoScaling will have no effect. 

For most file formats, when AutomaticLayoutScaling is false and NoScaling is true, no scaling is done at all, i.e. if your file's contents fit within 5 relative units defined in file, then these contents will be rasterized into 5 pixels, so you won't be able to discern anything if you export to raster. If you export to PDF, correspondingly, you'll have to zoom in to see details. 

Exceptions are IFC, which is always scaled to fit, and AutoCAD files, for which, in that case, viewport will be scaled and centered to image's center, instead of actual entities. So, if you're exporting AutoCAD file where entities occupy a small part of viewport and ScaleMethod is set to none, entities will occupy a correspondingly small part of output image, and on the contrary, if scaling is used, these entiites will occupy the whole output image.


That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
