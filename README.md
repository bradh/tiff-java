# TIFF Java

## About

[TIFF](http://ngageoint.github.io/tiff-java/) is a Java library for reading and writing Tagged Image File Format files. It was primarily created to provide license friendly TIFF functionality to Android applications. Implementation is based on the [TIFF specification](https://partners.adobe.com/public/developer/en/tiff/TIFF6.pdf) and this JavaScript implementation: https://github.com/constantinius/geotiff.js

## Tagged Image File Format Lib

The [GeoPackage Libraries](http://ngageoint.github.io/GeoPackage/) were developed at the [National Geospatial-Intelligence Agency (NGA)](http://www.nga.mil/) in collaboration with [BIT Systems](https://www.caci.com/bit-systems/). The government has "unlimited rights" and is releasing this software to increase the impact of government investments by providing developers with the opportunity to take things in new directions. The software use, modification, and distribution rights are stipulated within the [MIT license](http://choosealicense.com/licenses/mit/).

## Usage

View the latest [Javadoc](http://ngageoint.github.io/tiff-java/docs/api/)

### Read Example

```java
//File input = ...
//InputStream input = ...
//byte[] input = ...
//ByteReader input = ...

TIFFImage tiffImage = TiffReader.readTiff(input);
List<FileDirectory> directories = tiffImage.getFileDirectories();
FileDirectory directory = directories.get(0);
Rasters rasters = directory.readRasters();
```

### Write Example

```java
int width = 256;
int height = 256;
int samplesPerPixel = 1;
FieldType fieldType = FieldType.FLOAT;
int bitsPerSample = fieldType.getBits();

Rasters rasters = new Rasters(width, height, samplesPerPixel, fieldType);

int rowsPerStrip = rasters.calculateRowsPerStrip(
        TiffConstants.PLANAR_CONFIGURATION_CHUNKY);

FileDirectory directory = new FileDirectory();
directory.setImageWidth(width);
directory.setImageHeight(height);
directory.setBitsPerSample(bitsPerSample);
directory.setCompression(TiffConstants.COMPRESSION_NO);
directory.setPhotometricInterpretation(
        TiffConstants.PHOTOMETRIC_INTERPRETATION_BLACK_IS_ZERO);
directory.setSamplesPerPixel(samplesPerPixel);
directory.setRowsPerStrip(rowsPerStrip);
directory.setPlanarConfiguration(
        TiffConstants.PLANAR_CONFIGURATION_CHUNKY);
directory.setSampleFormat(TiffConstants.SAMPLE_FORMAT_FLOAT);
directory.setWriteRasters(rasters);

for (int y = 0; y < height; y++) {
    for (int x = 0; x < width; x++) {
        float pixelValue = 1.0f; // any pixel value
        rasters.setFirstPixelSample(x, y, pixelValue);
    }
}

TIFFImage tiffImage = new TIFFImage();
tiffImage.add(directory);
byte[] bytes = TiffWriter.writeTiffToBytes(tiffImage);
// or
// File file = ...
// TiffWriter.writeTiff(file, tiffImage);

```

## Installation

Pull from the [Maven Central Repository](http://search.maven.org/#artifactdetails|mil.nga|tiff|2.0.2|jar) (JAR, POM, Source, Javadoc)

``` xml
<dependency>
    <groupId>mil.nga</groupId>
    <artifactId>tiff</artifactId>
    <version>2.0.2</version>
</dependency>
```

## Build

[![Build & Test](https://github.com/ngageoint/tiff-java/workflows/Build%20&%20Test/badge.svg)](https://github.com/ngageoint/tiff-java/actions/workflows/build-test.yml)

Build this repository using Eclipse and/or Maven:

``` bash
mvn clean install
```

## Pull Requests

If you'd like to contribute to this project, please make a pull request. We'll review the pull request and discuss the changes. All pull request contributions to this project will be released under the MIT license.

Software source code previously released under an open source license and then modified by NGA staff is considered a "joint work" (see 17 USC § 101); it is partially copyrighted, partially public domain, and as a whole is protected by the copyrights of the non-government authors and must be released according to the terms of the original open source license.
