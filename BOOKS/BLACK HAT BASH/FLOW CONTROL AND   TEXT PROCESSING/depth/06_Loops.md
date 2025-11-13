# Loops and Loop Controls

Like many programming languages, bash lets you repeat chunks of code 
by using loops. Loops can be particularly useful in your penetration
testing adventures because they can help you accomplish tasks such as 
the following:

*   Continuously checking whether an IP address is online after a reboot 
until the IP address is responsive
*   Iterating through a list of hostnames (for example, to run a specific 
exploit against each of them or determine whether a firewall is protect
ing them)
*   Testing for a certain condition and then running a loop when it is met 
(for example, checking whether a host is online and, if so, performing a 
brute-force attack against it)

The following sections introduce you to the three kinds of loops in 
bash (`while`, `until`, and `for`) as well as the `break` and `continue` statements for 
working with loops.