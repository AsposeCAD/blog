---
title: File formats supported by Aspose.CAD.
layout: post
tags: csharp, java, cad, export
---

<a href="https://products.aspose.com/cad">Aspose.CAD</a> supports reading several CAD file formats. Given that the CAD file formats are complex and are of different prevalence, Aspose.CAD has different level of support for them. In this article I will describe the current (as of v. 18.8 to v. 18.11) level of support for them.

## <a href="https://en.wikipedia.org/wiki/.dwg">DWG</a> and <a href="https://en.wikipedia.org/wiki/AutoCAD_DXF">DXF</a>
These are the most common CAD formats. DWG is the original AutoCAD data format, that contains all the data in a CAD document that may be needed to work with it and/or edit the CAD data and is proprietary. DXF has originally been envised as interchange format, however, it is also not completely documented and as such, is not really useful. However, as general file structure and most 2D entities are openly documented, DXF is used as a primary vector graphic format by many open-source tools. 
Aspose.CAD is for the most part focused on support of DWG and DXF file formats due to their widespread use. Versions from R11 to 2013 (AutoCAD Release 11 to AutoCAD 2017) are supported. Most features are supported, and v.18.11 will have support for representation of all entity types in both 2D and 3D display, and full support for proxy entities.

## <a href="https://en.wikipedia.org/wiki/Design_Web_Format">DWF</a>
This file format is also originally developed by Autodesk, however, unlike DWG and DXF it is both properly open and is intended as a non-editable format, allowing easy review and print by people without knowledge of AutoCAD or other design software. As such it is much simpler than DWG and DXF formats and is essentially just an vector graphic format. DWFx, which was introduced in 2009, is essentially a completely different format based on Microsoft XPS.
Aspose.CAD supports DWF V06 and newer (V06.xx) with original ePlot graphic format, so DWFx is not yet supported. There could be some missing token implementations, but support is mostly complete.

## <a href="https://en.wikipedia.org/wiki/DGN">DGN</a>
By this name, essentially two different file formats exist: DGN V7 is covered by Intergraph Standard File Formats specification, a relatively simple file format, essentially, a vector graphic format supporting 3D objects. DGN V8 is developed by Bentley Systems and is a superset of both DGN V7 and DWG formats, and has a different file structure to DGN V7.
Aspose.CAD supports DGN V7 format. Support is mostly complete.

## <a href=https://ru.wikipedia.org/wiki/Industry_Foundation_Classes">IFC</a>
IFC format has been developed as open specification to facilitate interoperability in architecture and construction, it is used for as collaboration format in building informational modelling projects.
Aspose.CAD supports IFC2x3, the visual representation part of the standard is supported. Also, objects in IFC files, unlike others, can be rendered by Aspose.CAD from an arbitrary point of view. 

## <a href="https://en.wikipedia.org/wiki/STL_(file_format)">STL</a>
STL format is a very simple format describing surface geometry of a 3D object as an array of triangle polygons. It is very common in rapid prototyping, 3D printing and computer-aided manufacturing.
Aspose.CAD supports both ASCII and binary STL files. For binary STL, the two existing non-standard color schemes are supported as well: VisCAM/SolidView's and Materialise Magics's. However, at the current moment, only one view is supported - along Z axis "downward" in orthogonal projection.

## <a href="https://en.wikipedia.org/wiki/IGES">IGES</a>
IGES has been created as interoperability and preservation format for 2D and 3D CAD models. Besides that, it also supports storage of FEM models and results of their simulations. US Departament of Defense stores all digital manufacturing information on their products in IGES format for preservation. 
Aspose.CAD isn't restricted to particular IGES file version, as the basis of the format stayed the same between all versions, but doesn't provide support for all features. Complete support is implemented for 2D graphic objects and their transformations. Only regular ASCII IGES format is supported, as both compressed text and binary IGES formats didn't gain popularity due to limited compression capability, which was easily surpassed by common file compression tools.

## PLT
PLT is essentially a list of plotter commands. PLT is an umbrella term, the file can contain commands in one of several particular plotter command languages.
Support for the most common PLT formats: <a href="https://en.wikipedia.org/wiki/Printer_Job_Language">PJL</a>, <a href="https://en.wikipedia.org/wiki/HP-GL">HPGL</a> and <a href="https://en.wikipedia.org/wiki/Hewlett-Packard_Raster_Transfer_Language">HPRTL</a> is being integrated into Aspose.CAD and should be availible in version 18.11. These were originally developed by Hewlett-Packard, but since have become supported by most plotters. HPGL is an command format for vector plotters, while HPRTL is used to embed raster images into plotter files, as most modern plotters are raster devices. Typically they also support vector input as well, so actually, files can contain commands from different languages at once. PJL is an extension of <a href="https://en.wikipedia.org/wiki/Printer_Command_Language">PCL</a>, Printer Command Language, that encompasses HPGL, and adds features for job batch control, device status report, etc.
Aspose.CAD supports commands described in PCL language reference, which encompasses all these formats, and successfully prints PLT files generated by AutoCAD.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
