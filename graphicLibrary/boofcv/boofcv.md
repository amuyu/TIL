# Converting Bitmap image
```java
// Easiest way to convert a Bitmap into a BoofCV type
GrayU8 image = ConvertBitmap.bitmapToGray(bitmap, (GrayU8)null, null);

// If you are converting a sequence of images it is faster reuse a
// previously declare image and buffer
byte[] workBuffer = ConvertBitmap.declareStorage(bitmap, null);
ConvertBitmap.bitmapToGray(bitmap, image, workBuffer);

// Convert back into a Bitmap
ConvertBitmap.grayToBitmap(image, bitmap, workBuffer);

// another less efficient way
bitmap = ConvertBitmap.grayToBitmap(image, Bitmap.Config.ARGB_8888);

// Functions are also provided for multi-spectral images
Planar<GrayF32> color = ConvertBitmap.bitmapToMS(bitmap, null, GrayF32.class, null);
```

# Converting NV21 image
```java
// from NV21 to gray scale
ConvertNV21.nv21ToGray(bytes,width,height,gray)

// from NV21 to YUV Multi-Spectral
ConvertNV21.nv21ToMsYuv(bytes,width,height,colorMS,GrayU8.class);
```


# line Detecting
```java
FactoryDetectLineAlgs.houghFoot(
						new ConfigHoughFoot(5,6,5,40,numLines),GrayU8.class,GrayS16.class);
```
