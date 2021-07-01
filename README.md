# RemarkableLamyEraser
Standalone tool that turns the button on the Lamy Pen into an eraser on the reMarkable.

Also confirmed to work with these other styli:
 * Samsung S6 S Pen

The tool will definitely break when the reMarkable updates. When that happens, just reinstall!
# Install Instructions
SSH into your reMarkable and make a directory to store our files:
```Shell
cd
mkdir RemarkableLamyEraser
```
Download the binary from the release page, and the .service file from the project page:
```Shell
cd ~/RemarkableLamyEraser
wget https://github.com/isaacwisdom/RemarkableLamyEraser/releases/latest/download/RemarkableLamyEraser
wget https://github.com/isaacwisdom/RemarkableLamyEraser/raw/main/RemarkableLamyEraser/LamyEraser.service
```
Make the binary exectuable, copy the .service file to systemd file, enable, and start it. This means the tool will automatically start on boot:
```Shell
chmod +x RemarkableLamyEraser
cp LamyEraser.service /lib/systemd/system/
systemctl daemon-reload
systemctl enable LamyEraser.service
systemctl start LamyEraser.service
```

# Uninstall Instrucions
```Shell
systemctl stop LamyEraser.service
systemctl disable LamyEraser.service
rm -rf ~/RemarkableLamyEraser
rm /lib/systemd/system/LamyEraser.service
systemctl daemon-reload
systemctl reset-failed
```


# Usage 
When you press the button on the Lamy Pen, an input event with code BTN_TOOL_RUBBER is sent into dev/input/event1. Essentially, this tricks the reMarkable into
thinking you are using the eraser side of the Marker Plus.
Press and hold to erase, release to use as a normal pen.

Toggle Mode:
I also created a mode that toggles between eraser mode and pen mode on the button press. However, due to the way the EMR technology works, the reMarkable can only catch when you've pressed the button on the Lamy pen when it is close to the screen. Personally, I found this made it difficult to use this mode, but if you'd like to switch modes, add --toggle argument in line 6 of the LamyEraser.service file (using nano or whatever). It should look like this:
```
ExecStart=/home/root/RemarkableLamyEraser/RemarkableLamyEraser --toggle
```
(Remove the argument to return to press and hold mode.)
Then run these commands
``` Shell
cd ~/RemarkableLamyEraser
cp LamyEraser.service /lib/systemd/system/
systemctl stop LamyEraser.service
systemctl daemon-reload
systemctl start LamyEraser.service
```

# TODO:
- [ ] RM1 support (testers needed)
- [ ] Nice install script
- [ ] toltec package
- [ ] config file (as opposed to current command line argument system)
- [ ] flexible triggers (such as "click", "press and hold", "double click", "double click and hold", etc.)
- [ ] freely assignable actions (as listed below, able to assign to any trigger above) *(these last two will require
      some significant code restructuring)*

# Testing Branch
The testing branch is used for code that builds towards the items on the TODO list above, but isn't yet in a format that is
ready to be committed to the main branch. Essentially, any commits to the main branch should be stable and relatively clean,
while the testing branch code will be "stable", but messy.
Currently, the testing branch is being used to test:
 - [ ] Double click actions

The actions currently being implemented are:
- [X] undo *(working only for portrait view with right hand orientation)*
- [ ] redo
- [ ] erase (needed for RM1 support, since it can't use the Marker Plus style rubber events)
- [ ] erase selection
- [ ] select
