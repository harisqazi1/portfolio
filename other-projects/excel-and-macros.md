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

```cpp
Sub Workbook_Open()
    Set Excel_Shell = CreateObject("WScript.Shell")
    Set Excel_Shell_Exec = Excel_Shell.Exec("whoami")
    MsgBox (Excel_Shell_Exec.StdOut.ReadAll)
End Sub
```

We have to copy this code under Microsoft Excel Objects in order to get it to work.

![On the right top we can see where the code will be edited](../.gitbook/assets/screenshot-2021-03-06-150627.png)

After this you can save and exit the Excel file. When you open it again, there should be a popup with the Desktop name.



