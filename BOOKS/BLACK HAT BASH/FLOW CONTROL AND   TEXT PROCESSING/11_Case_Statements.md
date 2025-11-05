# case Statements

In bash, `case` statements allow you to test multiple conditions in a cleaner 
way by using more readable syntax. Often, they help you avoid many `if` con
ditions, which can become harder to read as they grow in size.

Listing 2-24 shows the `case` statement syntax.

```bash
case EXPRESSION in
PATTERN1)
   # Do something if the first condition is met.
 ;;
PATTERN2)
   # Do something if the second condition is met.
 ;;
esac
```
*Listing 2-24: A case statement*

A `case` statement starts with the keyword `case` followed by an expression, 
such as a variable you want to match a pattern against. `PATTERN1` and `PATTERN2` 
in this example represent a pattern case (such as a regular expression, a 
string, or an integer) that you want to compare to the expression. To close a 
`case` statement, you use the keyword `esac` (`case` inverted).

Let’s take a look at an example `case` statement that checks whether an 
IP address is present in a specific private network (Listing 2-25).

*case_ip_address_check.sh*
```bash
#!/bin/bash
IP_ADDRESS="${1}"
case ${IP_ADDRESS} in
 192.168.*)
   echo "Network is 192.168.x.x"
 ;;
 10.0.*)
   echo "Network is 10.0.x.x"
 ;;
 *)
   echo "Could not identify the network"
 ;;
esac
```
*Listing 2-25: Checking an IP address and determining its network*

We define a variable that expects one command line argument to be 
passed (`${1}`) and saves it to the `IP_ADDRESS` variable. We then use a pattern to 
check whether the `IP_ADDRESS` variable starts with `192.168.` and a second pat
tern to check whether it starts with `10.0.`

We also define a default wildcard pattern using `*`, which returns a 
default message to the user if nothing else has matched.

Save this file as `case_ip_address_check.sh` and run it:

```
$ chmod u+x case_ip_address_check.sh
$ ./case_ip_address_check.sh 192.168.12.55
Network is 192.168.x.x
$ ./case_ip_address_check.sh 212.199.2.2
Could not identify the network
```

A `case` statement can be used for a variety of use cases. For example, it 
can be used to run functions based on input the user has entered. Using 
`case` statements is a great way to handle the evaluation of multiple condi
tions without sacrificing the readability of the code.