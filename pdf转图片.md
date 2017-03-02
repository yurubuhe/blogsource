---
title: pdf转图片
date: 2017-03-01 
tags: 工具
categories: java
---

#### pom.xml

当前最新版本
```java
<dependency>
	<groupId>org.apache.pdfbox</groupId>
	<artifactId>pdfbox</artifactId>
	<version>2.0.4</version>
</dependency>
<dependency>
	<groupId>org.apache.pdfbox</groupId>
	<artifactId>fontbox</artifactId>
	<version>2.0.4</version>
</dependency>
```
<!--more-->

#### pdf转图片

pdf转图片，可以转成一页的，也可以转成多页的。

```java
import java.awt.image.BufferedImage;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.ArrayList;
import java.util.List;

import javax.imageio.ImageIO;

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.rendering.ImageType;
import org.apache.pdfbox.rendering.PDFRenderer;

/**
 * Hello world!
 */
public class Pdf2ImageUtil {
	/**
	 * @Description pdf转成一张图片
	 * @created 2017年1月4日 下午1:54:13
	 * @param pdfFile
	 * @param outpath
	 */
	public static void pdf2multiImage(String pdfFile, String outpath) {
		try {
			InputStream is = new FileInputStream(pdfFile);
			PDDocument pdf = PDDocument.load(is);
			int actSize = pdf.getNumberOfPages();
			List<BufferedImage> piclist = new ArrayList<BufferedImage>();
			for (int i = 0; i < actSize; i++) {
				// renderImageWithDPI的数值越大，保真度越高，jpg的大小也越大
				BufferedImage image = new PDFRenderer(pdf).renderImageWithDPI(i, 150, ImageType.RGB);
				piclist.add(image);
			}
			yPic(piclist, outpath);
			is.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public static void pdf2multiImage(InputStream is,String outpath) {
		try {
			PDDocument pdf = PDDocument.load(is);
			int actSize = pdf.getNumberOfPages();
			List<BufferedImage> piclist = new ArrayList<BufferedImage>();
			for (int i = 0; i < actSize; i++) {
				BufferedImage image = new PDFRenderer(pdf).renderImageWithDPI(i, 130, ImageType.RGB);
				piclist.add(image);
			}
			yPic(piclist, outpath);
			is.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	
	public static byte[] pdf2multiImage(InputStream is) {
		byte[] bufs = null;
		try {
			PDDocument pdf = PDDocument.load(is);
			int actSize = pdf.getNumberOfPages();
			List<BufferedImage> piclist = new ArrayList<BufferedImage>();
			for (int i = 0; i < actSize; i++) {
				BufferedImage image = new PDFRenderer(pdf).renderImageWithDPI(i, 130, ImageType.RGB);
				piclist.add(image);
			}
			ByteArrayOutputStream out = new ByteArrayOutputStream();
			yPic(piclist, out);
			bufs = out.toByteArray();
			is.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return bufs;
	}

	/**
	 * 将宽度相同的图片，竖向追加在一起 ##注意：宽度必须相同
	 * 
	 * @param piclist
	 *            文件流数组
	 * @param outPath
	 *            输出路径
	 */
	public static void yPic(List<BufferedImage> piclist, String outPath) {// 纵向处理图片
		if (piclist == null || piclist.size() <= 0) {
			System.out.println("图片数组为空!");
			return;
		}
		try {
			int height = 0, // 总高度
					width = 0, // 总宽度
					_height = 0, // 临时的高度 , 或保存偏移高度
					__height = 0, // 临时的高度，主要保存每个高度
					picNum = piclist.size();// 图片的数量
			int[] heightArray = new int[picNum]; // 保存每个文件的高度
			BufferedImage buffer = null; // 保存图片流
			List<int[]> imgRGB = new ArrayList<int[]>(); // 保存所有的图片的RGB
			int[] _imgRGB; // 保存一张图片中的RGB数据
			for (int i = 0; i < picNum; i++) {
				buffer = piclist.get(i);
				heightArray[i] = _height = buffer.getHeight();// 图片高度
				if (i == 0) {
					width = buffer.getWidth();// 图片宽度
				}
				height += _height; // 获取总高度
				_imgRGB = new int[width * _height];// 从图片中读取RGB
				_imgRGB = buffer.getRGB(0, 0, width, _height, _imgRGB, 0, width);
				imgRGB.add(_imgRGB);
			}
			_height = 0; // 设置偏移高度为0
			// 生成新图片
			BufferedImage imageResult = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
			for (int i = 0; i < picNum; i++) {
				__height = heightArray[i];
				if (i != 0)
					_height += __height; // 计算偏移高度
				imageResult.setRGB(0, _height, width, __height, imgRGB.get(i), 0, width); // 写入流中
			}
			File outFile = new File(outPath);
			ImageIO.write(imageResult, "jpg", outFile);// 写图片
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	
	/**
	 * 将宽度相同的图片，竖向追加在一起 ##注意：宽度必须相同
	 * 
	 * @param piclist
	 *            文件流数组
	 * @param outPath
	 *            输出路径
	 */
	public static void yPic(List<BufferedImage> piclist, OutputStream output) {// 纵向处理图片
		if (piclist == null || piclist.size() <= 0) {
			System.out.println("图片数组为空!");
			return;
		}
		try {
			int height = 0, // 总高度
					width = 0, // 总宽度
					_height = 0, // 临时的高度 , 或保存偏移高度
					__height = 0, // 临时的高度，主要保存每个高度
					picNum = piclist.size();// 图片的数量
			int[] heightArray = new int[picNum]; // 保存每个文件的高度
			BufferedImage buffer = null; // 保存图片流
			List<int[]> imgRGB = new ArrayList<int[]>(); // 保存所有的图片的RGB
			int[] _imgRGB; // 保存一张图片中的RGB数据
			for (int i = 0; i < picNum; i++) {
				buffer = piclist.get(i);
				heightArray[i] = _height = buffer.getHeight();// 图片高度
				if (i == 0) {
					width = buffer.getWidth();// 图片宽度
				}
				height += _height; // 获取总高度
				_imgRGB = new int[width * _height];// 从图片中读取RGB
				_imgRGB = buffer.getRGB(0, 0, width, _height, _imgRGB, 0, width);
				imgRGB.add(_imgRGB);
			}
			_height = 0; // 设置偏移高度为0
			// 生成新图片
			BufferedImage imageResult = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
			for (int i = 0; i < picNum; i++) {
				__height = heightArray[i];
				if (i != 0)
					_height += __height; // 计算偏移高度
				imageResult.setRGB(0, _height, width, __height, imgRGB.get(i), 0, width); // 写入流中
			}
			ImageIO.write(imageResult, "jpg", output);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### 压缩

通常转完之后，会很大，需要压缩一下，才能方便传输。

```java
<dependency>
	<groupId>net.coobird</groupId>
	<artifactId>thumbnailator</artifactId>
	<version>0.4.8</version>
</dependency>
```

```java
Thumbnails.of("D:\\xxx.jpg").scale(0.7f).outputQuality(0.25f).toFile("D:\\123.jpg");
```
