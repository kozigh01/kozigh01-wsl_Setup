# Setup up new WSL Distribution

## Resources:
* WSL:
    * [Export and import any Linux distribution in Windows Subsystem for Linux (WSL)](https://4sysops.com/archives/export-and-import-any-linux-distribution-in-windows-subsystem-for-linux-wsl/)
    * [https://4sysops.com/archives/export-and-import-any-linux-distribution-in-windows-subsystem-for-linux-wsl/](https://superuser.com/questions/1566022/how-to-set-default-user-for-manually-installed-wsl-distro/1627461#1627461)
* [Scala](https://www.scala-lang.org/)
  * [Install Scala](https://www.scala-lang.org/download/)
  * Install specific release: [releases](https://www.scala-lang.org/download/all.html)
  * https://github.com/kozigh01/wsl_Setup/edit/main/README.md
  * Scala setup: [Single command #Scala Environment Setup via #Coursier](https://www.youtube.com/watch?v=o9H2EQO3fVs&list=PLZ9n2Pz060AZ5YpfXHUBzFWdVp0WFIN-M&index=2)
* [Coursier](https://get-coursier.io/)

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

## Setup a Scala distribution ([directions link](https://www.youtube.com/watch?v=o9H2EQO3fVs&list=PLZ9n2Pz060AZ5YpfXHUBzFWdVp0WFIN-M&index=2))
1. Uising the windows terminal:
    1. Open the target distribution using the dropdown
    1. Or - run: `wsl -d <distro name>`
1. Go to the Coursier site to find [installation instructions](https://get-coursier.io/docs/cli-installation):
1. In the distrobution, run the following:
    ```bash
    # install gzip if not found (`$ which gzip`)
    $ sudo apt update && sudo apt full-upgrade -y
    $ sudo apt install gzip

    # install Coursier
    $ curl -fL https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-linux.gz | gzip -d > cs
    $ chmod +x cs
    $ ./cs setup

    ## follow the prompts to: install java, update ~/.profile, update path
    ##   then close and reopen the distrobution, and check apps installed
    $ scala -version
    $ cs version
    $ java -version
    
    ## I

    # can now remove the original downloaded cs launcher:
    $ which cs
    $ rm -rf ./cs
    
    # additional tools that can be installed using Coursiers:
    $ cs list
    $ cs install giter8

    # use gliter8 to create a scala-seed project
    $ mkdir scala && cd scala
    $ g8 devinsideyou/scala-seed
    # Answer prompts, for example:
    #   name: scala-project01
    #   organization: com.mkozi
    #   package:
    $ cd scala-project01/
    $ sbt test
    $ code .
    ```

1. In VS Code, install the metals extension
1. Look at the VS Code notifications, should see one for importing the build - do that.
1. Can use metals to create new app by selecting from list of templates - just hit the 'New Scala Project' button (don't forget to look in the VS Code notifications for an option to import the build)
1. See end of youtube video for how to use the extension 'Scaladex' to generate code for importing dependencies into the 'build.sbt' file.

