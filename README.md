# A Quick Guide To ImageMagick

ImageMagick is a commandline application for editing images.\
I find it good for cropping and scaling images in bulk, creating animated gifs and assembling sprite sheets.\
You can even stick the commands in a script to run. My examples will output all results to the Home directory.\
I'll also mention some tips and pitfalls I've discovered.  
### Resources
First some resources.\
[The ImageMagick site](http://www.imagemagick.org)\
The [good intro](http://www.imagemagick.org/script/command-line-processing.php)\
The [bad official docs](http://www.imagemagick.org/Usage/crop/#crop_tile). These are detailed but also obsolete, misspelled, verbose and purple.


---

You can generate some builtin images to practice on
>`magick rose: rose.gif`

![a](images/rose.gif)  

---

convert png to gif to remove whitespace  
> `convert dude.png +repage dude.gif`  

---

Scale
Avoid blur with -scale and a whole fraction %  
Double blow up:  
$ `convert in.gif  -scale 200%  out.gif`  
Half shrink:  
$ `convert in.gif  -scale 50%  out.gif`  


---

To crop a sprite sheet  
![a](/images/bobs.png)  
do this
> `convert bobs.gif -crop 32x32 +repage d%03d.gif`

Produces  
![a](images/c028.png)

---

There's also a two step way of doing this. First crop the spritesheet into horizontal strips  
> `convert bobs.gif -crop 0x32 +repage b%02d.gif`  
![a](images/b00.png)  
![a](images/b01.png)  

Then swap the dimensions to vertically crop the strips.  
> `convert b00.gif -crop 32x0 +repage f%02d.gif`  
![a](images/f01.gif) ![a](images/f02.gif)  

Crop 10px from the top  
$ `convert in.gif -crop +0+10 +repage ftop.gif`  
![a](images/ftop.gif)  
Crop 10px from the left  
$ `convert in.gif -crop +10+0 +repage fleft.gif`  
![a](images/fleft.gif)  
Crop 10px from the right  
$ `convert in.gif -crop -10+0 +repage fright.gif`  
![a](images/fright.gif)  
Crop 10px from the bottom  
$ `convert in.gif -crop +0-10 +repage fbtm.gif`  
![a](images/fbtm.gif)  

---

</br>


In this row I want to crop just the sprites
![a](images/dude.png)  

Since I know every sprites is 32px wide

> `convert ~/dude-cropped.png -crop 32x0 +repage ~/d%02d.png`
![a](images/d04.png)

---
To put these 3 sprites in a row
![a](images/d019.gif) ![a](images/d120.gif) ![a](images/d121.gif)  
I can target them individually
>`magick d009.gif d010.gif d011.gif +append tv1.gif`

![a](images/tv1.gif)

Or I can grab a range based on filename!

>`magick d%03d.gif[9-11] +append tv1.gif`

To stack them up
>`magick d1.gif d2.gif d3.gif -append tv1.gif`

![a](images/tv2.gif)

---


For offset, the origin (0,0) is upper-left corner. (w)x(h)(+right)(+down)  
The following negatively colours a (tall) area:  
First in topL corner 10px to the right, 20px down. turned a red rose blue
>`magick rose: -region '100x200+10+20' -negate rNeg1.gif`

![a](images/rNeg1.gif)

This spills off the left edge
>`magick rose: -region '100x200-10+20' -negate rNeg2.gif`

![a](images/rNeg2.gif)

This resets the origin (0,0) to the centre so offset is below+L of it.
>`magick rose: -gravity center -region '100x200-10+20' -negate rNeg3.gif`

![a](images/rNeg3.gif)

dead centre
>`magick rose: -gravity center -region '100x200' -negate rNeg4.gif`

![a](images/rNeg4.gif)

---

source  
Make a gif

> `magick d00%d.gif[0-7] bl.gif`

</br>

---

Tips:  
Convert a sprite to gif before cropping to see the whitespace that pngs hide.  
Name output to %03d if it will generate 100s of sprites.
Output filenames start 00, 01, etc. If you want output to start on 01 use "null:" as the first input  
> `convert null: b00.gif -crop 32x0 +repage f%02d.gif`

This produces f01.gif, f02.gif



