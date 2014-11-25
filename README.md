XtremIOSnap
===========
   ------------------------------------------------------------------------
   Version 3.0
   
Summary
-------

Snapshots can provide a number of benefits, from being used as backups in the event a recovery is needed at some point in the future, to making copies of production datasets available for non-production use (test/dev, reporting, etc).  Historically, snapshot use has been limited due to performance constraints introduced by COFW mechanisms or limitations on the underlying block filesystem (redirect on write).  

With XtremIO, snapshots can be easilly created and mapped to hosts to create full-performance, space-efficient, writeable copies of production data for a variety of uses without the legacy caveats that have become commonplace with legacy storage arrays.  However, in the current version, 3.0, a built in snapshot scheduler does not exist, making it challenging to automate snapshots for operational uses.

As a stop-gap, XtremIOSnap has been designed to bridge the gap that exists in the scheduling of snapshots until a scheduler is included in XMS. Additionlly, XtremIOSnap provides a working example of using the XtremIO REST API to interact and perform actions on the XtremIO array.

<br> 
Install
-------
   Create and maintain snapshots on an XtremIO array utilizing the REST API interface.  Designed and tested for v3.0.
   Visual C++ 2008 Redistributable package from MS (http://www.microsoft.com/en-us/download/details.aspx?id=29 ) is required for the compiled Windows executable.
   
   If you are running this in python, it was written against Python 2.7.8 and will require the requests (2.4.3 or newer) package and the docopt package to be installed using:
   
      pip install requests
      pip install requests --upgrade (if using an older version of requests)
      pip install docopt
    
   The script has been tested back to python 2.6 on both Linux (Ubuntu 14.04) and Mac OSX Mavericks.
<br>
Usage
-------   
      
      XtremIOSnap -h | --help
      XtremIOSnap (--encode) XMS_USER XMS_PASS [--l=<log_path>] [--debug]
      XtremIOSnap XMS_IP XMS_USER XMS_PASS [--e]  [(--f --snap=<object_to_snap>)] [--n=<number_of_snaps>] [--schedule=<schedule>] [--tf=<target_folder>] [--l=<log_path>] [--debug]
      XtremIOSnap XMS_IP XMS_USER XMS_PASS [--e]  [(--v --snap=<object_to_snap>)] [--n=<number_of_snaps>] [--schedule=<schedule>] [--tf=<target_folder>] [--l=<log_path>] [--debug]

   Create and maintain snapshots of both volumes and folders on an XtremIO array utilizing the REST API interface.  Designed and tested for XtremIO v3.0+.

Arguments
---------

      XMS_IP                  IP or Hostname of XMS (required)
      XMS_USER                Username for XMS
      XMS_PASS                Password for XMS

Options
-------

      -h --help               Show this help screen

      --encode                Use this option with the XMS_USER and XMS_PASS
                            arguments to generate an encoded Username and Password
                            so the user and password don't need to be saved in
                            clear text when using in a script or task scheduler.

    --e                     If specified, will use the encoded User and Password
                            generated by the --encode option.

    --f                     Specify to signify the object to snap is a folder.

    --v                     Specify to signify the object to snap is a volume.

    --snap=<object_to_snap> Object to snap, either a volume or folder

    --n=<number_of_snaps>   Number of snapshots to retain [default: 5]

    --schedule=<schedule>   [hourly | daily | weekly] Used in naming the snapsots
                            based on how they are scheduled [default: hourly]

    --tf=<target_folder>    When specified, a _Snapshot subfolder will be created
                            in this folder.  If not used, snapshots will be saved
                            in a _Snapshot folder at the root.

    --l=<log_path>          [default: """+var_cwd+"""/XtremIOSnap.log]

    --debug



   --schedule=hourly will append the suffix .hourly.0 allong with a timestamp to the newest snapshot.  The suffix will be shifted as new snaps are taken, up to the specified number of snapshots (hourly.0 will become hourly.1, hourly.1 will become hourly.2, etc.).  By default we will keep 5 hourly snapshots.

   --schedule=daily will append the suffix .daily.0 allong with a timestamp to the newest snapshot.  The suffix will be shifted as new snaps are taken, up to the specified number of snapshots (daily.0 will become daily.1, daily.1 will become daily.2, etc.). By default we will keep 5 daily snapshots.

   --schedule=weekly will append the suffix .weekly.0 allong with a timestamp to the newest snapshot.  The suffix will be shifted as new snaps are taken, up to the specified number of snapshots (weekly.0 will become weekly.1, weekly.1 will become weekly.2, etc.). By default we will keep 5 weekly snapshot.

   If the --n= option is not used, the script will maintain a maximum of 5 snapshots, deleting snaps on a FIFO basis, depending on the --schedule=hourly/daily/weekly option.

   The --f= switch is optional, if not specified, all snapshots will be placed into the /_Snapshots folder.  This is not really necessary for anything other than aesthetics.  You can select the "Show as Snapshot Hierarchy" to view snaps with their source LUN.

Contributing
-----------
Please contribute in any way to the project.  Specifically, normalizing differnet image sizes, locations, and intance types would be easy adds to enhance the usefulness of the project.


Licensing
---------
Licensed under the Apache License, Version 2.0 (the “License”); you may not use this file except in compliance with the License. You may obtain a copy of the License at <http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an “AS IS” BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

Support
-------
Please file bugs and issues at the Github issues page. For more general discussions you can contact the EMC Code team at <a href="https://groups.google.com/forum/#!forum/emccode-users">Google Groups</a>. The code and documentation are released with no warranties or SLAs and are intended to be supported through a community driven process.
