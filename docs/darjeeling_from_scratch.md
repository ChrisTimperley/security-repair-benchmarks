## Prerequisites
This guide assumes a fresh updated installation of Ubuntu 21.04. Other Ubuntu versions
may require more/different packages to be installed. This guide is annotated with
instructions necessary to configure Ubuntu 18.04 when instructions differ. 

## Install docker
Docker is required for this project and must be installed first. We recommend using the official
[docker](https://docs.docker.com/engine/install/ubuntu/) documentation.
## Retrieve the code from the repository and build
1. Clone the repository :
~~~
git clone git@github.com:squaresLab/security-repair-benchmarks.git
~~~
In the event that you run into errors cloning, such as:
~~~
Please make sure you have the correct access rights
and the repository exists.
~~~
use the https version: 
~~~
git clone https://github.com/squaresLab/security-repair-benchmarks.git
~~~
2. Make the project in the cloned repository
~~~
cd security-repair-benchmarks
~~~
Checkout the demonstration branch:
~~~
git checkout aflr_demo_12_2021
~~~
Make the project:
~~~
make
~~~
In the event that make gives the same error about access rights, edit `.git/config` such
that the URLs are of the https type. For example:
~~~
[submodule "tools/Darjeeling"]
	active = true
	url = https://github.com/squaresLab/Darjeeling

~~~
This will make all the test containers and requires no less than an hour.
To make an individual test: ADD DOCS HERE AFTER VERIFYING.

3. Verify the project made correctly:
~~~
scripts/run.sh
~~~
This will open the secbugs docker container. `<ctrl> d` to exit.

## Install requirements and run darjeeling:
The following steps assume you are in the same directory as as above (`security-repair-benchmarks`).
1. Install pipenv and pip:
~~~
sudo apt-get install python3-pip
python3 -m pip install --user pipenv
sudo apt-get install pipenv
~~~
2. Set up the python environment:
~~~
pipenv install
~~~
If prompted about the following "lock" error, log out and back in to solve this issue.
~~~
Traceback (most recent call last):
  File "/home/tony/.local/lib/python3.9/site-packages/pipenv/resolver.py", line 766, in <module>
    main()
  File "/home/tony/.local/lib/python3.9/site-packages/pipenv/resolver.py", line 751, in main
    from pipenv.vendor.vistir.compat import ResourceWarning
ModuleNotFoundError: No module named 'pipenv.vendor.vistir'
~~~


***If running Ubuntu 18.04 use the following instructions in stead of 1 and 2 above:***
~~~
sudo apt update && sudo apt install software-properties-common && sudo add-apt-repository ppa:deadsnakes/ppa && sudo apt install python3.9
sudo apt install python3.9-distutils
sudo apt install python3-pip
python3.9 -m pip install --user pipenv
pipenv install
~~~
If this fails, log out and log back in again, then rerun in `security-repair-benchmarks`:
~~~
pipenv install
~~~

3. Run darjeeling


To run darjeeling first generate a configuration file, for example: 
~~~
pipenv run scripts/generate-darjeeling-config.py bugs/vulnloc/coreutils/gnubug_19784
~~~
Next run darjeeling on the generated yml file:
~~~
pipenv run darjeeling repair bugs/vulnloc/coreutils/gnubug_19784/repair.darjeeling.yml
~~~
The following bugs can be used with darjeeling:
| Project | CVE/exploit |
|---------|-------------|
|libtiff  |bugzilla_2633    |
|libtiff  |cve_2016_10092   |
|libtiff  |cve_2016_10094   |
|libtiff  |cve_2016_10272   |
|libtiff  |cve_2016_3186    |
|libtiff  |cve_2016_5321    |
|libtiff  |cve_2016_9273    |
|libtiff  |cve_2016_9532    |
|libtiff  |cve_2017_7599    |
|libxml2  |cve_2012_5134    |
|libxml2  |cve_2016_1839    |
|libxml2  |cve_2017_5969    |
|binutils |cve_2017_14745   |
|binutils |cve_2017_15025   | 
|coreutils|gnubug_19784     |

