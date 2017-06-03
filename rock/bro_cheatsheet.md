# Bro Command Cheatsheet

* this document is a jumping poing to change the listed template commands to suit your situation / learning  

* if you find that `bro-cut` is not running, run: `PATH=$PATH:/opt/bro/bin`  

## Basic Triage  

* search the current conn.log for ip address:  
`cat current/conn.log | bro-cut id id id | grep x.x.x.x | sort | uniq -c`  

* tbd
`bro-cut id.resp_p < conn.log | sort | uniq -c | sort -rn | head -n 100`  

* search the current http.log
`cat http.log | bro-cut -d ts id.orig_h id.resp_h id.resp_p method host uri user_agent | grep x.x.x.x`  

* look for a JPGs in the BRO extracted files
`file extract-HTTP-F* | grep JPEG | awk -F: '{print $1}' | xargs -I% mv % %.jpg`  

* AWK Search this, but not that:  
`awk '/x/ && !/y/' infile`  

* List the connections by in increasing order of duration, i.e., the longest connections at the end:  
`awk 'NR > 4' < conn.log | sort -t$'\t' -k 9 -n`  

* Find all connections that are last longer than one minute:  
`tbd`

* Show web servers not running on standard ports:  
`bro-cut service id.resp_p id.resp_h < conn.log | awk '$1 == "http" && ! ($2 == 80 || $2 == 8080) { print $3 }' | sort -u`  

* Show a breakdown of the number of connections by service:  
`bro-cut service < conn.log | sort | uniq -c | sort -n`  

* Show the top 10 destination ports in descending order:  
`bro-cut id.resp_p < conn.log | sort | uniq -c | sort -rn | head -n 10`  

* Show the top 10 talkers:  
`bro-cut id.orig_h orig_bytes < conn.log | sort | awk '{ if (host != $1) {if (size != 0) print $1, size; host=$1; size=0 } else size += $2 } END {if (size != 0) print $1, size}'| sort -k 2 | head -n 10`  

* Unique browsers being used:  
`bro-cut user_agent < http.log | sort -u`  

* Unique file types being transfered:  
`bro-cut mime_type < http.log | sort -u`  

* Show the top 3 websites:  
`bro-cut host < http.log | sort | uniq -c | sort -n | tail -n 3`  

* description
`cat http.log |bro-cut id.orig_h id.orig_p id.resp_h id.resp_p  method uri |grep 46.36.197.5 |wc -l`  

* SSN regex
`egrep "[0-9]{3}[-][0-9]{2}[-][0-9]{4}"`  


## Colorize Text Output

`lesscolor` - Use like less, but provides colorized columns.  (Scrollable output.)  
Example: `lesscolor smtp.log`  

`cm` - Short for â€œcolor meâ€, spits out the provided file with colorized columns.  


## Custom Aliases

* `bro-column` = `"sed \"s/fields.//;s/types.//\" | column -s $'\t' -t"`  
* `bro-awk` = `'awk -F"  "'`  
* `bro-grep` = `{ grep -E "(^#)|$1" $2; }`
* `bro-grep` = grep replacement for piping grep results into bro-cut.  Use just like grep.  
* `topcount` = `{ sort | uniq -c | sort -rn | head -n ${1:-10}; }`
* `bro-column` = `"sed \"s/fields.//;s/types.//\" | column -s $'\t' -t"`  
* `bro-awk` = `'awk -F" "'`
* `fields` = View the field names for a Bro log file.  Useful to get field names to use with bro-cut.
Example:  `fields files.log`  

`topconn` - Used to quickly pull stats from the conn.log  
Usage: `topconn {resp|orig} {proto|service} {tcp|udp|icmp|http|dns|ssl|smtp|"-"}`  
* Examples:  

* Top UDP sources:  
`topconn orig proto udp`  

* Top UDP destinations:  
`topconn resp proto udp`  

* Top HTTP sources:  
`topconn orig proto http`  

* Top DNS destinations:  
`topconn resp service dns`  

* Top source of unidentified protocol:  
`topconn orig proto â€œ-â€`  

`topcount` - Replaces `â€œsort | uniq -c | sort -n | tailâ€`. Displays the top 10 items in descending order.
* Example:  

* View the top services from conn.log:  
`cat conn.log | bro-cut proto | topcount`  

* View the top MIME types from files.log:  
`cat files.log | bro-cut mime_type | topcount`  


## ***Needs Sorting***

cat $1 | sed -e 's/#types\t/ /g' -e 's/#fields\t/ /g' | awk 'BEGIN {FS="\t"};{for(i=1;i<=NF;i++) printf("\x1b[%sm %s \x1b[0m",(i%7)+31,$i);print ""}'  
* topcount() { sort | uniq -c | sort -rn | head -n ${1:-10}; }  
* colorize() { sed 's/#fields\t\|#types\t/#/g' | awk 'BEGIN {FS="\t"};{for(i=1;i<=NF;i++) printf("\x1b[%sm %s \x1b[0m",(i%7)+31,$i);print ""}'; }  
cm() { cat $1 | sed 's/#fields\t\|#types\t/#/g' | awk 'BEGIN {FS="\t"};{for(i=1;i<=NF;i++) printf("\x1b[%sm %s \x1b[0m",(i%7)+31,$i);print ""}'; }  
lesscolor() { cat $1 | sed 's/#fields\t\|#types\t/#/g' | awk 'BEGIN {FS="\t"};{for(i=1;i<=NF;i++) printf("\x1b[%sm %s \x1b[0m",(i%7)+31,$i);print ""}' | less -RS; }  
topconn() { if [ $# -lt 2 ]; then echo "Usage: topconn {resp|orig} {proto|service} {tcp|udp|icmp|http|dns|ssl|smtp|\"-\"}"; else cat conn.log | bro-cut id.$1_h $2 | grep $3 | topcount; fi; }
fields() { grep -E "^#fields" $1; }  
watch -d -n15 "cat current/conn.log | bro-cut -d ts id.orig_h id.resp_h id.resp_p proto service | grep '10.178.*10.223\|10.223.*10.178' | sort -t\t -rk1 -nk2 -nk3 | uniq -f1"  

for i in $(bro-grep 46.36.197.5 http.log | bro-cut cookie_vars); do echo -e "========================="; echo "$i==" | base64 --decode; done  

for i in `cat ssnquery.txt` ;do clamscan -d /var/lib/clamav/ --debug --max-filesize=0 --max-scansize=0 --detect-pua=yes --detect-structured=yes --block-encrypted=yes $i 2>&1 ;done |grep 'SSN'

Show only contents in ()
awk -F'[()]' '{print $2}'

read -t5 -n1 -r -p "Press any key in the next five seconds..." key
if [ $? -eq 0 ]; then
    echo A key was pressed.
else
    echo No key was pressed.

fi
