# ClusterLogic
This project is designed to help linux cluster implementations to be more flexible and suitable for complex scenarios


[Global]
plugins=check_drbd
plugins=check_drbd,check_controller,check_pgsql,check_gw
order=random,order_by_id

[Logic=network]
id=1
case=hardware
reries=3
members=check_drbd,check_fcc
result=0        # 0=all up, N+1=at least one up
negate=0        # 0=disable 1=enable
status=0        # 0=disable 1=enable

[Logic=hardware]
id=2
reries=3
members=check_drbd,check_fcc
result=0
negate=0
status=1


[check_drbd]
id=1
dir=/etc/keepalived/plugins
plugin=check_drbd
params=MASTER
status=0	# 0=enable, 1=disable
exit=0		# what result to expect
retries=1	# how much retries for each check
fail=3		# what consider fails
interval=5m	# when to run
handler1=1	# 0=disable / 1=enable
handler1_plugin=/etc/keepalived/handlers/ctrl_drbd
handler1_params_exec="FAULT"
handler1_result_output="failed"


[check_controller]
id=2
dir=/etc/keepalived/plugins
plugin=check_fcc
params=MASTER
status=1
retries=10
sleep=0

[check_gw]
id=2
dir=/etc/keepalived/plugins
plugin=check_ping
params=-H 192.168.1.1
status=0
