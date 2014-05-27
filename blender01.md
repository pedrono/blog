Blender command line rendering
================================

Warning: Still under construction

Blender offers the possibility to render images or animations from the command line without UI.
This saves memory and reduces the render time a bit.
However, I think the most useful application is, if it is required to render multiple files as a batch process.

##Syntax

```bash
 blender [-b <dir><file> [-o <dir><file>][-F <format>][-f <frame>][-s <frame> -e <frame> -a]]
``` 

###Render options

```bash
 -b or --background <file>
     Load <file> in background (often used for UI-less rendering)
 
 -a or --render-anim 
     Render frames from start to end (inclusive)
 
 -f or --render-frame <frame>
     Render frame <frame> and save it.
     +<frame> start frame relative, -<frame> end frame relative.
 
 -s or --frame-start <frame>
     Set start to frame <frame> (use before the -a argument)
 
 -e or --frame-end <frame>
     Set end to frame <frame> (use before the -a argument)
 
 -o or --render-output <path>
     Set the render path and file name.
     Use // at the start of the path to
         render relative to the blend file.
     The # characters are replaced by the frame number, and used to define zero padding.
         ani_##_test.png becomes ani_01_test.png
         test-######.png becomes test-000001.png
         When the filename does not contain #, The suffix #### is added to the filename
     The frame number will be added at the end of the filename.
         eg: blender -b foobar.blend -o //render_ -F PNG -x 1 -a
         //render_ becomes //render_####, writing frames as //render_0001.png//
	 
 -x or --use-extension <bool>
     Set option to add the file extension to the end of the file
```

Note:
- The option -b or --background should be always set. We don't need the UI!
- The last argument has to be either -f (for still image) or -a (for an animation).

##Examples
It is easiest to set all render parameters in Blender (with UI), then you don't need to think about the format or the render engine.
There are actually much more options available than mentioned here. However, they are for more special usages that are not covered here.
Check the [Blender documentation](http://wiki.blender.org/index.php/Doc:2.6/Manual/Render/Command_Line) for further information.

We are keepin' it fairly simple, then everything is quite straightforward.

###Still images (single frames)

    blender -b example/example.blend  –o //rendered_files -x 1 -f 1
	
That means Blender in command line mode (-b) shall render the file example.blend, save the output in rendered_files (-o), the file extension should be added (-x), and frame No. 1 should be rendered (-f).

###Animations

    blender -b example/example.blend  -s 20 -e 100 -a

That means Blender in command line mode (-b) shall render the file example.blend, start the rendering at frame 20 (-s) until frame 100 (-e) as an animation (-a). All other render settings like format, render engine, etc are done in the way like defined in the .blend file.

##Batch rendering
As I said in the introduction this is in my opinion the most useful application of the command line rendering of blender, at least for the general use of Blender.
There are several ways to implement it.

###Directly from the command line
1. Windows (DOS)
   In the Windows command prompt you can combine commands by '&&'.
   See this example:
   
   ```bash
	blender -b example\example1.blend  -s 20 -e 100 -a && blender -b example\example2.blend  -s 20 -e 100 -a
   ```
    
   If you are rendering overnight or over the weekend, it is rekommendable to include a command to shutdown the PC automatically.
   The command would then look like this:
   
    ```bash
    blender -b example\example1.blend  -s 20 -e 100 -a && blender -b example\example2.blend  -s 20 -e 100 -a && shutdown /s /t 0
    ```

2. Unix (shell)
   The Blender commands are identical on Unix (Linux,MAC OS X, etc) and Windows systems. However, there some pecularities for each OS, e.g. Windows uses the \ character as the path separator and UNIX the / character.
   The only important difference is the shutdown command. The syntax is different and you need superuser rights to execute that command.
   In short it looks like this:

    ```bash
    blender -b example/example1.blend  -s 20 -e 100 -a && blender -b example/example2.blend  -s 20 -e 100 -a && sudo shutdown -h now

###Batch file or shell script
1. Windows (DOS)
Simply create a new file in you favorite text editor and copy and paste each command line seperately in a new line. 
Somthing like this:

   ```bash
   blender -b example\example1.blend  -s 20 -e 100 -a 
   blender -b example\example2.blend  -s 20 -e 100 -a
   shutdown /s /t 0
   ```
   Note: Omit '&&'.

   Then save the file and name the file extention .bat, and execute file. 
   
2. Unix (bash)
Bearing in mind the small differences between UNIX and DOS it is practically the same.
Here an example.

   ```bash
   #!/bin/bash
   blender -b example/example1.blend  -s 20 -e 100 -a 
   blender -b example/example2.blend  -s 20 -e 100 -a
   shutdown -h now
   ```
   Note: Omit '&&'.

   Then save the file and name the file extention .sh, and execute file.
   
   Note: In order to make the shutdown work, you have to execute the script with super user rights. 

