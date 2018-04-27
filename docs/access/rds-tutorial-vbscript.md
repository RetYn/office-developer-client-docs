---
title: "RDS Tutorial (VBScript)"
 
 
manager: soliver
ms.date: 11/16/2014
ms.audience: Developer
ms.topic: reference
  
localization_priority: Normal
ms.assetid: 7a6596fd-00b9-a637-7d00-fb55a621305f
description: "This is the RDS Tutorial, written in Microsoft Visual Basic Scripting Edition. For a description of the purpose of this tutorial, see the RDS Tutorial."
---

# RDS Tutorial (VBScript)

This is the RDS Tutorial, written in Microsoft Visual Basic Scripting Edition. For a description of the purpose of this tutorial, see the [RDS Tutorial](chapter-12-rds-tutorial.md).
  
In this tutorial, [RDS.DataControl](datacontrol-object-rds.md) and [RDS.DataSpace](dataspace-object-rds.md) are created at design time — that is, they are defined with object tags, like this: . Alternatively, they could be created at run time with the **Server.CreateObject** method. For example, the **RDS.DataControl** object could be created like this: 
  
```
Set DC = Server.CreateObject("RDS.DataControl") 
 <!-- RDS.DataControl --> 
 <OBJECT 
 ID="DC1" CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E33"> 
 </OBJECT> 
 
 <!-- RDS.DataSpace --> 
 <OBJECT 
 ID="DS1" WIDTH=1 HEIGHT=1 
 CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36"> 
 </OBJECT> 
 
 <SCRIPT LANGUAGE="VBScript"> 
 
 Sub RDSTutorial() 
 Dim DF1 

```

 **Step 1 — Specify a server program**
  
VBScript can discover the name of the IIS Web server it is running on by accessing the VBScript **Request.ServerVariables** method available to Active Server Pages: 
  
```
 
"http://<%=Request.ServerVariables("SERVER_NAME")%>" 

```

However, for this tutorial, use the imaginary server, "yourServer".
  
> [!NOTE]
> Pay attention to the data type of **ByRef** arguments. VBScript does not let you specify the variable type, so you must always pass a Variant. When using HTTP, RDS will allow you to pass a Variant to a method that expects a non-Variant if you invoke it with the **RDS.DataSpace** object [CreateObject](createobject-method-rds.md) method. When using DCOM or an in-process server, match the parameter types on the client and server sides or you will receive a "Type Mismatch" error. 
  
```
 
Set DF1 = DS1.CreateObject("RDSServer.DataFactory", "http://yourServer") 

```

 **Step 2a — Invoke the server program with RDS.DataControl**
  
This example is merely a comment demonstrating that the default behavior of the **RDS.DataControl** is to perform the specified query. 
  
```
 
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1"> 
 <PARAM NAME="SQL" VALUE="SELECT * FROM Authors"> 
 <PARAM NAME="Connect" VALUE="DSN=Pubs;"> 
 <PARAM NAME="Server" VALUE="http://yourServer/"> 
</OBJECT> 
... 
<SCRIPT LANGUAGE="VBScript"> 
 
Sub RDSTutorial2A() 
 Dim RS 
 DC1.Refresh 
 Set RS = DC1.Recordset 
... 

```

 **Step 2b — Invoke the server program with RDSServer.DataFactory**
  
 **Step 3 — Server obtains a Recordset**
  
 **Step 4 — Server returns the Recordset**
  
```
 
Set RS = DF1.Query("DSN=Pubs;", "SELECT * FROM Authors") 

```

 **Step 5 — DataControl is made usable by visual controls**
  
```
 
' Assign the returned recordset to the DataControl. 
 
DC1.SourceRecordset = RS 

```

 **Step 6a — Changes are sent to the server with RDS.DataControl**
  
This example is merely a comment demonstrating how the **RDS.DataControl** performs updates. 
  
```
 
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1"> 
 <PARAM NAME="SQL" VALUE="SELECT * FROM Authors"> 
 <PARAM NAME="Connect" VALUE="DSN=Pubs;"> 
 <PARAM NAME="Server" VALUE="http://yourServer/"> 
</OBJECT> 
... 
<SCRIPT LANGUAGE="VBScript"> 
 
Sub RDSTutorial6A() 
Dim RS 
DC1.Refresh 
... 
Set RS = DC1.Recordset 
' Edit the Recordset object... 
' The SERVER and CONNECT properties are already set from Step 2A. 
Set DC1.SourceRecordset = RS 
... 
DC1.SubmitChanges 

```

 **Step 6b — Changes are sent to the server with RDSServer.DataFactory**
  
```
 
DF.SubmitChanges"DSN=Pubs", RS 
 
End Sub 
</SCRIPT> 
</BODY> 
</HTML> 

```

 **This is the end of the tutorial.**
  
