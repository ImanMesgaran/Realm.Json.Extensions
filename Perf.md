#Android "Low End" Device

###Determine worst-case issues with memory consumption and performance

	[CpuAbi:	armeabi-v7a armeabi]
	[VERSION.Release:	5.1.1]
	[VERSION.Sdk:		22]
	[VERSION.SdkInt:	LollipopMr1]

###AutoMapper vs. Realm.Manage

	Xamarin.Android Version: 7.0.2.37
	Realm Version: 0.80.0
	SushiHangover.RealmJson.Estensions 1.1.0

###Performance Testing Result: 

`Realm.Manage` is **faster** than using AutoMapper for bulk loading a *small* number of record, under **`~1k`**.

From bulk loading `1k` to `64k` records, there is no speed advantage between the two methods.

Use `AutoMapper` if bulk loading 65k(+) records within one transaction as the memory requirements of using `Realm.Manage` is higher.

**Note:** The `SlidingStream` needs to be replaced with a Circular single byte buffer to avoid the memory allocs of `new ArraySegment<byte>` that it does and the alloc error when using a high number of bulk record inserts so a clearer picture during benchmarking of the memory usage of large Realm transactions and Json.Net Stream-based Reading. 

`AutoMapper`:

	D/REALM   (14529): PerformanceOfJsonTextReaderUsingAutoMapper
	D/REALM   (14529): Records: 256		| 0.7103ms/rec	| Time: 181.8245ms
	D/REALM   (14529): Records: 512		| 0.3309ms/rec	| Time: 169.4295ms
	D/REALM   (14529): Records: 1024	| 0.2001ms/rec	| Time: 204.8747ms
	D/REALM   (14529): Records: 2048	| 0.1497ms/rec	| Time: 306.5899ms
	D/REALM   (14529): Records: 4096	| 2.5678ms/rec	| Time: 10517.7937ms
	D/REALM   (14529): Records: 8192	| 0.1045ms/rec	| Time: 855.7301ms
	D/REALM   (14529): Records: 16384	| 0.099ms/rec	| Time: 1622.4953ms
	D/REALM   (14529): Records: 32768	| 0.0958ms/rec	| Time: 3138.9236ms
	D/REALM   (14529): Records: 65536	| 0.0896ms/rec	| Time: 5873.619ms
	D/REALM   (14529): Records: 131072	| 0.0886ms/rec	| Time: 11614.4495ms
	D/REALM   (14529): Avg: 0.1317 ms/rec (261888/34485.7298)

`Manage`:

	D/REALM   (14529): PerformanceOfJsonTextReaderUsingRealmManage
	D/REALM   (14529): Records: 256		| 0.4542ms/rec	| Time: 116.2785ms
	D/REALM   (14529): Records: 512		| 0.2227ms/rec	| Time: 114.0283ms
	D/REALM   (14529): Records: 1024	| 0.1629ms/rec	| Time: 166.8574ms
	D/REALM   (14529): Records: 2048	| 0.1189ms/rec	| Time: 243.4424ms
	D/REALM   (14529): Records: 4096	| 2.535ms/rec	| Time: 10383.3853ms
	D/REALM   (14529): Records: 8192	| 0.0861ms/rec	| Time: 705.1489ms
	D/REALM   (14529): Records: 16384	| 0.0889ms/rec	| Time: 1457.1218ms
	D/REALM   (14529): Records: 32768	| 0.0851ms/rec	| Time: 2789.4739ms
	D/REALM   (14529): Records: 65536	| 0.0857ms/rec	| Time: 5616.1939ms
	D/REALM   (14529): Avg: 0.1651 ms/rec (130816/21591.9304)

SushiHangover.RealmJson.Estensions 1.0.1 (Using Realm 0.80.0) | Replaced AutoMapper w/ Realm.Manage (Testing)

	D/REALM   (28272): PerformanceOfJsonTextReaderNoUpdateRecords
	D/REALM   (28272): Records: 256		| 0.338741015625ms/rec	| Time: 86.7177ms
	D/REALM   (28272): Records: 512		| 0.277202734375ms/rec	| Time: 141.9278ms
	D/REALM   (28272): Records: 1024	| 0.182434765625ms/rec	| Time: 186.8132ms
	D/REALM   (28272): Records: 2048	| 0.12649150390625ms/rec	| Time: 259.0546ms
	D/REALM   (28272): Records: 4096	| 2.54397041015625ms/rec	| Time: 10420.1028ms
	D/REALM   (28272): Records: 8192	| 0.0888458862304688ms/rec	| Time: 727.8255ms
	D/REALM   (28272): Records: 16384	| 0.0834081359863281ms/rec	| Time: 1366.5589ms
	D/REALM   (28272): Records: 32768	| 0.0834730682373047ms/rec	| Time: 2735.2455ms
	D/REALM   (28272): Records: 65536	| 0.0811461029052734ms/rec	| Time: 5317.991ms
	D/REALM   (28272): Avg: 0.16238256023728 ms/rec (130816/21242.237)

RealmJson.Estensions 1.0.1 (Using Realm 0.80.0) | Using AutoMapper (Testing)

	D/REALM   (30722): PerformanceOfJsonTextReaderNoUpdateRecords
	D/REALM   (30722): Records: 256		| 0.775414453125ms/rec	| Time: 198.5061ms
	D/REALM   (30722): Records: 512		| 0.29513125ms/rec	| Time: 151.1072ms
	D/REALM   (30722): Records: 1024	| 0.206872265625ms/rec	| Time: 211.8372ms
	D/REALM   (30722): Records: 2048	| 0.168656103515625ms/rec	| Time: 345.4077ms
	D/REALM   (30722): Records: 4096	| 2.55110466308594ms/rec	| Time: 10449.3247ms
	D/REALM   (30722): Records: 8192	| 0.102524548339844ms/rec	| Time: 839.8811ms
	D/REALM   (30722): Records: 16384	| 0.101116320800781ms/rec	| Time: 1656.6898ms
	D/REALM   (30722): Records: 32768	| 0.0936478851318359ms/rec	| Time: 3068.6539ms
	D/REALM   (30722): Records: 65536	| 0.0914852951049805ms/rec	| Time: 5995.5803ms
	D/REALM   (30722): Records: 131072	| 0.0905058624267578ms/rec	| Time: 11862.7844ms
	D/REALM   (30722): Avg: 0.132803994073803 ms/rec (261888/34779.7724)

SushiHangover.RealmJson.Estensions 1.0.1

Run 1:

	D/REALM   (14205): Records: 256		| 0.443012109375ms/rec		| Time: 113.4111ms
	D/REALM   (14205): Records: 512		| 0.2919369140625ms/rec		| Time: 149.4717ms
	D/REALM   (14205): Records: 1024	| 0.2102724609375ms/rec		| Time: 215.319ms
	D/REALM   (14205): Records: 2048	| 0.1439349609375ms/rec		| Time: 294.7788ms
	D/REALM   (14205): Records: 4096	| 2.56676274414063ms/rec	| Time: 10513.4602ms
	D/REALM   (14205): Records: 8192	| 0.106145129394531ms/rec	| Time: 869.5409ms
	D/REALM   (14205): Records: 16384	| 0.110161462402344ms/rec	| Time: 1804.8854ms
	D/REALM   (14205): Records: 32768	| 0.105295004272461ms/rec	| Time: 3450.3067ms
	D/REALM   (14205): Records: 65536	| 0.100203950500488ms/rec	| Time: 6566.9661ms
	D/REALM   (14205): Records: 131072	| 0.101116456604004ms/rec	| Time: 13253.5362ms
	D/REALM   (14205): Records: 262144	| 0.103861807632446ms/rec	| Time: 27226.7497ms

Run 2:

	D/REALM   (26295): Records: 256		| 0.45726640625ms/rec		| Time: 117.0602ms
	D/REALM   (26295): Records: 512		| 0.2904365234375ms/rec		| Time: 148.7035ms
	D/REALM   (26295): Records: 1024	| 0.17008076171875ms/rec	| Time: 174.1627ms
	D/REALM   (26295): Records: 2048	| 0.138494287109375ms/rec	| Time: 283.6363ms
	D/REALM   (26295): Records: 4096	| 2.56015698242188ms/rec	| Time: 10486.403ms
	D/REALM   (26295): Records: 8192	| 0.109705065917969ms/rec	| Time: 898.7039ms
	D/REALM   (26295): Records: 16384	| 0.106557659912109ms/rec	| Time: 1745.8407ms
	D/REALM   (26295): Records: 32768	| 0.106424710083008ms/rec	| Time: 3487.3249ms
	D/REALM   (26295): Records: 65536	| 0.105907780456543ms/rec	| Time: 6940.7723ms
	D/REALM   (26295): Records: 131072	| 0.0999306144714356ms/rec	| Time: 13098.1055ms

Run 3:

	D/REALM   (26295): Records: 256		| 0.460425ms/rec			| Time: 117.8688ms
	D/REALM   (26295): Records: 512		| 0.28135859375ms/rec		| Time: 144.0556ms
	D/REALM   (26295): Records: 1024	| 0.19786767578125ms/rec	| Time: 202.6165ms
	D/REALM   (26295): Records: 2048	| 0.14070439453125ms/rec	| Time: 288.1626ms
	D/REALM   (26295): Records: 4096	| 2.56657346191406ms/rec	| Time: 10512.6849ms
	D/REALM   (26295): Records: 8192	| 0.106971716308594ms/rec	| Time: 876.3123ms
	D/REALM   (26295): Records: 16384	| 0.105365350341797ms/rec	| Time: 1726.3059ms
	D/REALM   (26295): Records: 32768	| 0.104184643554688ms/rec	| Time: 3413.9224ms
	D/REALM   (26295): Records: 65536	| 0.101032057189941ms/rec	| Time: 6621.2369ms
	D/REALM   (26295): Records: 131072	| 0.10309736328125ms/rec	| Time: 13513.1776ms

**Note:** The time increase for 4096 records is reproduceable, *but* I wrote a test (`Create4kBoundary`) that 
does not exhibit it....?

	D/REALM   (26295): Records: 4080	| 0.0241189215686275 ms/rec	| Time: 98.4052 ms
	D/REALM   (26295): Records: 4081	| 0.022740602793433 ms/rec	| Time: 92.8044 ms
	D/REALM   (26295): Records: 4082	| 0.0239633023027928 ms/rec	| Time: 97.8182 ms
	D/REALM   (26295): Records: 4083	| 0.0254490570658829 ms/rec	| Time: 103.9085 ms
	D/REALM   (26295): Records: 4084	| 0.0226476493633693 ms/rec	| Time: 92.4930000000001 ms
	D/REALM   (26295): Records: 4085	| 0.0226672949816402 ms/rec	| Time: 92.5959 ms
	D/REALM   (26295): Records: 4086	| 0.0240611111111111 ms/rec	| Time: 98.3137 ms
	D/REALM   (26295): Records: 4087	| 0.0231756545143137 ms/rec	| Time: 94.7189000000001 ms
	D/REALM   (26295): Records: 4088	| 0.0301792318982387 ms/rec	| Time: 123.3727 ms
	D/REALM   (26295): Records: 4089	| 0.0223608950843727 ms/rec	| Time: 91.4337 ms
	D/REALM   (26295): Records: 4090	| 0.0239501222493888 ms/rec	| Time: 97.956 ms
	D/REALM   (26295): Records: 4091	| 0.030405255438768 ms/rec	| Time: 124.3879 ms
	D/REALM   (26295): Records: 4092	| 0.0233639051808407 ms/rec	| Time: 95.6051 ms
	D/REALM   (26295): Records: 4093	| 0.0196750549719032 ms/rec	| Time: 80.53 ms
	D/REALM   (26295): Records: 4094	| 0.027176306790425 ms/rec	| Time: 111.2598 ms
	D/REALM   (26295): Records: 4095	| 0.0202508913308913 ms/rec	| Time: 82.9274 ms
	D/REALM   (26295): Records: 4096	| 0.02789453125 ms/rec	| Time: 114.256 ms
	D/REALM   (26295): Records: 4097	| 0.0209337564071272 ms/rec	| Time: 85.7656000000001 ms
	D/REALM   (26295): Records: 4098	| 0.0207603465104929 ms/rec	| Time: 85.0759 ms
	D/REALM   (26295): Records: 4099	| 0.0236639180287875 ms/rec	| Time: 96.9984000000001 ms
	D/REALM   (26295): Records: 4100	| 0.0257108292682927 ms/rec	| Time: 105.4144 ms
	D/REALM   (26295): Records: 4101	| 0.0228245793708852 ms/rec	| Time: 93.6036 ms
	D/REALM   (26295): Records: 4102	| 0.0221304485616773 ms/rec	| Time: 90.7791000000001 ms
	D/REALM   (26295): Records: 4103	| 0.0217957592005849 ms/rec	| Time: 89.428 ms
	D/REALM   (26295): Records: 4104	| 0.0308399610136452 ms/rec	| Time: 126.5672 ms
	D/REALM   (26295): Records: 4105	| 0.0202588306942753 ms/rec	| Time: 83.1625 ms
	D/REALM   (26295): Records: 4106	| 0.0225466390647832 ms/rec	| Time: 92.5765 ms
	D/REALM   (26295): Records: 4107	| 0.0247426832237643 ms/rec	| Time: 101.6182 ms
	D/REALM   (26295): Records: 4108	| 0.0228745861733203 ms/rec	| Time: 93.9688 ms
	D/REALM   (26295): Records: 4109	| 0.0186426867851059 ms/rec	| Time: 76.6028 ms
	D/REALM   (26295): Records: 4110	| 0.0219411192214112 ms/rec	| Time: 90.178 ms
	D/REALM   (26295): Records: 4111	| 0.0226164923376307 ms/rec	| Time: 92.9764 ms
	D/REALM   (26295): Records: 4112	| 0.0245586332684825 ms/rec	| Time: 100.9851 ms

Release Build: AotAssemblies=true / AndroidAotAdditionalArguments=no-write-symbols 
Note: LLVM is disable/broken in 7.0.2.37



	<AotAssemblies>true</AotAssemblies>
	<AndroidAotAdditionalArguments>no-write-symbols</AndroidAotAdditionalArguments>
	<AotAdditionalArguments>no-write-symbols </AotAdditionalArguments>
	<LLVMEnable>true</LLVMEnable>



