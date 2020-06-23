# awk_cheat_sheet

# awk vs .csv firewall log files
## Summing fields directly from palo alto log files:
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

### to create a list of all applications in descending order indexed by total_bytes sent
$ awk 'BEGIN {FS=","} {a[$15] += $33} END{for (i in a) print i, a[i]}' panlog.txt | awk '{print $2,$1}' | sort -gr >> most_bytes_sent.txt

### to create a list of all applications in descending order indexed by total_bytes recieved
$ awk 'BEGIN {FS=","} {a[$15] += $34} END{for (i in a) print i, a[i]}' panlog.txt | awk '{print $2,$1}' | sort -gr >> most_bytes_received.txt

### to create a list of all applications in descending order indexed by total of the unclear total_bytes? field
$ awk 'BEGIN {FS=","} {a[$15] += $32}' END{for (i in a) print i, a[i]}' panlog.txt | awk '{print $2,$1}' | sort -gr >> most_bytes_in_bytes_field.txt




### to create a new log file with the "destination_address:destination_port" value
$ awk 'BEGIN {OFS=FS=","}{$(NF+1)=$9":"$26}{print $0}' panlog.txt >> panlog_dport.txt

### create a table showing what host and ports you are transacting with the most beyond this firewall
$ awk 'BEGIN {FS=","} {a[$9] += $32} END{for (i in a) print i, a[i]}' panlog.txt | awk '{print $2,$1}' | sort -gr >> session_stats.txt

### do the same as above but broken down by L4 port
$ awk 'BEGIN {FS=","} {a[$68] += $32} END{for (i in a) print i, a[i]}' panlog_dport.txt | awk '{print $2,$1}' | sort -gr >> session_stats_v2.txt



###### Run through these commands with a medium size log file to kind of get a feel for it, substitute $application($15) for $destination_port($26).
###### This will become some of the framework for the generation of weighted link graphs similar to existing tools (think firemon, afterglow, SPML2).
###### The aforementioned tools struggle to give a clear view of whats going on in cases of extreme technical debt and policy bloat.
