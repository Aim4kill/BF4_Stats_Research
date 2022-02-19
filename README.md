# BF4_Stats_Research

Here i am trying to understand stat values that are sent from blaze server, more specifically the stat values that starts with c_
Later i may look at other stat values.

# Criteria stat codes

-----------------------------------------------------------  
The criteria stat code format
  1) **c_code_paramX_paramY_statEvent_criteriaType**
  2) **c_paramX_paramY_statEvent_criteriaType**
-----------------------------------------------------------  

**code**, **paramX** and **paramY** can be null, in other words they can be empty string in stat code (yes i know null != "")


# Stat Events

By looking at stat codes (for example "c_aku__shw_g") it is unclear what stat event (in this case "shw"). I took a look at the battlelog apk awards json file (my interest in bf4 stats research appeared when i stumbled upon this file). I understood that these short names are stat events (for example "kwa" => StatEvent_KillWith_X_As_Y), so i looked further to get full stat event list. I searched for these stat events in bf4 ebx files (https://github.com/GreyDynamics/BF4_EBX) and found a place where stat event enum is used (https://github.com/GreyDynamics/BF4_EBX/blob/4e19b479df7294c70744fedf762575bdef9c6db8/Persistence/SpamSettings_offset_.txt). In order to get full stat event list, i needed to dump that particular ebx file, so i did and got the full stat event list. In order to get this stat event list usable i wrote this stat event enum in C#. You can see it down below.

