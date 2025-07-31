# dpkg



### Error in dpkg, manuelle Entfernung von Packages/Dependencies

* `pkexec gedit /var/lib/dpkg/status`
* Search for the offending package by name and remove its entry.
* Save the file and exit gedit.
* run `sudo dpkg --configure -a`
* run `sudo apt-get -f install` just in case.
* Continue on if there are no errors.
