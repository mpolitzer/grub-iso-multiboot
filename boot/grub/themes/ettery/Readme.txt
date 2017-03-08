+-----------------------------------+
+     GRUB2 THEME: Ettery           +
+     Author: Dacha204              +
+     Version: 1.1                  +
+-----------------------------------+

Web: github.com/Dacha204/grub2-themes-Ettery
>>>>> README FILE <<<<<<

** ABOUT **

Grub2 theme based on:
    - ZorinOS Grub2 theme 
    - Vimix by vinceliuice

Designed primary for 1920x1080 (wide 16:9) resolution 
but should work for common resolution as well (between 800x600 and 1600x1200).
Background image found on imgur.com


Use grub-customizer further customize grub, to add/remove/group/sort grub entry...
** Installation instructions ***

First you're going to determine what resolutions grub supports.
!!! USE ONLY SUPPORTED RESOLUTIONS
# Method 1
    - install/build hwinfo 
    - start it with
        sudo hwinfo --framebuffer

# Method 2
    - reboot
    - open up the command line by pressing 'C'
    - enter
        vbeinfo
        
The outputs may be different. So after you find out your supported resolutions, write down
or remeber your highest supported resolution. We will need it for further installation process.

Now we begin with install process. There are two ways: Automatic using install.sh script from
original ZorinOS theme and Manual.

For Version 1.0 I do not recommend using script because it's not tested. I recommend manual install.
Just follow the instructions.

[Automatic - scripted] - NOT RECOMMENDED FOR VERSION 1.0 and 1.1 - NOT TESTED
    - just run install.sh with root and follow instructions
    
[Manual install]
    - copy entire Ettery folder in /boot/grub/themes/ (/boot/grub2/themes/, /grub/themes/)
        sudo cp -r Ettery /boot/grub/themes/
    - open /etc/default/grub in text editor with root
        sudo gedit /etc/default/grub
        
    - add/change folowing lines:
        GRUB_GFXMODE="1920x1080" #change with your supported resolution
        GRUB_GFXPAYLOAD_LINUX="keep"
        GRUB_THEME="/boot/grub/themes/Ettery/theme.txt"

    - save changes
    - update the grub:
        sudo update-grub (or sudo update-grub2, or sudo grub2-mkconfig -o /boot/grub/grub.cfg)
    
    - enjoy. (reboot to see changes)

** Disable theme **
    - open /etc/default/grub with text editor with root
        sudo gedit /etc/default/grub
    - remove/comment line (add # at beginning)
        GRUB_THEME="/boot/grub/themes/Ettery/theme.txt" to #GRUB_THEME="/boot/grub/themes/Ettery/theme.txt"
    - save file
    - update grub
        sudo update-grub (or sudo update-grub2, or sudo grub2-mkconfig -0 /boot/grub/grub.cfg)
        
** Unistall theme **
    - First disable theme (instructions above)
    - Remove Ettery folder from /boot/grub/themes with root
        sudo rm -r /boot/grub/themes/Ettery

        
