# Proxy Tools
Proxy tools and configs to exist in corp land and at home

Preface
----------
Let's face it, having a corporate proxy with NTLM based authentication really sucks.
The forced authentication to an AD backed directory makes sense but creates pain when you work in a terminal and unix/linux world.
Thankfully over the past decade or so some amazing people have created some tooling to help navigate the NTLM proxy landscape.
Among these tools, cntlm is a fantastic solution to this problem.  While cntlm solves 90% of your problems, that last 10% involves turning proxy settings off easily when you are away from the office.
There are some sophisticated ways to solve the in-office/out-of-office settings detection dilemma, but I have opted for a "simple" one which was inspired from a ServerFault post (http://serverfault.com/questions/506046/configure-cntlm-to-use-no-proxy-if-none-are-available).

The solution preseneted here assumes the following:
* Stuck using Windows 7/10/x as your host OS
* Stuck with a corporate NTLM authenticated proxy
* Using Virtualbox for development and virtualization tooling
* Using a Linux VM in Virtualbox
* Desire to seamlessly move in and out of corp offices and home without the need to edit the cntlm.ini or autodetect network locations
* Have local admin priviledges

Solution Design
------------------
* Cntlm configured with:
	1. Corp proxy
	2. Local squid proxy on alternative port
* Linux VM bash profile configured to use the cntlm proxy


Functionality
---------------
* When connected to the corp proxy, the connections will use it via the authentication creds configured in cntlm
* When at home or away from the office, connections will first try the corp proxy, then failover to the 2nd local squid proxy which uses a direct internet connection

This makes things simple and you dont have to change all your proxy settings all the time.  Just use the local cntlm proxy and it will provide seamless functionality for out of office non-proxy direct connections
by failing over to the local squid proxy.  There is a downside to having to install the squid proxy and do the extra configuration, but it works nicely once setup.


Components
--------------
* Cntlm.ini file pre-configured, but you need to add your user account and password hash, domain, and proxy
* Squid.conf pre-configured to bind to port 3129
* Example .bash_profile relevant config parts
* Example yum/apt/dnf config
* Example wget config

Pre-requisites:  
[Cntlm] (https://sourceforge.net/projects/cntlm/files/)  
[Squid for Windows] (http://docs.diladele.com/tutorials/installing_squid_windows/index.html))   


Operations
------------
1. Launch Squid proxy (use Windows services or Squid systray tool)

2. Start Cntlm from administrator enabled command prompt:  net start cntlm









