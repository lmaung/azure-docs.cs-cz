---
title: Použití Hadoop Oozie v HDInsight
description: Použití Hadoop Oozie v HDInsight, velké objemy dat služby. Zjistěte, jak definovat pracovní postup Oozie a odešlete úlohu Oozie.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/25/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 5048a4ab4db6d4cb168d2a643a412f89273658b4
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984252"
---
# <a name="use-apache-oozie-with-apache-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Použití Apache Oozie s Hadoopem Apache k definování a spuštění workflowu v HDInsight
[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Další informace o použití Apache Oozie pro definování pracovního postupu a spouštění pracovního postupu na HDInsight. Další informace o Oozie coordinator najdete v tématu [pomocí HDInsight založeného na čase Apache Oozie Coordinator][hdinsight-oozie-coordinator-time]. Další služby Azure Data Factory najdete v tématu [použití Apache Pig a Apache Hivu se službou Data Factory][azure-data-factory-pig-hive].

Apache Oozie je systém koordinace pracovních postupů /, který spravuje úlohy systému Hadoop. Je integrován do zásobníku Hadoop a podporuje úlohy systému Hadoop pro Apache MapReduce, Apache Pig, Apache Hivu a Apache Sqoop. To lze použít také k plánování úloh, které jsou specifické pro systém, jako jsou programy v jazyce Java nebo skripty prostředí.

Pracovní postup, který můžete implementovat podle pokynů uvedených v tomto kurzu obsahuje dvě akce:

![Diagram pracovního postupu][img-workflow-diagram]

1. Akce Hive spouští skript HiveQL k výpočtu výskytů jednotlivých typů úroveň protokolování v souboru Apache Log4j. Každý soubor log4j se skládá z řádku polí, která obsahuje pole [úroveň protokolu], který popisuje typ a závažnost, například:
   
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
   
    Výstup skriptu Hive je podobný:
   
        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4
   
    Další informace o Hivu najdete v tématu [použití Apache Hivu se službou HDInsight][hdinsight-use-hive].
2. Sqoop akce exportuje výstup HiveQL do tabulky v databázi Azure SQL. Další informace o Sqoop najdete v tématu [použití Apache Sqoop s HDInsight][hdinsight-use-sqoop].

> [!NOTE]  
> Podporované verze Oozie na clusterech HDInsight najdete v tématu [co je nového ve verzích clusterů systému Apache Hadoop poskytovaných službou HDInsight?] [hdinsight-versions].
> 
> 

### <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující položky:

* **Pracovní stanice s prostředím Azure PowerShell**. 
  

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
  

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Definice pracovního postupu Oozie a související skript HiveQL
Definice pracovních postupů Oozie jsou napsané v hPDL (jazyk definice procesu XML). Výchozí název souboru pracovního postupu je *workflow.xml*. Níže je soubor pracovního postupu, který používáte v tomto kurzu.
```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
```
Existují dvě akce, které jsou definovány v pracovním postupu. Je tato akce start *RunHiveScript*. Pokud tato akce proběhne úspěšně, je další akci *RunSqoopExport*.

RunHiveScript má několik proměnných. Můžete předat hodnoty, když odešlete úlohu Oozie z pracovní stanice pomocí Azure Powershellu.

|Proměnné pracovního postupu|Popis|
|---|---|
|${jobTracker}|Určuje adresu URL sledování úloh Hadoopu. Použití **jobtrackerhost:9010** v HDInsight verze 3.0 a 2.1.|
|${nameNode}|Určuje adresu URL uzlu název Hadoop. Použít výchozí adresu systému souboru, například *wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net*.|
|${queueName}|Určuje název fronty, odeslání úlohy. Použití **výchozí**.|

|Proměnné akcí Hive|Popis|
|---|---|
|${hiveDataFolder}|Určuje zdrojový adresář pro příkaz Hive Create Table.|
|${hiveOutputFolder}|Určuje výstupní složka pro příkaz INSERT PŘEPSAT.|
|${hiveTableName}|Určuje název tabulky Hive, který odkazuje log4j datových souborů.|

|Proměnné akcí Sqoop|Popis|
|---|---|
|${sqlDatabaseConnectionString}|Určuje připojovací řetězec databáze Azure SQL.|
|${sqlDatabaseTableName}|Určuje, kde data se exportují do tabulky databáze Azure SQL.|
|${hiveOutputFolder}|Určuje výstupní složka pro příkaz Hive vložit PŘEPSAT. Toto je stejné složce, Sqoop export (export-dir).|

Další informace o pracovním postupu Oozie a pomocí akce pracovního postupu najdete v tématu [dokumentaci Apache Oozie 4.0][apache-oozie-400] (pro HDInsight verze 3.0) nebo [dokumentaci Apache Oozie 3.3.2][apache-oozie-332] (pro HDInsight verze 2.1).

Akce Hive v pracovním postupu volá skript HiveQL. Tento soubor skriptu obsahuje tři příkazy HiveQL:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **Příkaz DROP TABLE** odstraní tabulku Hive log4j, pokud existuje.
2. **Příkaz CREATE TABLE** vytvoří log4j externí tabulku Hive, která odkazuje na umístění souboru protokolu log4j. Oddělovač polí je ",". Oddělovač řádků výchozí je "\n". Externí tabulka Hive slouží k vyhnout datový soubor odebírán z původního umístění, pokud chcete spustit pracovní postup Oozie více než jednou.
3. **Příkaz INSERT PŘEPSAT** počty výskytů jednotlivých typů úroveň protokolu z tabulky Hive log4j a ukládání výstupu do objektu blob ve službě Azure Storage.

Existují tři proměnných použitých ve skriptu:

* ${hiveTableName}
* ${hiveDataFolder}
* ${hiveOutputFolder}

Soubor definice pracovního postupu (workflow.xml v tomto kurzu) tyto hodnoty předá do tento skript HiveQL v době běhu.

Soubor pracovního postupu a soubor HiveQL ukládat do kontejneru objektů blob.  Skript Powershellu, které použijete později v tomto kurzu kopíruje oba soubory do výchozího účtu úložiště. 

## <a name="submit-oozie-jobs-using-powershell"></a>Odesílání úloh Oozie pomocí Powershellu
Prostředí Azure PowerShell aktuálně neposkytuje žádné rutiny pro definování Oozie úloh. Můžete použít **Invoke-RestMethod** rutiny invoke Oozie webové služby. Rozhraní API webových služeb Oozie je JSON rozhraní HTTP REST API. Další informace o rozhraní API Oozie webových služeb najdete v tématu [dokumentaci Apache Oozie 4.0][apache-oozie-400] (pro HDInsight verze 3.0) nebo [dokumentaci Apache Oozie 3.3.2][apache-oozie-332] (pro HDInsight verze 2.1).

Skript Powershellu v této části provede následující kroky:

1. Připojení k Azure.
2. Vytvořte skupinu prostředků Azure. Další informace najdete v tématu [pomocí Azure Powershellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).
3. Vytvoření serveru Azure SQL Database, Azure SQL database a dvě tabulky. Ty se používají Sqoop akcí v pracovním postupu.
   
    Název tabulky je *log4jLogCount*.
4. Vytvoření clusteru HDInsight používá ke spouštění úloh Oozie.
   
    Prozkoumat clusteru, můžete na webu Azure portal nebo Azure Powershellu.
5. Zkopírujte soubor pracovního postupu oozie a skript HiveQL do výchozího systému souborů.
   
    Oba soubory jsou uloženy ve veřejném kontejneru objektů Blob.
   
   * Zkopírujte skript HiveQL (useoozie.hql) do služby Azure Storage (wasb:///tutorials/useoozie/useoozie.hql).
   * Zkopírujte workflow.xml wasb:///tutorials/useoozie/workflow.xml.
   * Zkopírujte datový soubor (/ example/data/sample.log) k wasb:///tutorials/useoozie/data/sample.log.
6. Odešlete úlohu Oozie.
   
    Prozkoumat výsledky úlohy OOzie, použijte Visual Studio nebo jiné nástroje pro připojení ke službě Azure SQL Database.

Tady je skript.  Skript můžete spustit z Windows PowerShell ISE. Potřebujete nakonfigurovat prvních 7 proměnné.
```powershell
    #region - provide the following values

    $subscriptionID = "<Enter your Azure subscription ID>"

    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"

    # HDInsight cluster HTTP user credential used for creating and connecting
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"

    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "https://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion

    #region - Create Azure resource group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion

    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green

    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()

    $conn.close()
    #endregion

    #region - Create HDInsight cluster

    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green

    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - copy Oozie workflow and HiveQL files

    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green

    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous

    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force

    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml

    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql

    #endregion

    #region - copy the sample.log file

    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green

    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 

    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log

    #endregion

    #region - submit Oozie job

    $storageUri="wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"

    $oozieJobName = $namePrefix + "OozieJob"

    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>

    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>

    <property>
        <name>queueName</name>
        <value>default</value>
    </property>

    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>

    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>

    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>

    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>

    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>

    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>

    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>

    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>

    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>

    </configuration>
    "@

    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerStatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerStatus."

    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."

    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug

    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")

    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }

    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green

    #endregion
```

**Znovu spustit kurz**

Chcete-li znovu spustit pracovní postup, musíte odstranit následující položky:

* Výstupní soubor skriptu Hive
* Data v tabulce log4jLogsCount

Tady je ukázkový skript Powershellu, které můžete použít:
```powershell
    $resourceGroupName = "<AzureResourceGroupName>"

    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()
```

## <a name="next-steps"></a>Další postup
V tomto kurzu jste zjistili, jak definovat pracovní postup Apache Oozie a jak se spouští úloha Oozie s použitím prostředí PowerShell. Další informace naleznete v následujících článcích:

* [Pomocí HDInsight založeného na čase Apache Oozie Coordinator][hdinsight-oozie-coordinator-time]
* [Začněte používat Apache Hadoop s Apache Hive v HDInsight k analýze pomocí mobilního telefonu][hdinsight-get-started]
* [Použití Azure Blob storage s HDInsight][hdinsight-storage]
* [Správa HDInsight pomocí Powershellu][hdinsight-admin-powershell]
* [Nahrání dat pro úlohy Apache Hadoop v HDInsight][hdinsight-upload-data]
* [Použití Apache Sqoop s Apache Hadoop v HDInsight][hdinsight-use-sqoop]
* [Použití Apache Hivu s Apache Hadoop v HDInsight][hdinsight-use-hive]
* [Použití Apache Pig s Apache Hadoop v HDInsight][hdinsight-use-pig]
* [Vývoj programů Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: https://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/transform-data.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: https://hadoop.apache.org/
[apache-oozie-400]: https://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: https://oozie.apache.org/docs/3.3.2/

[powershell-download]: https://azure.microsoft.com/downloads/
[powershell-about-profiles]: https://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
