# Lab: Remote code execution via web shell upload (Easy)

It contains a vulnerable image upload function. It doesn't perform any validation on the files users upload before storing them on the server's filesystem.
To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file ‘/home/carlos/secret’ . Submit this secret using the button provided in the lab banner.
You can log in to your own account using the following credentials: ‘wiener:peter’ .

Process :

Step-1 : Open My account and login using given credentials.

Step-2 :This type of vulnerability needs a MIME file type.
The abbreviation MIME stands for Multi-purpose Internet Mail Extensions. MIME types form a standard way of classifying file types on the internet.

-->	Let’s see some other common MIME types:
multipart/form-data
text/xml
text/csv
text/plain
application/xml
application/zip
application/pdf

Step-3 : Upload an Avatar then it appear like this..
 
Step-4 : Click on My Account. Now we can see the image.

Step-5 : Now open the image location and make file using a sample code like –

    ```url
    “<?php echo file_get_contents('/home/carols/secret'); ?>”
    ``` 
Upload the file in the link.

Step-6 : When you paste the image in new tab You can see the answer.
 
In the labs, exploiting remote code execution via web shell upload allows attackers to upload and execute a web shell on the server. This enables unauthorized access, giving them control to execute commands, manipulate files, and potentially compromise the entire system, demonstrating the severity of file upload vulnerabilities in real-world scenarios.


# Lab: Web shell upload via Content-Type restriction bypass (Easy)

This lab contains a vulnerable image upload function. It attempts to prevent users from uploading unexpected file types but relies on checking user-controllable input to verify this.

Process :

Step-1 : Login to the account using the given credentials and upload any file into the given section.

Setp-2 : Open the Burp Suite and capture the data.

Step-3 : Send the data to the repeater. 

Step-4 : Open the chrome upload the image.png then it displays as error.
 
Step-5 : To not to get the error we should make the php file to image file.

Step-6 : Open the data in the repeater and change the 

    ```php
    ‘Content-Type: application/x-php’ section to ‘Content-Type: image/jpeg’
    ``` 
then send the request.

Step-7 : We can observe the response without any error.
 
Step-8 : Open the URL in the browser then click on ‘back to my account’. There you can observe a corrupted image. Open that image in the new tab. Finaly we can complete the lab.

This lab can make us understand the below content…
In PortSwigger labs, bypassing Content-Type restrictions during web shell upload enables the upload of malicious scripts disguised as permitted file types (e.g., images). This breach allows attackers to execute commands, gain unauthorized access, and potentially control the server, showcasing the exploitation of a vulnerability to circumvent upload restrictions and execute arbitrary code.


# Web shell upload via path traversal (Medium)

This lab contains a vulnerable image upload function. The server is configured to prevent execution of user-supplied files, but this restriction can be bypassed by exploiting a secondary vulnerability.

Process :

Step-1 : Login to the account and upload an image.

Step-2 : It shows no error now, come back to the account.

Step-3 : When you upload the file there you can see the text in the file this says an error.

Step-4 : So, upload the file in the burp browser then intercept the data. 

Step-5 : Send the data to the repeater. Change the file name 
    ‘virus.php’ to ‘../virus.php’. then send the request.
 
Step-6 : Open the response in the Browser and change the URL where it shows ‘/files/image.png’ change that to ‘/files/virus.php’.
 

# Web shell upload via extension blacklist bypass (Medium)

This lab contains a vulnerable image upload function. Certain file extensions are blacklisted, but this defence can be bypassed due to a fundamental flaw in the configuration of this blacklist.

Process :

Step-1 : Login to the account using the given credentials and upload an image into the given section.

Step-2 : Now, Upload the file which is named as virus.php,it says sorry php files are not allowed.
 
Step-3 : Open the URL in the Burp Browser and turn on the intercept.

Step-4 : send the data to the repeater open an extra repeater page.

Step-5 : change filename to “.htaccess” the add the content which is AddType  “AddType Application/x-httpd-php .hmcyberacademy”.

Step-6 : Make ContentType to text/plain. Send the request.
 
Step-7 : Edit the extra page with same data but change the filename to “virus.hmcyberacademy” and ContentType to “Application/x-httpd-php”. Send the request.
 
Step-8 : Open the URL in the browser click on the back to account there we can observe a broken image.

Step-9 : Open the broken image in the new tab We can see the answer.
 
From this lab we can learn…
leveraging path traversal during web shell upload allows attackers to navigate outside intended directories and place web shells in restricted areas. This exploit grants unauthorized access, enabling the execution of commands, manipulation of files, and potential compromise of the server by bypassing file upload restrictions via directory traversal techniques.





