The issue encountered is that Xcode is downloadable from Apple's Developer Portal (https://developer.apple.com/download/all/?q=xcode); however, it is an extremely large xip file archive, approximately 12GB in size. The archive  request 32GB of free space in order to extract the app even though the final file size is smaller than this.

Editing the Metadata file contained within the Xip archive to request less space results in a failed signature check (as expected). Thus, the easiest thing to do is to move the file off the main disk and extract it, then move the extracted files back. One way to do this - provided there is sufficient RAM - is to copy the installer to sufficiently large RAM disk and then extract it to /Applications. Note that the xip utility extracts the file to the current working directory. 

We can do this with the following code snippet:

    hdiutil attach -nobrowse -nomount ram://25165824
    diskutil erasevolume HFS+ 'RAM Disk' /dev/diskX
    mv ~/Downloads/Xcode_VERSION.xip /Volumes/RAM\ Disk/
    cd /Applications
    xip --expand /Volumes/RAM\ Disk/Xcode_VERSION.xip
    hdiutil eject /dev/diskX
