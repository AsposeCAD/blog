---
title: Interesting features of Aspose.CAD
layout: post
tags: csharp, java, cad, convert
---


Hello, in this article I'm going to list what distinguishes <a href="https://products.aspose.com/cad/">Aspose.CAD</a> from other similar software and generally about it's interesting features.

## Export to specified output dimension
It is possible to set up specific dimensions for output PDF file, to export to A4 sized document, for example. There is an example on how to setup Aspose.CAD to output desired PDF size. Contents of the exported area will be rescaled to fit into output document area, without aspect ratio change. Also note that the code sample also contains example for raster output, which is much simpler - just multiply desired DPI by desired output document dimensions and you get desired the raster image resolution.
```csharp
public static void Run()
{
    using (var cadImage = Image.Load("visualization_-_conference_room.dwg"))
    {

        // export to pdf
        CadRasterizationOptions rasterizationOptions = new CadRasterizationOptions();
        rasterizationOptions.Layouts = new string[] { "Model" };

        bool currentUnitIsMetric = false;
        double currentUnitCoefficient = 1.0;
        DefineUnitSystem(cadImage.UnitType, out currentUnitIsMetric, out currentUnitCoefficient);

        if (currentUnitIsMetric)
        {
            double metersCoeff = 1 / 1000.0;

            double scaleFactor = metersCoeff / currentUnitCoefficient;

            rasterizationOptions.PageWidth = (float)(210 * scaleFactor);
            rasterizationOptions.PageHeight = (float)(297 * scaleFactor);
            rasterizationOptions.UnitType = UnitType.Millimeter;
        }
        else
        {
            rasterizationOptions.PageWidth = (float)(8.27f / currentUnitCoefficient);
            rasterizationOptions.PageHeight = (float)(11.69f / currentUnitCoefficient);
            rasterizationOptions.UnitType = UnitType.Inch;
        }

        rasterizationOptions.AutomaticLayoutsScaling = true;

        PdfOptions pdfOptions = new PdfOptions
        {
            VectorRasterizationOptions = rasterizationOptions
        };

        cadImage.Save("out.pdf", pdfOptions);

        PngOptions png = new PngOptions();
        png.VectorRasterizationOptions = rasterizationOptions;
        // export to raster
        //A4 size at 300 DPI - 2480 x 3508  
        rasterizationOptions.PageHeight = 3508;
        rasterizationOptions.PageWidth = 2480;

        cadImage.Save("out.png", png);
    }

}

private static void DefineUnitSystem(UnitType unitType, out bool isMetric, out double coefficient)
{
    isMetric = false;
    coefficient = 1.0;

    switch (unitType)
    {
        case UnitType.Parsec:
            coefficient = 3.0857 * 10000000000000000.0;
            isMetric = true;
            break;
        case UnitType.LightYear:
            coefficient = 9.4607 * 1000000000000000.0;
            isMetric = true;
            break;
        case UnitType.AstronomicalUnit:
            coefficient = 1.4960 * 100000000000.0;
            isMetric = true;
            break;
        case UnitType.Gigameter:
            coefficient = 1000000000.0;
            isMetric = true;
            break;
        case UnitType.Kilometer:
            coefficient = 1000.0;
            isMetric = true;
            break;
        case UnitType.Decameter:
            isMetric = true;
            coefficient = 10.0;
            break;
        case UnitType.Hectometer:
            isMetric = true;
            coefficient = 100.0;
            break;
        case UnitType.Meter:
            isMetric = true;
            coefficient = 1.0;
            break;
        case UnitType.Centimenter:
            isMetric = true;
            coefficient = 0.01;
            break;
        case UnitType.Decimeter:
            isMetric = true;
            coefficient = 0.1;
            break;
        case UnitType.Millimeter:
            isMetric = true;
            coefficient = 0.001;
            break;
        case UnitType.Micrometer:
            isMetric = true;
            coefficient = 0.000001;
            break;
        case UnitType.Nanometer:
            isMetric = true;
            coefficient = 0.000000001;
            break;
        case UnitType.Angstrom:
            isMetric = true;
            coefficient = 0.0000000001;
            break;
        case UnitType.Inch:
            coefficient = 1.0;
            break;
        case UnitType.MicroInch:
            coefficient = 0.000001;
            break;
        case UnitType.Mil:
            coefficient = 0.001;
            break;
        case UnitType.Foot:
            coefficient = 12.0;
            break;
        case UnitType.Yard:
            coefficient = 36.0;
            break;
        case UnitType.Mile:
            coefficient = 63360.0;
            break;
    }
}
```

## Normalisation to absolute metrics for export
By default, Aspose.CAD works with relative units of the drawing. However, there is a <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/unittype">UnitType</a> property in <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/">CadRasterizationOptions</a> class, which specifies used unit type. During render, a unit is interpreted as 1 pixel, so if an image has specified size of 10x10 m and centimeter is selected as unit type, the image will be rasterized to 1000x1000 pixel image.
```csharp
    string fileName = GetFileFromDesktop("Floorplan.dwg");
    using (Aspose.CAD.Image image = Aspose.CAD.Image.Load(fileName))
    {
        BmpOptions bmpOptions = new BmpOptions();
        CadRasterizationOptions cadRasterizationOptions = new CadRasterizationOptions();
        bmpOptions.VectorRasterizationOptions = cadRasterizationOptions;
        cadRasterizationOptions.CenterDrawing = true;
        cadRasterizationOptions.UnitType = UnitType.Centimeter;
        cadRasterizationOptions.Layouts = new string[] { "Model" };
        // export
        string outPath = fileName + ".bmp";
        image.Save(outPath, bmpOptions);
    }
```

## Support for PDF/A
Aspose.CAD supports specifying compliance to PDF/A standard to render archival PDF documents. The process involves creating regular <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pdfoptions/">PdfOptions</a>  to export images, setting its <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pdfoptions/properties/corepdfoptions">CorePdfOptions</a>  property to a new instance of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pdfdocumentoptions/">PdfDocumentOptions</a>  and setting <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/pdfdocumentoptions/properties/compliance">Compliance</a>  field in the instance. After that, an image saved using that PdfOptions instance will be saved as PDF/A compliant PDF file. Example:
```csharp
PdfOptions pdfOptions = new Aspose.CAD.ImageOptions.PdfOptions
{
	VectorRasterizationOptions = rasterizationOptions
};

pdfOptions.CorePdfOptions = new PdfDocumentOptions();

pdfOptions.CorePdfOptions.Compliance = PdfCompliance.PdfA1a;
cadImage.Save(outPath, pdfOptions);

pdfOptions.CorePdfOptions.Compliance = PdfCompliance.PdfA1b;
cadImage.Save(outPath, pdfOptions);
```

## Margin control during export.
By default, Aspose.CAD renders CAD files with small margins around the whole content of the file or page. There is a <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/zoom">Zoom</a>  property in <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/">CadRasterizationOptions</a>  class that controls the image scaling. By default, it is set to a bit less than 1, to provide margins. If you need no margins, set it to 1. Example:
```csharp
using (CadImage cadImage = (CadImage)Image.Load(fileName))
{
	// call after changes done to image to check new size of the image.
	cadImage.UpdateSize();

	CadRasterizationOptions rasterizationOptions = new CadRasterizationOptions();

	rasterizationOptions.UnitType = UnitType.Micrometer;
	rasterizationOptions.PageHeight = cadImage.Height;
	rasterizationOptions.PageWidth = cadImage.Width;
	rasterizationOptions.Zoom = 1f;


	PdfOptions pdfOptions = new PdfOptions();
	pdfOptions.VectorRasterizationOptions = rasterizationOptions;
	cadImage.Save(outDir + fileName + ".pdf", pdfOptions);
}
```

## Export of a specific area of CAD document
This is also possible, however, it works for DWG documents and isn't straightforward to do, as it involves creating a custom viewport for an area. See the example:
```csharp
    CadImage cadImage = Image.Load(FileName) as CadImage;

    CadRasterizationOptions rasterizationOptions = new CadRasterizationOptions();
    rasterizationOptions.Layouts = new string[] { "Model" };
    rasterizationOptions.NoScaling = true;


    rasterizationOptions.PageHeight = height;
    rasterizationOptions.PageWidth = width;

    // note: preserving some empty borders around part of image is the responsibility of customer
    // top left point of region to draw

    CadVportTableObject newView = new CadVportTableObject();

    // note: exactly such table name is required for active view
    newView.TableName = "*Active";
    newView.CenterPoint.X = topLeft.X + width / 2f;
    newView.CenterPoint.Y = topLeft.Y - height / 2f;
    newView.ViewHeight.Value = height;
    //newView.ViewAspectRatio.Value = width / height;

    // search for active viewport and replace it
    for (int i = 0; i < cadImage.ViewPorts.Count; i++)
    {
        CadVportTableObject currentView = (CadVportTableObject)(cadImage.ViewPorts[i]);
        if ((currentView.TableName == null && cadImage.ViewPorts.Count == 1) ||
        string.Equals(currentView.TableName.ToLowerInvariant(), "*active"))
        {
            cadImage.ViewPorts[i] = newView;
            break;
        }
    }
    PngOptions pngOptions = new PngOptions();
    pngOptions.VectorRasterizationOptions = rasterizationOptions;
    cadImage.Save("output.png", pngOptions);
```


## Tracking of export errors
Aspose.CAD has a way to record errors that happened during export of an CAD file to raster or PDF. There is a <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/fields/renderresult">RenderResult</a>  event in <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/">CadRasterizationOptions</a>  class, which is called when an export is completed. Event handler receives a <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrenderresult/">CadRenderResult</a>  which contains a list of errors in the <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrenderresult/fields/failures">Failures</a>  field. See an example on how to use it:
```csharp
using (Image image = Image.Load("example.dxf"))
            using (FileStream stream = new FileStream("output_example.pdf", FileMode.Create))
            {
                PdfOptions pdfOptions = new PdfOptions();

                CadRasterizationOptions cadRasterizationOptions =
					new CadRasterizationOptions();
                pdfOptions.VectorRasterizationOptions = cadRasterizationOptions;
                cadRasterizationOptions.PageWidth = 800;
                cadRasterizationOptions.PageHeight = 600;

                int idxError = 1;
                cadRasterizationOptions.RenderResult +=
					new CadRasterizationOptions.CadRenderHandler(
						delegate (CadRenderResult result)
                {

                    Console.WriteLine("Tracking results of exporting");

                    if (result.IsRenderComplete)
                        return;

                    Console.WriteLine("Have some problems:");

                    foreach (RenderResult rr in result.Failures)
                        Console.WriteLine(string.Format("{0}. {1}, {2}", idxError++, rr.RenderCode.ToString(), rr.Message));

                });

                Console.WriteLine("Exporting to pdf format");
                image.Save(stream, pdfOptions);
            }
```

## 3D object export support
3D objects in AutoCAD and other file formats can be exported using Aspose.CAD. The library uses viewpoint stored in the file - so exported image will appear just as what can be seen in AutoCAD immediately on loading the file. 
By default, only 2D objects are exported for AutoCAD files. To switch to 3D object export, set <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/typeofentities">TypeOfEntities</a> property of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions">CadRasterizationOptions</a> instance  to <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/typeofentities">TypeOfEntities</a>.Entities3D and perform export.
Note that IFC files do not have stored viewpoint information, so you have to provide observation point during export. See the example:
```csharp
            using (IfcImage ifcImage = (IfcImage)Image.Load("ifcimage.ifc"))
            {
                JpegOptions options = new JpegOptions();
                options.VectorRasterizationOptions = new CadRasterizationOptions();
                options.VectorRasterizationOptions.PageWidth = 1500;
                options.VectorRasterizationOptions.PageHeight = 1500;
		float xAngle = 45;
		float yAngle = 0;
		float zAngle = 180;
                ((CadRasterizationOptions)(options.VectorRasterizationOptions)).ObserverPoint = new ObserverPoint(xAngle, yAngle, zAngle);
                ((CadRasterizationOptions)(options.VectorRasterizationOptions)).DrawType = CadDrawTypeMode.UseObjectColor;
                ifcImage.Save("ifcrender.jpg", options);
            }
```

## Multithreading support.
All CAD files loaded by Aspose.CAD - the <a href="https://apireference.aspose.com/net/cad/aspose.cad/image/">Image</a> class instances - are independent and can be processed concurrently without problems. Though, manipulation with a single image should happen only within one thread.

## Coming soon - .Net Core support
.Net Standard will be supported in the near future, so there will be a version of the library with native .NET Core support and hence, multiplatform support not only in Java, but in .Net as well.

## Coming soon - Cloud version
A public REST API service that allows you to use Aspose.CAD, upload files to the API host, process them and download them back is in the works.


For now, that's all! This article will be updated with new and old interesting features in the future.
