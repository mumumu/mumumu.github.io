---
layout: post
title: "dmidecode"
date: 2013-07-04 18:01
comments: true
categories: [hardware]
---
[http://www.nongnu.org/dmidecode/](http://www.nongnu.org/dmidecode/)  
[http://man.cx/dmidecode%288%29](http://man.cx/dmidecode%288%29)

自作とかしてないと dmidecode とか使わないよなぁ、と心の隅に置いていただけのコマンドであるが、自分サーバ自作してたんだった(´ー｀；)  
オプションなしで実行すると山盛りの情報が出てくるので、--type だけはちゃんと覚えておこう。
 
	-t, --type TYPE
	    Only  display  the  entries  of type TYPE. TYPE can be either a DMI type number, or a comma-separated list of type numbers, or a
	    keyword from the following list: bios, system, baseboard, chassis, processor, memory, cache, connector, slot. Refer to  the  DMI
	    TYPES  section  below for details.  If this option is used more than once, the set of displayed entries will be the union of all
	    the given types.  If TYPE is not provided or not valid, a list of all valid keywords is printed  and  dmidecode  exits  with  an
	    error.

Thinkpad X230 でプロセッサ情報を見ると以下のようになる。

	$ sudo dmidecode  --type processor
	# dmidecode 2.11
	SMBIOS 2.7 present.
	
	Handle 0x0001, DMI type 4, 42 bytes
	Processor Information
	        Socket Designation: CPU Socket - U3E1
	        Type: Central Processor
	        Family: Core i5
	        Manufacturer: Intel(R) Corporation
	        ID: A9 06 03 00 FF FB EB BF
	        Signature: Type 0, Family 6, Model 58, Stepping 9
	        Flags:
	                FPU (Floating-point unit on-chip)
	                VME (Virtual mode extension)
	                DE (Debugging extension)
	                PSE (Page size extension)
	                TSC (Time stamp counter)
	                MSR (Model specific registers)
	                PAE (Physical address extension)
	                MCE (Machine check exception)
	                CX8 (CMPXCHG8 instruction supported)
	                APIC (On-chip APIC hardware supported)
	                SEP (Fast system call)
	                MTRR (Memory type range registers)
	                PGE (Page global enable)
	                MCA (Machine check architecture)
	                CMOV (Conditional move instruction supported)
	                PAT (Page attribute table)
	                PSE-36 (36-bit page size extension)
	                CLFSH (CLFLUSH instruction supported)
	                DS (Debug store)
	                ACPI (ACPI supported)
	                MMX (MMX technology supported)
	                FXSR (FXSAVE and FXSTOR instructions supported)
	                SSE (Streaming SIMD extensions)
	                SSE2 (Streaming SIMD extensions 2)
	                SS (Self-snoop)
	                HTT (Multi-threading)
	                TM (Thermal monitor supported)
	                PBE (Pending break enabled)
	        Version: Intel(R) Core(TM) i5-3320M CPU @ 2.60GHz
	        Voltage: 0.9 V
	        External Clock: 100 MHz
	        Max Speed: 2600 MHz
	        Current Speed: 2600 MHz
	        Status: Populated, Enabled
	        Upgrade: Socket rPGA988B
	        L1 Cache Handle: 0x0003
	        L2 Cache Handle: 0x0004
	        L3 Cache Handle: 0x0005
	        Serial Number: None
	        Asset Tag: None
	        Part Number: None
	        Core Count: 2
	        Core Enabled: 2
	        Thread Count: 4
	        Characteristics:
	                64-bit capable

メモリの情報は以下のような感じだ。現状 4G しか載ってないが最大16GBらしい。今度頼んでおこう(*´〜｀)

	$ sudo dmidecode  --type memory
	# dmidecode 2.11
	SMBIOS 2.7 present.
	
	Handle 0x0007, DMI type 16, 23 bytes
	Physical Memory Array
	        Location: System Board Or Motherboard
	        Use: System Memory
	        Error Correction Type: None
	        Maximum Capacity: 16 GB
	        Error Information Handle: Not Provided
	        Number Of Devices: 2
	
	Handle 0x0008, DMI type 17, 34 bytes
	Memory Device
	        Array Handle: 0x0007
	        Error Information Handle: Not Provided
	        Total Width: 64 bits
	        Data Width: 64 bits
	        Size: 4096 MB
	        Form Factor: SODIMM
	        Set: None
	        Locator: ChannelA-DIMM0
	        Bank Locator: BANK 0
	        Type: DDR3
	        Type Detail: Synchronous
	        Speed: 1600 MHz
	        Manufacturer: Hynix/Hyundai
	        Serial Number: 1082DC09
	        Asset Tag: None
	        Part Number: HMT351S6CFR8C-PB  
	        Rank: Unknown
	        Configured Clock Speed: 1600 MHz
	
	Handle 0x0009, DMI type 17, 34 bytes
	Memory Device
	        Array Handle: 0x0007
	        Error Information Handle: Not Provided
	        Total Width: Unknown
	        Data Width: Unknown
	        Size: No Module Installed
	        Form Factor: DIMM
	        Set: None
	        Locator: ChannelB-DIMM0
	        Bank Locator: BANK 2
	        Type: Unknown
	        Type Detail: None
	        Speed: Unknown
	        Manufacturer: Not Specified
	        Serial Number: Not Specified
	        Asset Tag: None
	        Part Number: Not Specified
	        Rank: Unknown
	        Configured Clock Speed: Unknown
	
