# efind

I've abandoned this project to work on a new version in Go.
I'll likely make that repo public once it's more mature.

## Archive

### Why?

Ever find yourself running 'find' and then tacking a million 'greps'
on the end of it to get the list of files you want?  I use 'find' on
the Linux Kernel, pipe the output to a file, and then spend 10 minutes
editing in emacs so I can use the results to generate a manageable tag
file for the HW target I'm coding.  But what happens when you add a
file or two and want them tagged too?  You have to add it manually.
What happens when you lose the filelist?  Time to generate the list
again.


### Potential Solution

I thought about the problem for a while and hacked up some Python to
get easier control over file list generation.  You shouldn't even have
to traverse directories you don't want, IMHO.  'efind', or Enhanced
Find, lets you specify actions at directories of your choice.  The
'config' file I use to get a file list from the kernel, for example,
is shown below.

	filter C
	include arch include drivers block init kernel lib mm net
	at arch include mips
	at arch/mips include cavium-octeon
	at include include asm linux net
	at drivers include ata base char gpio i2c ide leds misc mtd net of rtc platform serial usb watchdog
	at drivers/net include e1000 octeon phy usb
	at net include 802 core ethernet ipv4 netfilter packet sched

What if you just wanted to do 'efind -f pics' to find all the pictures
in the current and child dirs?  Now you can with the 'pics' filter,
which is just a collection of file extensions that are probably
pictures.  I used the "C" filter in my kernel config above.

The efind tool script is a work in progress and a way to experiment
with ideas.  It seems to work pretty well so far.  There are a few
things I'd like to add already.  The kernel config above could be more
efficient if efind implicitly added an 'at arch include mips' rule
after it parsed 'at arch/mips include cavium-octeon', for example.
