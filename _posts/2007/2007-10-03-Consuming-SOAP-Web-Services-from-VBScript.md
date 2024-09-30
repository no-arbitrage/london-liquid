---
title: "Consuming SOAP Web Services from VBScript"
date: "2007-10-03T17:21:17"
tags: [
  "scripting",
  "web"
]
---
I spent a day this past week visiting one of our Technology Partners in New Jersey. The objective was do put together a proof concept for some integration we are planning.

We have some very cool stuff, about to be announced at one of the big shows coming up and the integration is around this new stuff. Anyway one of the things we had to do was to connect to a Web Service that returned complex types from a dynamic language.

I chose VBScript as the language because it’s widely available and can be executed on virtually any windows machine. I also wanted to ensure we only used commonly available (installed) ActiveX objects. The other constraint was the Web Services only used SOAP and HTTP POSTs, no HTTP GET support.

So the work involved was to :

1.  Create an XMLHTTP object
2.  Create the SOAP Body
3.  Send it to the Web Service
4.  Parse the results

Below is how we went about it.  
It is messy and pretty horrible text manipulation to generate the SOAP bodies etc, but it is the only way I found from scripting..

Let me know if you have better / easier methods…

Const URL = "http://www.domain.com/folder/service.asmx"
Const nsUrl = "http://www.domain.com/namespace-name"

' Create the http request text
Dim strSoapReq
strSoapReq = GenerateSoapBodyStart()
strSoapReq = strSoapReq + GenerateSoapFunctionCallStart("WebServiceFunctionName")
strSoapReq = strSoapReq + GenerateSoapParameter("paramName1", paramValue1)
strSoapReq = strSoapReq + GenerateSoapParameter("paramName2", paramValue2)
strSoapReq = strSoapReq + GenerateSoapFunctionCallEnd("WebServiceFunctionName")
strSoapReq = strSoapReq + GenerateSoapBodyEnd()

Dim oHttp
Dim strResult
Set oHttp = CreateObject("Msxml2.XMLHTTP")
oHttp.open "POST", URL, false
oHttp.setRequestHeader "Content-Type", "text/xml"
oHttp.setRequestHeader "SOAPAction", URL + "WebServiceFunctionName"
oHttp.send strSoapReq
strResult = oHttp.responseText

' parse XML in strResult

Function GetResult(byval responseText, byval resultParam)

    Dim oXml
    Set oXml = CreateObject("Msxml2.DOMDocument")
    oXml.Async = true
    oXml.LoadXml responseText

    Dim strPath
    strPath = "/\*/\*/\*/" + resultParam
    Dim oNode
    Set oNode = oXml.documentElement.SelectSingleNode(strPath)
    GetResult = oNode.Text

End Function

Function GenerateSoapBodyStart()

    Dim strSoap
    strSoap = "<?xml version=""1.0"" encoding=""utf-8""?>"
    strSoap = strSoap + "<soap12:Envelope "
    strSoap = strSoap + "xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" "
    strSoap = strSoap + "xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" "
    strSoap = strSoap + "xmlns:soap12=""http://www.w3.org/2003/05/soap-envelope""> "
    strSoap = strSoap + "<soap12:Body>"
    GenerateSoapBodyStart = strSoap

End Function

Function GenerateSoapBodyEnd()

    Dim strSoap
    strSoap = "</soap12:Body>"
    strSoap = strSoap + "</soap12:Envelope>"
    GenerateSoapBodyEnd = strSoap

End Function

Function GenerateSoapFunctionCallStart(byval strFunction)

    Dim strSoap
    strSoap = "<" + strFunction + " xmlns=""" + nsUrl + """>"
    GenerateSoapFunctionCallStart = strSoap

End Function

Function GenerateSoapFunctionCallEnd(byval strFunction)

    Dim strSoap
    strSoap = "</" + strFunction + ">"
    GenerateSoapFunctionCallEnd = strSoap

End Function

Function GenerateSoapParameter(byval strParam, byval strValue)

    Dim strSoap
    strSoap = "<" + strParam + ">"
    strSoap = strSoap + strValue
    strSoap = strSoap + "</" + strParam + ">"
    GenerateSoapParameter = strSoap

End Function

[WS-Sample.vbs (2.42 KB)](https://kapie.com/content/binary/WS-Sample.vbs)

Fair Lawn, NJ – 40.9381556054652:\-74.132137298584