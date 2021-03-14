# Introduction
This branch of JPEXS adds two new command-line commands: -replacebatch and -replacemethod. 
They operate the same way -replace operates in vanilla JPEXS. 
It's designed to be used with batch (.bat) files to make porting mods easier.
Note that this is designed for advanced users, and off the top of my head, I only know one person who *might* use this.

In order to use this branch of JPEXS, you must first unzip the "JPEXS SSF2Patcher.zip" file to a location.
Note: The contents of "JPEXS SSF2Patcher.zip" are also the contents of the dist folder in this repository. 
If you choose to modify this build of JPEXS in a Java IDE of your choice, building it should send it to the dist folder.


# Documentation

Below is the documentation for the new methods added, as well as the vanilla -export, -format, and -replace methods, since you'll probably be using them too.

```
 4) -export <itemtypes> <outdirectory> <infile_or_directory>
  ...export <infile_or_directory> sources to <outdirectory>.
  Exports all files from <infile_or_directory> when it is a folder.
     Values for <itemtypes> parameter:
        script - Scripts (Default format: ActionScript source)
        image - Images (Default format: PNG/JPEG)
        shape - Shapes (Default format: SVG)
   morphshape - MorphShapes (Default format: SVG)
        movie - Movies (Default format: FLV without sound)
        font - Fonts (Default format: TTF)
        frame - Frames (Default format: PNG)
        sprite - Sprites (Default format: PNG)
        button - Buttons (Default format: PNG)
        sound - Sounds (Default format: MP3/WAV/FLV only sound)
        binaryData - Binary data (Default format:  Raw data)
        text - Texts (Default format: Plain text)
        all - Every resource (but not FLA and XFL)
        fla - Everything to FLA compressed format
        xfl - Everything to uncompressed FLA format (XFL)
   You can export multiple types of items by using colon ","
      DO NOT PUT space between comma (,) and next value.

 5) -format <formats>
  ...sets output formats for export
    Values for <formats> parameter:
         script:as - ActionScript source
         script:pcode - ActionScript P-code
         script:pcodehex - ActionScript P-code with hex
         script:hex - ActionScript Hex only
         shape:svg - SVG format for Shapes
         shape:png - PNG format for Shapes
         shape:canvas - HTML5 Canvas format for Shapes
         shape:bmp - BMP format for Shapes
         morphshape:svg - SVG format for MorphShapes
         morphshape:canvas - HTML5 Canvas  format for MorphShapes
         frame:png - PNG format for Frames
         frame:gif - GIF format for Frames
         frame:avi - AVI format for Frames
         frame:svg - SVG format for Frames
         frame:canvas - HTML5 Canvas format for Frames
         frame:pdf - PDF format for Frames
         frame:bmp - BMP format for Frames
         sprite:png - PNG format for Sprites
         sprite:gif - GIF format for Sprites
         sprite:avi - AVI format for Sprites
         sprite:svg - SVG format for Sprites
         sprite:canvas - HTML5 Canvas format for Sprites
         sprite:pdf - PDF format for Sprites
         sprite:bmp - BMP format for Sprites
         button:png - PNG format for Buttons
         button:svg - SVG format for Buttons
         button:bmp - BMP format for Buttons
         image:png_gif_jpeg - PNG/GIF/JPEG format for Images
         image:png - PNG format for Images
         image:jpeg - JPEG format for Images
         image:bmp - BMP format for Images
         text:plain - Plain text format for Texts
         text:formatted - Formatted text format for Texts
         text:svg - SVG format for Texts
         sound:mp3_wav_flv - MP3/WAV/FLV format for Sounds
         sound:mp3_wav - MP3/WAV format for Sounds
         sound:wav - WAV format for Sounds
         sound:flv - FLV format for Sounds
         font:ttf - TTF format for Fonts
         font:woff - WOFF format for Fonts
         fla:<flaversion> or xfl:<flaversion> - Specify FLA format version
            - values for <flaversion>: cs5,cs5.5,cs6,cc
      You can set multiple formats at once using comma (,)
      DO NOT PUT space between comma (,) and next value.
      The prefix with colon (:) is neccessary.

 28) -replace <infile> <outfile> (<characterId1>|<scriptName1>) <importDataFile1> [nofill] ([<format1>][<methodBodyIndex1>]) [(<characterId2>|<scriptName2>) <importDataFile2> [nofill] ([<format2>][<methodBodyIndex2>])]...
 ...replaces the data of the specified BinaryData, Image, Shape, Text, DefineSound tag or Script
 ...nofill parameter can be specified only for shape replace
 ...<format> parameter can be specified for Image and Shape tags
 ...valid formats: lossless, lossless2, jpeg2, jpeg3, jpeg4
 ...<methodBodyIndexN> parameter should be specified if and only if the imported entity is an AS3 P-Code

29) -replacebatch <infile> <outfile> <importDataFolder> [nofill] [format1] 
 ...replaces the data of the specified BinaryData, Image, Shape, Text, and DefineSound tags 
 ...<importDataFolder> paramemeter must point to a folder that contains folders for each type of tag you want to replace 
 ...ex. dat15/exports contains the folders images, sounds, and binaryData, so it will replace images, sounds, and binaryData 
 ...nofill parameter can be specified only for shape replace 
 ...<format> parameter can be specified for Image and Shape tags 
 ...valid formats: lossless, lossless2, jpeg2, jpeg3, jpeg4

 30) -replacemethod <infile> <outfile> <scriptName> <importDataFile1> methodName 
 ...replaces the p-code of the specified method
```



# Tutorial

First, I'd like to show you how to get this documentation if you choose to see what other methods are at your disposal. 
Simply create a new batch file in the same directory as jpexs.jar, and type in the following:

```
java -jar jpexs.jar help
```

After that, run the .bat file, and it should show you all of the JPEXS commands! 
The general gist of jpexs commands is that you use them by first typing java -jar (which runs a jar using Java), jpexs.jar (the path to the jar Java should run), and then the command you want.



## -replace
Now, I'd like to cover the base -replace method. First, let's talk about what it can do:

+ Replace Binary Data 
+ Replace Images 
+ Replace Shapes 
+ Replace Text 
+ Replace Sounds 
+ Replace Scripts* 
*note that replacing scripts using the -replace method is invariably a bad idea, for reasons discussed below

The "replace binary data" aspect is typically useful for replacing the encrypted mappings/resource manager with a decrypted version.

The "replace images" aspect is useful for creating resprites of characters with no hitbox changes.

The "replace shapes" aspect is useful for creating new effects for characters, but do note that you should not replace character sprites by replacing a shape corresponding to it. 
This is because it'll come out blurry and weird.

The "replace text" aspect has no practical application in SSF2 off the top of my head, but if you can find one, I tip my hat to you!

The "replace sounds" aspect is useful for doing voice mods. 
If you ever wanna replace Pit's voice with his Brawl voice, this is the way to do it!

The "replace scripts" aspect is extremely flawed.
Using the -format method before using it allows the user to clarify whether they're replacing a .as script or a p-code method. 
However, if the user chooses to replace a .as script, they risk JPEXS exporting a version of it that doesn't work. 
On the other hand, if the user chooses to replace the p-code of a method, they have to know the method's body ID. 
Problem is, the only way of knowing a method's body ID is if you open JPEXS, go to Settings, go to the Scripts tab, and check the "Show method body id" checkbox, then look for it in the script pane. 
The method's body ID changes every time the SWF is exported from Animate, however, so it won't be consistent among different versions of SSF2. 
It's very inefficient and doesn't work if you're trying to be clever with version-proof code. 
The way around this is to use the -replacemethod method, which allows you to specify the method name instead of the method body id. 

When using the -replace method, you have to specify a few things. 
First, the path to the input .swf (or .ssf) file. This is the base file that you're modding. 
Then, the path to the the output .swf file. This is the file you'll be saving your changes to. 
Then, you put the character ID, the number next to the thing in JPEXS. 
For example: Tingle's Image's character ID in DAT8 (misc.ssf) in v0.9b is 131. 
Next, you put the path to the file you're replacing the target thing with. 
Then, if you're replacing a shape, you have to type "nofill" if you want the background to be transparent. 
Lastly, if you're replacing an image or a shape, you have to type the format. 
Valid formats are listed in the replace documentation. 

All in all, replacing Tingle's sprite in 9b with the contents of an image called "image.png" would look something like this:

java -jar jpexs.jar -replace DAT8.ssf DAT8new.ssf 131 image.png nofill lossless2

Meanwhile, replacing Kirby's victory theme in 9b with the contents of a .wav file called "song.wav" would look something like this:

java -jar jpexs.jar -replace DAT15.ssf DAT15new.ssf 31 song.wav



## -replacebatch

The -replacebatch method is just like the replace method, only instead of specifying a file you wanna replace stuff with, you specify the folder containing folders containing files you wanna replace stuff with. 
For example: Say you exported all of 9b Kirby's images to a folder called "exports" so that the path to the black mage hat was "exports/images/99.png" 
You could run the following to replace all of Kirby's images with the exported images: 

```
java -jar jpexs.jar -replacebatch DAT15.ssf DAT15new.ssf exports nofill lossless2
```

Additionally, if exports contained appropriately numbered sounds in the a folder labeled "sounds" in exports, it would try to replace the associated sounds in Kirby's file.



## -replacemethod

The -replacemethod method replaces the contents of a method's p-code with the stuff you provide in an external file. 
In order to replace it, you must specify the class it's in using the full classpath. 
For example: Say you wanted to replace MainTimeline.getChar1 in 9b Kirby's file with the contents of a file called "newGetChar1.pcode" 
You could run the following to do so: 

```
java -jar jpexs.jar -format script:pcode -replacemethod DAT15.ssf DAT15new.ssf kirby_fla.MainTimeline newGetChar1.pcode getChar1
```

This method is notably used in SSF2IDK Generator to export the ResourceManager.



# SSF2Patcher Challenges

## SSF2PATCHER CHALLENGE 1 
Get JPEXS to output a list of commands

## SSF2PATCHER CHALLENGE 2 
Write a .bat script to replace 9b Meta Knight's victory music with a song of your choice, as well as making his fsmash deal 999%.

## SSF2PATCHER CHALLENGE 3 
Make a .bat script that replaces Kirby's sprites with Bandana Dee's sprites.