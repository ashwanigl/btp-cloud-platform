<!-- loio46355fab22814944bedf449a6c953369 -->

# Enable Command Autocompletion

Use command autocompletion to save keystrokes when entering command actions, group-object combinations, and their parameters in the SAP BTP command line interface \(btp CLI\).



## Context

Autocompletion in btp CLI currently supports the following shells:

-   Bash
-   PowerShell
-   Zsh

> ### Note:  
> The respective shell must be installed on your operating system before enabling autocomplete.

Once autocomplete is enabled \(it’s disabled by default\), you use the autocomplete feature as follows in the command line:

-   Enter a partial command action, group-object combination, or parameter, and then press the [Tab\] key. The command line either automatically completes your command or, when there’s more than one option available, it displays a list of suggested command actions/options/parameters.

-   When a suggestion list is displayed, depending on your shell, use the [Tab\] or arrow keys to move through the list and press [Enter\] to make a selection.




### Examples

The following examples show various ways that you can use autocompletion:

-   Enter `btp` and press [Tab\] to display all available actions:

    ```nocode
    ./btp ***TAB***
    add          create       enable       list         logout       register     subscribe    unassign     unsubscribe
    assign       delete       get          login        move         remove       target       unregister   update 
    
    ```

-   Partially enter `btp cre` and press [Tab\] to autocomplete the command to `btp create`. Then, press [Tab\] again to display a suggested list of group-object combinations:

    ```nocode
    ./btp create ***TAB***
    accounts/directory             accounts/resource-provider     security/role                  services/binding
    accounts/environment-instance  accounts/subaccount            security/role-collection       services/instance
    
    ```

-   Partially enter a group and press [Tab\] to display a suggested list of objects:

    ```nocode
    ./btp create accounts***TAB***
    accounts/directory             accounts/environment-instance  accounts/resource-provider     accounts/subaccount
    ```

-   Partially enter a parameter and press [Tab\] to display a suggested list of parameters:

    ```nocode
    ./btp create accounts/subaccount -***TAB***
    --beta-enabled
    --custom-properties
    --description
    --directory
    --display-name
    --global-account
    --region
    --subaccount-admins
    --subdomain
    --used-for-production
    ```




<a name="loio46355fab22814944bedf449a6c953369__steps_mdz_lzw_xmb"/>

## Procedure

1.  Use `btp enable autocomplete *<SHELL\>*` to enable command autocompletion for a specified shell.

    The valid values for the supported shells are:

    -   `bash`
    -   `powershell`
    -   `zsh`

    > ### Sample Code:  
    > `btp enable autocomplete zsh`

2.  The client asks for confirmation to install the autocomplete plugin script at the specified location. Enter "y" or "yes" to continue.

    Apart from the script being added to your file system, the RC file of your shell is modified to call this script at startup. The client looks for this RC file in your system and proposes possible files. You can choose to accept a proposal or specify a different RC file. If no RC file is found, the clients prints an error message with the full path of the missing file, so you can create it and run the enable command again.

3.  Enter the option of your choice.

    To specify a custom path, first choose "Custom" and then enter the full path of the RC file to use.

    For example:

    ```nocode
    ./btp enable autocomplete powershell
    This will install the autocomplete plugin script for powershell to C:\AppData\btp\autocomplete\scripts. Do you want to continue? [no]>y
    Which RCFile should be used for the installation?
    1: C:\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
    2: Custom
    Enter option>**2**
    Enter the full path of your RCFile>**C:\\Documents\\WindowsPowerShell\\Microsoft.PowerShell\_my-custom-profile.ps1**
    ```

4.  Start a new terminal session to activate the installed autocomplete script.




<a name="loio46355fab22814944bedf449a6c953369__result_l3y_pyw_xmb"/>

## Results

When you enable command autocompletion, a script containing all the autocomplete commands is downloaded and installed in your file system.

The autocompletion option remains enabled in future sessions in your current client, until you disable it. To disable command autocompletion and uninstall the autocomplete script, run the following command:

```
btp disable autocomplete *<SHELL\>*
```

You can run either `btp` or `btp --info` to see if command autocompletion is currently enabled and where the autocomplete script for your shell is located. If you don't see a line specifying the location of the autocomplete script, then it’s disabled.

> ### Tip:  
> If you see a discrepancy between the version of the autocomplete script and the client, the update of the autocomplete script might have failed. In such a case, try to disable and enable the autocomplete feature again.

Whenever you start a new btp CLI terminal session, the installed autocomplete scripts are automatically updated to include the latest commands. If a script is updated, you're prompted to restart your terminal session to load the newest autocomplete information.

If disabling the command autocompletion fails or you have uninstalled the btp CLI client without disabling autocompletion, you can manually remove traces of the autocomplete installation in your shell initialization file \(RC or profile file depending on your shell\) by deleting the line that starts with `SAPCP_CLI_AUTOCOMPLETE`.

**Related Information**  


[Get Help](get-help-f8fd1e5.md "There is extensive help in the btp CLI about every command. You can get help with the help action or the --help option.")

[View Version and Current Context](view-version-and-current-context-9c29222.md "To find out the current context you’re working in, run the command btp --info or simply btp.")

[Log in](log-in-e241b30.md "Log in with the btp CLI is on global account level.")

[Log out](log-out-9f1c87a.md "Logging out of the configured server removes all user-specific data from the configuration file.")

[Set the Default Command Context](set-the-default-command-context-720645a.md "Change the default context for all command calls to the global account, a directory, or a subaccount by using the btp target command.")

[Change the Output Format to JSON](change-the-output-format-to-json-dcb85b7.md "Use the --format json option to change the output format of a command to JSON.")

[Specify the Location of the Configuration File](specify-the-location-of-the-configuration-file-e57288d.md "You can change the location of the configuration file by using the --config option or the environment variable.")

