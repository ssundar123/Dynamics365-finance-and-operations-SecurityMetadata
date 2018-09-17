Use [AXDBName]
go
DECLARE @Duties TABLE
(
	IDENTIFIER nvarchar(81),
	XMLData xml
)

DECLARE @DutiesPrivilege TABLE
(
	Privilege nvarchar(100),
	Duty nvarchar(200)
)

INSERT INTO @Duties(Identifier, XMLData)
  SELECT [IDENTIFIER], (CAST([DATA] as XML))
    FROM [AxDB].[dbo].[SECURITYPRIVILEGEPARENTREFERENCES]

;WITH XMLNAMESPACES (DEFAULT 'http://schemas.datacontract.org/2004/07/Microsoft.Dynamics.AX.Security.Management')	
INSERT INTO @DutiesPrivilege(Privilege, Duty)
SELECT T.IDENTIFIER,(Cast(T2.Duties.query('text()') as nvarchar(200))) as Duty
FROM  @Duties T
CROSS APPLY T.XMLData.nodes('/PrivilegeParents/Duties/node()') as T2(Duties) 

select 
Role1.AOTNAME as RoleAOTName,
Role1.NAME as RoleName,
Duty.IDENTIFIER as DutyIdentifier,
Duty.NAME as DutyName,
DP.Privilege as PrivilegeIdentifier,
P.NAME as PrivilegeName
 from dbo.SECURITYROLEDUTYEXPLODEDGRAPH graph
join dbo.SECURITYDUTY Duty on Duty.RECID = graph.SECURITYDUTY
join dbo.SECURITYROLE Role1 on Role1.RECID = graph.SECURITYROLE
join @DutiesPrivilege DP on DP.Duty = Duty.IDENTIFIER
join dbo.SECURITYPRIVILEGE P on P.IDENTIFIER = DP.Privilege

select PP.PRIVILEGEIDENTIFIER,P.Name,
case PP.SECURABLETYPE
when 1 then 'menuitemdisplay'
when 2 then 'menuitemoutput'
when 3 then 'menuitemaction'
when 55 then 'weburlitem'
when 56 then 'webactionitem'
when 57 then 'webdisplaycontentitem'
when 58 then 'weboutputcontentitem'
when 75 then 'webmanagedcontentitem'
when 73 then 'webcontrol'
when 59 then 'webletitem'
when 44 then 'tablefield'
when 45 then 'classmethod'
when 76 then 'serviceOperation'
when 11 then 'FormControl'
when 82 then 'formpart'
when 81 then 'infopart'
when 85 then 'ssrsreport'
when 18 then 'report'
when 115 then 'codepermission'
when 143 then 'formdatasource'
when 67 then 'dataentity'
when 146 then 'dataentitymethod'
End as SecurableType,
PP.AOTName  as PermissionAOTname,
PP.AOTCHILDNAME	as PermissionAOTChildName,
PP.READACCESS as ReadAccess,
PP.UPDATEACCESS as UpdateAccess,
PP.CREATEACCESS as CreateAccess,
PP.CorrectAccess as CorrectAccess,
PP.DeleteAccess as DeleteAccess,
PP.INVOKEACCESS as InvokeAccess from
 dbo.[SECURITYRESOURCEPRIVILEGEPERMISSIONS] PP 
 join dbo.SECURITYPRIVILEGE P on P.IDENTIFIER = PP.PRIVILEGEIDENTIFIER


