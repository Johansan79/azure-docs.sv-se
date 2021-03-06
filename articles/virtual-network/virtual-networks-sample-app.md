---
title: "Azure-exempelprogram för användning med DMZs | Microsoft Docs"
description: "Distribuera det här enkla webbprogrammet när du har skapat en DMZ att testa scenarier för flödet av trafik"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 60340ab7-b82b-40e0-bd87-83e41fe4519c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 8506238e41c5d9dac8d76d729d4919b30a0528b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="sample-application-for-use-with-dmzs"></a>Exempelprogram för användning med DMZs
[Gå tillbaka till gränsen bästa praxis säkerhetssidan][HOME]

Dessa PowerShell-skript kan köras lokalt på IIS01 och AppVM01 att installera och konfigurera en enkel webbapp som visar en HTML-sida från servern IIS01 med innehåll från backend-AppVM01 server.

Det här programmet innehåller en enkel testmiljö för många DMZ exempel och hur ändringar på slutpunkter, NSG: er, UDR och brandväggen kan påverka trafikflöden.

## <a name="firewall-rule-to-allow-icmp"></a>Brandväggsregel för att tillåta ICMP
Den här enkla PowerShell-instruktionen kan köras på alla Windows-VM att tillåta ICMP (Ping)-trafik. Uppdateringen brandväggen kan enklare testa och felsöka genom att tillåta ping-protokollet passera windows-brandväggen (för de flesta Linux-distributioner ICMP är aktiverat som standard).

```PowerShell
# Turn On ICMPv4
New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
    -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
```

Om du använder följande skript, är det här tillägget för brandväggen regeln den första satsen.

## <a name="iis01---web-application-installation-script"></a>IIS01 - Web application installationsskript
Det här skriptet kommer att:

1. Öppna IMCPv4 (Ping) i windows-brandväggen för lokal server för enklare testning
2. Installera IIS och .net Framework 4.5
3. Skapa en ASP.NET-webbsida och en Web.config-fil
4. Ändra standardprogrampoolen lättare åtkomst till filen
5. Ange den anonyma användaren till ditt administratörskonto och lösenord

Detta PowerShell-skript ska köras lokalt medan RDP hade till IIS01.

```PowerShell
# IIS Server Post Build Config Script
# Get Admin Account and Password
    Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
    $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
    $thePassword = Read-Host -Prompt "The Admin Password"

# Turn On ICMPv4
    Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Install IIS
    Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
    add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console

# Create Web App Pages
    Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
    $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
    <%@ Import Namespace="System.IO" %>
    <script language="vb" runat="server">
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
            Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
            Dim objStreamReader As StreamReader
            objStreamReader = File.OpenText(FILENAME)
            Dim contents As String = objStreamReader.ReadToEnd()
            lblOutput.Text = contents
            objStreamReader.Close()
            lblTime.Text = Now()
        End Sub
    </script>

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
        <title>DMZ Example App</title>
    </head>
    <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
      <form id="frmMain" runat="server">
        <div>
          <h1>Looks like you made it!</h1>
          This is a page from the inside (a web server on a private network),<br />
          and it is making its way to the outside! (If you are viewing this from the internet)<br />
          <br />
          The following sections show:
          <ul style="margin-top: 0px;">
            <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
            <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
            <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
          </ul>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Image File Linked from the Internet</b>:<br />
            <br />
            <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
        </div>
      </form>
    </body>
    </html>'

    $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.web>
        <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
        <httpRuntime targetFramework="4.5" />
        <identity impersonate="true" />
        <customErrors mode="Off"/>
      </system.web>
      <system.webServer>
        <defaultDocument>
          <files>
            <add value="Home.aspx" />
          </files>
        </defaultDocument>
      </system.webServer>
    </configuration>'

    $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
    $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii

# Set App Pool to Clasic Pipeline to remote file access will work easier
    Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
    c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
    c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost

# Make sure the IIS settings take
    Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
    Restart-Service -Name W3SVC

    Write-Host
    Write-Host "Web App Creation Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="appvm01---file-server-installation-script"></a>AppVM01 - skript för installation
Det här skriptet konfigurerar serverdelen för detta enkla program. Det här skriptet kommer att:

1. Öppna IMCPv4 (Ping) i brandväggen för enklare testning
2. Skapa en katalog för webbplatsen
3. Skapa en textfil för att via fjärranslutning åtkomst av webbsidan
4. Ange behörighet för katalogen och filen att anonym åtkomst ska tillåtas
5. Inaktivera Förbättrad säkerhetskonfiguration i Internet Explorer så att surfa lättare från den här servern 

> [!IMPORTANT]
> **Bästa praxis**: aldrig inaktivera Förbättrad säkerhetskonfiguration i Internet Explorer på en produktionsserver plus är vanligtvis en felaktig idé att surfa på Internet från en produktionsserver. Dessutom är öppnandet av filresurser för anonym åtkomst en felaktig idé, men klart här för enkelhetens skull.
> 
> 

Detta PowerShell-skript ska köras lokalt medan RDP hade till AppVM01. PowerShell krävs för att köra som administratör för att kontrollera har körts.

```PowerShell
# AppVM01 Server Post Build Config Script
# PowerShell must be run as Administrator for Net Share commands to work

# Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Create Directory
    New-Item "C:\WebShare" -ItemType Directory

# Write out Rand.txt
    $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
    $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii

# Set Permissions on share
    $Acl = Get-Acl "C:\WebShare"
    $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
    $Acl.SetAccessRule($AccessRule)
    Set-Acl "C:\WebShare" $Acl

# Create network share
    Net Share WebShare=C:\WebShare "/grant:Everyone,READ"

# Turn Off IE Enhanced Security Configuration for Admins
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0

    Write-Host
    Write-Host "File Server Set up Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="dns01---dns-server-installation-script"></a>DNS01 - installationsskript för DNS-server
Det finns inget skript som ingår i det här exempelprogrammet för att ställa in DNS-servern. Testning av brandväggsregler, NSG eller UDR måste inkludera DNS-trafik, måste DNS01-servern konfigureras manuellt. Nätverkskonfigurationen XML-filen och Resource Manager-mall för både exempel innehåller DNS01 som den primära DNS-servern och den offentliga DNS-servern nivå 3 som säkerhetskopiering DNS-server som värd. Nivå 3-DNS-servern skulle den faktiska DNS-server som används för icke-lokala trafik och med DNS01 inte installera några lokala nätverk DNS skulle uppstå.

## <a name="next-steps"></a>Nästa steg
* Kör skriptet IIS01 på en IIS-server
* Kör skript för filserver på AppVM01
* Bläddra till den offentliga IP-adress på IIS01 att verifiera din build

<!--Link References-->
[HOME]: ../best-practices-network-security.md
