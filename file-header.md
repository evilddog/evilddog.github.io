title: 各类文件的文件头
date: 2014-11-09 15:39:21
tags:

---

the hex header of some common files
<!--more -->

##0x00 各种常用文件的文件头格式 

MISC还有信息隐藏经常有各种蛋疼的图片隐藏还有修复图片的题目,简直不会,所以记下备忘


##0x01



```
File                             Type                          ExtensionsHeader
               
JPEG                             jpg;jpeg                      0xFFD8FF
PNG                              png                           0x89504E470D0A1A0A
GIF                              gif                           47494638 
TIFF                             tif;tiff                      0x49492A00
TIFF                             tif;tiff                      0x4D4D002A
Bit   map                        bmp                           BM
AOL ART                          art                           0x4A47040E000000
PC Paintbrush                    pcx                           0x0A050108
Graphics Metafile                wmf                           0xD7CDC69A
Graphics Metafile                wmf                           0x01000900
Graphics Metafile                wmf                           0x02000900
Enhanced Metafile                emf                           0x0100000058000000
Corel Draw                       cdr                           CDR
CAD                              dwg                           0x41433130
Adobe Photoshop                  psd                           8BPS
Rich Text Format                 rtf                           rtf
XML                              xml                           3C3F786D6C   
HTML                 html;htm;php;php3;php4;phtml;shtml        68746D6C3E 
Email                            eml                           Delivery-date:
Outlook Express                  dbx                           0xCFAD12FE
MS Office/OLE2     doc;xls;dot;ppt;xla;ppa;pps;pot;msi;sdw;db  0xD0CF11E0A1B11AE1
MS Access                        mdb;mda;mde;mdt               Standard J
WordPerfect                      wpd                           0xFF575043
OpenOffice Writer                sxw                           writer
OpenOffice Calc                  sxc                           calc
OpenOffice Math                  sxm                           math
OpenOffice Impress               sxi                           impress
OpenOffice Draw                  sxd                           draw
Adobe FrameMaker                 fm                            <MAKERFILE
PostScript                eps.or.ps;ps;eps                     %!PS-Adobe
Adobe Acrobat                    pdf                           %PDF-1.
Quicken                          qdf                           0xAC9EBD8F
QuickBooks Backup                qbb                           0x458600000600
Sage                sly.or.srt.or.slt;sly;srt;slt              0x53520100
Sage Backup                      1                             SAGEBACKUP
Lotus WordPro v9                 lwp                           0x576F726450726F
Lotus 123 v9                     123                           0x00001A00051004
Lotus 123 v5                     wk4                           0x00001A0002100400
Lotus 123 v3                     wk3                           0x00001A0000100400
Lotus 123 v1                     wk1                           0x2000604060
Windows Password                 pwl                           0xE3828596
ZIP Archive                      zip;jar                       0x504B0304
ZIP Archive (outdated)           zip                           0x504B3030
RAR Archive                      rar                           Rar!
GZ Archive                       gz;tgz                        0x1F8B08
BZIP Archive                     bz2                           BZh
ARJ Archive                      arj                           0x60EA
7－ZIP Archive                   7z                            7z集'
Wave                             wav                           WAVE
AVI                              avi                           AVI 
Real Audio                       ram;ra                        .ra?0
Real Media                       rm                            .RMF
MPEG                             mpg;mpeg                      0x000001BA
MPEG                             mpg;mpeg                      0x000001B3
Quicktime                        mov                           moov
Windows Media                    asf                           0x3026B2758E66CF11
MIDI                             mid                           MThd
ELF Executable                   elf;;                          0x7F454C4601010100
```