# Study Design with EPRIME

## Contents
  1. [Using EPrime](#using-eprime)
  2. [The most important thing to know](#the-most-important-thing)
  3. [Variables vs. Attributes](#variables-vs-attributes)
      1. [Variables](#variables)
      2. [Attributes](#attributes)
      3. [Attribute Scoping](#attribute-scoping)
      4. [An exception to the attribute/variable distinction](#exception)
  4. [Displaying Feedback, Dynamic Timing, Formatting](#displaying)
  5. [Move, Copy, Shortcut](#move-copy-shortcut)
  6. [Some General Tips](#some-general-tips)
  7. [EPrime Timing Issues](#eprime-timing-issues)
  8. [Implementing Drift Adjust in E-Prime](#implementing)
  9. [How to make an Eprime task ready for the scanner](#ready-for-scanner)
  
<a name='using-eprime'></a>
## Using EPrime
This manual assumes you have played with EPrime for at least a few minutes to familiarize yourself with the basic structure of an EPrime program. If you are computer savvy you can probably pick up the basics of EPrime on your own. Nevertheless, to expedite the learning process it is recommended you find someone in the lab who is familiar with EPrime to teach you. This could be the lab manager, an RA, or a grad student.

Here are some things you should understand before continuing through the guide:
  - Inline object: those little paper-looking icons in the program structure in the left pane.
  - List: the little Excel spreadsheet-looking icons in the left pane.
  - Procedure object: the little arrow-and-shapes objects in the left pane, usually under a List.
  - Object (general): any item from the very left pane that can be put into the tree. Object means other things in programming terminology, but this is the only way it will be used in this document. EPrime is not a great program.
  - Note that a single quote ( ' ) anywhere in a line makes anything after it a comment.
  - Note also that EPrime is completely case insensitive (you can type entirely in UPPERCASE, or entirely in lowercase, or in a MiX, and it will read it all the same way).
  
<a name='the-most-important-thing'></a>
## The most important thing to know
When you've completed creating your task: break it. You will be very sad if you don't know to do this and end up with your TRs unlocked from your slide duration.

Don't sit down and test it like the sane, attentive, personally invested, coordinated individual you believe all your subjects to be. __Test it like a circus clown on a bender.__ Press the response key prematurely. Press the response key prematurely and continuously. Press all the response keys simultaneously, prematurely, and continuously while desecrating the spirit of ABBA, checking your PDA, and looking anywhere BUT the screen.

Mash your face around on the keyboard.

This, my labfriend, is more like a real subject.

If your task doesn't break and ends at the correct time, you're golden. Otherwise, go in and figure out how your bender clown of a test subject unlocked the timing. There are two likely suspects: 
  - The end action field should in most cases be set to "none" for ALL slides in your task. When Jumpy McFingers jumps the gun and responds to a slide other than the response slide, the wrong end action will cause that slide to immediately end, to be followed by the next. In the past the lab has not been careful about this because the assumption has been that the subject won't be responding during the non-response slides. WRONG. Be careful to check this when you inherit a historic script.
  - Drift adjust, which may do some wacky things if the parameters aren't correct. If you have questions about end action options, contact me (jpg).
  
<a name='variables-vs-attributes'></a>
## Variables vs. Attributes
EPrime has a very unusual distinction between two ways of storing program data: variables and attributes. The simplest difference between them is this: variables are used to store information about the state of the program, and attributes are used for data you want logged in the output .edat file. __This is so important it bears repeating:__
  - Variables are to control the program.
  - Attributes are for data you want logged.
  
Here's how they work:

<a name='variables'></a>
### Variables
  - Before you can use a variable, it must have a Dim (short for dimension) statement, like this: Dim myVar as Integer
  - Variables may be declared (Dim'ed) in the User tab of the Script window if you want the variable to be completely global. Variables may also be declared in an Inline object if you want the variable to have a more limited scope. Scope is how much of the program a variable lives in: global variables have a global scope and can be seen anywhere, whereas variables declared in an Inline under a Procedure live only in that Procedure and any Procedure below it in the hierarchy (indented more). Note that since a List is higher up in the hierarchy than its children Procedures, when you have a List that executes a Procedure multiple times (has a weight >1) the variables in the Procedure will go out of scope and be redeclared every time the List executes it. This means they get blanked on every iteration.
  - To assign a value to a variable, use =
  
_Examples:\
myVar = 5\
myVar = myVar2 + 2\
myVar = myVar + 1 (this last statement increments myVar by 1)._

  - Do NOT use c.SetAttrib or c.GetAttrib on variables. It will do something you do not expect: that would create an attribute with the same name but independent of the variable.
  - Do NOT declare a variable in one scope and then again in a scope contained in the first one. For example, do not declare a variable in the User tab of the Script window and again in an Inline. This will create a second variable with a different value that will not get propagated back to the larger-scoped variable. Even if you don't understand what that means, just don't do it.
  - Variables cannot be accessed by cues or text slides for display purposes. You must use an attribute for this. Sometimes, though, you may wish to use a variable for convenience or scope reasons, and then set an attribute for the sole purpose of display. This is a reasonable thing to do under certain circumstances.
  
<a name='attributes'></a>
### Attributes
List fields are automatically made attributes. Any data logged by a response (RTs [reaction times], question responses, et cetera) are also automatically made attributes. You can also make attributes, using a different process from what you do for variables. Attributes are something like variables, but with some important differences:
  - Attributes are logged
  - Attributes use c.SetAttrib and c.GetAttrib to set and retrieve their values, respectively. You cannot use = to set attribute values.

_Examples:\
'set myAtt to 5\
c.SetAttrib myAtt, 5

'set variable myVar to value of attribute myAtt\
myVar = c.GetAttrib("myAtt")

'set myAtt2 to value of myAtt\
c.SetAttrib myAtt2, c.GetAttrib("myAtt")_

  - Attributes do not need to be declared (Dim'ed). In fact, NEVER Dim an attribute; that would declare an independent variable with the same name, and could confuse you and people who later modify your code. Instead, when you want to create or set an attribute, just do it with c.SetAttrib.
  - c.GetAttrib does not work on an attribute unless it already exists. This means you must have already called c.SetAttrib on that attribute, or it must have already been logged by a list.
  - Attributes exist simultaneously at multiple scopes. See next section.
  
<a name='attribute-scoping'></a>
### Attribute Scoping
__Attribute scoping is very, very strange, so please read this section carefully.__

Say we set an attribute. If we mess with the attribute at the same level of hierarchy, we are messing with the same attribute. Now say we add a List with some Procedures under it. This will create a new attribute, with the same name, that has an initial value of whatever its parent is set to. If this is unchanged, this inherited value will appear in the data table in italics. Let's say we do a c.SetAttrib on this attribute (in this Procedure inside a List). This will change this smaller instantiation of the attribute -- and NOT its parent. This smaller version will get logged, but if the List has a weight >1 for this Procedure, on each iteration a new smaller attribute will be created and re-inherit an initial value from its parent (which has not changed, and we cannot change from here). This is why we use variables for program control: we cannot change an attribute's parent from the scope of its child.

For each level of Lists inside Lists, EPrime has a special name for these child-level attributes. Starting at the level under the main Experiment node in the tree, the levels are:
  - att[Session]
    - att[Block]
      - att[Trial]
        - att[Sub-Trial]
        
I don't know if there are more. It probably just says Sub-Sub-Trial or something.

A final note on attributes: the value logged for an attribute is whatever it is when it goes out of scope. That is why in data files you always see attributes at the [Session] level with the same value -- it's a revisionist history of a sort. If you want to save intermediate values of a higher-level attribute, you will need to set another attribute, at the lower-level scope, to nab that value.

<a name='exception'></a>
### An exception to the attribute/variable distinction
There is one special case for the attribute/variable distinction. When you log a piece of data from a cue or slide or other display object, it is logged but should be accessed through code as a variable. Example: you have a cue name "MyCue" that you set to record the RESP field; that is, what the user pushed when they responded. This is logged. But, if you want to access this data from code while the program is running, you use the following syntax:

MyCue.RESP

as in the following line of code:

MyVariable = MyCue.RESP

<a name='displaying'></a>
## Displaying Feedback, Dynamic Timing, Formatting
In the Properties menus for all objects (cues, text slides, etc.), the only "variables" you can access are attributes; you cannot access your variables from there. That means that to display dynamic feedback or to do dynamic timing, you may control your data through variables but must eventually set an attribute to the value you wish to display or use for timing. These attributes can be accessed in the object by putting the attribute's name in brackets like this: [myAtt]. I really recommend using variables to keep track of dynamic timing numbers because of scope issues, but before you can use the numbers you will have to do something a bit tacky like this:

c.SetAttrib TgtDur, myVar

The Format function in Basic languages is used to make numbers display how you want them, and is quite powerful. While you may occasionally have to use concatenation (the & operator) for data display, this can often be avoided. The idea is to call Format like this:

Format(myNum,{formatting-string})

where {formatting-string} is a string (like "0.00") used as a template to display the number. The string can contain both placeholder-numbers and symbols, and can treat all numbers the same or treat positive and negatives differently. Format is best demonstrated. The results of each Format statement are written immediately to the right.

Dim positive as Single\
positive = 12.3456\
Format(positive) 12.3456\
Format(positive,"0.00") 12.34\
Format(positive,"0.00#") 12.345\
Format(positive,"0.00000") 12.34560\
Format(positive,"0.00###") 12.3456\
Format(positive,"000") 012\
Format(positive,"$0.00") $12.35\
Format(0.583,"0.00%") 58.30%

Format expressions with multiple fields separated by semicolons separate by signs. Two fields will use the first for numbers >=0 and the second for negatives, three fields will do positive, negative, and zero separately.

Dim negative as Integer\
Dim zero as Integer\
negative = -20\
zero = 0\
Format(negative) -20\
Format(positive,"+0.00;-0.00") +12.35\
Format(negative,"+$0.00;-$0.00") -$20.00\
Format(negative,"$0.00") ($20.00)\
Format(zero,"+$0.00;-$0.00") +$0.00\
Format(positive,"+$0.00;-$0.00;$0.00") +$12.35\
Format(negative,"+$0.00;-$0.00;$0.00") -$20.00\
Format(zero,"+$0.00;-$0.00;$0.00") $0.00\

<a name='move-copy-shortcut'></a>
## Move, Copy, Shortcut
This section is about moving stuff around in the hierarchy tree on the left. You should read this section even if you think you're clear about it. These operations occur when you click and drag something in the tree to another place in the tree. Depending on exactly where you move something from and where you want it to end up, you will get different actions by default. In particular, Shortcut is not an intuitively obvious operation.

To start, here are the exact definitions of what each of these things are:
  - Move: Take this object and put it somewhere else. It ends up ONLY in the destination, and is gone from its original location.
  - Copy: Take this object and put it somewhere else. It ends up BOTH in the old and new locations, but has a slightly different name in the new location (usually the old name with the number "1" appended). The resulting two objects are INDEPENDENT; that is, if you change one, the other does not change.
  - Shortcut: Here's the tricky one. This makes a reference to the same object, and has the same name. The resulting objects are DEPENDENT; that is, if you change one, the other changes as well. Note: Any two objects of the same type with the same name are really shortcuts. Be very careful with this.
  
As stated above, clicking and dragging an object to a new location in the hierarchy tree has a different default behavior depending on the relationship between the source location and the destination. Here are the default behaviors:
  - When dragging between two locations in the exact same scope (e.g., under the same procedure, and not nested any more or less deeply): Move.
  - When dragging between any other kinds of two locations (as far as I can tell): Shortcut.
  - Note that Copy is never the default behavior.
  
Fortunately for us, these operations all look somewhat different on the screen. Notice that when you do any of these operations, the pointer get a little hollow gray rectangle trailing along with it, and has an additional little symbol depending on which operation you're doing. Further, you can force the program to do the one you want. These two bits of info are in this last list below:
  - Move: Appears as just the pointer with the little rectangle. To assure a move, hold down Shift while clicking and dragging the object.
  - Copy: Appears as the pointer with the little rectangle, with a little plus sign ("+") in a box. To assure a copy, hold down Ctrl while clicking and dragging the object.
  - Shortcut: Appears as the pointer with the little rectangle, with a little curved arrow in a box. To assure a Shortcut, hold down Ctrl+Shift while clicking and dragging the object. You generally will not want to do this, but it can be handy for repetitious code. However, remember that all the code / everything on a slide must be identical; change it in one place and you change it in all of them. If you don't understand this, it is easy to get confusing errors, especially if you have Inlines like "ResetVars" at multiple scopes, and try to change the lower one not to reset something like "TotalWinnings".
  
<a name='some-general-tips'></a>
## Some General Tips
Whenever you have a "magic number," like how many milliseconds a trial will take or the initial value for some important process, try to put it in a variable, and put the variable near the top of whatever Inline \*it's written in (or the top of the User Script). This makes it much easier to go back and adjust timing or change how long a cue is presented or alter the seed for dynamic timing.
  - Build in the ability to modify the code easily using techniques like the one above. Think ahead to the person who will have to change update your code.
  - Give variable descriptive names. What does g_total mean? Now, what does TotalMoney mean? Even if you don't know exactly, you can probably figure it out a lot faster.
  
<a name='eprime-timing-issues'></a>
## EPrime Timing Issues
See Jenny for information about EPrime timing issues.

<a name='implementing'></a>
## Implementing Drift Adjust in E-Prime
Rationale: Although you may specify exact durations for various intervals in your experiment, in paradigms that involve a large number of repeated intervals (i.e. most of them) you will find that the sequence of events can drift as much as 10ms per interval from the expected time. That is, if you have a 10 second trial duration, 100 trials into the experiment you may find that the actual time at the beginning of trial #101 is 1001 sec from the beginning of the experiment instead of the 1000 sec that you would expect (a 1 second “drift”). If you divide the trial further into sub-intervals and include a lot of picture loading your drift can be even greater. The drift adjust code below performs a clock check every trial and adjusts the trial lengths appropriately so that the actual timing is locked into the expected intervals. Explaining code in a document is difficult so please ask the lab coordinator if you have questions about including drift adjust in your e-prime task.

Implementation (3 steps):

1. Include the code below at the beginning of the experiment (for scanning paradigms you would want this immediately following the magnet ‘trigger’) to store the actual start time of the experiment in an attribute named “TrigTime”.
```
g_Trigtime = Clock.ReadMillisec
c.SetAttrib "TrigTime", g_Trigtime
```
2. Figure out a good place during a trial to check the clock drift, near the beginning of the trial is a good place. The clock check should occur at a time when you have an actual time marker (e.g. Cue.OnsetTime in the MID task) to compare to an expected time when that event should have occurred. Then you can take the difference and store it as your DriftAdjust for that trial. Once you have the DriftAdjust calculated, find another trial interval and subtract DriftAdjust from its duration. The code below is placed between the Cue and Target intervals in the MID task, variables you will likely need to modify are marked in italics
```
'This inline makes sure that timing does not drift from calibration from the TRs. 
'TRLength should be set to the number of msec used for a TR
Dim TRLength as Integer

'Needed to possibly correct for undershoot. Probably don't play with this.
Dim MaxNegative as Integer

'Intermediate for calculations.
Dim TotalTimeTemp as Long
TRLength = 2000
MaxNegative = 100

'Calculate how much time has elapsed since the magnet started, plus a factor to make
' sure the results are positive in case of undershoot. This factor is subtracted back out
' later.

TotalTimeTemp = Cue.OnsetTime - TrigTime + MaxNegative
c.SetAttrib "DriftCorrection", (TotalTimeTemp Mod TRLength) - MaxNegative

'Take the drift out of the post-target delay
c.SetAttrib "Delay2", c.GetAttrib("Delay2") - c.GetAttrib("DriftCorrection")

'Make sure the post-target delay doesn't end up <0. If so, allow remainder of
' correction to be done next cycle.
If c.GetAttrib("Delay2") < 0 Then c.SetAttrib "Delay2", 0
```
In the above code, the attribute Delay2 corresponds to the duration of the interval that we are adjusting to account for the clock drift. MaxNegative is included to limit the amount of correction for drift in the opposite direction (when the trial was shorter than you would expect, rather than longer – this is unusual and may indicate another type of timing problem in your program).

3. Remember to set the duration of the appropriate interval to be [Delay2].

<a name='ready-for-scanner'></a>
## How to make an Eprime task ready for the scanner
Several modifications need to be made in behavioral EPrime tasks to make them useable in the scanner.
1. __SRBox__
      - This is also relevant to behavioral experiments for which you will be using an SR Box attached to the computer. To enable SR box responses, open your EPrime file and go to\
      Edit → Experiment → Devices\
     to check to make sure the SR Box option is available. (If you don’t see it listed, click the “Add” button below to add it, then check it.)
    - You also need to enable it for individual slides (that is to say, slides during which your subjects will be making a response). You need to do this individually for each slide associated with a response. Open your each slide, select (NameofSlide, in this case ChoiceResponse) from the pull-down menu above (the NameofSlide option is at the very top, hidden when you first click the drop down). Then press the Property Pages button immediately to the right of the pull-down menu.
    - Navigate to “Duration/Input” in the Property Pages menu and make sure that the SR Box option is checked.
    - If the SRBox has been added globally and for each input slide, you should be set. Note that this is also the menu tab where you set the duration the slide will appear for, allowable keyboard and SR box responses, correct responses, time limits, and instructions for EPrime as to what it should do at the end of the presentation of that particular slide.
2. __Trigger__
    - The scanner must be triggered to begin functional (BOLD) data acquisition when the task begins, allowing task and scanner timing to be synchronized. After the last instruction screen in EPrime (which is usually terminated with a 'Spacebar' press by one of the scanner operators), insert the 'trigger' via an Inline element, with the text copied (and usually unmodified) from a previous task.
3. __Magnet Stabilization__
    - After the scanner is triggered to begin acquisition, several TRs should elapse before the task begins and data is actually collected to allow for magnet stabilization (data acquisition here is noisy). So, insert a text element that lasts for 12seconds ( = 6TRs, usually) after the trigger and before the Runlist. The screen is usually a white x or + on black background.
4. __Drift adjust (see 'DriftAdjust' handout for details of function)__
    - Each run of a task usually takes a little bit more time than it should, so to keep EPrime in synch with the scanner, which acquires data very precisely, we need to add or subtract a little time from each run (ex: an 8 second MID actually clocks in at 8020; we need to cut 20ms from somewhere in the next trial). So, find a period in the task where such messing with timing won’t affect anything critical - in the MID, this is in the post-target delay period, while in a purchasing task, it is in the pre-product fixation period.
    - Check out the scripts used in a few different tasks to make this clearer.
    - Sometimes the driftadjust is calculated and adjusted in the same Inline element, and sometimes the driftadjust is calculated in one and adjusted in another element. e.g.: the amount of drift to be corrected might be calculated at the end of a run, but the task requires that the adjustment be done in the pre-run fixation. So a value for 'DriftAdjust' is stored at the end (say, in 'Driftadjustcalc') and actaully subtracted/added at the beginning (in 'Driftadjust').
    - Check the variables to use for each separate task.
5. __Adding Button Box (SR Box) Responses__
    - First, click on the "Edit" tab in E-Prime and go down to "Experiment." In the "Devices" section, make sure that SRBOX is selected as an available device
    - Now, for every slide that requires the subject to respond, jump into the slide's DataLogging section and verify that SRBOX is selected as a device.
    - Now, \*AND THIS IS IMPORTANT*, the button box at the scanner has four buttons. However, the values for the buttons are not the intuitive 1,2,3,4 but rather 1,2,3,5. The "4" input corresponds to physio data rather than the final button on the button box. Strange, I know. In the DataLogging section, make sure that 1,2,3,5 are allowable response options for the button box. Also, jump into your E-prime scripts to make sure your task feedback will respond to an input of "5" as opposed to "4."
6. __Slide Layout__
    - Due to limitations of the scanner projector, the top and bottom 20% of your screen will likely be invisible to the subject when they are inside the magnet. It is important to keep this in mind when constructing your tasks. Whatever stimuli they need to see to successfully complete the task, make sure it fits within the middle 60% of the page.
7. __Final Checks__
    - Multiply the time of each trial by the number of trials and add in seconds for the magnet stabilization.
    - Ensure that data logging is correct, etc
