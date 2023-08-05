# Moodle on Windows with IIS Express + MSSQL/MySQL

## Requirements

* [Moodle](https://download.moodle.org/releases/latest/)
* [PHP For Windows](https://windows.php.net/download/)
* [Microsoft Drivers for PHP for SQL Server](https://learn.microsoft.com/en-us/sql/connect/php/download-drivers-php-sql-server?view=sql-server-ver16)

## Getting started

Download the latest Moodle release and extract it into the moodle folder/directory.

Open `.\.vscode\iis-express\applicationHost.config` and replace `C:\Users\lione\Github\moodle-iis-express\` with the path to your cloned repo.<br> Configs that need to be updated:<br>
```
<site name="Moodle" id="2">
	<application path="/" applicationPool="Clr4IntegratedAppPool">
		<virtualDirectory path="/" physicalPath="C:\Users\lione\Github\moodle-iis-express\moodle\" />
	</application>
	<bindings>
		<binding protocol="http" bindingInformation="*:49902:*" />
		<binding protocol="https" bindingInformation="*:44398:*" />
	</bindings>
</site> 
``` 
```
<fastCgi>
    <application fullPath="C:\Users\lione\Github\moodle-iis-express\php-8.2.9\php-cgi.exe" monitorChangesTo="" maxInstances="2" idleTimeout="120" activityTimeout="120" requestTimeout="120" >
        <environmentVariables>
            <environmentVariable name="PHP_FCGI_MAX_REQUESTS" value="10000" />
            <environmentVariable name="PHPRC" value="C:\Users\lione\Github\moodle-iis-express\php-8.2.9" />
        </environmentVariables>
    </application>
</fastCgi>
```
```
<handlers accessPolicy="Read, Script">
	<add name="PHPv8" path="*.php" verb="*" modules="FastCgiModule" scriptProcessor="C:\Users\lione\Github\moodle-iis-express\php-8.2.9\php-cgi.exe" resourceType="File" requireAccess="Script" />
</handlers>
```

Open `.\.vscode\moodle.code-workspace` in Visual Studio Code and then run the `iis-express` task.

## Notes
If you have issues accessing `/install.php`, you may need to comment out the rewrite rules in `applicationHost.config`.

The MSSQL driver has already been enabled in `.\php-8.2.9\php.ini` with `extension=php_sqlsrv_82_ts_x64.dll`