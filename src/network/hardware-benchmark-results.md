# Benchmark Results

## Definitions
`D2E` Device to Endpoint - Device connected to endpoint and `iperf3` between the two.

`E2E` Endpoint to Endpoint - Device connected to two endpoints on different subnets. `iperf3` between two endpoints through device.

`WG D2D` Device to Endpoint over WG - Device connected to endpoint with `wg` tunnel and `iperf3` over `wg`.

`WG E2E`  Endpoint to Endpoint over WG - Device connected to two endpoints on different subnets. `wg` between device and one endpoint. `iperf3` between two endpoints through device over WG

`L2TP D2D` Device to Endpoint over L2TP - Device connected to endpoint with L2TP tunnel and `iperf3` over L2TP.

`L2TP E2E`  Endpoint to Endpoint over L2TP - Device connected to two endpoints on different subnets. L2TP between device and one endpoint. `iperf3` between two endpoints through device over L2TP.


## Results

|Devices          | D2E     |  E2E    | WG D2D| WG E2E   | L2TP D2D| L2TP E2E   |
|-----------------|---------|---------|-------|----------|---------|------------|
|AtomicPi         | :heavy_check_mark: 923 | :heavy_check_mark: 837| :heavy_check_mark: 895| :yellow_square:  665 | :heavy_check_mark: 767/863 | :heavy_check_mark: 798/705 |
|EdgerouteX       | 356/533| 750/510 | | | | |
|EdgerouteX HW OFFLOAD|  | 913/927 | 217/180 | 180/211  | | |
|EspressoBin      | :heavy_check_mark: 931 | :x: 335/403 | :x: 213/335 | | |
|OmniTik POE      | | 900 | | | | |
|Raspberry Pi 4B  | 950 | | 860 | | | | 
|WRT1900ACV1      |  :heavy_check_mark: 920 | :heavy_check_mark: 879 | :yellow_square:  350/450 | :x: 280/338 |  |  |

