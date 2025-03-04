# webp-imageio

[![Build](https://github.com/usefulness/webp-imageio/actions/workflows/after_merge.yml/badge.svg?branch=master)](https://github.com/usefulness/webp-imageio/actions/workflows/after_merge.yml)
![Maven Central](https://img.shields.io/maven-central/v/com.github.usefulness/webp-imageio)
![Static Badge](https://img.shields.io/badge/java-8-blue)

## Description

[Java Image I/O](https://docs.oracle.com/en/java/javase/20/docs/api/java.desktop/javax/imageio/package-summary.html) reader and writer for the
[Google WebP](https://developers.google.com/speed/webp/) image format.

### Highlights:
- `macos-aarch64` architecture support (ARM chipsets from Apple, M1, M2) ✅
- Sharp YUV option support ✅
- Written in Kotlin

### Supported platforms

See the [full list](https://github.com/usefulness/webp-imageio/tree/master/webp-imageio/src/main/resources/native) of supported platforms

## Usage

1. Add dependency `com.github.usefulness:webp-imageio` to your application
```groovy
dependencies {
    runtimeOnly("com.github.usefulness:webp-imageio:x.y.z")
}
```
2. The WebP reader and writer can be used like any other Image I/O reader and writer.

### Decoding

WebP images can be decoded using default settings as follows.

```
BufferedImage image = ImageIO.read(new File("input.webp"));
```

To customize the WebP decoder settings you need to create instances of ImageReader and WebPReadParam.

```java
// Obtain a WebP ImageReader instance
ImageReader reader = ImageIO.getImageReadersByMIMEType("image/webp").next();

// Configure decoding parameters
WebPReadParam readParam = new WebPReadParam();
readParam.setBypassFiltering(true);

// Configure the input on the ImageReader
reader.setInput(new FileImageInputStream(new File("input.webp")));

// Decode the image
BufferedImage image = reader.read(0, readParam);
```

### Encoding

Encoding is done in a similar way to decoding.

You can either use the Image I/O convenience methods to encode using default settings.

<details open>
<summary>Kotlin</summary>

```kotlin
// Obtain an image to encode from somewhere
val image = ImageIO.read(File("input.png"))

// Encode it as webp using default settings
ImageIO.write(image, "webp", File("output.webp"))
```
</details>

<details>
<summary>Java</summary>

```java
// Obtain an image to encode from somewhere
BufferedImage image = ImageIO.read(new File("input.png"));

// Encode it as webp using default settings
ImageIO.write(image, "webp", new File("output.webp"));
```
</details>

Or you can create an instance of ImageWriter and WebPWriteParam to use custom settings.

<details open>
<summary>Kotlin</summary>

```kotlin
// Obtain an image to encode from somewhere
val image = ImageIO.read(File("input.png"))

// Obtain a WebP ImageWriter instance
val writer = ImageIO.getImageWritersByMIMEType("image/webp").next()

// Configure encoding parameters
val writeParam = (writer.defaultWriteParam as WebPWriteParam).apply {
    compressionType = CompressionType.Lossy
    alphaCompressionAlgorithm = 1
    useSharpYUV = true
}

// Configure the output on the ImageWriter
writer.output = FileImageOutputStream(File("output.webp"))

// Encode
writer.write(null, IIOImage(image, null, null), writeParam)
```
</details>

<details>
<summary>Java</summary>

```java
// Obtain an image to encode from somewhere
BufferedImage image = ImageIO.read(new File("input.png"));

// Obtain a WebP ImageWriter instance
ImageWriter writer = ImageIO.getImageWritersByMIMEType("image/webp").next();

// Configure encoding parameters
WebPWriteParam writeParam = ((WebPWriteParam) writer.getDefaultWriteParam());
writeParam.setCompressionType(CompressionType.Lossy);
writeParam.setAlphaCompressionAlgorithm(3);
writeParam.setUseSharpYUV(true);

// Configure the output on the ImageWriter
writer.setOutput(new FileImageOutputStream(new File("output.webp")));

// Encode
writer.write(null, new IIOImage(image, null, null), writeParam);
```
</details>

## License

`webp-imageio` is distributed under the [Apache Software License](https://www.apache.org/licenses/LICENSE-2.0) version
2.0.  
`libwebp` binaries is distributed under the [Following License](https://chromium.googlesource.com/webm/libwebp/+/refs/tags/v1.3.0/COPYING)
