# Benchmark Results

## Definitions
`D2E` Device to Endpoint - Device connected to endpoint and `iperf3` between the two.

`E2E` Endpoint to Endpoint - Device connected to two endpoints on different subnets. `iperf3` between two endpoints through device.

`WG D2E` Device to Endpoint over WG - Device connected to endpoint with `wg` tunnel and `iperf3` over `wg`.

`WG E2E`  Endpoint to Endpoint over WG - Device connected to two endpoints on different subnets. `wg` between device and one endpoint. `iperf3` between two endpoints through device over WG

`L2TP D2E` Device to Endpoint over L2TP - Device connected to endpoint with L2TP tunnel and `iperf3` over L2TP.

`L2TP E2E`  Endpoint to Endpoint over L2TP - Device connected to two endpoints on different subnets. L2TP between device and one endpoint. `iperf3` between two endpoints through device over L2TP.


## Results

|Devices          | D2E     |  E2E    | WG D2E  | WG E2E   | L2TP D2E| L2TP E2E   |
|-----------------|---------|---------|---------|----------|---------|------------|
|AtomicPi         | 923     | 837     | 895     | 665      | 767/863 | 798/705    |
|EdgerouteX       | 533/356 | 750/510 |         |          |         |            |
|EdgerouteX FastPath | 569/388| 913/927 | 217/180 | 180/211  | 312/188 | 320/290    |
|EspressoBin      | 931     | 335/403 | 213/335 |          |         |            |
|OmniTik POE      |         | 900     |         |          |         |            |
|PC Engines apu2c4| 936     |         | 645     |          |         |            |
|Raspberry Pi 4B  | 950     |         | 770     |          |         |            | 
|WRT1900ACV1      | 920     | 879     | 350/450 | 280/338  |         |            |


