---
author: Alfeze
created: 2026-10-28
---

# Digital Forensics Fundamentals

> Forensics is the application of methods and procedures to investigate and solve crimes. The branch of forensics that investigates cyber crimes is known as **digital forensics**. **Cyber crime** is any criminal activity conducted on or using a digital device. Several tools and techniques are used to investigate digital devices thoroughly after any crime to find and analyze evidence for necessary legal action.

---
## Digital Forensics Methodology

The National Institute of Standards and Technology (NIST) defines a general process for every case. 

The NIST works on defining frameworks for different areas of technology, including cyber security, where they introduce the process of digital forensics in four phases.

![[Pasted image 20261028162725.png|519]]
**NIST SP 800-86 :**

[Guide to Integrating Forensic Techniques into Incident Response](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-86.pdf)

## Types of Digital Forensics


![[Pasted image 20261028165756.png|437]]


---
## Evidence Acquisition

Collect evidence **without tampering** so it stays admissible in court. 

- **Authorization**: get approval (e.g. search warrant) before collecting, or evidence may be inadmissible. 
- **Chain of Custody**: document tracking who collected, accessed and stored the evidence, and when → proves integrity.
- **Write Blocker**: read-only access to the drive → prevents altering timestamps/data during collection.

---
## Popular Tools

 Some popular tools used for disk and memory image acquisition and analysis of the Windows operating system.

- **FTK Imager:** [FTK Imager](https://www.exterro.com/digital-forensics-software/ftk-imager) is a widely used tool for taking disk images of Windows operating systems. It offers a user-friendly graphical interface for creating the image in various formats. This tool can also analyze the contents of a disk image. It can be used for both acquisition and analysis purposes.

- **Autopsy:** [Autopsy](https://www.autopsy.com/) is a popular open-source digital forensics platform. An investigator can import an acquired disk image into this tool, and the tool will conduct an extensive analysis of the image. It offers various features during image analysis, including keyword search, deleted file recovery, file metadata, extension mismatch detection, and many more.

- **DumpIt:** [DumpIt](https://www.toolwar.com/2014/01/dumpit-memory-dump-tools.html) offers the utility of taking a memory image from a Windows operating system. This tool creates memory images using a command-line interface and a few commands. The memory image can also be taken in different formats.

- **Volatility:** [Volatility](https://volatilityfoundation.org/) is a powerful open-source tool for analyzing memory images. It offers some extremely useful plugins. Each artifact can be analyzed using a specific plugin. This tool supports various operating systems, including Windows, Linux, macOS, and Android.

- **TestDisk:** [TestDisk](https://www.cgsecurity.org/wiki/TestDisk) is a free, open-source command-line tool for recovering lost partitions and repairing damaged disks. It can rebuild partition tables, fix boot sectors, and undelete files on some file systems (FAT, NTFS, ext). Works on Windows, Linux and macOS.

- **PhotoRec:** [PhotoRec](https://www.cgsecurity.org/wiki/PhotoRec) is a companion tool to TestDisk (same developer) that recovers deleted files using **file carving**: it ignores the file system and searches the raw data for known file signatures (JPG, PDF, DOCX, ZIP, etc.). It still works when the file system is corrupted or formatted, but recovered files lose their original names and folder structure.

---
## pdfinfo

We can try to read the metadata using the program **pdfinfo**. **Pdfinfo** displays various metadata related to a PDF file, such as title, subject, author, creator, and creation date.

You can install it using the following command : 

```shell
sudo apt install poppler-utils
```

Consider the following example of using :

```shell
pdfinfo DOCUMENT.pdf
```


---
## Photo EXIF Data

EXIF stands for Exchangeable Image File Format; it is a standard for saving metadata to image files.
Whenever you take a photo with your smartphone or with your digital camera, plenty of information gets embedded in the image. 

The following are examples of metadata that can be found in the original digital images:

- Camera model / Smartphone model
- Date and time of image capture
- Photo settings such as focal length, aperture, shutter speed, and ISO settings

Because smartphones are equipped with a GPS sensor, finding GPS coordinates embedded in the image is highly probable. The GPS coordinates, i.e., latitude and longitude, would generally show the place where the photo was taken.

There are many online and offline tools to read the EXIF data from images. 

One command-line tool is `exiftool`. ExifTool is used to read and write metadata in various file types, such as JPEG images. 

You can install it using the following command :

```shell
sudo apt install libimage-exiftool-perl
```

In the following terminal window, we executed exiftool tor read all the EXIF data embedded in this image :

```shell
exiftool IMAGE.jpg
```

---
## Related 

- [[MOC_Security|Security]]
- [[MOC_Tools|Tools]]

