#BMML File Format Specification

BMML is the Balsamiq Mockups Markup Language, the flavor of XML used by Balsamiq Mockups to save its data.
	
This document outlines the BMML file format, in the hopes that this will be useful to you when integrating Mockups in your daily work.  You could for instance import BMML into your
tool, write a BMML parser which generates HTML or MXML or XAML or running code...the sky is the limit!

**Warning:** there are some things that aren't documented here, like what properties each control type supports. To figure those things out, just open Mockups, drag a control type to the canvas, change some properties in the property inspector and look at the resulting BMML.  In other words, we try our best to keep this document up‑to‑date, but you should trust
shipped code more. :)

>Remember that XML syntax is case‑sensitive, e.g., <tag> and <Tag> are different.

##Quick Overview

Here's a sample BMML containing a single `Callout` control:

<table>
<tr>

<mockup version="1.0" skin="sketch" fontFace="Balsamiq Sans" measuredW="941" measuredH="169" mockupW="36" mockupH="40">

<controls>

<control controlID="1" controlTypeID="com.balsamiq.mockups::CallOut" x="644" y="129" w="-1" h="-1" measuredW="36" measuredH="40" zOrder="1" locked="false" isInGroup="-1">

<controlProperties>

<text>Hello!</text>

<backgroundAlpha>0.25</backgroundAlpha>

<color>65280</color>

</controlProperties>

</control>

</controls>

</mockup>```
</table>
As you can see, the top‑level tag is a 'mockup' tag. Within it, there's a 'controls' tag, which
includes a list of 'control' elements, one for each control in the mockup. Each control element
can include an optional 'controlProperties' element, with different children depending on the
type of element. We will see below what all the attributes mean in detail.
The Mockup Tag
Here's the DTD snippet for the mockup tag:

```<!ELEMENT mockup ( controls? ) >
<!ATTLIST mockup measuredW NMTOKEN #REQUIRED >
<!ATTLIST mockup measuredH NMTOKEN #REQUIRED >
<!ATTLIST mockup mockupW NMTOKEN #REQUIRED >
<!ATTLIST mockup mockupH NMTOKEN #REQUIRED >
<!ATTLIST mockup skin NMTOKEN #REQUIRED >
<!ATTLIST mockup fontFace NMTOKEN #REQUIRED >
<!ATTLIST mockup version NMTOKEN #REQUIRED >
<!ELEMENT controls ( control? ) >```

A `mockup` tag includes a `controls` tag, described below. measuredW and measuredH are the dimensions, in pixels, of the mockup, including the top‑left white space around it. mockupW
and mockupH are the dimensions of the mockup without any space around it. In other words,
this would be the size of the PNG if you exported it. The skin tag, not used prior to 2.2.1, can
either be "sketch" or "wireframe". The fontFace tag, introduced in 2.2.1, can either be
"Balsamiq Sans" or "_sans". The version tag is for the version of the BMML specification, which
is 1.0 so far. The controls tag is simple, it doesn't have any attributes and just contains a list
of 'control' tags.
The Control Tags

Here's the DTD snippet for the control tag:

`<!ELEMENT control ( controlProperties? ) >`
`<!ATTLIST control controlID NMTOKEN #REQUIRED >`
`<!ATTLIST control controlTypeID NMTOKEN #REQUIRED >`
`<!ATTLIST control w NMTOKEN #FIXED "-1" >`
`<!ATTLIST control h NMTOKEN #FIXED "-1" >`
`<!ATTLIST control measuredH NMTOKEN #REQUIRED >`
`<!ATTLIST control measuredW NMTOKEN #REQUIRED >`
`<!ATTLIST control x NMTOKEN #REQUIRED >`
`<!ATTLIST control y NMTOKEN #REQUIRED >`
`<!ATTLIST control zOrder NMTOKEN #REQUIRED >`
`<!ATTLIST control isInGroup NMTOKEN #FIXED "-1" >`
`<!ATTLIST control locked NMTOKEN #FIXED "false" >`

The control tag can contain a controlProperties tag, described below.
controlID is always unique for this list of controls and identifies this control's instance in
the mockup controlTypeID in the control type.
w and h represent the size in pixels of the control.
A value of ‑1 means that you should look at measuredW or measuredH instead.
measuredW and measuredH represent the size in pixels of the control, when shown in its
'natural state', i.e., it's the preferred dimension of the control as dictated by the control
itself. For instance, for a Label control, measuredW would be large enough to show the
whole text with no cropping.
x and y represent the position in pixels of the control, relative to the top‑left corner of the
canvas.
zOrder represents the layer ordering of this control.
Values are unique and sequential within the current list of controls.
isInGroup tells you if a control is part of a group (‑1 means "no", otherwise it uses the
controlID of the group)
locked is a flag that tells you...you guessed it!
The ControlProperties Tag
The controlProperties tag contains a child for each property that can be modified for a control
([using the property inspector](http://support.balsamiq.com/customer/portal/articles/110114)).

#Control Properties List

>Here's a list of the different control properties and where they are used
	
#`Lists` and `Data Grids` controls

`alternateRowColor`, `rowHeight`, `hasHeader`, `vLines`, `hLines` 

<table>
<tr><small>**backgroundAlpha** 
(`0`..`1`): 

>the transparency of the background

#iPhone Control
	
<table>
<tr><small>**bgPattern**(`allWhite`,`allBlack`,`topOnly`,`topBlueBottom`,`topBlackBottom`)
<table>
<tr><small>**topBar**(`true`,`false`)

#Text Controls

>any control with text in it

`bold`, `italic`, `underline`, `align`, `size`

#Controls with Borders

>all controls which can show a border or not

<table><tr><small>**borderStyle** (`none` or `square`)

#`Dialog` and `Window` Controls

>used by the Dialog /
Window control

`close`, `minimize`, `maximize`, `dragger`, `topheight`, `bottomheight`

<table><tr><small>**color** (`int`): 
	
	the color of the text or the background of a component,
	depending on the control type
	
<table><tr><small>**crop** (`x`,`y`,`h`,`w`): 
	
<table>
	
	four comma separated values with range...
`[0,1000]`	
	that represent cropped bounds ratios respect 
	to full control size. 
	
>For example 0,0,1000,1000 represents the full control.

<table><tr><small>**customData** (`String`)
>added on 10/21/2009, [see here](http://blogs.balsamiq.com/product/2009/10/21/weekly‑release‑custom‑ids‑and‑bug‑fixes/) for details

<table><tr><small>**customID** (`String`)
>added on 10/21/2009, [see here](http://blogs.balsamiq.com/product/2009/10/21/weekly‑release‑custom‑ids‑and‑bug‑fixes/) for details

#Image Control

<table><tr><small>**filter**(`true`) 

	true if the image should be sketched

#`Arrows`, `Tabs`, `Curly Braces`, `Pointy Button` etc.

<table><tr><small>**direction** (`left`, `center`, `right`, `bottom`, `top`)

#`Button`, `Canvas`

	all controls that can have a 
	single link to other controls

<table><tr><small>**href**  

<table>
<tr>

1.	Mockups saved before 9/14/2009 only contain the file name to link to,  	

	i.e. `foo.bmml` with no path. 

2. Mockups saved after 9/14/2009 contain a string of this form: 	

	`<name>&bm;<viewURL>&bm;<loadURL>&bm;<editURL>`. 
	
>The last 3 values are all the same for Mockups for Desktop but are different in the plugin versions.

#Accordion, LinkBar

	like href, but used by controls that link to 
	multiple files

<table><tr><small>**hrefs** 

	It's a comma‑separated list of strings 
	formatted the same way as a single href string 
	
>(see previous bullet). 
>
>TODO-ADD Reference link to previous bullet `href`

<table><tr><small>**icon**

	a string composed of both

`iconID`of the icon to use for this control and its `size`

like this 

`iconID`|`48`
	
	There are over 220 icons, each with a unique ID. 
	
	The sizes are either 16, 24, 32 or 48. 
	
>As of version 2.1.7, 
>

the left part can also be a src based on where the custom icon is loaded from.

<table>**Example 1:** 

`./assets/filename`|`32` 

**Example 2:** 

`$ACCOUNT/assets/filename`|`32`	

#`Progress Bar` Controls

<table><tr><small>**indeterminate** (`true`,`false`) and **scrollbarValue** (`0‑100`)

#`Icon` and `Label` Controls

<table><tr><small>**labelPosition** (`bottom` or `right`)

#`Arrow` Controls

<table><tr><small>**leftArrow** (`true`,`false`),
**rightArrow** (`true`,`false`), **curvature** (`‑1`, `0`, `1`)

<table><tr><small>**map** (`string`)
>added on 9/14/2009
	
	an escaped image map 
	for the linked areas of the mockups, 
	following the same format as the HTML map tag.

#`Switch` control	

<table><tr><small>**onOffState** (`on` or `off`)
 
#`iPhone` Control

<table><tr><small>**orientation** (`portrait` or `landscape`)

#`Component` Control

**override**

>(see below)

#`Vertical Tab Bar` Controls

<table><tr><small>**position** (`left` or `right`)

#`List` Controls

<table><tr><small>**selectedIndex** (`‑1 for none`)
	
	the selected item in a list

#`Image` and `Component` Controls

<table><tr><small>**src**

It's either a relative path (for images
loaded from disk) or an http url. If it starts with $ACCOUNT/assets, it means the image is
in the account assets folder.
state ('up', 'selected,', 'focused', 'disabled', 'disabledSelected'): the state of the control
tabHPosition and tabVPosition: used by the Tab Bar controls
tooltipDirection (NW, N, NE, E, SE, S, SW, W): used by the tooltip control
value: the value of a scrollbar, from 0 to 100
verticalScrollbar (bool) and value (0‑100): whether the control has a scrollbar or not, and
its value (0 means "top", 100 means "all the way down")
The Component Control
The Component control, known as "Symbol" in Mockups, contains a link to another bmml
(here are the docs for the Symbols feature
(http://support.balsamiq.com/customer/portal/articles/110439)). The link is an url with an
optional anchor. If the anchor is missing, then the whole bmml is the target. Otherwise, the
target is a group which has the same name of the anchor. The group must be at top level in
the bmml.
<control controlID="1" controlTypeID="com.balsamiq.mockups::Component"
x="588" y="92" w="407" h="314" measuredW="407" measuredH="314"
zOrder="1" locked="false" isInGroup="-1">
<controlProperties>
<override controlID="1" x="0" y="0" w="116" h="70">
<color>7775663</color>
<text>This text is overridden</text>
</override>
<src>./assets/symbols.bmml%23Options%20dialog</src>
</controlProperties>
</control>
The ControlProperties tag of a Component control can contain an override tag, for the
properties of the Symbol that have been overridden in this symbol instance. Attributes of
override tag:
controlID: id of the overridden control as in the symbol definition. In case the control is
nested in groups, then the controlID is the relative path to the control (for example,
"2:3:0" is the control with id 0 belonging to group 3 nested in group 2 in the symbol
definition).
x, y, w, h: (optional) these attributes override the original size and position of the control.
Measured dimensions are not saved in the bmml.
It is not possible to override the zOrder attribute. It is not possible to add or remove controls
in the symbol instance.
