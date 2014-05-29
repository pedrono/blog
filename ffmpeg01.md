Merge H.264 videos with FFMPEG
================================

Warning: Still under construction

If it is required to merge two or more H.264 (MP4, MOV) video clips without re-encoding, it is very easy to use (FFMEG)[http://ffmpeg.org/] command line tool.

The procedure is quite easy:
   1. FFMPEG need to be installed
   2. Copy the video file in one directory
   3. Create a text file (e.g. list.txt) that contains file names of the H.264 file, see below.
   ```bash
   file 'file1.mp4'
   file 'file2.mp4'
   file 'file3.mp4'
   ```
       
   4. The execute following command
      ```bash
         $ ffmpeg -f concat -i list.txt -c copy output.mp4
      ```


See the full (documentation)[http://ffmpeg.org/ffmpeg-formats.html#concat-1] of this command for reference.