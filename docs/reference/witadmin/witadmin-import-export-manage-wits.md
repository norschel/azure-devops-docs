---
title: Import, export, and manage work item types
titleSuffix: Azure DevOps  
description: Manage work item types for a project 
ms.service: azure-devops-boards
ms.custom: witadmin
ms.assetid: 97d7ea1c-df1f-4999-adc9-b38dd2a6cca6
ms.topic: reference
ms.author: chcomley
author: chcomley
monikerRange: '<= azure-devops'
ms.date: 12/01/2022
---

# Import, export, and manage work item types

[!INCLUDE [version-lt-eq-azure-devops-plus-witadmin](../../includes/version-lt-eq-azure-devops-plus-witadmin.md)]

You can manage work item types for a project by using the following `witadmin` commands:  
- `destroywitd`:  Destroys a work item type, and destroys every work item of that type permanently without recovery.    
- `exportwitd`:  Exports the definition of a work item type to an XML file, or to the Command Prompt window.    
- `importwitd`:  Imports work item types from an XML definition file into a project. If a work item type with the same name already exists, the new work item type definition overwrites the existing one. If the work item type doesn't exist, a new work item type is created. To validate the XML that defines a work item type, but not import the file, use the `/v` option.   
- `listwitd`:  Displays the names of the work item types in the specified project in the Command Prompt window. 
- `renamewitd`:  Changes the display name of a work item type within a specific project. After you run this command, work items of this type show the new name.  

To learn more about how work item types are used to track work, see [Track your work items in Azure Boards user stories, issues, bugs, features, and epics](../../boards/work-items/about-work-items.md).



[!INCLUDE [temp](../../includes/witadmin-run-tool.md)]  
 
[!INCLUDE [temp](../../includes/process-editor.md)]


## Prerequisites  
  
For the project where the work item types are defined, have the following permissions set:   
-   To export or list work item types, be a member of the **Project Administrators** group or have your **View project-level information** permission set to **Allow**.    
-   To destroy, import, or rename work item types, be a member of the **Team Foundation Administrators** security group or the **Project Administrators** security group.  
  
For more information, see [Change project collection-level permissions](../../organizations/security/change-organization-collection-level-permissions.md).  
  
> [!NOTE]
>  Even if you sign in with administrative permissions, you must open an elevated Command Prompt window to perform this function on a server that is running Windows Server 2008. To open an elevated Command Prompt window, choose **Start**, open the shortcut menu for the **Command Prompt**, and then choose **Run as Administrator**. For more information, see the Microsoft Web site: [User Access Control](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772207(v=ws.10)).  
  
## Syntax  
  
```  
witadmin destroywitd /collection:CollectionURL /p:Project /n:TypeName [/noprompt]   
witadmin exportwitd /collection:CollectionURL /p:Project /n:TypeName [/f:FileName] [/e:Encoding] [/exportgloballists]  
witadmin importwitd /collection:CollectionURL [/p:Project] /f:FileName [/e:Encoding] [/v] 
witadmin listwitd /collection:CollectionURL /p:Project    
witadmin renamewitd /collection:CollectionURL /p:Project /n:TypeName /new:NewName [/noprompt]   
```  

  
#### Parameters  
  
|**Parameter**|**Description**|  
|-------------------|---------------------|  
|`/collection`:`CollectionURL`|Specifies the URI of the project collection. For example:<br /><br /> **On-premises format:** `http://ServerName:Port/VirtualDirectoryName/CollectionName`<br /> If no virtual directory is used, then use the following format: `http://ServerName:Port/CollectionName`.|  
|`/p:Project`|The project for which the types of work items are to be managed. This project must be defined in the project collection specified by the **/collection** parameter.<br /><br /> The **/p** parameter is required unless you run the **importwitd** command with the **/v** option.|  
`/n:TypeName`|The name of the work item type to destroy, export, import, or rename.|  
|`/f:FileName`|The path and file name of the XML definition file that contains the types of work items to be exported or imported. If you omit this parameter when you use the **exportwitd** command, the XML appears in the Command Prompt window.<br /><br /> **Note:**  If you are using Windows Vista you might not have permissions to certain folders. If you try to export the work item type to a location where you do not have permissions, the registry virtualization technology automatically redirects the exported file and saves it to the virtual store. To avoid this redirection, you can export the file to a location where you have permissions. For more information, see the [Registry Virtualization](/windows/win32/sysinfo/registry-virtualization) page on the Microsoft web site.|  
|`/e:*Encoding`|The name of a .NET Framework 2.0 encoding format. The command uses the specified encoding to export or import the XML data. For example, `/e:utf-7` specifies Unicode (UTF-7) encoding. If you omit this parameter, **witadmin** tries to detect the encoding, and if detection fails, **witadmin** uses UTF-8.|  
|`/exportgloballists`|Exports the definitions of global lists referenced by the work item type. The definitions for global lists will be embedded into the work item type definition XML. When not specified, the definitions for global lists are omitted.|  
|`/v`|Validates the XML that defines the work item type, but doesn't import the XML definition file. **Note:**  You can validate the type definition without specifying a project. References to project-scoped groups is ignored.|  
|`/new:NewName`|The new name of the work item type.|  
|`/noprompt`|Disables the prompt for confirmation.|  
|`/?` or `help`|Displays help about the command in the Command Prompt window.|  
  
## Remarks  

When you use the `destroywitd` command, it destroys all the following objects:  
  
- The work item type   
- All work items of that type    
- Corresponding entries in the work item tables, the long text tables, and the link tables   
- Objects in the work item type metadata cache  
  
## Examples  

Unless otherwise specified, the following values apply in each example:  
  
-   URI for the project collection: `http://AdventureWorksServer:8080/tfs/DefaultCollection`    
-   Project name: `AdventureWorks`    
-   Input or output file name: `myworkitems.xml`  
-   Work item type name: `myworkitem`   
-   Default encoding: `UTF-8`  
  
### Export the definition of a WIT  

The following command exports the definition for myworkitem to the file, myworkitems.xml.  
  
```  
witadmin exportwitd /collection:http://AdventureWorksServer:8080/tfs/DefaultCollection /p:AdventureWorks /f:myworkitems.xml /n:myworkitem  
```  
  
The following example exports the work item by using Unicode (UTF-7) encoding.  
  
```  
witadmin exportwitd /collection:http://AdventureWorksServer:8080/tfs/DefaultCollection /p:AdventureWorks /f:myworkitems.xml /n:myworkitem /e:utf-7  
```  
  
### Export the definition of a WIT and its referenced global lists  

The following example exports the work item type and its referenced global lists.  
  
```  
witadmin exportwitd /collection:http://AdventureWorksServer:8080/tfs/DefaultCollection /p:AdventureWorks /f:myworkitems.xml /n:myworkitem /exportgloballists  
```  
  
### List the definition of a WIT  

The following example displays the definition of the work item type the Command Prompt window.  
  
```  
witadmin exportwitd /collection:http://AdventureWorksServer:8080/tfs/DefaultCollection /p:AdventureWorks /n:myworkitem  
```  
  
### Import the definition of WITs  

The following example imports the work item definition from the XML file.  
  
```  
witadmin importwitd /collection:http://AdventureWorksServer:8080/tfs/DefaultCollection /f:myworkitem.xml /p:AdventureWorks  
```  
  
### Validate the XML definition of a WIT  
 The following example validates the XML that defines the work item type but doesn't import the definition.  
  
```  
witadmin importwitd /collection:http://AdventureWorksServer:8080/tfs/DefaultCollection /f:myworkitem.xml /p:AdventureWorks /v  
```  
  
## Q & A  
  
### Q: What customizations can I make and still use the Configure Features Wizard to update my project after an upgrade?  

**A:** You can add custom WITs and change the form layout. The [Configure Features Wizard](/previous-versions/azure/devops/reference/upgrade/configure-features-after-upgrade?view=tfs-2017&preserve-view=true) will update your projects and you'll get access to the latest features.  
  
Changing the workflow or renaming a WIT might require you to perform some manual operations when updating your project. To learn about which customizations you can safely make and which you should avoid, see [Customize the work tracking experience: Before you customize, understand the maintenance and upgrade implications](../on-premises-xml-process-model.md#before-you-customize).  
  
<a name="color"></a> 

###  Q: How do I change the color associated with a WIT?  

**A:** In the web portal, work items appear in query results and on the backlog and board pages of the Agile planning tools. To change the color associated with an existing WIT or add the color to use for a new WIT, [edit the process configuration](../xml/process-configuration-xml-element.md).  
  
 ![Color assignments to different work item types](media/alm_pc_colorconfig.png "ALM_PC_ColorConfig")  
  
### Q: How do I deactivate or disable a WIT? How do I restrict users from creating work items of a certain type?  
**A:** If you have a work item type that you want to retire, but maintain the work items that have been created based on that type, you can add a rule that disables all valid users from saving the work item type.  
  
> [!div class="tabbedCodeSnippets"]
> ```XML  
>     <TRANSITION from=" " to="New">  
>        <FIELDS>  
>          <FIELD refname="System.CreatedBy">  
>               <VALIDUSER not="[Team Project Name]Project Valid Users" />  
>          </FIELD>  
>        </FIELDS>  
>     </TRANSITION>     
> ```  
  
If you want to restrict creation of a specific WIT to a group of users, there are two ways to restrict access:  
  
- [Add the WIT to the Hidden Categories group](../xml/use-categories-to-group-work-item-types.md) to prevent the majority of contributors from creating them. If you want to allow a group of users access, you [can create a hyperlink to a template](../../boards/backlogs/work-item-template.md) that opens the work item form and share that link with those team members who you do want to create them.  
- Add [a field rule to the workflow](../../organizations/settings/work/rule-reference.md) for the System.CreatedBy field to effectively restrict a group of users from creating a work item of a specific type. As the following example shows, the user who creates the work item must belong to the `Allowed Group` in order to save the work item.  
  
> [!div class="tabbedCodeSnippets"]
> ```XML 
> <TRANSITION from=" " to="New">  
>    <FIELDS>  
>      <FIELD refname="System.CreatedBy">  
>          <VALIDUSER for="Allowed Group" not="Disallowed Group" />  
>      </FIELD>  
>    </FIELDS>  
> </TRANSITION>  
> ```  


<a name="delete"></a>  

### Q: How do I delete a WIT?  

**A:** To prevent team members from using a specific WIT to create a work item, you can remove it from the project. When you use `witadmin destroywitd`, you permanently remove all work items that were created using that WIT as well as the WIT itself. For example, if your team doesn't use Impediment you can delete the WIT labeled Impediment from the Fabrikam Web Site project.  
  
```  
witadmin destroywitd /collection:"http://FabrikamPrime:8080/tfs/DefaultCollection" /p:"Fabrikam Web Site" /n:"Impediment"   
```  
  
When you delete a WIT that belongs to a category, you must update the categories definition for the project to reflect the new name. In particular, the [Agile planning tools](../../boards/get-started/what-is-azure-boards.md) will not work until you update the categories definition.  
  
For more information, see [Import and export categories](/previous-versions/azure/devops/reference/witadmin/witadmin-import-export-categories).  
  
## Related articles  
-  [Customize your work tracking experience](../customize-work.md)
-  [Work item field index](../../boards/work-items/guidance/work-item-field.md)   
-  [witAdmin: Customize and manage objects for tracking work](witadmin-customize-and-manage-objects-for-tracking-work.md)