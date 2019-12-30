#Time Sheets

a repo for timesheets on various projects using a pomodoro-like timing method

Note: This is not in any way comprehensive, at the time of writing I still 90%+ of my work completely unlogged, I usually use timesheets for specific projects or in specific situations where it helps.

##What is this?
Time Sheets is a public facing work experiment based on a basic work time keeping method by [Steve Lambert](https://visitsteve.com/) (I don't know if he started it but he was the one who introduced it to me) 

The most basic version just involves setting a time for 20 minutes, every 20 minutes, and recording what you're doing at that time. More than anything else, this method helps you see how much time you're actually spending on a certain task as opposed to how much time it feels like. This helps you see when you're spending too long on a nominal task, or not enough time on an important task, both of which are very hard to purely by feel

##How does it work?

Naturally I got the itch to completely automate the whole process of timing. I use MacOS so the tools I've written work on Mac, but they're created in bash so can easily be used in linux with some slight modifications (and perhaps the linux subsystem in windows? not sure how that works) 

tldr: I select a plaintext timesheet file using "touch" then use a simple bash script to insert the date, wait 20 minutes, alert me, then find the last touched file in the folder and open it automically. (Also I put everything in aliases duh)

Also the whole folder is a very simple git repo that I just  ocasionally add all and push 

The tools are so small that I'm just going to paste them here rather than linking files

You will need need

- a folder in your home diretory called timeSheets (add one or more .txt timesheet files) 
- a file in your home directory called everyTwenty.sh
- the following aliases for bash

runs the time script

`alias t='cd ~/ && ./everyTwenty.sh'`

finds the last edited text file (only seperate because I ocasionally use this seperately)
`alias lastTxt='find . -type f -name "*.txt" -print0 | xargs -0 ls -t | head -1'`

lists the timesheets (optional)
`alias ts='cd ~/timesheets && l'`

opens the last used timesheet (optional)
`alias te='ts && open $(lastTxt)'`

Here is the code for everyTwenty.sh

```
total=0

#writes the date to the current timeSheet
cd ~/timesheets && date "+%Y-%m-%d @ %H:%M %p" >> $(find . -type f -name "*.txt" -print0 | xargs -0 ls -t | head -1)

while true
do
	echo "Timer Started at" && date +"%I:%M"

	#wait 20 minutes
	sleep 1200

	#use below lines to have a speech synth timer sound
	#sleep 5
	#say "beep beep beep"

	#shows mac pop up (this changes window focus so is useful for timing without sound)
	osascript -e 'tell app "System Events" to display dialog "Its Been 20 Minutes"'

	let "total=total+20"
	echo "Its been a total of $total minutes"

	#opens the last edited timesheet
	cd ~/timesheets && open $(find . -type f -name "*.txt" -print0 | xargs -0 ls -t | head -1)
done
```


(Note: Yes! this is a dumb script! It just sleeps for 20 minutes, if you close your laptop it will stop timing yada yada yada. I wrote a more complex version of this in python but at the end of the day I come back to the dead simple bash one because its easier and it works)


1. create one or more .txt timeSheet files in the timeSheets directory
2. 






