# CiscoCDR

SSIS Package to import Cisco Unified Communication Manager (CUCM / CallManager v10.5) Call Detail Records (CDR) from  a Windows file share into Microsoft SQL Server.  This is a project I created back around 2011 with very little CUCM/SQL/SSIS knowledge at the time, so I'm sure it can be improved on a lot.

There were additional fields added from v7 => 10.5 when I upgraded... it broke the import, so I'm not sure if it's backwards compatible at this point... shouldn't be _too_ difficult to troubleshoot and remove the fields causing a problem.

Requires Visual Studio 2015 (with the proper SQL Server Data Tools and whatever else is necessary to create new SSIS / Business Intelligence projects).

CDR Billing Server needs to be setup to deliver the files via FTP/SFTP to a Windows server somewhere (this is done in CUCM's /ccmservice page... refer to Cisco's documentation for this setup).

You should be able to do a simple File => Open => File... (CTRL+O) and select the .dtsx package to get started on setting up the package.

The cdr files are copied into \\\\Windows-Server\\CiscoCDR

When the SSIS package runs, a ./working directory is created and the files are moved into that directory.

The data flow task runs against this directory.

Once complete, the files are moved into a ./deleted folder (this was done... for some reason I don't really know why or even how the cdr files are deleted... maybe the folder creation step deletes it?)

When updating the connection managers, you'll have a problem saving the ImportCDR one.  You have to have a cdr file available to select when making changes, then revert the filename back to the form with the wildcard.  Frustrating... but, yeah, create the ./working directory, copy a CDR file into there to use to select (I've made a "cdr_samplefile.txt" available on here as well to use if needed).  It'll tell you the file has been changed and ask if you want to keep the mapping / metadata... select Yes, make your changes (servername, etc...) and then change the filename back to "\\\\Windows-Server\\ciscocdr\\working\\cdr\*".

I think CUCM also copies cmr files... you might need a separate process to delete those out of there... or add it to the SSIS package.
