# Linux bash code snippets

A collection of useful Linux bash code snippets that I keep looking up over and over again ;-).  
NOTE: Intended for use in `#!/bin/bash` files ONLY! Tested with `GNU bash, version 5`.

## Date and time

- Get the "now" date-time in a certain format: `NOW=$(date +"%Y_%m_%d_%H:%M:%S")`

## Environment

- Get the bash-script folder, e.g. to ref. a log file: `LOG=$(dirname "$BASH_SOURCE")"/log.out"`

## Write files

- Create or overwrite file: `echo "Hello world" > file.name`
- Append a "log" entry (using date and file from above): `echo "$NOW - [source] - [message]" >> "$LOG"`

## Arguments, IF-ELSE and variable check

- Print script argument one and two: `echo "$1" and "$2"`
- Print arg one if its given: `if [ -n "$1" ]; then echo "$1"; fi`
- Print arg one if its "correct": `if [ -n "$1" ] && [ "$1" = "answer" ]; then echo "$1 is correct"; fi`
- Test if argument is numerical 1 (NOTE: fails if not): `if [ $1 -eq 1 ]; then echo "$1 is 1"; fi`

## User-prompt

- Pause and wait for key-press to continue: `read -p "Press any key to continue."`
- Prompt user for input and store answer in 'yesno' var.: `read -p "Enter 'yes' to continue: " yesno`

## Pipe, grep, etc.

- Get last file in current folder matching regular-exp.: `LAST_FILE=$(ls | grep ".*sh" | tail -n 1)`
- Print entries in file matching reg-exp and sort unique + reverse: `cat log.out | grep "^2021_10" | sort -ru`

## Example: Input Menu

```
while true; do
	echo ""
	echo "What would you like to do next?"
	echo "1: Run script A"
	echo "2: Run script B"
	echo "0: Exit"
	echo ""
	read -p "Enter a number plz: " option
	echo ""
	if [ -z "$option" ] || [ $option = "0" ]
	then
		break
	elif [ $option = "1" ]
	then
		clear
		echo "Running script A ..." && sleep 2 && echo "DONE"
	elif [ $option = "2" ] 
	then
		clear
		echo "Script B is running ..." && sleep 2 && echo "DONE"
	else
		clear
		echo "Not an option, please try again."
	fi
	option=""
done
```

## Example: Count something and act

```
sound_card_count=$(aplay -l | grep "^card" | wc -l)
if [ $sound_card_count -gt 0 ]; then
	echo "Found $sound_card_count cards"
fi
```

## Example: Define and use function

```
# define:
is_armv7l() {
	if [ -n "$(uname -m | grep 'armv7l')" ]; then return 0;
	else return 1; fi
}
# use:
if is_armv7l; then
	echo "System is ARMv7l"
fi
```
