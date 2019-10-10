# WindowsPlaybooks

## Supported OS
- Windows 2012 R2
- Windows 2016
- Windows 2019

## Step 0 - Create Windows VM as Baseline
- "Automatic Updates" must be disabled in Local Group Policy
- Internet access to Microsoft Update Catalog must be available

## Step 1 - Install Updates on Windows VM as Baseline
`ansible-playbook -i <your host file> install_updates_as_baseline.yml -e '{"inputs": {"whitelist": [], "blacklist": ["Windows Malicious Software Removal Tool"]}}'`
- whitelist is a list of additional updates to install (must be provided as [] if not appplicable)
- blacklist is a list of updates to exclude from installation (must be provided as [] if not applicable)

## Step 2 - Create Baseline as JSON
`ansible-playbook -i <your host file> list_vm_installed_updates.yml -e '{"inputs": {"whitelist": [], "blacklist": ["Windows Malicious Software Removal Tool"]}}'`
- blacklist is a list of updates to exclude from baseline output (must be provided as [] if not applicable)

Example baseline JSON: 
`[
	{
		"Categories": "Security Updates,Windows Server 2016",
		"CveIDs": "",
		"Description": "Install this update to resolve issues in Windows. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article for more information. After you install this item, you may have to restart your computer.",
		"ID": "83d7bc64-ff39-4073-9d77-02102226aff6",
		"KB": "KB4521858",
		"MoreInfoUrls": "https://support.microsoft.com/help/4521858",
		"MsrcSeverity": "Critical",
		"RebootRequired": false,
		"ReleaseNotes": null,
		"SecurityBulletinIDs": "",
		"SizeMB": 11.6,
		"Title": "2019-10 Servicing Stack Update for Windows Server 2016 for x64-based Systems (KB4521858)"
	},
	{
		"Categories": "Security Updates,Windows Server 2016",
		"CveIDs": "",
		"Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
		"ID": "0c7eb702-e48b-48da-8ef8-e984aa6cb0b8",
		"KB": "KB4519998",
		"MoreInfoUrls": "https://support.microsoft.com/help/4519998",
		"MsrcSeverity": "Critical",
		"RebootRequired": false,
		"ReleaseNotes": null,
		"SecurityBulletinIDs": "",
		"SizeMB": 1422.2,
		"Title": "2019-10 Cumulative Update for Windows Server 2016 for x64-based Systems (KB4519998)"
	}
]`

## Step 3.1 - Check Updates to Install on Target Windows
`ansible-playbook -i <your host file> install_updates_match_baseline.yml -e '{"inputs": {"server_selection": "managed_server", "src_baseline": "<your baseline path>/win-2016-base-2019-10.json", "working_dir": "C:\\Users\\Public\\Documents", "whitelist": [], "blacklist": []}}' --check`

## Step 3.2 - Install Updates on Target Windows
`ansible-playbook -i <your host file> install_updates_match_baseline.yml -e '{"inputs": {"server_selection": "managed_server", "src_baseline": "<your baseline path>/win-2016-base-2019-10.json", "working_dir": "C:\\Users\\Public\\Documents", "whitelist": [], "blacklist": []}}' --check`
- server_selection can be "managed_server" for Windows Server Update Service or "windows_update" for Windows Update Catalog
- src_baseline is the path of the baseline JSON file
- working_dir is the working directory to use on target Windows
- whitelist is a list of additional updates to install (must be provided as [] if not appplicable)
- blacklist is a list of updates to exclude from installation (must be provided as [] if not applicable)
