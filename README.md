# memory_analysis
系统内存带宽分析


1 使用dmidecode，找出系统内存的几个重要参数指标
   data width
   type
   size
   Speed
   
#>dmidecode -t 17
#dmidecode 3.1
Getting SMBIOS data from sysfs.
SMBIOS 3.2.0 present.
#SMBIOS implementations newer than version 3.1.1 are not
#fully supported by this version of dmidecode.

Handle 0x1100, DMI type 17, 84 bytes

Memory Device

        Array Handle: 0x1000
        
        Error Information Handle: Not Provided
        
        Total Width: 72 bits
        
        Data Width: 64 bits
        
        Size: 8192 MB
        
        Form Factor: DIMM
        
        Set: 1
        
        Locator: A1
        
        Bank Locator: Not Specified
        
        Type: DDR4
        
        Type Detail: Synchronous Registered (Buffered)
        
        Speed: 2666 MT/s
        
        Manufacturer: 002C00B3002C
        
        Serial Number: 2143E33B
        
        Asset Tag: 0F191351
        
        Part Number: 9ASF1G72PZ-2G6D1
        
        Rank: 1
        
        Configured Clock Speed: 2666 MT/s
        
        Minimum Voltage: 1.2 V
        
        Maximum Voltage: 1.2 V
        
        Configured Voltage: 1.2 V
        
        
 这里系统中DDR的类型是DDR4，8G, 数据位宽 64bit， 工作频率 2666MT/s
 
 可以计算出一个DDR的工作带宽   64bit x 2666MT/s = 20.828125GB/s
 系统中一共有8个内存 64G，总共的带宽8x20.828125GB/s = 166.625GB/s
 
 2 如果是intel的平台，可以利用memory latency checker-mlc 测试
   https://www.intel.com/content/www/us/en/developer/articles/tool/intelr-memory-latency-checker.html

./mlc --peak_injection_bandwidth

$ ./mlc --peak_injection_bandwidth

Intel(R) Memory Latency Checker - v3.10

Command line parameters: --peak_injection_bandwidth


Using buffer size of 100.000MiB/thread for reads and an additional 100.000MiB/thread for writes


Measuring Peak Injection Memory Bandwidths for the system

Bandwidths are in MB/sec (1 MB/sec = 1,000,000 Bytes/sec)

Using all the threads from each core if Hyper-threading is enabled

Using traffic with the following read-write ratios

ALL Reads        :      116929.8

3:1 Reads-Writes :      60449.2

2:1 Reads-Writes :      46092.1

1:1 Reads-Writes :      34284.2

Stream-triad like:      48498.2


这里测试带宽的结果 116929.8    116929.8/1024= 114.1892578125GBs


还可以利用./mlc --loaded_latency测试负载延时

./mlc --loaded_latency

Intel(R) Memory Latency Checker - v3.10

Command line parameters: --loaded_latency


Using buffer size of 100.000MiB/thread for reads and an additional 100.000MiB/thread for writes

Measuring Loaded Latencies for the system

Using all the threads from each core if Hyper-threading is enabled

Using Read-only traffic type

Inject  Latency Bandwidth

Delay   (ns)    MB/sec

==========================

 00000  143.76   114235.5
 
 00002  140.90   114132.3
 
 00008  143.01   113917.7
 
 00015  137.72   114136.7
 
 00050  135.67   112503.6
 
 00100  111.64    72658.7
 
 00200  104.17    46424.4
 
 00300  101.11    32820.2
 
 00400   98.12    25123.2
 
 00500   98.15    20435.2
 
 00700   97.53    15021.4
 
 01000   92.84    10773.6
 
 01300   90.85     8496.2
 
 01700   90.47     6718.4
 
 02500   87.62     4810.1
 
 03500   87.02     3653.8
 
 05000   87.77     2783.6
 
 09000   85.29     1889.8
 
 20000   84.75     1270.7
 



