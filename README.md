## A heads up:

This is a fork of [Tim Patersons Fusion 360 Batch Post processor](https://github.com/TimPaterson/Fusion360-Batch-Post) that I have modded to work with my [DDCSV11-Plasma Post Processor](https://github.com/DeeEmm/DDCSV11-Plasma). It only required a couple of small changes and I have [raised a feature request](https://github.com/TimPaterson/Fusion360-Batch-Post/issues/22) to get these changes intgrated into the Main Repo.

I take no credit for Tims awesome work, so if you use this and appreciate it be sure to send some love Tims way.

All documentation and setup listed here is as-per Tims original code



## Post Process All: Fusion 360 CAM Batch Post Add-In

#### NEW! Fixes tool change restriction in Fusion 360 for Personal Use
Also restores rapid moves the free version of Fusion 360 limits to 
feed-rate moves. See section below.

### Introduction
This add-in for Fusion 360 will post process all CAM setups at once.
Each setup is put in a file with the name of the setup. You can
optionally use a special setup naming convention to put files in
subfolders. You can also put sequence numbers on the name to maintain
the order of operations.

The add-in creates a new command in the `Manufacture` workspace next to
the native Post Process command called Post Process All:

![Post Process All](https://raw.githubusercontent.com/TimPaterson/Fusion360-Batch-Post/master/resources/Command/32x32.png)

Clicking this command will bring up the following dialog:

![Dialog Image](https://raw.githubusercontent.com/TimPaterson/Fusion360-Batch-Post/master/ReadMeImages/DialogImage.PNG)

The first time you run `Post Process All`, the `Output Folder` and `Post Processor`
fields will be blank. You must set both of these fields before you can
click OK. Once `Post Process All` has been run, all settings will be saved in
the design. New designs will also be given default settings.
### Subfolders
You can put the output files in subfolders by using a colon (":") in
the name. The part of the name to the left of the colon is the folder
name; multiple colons result in nested subfolders.

Here is an example of a design with three components. The name of the
component appears first, then a colon and the name of the setup for
that component:

![Setup Image](https://raw.githubusercontent.com/TimPaterson/Fusion360-Batch-Post/master/ReadMeImages/SetupImage.PNG)

When you run the `Post Process All` command, it will create three
subfolders in the output folder, named "Cover", "Block", and "Insulator".
Within these subfolders will be five, three, and five G-code files
respectively with names like "first long edge", "second long edge", etc.
### Sequence Numbers
A sequence number can be added to the front of each file name. This
provides a reminder of the order of operations and will generally
keep the files sorted in that order. Sequence numbers start at 1 for
each folder.

In the above example, when adding sequence numbers the files in
the "Block" folder would have the names "1 first edge" and "2 second
edge".
### Fusion 360 for Personal Use
The free version of Fusion 360 now has some limitations on G-code
output: It will not support tool changes, and it slows down all
moves to feed rate. `Post Process All` can work around the tool change
limitation and can attempt to restore rapid moves.

In the `Personal Use` section of the `Post Process All` dialog, you will find
the option to break each setup into individual operations. This 
is required if you have the free version of Fusion 360 and there
is a tool change in any setup. `Post Process All` will combine the G-code
for each operation of a setup back into a single file for that
setup, hiding this limitation. Since there is no example of a
tool change in the G-code generated by Fusion 360, there is a
box to type in any additional G-codes needed when a tool change 
is made (such as turning off coolant, etc.).

In addition, you can also select the option to restore rapid (G0) 
moves. This analysis is experimental and should be reviewed 
before use (comments are included where the G-code is changed).
You are responsible for ensuring a tool crash does not occur.
### Installation
To install, start by putting the `PostProcessAll.py` and 
`PostProcessAll.manifest` files along with
the `resources` subfolder into a folder on your machine. This can
be a Git repository or just a copy of the files. (The `ReadMe.md`
file and `ReadMeImages` folder are not required.) Using Git is
recommended (newcomers see below) because it makes it easy to
come back and get updates.

In Fusion 360, go to the `Tools` tab in the `Design` workspace, or the
`Utilities` tab in the `Manufacture` workspace. Select the
`Add-Ins` command, which will bring up the `Scripts and Add-Ins` dialog.
Switch to the `Add-Ins` tab, then click the green `+` to add a new
add-in. You can now browse to the folder in which you placed the
`Post Process All` add-in files.

Once the folder is selected, you will be returned to the Add-Ins dialog
and find that PostProcessAll now appears in the list of `My Add-Ins`.
Select it in the list, ensure `Run on Startup` is checked, and then
click the `Run` button. It should like like this:

![AddIn Image](https://raw.githubusercontent.com/TimPaterson/Fusion360-Batch-Post/master/ReadMeImages/FusionAddIn.PNG)

To see the new command, go to the `Manufacture` workspace and select the
`Milling` or `Turning`tab. The command appears on the toolbar next to
the native Post Process command. It also appears in the Actions
drop-down menu.

If you give `Post Process All` a try, please help compile a list of
how it does with different post proecessors. Post new results
— success or failure — on the Issues page. That info will make
it into the table below.

**New to Git?** A great Git GUI is [SmartGit](https://www.syntevo.com/smartgit/),
which hobbyists can use for free. You just select `Clone...` from its
`Repository` menu, and paste in the address of this web page. That will
bring in a copy of `Post Process All` that's ready to use. To update, click
`Pull` and then `Pull` again.

### Issues
If `Post Process All` fails, run it again. There seem to be occasional timing 
issues that usually resolve themselves. It may help to increase the time
delay for posting operations in the `Advanced` section of the 
`Post Process All` dialog.

The concatenation and modification of G-code has been only been lightly
tested, and work is ongoing. Given that there are
very specific assumptions about what the G-code input will look like,
it would not be surprising to find problems come up. Feel free to report
them on the Issues page, and include the G-code files.

### Compatibility
Compatibility can be an issue if you have Fusion 360 for Personal Use and
therefore select the `Use individual operations` option. This requires
parsing the G-code, which is dependent on the post processor you use.

Here is a list of the post processors that have been tested with `Post Process All`.
The `Review OK?` column means a visual inspection of output, while
`Tested` means it has been run on the CNC machine. The remaining entries
are the suggested option settings for that post processor.
This table is based on user feedback. Please add an issue for
corrections or additions.

| Post Processor | Review OK? | Tested | Tool change |  Numeric Name | File Ext. |
|----           | :----: | :----: |----    | :----: |---- |
| centroid.cps  | Yes | Yes |                        | Yes | .nc  |
| eding.cps     | Yes | No  | N10 M9:G28             | No  | .cnc |
| gbrl.cps      | Yes | Yes | M9:G28 G91 Z0:G90      | No  | .nc  |
| linuxcnc.cps  | Yes | No  | M9 G30                 | No  | .ngc |
| mach3mill.cps | Yes | Yes |                        | No  | .tap |
| tinyg.cps     | Yes | Yes |                        | No  | .gcode |
| tormach.cps   | Yes | Yes | M9 G30                 | No  | .nc  |
