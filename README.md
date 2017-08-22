laptop-mode-tools.nouveau
=================

 This git repository provides the proper configuration files and modules 
 (modules/nouveau-performance and conf.d/nouveau-performance.conf), for power management 
 of a nouveau-driven card (currently limited to performance level checking/switching, where possible). 
 This files are meant to be used within the laptop-mode framework.

# How to install and configure

 Download the tarball of this git archive (or clone the git repository) and unpack it directly into /
 (or manually copy the two files as they are).

 Edit /etc/laptop-mode/conf.d/nouveau-performance.conf and set the two most important parameters:

 CONTROL_NOUVEAU_PERFORMANCE=1   # this enables the module



 NOUVEAU_DRM_CARD=0             # 0 should be fine in most cases, check /sys/class/drm/cardX/device/name to be "nouveau"


 and set the module behaviour in {BATT,[NO]LM_AC}_NOUVEAU_DRM_COMMANDS variables by substituting the [value]
 parameter with the desidered performance level number.

 Consider that nVidia(C) cards can have up to 4 performance level: the first (and the lower) one is generally 
 the "powersave" state; the second corresponds to the boot one and the remaining two are for higher performance 
 requirements.
 For card with less performance levels things get obviously simpler. 
 If you've got just one performance level on your card, you shouldn't be needing this module at all. 
 Set the *_COMMANDS parameter to reasonable values, or in order to fit your needs.

laptop-mode-tools.pcie_aspm
=================
 This package provides also a simple module for the pcie-aspm link tuning. Your laptop can benefit from it, 
 expecially when working aside the nouveau module, indeed.

# How to install and configure

 The installation procedure of the two files (etc/laptop-mode/conf.d/pcie_aspm.conf and 
 usr/share/pcie_aspm/module/pcie_aspm) is the same as nouveau's.

 Its working mechanism is simpler, as the ASPM comes up with 3 (2) performance states, 
 performance, powersave and default, tunable via /sys/class/modules/pcie_aspm/policy 
 
 This module selects one of them, depeding on the state of your laptop.
 
  BATT_PCIE_ASPM_COMMAND="echo powersave"     # default values are usually fine
  
  NOLM_PCIE_ASPM_COMMAND="echo performance"
  
  LM_PCIE_ASPM_COMMAND="echo default"
 
  CONTROL_PCIE_ASPM=[0,1]                     # Enables/disables the module
 
# Enabling power management

 Restart laptop-mode when the module is configured:

 \# systemctl restart laptop-mode.service 

 or 

 \# /etc/rc.d/laptop-mode restart

 Screen should "zap" for an instant (actually that's what nouveau does when recklocking the card), and that means that power management for the current laptop state
 has been started.

# Debugging

 You could increase verboseness of the laptop-mode subsystem by setting VERBOSE=1 in /etc/laptop-mode/laptop-mode.conf
 or enable module's debugging with DEBUG=1 in /etc/laptop-mode/conf.d/<module>.conf

 Check /var/log/boot.log for the output messages.
 
# License

 This package is distributed under the version 2 of the General Public License.
