---
title: "Profilen live-webbappar på Azure med Application Insights Profiler | Microsoft Docs"
description: "Identifiera hot sökvägen i koden web server med en låg storleken profiler."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mbullwin
ms.openlocfilehash: e66dc2af18785c6c8e83815129c8bca5b877d25b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/03/2017
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Profilen live Azure-webbappar med Application Insights

*Den här funktionen av Application Insights är allmänt tillgänglig för Azure App Service och är i förhandsvisning för Azure-beräkningsresurser.*

Ta reda på hur mycket tid läggs på varje metod i webbprogrammet live med hjälp av [Application Insights Profiler](app-insights-overview.md). Application Insights profilering verktyget visas detaljerad profiler för live-frågor som har hanteras av din app och markeras den *varm sökvägen* som används mest tid. Profileraren väljer automatiskt exempel som har olika svarstider och använder sedan olika metoder för att minimera kostnader.

Profileraren fungerar för närvarande för ASP.NET-webbprogram som körs på Azure App Service i minst grundläggande tjänstnivån.

## <a id="installation"></a>Aktivera profileraren

[Installera Application Insights](app-insights-asp-net.md) i koden. Om den redan är installerad, kontrollera att du har den senaste versionen. Om du vill söka efter den senaste versionen i Solution Explorer högerklickar du på projektet och välj sedan **hantera NuGet-paket** > **uppdateringar** > **uppdatera alla paket**. Sedan omdistribuera din app.

*Med hjälp av ASP.NET Core? Hämta [mer](#aspnetcore).*

I [Azure-portalen](https://portal.azure.com), öppna Application Insights-resurs för webbappen. Välj **prestanda** > **aktivera Application Insights Profiler**.

![Välj Aktivera profileraren banderoll][enable-profiler-banner]

Du kan också välja **konfigurera** att visa status och aktivera eller inaktivera profileraren.

![Välj Konfigurera under prestanda][performance-blade]

Webbprogram som är konfigurerade med Application Insights visas under **konfigurera**. Följ instruktionerna för att installera agenten profiler om det behövs. Om inget webbprogram har konfigurerats med Application Insights, Välj **Lägg till länkade appar**.

Kontrollera profiler på alla dina länkade webbprogram, i den **konfigurera** väljer **aktivera profileraren** eller **inaktivera profileraren**.

![Konfigurera alternativ för][linked app services]

Till skillnad från webbprogram som är värd via programtjänstplaner hanteras program som finns i Azure-beräkningsresurser (till exempel Azure virtuella datorer, virtuella datorer, Azure Service Fabric eller Azure Cloud Services) direkt inte av Azure. I det här fallet finns inga webbprogram för att länka till. I stället för att länka till en app, Välj den **aktivera profileraren** knappen.

## <a name="disable-the-profiler"></a>Inaktivera profileraren
Stoppa eller starta om profiler för en enskild App Service-instans under **Webjobs**, gå till App Service-resurs. Om du vill ta bort profileraren, gå till **tillägg**.

![Inaktivera profiler för en webb-jobb][disable-profiler-webjob]

Vi rekommenderar att du har den profilerare som är aktiverad på alla dina webbprogram för att identifiera eventuella prestandaproblem så snart som möjligt.

Om du använder WebDeploy för att distribuera ändringarna till ditt webbprogram, kan du kontrollera att du utesluter mappen App_Data tas bort under distributionen. Annars tas tillägget profileraren filer bort nästa gång du distribuera webbappen till Azure.

### <a name="using-profiler-with-azure-vms-and-azure-compute-resources-preview"></a>Med hjälp av profiler med virtuella Azure-datorer och Azure-beräkningsresurser (förhandsgranskning)

När du [aktivera Application Insights för Azure App Service vid körning](app-insights-azure-web-apps.md#run-time-instrumentation-with-application-insights), Application Insights Profiler är automatiskt tillgänglig. Om du redan har aktiverat Programinsikter för resursen kan du behöva uppdatera till den senaste versionen med hjälp av guiden Konfigurera.

Hämta information en [förhandsversionen av profileraren för Azure-beräkningsresurser](https://go.microsoft.com/fwlink/?linkid=848155).


## <a name="limitations"></a>Begränsningar

Standard-datalagring är fem dagar. Den maximala data som inhämtas per dag är 10 GB.

Det finns inga avgifter för användning av tjänsten profiler. Om du vill använda tjänsten profileraren ditt webbprogram måste vara finns i den grundläggande nivån av App Service.

## <a name="overhead-and-sampling-algorithm"></a>Kostnader och provtagning algoritm

Profileraren körs slumpmässigt två minuter varje timme på varje virtuell dator som är värd för det program som har aktiverat för att samla in spårningar profiler. När profileraren körs läggs mellan 5 och 15% CPU-belastning på servern.
Flera servrar som är tillgänglig som värd för programmet, mindre påverkan profileraren har på den övergripande prestandan. Det beror på att algoritmen provtagning resulterar i profiler som körs på 5% av servrarna när som helst. Flera servrar är tillgänglig för att hantera webbegäranden till förskjutning serverbelastningen orsakas genom att köra profileraren.


## <a name="view-profiler-data"></a>Visa profiler data

Gå till den **prestanda** rutan och bläddrar sedan nedåt i listan med operationer.

![Application Insights prestanda rutan exempel kolumn][performance-blade-examples]

Dessa kolumner har tabellen åtgärder:

* **ANTAL**: antal dessa begäranden i tidsintervallet på den **antal** fönstret.
* **MEDIANVÄRDET**: en normal tid din app för att svara på en begäran. Hälften av alla svar var snabbare än detta värde.
* **95: e PERCENTILEN**: 95% av svar var snabbare än detta värde. Om det här värdet är avsevärt skiljer sig från medianvärdet, kan det finnas ett tillfälligt problem med din app. (Eller det kan förklaras av en funktion som cachelagring design.)
* **PROFILER-SPÅRNINGAR**: en ikon som indikerar att profileraren har hämtats stackspår för den här åtgärden.

Välj **visa** öppna trace explorer. Utforskaren innehåller flera exempel att profileraren har hämtats, klassificeras efter svarstid.

Om du använder den **Preview prestanda** rutan, gå till den **vidta åtgärder** avsnitt på sidan för att visa profiler-spårningar. Välj den **Profiler-spårningar** knappen.

![Application Insights prestanda rutan Förhandsgranska profiler-spårningar][performance-blade-v2-examples]

Välj ett exempel för att visa en kod på objektnivå uppdelning av tid köra begäran.

![Application Insights trace explorer][trace-explorer]

Spåra explorer visar följande information:

* **Visa varm sökvägen**: öppnas den största löv nod eller Stäng minst något. I de flesta fall är den här noden gränsar till en prestandabegränsning.
* **Etiketten**: namnet på den funktion eller en händelse. Trädet innehåller en blandning av koden och händelser som inträffade (till exempel SQL- och HTTP-händelser). Händelsen översta representerar varaktighet för begäran.
* **Förfluten tid**: tidsintervallet mellan starten av åtgärden och i slutet av åtgärden.
* **När**: när funktionen eller händelse kördes i relation till andra funktioner.

## <a name="how-to-read-performance-data"></a>Läsa prestandadata

Microsoft service profiler använder en kombination av provtagningsmetoder och instrumentation för att analysera prestandan för ditt program. När detaljerad samling pågår prover service profiler instruktionspekaren för den datorn CPU i varje millisekunder. Varje prov avbildar fullständig anropsstacken för tråden som för närvarande körs. Den ger detaljerad och användbar information om vad tråden utfördes, både på en hög nivå och på låg nivå av abstraktion. Tjänsten profileraren samlar även in andra händelser för att spåra aktivitet korrelation och orsakssamband, inklusive vilket sammanhang växla händelser, aktivitet parallella bibliotek (TPL) händelser och tråd pool händelser.

Anropsstacken som visas i tidslinjevyn är resultatet av provtagning och instrumentation. Eftersom varje prov fångar fullständig anropsstacken trådens, inkluderar den kod från Microsoft .NET Framework och från andra ramverk som du refererar till.

### <a id="jitnewobj"></a>Objektet allokering clr! JIT\_nytt eller clr! JIT\_Newarr1)
**CLR! JIT\_ny** och **clr! JIT\_Newarr1** helper-funktioner i .NET Framework som du allokera minne från en hanterad heap. **CLR! JIT\_ny** anropas när ett objekt har allokerats. **CLR! JIT\_Newarr1** anropas när en objektmatris allokeras. Dessa två funktioner normalt är mycket snabbt och ta relativt små mängder tid. Om du ser **clr! JIT\_ny** eller **clr! JIT\_Newarr1** ta en lång tid i tidslinjen, det är en indikation på att koden kan fördela många objekt och förbrukar stora mängder minne.

### <a id="theprestub"></a>Inläsning av kod clr! ThePreStub)
**CLR! ThePreStub** är en hjälpfunktion i .NET Framework som förbereder koden ska köras för första gången. Detta vanligtvis inkluderar, men är inte begränsat till just-in-time (JIT) kompilering. För varje C#-metoden **clr! ThePreStub** ska anropas en gång under livslängden för en process.

Om **clr! ThePreStub** tar lång tid en begäran detta anger att begäran är den första som kör den här metoden. Tiden för körningen av .NET Framework att läsa in den här metoden är viktig. Du kan använda en upp process som körs som del av koden innan användarna åtkomst till den eller Överväg att köra Native Image Generator (ngen.exe) på din sammansättningar.

### <a id="lockcontention"></a>Låskonkurrens clr! JITutil\_MonContention eller clr! JITutil\_MonEnterWorker)
**CLR! JITutil\_MonContention** eller **clr! JITutil\_MonEnterWorker** anger att den aktuella tråden väntar på en låsas upp. Detta normalt visas när du kör en C# **Lås** -instruktion, när du anropar den **Monitor.Enter** metod, eller när du startar en metod med det **MethodImplOptions.Synchronized**attribut. Låskonkurrens uppstår vanligen när tråden A hämtar ett lås och tråd B försöker hämta samma låset innan tråd A släpper den.

### <a id="ngencold"></a>Inläsning av kod ([KALLA])
Om metodnamnet innehåller **[COLD]**, som **mscorlib.ni! [ COLD]system.Reflection.CustomAttribute.IsDefined**, .NET Framework-körningen kör kod för första gången som inte är optimerad av <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profil interaktiv optimering</a>. För varje metod ska det visas högst en gång under livslängden för processen.

Om du läser in koden tar lång tid en begäran, anger att begäran är den första att köra den icke-optimerad delen av metoden. Överväg att använda en upp process som körs som del av koden innan användarna åtkomst till den.

### <a id="httpclientsend"></a>Skicka HTTP-begäran
Metoder som **HttpClient.Send** indikerar att koden väntar på en HTTP-begäran ska slutföras.

### <a id="sqlcommand"></a>Databasåtgärden
Metoder som **SqlCommand.Execute** indikerar att koden väntar en åtgärd ska slutföras.

### <a id="await"></a>Väntar på (AWAIT\_tid)
**AWAIT\_tid** anger att koden väntar på en annan aktivitet ska slutföras. Det händer vanligtvis med C# **AWAIT** instruktionen. När koden har en C# **AWAIT**tråden unwinds och returnerar kontrollen till trådpoolen och det finns ingen tråd blockeras väntar på att den **AWAIT** ska slutföras. Dock logiskt tråden som gjorde den **AWAIT** ”blockeras”, och väntar på att åtgärden ska slutföras. Den **AWAIT\_tid** instruktionen anger blockerade väntar på att slutföra uppgiften.

### <a id="block"></a>Blockerade tid
**BLOCKED_TIME** anger att koden väntar på en annan resurs ska vara tillgängliga. Det kan till exempel Vänta ett synkroniseringsobjekt, en tråd ska vara tillgängliga eller en begäran om att avsluta.

### <a id="cpu"></a>CPU-tid
Processorn är upptagen med att köra instruktionerna.

### <a id="disk"></a>Disktid i procent
Programmet utför diskåtgärder.

### <a id="network"></a>Tid för nätverk
Programmet utför nätverksåtgärder.

### <a id="when"></a>När kolumnen
Den **när** kolumnen är en visualisering av hur INKLUSIVA exemplen som samlas in för en nod variera över tid. Det totala intervallet för begäran är uppdelat i 32 tid buckets. Sammanlagt exemplen för noden ackumuleras i dessa 32 buckets. Varje bucket representeras som en stapel. Höjden på fältet representerar ett skalade värde. För markerade noder **CPU_TIME** eller **BLOCKED_TIME**, eller om det finns en tydlig relationen mellan förbrukar en resurs (CPU, disk, tråd), i fältet representerar förbrukar en av resurserna för tidsperioden den tid på att bucket. För de här måtten är det möjligt att hämta ett värde som är större än 100% genom att använda flera resurser. Till exempel om du använder i genomsnitt två processorer under ett intervall måste hämta du 200%.


## <a id="troubleshooting"></a>Felsökning

### <a name="too-many-active-profiling-sessions"></a>För många aktiva profilering sessioner

För närvarande kan du aktivera profiler på högst fyra Azure-webbappar och distributionsplatser som körs i samma service-plan. Om profiler web jobbet rapporterar för många aktiva profilering sessioner, flytta vissa webbprogram till en annan serviceplan.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Hur tar jag reda på om Application Insights Profiler körs?

Profileraren körs som en kontinuerlig web-jobb i webbprogrammet. Du kan öppna appen webbresurs i [Azure-portalen](https://portal.azure.com). I den **WebJobs** rutan kontrollerar du statusen för **ApplicationInsightsProfiler**. Om den inte körs, öppna **loggar** för mer information.

### <a name="why-cant-i-find-any-stack-examples-even-though-the-profiler-is-running"></a>Varför hittar inte någon stack-exempel, trots att profileraren körs?

Här följer några saker som du kan kontrollera:

* Se till att web app service-plan är grundläggande nivån eller senare.
* Kontrollera att ditt webbprogram har Application Insights SDK 2.2 Beta eller senare aktiverad.
* Kontrollera att ditt webbprogram har den **APPINSIGHTS_INSTRUMENTATIONKEY** inställningar som konfigureras med samma instrumentation nyckel som används av Application Insights SDK.
* Kontrollera att ditt webbprogram körs på .NET Framework 4.6.
* Kontrollera om ditt webbprogram är ett program för ASP.NET Core [de nödvändiga beroendena](#aspnetcore).

Finns en kort uppvärmningsperiod då profileraren aktivt samlar in spårningar för flera prestanda när profileraren har startats. Samlar in prestanda spårningar i två minuter i varje timme efter att profileraren.  

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Jag använde Azure Service Profiler. Vad hände med den?  

När du aktiverar Application Insights profileraren har Azure Service Profiler-agenten inaktiverats.

### <a id="double-counting"></a>Dubbel inventering i parallella trådar

I vissa fall är total tidsmått i visningsprogrammet för stacken större än varaktigheten för begäran.

Detta kan inträffa när det finns två eller flera trådar som är associerade med en begäran och de fungerar parallellt. I så fall är den totala tråd tid större än tid som gått. En tråd kan vänta på den andra ska slutföras. Visningsprogrammet försöker identifiera detta och utesluter ointressanta vänta, men den överför kanten av visar för mycket i stället för att utelämna vad kan vara viktig information.  

När du ser parallella trådar i dina spår avgör vilka trådar som väntar på och identifiera kritiska för begäran. I de flesta fall är helt enkelt tråden som snabbt försätts i tillståndet vänta väntar på andra trådar. Koncentrera dig på de andra trådarna och ignorera tiden i väntande trådar.

### <a id="issue-loading-trace-in-viewer"></a>Inga profildata

Här följer några saker som du kan kontrollera:

* Om de data som du försöker visa är äldre än några veckor, begränsa tiden filtret och försök igen.
* Kontrollera att proxyservrar eller en brandvägg har inte blockerat åtkomsten till https://gateway.azureserviceprofiler.net.
* Kontrollera att du använder i appen Application Insights instrumentation nyckeln är samma som Application Insights-resurs som du använde när du aktiverade profilering. Nyckeln är vanligtvis i ApplicationInsights.config, men kan även finns i filen web.config eller app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Felrapport i visningsprogrammet för profilering

Skicka ett supportärende i portalen. Se till att inkludera Korrelations-ID i felmeddelandet.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Distributionsfel: katalogen är inte tom ”D:\\hem\\plats\\wwwroot\\App_Data\\jobben

Om du omdistribuera ditt webbprogram till en resurs i App Service med profiler som är aktiverad, kan du se ett meddelande som ser ut som följande:

Katalogen är inte tom ”D:\\hem\\plats\\wwwroot\\App_Data\\jobben

Det här felet uppstår om du kör webbdistribution från skript eller Visual Studio Team Services distribution Pipeline. Lösningen är att lägga till följande ytterligare parametrar Web Deploy-uppgiften:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Dessa parametrar bort mappen som används av Application Insights Profiler och avblockera Omdistributionen processen. De påverkar inte profiler-instans som körs för tillfället.


## <a name="manual-installation"></a>Manuell installation

När du konfigurerar profileraren görs uppdateringar till webbappens inställningar. Du kan tillämpa uppdateringarna manuellt om din miljö kräver. Till exempel om programmet körs i Apptjänst-miljö för PowerApps.

1. I fönstret web app kontroll öppna **inställningar**.
2. Ange **.Net Framework-version** till **v4.6**.
3. Ange **alltid på** till **på**.
4. Lägg till den __APPINSIGHTS_INSTRUMENTATIONKEY__ app inställningen och värdet till samma instrumentation nyckel som används av SDK.
5. Öppna **avancerade verktyg**.
6. Välj **Gå** att öppna Kudu-webbplatsen.
7. Välj på webbplatsen Kudu **plats tillägg**.
8. Installera __Programinsikter__ från galleriet Azure Web Apps.
9. Starta om webbprogrammet.

## <a id="aspnetcore"></a>ASP.NET Core-stöd

En ASP.NET Core-programmet måste installera Microsoft.ApplicationInsights.AspNetCore NuGet-paketet 2.1.0-beta6 eller senare för att arbeta med profileraren. Från och med den 27 juni 2017 stöder vi inte tidigare versioner.

## <a name="next-steps"></a>Nästa steg

* [Arbeta med Application Insights i Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png
