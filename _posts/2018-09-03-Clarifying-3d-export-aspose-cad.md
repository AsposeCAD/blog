---
title: Clarifying export of 3D entities with Aspose.CAD
layout: post
tags: csharp, java, cad, export
---

<a href="https://products.aspose.com/cad">Aspose.CAD</a> supports export of 3D entities from AutoCAD and STL files. For other formats, there is currently (as of 18.8) no support for 3D entities. 

One can specify which entities to export using <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions">CadRasterizationOptions</a>'s <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/typeofentities">TypeOfEntities</a> property - it's 
of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/typeofentities/">TypeOfEntities</a> enum type, accepting either Entities3D or Entities2D (default) value. See the example:
```csharp
using (Aspose.CAD.Image cadImage = Aspose.CAD.Image.Load("image.dwg"))
{
    Aspose.CAD.ImageOptions.CadRasterizationOptions rasterizationOptions = new
	Aspose.CAD.ImageOptions.CadRasterizationOptions();
    rasterizationOptions.PageWidth = 500;
    rasterizationOptions.PageHeight = 500;
    rasterizationOptions.TypeOfEntities = TypeOfEntities.Entities3D;
    
    
    PdfOptions pdfOptions = new PdfOptions();
    pdfOptions.VectorRasterizationOptions = rasterizationOptions;
   
    cadImage.Save("Export3DImagestoPDF_out.pdf", pdfOptions);
}
```

For AutoCAD files, either 3D entities or 2D entities will be exported, according to specified TypeOfEntities, and not both. If 3D entities are selected for export, output image will be automatically centered to them - same way as when setting CenterDrawing property of CadRasterizationOptions to <b>true</b> - geometric center of all 3D entities in image is determined and is pinned to output image's center.

For an STL file, you may not specify TypeOfEntities - it will be automatically set to Entities3D, as STL only has 3D entities anyway. Centering happens the same way.

For other file formats, specifying 3D entities for export will only cause centering - this is a default behaviour for all formats.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
