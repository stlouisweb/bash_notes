# BASH Scripting

### Table of Contents
- [Comparison Operators](#comparison)

## Comparison operators

**Integer Comaprison**

```-eq```
is equal to

	if [ "$a" -eq "$b" ]

```-ne```
is not equal to

	if [ "$a" -ne "$b" ]

```-gt```
is greater than

	if [ "$a" -gt "$b" ]

```-ge```
is greater than or equal to

	if [ "$a" -ge "$b" ]

```-lt```
is less than

	if [ "$a" -lt "$b" ]

```-le```
is less than or equal to

	if [ "$a" -le "$b" ]

```<```
is less than (within double parentheses)

	(("$a" < "$b"))

```<=```
is less than or equal to (within double parentheses)

	(("$a" <= "$b"))

```>```
is greater than (within double parentheses)

	(("$a" > "$b"))

```>=```
is greater than or equal to (within double parentheses)

	(("$a" >= "$b"))

- - -

**String Comparison**

```=```

is equal to

	if [ "$a" = "$b" ]

> *Caution*
Note the whitespace framing the =.

```if [ "$a"="$b" ]``` is not equivalent to the above.

```==```
is equal to

	if [ "$a" == "$b" ]

This is a synonym for =.

```!=```
is not equal to

	if [ "$a" != "$b" ]

This operator uses pattern matching within a [[ ... ]] construct.

```<```
is less than, in ASCII alphabetical order

	if [[ "$a" < "$b" ]]

	if [ "$a" \< "$b" ]

> Note that the "<" needs to be escaped within a [ ] construct.

```>```
is greater than, in ASCII alphabetical order

	if [[ "$a" > "$b" ]]

	if [ "$a" \> "$b" ]

> Note that the ">" needs to be escaped within a [ ] construct.


```-z```
string is null, that is, has zero length

 	String=''   # Zero-length ("null") string variable.

	if [ -z "$String" ]
	then
	  echo "\$String is null."
	else
	  echo "\$String is NOT null."
	fi     # $String is null.

```-n```
string is not null.

#### EXAMPLE: Arithmetic and string comparisons
	#!/bin/bash
	
	a=4
	b=5
	
	#  Here "a" and "b" can be treated either as integers or strings.
	#  There is some blurring between the arithmetic and string comparisons,
	#+ since Bash variables are not strongly typed.
	
	#  Bash permits integer operations and comparisons on variables
	#+ whose value consists of all-integer characters.
	#  Caution advised, however.
	
	echo
	
	if [ "$a" -ne "$b" ]
	then
	  echo "$a is not equal to $b"
	  echo "(arithmetic comparison)"
	fi
	
	echo
	
	if [ "$a" != "$b" ]
	then
	  echo "$a is not equal to $b."
	  echo "(string comparison)"
	  #     "4"  != "5"
	  # ASCII 52 != ASCII 53
	fi
	
	# In this particular instance, both "-ne" and "!=" work.
	
	echo
	
	exit 0

#### EXAMPLE: Testing if a String is *null*
	#!/bin/bash
	#  str-test.sh: Testing null strings and unquoted strings,
	#+ but not strings and sealing wax, not to mention cabbages and kings . . .
	
	# Using   if [ ... ]
	
	# If a string has not been initialized, it has no defined value.
	# This state is called "null" (not the same as zero!).
	
	if [ -n $string1 ]    # string1 has not been declared or initialized.
	then
	  echo "String \"string1\" is not null."
	else  
	  echo "String \"string1\" is null."
	fi                    # Wrong result.
	# Shows $string1 as not null, although it was not initialized.
	
	echo
	
	# Let's try it again.
	
	if [ -n "$string1" ]  # This time, $string1 is quoted.
	then
	  echo "String \"string1\" is not null."
	else  
	  echo "String \"string1\" is null."
	fi                    # Quote strings within test brackets!
	
	echo
	
	if [ $string1 ]       # This time, $string1 stands naked.
	then
	  echo "String \"string1\" is not null."
	else  
	  echo "String \"string1\" is null."
	fi                    # This works fine.
	# The [ ... ] test operator alone detects whether the string is null.
	# However it is good practice to quote it (if [ "$string1" ]).
	#
	# As Stephane Chazelas points out,
	#    if [ $string1 ]    has one argument, "]"
	#    if [ "$string1" ]  has two arguments, the empty "$string1" and "]" 
	
	
	echo
	
	
	string1=initialized
	
	if [ $string1 ]       # Again, $string1 stands unquoted.
	then
	  echo "String \"string1\" is not null."
	else  
	  echo "String \"string1\" is null."
	fi                    # Again, gives correct result.
	# Still, it is better to quote it ("$string1"), because . . .
	
	
	string1="a = b"
	
	if [ $string1 ]       # Again, $string1 stands unquoted.
	then
	  echo "String \"string1\" is not null."
	else  
	  echo "String \"string1\" is null."
	fi                    # Not quoting "$string1" now gives wrong result!
	
	exit 0   # Thank you, also, Florian Wisser, for the "heads-up".

**Compound Comparison**

```-a```
logical and

*exp1 -a exp2* returns true if *both* exp1 and exp2 are true.

```-o```
logical or

*exp1 -o exp2* returns true if either exp1 *or* exp2 is true.


	if [ "$expr1" -a "$expr2" ]
	then
	  echo "Both expr1 and expr2 are true."
	else
	  echo "Either expr1 or expr2 is false."
	fi
