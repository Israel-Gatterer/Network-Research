#!/bin/bash

#Project Network research

# Verify if you are root.
#Credit enabling root's command -  https://www.kali.org/docs/general-use/enabling-root/#enabling-root-for-gnome-and-kde-login

#Installing function for applications
function INSTL()
{	
		#Installing other tools for automatic deacorative output:
		#Installing figlet
		#apt install figlet 1>/dev/null
		#Installing  lolcat
		#apt install lolcat 1>/dev/null
		
	# [1] installing nipe
	if [ -d /home/kali/nipe ]
	then
		echo "Nipe is already installed :)"  | lolcat -af
		echo "-----------------------------------------------------"  | lolcat -af
	else
		echo "Nipe is not installed .. installing now! " | lolcat -af
		# Downloading
		cd /home/kali 1>/dev/null 
		git clone https://github.com/htrgouvea/nipe && cd nipe 1>/dev/null 
		echo "-----------------------------------------------------" | lolcat -af
		# Installing libs and dependencies
		cpanm --installdeps -y 1>/dev/null
		echo "-----------------------------------------------------" | lolcat -af
		# Nipe must be run as root
		perl nipe.pl install 1>/dev/null
		echo "$(figlet "nipe is installed")" | lolcat -af
		echo "-----------------------------------------------------" | lolcat -af
		echo " :):):):):):):):):):):):):):):):):):):):):):):):):):):):):):) " | lolcat -af

	fi
	
	# [2] installing ssh.
	if [ -d "/usr/share/doc/sshpass" ]
	then
		echo "SSHPASS is now installed!! " | lolcat -af
		echo "-----------------------------------------------------" | lolcat -af
	else
		echo "[*] sshpass is not installed -> [*] Installang sshpass" | lolcat -af
		echo " "
		# Downloading
		apt-get install sshpass 
		echo "sshpass installed :) " | lolcat -af
		echo "-----------------------------------------------------" | lolcat -af
	fi
	
	# [3] installing geoipbin.
	if [ -d "/usr/share/GeoIP" ]
	then
		echo "geoipbin is installed" | lolcat -af
	else
		echo "geoipbin isn't installed -> [*] Installing geoipbin" | lolcat -af
		echo " "
		apt-get install geoip-bin 
		echo "geoipbin is installed :) " | lolcat -af
		echo "-----------------------------------------------------" | lolcat -af
		echo " " | lolcat -af
		
	fi
} 

# Function checking for anonymous
	EXTIP=$(curl -s ifconfig.co)

	CNT=$(geoiplookup $EXTIP | awk '{print $4}' | sed 's/,//g')

function ANON()
{	
  if    [ "$CNT" != "IL" ]
	then
		echo "  You are" | lolcat -af
		echo "$(figlet "ANONIMOUS :) ")" | lolcat -af
		echo "
		     .... NO! ...                  ... MNO! ...
   ..... MNO!! ...................... MNNOO! ...
 ..... MMNO! ......................... MNNOO!! .
.... MNOONNOO!   MMMMMMMMMMPPPOII!   MNNO!!!! .
 ... !O! NNO! MMMMMMMMMMMMMPPPOOOII!! NO! ....
    ...... ! MMMMMMMMMMMMMPPPPOOOOIII! ! ...
   ........ MMMMMMMMMMMMPPPPPOOOOOOII!! .....
   ........ MMMMMOOOOOOPPPPPPPPOOOOMII! ...
    ....... MMMMM..    OPPMMP    .,OMI! ....
     ...... MMMM::   o.,OPMP,.o   ::I!! ...
         .... NNM:::.,,OOPM!P,.::::!! ....
          .. MMNNNNNOOOOPMO!!IIPPO!!O! .....
         ... MMMMMNNNNOO:!!:!!IPPPPOO! ....
           .. MMMMMNNOOMMNNIIIPPPOO!! ......
          ...... MMMONNMMNNNIIIOO!..........
       ....... MN MOMMMNNNIIIIIO! OO ..........
    ......... MNO! IiiiiiiiiiiiI OOOO ...........
  ...... NNN.MNO! . O!!!!!!!!!O . OONO NO! ........
   .... MNNNNNO! ...OOOOOOOOOOO .  MMNNON!........
   ...... MNNNNO! .. PPPPPPPPP .. MMNON!........
      ...... OO! ................. ON! .......
         ................................
		" | lolcat -a
		echo "-----------------------------------------------------" | lolcat -af
		
	else
		echo "You are not anonymous.[*] Starting nipe services" | lolcat -af
		#starting nipe services
		cd /home/kali/nipe
		perl nipe.pl start 
		perl nipe.pl stop 
		perl nipe.pl restart 
		perl nipe.pl status
		echo "Now you are..." | lolcat -af
		echo "
		     .... NO! ...                  ... MNO! ...
   ..... MNO!! ...................... MNNOO! ...
 ..... MMNO! ......................... MNNOO!! .
.... MNOONNOO!   MMMMMMMMMMPPPOII!   MNNO!!!! .
 ... !O! NNO! MMMMMMMMMMMMMPPPOOOII!! NO! ....
    ...... ! MMMMMMMMMMMMMPPPPOOOOIII! ! ...
   ........ MMMMMMMMMMMMPPPPPOOOOOOII!! .....
   ........ MMMMMOOOOOOPPPPPPPPOOOOMII! ...
    ....... MMMMM..    OPPMMP    .,OMI! ....
     ...... MMMM::   o.,OPMP,.o   ::I!! ...
         .... NNM:::.,,OOPM!P,.::::!! ....
          .. MMNNNNNOOOOPMO!!IIPPO!!O! .....
         ... MMMMMNNNNOO:!!:!!IPPPPOO! ....
           .. MMMMMNNOOMMNNIIIPPPOO!! ......
          ...... MMMONNMMNNNIIIOO!..........
       ....... MN MOMMMNNNIIIIIO! OO ..........
    ......... MNO! IiiiiiiiiiiiI OOOO ...........
  ...... NNN.MNO! . O!!!!!!!!!O . OONO NO! ........
   .... MNNNNNO! ...OOOOOOOOOOO .  MMNNON!........
   ...... MNNNNO! .. PPPPPPPPP .. MMNON!........
      ...... OO! ................. ON! .......
         ................................
		" | lolcat -a
		echo "$(figlet "ANONIMOUS :)" )" | lolcat -af
		echo "-----------------------------------------------------" | lolcat -af
		
	fi
}

#Requiring details for choosing vps 
function VPS()
{
	echo "Enter username: " | lolcat -af
	read USR
	echo ""
	echo "Enter ip address: " | lolcat -af
	read IP
	echo " "
	echo "Enter password: " | lolcat -af
	read PASS
	echo " "
	echo "Enter an ip range or ip address to scan: " | lolcat -af
	read RNG
	echo " "
	#Starting VPS communication
	sshpass -p "$PASS" ssh -o StrictHostKeyChecking=no $USR@$IP "nmap -Pn $RNG -p 22,80,443"
}

if [ "$(whoami)" == "root" ]
	then
		echo "You are root" | lolcat -af
		echo "-----------------------------------------------------" | lolcat -af
	else
		echo "You are not root. [*] Starting connection as a root" | lolcat -af
		sudo apt -y install kali-root-login 
		sudo su
		kali
		echo "Now you are root :)" | lolcat -af
		echo "-----------------------------------------------------" | lolcat -af
fi

#Thanks & a little jock 
function TNKS()
{
	figlet " Thank you for watching" | lolcat -af
echo "
dddOkkO0OKOK00kOO0d0OO00OkOxxdddddddddddxxkkxxoooooolllllldxkkxxkkkkkO
dxxkkkOkkkkOkOkkOkxkkkkxxxxxxxxxxxxxollccllll:;;;;:llllllodkkkkkOOOOOO
xxxxkkkkkkkkkkkkkxoooxkkkxdodoodoolc;''.'....... .,:clooooxOOOOO00O000
xxkkkkkkooookkkkkxollxkkkkxol;;col,................colooookO000O0K0KKK
xkkkkkkkloloxkkkkkdxdkkkOkkkkol;:;';ccccc;,',;,'...:dkxdddk0KXK0KKKKKK
xxxxxkkOOOOOOOO000000000000OOkc.''cooddddddxxxxxd:';:okdxdOKXXK0KXKKKK
kxxxxkOOOOOO00000000000000000d,.'cooooddddxxxkkkOOd;:oOxxxOXXXXKKXKKKX
kxxxxkOOOO0000000000000000000:.'coooodxxxxxxxxxkO0KxokOxxxkXXNXKKXKKKX
kxoxxkOOOOO0O000000000000000Od''loooooodxxxxkkkO0KXXd00xxxkXXNXKKXXKKX
dooodkO0000000K0O0OKKKKKKK000k,.cl;;,,,:lddxkxdoookNdO0xddxXNNNKKXXXXX
xdollkO00000KKK00OO0KKKKKKK00O;.cc;,',,',cdkkl;;cdOKdkOxocdXNNNKKKXXXX
kdoodO00000KK00000OOOKK00000Ol,'olcc:::::coxOdccokOXkOoodooKNNNKKKXKKK
kkxxxkkkkkkkkkkkkkkkkxxxxxxxxoc:ollloooccokkKKkkkk0Xkxlcooo0NXNX0KKKKK
kxkkkOOOOOOOOOOOOOOOOOOOOOOkkkdlllllooccldxkKKkkkOKXO:c::;:OXXNX0KKKKK
oxxkkkOOOOxdxkOOOOOkkkkkoOOOkkkdlcccllc:;;lxxkkkkOKK:;;c;;,kXXXX00KKKK
xxxxkkkkxoOOKX0ocokolxkdlkkkkxdklc:cc:;::cloddxdkO0c,,.:ld;dXXXK00KKKK
dxdxxxxolox0XN0c''ccllol'ooo:::ll:::ll;;:loolloxO0O.';'';:,oKXXK00K00K
oxxxxoooollOKNOlccododxkkKkxo;;'c;,::ccccccodkOO00xkkkkkkkkOKXKK00K000
looodll:cccx0X0c,;c;;o0N0Xl,,;..c:,';cllllldxkO00ddoddkkkkkk0KKK00K000
dolloxdooolldkkdxxddddOXXKk:;. .;;,,..;:coodxxkk0'......::cokKKK000000
oo::codxkkkkkkkxdll;;;dx:;...   .,,,,'..',;:cokkd....  ......,;clx0000
:;;;codxxxxdcllol,..''...  .     ',;;;',;:lodkxx'       .........;lk00
'loolll:'::::clo:,'.....         .',::;,;;clodo;          .......'';oO
oddxdxxddoooo:,...........  .  .:oo:,:cc:ccllo:             ........,:
ddxxxxxxkkkxcclloodddxxkkkxxlcoOO00K0xdoccccl:.              ........'
xdxxxkkkkkl',xxkOO00KKXXXKK0OOOkolxkk0KKOl,..                  .......
xxxxxkkkk;..;xxkOO000KXXXKOkkO0K0xolok0KKKl.                     .....
xdxxxxkk;...:o;;ox:O0KXXK0xxxxxxkkO00KKKKKKk.                      ...
xdxxxxx:....clc;:lcx0KKX00kdl::clloxOOO00KKK0l.                      .
ddxxxxl.....::ooolddoOKK0OxllooddxxxxkkOOOO00Kx'                      
dddxxd......:',,.,;'.k0KOkdlcclodk000OOkkkkkkOOOd;.                   
dddxxc..... looxxdkOO000Okdlc;;:ccdkkkkxkkxxxxkkkkkd:.                
doddd'......oodxkkOOO000Okxlcllllcloddxddxxxxxxdoodxkkd:. .           
doddl....  .oodxkkOOO000Oxol::;:codxdddddddddddddddddddxxo:.  ..     .
ooddc.......oodxxkkOO000kokkxxdoccloooooooooooooddddddddddxd;..... ..,

" | lolcat -a

echo " YOU HAVE BEEN HACKED !!!" | lolcat -af
echo "


"
echo " Just kidding ;) LOL " | lolcat -af
echo " Have a good day" | lolcat -af
}

# Execute the functions
INSTL
ANON
VPS
TNKS

