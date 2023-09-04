# LPKF E44 PCB Mill

E44 Mill How to Guide:

Prep Steps:

- Ensure Stock Board is Suitable for use
    - Ensure that it’s not all gone basically
- Ensure you are using the correct PCB Board Material for your design(2 or 1 Layer)
- Ensure you Scotch Tape the Stock Board in place
- Ensure you Scotch tape the Copper Board in Place
- Mark corner of the Material Board on the Bottom Left for Lining up the Camera Later on

https://lh4.googleusercontent.com/mfeRCyPBT0mcizEsaJ6unnasWAke1s4P1JnYKM7IXTJKVwysGBAnuuV2ZW59kXaCQBqeoUmc6zc_gn1tbIYSt6ChCnsA1h3L9slEmVXZeU_PTztREn8NnMZ4kxJayTfqqlNVT3Hj7e7nmAoWjuY79Q

Sample Corner Marking

1. When starting the Program you’re prompted to choose the port of the Mill.
    1. Should be COM Port 5 usually
    2. If it does not connect, check if machine is on
    3. If not on, turn it on by pressing te
2. Load files onto the Project
    1. Use the Template depending on your design
        1. SingleSide Template
        2. DoubleSide Template
        3. Etc.
    2. Import > Choose Gerber Files for your design
        1. .Drl Files for Drilling
        2. Copper Layers F_Cu, B_Cu or whatever they're called
        3. Edge.Cuts Files, or whatever the outline file is called

https://lh3.googleusercontent.com/LnP8oflPlrERYFnXGQHJ_I-LMperRpX41nWIewDkxSXzQ8dYkKkEeBxjkcBAV8NEfqzQaYdqXsNNyiV9MvUSpvIunI_1WE0arWjKARBZalyS4PYXBbh81siXb0abCSSBS-myzUJmQblP95BAXaZfWw

Machining View Controls

1. For each file, assign the layers to the type of layer to be recognised by the Application in the import menu
    1. Front Copper layer, Back Copper Layer etc
2. Inspect the file that is imported to ensure that its not munted
    1. Check if drill holes are at the right places
    2. Check that traces do not short where not wanted
3. Press the Create Fiducial Button and Place three fiducials at by clicking near the Corners of the Job approximately, in the Machining view.
    1. Fiducials are needed so that the machine can detect the board when the material is flipped
    2. If you are doing a single sided board, this step is not needed
4. Generate ToolPaths, Button next to Fiducial Creation Button
    1. Technology Dialog will prompt
    2. Check if all the tools you need are there, you may need to change the material to FR4 if the tools aren’t there
    3. 35um Copper thickness (1oz)
    4. Probs better to Turn off MicroCutter if there are alot of 90 degree angles in job as this will incessantly try to make all your corners very sharp making the job longer
    5. Set Process to the 4/4 option
    6. For Contour Routing use any. Option 2 should be fine
    7. Start
    8. After toolpaths generated click close
5. Check through the Phases menu to see if all the steps make sense
    1. The phases denote the individual steps the machine will complete to finish the job and with which end piece
6. Assuming Toolpaths are good, Head to Machining View
7. Move the machine using Ctrl + Arrow Keys,
    1. Arrow keys without CTRL will change how much each step of movement is
    2. You can move z axis down as well but you shouldn't have to change it unless you are drilling something manually (Using the machine as a drill press)
8. Select Camera Head and move to the Marked corner of the board
9. Change to Drill Head
10. Start Board Production Wizard
    1. Go through the wizard for board
    2. There will be errors about the tool bits. Don’t worry about them
    3. Check the Material Settings match your board (VERY IMPORTANT, can cause failure and bit breakage)
    4. To calibrate geometry of the board
        1. P1: Bottom Left corner (This should match up with the corner mark you made)
        2. P2: Top Right Corner (should always match up with the P position, so you dont have to mark it)
    5. Click Continue
11. In Placement Menu, select the board and place where there is a good spot for it
    1. You can rotate and flip the board if required
12. Now it should start the job after prompting you to save the job with a name. Call it whatever
13. Tool Changing
    1. Select the Tool that the Computer prompts
    2. Put the Bit into the Applicator
    3. Push but into the Machine all the way in
    4. Unscrew the tension screw
    5. Screw the tension screw
    6. Ensure bit is securely in place
    7. Once secure and ready to go press enter to start the layer
    8. If there is a bit that is not in the machine it should be in the box
        1. Refer to the key on top of the box and grab it.
14. If nothing goes wrong you should be done.
15. If something does go wrong
    1. You are able to run through the wizard again and redo a phase if needed
    2. You can also run through the Phases manually through the Menu

To connect to machine

Machining > Connect

- Choose machine and it should connect

Mounting the Stock

- The PCB Stocks Long part goes on top and a smaller hole on bottom.

Bits:

- Colours denote what they are for
    - Yellow: Board Outline/Edge Cuts, Cuts the board out
    - Purple: End Mills, Mostly used for cutting big cuts of copper when needed
    - Green: Drilling
    - Orange: Isolation routing

LPKF CIrcuit 3 is the App

There are several views:

- Machining view: Shows camera and Physical aspect of the Job with current head and camera pose
- CAM View: Shows the Job File in Layers
- 3D View: View all the layers of the Job File for inspection in a 3D View