# PROMETHEUS FUSE EBPF EXPORTER

This script monitors I/O performance of FUSE file systems. This includes async read and write requests.
By default it is a Prometheus exporter (port 9500).

For quick sanity checking, it can also monitor xfs or ext4 file systems (but async requests are not supported).
The script can also print a stream of measurement events, or every few seconds an almost unprocessed dump of the histogram data that has been collected.

Prometheus output looks as below.

Times are reported in ms (Prometheus mis-formats the shorter times too much if they are in seconds).

File names are split into 3 groups: "disk", "cinder*" and "rest".

File operations are "open", "read", "write" and "fsync".

The histogram buckets are cumulative, i.e. the bucket for meaurements "less than or equal to 0.016 ms" also counts the
meaurements faster than 0.008 ms, 0.004 ms, etc.

The number of buckets is fixed. This is because Prometheus doesn't really understand the structure of histograms very well.
If multiple monitored nodes report different buckets, the totals are generated in a naive way which is incorrect for histograms. 

```
# HELP process_virtual_memory_bytes Virtual memory size in bytes
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 239751168.0
# HELP process_resident_memory_bytes Resident memory size in bytes
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 87580672.0
# HELP process_start_time_seconds Start time of the process since unix epoch in seconds.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1520947070.16
# HELP process_cpu_seconds_total Total user and system CPU time spent in seconds.
# TYPE process_cpu_seconds_total counter
process_cpu_seconds_total 157.25
# HELP process_open_fds Number of open file descriptors.
# TYPE process_open_fds gauge
process_open_fds 86.0
# HELP process_max_fds Maximum number of open file descriptors.
# TYPE process_max_fds gauge
process_max_fds 1024.0
# HELP ebpf_fuse_req_latency_ms Latency of various FUSE operations
# TYPE ebpf_fuse_req_latency_ms histogram
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.001",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.002",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.004",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.008",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.016",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.032",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.064",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.128",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.256",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.512",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1.024",operation="write"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="2.048",operation="write"} 8505.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="4.096",operation="write"} 22485.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="8.192",operation="write"} 23884.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="16.384",operation="write"} 24129.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="32.768",operation="write"} 24754.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="65.536",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="131.072",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="262.144",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="524.288",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1048.576",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="2097.152",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="4194.304",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="8388.608",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="16777.216",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="33554.432",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="67108.864",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="134217.728",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="268435.456",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="536870.912",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1073741.824",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="+Inf",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_count{filename="rest",operation="write"} 24875.0
ebpf_fuse_req_latency_ms_sum{filename="rest",operation="write"} 78006.547
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.001",operation="write"} 1737341.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.002",operation="write"} 1737341.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.004",operation="write"} 1737341.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.008",operation="write"} 1737341.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.016",operation="write"} 1737341.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.032",operation="write"} 1767203.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.064",operation="write"} 2586122.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.128",operation="write"} 4412068.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.256",operation="write"} 5081416.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.512",operation="write"} 5087992.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1.024",operation="write"} 5098420.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2.048",operation="write"} 5115389.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4.096",operation="write"} 5168157.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8.192",operation="write"} 5193000.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16.384",operation="write"} 5196351.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="32.768",operation="write"} 5196927.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="65.536",operation="write"} 5197183.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="131.072",operation="write"} 5197526.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="262.144",operation="write"} 5197678.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="524.288",operation="write"} 5197847.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1048.576",operation="write"} 5197847.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2097.152",operation="write"} 5198584.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4194.304",operation="write"} 5199898.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8388.608",operation="write"} 5202272.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16777.216",operation="write"} 5206120.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="33554.432",operation="write"} 5209531.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="67108.864",operation="write"} 5211994.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="134217.728",operation="write"} 5214458.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="268435.456",operation="write"} 5216953.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="536870.912",operation="write"} 5216953.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1073741.824",operation="write"} 5216953.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="+Inf",operation="write"} 5216953.0
ebpf_fuse_req_latency_ms_count{filename="disk",operation="write"} 5216953.0
ebpf_fuse_req_latency_ms_sum{filename="disk",operation="write"} 17284837402.133
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.001",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.002",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.004",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.008",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.016",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.032",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.064",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.128",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.256",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.512",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1.024",operation="open"} 14431.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2.048",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4.096",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8.192",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16.384",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="32.768",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="65.536",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="131.072",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="262.144",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="524.288",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1048.576",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2097.152",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4194.304",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8388.608",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16777.216",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="33554.432",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="67108.864",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="134217.728",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="268435.456",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="536870.912",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1073741.824",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="+Inf",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_count{filename="disk",operation="open"} 16097.0
ebpf_fuse_req_latency_ms_sum{filename="disk",operation="open"} 41678.061
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.001",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.002",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.004",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.008",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.016",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.032",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.064",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.128",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.256",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.512",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1.024",operation="open"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="2.048",operation="open"} 1837.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="4.096",operation="open"} 2164.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="8.192",operation="open"} 2164.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="16.384",operation="open"} 4219.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="32.768",operation="open"} 5034.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="65.536",operation="open"} 5094.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="131.072",operation="open"} 5094.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="262.144",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="524.288",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1048.576",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="2097.152",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="4194.304",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="8388.608",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="16777.216",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="33554.432",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="67108.864",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="134217.728",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="268435.456",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="536870.912",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1073741.824",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="+Inf",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_count{filename="rest",operation="open"} 5095.0
ebpf_fuse_req_latency_ms_sum{filename="rest",operation="open"} 49369.314
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.001",operation="fsync"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.002",operation="fsync"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.004",operation="fsync"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.008",operation="fsync"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.016",operation="fsync"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.032",operation="fsync"} 764.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.064",operation="fsync"} 24475.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.128",operation="fsync"} 42443.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.256",operation="fsync"} 42869.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.512",operation="fsync"} 42869.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1.024",operation="fsync"} 42890.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2.048",operation="fsync"} 139196.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4.096",operation="fsync"} 835102.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8.192",operation="fsync"} 844181.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16.384",operation="fsync"} 847264.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="32.768",operation="fsync"} 847669.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="65.536",operation="fsync"} 847855.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="131.072",operation="fsync"} 847962.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="262.144",operation="fsync"} 847989.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="524.288",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1048.576",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2097.152",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4194.304",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8388.608",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16777.216",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="67108.864",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="134217.728",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="268435.456",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="536870.912",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1073741.824",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="+Inf",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_count{filename="disk",operation="fsync"} 847998.0
ebpf_fuse_req_latency_ms_sum{filename="disk",operation="fsync"} 2011127.908
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.001",operation="read"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.002",operation="read"} 1429.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.004",operation="read"} 20786.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.008",operation="read"} 41121.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.016",operation="read"} 49625.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.032",operation="read"} 51516.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.064",operation="read"} 52288.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.128",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.256",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="0.512",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1.024",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2.048",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4.096",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8.192",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16.384",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="32.768",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="65.536",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="131.072",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="262.144",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="524.288",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1048.576",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="2097.152",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="4194.304",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="8388.608",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="16777.216",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="33554.432",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="67108.864",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="134217.728",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="268435.456",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="536870.912",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="1073741.824",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_bucket{filename="disk",le="+Inf",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_count{filename="disk",operation="read"} 60385.0
ebpf_fuse_req_latency_ms_sum{filename="disk",operation="read"} 22802.408
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.001",operation="read"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.002",operation="read"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.004",operation="read"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.008",operation="read"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.016",operation="read"} 0.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.032",operation="read"} 912.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.064",operation="read"} 3393.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.128",operation="read"} 5185.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.256",operation="read"} 5231.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="0.512",operation="read"} 5231.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1.024",operation="read"} 5231.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="2.048",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="4.096",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="8.192",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="16.384",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="32.768",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="65.536",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="131.072",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="262.144",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="524.288",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1048.576",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="2097.152",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="4194.304",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="8388.608",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="16777.216",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="33554.432",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="67108.864",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="134217.728",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="268435.456",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="536870.912",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="1073741.824",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_bucket{filename="rest",le="+Inf",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_count{filename="rest",operation="read"} 7737.0
ebpf_fuse_req_latency_ms_sum{filename="rest",operation="read"} 5083.806
# HELP ebpf_fuse_req_sync_outstanding The number of sync I/O requests that did not complete after being registered
# TYPE ebpf_fuse_req_sync_outstanding gauge
ebpf_fuse_req_sync_outstanding 0.0
# HELP ebpf_fuse_req_async_outstanding The number of async I/O requests that did not complete after being registered
# TYPE ebpf_fuse_req_async_outstanding gauge
ebpf_fuse_req_async_outstanding 0.0
```
