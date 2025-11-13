# Exercise 2: Pinging a Domain

In this exercise, you’ll write a bash script that accepts two arguments: a 
name (for example, `mysite`) and a target domain (for example, `nostarch.com`). 
The script should be able to do the following:

1.  Throw an error if the arguments are missing and exit using the right 
exit code.
2.  Ping the domain and return an indication of whether the ping was suc
cessful. To learn about the `ping` command, run `man ping`.
3.  Write the results to a CSV file containing the following information:
    a.  The name provided to the script
    b.  The target domain provided to the script
    c.  The ping result (either `success` or `failure`)
    d.  The current date and time

As with most tasks in bash, there are multiple ways to achieve this goal. 
You can find an example solution to this exercise, `exercise_solution.sh`, in the 
book’s GitHub repository.