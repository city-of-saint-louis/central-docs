# Style Guides : ColdFusion 

## ColdFusion Tags
Wherever possible, try not to use coldfusion tags for the bulk of your scripts. Instead use cfscript, as it's easier to write and read. For necessary logic in views tags do make more sense, but try to keep that to a minimum.

When updating legacy code that uses tags for everything it's fine to continue using tags if more convenient.

``` cfm title="Tags are lowercase"
<!-- Incorrect -->
<CFPARAM name = "foo" Default = "bar">
<!-- Correct -->
<cfparam name = "foo" default = "bar">
```

``` cfm title="Putting tags on their own improves readability"
<!--- Incorrect --->
<cfquery name = "getItems" dataSource = "tblSomeTable">
    ...</cfquery> <cfif thisVariable EQ thatVariable>
    ...
    </cfif>
<!--- Correct --->
<cfquery name = "getItems" dataSource = "tblSomeTable">
    ...
</cfquery>
<cfif thisVariable EQ thatVariable>
    ...
</cfif>
```

``` cfm title="Break long lists of attributes into several lines so they are less cluttered"
<!--- Incorrect --->
<cfchart format = "png" chartHeight = "200" chartWidth = "300" showLegend = "true" font = "verdana">
    ...
</cfchart>
<!--- Correct --->
<cfchart
    format = "png"
    chartHeight = "200"
    chartWidth = "300"
    showLegend = "true"
    font = "verdana">
    ...
</cfchart>
```

## ColdFusion Control Structures
These include cfif, cfswitch, etc.

``` cfm title="Nest tags appropriately using line breaks and indenting"
<cfif thisVariable GTE 12 OR anotherVariable EQ 9>
    <cfset someVariable = 33>
<cfelseif thisVariable LTE 3>
    <cfset someVariable = 65>
<cfelse>
    <cfset someVariable = 1>
</cfif>
<cfscript>
    if (thisVariable GTE 12 OR anotherVariable EQ 9) {
        someVariable = 33;
    }
    else if (thisVariable LTE 3) {
        someVariable = 65;
    }
    else {
        someVariable = 1;
    }
</cfscript>
```

``` cfm title="Split long statements into several lines"
<cfif
    thisVariable GTE 12
    OR anotherVariable EQ 9
    OR someVariable NEQ 50
    OR thisVarialbe EQ 340>
    ...
</cfif>
<cfscript>
    if (
        thisVariable GTE 12
        OR anotherVariable EQ 9
        OR someVariable NEQ 50
        OR thisVarialbe EQ 340
    ) {
        someVariable = 33;
    }
</cfscript>
```

## ColdFusion and HTML
``` cfm title="Avoid ColdFusion logic within HTML tags where possible"
<!-- Avoid this -->
<ul>
    <li>#((totalWidgets - soldWidgets) / totalWidgets) * 100#%</li>
</ul>
<!-- This is preferrable -->
<cfset percentWidgetsLeft = ((totalWidgets - soldWidgets) / totalWidgets) * 100>
<ul>
    <li>#percentWidgetsLeft#%</li>
</ul>
```

## ColdFusion Scope
``` cfm title="Use the variable's scope prefix (except for the 'variable' scope)"
#form.someField#
#url.someVariable#
#application.thatFilePath#
#arguments.name#
#cfhttp.fileContents#
```