Rsyslog Configuration for LibreNMS
==================================

##License
Repository which helps you to configure Rsyslog for LibreNMS and your clients.

The license doesn't apply to external configuration files, see comments in each files to see relative license.

Copyleft (C) Nicolas Simond - 2016

This script is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This script is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this script.  If not, see <http://www.gnu.org/licenses/gpl.txt>


##About this repo
This repository helps you to configure Rsyslog for LibreNMS and your clients.

##Dependencies
Rsyslog

Port 554 (udp/tcp) open on LibreNMS server


##Designed for
Debian 8


##Installation

A french version of this is available here: https://www.abyssproject.net/2016/07/collection-rsyslog-librenms/

A english well formated version of this is available here: https://enter.thewhiterabbit.space/rsyslog-with-librenms/


<h3><span style="text-decoration: underline;">LibreNMS Server</span></h3>

Install Rsyslog:
<pre>apt-get install rsyslog -y</pre>
&nbsp;

Apply necessary configuration:
<pre>cd /etc
wget https://raw.githubusercontent.com/stylersnico/rsyslog/master/etc/rsyslog-server.conf
rm rsyslog.conf &amp;&amp; mv rsyslog-server.conf rsyslog.conf</pre>
&nbsp;

Apply LibreNMS specific configuration:
<pre>cd /etc/rsyslog.d/
wget https://raw.githubusercontent.com/stylersnico/rsyslog/master/etc/rsyslog.d/30-librenms.conf
systemctl restart rsyslog</pre>
&nbsp;

Add Syslog support into LibreNMS:
<pre>cd /opt/librenms
wget https://raw.githubusercontent.com/stylersnico/rsyslog/master/opt/librenms/config-add.php
cat config-add.php &gt;&gt; config.php
rm config-add.php</pre>
&nbsp;

<h3><span style="text-decoration: underline;">Clients</span></h3>

Install Rsyslog:
<pre>apt-get install rsyslog -y</pre>

Apply necessary configuration:
<pre>cd /etc
wget https://raw.githubusercontent.com/stylersnico/rsyslog/master/etc/rsyslog-client.conf
rm rsyslog.conf &amp;&amp; mv rsyslog-client.conf rsyslog.conf</pre>
&nbsp;

You can add support for NGINX, PHP 7 and PHP 5 logs with this command:
<pre>cd /etc/rsyslog.d/
wget https://raw.githubusercontent.com/stylersnico/rsyslog/master/etc/rsyslog.d/lemp.conf</pre>
&nbsp;

&nbsp;

Edit Rsyslog conf with this command:
<pre>nano /etc/rsyslog.conf</pre>
&nbsp;

Replace <em><strong>syslog_server_ip</strong></em> with LibreNMS server's IP or DNS.
<pre>#Remote server
*.* @syslog_server_ip:514</pre>
&nbsp;

Do the same with <em><strong>/etc/rsyslog.d/lemp.conf</strong></em>.

&nbsp;

Restart Rsyslog with this command:
<pre>systemctl restart rsyslog</pre>
&nbsp;

<h3><span style="text-decoration: underline;">Test Rsyslog LibreNMS server</span></h3>
Use TCPdump and see if you see incoming traffic :
<pre>tcpdump -i eth0 udp port 514</pre>
&nbsp;

An example of what you must see:
<pre>root@monitoring:~# tcpdump -i eth0 udp port 514
 tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
 listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
 11:49:28.793109 IP webhost.ragondin.com.45155 &gt; monitoring.ragondin.com.syslog: SYSLOG syslog.info, length: 153
 11:49:28.829808 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG syslog.info, length: 137
 11:49:28.829896 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG syslog.info, length: 71
 11:49:32.558670 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG daemon.error, length: 813
 11:49:33.470379 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG kernel.warning, length: 267
 11:49:36.478635 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG kernel.warning, length: 267
 11:49:38.768606 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG daemon.debug, length: 86
 11:49:38.769138 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG daemon.debug, length: 86
 11:49:42.490359 IP webhost.ragondin.com.45397 &gt; monitoring.ragondin.com.syslog: SYSLOG kernel.warning, length: 267
 ^C
 9 packets captured
 9 packets received by filter
 0 packets dropped by kernel</pre>
&nbsp;

