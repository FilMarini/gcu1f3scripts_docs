******************************
Typical quick start procedures
******************************

Global trigger with few GCU firing and fixed threshold
######################################################

To have a run in global trigger selecting the GCU that send trigger to BEC for trigger validation, follow these steps:

* go to the scipts home directory (*gcu_v2_scripts/*)

* do a 'make clean'

* run the 'python quick_start.py -g' command

  * *How many GCUs are connected?* 13 (example number)

  * *Do you want a fixed value for threshold?* y

  * *Insert threshold*: 9000 (e.n. equal to all the GCUs)

  * *Insert pre-trigger*: 45 (e.n.)

  * *Insert trigger window*: 30 (e.n.)

  * *Do you want to store the acquisition data? (y/n)*: y

  * *Do you want binary output file? (y/n)*: y

  * *Write path to acquisition files storing folder*: /data/48PMT/test_python_script_bin/20200629

  * *Do you want to create a rarpd ethers file? (y/n)*: n

  * *Global trigger mode? (y/n, n enables auto-trigger mode)*: y

  * *Do you want to enable some triggers? (y/n)*: y

  * *Write all the GCU numbers that you want to fire to the BEC, separated by a comma (w/o spaces)*: 1,2,4,5,6,7,8,9,13

  * (Alternatively, you can type *python quick_start.py -s13 --t_type -t9000 -p45 -w30 --store -b --path /data/48PMT/test_python_script_bin/20200629 --global --set_auto --set_auto_which 1,2,4,5,6,7,8,9,13*)

* run 'make'

* run 'master_script.sh'

* cd to 'bec_cal_scripts'

* run 'python BEC_go_ext.py -e0' (no external trigger)

* run 'python BEC_set_trigger.py -g4603 -m3' (e.n. . The 4603 value applies to the legnaro system today (26/08/2020); 3 is the multeplicity value)

* cd back to the main scripts folder and then to 'gcu_acq_scripts'

* run the 'python general_soft_reset.py -s13' (to make sure the l1 cache are ok, 13 is the number of GCUs in the setup)

* run the 'source setup_channels.sh' to apply the DAQ parameters to all the GCUs

* run the 'master_readout.sh' script to start the acquisition
  
Global trigger with few GCU firing and std deviation threshold
##############################################################

To have a run in global trigger selecting the GCU that send trigger to BEC for trigger validation, follow these steps:

* go to the scipts home directory (*gcu_v2_scripts/*)

* do a 'make clean'

* run the 'python quick_start.py -g' command

  * *How many GCUs are connected?* 13 (example number)

  * *Do you want a fixed value for threshold?* n

  * *Insert threshold*: 6 (the baseline and the relative std deviation will be evaluated for each channel)

  * *Insert pre-trigger*: 45 (e.n.)

  * *Insert trigger window*: 30 (e.n.)

  * *Do you want to store the acquisition data? (y/n)*: y

  * *Do you want binary output file? (y/n)*: y

  * *Write path to acquisition files storing folder*: /data/48PMT/test_python_script_bin/20200629

  * *Do you want to create a rarpd ethers file? (y/n)*: n

  * *Global trigger mode? (y/n, n enables auto-trigger mode)*: y

  * *Do you want to enable some triggers? (y/n)*: y

  * *Write all the GCU numbers that you want to fire to the BEC, separated by a comma (w/o spaces)*: 1,2,4,5,6,7,8,9,12,13

  * (Alternatively, you can type *python quick_start.py -s13 -t6 -p45 -w30 --store -b --path /data/48PMT/test_python_script_bin/20200629 --global --set_auto --set_auto_which 1,2,4,5,6,7,8,9,13*)

* run 'make'

* run 'master_script.sh'

* cd to 'bec_cal_scripts'

* run 'python BEC_go_ext.py -e0' (no external trigger)

* run 'python BEC_set_trigger.py -g4603 -m3' (e.n. . The 4603 value applies to the legnaro system today (26/08/2020); 3 is the multeplicity value)

* cd back to the main scripts folder and then to 'gcu_acq_scripts'

* run the 'python general_soft_reset.py -s13' (to make sure the l1 cache are ok, 13 is the number of GCUs in the setup)

* run the 'source setup_channels.sh' to apply the DAQ parameters to all the GCUs

* run the 'master_readout.sh' script to start the acquisition

Auto-trigger
############

To have a run in auto trigger, follow these steps:

* go to the scipts home directory (*gcu_v2_scripts/*)

* do a 'make clean'

* run the 'python quick_start.py -g' command

  * *How many GCUs are connected?* 13 (example number)
    
  * *Do you want a fixed value for threshold?* y

  * *Insert threshold*: 9000 (e.n. equal to all the GCUs)

  * *Insert pre-trigger*: 15 (e.n.)

  * *Insert trigger window*: 30 (e.n.)

  * *Do you want to store the acquisition data? (y/n)*: y

  * *Do you want binary output file? (y/n)*: y

  * *Write path to acquisition files storing folder*: /data/48PMT/test_python_script_bin/20200629

  * *Do you want to create a rarpd ethers file? (y/n)*: n

  * *Global trigger mode? (y/n, n enables auto-trigger mode)*: n

  * (Alternatively, you can type *python quick_start.py -s13 --t_type -t9000 -p45 -w30 --store -b --path /data/48PMT/test_python_script_bin/20200629*)

* run 'make'

* run 'master_script.sh'

* cd to 'gcu_acq_scripts'

* run the 'python general_soft_reset.py -s13' (to make sure the l1 cache are ok, 13 is the number of GCUs in the setup)

* run the 'source setup_channels.sh' to apply the DAQ parameters to all the GCUs

* run the 'master_readout.sh' script to start the acquisition

External trigger
################

To have a run in external trigger, follow these steps:

* go to the scipts home directory (*gcu_v2_scripts/*)

* do a 'make clean'

* run the 'python quick_start.py -g' command

  * *How many GCUs are connected?* 13 (example number)

  * *Do you want a fixed value for threshold?* y (using the external trigger the threshold has no use)

  * *Insert threshold*: 9000 (e.n. equal to all the GCUs, this value does not matter in this case)

  * *Insert pre-trigger*: 45 (e.n.)

  * *Insert trigger window*: 30 (e.n.)

  * *Do you want to store the acquisition data? (y/n)*: y

  * *Do you want binary output file? (y/n)*: y

  * *Write path to acquisition files storing folder*: /data/48PMT/test_python_script_bin/20200629

  * *Do you want to create a rarpd ethers file? (y/n)*: n

  * *Global trigger mode? (y/n, n enables auto-trigger mode)*: y

  * *Do you want to enable some triggers? (y/n)*: n

  * (Alternatively, you can type *python quick_start.py -s13 --t_type -t9000 -p45 -w30 --store -b --path /data/48PMT/test_python_script_bin/20200629 --global*)

* run 'make'

* run 'master_script.sh'

* cd to 'bec_cal_scripts'

* run 'python BEC_go_ext.py -e1' (external trigger)

* cd back to the main scripts folder and then to 'gcu_acq_scripts'

* run the 'python general_soft_reset.py -s13' (to make sure the l1 cache are ok, 13 is the number of GCUs in the setup)

* run the 'source setup_channels.sh' to apply the DAQ parameters to all the GCUs

* run the 'master_readout.sh' script to start the acquisition
