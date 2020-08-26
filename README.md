# Notes

## Challenges on Linux

### For pre-recording lectures

* The Yuja desktop app doesn't exist.
* You can use the web version, but I found the performance to be poor.

## Equipment:

* A desktop machine with PopOS! 20.04. Specs shouldn't matter,
  as the only resource-intensive part will be the final encoding.
* Logitech Brio Web Cam. (This is not the recording device!)
* Samson C01U USB condensing microphone with boom stand and Samson PS01 pop filter.
  (A family member who does audio recording for a living sent me this as a hand-me-down.
   I will say that a good quality mic does matter.)

## Recording screen casts

I used [simplescreenrecorder](https://www.maartenbaert.be/simplescreenrecorder/):

```sh
sudo apt install simplescreenrecorder
```

I also tried [kazam](https://github.com/hzbd/kazam), but could never get
input levels sorted out right.

### The "show your face" trick

You need a webcam for this!

Colleagues elsewhere told me that students appreciated seeing the instructor's face 
in the corner. [This trick](https://leehblue.com/record-screen-and-webcam-ubuntu/) works, but I am not doing it.

In case the link goes dead:

```sh
sudo apt install mplayer
mplayer tv:// -tv driver=v4l2:width=400:height=300 -vo xv -geometry 100%:100% -noborder
```

Pressing `T` when focussed on the window will force the webcam grab to be "always on top".

The command shown above will display your webcam in the bottom right.  However,
if you use an auto-tiling window manager, then the window may not show up there!
Further, it has no border to grab and move.  The solution is to disable any
auto-tiling when recording.

Another option is to use `guvcview`:

```sh
sudo apt install guvcview
```

`guvcview` lets you use the mouse to move, and your window manager's border is
still there, which will probably let you (right) click on to say "always on top".
However, the `mplayer` method is cleaner because of the lack of window manager
decorations.

FWIW, `guvcview` seems like a good screencast recorder, too.  I am not 
familiar enough with its output formats to know if there would be any
downstream issues when editing.  Probably not, but I started with
`simplescreenrecorder` and that software outputs in boring old `mp4`,
so I ran with that.

NB: I didn't do this because I didn't learn that it was possible until I was
close to done. I also have glare issues and probably wore the same shirt for each
lecture.

NB: Apparently `kazam` supports this, but Ubuntu/Pop 20.04 doesn't have the right version.
This `kazam` info is from [here](https://askubuntu.com/questions/536563/how-to-record-the-screen-and-webcam-at-the-same-time),
but it is several years old, and the version referred to may never have been released?
It seems that this release is *still* in the unstable `ppa` after all this time.  I'm
tempted to call `kazam` a dead project at this point.

### Mic input volume

### Things to double check each time

* Is your input device set to the expected mic?
* Are you actually getting sound recorded? (I do a 5 second trial recording each time.)

### Helpful behaviors

Start recording (`Ctrl-R`), then make slides full screen, start talking after a pause.
The pause gives you a place to clip so the edited video starts right when you start talking.

If you need a break, or make a mistake requiring a do-over:

* Get out of full screen
* Wait a few seconds
* Hit `Ctrl-R` to stop recording
* Hit `Ctrl-R` to start recording
* Enter full screen
* Start talking again

These long pauses are easy to see in the tracks, making them easy to edit out.

## Saving the "raw" screen casts

During recording, they end up in `~/Videos`.  By default, each video gets
a name that is the prefix you choose plus a time stamp.  I ended up with 3 or 4
videos per lecture.

**Before editing**, I *move* these raw videos to a single locations where
I keep all the raw mp4 for all lectures.  For example, `~/Documents/class/videos/raw`.

I used default recording settings and made sure to set the output file name
to something sensible for each lecture.  Make sure that "Add Timestamp"
is checked because that means that your files get a time stamp in their name.  Thus,
multiple rounds of record/pause/record/save recording will go to `lecture-timestamp.mp4`
and there's no files getting over-written or other bad things.

It is important to move the videos to one place because the editing software I use
internally refers to the *full path* of the raw videos!  So, if you edit, and then
move the raw videos, you "break" the project file created by the editing software.
(Below, I talk about how to create a project file in the editing software that
can be used for long-term storage and moving around.)

## Editing videos

I used [kdenlive](https://www.kdenlive.org), which is a high-quality
nonlinear video editor.  It is way over-powered
for what we are doing here, but I found it simple
enough to use.

```sh
sudo apt install kdenlive
```

This sections is not a great tutorial.  I kinda learned by trial and
error.  This is a pretty full-featured program with a large user 
community.  You can find many tutorials online.

### File locations

If I edit in  `~/Documents/class/videos/raw`, I save the `kdenlive`
project files in  `~/Documents/class/videos/projects`.

**See below for note regarding paths in these project files and what to do for long-term storage.**

### File sizes

From `simplescreenrecorder`, I ended up with about `100MB` of `mp4` files per lecture (roughly 40 minutes 
of content).

### X and S

Pressing `x` changes into "cut" mode and `s` gets you back in
"drag the clips around mode".

### Encoding

I used [this profile](https://store.kde.org/p/1333301), but I'm still exploring 
best practices here.

### Export for long-term storage

By default, the project contains full paths to raw tracks,
making long-term storage a problem.  To solve this,
archive each project after editing via the `Project` menu.
Archiving creates a `.tar.gz` where you input tracks are copied in.

I don't bother with this until I'm all done with the entire class.
While in the throes of doing the work, I back up the raw mp4 and
the `kdenlive` project files to Google Drive and a second internal
drive on my machine.  It is easy enough for me to restore the original
full path names if anything goes wrong. (Famous last words...)

## Closed-captioning

UCI allows the videos to be uploaded to Yuja, which will auto-CC the file.
That option is enabled by default.  The files to into a queue, and it can
take a while.

The results are "okay".  In a perfect world, they'd be edited.  Unsure
if you can get the CC files in the standard format?  If so, there are
several open-source caption file editors out there.
