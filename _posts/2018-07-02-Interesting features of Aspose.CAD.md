---
title: Interesting features of Aspose.CAD
layout: post
tags: csharp, java, cad, convert
---


Hello, in this article I'm going to list what distinguishes Aspose.CAD from other similar software and generally about it's interesting features.


## Normalisation to absolute metrics for export
By default, Aspose.CAD works with relative units of the drawing. However, there is a <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/unittype">UnitType</a> property in <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/">CadRasterizationOptions</a> class, which specifies used unit type. During render, a unit is interpreted as 10x10 pixels, so if an image has specified size of 10x10 m, it will be rasterized to 1000x1000 pixel image.
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

## Coming soon - .Net Core support
.Net Standard will be supported in the near future, so there will be a version of the library with native .NET Core support and hence, multiplatform support not only in Java, but in .Net as well.

## Coming soon - Cloud version
A public REST API service that allows you to use Aspose.CAD, upload files to the API host, process them and download them back is in the works.


For now, that's all! This article will be updated with new and old interesting features in the future.
