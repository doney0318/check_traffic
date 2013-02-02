#check_traffic.sh

*The help written by gould. Thanks for Gould Chu.*

##This plugin checks traffic usage and jitter of:
------------------------------------------------
1. a single interface on a single network device
2. multiple interfaces on a single network device
3. interface(s) on a single or multiple network devices
+ The amount of interfaces is not limited. However, aggregation of too many interfaces might make impact on accuracy. 
+ The value of amount less than 8 interfaces is recommended.
4. Both 32-bit and 64-bit counters are supported.

##Usage:
	check_traffic.sh [ -v ] [ -6 ] [ -i Suffix ] [ -F s|S ] [-p N] [ -r ] -V 1|2c|3 ( -C snmp-community | -A "AuthString" (when use snmp v3, U must give the AuthString)) -H host [ -L ] (-I interface|-N interface name) -w in,out-warning-value  -c in,out-critical-value -K/M -B/b

##Options:
1. -h (help)
+ Get the help message
2. -v (verbose)
+ Verbose mode, to debug some messages out to the /tmp directory with log file name check\_traffic.$$.
3. -V (snmp version, protocol=[1|2c|3])
+ Specify the version of snmp
4. -C (community)
+ Specify the Community
5. -H (host)
+ Specify the host
6. -6 (use 64 bit)
+ Use 64 bit counter, ifHC\*  instead of if\*.
7. -r (range)
+ Use Range instead of single value in warning and critical Threshold
8. -I (ifIndex of interface)
+ Specify ifIndex of each interface
9. -N (interface name)
+ Specify the interface name
10. -L (list)
+ List all Interfaces on specify host
11. -B/b (Bps/bps)
+ Switch to Bps(B/s) or bps(b/s), default is -b, bps
12. -K/M (kilo/mega)
+ Switch to K (kilo) or M (mega) Bps|bps, default is -K
13. -w (warning)
+ Set inbound and outbound warning threshold for traffic usage or traffic jitter value
14. -c (critical)
+ Set inbound and outbound critical threshold for traffic usage or traffic jitter value
15. -F (format, type=[s|S])
+ -F s (simple output format)
+ -F S (simpler output format)
16. -p (amount of traffic usage records preserved for comparison=INTEGER)
+ This option is the amount of traffic usage records preserved prior to current traffic usage check. The average of all these preserved values is compared with current check value to detect whether jitter happens. The value (amount) between 4 and 12 is suggested.
17. -i	(suffix=STRING)
+ It's the individual suffix added to the CF/STAT_HIST_DATA if necessary.

##Example:
- Default
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -I 4 -w 200,100 -c 300,200 -K -B
- Or
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -N FastEthernet0/1 -w 200,100 -c 300,200 -K -B
- Or -r to use Range Value Options:
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -I 4 -r -w 200-300,100-200 -c 100-400,50-250 -K -B
- Or
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -N eth0 -r -w 200-300,100-200 -c 100-400,50-250 -K -B
- Or -p N to use Traffic Jitter Options:
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -I 4 -p 8 -w 45,45 -c 55,55
- Or
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -N eth0 -p 8 -w 45,45 -c 55,55

- Or for single host and multiple interfaces checking (in the same host/device) and traffic aggregation:
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -I 2,3,8,9 -w 200,100 -c 300,200 -K -B
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -N FastEthernet0/1,FastEthernet0/2 -w 200,100 -c 300,200 -K -B
- Or -r to use Range Value Options:
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -I 2,3,8,9 -r -w 200-300,100-200 -c 100-400,50-250 -K -B
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -I -N FastEthernet0/1,FastEthernet0/2 -r -w 200-300,100-200 -c 100-400,50-250 -K -B
- Or -p N to use Traffic Jitter Options:
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -I 2,3,8,9 -p 8 -w 45,45 -c 55,55
	./check_traffic.sh -V 2c -C public -H 127.0.0.1 -N FastEthernet0/1,FastEthernet0/2 -p 8 -w 45,45 -c 55,55

- Or for multiple hosts and multiple interfaces checking (in the same host/device) and traffic aggregation:
	./check_traffic.sh -V 2c,1 -C public,private -H 127.0.0.1,192.168.1.1 -I 2,3 -w 200,100 -c 300,200 -K -B
	./check_traffic.sh -V 2c,1 -C public,private -H 127.0.0.1,192,168.1.1 -N FastEthernet0/1,FastEthernet0/2 -w 200,100 -c 300,200 -K -B
- Or -r to use Range Value Options:
	./check_traffic.sh -V 2c,1 -C public,private -H 127.0.0.1,192.168.1.1 -I 2,3 -w 200-300,100-200 -c 100-400,50-250 -K -B
	./check_traffic.sh -V 2c,1 -C public,private -H 127.0.0.1,192.168.1.1 -N FastEthernet0/1,FastEthernet0/2 -r -w 200-300,100-200 -c 100-400,50-250 -K -B
- Or -p N to use Traffic Jitter Options:
	./check_traffic.sh -V 2c,1 -C public,private -H 127.0.0.1,192.168.1.1 -I 2,3 -p 8 -w 45,45 -c 55,55
	./check_traffic.sh -V 2c,1 -C public,private -H 127.0.0.1,192.168.1.1 -N FastEthernet0/1,FastEthernet0/2 -p 8 -w 45,45 -c 55,55

- Or use -L to list all interfaces on the host.
	./check_traffic.sh [ -v ] -V 1|2c|3 -C snmp-community -H host -L
- Or check for snmp v3 device:
	./check_traffic.sh -V 3 -A "-u kschmidt -l authPriv -a MD5 -A mysecretpass -x DES -X mypassphrase" -H 127.0.0.1 -I 4 -w 200,100 -c 300,200 -K -B
- Or
	./check_traffic.sh -V 3 -A "-u kschmidt -l authPriv -a MD5 -A mysecretpass -x DES -X mypassphrase" -H 127.0.0.1 -N eth0 -w 200,100 -c 300,200 -K -B

##Note:
1. If you don't use -K/M -B/b options, default -K -b, corresponding to Kbps.
- Combination:
+ -K -b (kbps, kilobits per second)
+ -K -B (KBps, kilobytes per second)
+ -M -b (Mbps, megabits per second)
+ -M -B (MBps, megabytes per second)
2. Make sure that the check interval greater than 30 Seconds, or modify the Min_Interval's default value as you need. 
3. And, if you want in Verbose mode, use "-v" to check the debug messages in the file /tmp/check_traffic.$$.

## Report bugs to: [cloved](cloved@gmail.com)
Home page: <http://bbs.itnms.info/forum.php?mod=viewthread&tid=767&extra=page%3D1>
Getting help: <http://bbs.itnms.info/forum.php?mod=forumdisplay&fid=10&page=1> or Email to cloved@gmail.com
