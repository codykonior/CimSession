# PowerShell: CimSession module

## TOPIC
    about_CimSession

## SHORT DESCRIPTION
    Allows CIM sessions to be opened to Windows Server 2003 and above with 
    caching and super fast remote registry operations.

## LONG DESCRIPTION
    CIM is the preferred method of interacting with WMI on a remote computer as
    it can reuse connections after they are made.

    However the built-in PowerShell command New-CimSession doesn't automatically
    cache connections or work with Windows Server 2003 remote hosts. This is 
    fixed by using this module's replacement function New-CimSessionDown.

    Furthermore, remote registry operations have always been problematic in 
    PowerShell. Either you resort to using .NET objects and handling COM 
    disposals, or you use a 3rd party module which just uses the same. This 
    module lets you read most common remote registry entries over a CIM 
    connection. This provides for output literally 2x-4x faster than other 
    methods which really adds up at scale.

    Functions are split between three types:
    * Common functions you should use
        New-CimSessionDown
        Get-CimKey
        Get-CimValues

    * Less common functions you should only use if required
        Get-CimEnumKey            
        Get-CimEnumValues         

        Get-CimBinaryValue        
        Get-CimDWORDValue         
        Get-CimExpandedStringValue
        Get-CimMultiStringValue   
        Get-CimQWORDValue         
        Get-CimStringValue   

    * Internal functions you can't use
        Invoke-CimGetValue

	Remember, you should always specify -OperationTimeoutSec on any Get-Cim
    cmdlet. However any functions in this module default this for you to 30.

## REQUIREMENTS
	PowerShell v3 only the computer using the function, not on the host.
	
## EXAMPLE #1
	$cimSession = New-CimSessionDown C1N1
	Get-CimInstance -CimSession $cimSession -Class Win32_Service -OperationTimeoutSec 30

## EXAMPLE #2
    Get-CimKey C1N1 "SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing"

	$cimSession = New-CimSessionDown C1N1
	Get-CimEnumKey $cimSession "SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing"
	Get-CimEnumValues $cimSession "SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing"
    Get-CimStringValue $cimSession "SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing" "RepairCategory"
    Get-CimDWORDValue $cimSession "SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing" "EnableLog"

## LINKS
	https://github.com/codykonior/CimSession

