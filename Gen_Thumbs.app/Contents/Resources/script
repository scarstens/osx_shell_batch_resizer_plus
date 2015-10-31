#!/bin/bash

#Making Shell Script Platypus App Compatible
#http://sveinbjorn.org/files/manpages/PlatypusDocumentation.html#11
app_used=0;
original_dir="$(pwd)";
if [[  $(pwd) == *.app* ]] ; then
    echo ";App Detected by Script;";
    echo "*** *** ***";
    echo "DRAG FOLDER TO APP TO BUILD THUMBNAILS"
    echo "*** *** ***";
    echo "App Dir: "$(pwd);
    app_dir=$(pwd);
    cd "../../..";
    original_dir="$(pwd)";
    echo "Fixed Root Dir: "$(pwd);
    app_used=1;
fi

###http://mywiki.wooledge.org/BashFAQ/035
# Usage info
show_help() {
cat << EOF
Usage: ${0##*/} [-hv] [-f OUTFILE] [FILE]...
Do stuff with FILE and write the result to standard output. With no FILE
or when FILE is -, read standard input.

    -h          display this help and exit
    -e          set file_types, defaults to jpg|jpeg|png|gif
                ex -f jpg (searches for just sample1.jpg, sample2.jpg)
    -o          set output type, defaults to "", keeps original file type format
                ex -o png
    -v          verbose mode. Can be used multiple times for increased
                verbosity.
    -s          set max height of images, default is 32 
                ex: -s 50 creates sample__50x50.png on a square image
EOF
}

function gen_thumbs() {
    # Initialize our own variables:
    verbose=0
    output_height_max=32
    file_extensions="jpg|jpeg|gif|png"
    OPTIND=1 # Reset is necessary if getopts was used previously in the script.  It is a good idea to make this local in a function.
    while getopts "e:o:s:hv" opt; do
        case "$opt" in
            h)
                show_help
                return 0
                ;;
            e)  file_extensions=$OPTARG
                echo 'set file extension: '$file_extensions;
                ;;
            o)  output_extension=$OPTARG
                echo 'set static output filetype: '$output_extension;
                ;;
            v)  verbose=$((verbose+1))
                ;;
            s)  output_height_max=$OPTARG
                echo 'set max height: '$output_height_max;
                ;;
            '?')
                show_help >&2
                return 1
                ;;
        esac
    done
    shift "$((OPTIND-1))" # Shift off the options and optional --.

    ### USE TO DEBUG GETOPS
    if [ "$verbose" == 1 ] ; then 
        echo 'GETOPS DEBUG';
        printf '  verbose=<%d>\n  output_height_max=<%s>\n' "$verbose" "$output_height_max";
        printf 'Leftovers:\n<%s>\n' "$@";
    fi

    #IF APP USED BUT NO FOLDER DRUG IN, RETURN
    if [[ ("$app_used" == 1) && ("$@" == "") ]] ; then
        echo "*** Please drop folder to be processed ***";
        return 1;
    #IF APP IS USED AND FOLDER IS DROPPED, BRING UP UIX QUESTIONS
    elif [[ ("$app_used" == 1) && ("$@" != "") ]] ; then
        echo "*** COCOA *** *** ***"
        CD="$app_dir/CocoaDialog.app/Contents/MacOS/CocoaDialog";
        echo "Cocoa found, moving to Cocoa Dir for dialogs: $(pwd)";
        # GET EXTENSIONS
        rv="jpeg|jpg|gif|png";
        rv=`$CD standard-inputbox --title "Step 1/2:" --no-newline --informative-text "Search for which file extensions? (empty = jpeg|jpg|gif|png)"`;
        apps_dash_e=${rv:2};
        if [[ ("$apps_dash_e" == "") ]] ; then
            apps_dash_e="jpeg|jpg|gif|png";
        fi
        echo "apps_dash_e: $apps_dash_e";

        #GET DESIRED SIZE
        rv="";
        rv=`$CD standard-inputbox --title "Step 2/2:" --no-newline --informative-text "Max height? (empty = 32)"`;
        apps_dash_s=${rv:2};
        if [[ ("$apps_dash_s" == "") ]] ; then
            apps_dash_s="32";
        fi
        echo "apps_dash_s: $apps_dash_s";

        rv="";
        rv=`$CD msgbox --no-newline \
        --text "Verbose debugging in the output?" \
        --informative-text \
        --button1 "Yes" --button2 "Cancel"`
        apps_dash_v=$rv;
        echo "apps_dash_v: $apps_dash_v";

        file_extensions=$apps_dash_e;
        #output_extension=$apps_dash_o;
        verbose=$apps_dash_v;
        output_height_max=$apps_dash_s;
        CD="$original_dir";
        echo "Cocoa Dialog Finished returning to original dir: $original_dir";
    fi

    if [[ ("$@" != "") && (-d "$@") ]] ; then 
        echo "  directory passed to app, changing processing directory";
        cd "$@"; 
    fi

    ####### CHECKING DEPENDENCIES ########
    #make sure brew is installed
    var_wb="`which brew | grep /`";
    echo $var_wb;
    if [ "$var_wb" != "" ] 
    then
        echo 'Found Brew, checking for imagemagick...'
    else
        echo $var_wb;
        cd $original_dir;
        return 1;
    fi

    #make sure imagemagick is installed by brew
    var_im="`brew ls | grep imagemagick`";
    if [ "$wb" != "brew not found" ] 
    then
        echo 'Found Imagemagick, checking for thumbs subfolder...'
    else
        echo $wb;
        cd $original_dir;
        return 1;
    fi

    #make sure thumbs folder exists for placement of images
    #images can be moved after creation but subfolder ensures convert command doesn't overwrite original sources
    if [ ! -d "thumbs" ]; then 
        mkdir thumbs;
    fi

    ##################### RUN APPLIANCE #########################
    SAVEIFS=$IFS
    IFS=$(echo -en "\n\b")
    eval 'the_files=$( ls -1 | grep -E ".('$file_extensions')" )' ; 
    #cd $original_dir; return 0;
    echo "the files: "$the_files;
    for f in $the_files;
    do
        filename="${f%.*}"
        #default to same filetype as source, otherwise convert to defined static file type
        if [[ ($output_extension != "") ]] ; then 
            extension=$output_extension;
        else
            extension="${f##*.}";
        fi

        convert $f -resize "x"$output_height_max -strip -quality 80 -set filename:area '%wx%h' "thumbs/"$filename'__%[filename:area].'$extension;
        if [ "$verbose" == 1 ] ; then 
            echo $f" thumb created"; 
        fi
    done

    IFS=$SAVEIFS;
    #return directory to oringal working directory
    cd $original_dir;
}

gen_thumbs "$@"

# End of file