A new version of Aspose.CAD, designated 18.6 is soon to be released. Here are the most important changes:

## New features

### Basic support of IGES file format
Now Aspose.CAD can load and render 2D IGES files. The support is still in early stages, so most likely, many files won't be properly rendered. If so, please send the files to us at Aspose via <a href="https://forum.aspose.com/">support forum</a>.

## Improvements

### Proxy entity support for DWG/DXF files
Proxy entity support was partially present before, but it was incomplete, and entity properties weren't resolved correctly, now it's correct. 

### Improvement of hatches rendering for DWG/DXF files.
Related to proxy entity support, hatches rendering has been improved now.

## Bug fixes

### Corrected render of dashed lines for raster formats.
Render of thick dashed lines when exporting to raster formats has been fixed.

### Render options now work when exporting to raster formats.
Now, antialiasing, interpolation and text rendering options can be used when exporting into raster formats by setting corresponding properties of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/vectorrasterizationoptions/properties/graphicsoptions">GraphicsOptions</a> of <a href="https://apireference.aspose.com/net/cad/aspose.cad.imageoptions/cadrasterizationoptions">CadRasterizationOptions</a> used to export an image. They were availible beforehand, but did not work with raster images.



Stay tuned!

For more examples please visit the <a href="https://github.com/aspose-cad">Aspose.CAD GitHub</a> page. There's also <a href="https://twitter.com/Asposecad">Twitter</a> and <a href="https://www.facebook.com/AsposeCAD">Facebook</a> pages for news on Aspose.CAD.
