# Setup up new WSL Distribution

## Resources:
* [Export and import any Linux distribution in Windows Subsystem for Linux (WSL)](https://4sysops.com/archives/export-and-import-any-linux-distribution-in-windows-subsystem-for-linux-wsl/)
* [https://4sysops.com/archives/export-and-import-any-linux-distribution-in-windows-subsystem-for-linux-wsl/](https://superuser.com/questions/1566022/how-to-set-default-user-for-manually-installed-wsl-distro/1627461#1627461)
* Additonal setup:
  * Scala setup: [Single command #Scala Environment Setup via #Coursier](https://www.youtube.com/watch?v=o9H2EQO3fVs&list=PLZ9n2Pz060AZ5YpfXHUBzFWdVp0WFIN-M&index=2)

## Setup with custom distribution name
1. Install a distribution from the Micrsoft store
1. Update distibution (windows terminal - select distribution from dropdown)
    ```bash
    $ apt update
    $ apt list --upgradable
    $ apt full-upgrade

    # add any packages that are desired in the base exported distro
    $ apt install <package>
    ``` 
1. Export/Import the installed distrubution:cl
    ```bash
    # open windows terminal (use powershell terninal)

    # list distributions
    $ wsl -l -v

    # terninate the target distribution if running
    $ wsl terminate -t <name of distribution from previous command>

    # export the distibution
    $ wsl --export <name of distributio> <filename to save distribution>
    # example
    $ wsl --export Ubuntu-20.04 "C:\WSL2\Ubuntu_20.04_test.tar"
    ```

1. Import custom named distribution
    1. Create a folder for the custom distro - for example C:\WSL2\my-distro
    1. Run the wsl import command:
        ```bash
        $ wsl --import my-distro "C:\WSL2\" "C:\WSL2\my-distro\Ubuntu_20.04_test.tar"
        $ wsl -l -v
        ```
      1. open the new distrobution from the windows terminal by selecting from the dropdown- may need to close and re-open the terminal
      1. If the distribution opens as the root user:
          1. Create a /etc/wsl.conf file in the distribution, with following contents:
              ```
              [user]
              default=<username>
              ```
          1. Terminate and restart the distrobution (in a windows terminal - powershell):
              ```
              $ wsl --terminate my-distro
              $ wsl -l -v
              ```
          1. Close and reopen the windows terminal
          1. Open the distrobution using the dropdown - should open with user defined in the wsl.conf file