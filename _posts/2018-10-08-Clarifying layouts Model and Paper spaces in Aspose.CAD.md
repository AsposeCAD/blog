In AutoCAD workflow, there is concept of separate model and paper spaces. In model space, you create entities as they should physically appear, with real-world dimensions etc. To create an printable drawing, you switch to paper space and create layouts which contain viewports that display different views of model space, and various annotations. Here dimensions correspond to dimensions you'll see in actual paper print. Model space is accessible from Model tab, and paper space is accessible from layout tabs. 


## How to see my paper space layouts using Aspose.CAD?

Aspose.CAD supports all that, but has a bit different terminology. Both model space and paper space's layouts are called layouts, and model space just has a specific layout name - "Model". To select which layout to export, create an array of layout names (of string type, of course) and set <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/">CadRasterizationOptions</a>'s instance's <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions/properties/layouts">Layouts</a> property with that array. Then all specified layouts will be exported at once as different pages, so if you do want to export several layouts at once, use PDF or TIFF as output format. See the following example:
```csharp
using (Aspose.CAD.Image image = Aspose.CAD.Image.Load("source.dwg"))
{
    // Create an instance of CadRasterizationOptions
    Aspose.CAD.ImageOptions.CadRasterizationOptions rasterizationOptions = new Aspose.CAD.ImageOptions.CadRasterizationOptions();

    // Set page width & height
    rasterizationOptions.PageWidth = 1200;
    rasterizationOptions.PageHeight = 1200;

    // Specify a list of layout names
    rasterizationOptions.Layouts = new string[] { "Model", "Layout1" };

    // Create an instance of TiffOptions for the resultant image
    ImageOptionsBase options = new Aspose.CAD.ImageOptions.TiffOptions(Aspose.CAD.FileFormats.Tiff.Enums.TiffExpectedFormat.Default);

    // Set rasterization options
    options.VectorRasterizationOptions = rasterizationOptions;

    // Save resultant image
    image.Save("", options);                
}
```
As we can see, here we export model space and paper space's "Layout1" layout to two separate pages. By default, Layouts property is null and only model space, i.e. "Model" layout in Aspose.CAD's terms, is exported, as a single page, obviously. Logically, same will happen if you will set it with an array containing only one "Model" entry. To export other layouts, you have to specify their names, and for that, you obviously need to know them. Given that, a question arises:


## So how do I know my layouts' names?

Thankfully, it is not a problem. <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/">CadImage</a> class' instances have a <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadimage/properties/layouts">Layouts</a> property, which contains a <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad/cadlayoutdictionary">CadLayoutDictionary</a>, that is just an dictionary with string keys and <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadlayout">CadLayout</a> values. This dictionary contains all the layouts that are present in file, including Model layout. CadLayout is used for internal representation of an AutoCAD layout in Aspose.CAD. Keys are corresponding layout's names. Layout names are also accessible via <a href="https://apireference.aspose.com/net/cad/aspose.cad.fileformats.cad.cadobjects/cadlayout/properties/layoutname">LayoutName</a> property of CadLayout. The following example will print all of file's layouts to console:
```csharp
using (Aspose.CAD.Image image = Aspose.CAD.Image.Load("source.dwg"))
{
    Aspose.CAD.FileFormats.Cad.CadImage cadImage = (Aspose.CAD.FileFormats.Cad.CadImage)image;

    Aspose.CAD.FileFormats.Cad.CadLayoutDictionary layouts = cadImage.Layouts;
    foreach (Aspose.CAD.FileFormats.Cad.CadObjects.CadLayout layout in layouts.Values)
    {
		//Console output:
		//Model
		//Layout1
        Console.WriteLine(layout.LayoutName); 
    }
}
``` 

Now we have everything we need to export layouts as we want.


## Exporting all paper space layouts from file.

So we get all layout names, filter "Model" out of them, and then export all remaining layouts:
```csharp
using (Aspose.CAD.Image image = Aspose.CAD.Image.Load("source.dwg"))
{
    Aspose.CAD.FileFormats.Cad.CadImage cadImage = (Aspose.CAD.FileFormats.Cad.CadImage)image;

    List<string> layoutNames = new List<string>();

    Aspose.CAD.FileFormats.Cad.CadLayoutDictionary layouts = cadImage.Layouts;
    foreach (Aspose.CAD.FileFormats.Cad.CadObjects.CadLayout layout in layouts.Values)
    {
        if (layout.LayoutName != "Model")
            layoutNames.Add(layout.LayoutName);
    }

    Aspose.CAD.ImageOptions.PdfOptions pdfOptions = new Aspose.CAD.ImageOptions.PdfOptions();

    Aspose.CAD.ImageOptions.CadRasterizationOptions cadOptions = new ImageOptions.CadRasterizationOptions();

    cadOptions.PageWidth = 1000;
    cadOptions.PageHeight = 1000;

    cadOptions.Layouts = layoutNames.ToArray();

    pdfOptions.VectorRasterizationOptions = cadOptions;

    cadImage.Save("output.pdf", pdfOptions);
}
```

Voila, we now have a PDF file, which contains all the paper space layouts from AutoCAD file as separate pages.

That's all for now, stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.

