# Neural-Tile
Creating larger Neural-Style images through automated tiling.

---
The idea for the script comes from a combination of techniques discovered by [SwoosHkiD/bododge](https://github.com/bododge) and [ProGamerGov](https://github.com/ProGamerGov) which was posted on the [Neural-Style Wiki](https://github.com/jcjohnson/neural-style/wiki/Techniques-For-Increasing-Image-Quality-Without-Buying-a-Better-GPU).

[0000sir](https://github.com/0000sir) created the [original skeleton](https://github.com/0000sir/larger-neural-style) of the script.

# Requirements

A Neural Art project like [Neural-Style](https://github.com/jcjohnson/neural-style/)

And ImageMagick, install it with:

`sudo apt-get install imagemagick`

# Usage

The default version of the script is **3x3** tiles. 

Copy this script to neural-style directory and add your [Neural-Style](https://github.com/jcjohnson/neural-style/) or [other neural network based image project](https://github.com/jcjohnson/neural-style/wiki/Similar-to-Neural-Style) settings on [lines 109-124](https://github.com/ProGamerGov/Neural-Tile/blob/master/neural-tile.sh#L106-L121) for the initial run through of the script. Then add the same settings, or different ones, to [lines 139-156](https://github.com/ProGamerGov/Neural-Tile/blob/master/neural-tile.sh#L136-L153) for the tiles. Whether or not you are doing multires, or the normal process, make sure that `-content_image $1`, `-style_image $2`, and `-output_image $3` remain the same for everytime the neural art project is ran with your settings. If using multires, then `-output_image $3` only needs to remain for the last run through/step in the chain. 

Then run:

`./neural-tile.sh content_image style_image`

If you face a permission error, try using `chmod` to fix the issue: 

`chmod u+x ./neural-tile.sh`

If you have an already styled image, or had to stop the script before all the tiles were made, the script will automaticaly pickup where it left off at:

* To use a previously styled image, place the image into in your `Neural-Style/output/<Styled_Image>` directory and change your specified "content_image" parameter to the name of your previously styled image. You also need to place a copy of you previously styled image into the `Neural-Style` directory. 

* To continue with already processed tiles, place the original tiles into the `Neural-Style/output/<Image_Name>/` directory. Place the previvously styled tiles into the `Neural-Style/output/<Image_Name>/tiles/` directory. Note that it if you are using tiles that you ran through Neural-Style in a previous session, make sure you are using the Neural-Style parameters for both sessions. 

Due to the way the script creates and procceses the tiles, it should be possible to safely use the `-save_iter` command during the processing of the tiles. This can be done to save different iteration sets of tiles, that can be renamed by the user, so that the script will put them together.

If you are using multires or the normal single step process and want the tiled image to look similar to your original styled image: 

* The style scale value that results in zero style change, is the square root of the number of tiles the image was divided into. This equation will need to be adjusted if you use a style scale value other than 1 for the initial image styling. 

* Using `-original_colors 1` may also help prevent style drift, at least in terms of color.

# How It Works

## 1. Generate The First Output Image:
It is recommended that you change the Neural-Style parameters to your linking.

## 2. Split The Initial Output Image Into Tiles:
ImageMagick is used to divide your first Neural-Style output image into a series of overlapping cropped images.

## 3. Run The Tiles Through Neural-Style To Increase Their Quality And Size:
The same Neural-Style parameters are then used to "U-Pres" the overlapping crop pieces, resulting in a higher resolution output. 

## 4. Feather The Tiles:

Feathering is used to blend the overlapping cropped tiles that have gone through Neural-Style in order to increase their resolution. Feathering values can be manipulated in order to find the best values for blending the tiles together. 

## 5. Merge The Feathered And Non-feathered Tiles Into Separate Outputs:

The feathered tiles are put back together into an image that is larger than your original Neural-Style output image.

## 6. Layer The Feathered Image Above the Non-feathered Image:

This is done to disguise the feathering that is done to blend the tiles together.
