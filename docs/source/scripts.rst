************
Introduction
************

The present user guide is intended to provide all the informations to start a data acquisition session from a JUNO GCU v2.x based setup.

All the files needed are provided in the repository. the path is *gcu1f3/gcu_v2_scripts*

Some acquisition script may change over time due to GCU/BEC firmware updates or to provide new features. If you find any discrepancy, feel free to ask.

**!!! TO READ: the double dashed line ("- -" without the space) is rendered as a single line ("--"). Keep that in mind when reading the python scripts options. In general if the option is a single letter, a single line is used ("-a"), if the options are multiple letters, double lines are used ("--abc")**

**************
quick_start.py
**************

This is the main script. It allows the user to configure the following DAQ parameters:

1. Threshold; The threshold can be set to a fixed ADC value (which will be the same for every channel) or the user can define a certain number of standard deviation from the baseline (evaluated for each channel).
2. pre-trigger and trigger window; a fixed value (number of nanoseconds / 8) which will be the same for each channel
3. Output file type; the user can decide whether to save or not the waveforms, which type (binary or txt) and the folder path.
4. Trigger type; the user can set the trigger in global mode (and decide which GCU to trigger) or in auto-trigger mode.

The script may be run in the assisted *user guide mode*, or in *flag mode* providing all the different options. To run the script in *user guide mode* provide the script with the *-g* option.

All the questions that the script will ask on screen and the related flag in *flag mode* are listed below:

* **How many GCUs are connected?**: Number of GCU in your setup

  flag: *-s*

* **Do you want a fixed value for threshold? (y/n, n set the threshold based on the std deviation)**: Choose between a fixed value or number of std deviation

  flag: *--t_type*
  
* **Insert threshold**: Threshold value which will be the same for every channel of every GCU

  flag: *-t*, options: *'threhsold value'*
  
* **Insert pre-trigger**: Pre-trigger value which will be the same for every channel of every GCU

  flag: *-p*, options: *'pre-trigger value'*
  
* **Insert trigger window**: Trigger window value which will be the same for every channel of every GCU

  flag: *-w*, options: *'trigger window value'*
  
* **Do you want to store the acquisition data? (y/n)**: if you wish to save the acquired data during the session select *y*, otherwise the data will go to */dev/null*

  flag: *--store*
  
  * If storing, **Do you want binary output file? (y/n)**: select y if you want a binary output file over txt file

    flag: *-b*
    
  * If storing, **Write path to acquisition files storing folder** write the full path to the folder where you want to store the files (example: */home/jdaq/data*)

    flag: *--path*, options: *'acquisition path'*
    
* **Do you want to create a rarpd ethers file? (y/n)**: If you need a RARPD ethers file to assign a IP address to the GCUs of your setup, select y. The file should be later moved to */etc* folder. Typically you only need to do this the first time you are using the setup or the DAQ computer

  flag: *-e*
  
* **Global trigger mode? (y/n, n enables auto-trigger mode)**: if you wish to have a validated trigger via synchronous link, sleect y, otherwise GCUs will self-trigger.

  flag: *--global*
  
  * If Global, **Do you want to enable some triggers? (y/n)**: if you wish to have GCUs firing to the BEC, those GCU must be enables, hence select y

    flag: *--set_auto*
  
    * If wanting to enable triggers, **Write all the GCU numbers that you want to fire to the BEC, separated by a comma (w/o spaces)**: write all all the GCU numbers, starting from 1, that you want to enable. Keep in mind that those GCU must have the synchronous link fully working (i.e. both the TTC and L1A channel calibration must return a plausible value, ranging from 40 to 100). (example: *1,2,5,10,12,13*)

      flag: *--set_auto_which*, options: *'gcu numbers'*

Example: If you wish to run a global trigger acquisition with GCU 1 and 10 firing to the BEC in a setup with 13 GCUs and save the data in binary files:
*python quick_start.py -s13 --t_type -t9000 -p45 -w30 --store -b --path /home/jdaq/data --global --set_auto --set_auto_which 1,10*

Example: If you wish to run a auto trigger acquisition with 3 std deviation threshold in a setup with 13 GCUs and save the data in binary files:
*python quick_start.py -s13 -t3 -p45 -w30 --store -b --path /home/jdaq/data*

Once run, the script will produce a *Makefile*. Just type 'make' and press *Enter*. All the necessary files should have been created. (If a precedent Makefile has been run, before running the new Makefile type 'make clean').

"Make" Generated Files
######################

master_script.sh
----------------

Once the 'run' command has been issued, a *master_script.sh* shall appear in the *gcu_v2_scripts* folder. This scripts will calibrate the synchronous link as well as setting the threshold, pre-trigger and trigger window values for all the GCUs in the setup.

master_readout.sh
-----------------

If you decided to store the acquisition data, in the 'gcu_acq_scripts' folder a 'master_script.sh' appears. Run this shell script to acquire from the setup.

***************
Bec_cal_scripts
***************
In here we find a collection of scripts to setup the BEC. Most of these scripts do not need to be run, as the BEC onfiguration in taken care by the 'master_script.sh', but some of them are new and haven't yet been implemented in the 'quick_start.py' script. I will list every script present in the folder, letting you know if it is implemented in the quick start procedure or not.

Debug Scripts
#############

These scripts has been created fr debug purposes. They are not useful unless there are issues with the link.

* BEC_hit_toggle.py

* BEC_l1a_calibrate.py

* BEC_l1a_eye_scan.py

* BEC_l1a_eye_scan_step.py

* BEC_timing.py

* BEC_ttc_calibrate.py

* BEC_ttc_eye_scan.py

* setup_bec_start_stop.py

Set the BEC external trigger
############################

**BEC_go_ext.py**

This script manages the trigger source of the BEC (i.e., whether the trigger source is from the GCUs or from the external trigger channel)

Script flags:
* *-e*: external trigger
  
  * 1: external

  * 0: GCUs

Usage example: *python BEC_go_ext.py -e1* to enable the external trigger validation

Script NOT included in the quick start procedure

Set the trigger multiplicity and GCU mask
#########################################

**BEC_set_trigger.py**

This trigger enables the multiplicity for global trigger validation.

Script flags:

* *-g*: mask of the GCU connected in the setup with the synchronouslink fully working, in decimal.

* *-m*: multiplicity, in decimal

Usage example: *python BEC_set_trigger.py -g 6651 -m 2* when the GCU connected that have the sync. link working are # 1,2,4,5,6,7,8,9,12,13 and with multiplicity set to 3

Script NOT included in the quick start procedure

Synchronous link calibration
############################

**setup_BEC.py**

To calibrate the synchronous link of the whole system as well as lauch the synchronization procedure.

Script flags:

* *-s*: number of GCUs in the setup

Usage example: *python setup_BEC.py -s13* for a setup wth 13 GCUs

Script included in the quick start procedure


***************
GCU_acq_scripts
***************

This folder includes all the necessary files to start an acquisition session.

Do-not-care scripts
###################

Some python files are import files for the main acquisition scripts.

* *asciichart.py*: needed to display the waveform in real time during the acquisition

* *gcu_aduX_conf.py*: methods for the gcu that the main readout scripts uses (e.g., acquire FIFO data, slow control data, etc.)

* *gcu_env.py*: same as *gcu_aduX_conf.py* but used in the GCU BX version

Debug scripts
#############

* *get_trigger_rate.sh*: reads a txt acquisiton file during acquisition to retrieve the trigger rate

* *auto_trigger.py*: sends trigger via ipbus at a user chosen gcu and channel at a user chosen frequency

* *pulse_presence_monitor.py*: Needed to check whether all gcu presented a pulse when triggered. To be used in autotrigger.

* *cache_monitor.py*: Needed to check the status of the l1 cache in all the GCUs in the system

DAQ Parameters with the GUI
###########################

**gui_control/gui_control.py**

Folder containing the *gui_control.py* script used to set the DAQ parameters for a single gcu and channel.

Script flags:

* *-s*: gcu number

Usage example: *python gui_control.py -s10* to control the DAQ parameters of GCU # 10.

INI Configuration files folder
##############################

**ini_files**

Collection of the ini files for all the GCUs in the setup. I suggest to let the quick start procedure generate them

Readout script
##############

**gcu_readout.py**

Main acquisition script. Start the acquisition for a single GCU and Channel.

Script Flags:
* *-c*: path to ini file to select gcu and channel
  
* *-t*: packet check online showing some metadata real time like timestamp, trigger number, window average value, etc.

  * *True*

  * *False*
    
* *-p*: plot the waveform together with the metadata

  * *True*
    
  * *False*
    
* *-s*: When *-p* is *True*, plot one waveform out of *s*
  
* *-b*: save the data in binary files instead of txt

  * *True*

  * *False*

Usage example: *python gcu_readout.py -c ini_files/gcu_10_ch1.ini -t True -p True -s1 -b True* when acquiring from GCU #10, channel #1 plotting the waveform online

Reset script
############

**general_soft_reset.py**

This script needs to run first when GCUs are rebooted. If not, some GCUs may not have the l1 cache synchronized, having the acquired data not time correlated (showing pretty much just noise during acquisition)

Script flags:
* *-s*: number of GCUs in the setup

Usage example: *python general_soft_reset.py -s13* for a setup with 13 GCUs

Configure the whole GCU setup
#############################

**setup_channels.sh**

Shell script that set ups all the gcus and channels with the ini files DAQ parameters (thresholds, trigger windows, etc.) as well as the trigger condition (global trigger, auto trigger)

This script is generated by the quick start procedure

The shell scripts uses the following scripts:

Configure a single channel
**************************

**setup_channel.py**

Takes the ini file and sets the corresponding DAQ parameters

Script Flags:

* *-c*: path to ini file

Usage example: *python setup_channel.py -c ini_files/gcu_6_ch1.ini* to configure GCU # 6, channel # 1

Set the trigger
***************

**setup_trg.py**

Sets the trigger condition

Script flags:

* *-b*: if true, trigger goes to BEC for validation, otherwise that GCU is in auto trigger

* *-e*: if true, GCU is only triggered by IPBus (but if *-b* is present, BEC validation messages still triggers the acquisition).

* *-s*: gcu number

Usage example: *python setup_trg.py -b -s 2* to set GCU # 2  in global trigger 

Start a global acquisition
##########################

**master_readout.sh**

Generated by the quick start procedure only if the user said he wanted to store the data, starts an acquisition session.

