# targets: prerequisites
# 	command
# 	command
# 	command

# The targets are file names, separated by spaces. Typically, there is only one per rule.
# The commands are a series of steps typically used to make the target(s). These need to start with a tab character, not spaces.
# The prerequisites are also file names, separated by spaces. These files need to exist before the commands for the target are run. These are also called dependencies

# 'make' will run the first target (default target) in the Makefile

# make hello
hello:
	echo "Hello, World"
	echo "This line will always print, because the file hello does not exist."
	
# make blah
blah: blah.o
	cc blah.o -o blah # Runs third

blah.o: blah.c
	cc -c blah.c -o blah.o # Runs second

# Typically blah.c would already exist, but I want to limit any additional required files
blah.c:
	echo "int main() { return 0; }" > blah.c # Runs first

# make clean
clean:
	rm -f blah.c blah blah.o

# make some_file
some_file: other_file
	echo "This will always run, and runs second"
	touch some_file

other_file:
	echo "This will always run, and runs first"

clean1:
	rm -f some_file

# Variables (can only be strings)
# make child_file
files := file1 file2
child_file: $(files)
	echo "Look at this variable: " $(files)
	touch child_file

file1:
	touch file1
file2:
	touch file2

clean2:
	rm -f file1 file2 child_file

# make all
a := one two # a is assigned to the string "one two"
b := 'one two' # Not recommended. b is assigned to the string "'one two'"
all:
	printf '$a'
	printf $b

# Reference variables using either ${} or $()
x := dude
dude:
	echo $(x)
	echo ${x}
	echo $x  # Bad practice, but works

# Targets
three_files: one two three

one:
	touch one
two:
	touch two
three:
	touch three

clean3:
	rm -f one two three

# When there are multiple targets for a rule, the commands will be run for each target. $@ is an automatic variable that contains the target name.
all_files: f1.o f2.o

f1.o f2.o:
	echo $@
# Equivalent to:
# f1.o:
#	 echo f1.o
# f2.o:
#	 echo f2.o

# Print out file information about every .c file
print: $(wildcard *.c)
	ls -la  $?

thing_wrong := *.o # Don't do this! '*' will not get expanded
thing_right := $(wildcard *.o)

all_four: one_file two_file three_file four_file

# Fails, because $(thing_wrong) is the string "*.o"
one_file: $(thing_wrong)
	echo $(thing_wrong)

# Stays as *.o if there are no files that match this pattern :(
two_file: *.o 
	echo 'Exists'

# Works as you would expect! In this case, it does nothing.
three_file: $(thing_right)
	echo $(thing_right)

# Same as rule three
four_file: $(wildcard *.o)
	echo 'Same as rule three'

# Automatic Variables
hey: ek dui
	# Outputs "hey", since this is the target name
	echo $@

	# Outputs all prerequisites newer than the target
	echo $?

	# Outputs all prerequisites
	echo $^

	touch hey

ek:
	touch one

dui:
	touch two

clean4:
	rm -f hey one two

# static pattern rules 
# targets...: target-pattern: prereq-patterns ...
#    commands

objects = foo.o bar.o all_code.o
call_all: $(objects)

# These files compile via implicit rules
# Syntax - targets ...: target-pattern: prereq-patterns ...
# In the case of the first target, foo.o, the target-pattern matches foo.o and sets the "stem" to be "foo".
# It then replaces the '%' in prereq-patterns with that stem
$(objects): %.o: %.c

# These files compile via implicit rules
# similar to
# foo.o: foo.c
# bar.o: bar.c
# all.o: all.c

all_code.c:
	echo "int main() { return 0; }" > all_code.c

%.c:
	echo $@
	touch $@

clean5:
	rm -f *.c *.o call_all

# Pattern Rules
# A way to define your own implicit rules
# A simpler form of static pattern rules

# Double Colon Rules
# Double-Colon Rules are rarely used, but allow multiple rules to be defined for the same target.
temp: double
	echo "Temp"

double::
	echo "hello"

double::
	echo "hello again"

# Add an @ before a command to stop it from being printed
echoing:
	@echo "This make line will not be printed"
	echo "But this will"

exec_cmd: 
	cd ..
	# The cd above does not affect this line, because each command is effectively run in a new shell
	echo `pwd`

	# This cd command affects the next because they are on the same line
	cd ..;echo `pwd`

	# Same as above
	cd ..; \
	echo `pwd`

# Double dollar sign
make_var = I am a make variable
double_dollar:
	# Same as running "sh_var='I am a shell variable'; echo $sh_var" in the shell
	sh_var='I am a shell variable'; echo $$sh_var

	# Same as running "echo I am a amke variable" in the shell
	echo $(make_var)

# Recursive use of make $(MAKE)
new_contents = "hello:\n\ttouch inside_file"
recursive:
	mkdir -p subdir
	printf $(new_contents) | sed -e 's/^ //' > subdir/makefile
	cd subdir && $(MAKE)

clean6:
	rm -rf subdir

# The export directive takes a variable and sets it the environment for all shell commands in all the recipes:
shell_env_var=Shell env var, created inside of Make
export shell_env_var
expo:
	echo $(shell_env_var)
	echo $$shell_env_var

# You need to export variables to have them run in the shell as well.
one=this will only work locally
export two=we can run subcommands with this

expo2: 
	@echo $(one)
	@echo $$one
	@echo $(two)
	@echo $$two

# Variable Pt.2
# recursive (use =) - only looks for the variables when the command is used, not when it's defined.
# simply expanded (use :=) - like normal imperative programming -- only those defined so far get expanded
# Recursive variable. This will print "later" below
one = one ${later_variable}
# Simply expanded variable. This will not print "later" below
two := two ${later_variable}

later_variable = later

var2: 
	echo $(one)
	echo $(two)

# Simply expanded (using :=) allows you to append to a variable. Recursive definitions will give an infinite loop error.
one = hello
# one gets defined as a simply expanded variable (:=) and thus can handle appending
one := there ${one}

var21: 
	echo $(one)

# ?= only sets variables if they have not yet been set
v1 = hello
v1 ?= will not be set
v2 ?= will be set

var22: 
	echo $(v1)
	echo $(v2)

# See examples from tutorial :p
# Command line arguments and override
# List of commands and define
# Target-specific variables
# Pattern-specific variables

# Conditional if/else
foo = ok

cond_if:
ifeq ($(foo), ok)
	echo "foo equals ok"
else
	echo "nope"
endif

# First Function - String Substitution $(patsubst pattern,replacement,text)
bar := ${subst not, totally, "I am not superman"}
subst_fn: 
	@echo $(bar)

# foreach function - $(foreach var,list,text)
foo := who are you
# For each "word" in foo, output that same word with an exclamation after
bar := $(foreach wrd,$(foo),$(wrd)!)

foreach_fn:
	# Output is "who! are! you!"
	@echo $(bar)

# if function
# call function
# shell function