nmaahc mediamicroservices
==================

table of contents
-------------------

1. [summary](https://github.com/aeschweik/nmaahcmm/mm#summary)
	* [License](#license)


***

## summary

This set of mediamicroservices (mm) have been developed for the purpose of processing digital audiovisual collections at the National Museum of African American History and Culture (NMAAHC). They are based on a set of microservices developed for use at CUNY Television. These microservices borrow both concepts and code from those scripts, which are available in [the mediamicroservices GitHub repository](https://github.com/mediamicroservices/mm/).

Like CUNY TV's original mediamicroservices, these microservices are written in Bash, developed and tested for a macOS environment, and installed and run using the terminal application.

***

## nmaahcmm functions and instructions for use

For all microservices, the structure of the command looks like this:
`[microservice] -options [input]`
* the microservice is the particular command you want to execute (for example, checksumpackage)
* options are any non-default choices that the script may contain
* the input is the package, directory, or file that you are working with

Across all mediamicroservices, you can always receive the usage information by typing the microservice and -h. Your command will look like this: `[microservice] -h`.

To view the specific ffmpeg encoding options for each file, view the sourcecode of the microservice directly on GitHub or using a text editor on your computer.

#### diffFrameMD5
* This application will compare frame MD5s between two video files. It takes two text files as arguments. The text files must list the frame MD5s of the relevant video files. The results of a diff command will be displayed within the terminal.
* Your command will look like this: `diffFrameMD5 file1.txt file2.txt`
  
#### ingestfile
* This script will run an interactive interview and then process an input file accordingly.
* Your command will look like this: `ingestfile fileorpackage1 [ fileorpackage2 ...]`
  
#### makechecksum
* This application creates md5 checksums. If you pass a single file as input, the application will create a checksum for that file and save it in a .md5 file in the same directory as your input. If you pass a directory as input, the application will create a checksum for every file in the directory and save them in .md5 files (one .md5 file for each original file) in the same directory that you supplied.
* Your command will look like this: `makechecksum fileorpackage1 fileorpackage2 [fileorpackage3 ...]`
  
#### makeconcat
* This application will concatenate multiple video files into a single video file. It takes two or more video files as arguments, all of which must be stored in the same directory. Be sure to list the video files in order. You will be asked to supply the name of the new concatenated file.
* Your command will look like this: `makeconcat file1 file2 [ file3... ]`
  
#### makeDer
* This application will create a high quality h264 file (suitable for uploading to YouTube). It takes a video file or package as input.
    * If the input is a file, the output will be named after the file's containing directory, which is assumed to be the media ID.
    * If the input is a directory, the script will create derivatives for all .mov files in the directory, and the output files will be named after those files.
* The script will send these files to a directory called "derivatives" that it creates within the input directory or containing directory. You can override this location and set the directory where the output should be sent with the -o flag. 
* Your command will look like this: `makeDer [ -o /output/directory/path/ ] fileorpackage1 [ fileorpackage2 ...]`
  
#### makedrivetree
* This application will write the contents of a drive directory in tree form and save it to a .txt file. It takes one or more drives as input.
* Your command will look like this: `makedrivetree /path/to/drive1 [ /path/to/drive2 ... ]`
  
#### makeframemd5
* This application will generate frame MD5s for all frames in a video file. It takes a video file or package as an argument. (If a package, the application will generate frame MD5s for all .mov files in the package.) The frame MD5s will be stored as text in a file with extension .framemd5.md5.
* Your command will look like this: `makeframemd5 fileorpackage`
  
#### makeH264
* This application will create a high quality h264 file (suitable for uploading to YouTube). It takes one or more video files or packages as input. You may pass both video files and packages in the same command.
    * If the input is a file, the script will create derivatives for all video files (regardless of extension) that are passed as input. Each derivative will be named after its input file. Derivatives will be created in a subdirectory called 'derivative' located within the input file's parent directory.
    * If the input is a package, the script will create derivatives for all .mov and .mkv files within the input package. Each derivative will be named after its corresponding .mov or .mkv file. Derivatives will be created in a subdirectory called 'derivative' located within the input directory.
* Your command will look like this: `makeH264 fileorpackage1 [ fileorpackage2 ...]`
  
#### makemetadata
* This application will generate metadata sidecar files for all video files in a package. It takes one or more packages as input. The application generates all of the following files: a tree XML, a MediaInfo XML, a MediaTrace XML, an FFprobe XML, and an Exiftool XML.
* This script takes a set of options. Options can be combined, and the order of the options does not matter, as long as they are in between "makemetadata" and your input. None of the options are required.
    * -m: generate an MD5 sidecar file. This file may take a while to generate, depending on the size of your video file.
        * Your command will look like this: `makemetadata -m package`
    * -q: generate a QCTools XML as well (this option is recommended for digitized video only). This file may take a while to generate, depending on the size of your video file.
        * Your command will look like this: `makemetadata -q package`
    * -o: overwrite any existing metadata files.
        * Your command will look like this: `makemetadata -o package`
    * -e: embed a copy of the metadata files generated by the script into an MKV wrapper (only for MKV files).
        * Your command will look like this: `makemetadata -e package`
* Your overall command will look like this: `makemetadata [ -e | -m | -o | -q ] package1 [ package2 ...]`
  
#### moveDPX
* This application will copy .md5 checksum files and the first .dpx file in a stack to a designated directory. It takes one or more DPX directories as arguments.
* Your command will look like this: `moveDPX dpxdirectory1 [ dpxdirectory2 ...]`
  
#### nmaahcmmfunctions
* This script does not perform any actions. It functions as a central repository for functions and other code that are called by multiple scripts. Because each microservice calls those shared functions from the same location, the functions can be changed and maintained in one place instead of in multiple scripts.
* You do not need to call this script yourself.
  
#### removeDSStore
* removeDSStore is a script to remove hidden .DS\_Store files from a package input.
* Your command will look like this: `removeDSStore [input]`
  
#### restructureDPX
* This application will create the desired directory structure and filenames for DPX packages received from VFS.
* This script takes a set of options. Options can be combined, and the order of the options does not matter, as long as they are in between "restructureDPX" and your input. Some options require an argument following immediately after the option flag (e.g., -d must be followed with the path of the directory you specify: `-d [directory/path]`); these arguments are detailed below. None of the options are required to run the command; the script will interview the operator if any of the options are left out.
    * -d: specify an output directory to deliver the resulting package
        * Your command will look like this: `restructureDPX -d /path/to/deliver/to/ dpxpackage`
    * -i: specify a DPX Object ID (ex. 2012\_79\_1\_54)
        * Your command will look like this: `restructureDPX -i 2012_79_1_54 dpxpackage`
    * -t: specify a DPX Object Title (ex. Something\_to\_Build\_On)
        * Your command will look like this: `restructureDPX -t Something_to_Build_On dpxpackage`
    * -r: specify the number of reels in the package, up to 3
        * If you have 3 reels, your command will look like this: `restructureDPX -r 3 dpxpackage`
    * -c: cleanup source files (remove all source files after script runs)
        * Your command will look like this: `restructureDPX -c dpxpackage`
* Your overall command will look like this: `restructureDPX [ -d /path/to/deliver/to/ ] [ -i DPX_Object_ID ] [ -t DPX_Object_Title ] [ -r (number of reels) ] [ -c ] dpxpackage`

  
#### restructureForVFCU
* This application will restructure a DPX package directory for VFCU pickup and place .md5s in a directory. It takes one or more DPX directories as arguments.
* Your command will look like this: `restructureForVFCU DPXdirectory1 [ DPXdirectory2 ... ]`
  
#### restructureSIP
* This script will restructure a SIP into an AIP based on the type of package being submitted. The script will do the following:
    * create an output AIP directory named after an operator-selected media id and located in an operator-selected destination directory
    * create 'objects' and 'metadata' subcategories within that output AIP directory
    * rsync files from the SIP into the AIP
    * remove SIP files or leave them in place upon successful completion, depending upon operator selection
* This script takes a set of options. Options can be combined, and the order of the options does not matter, as long as they are in between "restructureDPX" and your input. Some options require an argument following immediately after the option flag (e.g., -d must be followed with the path of the directory you specify: `-d [directory/path]`); these arguments are detailed below. None of the options are required to run the command; the script will interview the operator if any of the options are left out.
    * -m: specify MEDIAID (type media id for final package, e.g. SC0001\_20190101\_SMITH\_VHS\_01)
        * Your command will look like this: `restructureSIP SC0001_20190101_SMITH_VHS_01 /path/to/SIP`
    * -o: specify an output directory to deliver the resulting package
        * Your command will look like this: `restructureSIP -o /path/to/output/directory /path/to/SIP`
    * -r: remove source files after successful ingest
        * Your command will look like this: `restructureSIP -r /path/to/SIP`
    * There is also a set of options available that specify a package type. Choose a package type (required) from one of the following options:
        * -x Package type: Digitized Film (DPX package)
        * -f Package type: Digitized Film (MOV and/or MP4 files only)
        * -v Package type: Digitized Analog Video (vrecord package)
        * -d Package type: Transferred DV (MOV and/or MP4 files)
        * -u Package type: Other/Unknown
        * If you have a DPX package as your input, your command will look like this: `restructureSIP -x /path/to/SIP`
* Your overall command will look like this: `restructureSIP [ -x | -f | -v | -d | -u | -r ] [ -m MEDIAID ] [ -o /path/to/output/directory ] /path/to/SIP`
  
#### verifySIP
* This application will create a temporary tree of a SIP and compare it against an expected directory structure, using a series of xpath statements. It outputs mismatches and unexpected items found in AIPs into the terminal window.
    * The script applies rules based on what kind of package it thinks it's looking at. The packages are defined as follows:
        * film / analog / digitized in-house: script looks for .dpx files
        * film / analog / digitized on-location: script looks for directories named after derivatives, e.g. 'MP4_2048x1152'
        * video / analog / digitized on-location: script looks for the capture_options.log and/or qctools.xml.gz files generated in the vrecord process
        * video / DV / transferred on-location: script looks for the string 'DV' in filenames
        * If your package does not fall into one of the above categories, the script will apply a simpler baseline set of comparisons.
* Your command will look like this: `verifySIP package`
  

***

## nmaahcmm package definitions
  
The scripts in nmaahcmm take the principles of the Open Archival Information System as inspiration. The content in its form before ingest workflows is called the Submission Information Package (SIP). Once that content comes under archival management (i.e., once it has been normalized and restructured, and metadata has been generated), it becomes an Archival Information Package (AIP). Using automated processes with SIPs and AIPs requires that the packages be predictable; a script has to be able to expect certain files in order to categorize them and run the same processes every time.  
  
The documentation below sets expectations for the types of files that are created and submitted in accession processes for different media formats. The following scripts rely explicitly on these structures and must be updated if the definitions change:  [restructureSIP](https://github.com/aeschweik/nmaahcmm/restructureSIP) and [verifySIP](https://github.com/aeschweik/nmaahcmm/verifySIP)  
  
#### definitions
* Package (SIP) = "as-submitted" directory containing the content information and preservation description information generated upon digitization of one or more media items.  
* Package (AIP) = finalized directory with structured/fleshed-out content information (in "objects" folder) and preservation description information (in "metadata" folder) for one media item (e.g. one tape or one film).  
* Collection = parent directory containing all packages that belong to the same contributor. Collections are grouped by last name and can encompass any number of format of media item.  
* Name_Of_Event and Digitization_Location = NMAAHC digitizes both in-house and on the road. This information is included in package structures to provide context on when and where the files were created.  
  
#### proposed AIP directory structure
  
├── Digitization_Location  
│   ├── Name_Of_Event  
│   │   ├── Collection name: SC0001_YYYYMMDD_LASTNAME1  
│   │   │   ├── AIP name: SC0001_YYYYMMDD_LASTNAME1_FORMAT_ITEM#  
│   │   │   │   ├── objects  
│   │   │   │   │   └── videofiles.xyz  
│   │   │   │   └── metadata  
│   │   │   │   │   └── metadatafiles.xyz  
│   │   ├── Collection name: SC0001_YYYYMMDD_LASTNAME2  
│   │   │   ├── AIP name: SC0001_YYYYMMDD_LASTNAME2_FORMAT_ITEM#  
│   │   │   │   ├── objects  
│   │   │   │   │   └── videofiles.xyz  
│   │   │   │   └── metadata  
│   │   │   │   │   └── metadatafiles.xyz  
  
  
Example AIP directory structures:  


├── SC0001_YYYYMMDD_LASTNAME1_S8_01_03  
│   ├── objects  
│   │   ├── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.mp4  
│   │   └── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.mov  
│   ├── metadata  
│   │   ├── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.ffprobe.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.mediainfo.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.mediatrace.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.exiftool.xml  
│   │   └── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.md5  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2  
├── SC0001_YYYYMMDD_LASTNAME2_VHS_01  
│   ├── objects  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mkv  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mp4  
│   ├── metadata  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.framemd5  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.ffprobe.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mediainfo.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mediatrace.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.exiftool.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mkv.qctools.xml.gz  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01_QC_output_graphs.jpeg  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01_capture_options.log  
│   │   └── SC0001_YYYYMMDD_LASTNAME2_VHS_01_ffmpeg_decklink_input.log  
├── SC0001_YYYYMMDD_LASTNAME2_VHS_05  
│   ├── objects  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mkv  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mp4  
│   ├── metadata  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.framemd5  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.ffprobe.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mediainfo.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mediatrace.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.exiftool.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mkv.qctools.xml.gz  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05_QC_output_graphs.jpeg  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05_capture_options.log  
│   │   └── SC0001_YYYYMMDD_LASTNAME2_VHS_05_ffmpeg_decklink_input.log  
│   │   ├── SC0001_YYYYMMDD_LASTNAME3  
├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02  
│   ├── objects  
│   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02.mov  
│   │   └── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02.mp4  
│   ├── metadata  
│   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02.md5  
│   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02.ffprobe.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02.mediainfo.xml  
│   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02.mediatrace.xml  
│   │   └── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02.exiftool.xml  
  
  
#### current SIP structures
MEDIA TYPE / source format / provenance  
  
*** FILM / analog / digitized in-house ***  
TK: DPX package example  
  
*** FILM / analog / digitized on-location ***  
  
Digitization_Location  
├── Name_Of_Event  
│   ├── YYYYMMDD  
│   │   └── SC0001_YYYYMMDD_LASTNAME  
│   │     ├── S8_01_03  
│   │     │   ├── MP4_2048x1152  
│   │     │   │   └── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.mp4  
│   │     │   └── ProRes_2048x1536  
│   │     │       └── SC0001_YYYYMMDD_LASTNAME1_S8_01_03.mov  
│   │     └── SC0001_YYYYMMDD_LASTNAME.cdir  
  
Naming:  
Collection directory:       SC0001_YYYYMMDD_LASTNAME                = CODE_YYYYMMDD_LASTNAME  
Package name:               S8_01_03                                = ANALOGFORMATCODE_OBJECT#_OBJECT#2  
Derivative subdirectory:    MP4_2048x1152                           = DIGITIZEDFORMATCODE_WIDTHxHEIGHT  
Filename (derivative?):     SC0001_YYYYMMDD_LASTNAME1_S8_01_03.mov  = COLLECTIONNAME_PACKAGENAME.extension  
  
  
*** VIDEO / analog / digitized on-location ***  
  
Digitization_Location  
├── Name_Of_Event  
│   ├── YYYYMMDD  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.framemd5  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mkv  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mkv.qctools.xml.gz  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01.mp4  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01_QC_output_graphs.jpeg  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_01_capture_options.log  
│   │   │   └── SC0001_YYYYMMDD_LASTNAME2_VHS_01_ffmpeg_decklink_input.log  
│   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.framemd5  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mkv  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mkv.qctools.xml.gz  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05.mp4  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05_QC_output_graphs.jpeg  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME2_VHS_05_capture_options.log  
│   │   │   └── SC0001_YYYYMMDD_LASTNAME2_VHS_05_ffmpeg_decklink_input.log  
  
Naming:  
Collection directory:       n/a  
Package name:               SC0001_YYYYMMDD_LASTNAME2_VHS_01        = CODE_YYYYMMDD_LASTNAME_ORIGINALFORMATCODE_OBJECT#  
Filename (master):          SC0001_YYYYMMDD_LASTNAME2_VHS_01.mkv    = PACKAGENAME.extension  
Derivative subdirectory:    n/a  
  
  
*** VIDEO / DV / transferred on-location ***  
  
Digitization_Location  
├── Name_Of_Event  
│   ├── SC0001_YYYYMMDD_LASTNAME  
│   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02_TITLEOFTAPE  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02_TITLEOFTAPE.mov  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02_TITLEOFTAPE_YYYYMMDD_checksums.md5  
│   │   │   └── derivative  
│   │   │       └── SC0001_YYYYMMDD_LASTNAME3_MiniDV_02_TITLEOFTAPE.mp4  
│   ├── SC0001_YYYYMMDD_LASTNAME_MiniDV_04_TITLEOFTAPE  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME_MiniDV_04_TITLEOFTAPE.mov  
│   │   │   ├── SC0001_YYYYMMDD_LASTNAME_MiniDV_04_TITLEOFTAPE_YYYYMMDD_checksums.md5  
│   │   │   └── derivative  
│   │   │       └── SC0001_YYYYMMDD_LASTNAME_MiniDV_04_TITLEOFTAPE.mp4  
  
Filenaming:  
Collection directory:       SC0001_YYYYMMDD_LASTNAME                             = CODE_YYYYMMDD_LASTNAME  
Package name:               SC0001_YYYYMMDD_LASTNAME3_MiniDV_02_TITLEOFTAPE      = COLLECTIONNAME_ORIGINALFORMATCODE_OBJECT#_OBJECTTITLE  
Master filename:            SC0001_YYYYMMDD_LASTNAME3_MiniDV_02_TITLEOFTAPE.mov  = COLLECTIONNAME_PACKAGENAME.extension  
Derivative subdirectory:    derivative                                           = derivative  
