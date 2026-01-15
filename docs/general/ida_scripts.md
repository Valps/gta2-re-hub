# IDA scripts

IDA scripts that you can run on `File -> Script file` or just pressing `Alt+F7` and choosing the script file.

## Import match status

It updates the color of IDA functions based on the matching progress.

The IDA function colors are:

* Black: Matched functions.
* Red: Boot to map functions that are not matched.
* Green: Frontend functions that are not matched.
* Brown: Implemented functions but not matched.
* Light brown: Stubbed functions.
* White: Functions that are not stubbed.
