# osx_shell_batch_resizer_plus
Shell script for *Mac OSX 10.10+* that uses brew+imagemagick to batch resize and optimize a folder of images.

#Installation
#Requirements
- Currently only runs on Mac OSX, tested on 10.10+
- Homebrew (brew) http://brew.sh/. To install homebrew open terminal and enter: 
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- Homebrew Imagemagick, from CLI enter 
```
brew update && brew upgrade && brew install imagemagick
```
- XCode (https://itunes.apple.com/us/app/xcode/id497799835?mt=12)

##Option 1: Download to Images Directory and Execute Manually
The shell script is built to automatically "resize all the images in the same folder where you place it". So your first options would be to download the shell script standalone, copy/move it to the directory with your images, and run it from there. That might look something like:

1. click on the _gen_thumbs.sh file above, then click "raw" and then do a cmd+s and choose the location of your images (lets just assume thats going to be `~/Downloads` which is your user default downloads directory.
2. open up a terminal by typing cmd+space, then typing terminal and then hitting enter.
3. change to your downloads directory by typing `cd ~/Downloads`
4. run the script by typing `. _gen_thumbs.sh`

That will run the default options and create a subdirectory called "thumbs" in which any images in your downloads directory will have been resized and optimized for a max height of 32px.

##Options 2: Full install for quick access via any terminal window
This option requires some working knowledge of how to use the CLI iterface on a mac, and general terminal commands. thats said, this the the most efficient way to work with the batch resizer app.

Instructions on the full install are coming soon.

##Option 3: Use the GUI App
Currently Unavailable - for some reason the zip from Github breaks file permissions when extracted on a mac. If anyone has any ideas on how to deploy an osx ".app" through Github, please let us know.

#Flags or Options
Like any normal CLI command, you can pass options into the script so as to control how it works. We setup the -h flag to give you a printout of all the options you have to choose from (because we likely forget ourselves and its nice and convenient to have on hand when you in the CLI). Here is a preview of the -h flag output:
```
    -h          display this help and exit
    -e          set file_types, defaults to jpg|jpeg|png|gif
                ex -f jpg (searches for just sample1.jpg, sample2.jpg)
    -o          set output type, defaults to "", keeps original file type format
                ex -o png
    -v          verbose mode. Can be used multiple times for increased
                verbosity.
    -s          set max height of images, default is 32
                ex: -s 50 creates sample__50x50.png on a square image
```
