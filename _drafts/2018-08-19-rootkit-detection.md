---
title: Proactive detection of kernel-mode rootkits
description: Rootkit detection
category: papers, rootkits
---

The sophistication of malicious software (malware) used to break the computer security has increased exponentially in the last years.  
Frequently, malware is hidden into a computer by software components called rootkits.  
Therefore, early detection of rootkits is of primary importance to avoid the uncontrolled operation of malware.  
Most of current techniques for rootkit detection only allow a late detection after the malware has already been hidden by a rootkit.  
In this paper, a new technique is presented that enables the proactive detection of rootkits while they are hiding malware, and therefore, allowing that hiding can be avoided.  
The technique has been designed for rootkits that operate in kernel-mode.  
This rootkits are particularly difficult to detect because both the detector and the rootkit are executed with the same privileges.  
This technique can be used to improve the detection capabilities of intrusion detection and prevention systems.

[CLick here download the paper in PDF][1]

[1]:{{ site.url }}/ARES.pdf
