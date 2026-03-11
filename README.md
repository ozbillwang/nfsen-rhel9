# nfsen-rhel9
playbook to install nfsen on rhel9

```
# make sure you have updated the inventory file
ansible-playbook -i inventory.ini --limit nfsenserver-1  nfsen_setup/main.yml
```
### customized packages

NfSen is hard to run on Red Hat because it’s an old, interactive, Debian‑era tool that doesn’t match Red Hat’s modern security model, directory layout, or automation standards.

I did some changes directly and packaged it as files/nfsen-1.3.8-rhel9.tar.gz

Here are the difference, based on original version 1.3.8

```
diff -rw nfsen-1.3.8-rhel9 nfsen-1.3.8                                                                                                                                         [master]
diff -rw nfsen-1.3.8-rhel9/install.pl nfsen-1.3.8/install.pl
41,42d40
< use lib "./lib";
<
98,101c96,99
< 		#chomp($ans = <STDIN>);
< 		#if ( length $ans ) {
< 		# 	$whichperl = $ans;
< 		#}
---
> 		chomp($ans = <STDIN>);
> 		if ( length $ans ) {
> 			$whichperl = $ans;
> 		}
diff -rw nfsen-1.3.8-rhel9/libexec/NfProfile.pm nfsen-1.3.8/libexec/NfProfile.pm
1041c1041
< 	mkdir "$dir" unless -d $dir or
---
> 	mkdir "$dir" or
1197,1198d1196
<
< 	no strict 'refs';  # <--- ADD THIS LINE MANUALLY HERE
diff -rw nfsen-1.3.8-rhel9/libexec/NfSenRC.pm nfsen-1.3.8/libexec/NfSenRC.pm
97,100c97
< 	my $common_args = "-D -p $port -u $uid -g $gid $buffer_opts $subdirlayout -P $pidfile $ziparg $extensions";
<
< 	print STDERR "DEBUG common_args: $common_args\n";
<
---
> 	my $common_args = "-w -D -p $port -u $uid -g $gid $buffer_opts $subdirlayout -P $pidfile $ziparg $extensions";
diff -rw nfsen-1.3.8-rhel9/libexec/NfSenRRD.pm nfsen-1.3.8/libexec/NfSenRRD.pm
76c76
< 	if ( $rrd_version >= 1.2 && $rrd_version < 1.9 ) {
---
> 	if ( $rrd_version >= 1.2 && $rrd_version < 1.6 ) {
```
