# `You do not have the SUPER privilege and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)`

 - Incase you get this error when writing to docker mysql from a mysql client eg workbench in the local machine then this is the solution.
  
  NB: This answer is direct from chatgpt and it worked for me.

To clarify and provide an example for the given instruction:

**Instruction:**

- "Do this one `SET GLOBAL log_bin_trust_function_creators = 1;` before running the `docker exec -it -u root my_php_container bash` or `php artisan migrate`.

**Explanation with Example:**

- When you run Docker containers that interact with MySQL databases, you might encounter issues related to function creators not having sufficient privileges to create functions in MySQL. To resolve this, MySQL requires you to set a global variable `log_bin_trust_function_creators` to `1`, which allows non-superusers (like the MySQL user used by Docker containers) to create functions.

Here's how you can do it step by step:

1. **Access MySQL as the MySQL User:**
 You need to log in to the MySQL server as a user with sufficient privileges to execute the `SET GLOBAL` command. Typically, this user would be `root` or another user with administrative privileges.
```bash
 docker exec -it -u root my_php_container bash
```
- This command opens an interactive bash shell (`bash`) within the `my_php_container` Docker container, running as the `root` user (`-u root`).

2. **Access MySQL CLI:**
- Once inside the container, you access the MySQL command-line interface (CLI) to execute SQL commands.

```bash
 mysql -u root -p
```
You might be prompted for the MySQL root password here.

3. **Execute SQL Command:**
-  After logging in to MySQL, execute the following SQL command to set the global variable:
```sql
 SET GLOBAL log_bin_trust_function_creators = 1;
```
This command allows non-superusers to create functions.
4. **Verify and Exit:**
 Verify the variable is set correctly if needed:
```sql
 SHOW VARIABLES LIKE 'log_bin_trust_function_creators';
```
Once verified, you can exit the MySQL CLI and the container:
```sql
 exit
 exit
```
5. **Proceed with Docker Commands:**

-  Now that you've set the global variable, you can proceed with your Docker commands, such as running migrations with Laravel's `php artisan migrate`.

- By following these steps, you ensure that your Docker container has the necessary MySQL settings configured before executing commands that might require creating functions in the database.