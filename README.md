
# GSoC-23
Work I did as a part of Google Summer of Code in 2023 at PostgreSQL.

Parent repository: [https://github.com/pgadmin-org/pgadmin4](https://github.com/pgadmin-org/pgadmin4)

Patch file: [system_statistics.patch](https://github.com/Sahil1479/GSoC-23/blob/main/system_stats.patch)

PRs:
[#6721](https://github.com/pgadmin-org/pgadmin4/pull/6721)
[#6833](https://github.com/pgadmin-org/pgadmin4/pull/6833)

## Description

GUI representation of the system's activity using the 'system_stats' extension.

## Changes Made

- **Added tab to toggle between general (existing dashboard) and system statistics.**

	![image](https://github.com/pgadmin-org/pgadmin4/assets/56965873/86f2fb65-3d57-4cd2-8986-d67e43aa8591)

- **Updated StreamingChart**
	- Secondary Y-axis support (Different scale on the right hand side). Set `showSecondAxis=True` to enable this.
	- It is currently not formatting the axis values and takes much space in case of larger values (eg. memory_usage, &nbsp; handle_count etc.). So added custom formatter for y-axis values. `suffixes = ['', 'k', 'M', 'B', 'T']`

		![image](https://github.com/pgadmin-org/pgadmin4/assets/56965873/552b4c37-cb0e-481b-a20c-21d21df6080a)

	- Tooltip issue: When the container's size changes, tooltips continues to accumulate without being properly removed. This bug is also present in the current stable version of pgAdmin4 application.
Current method that is used to display tooltips involves inserting a new element into the DOM with the class name "uplot-tooltip". So, to resolve this issue, I have added method to remove all the existing tooltip elements before inserting a new one.

        ![tooltip-issue-on-container-resize](https://github.com/pgadmin-org/pgadmin4/assets/56965873/96bd9c67-ddda-4f75-ba23-1b501312b258)
        ![tooltip-issue-on-screen-resize](https://github.com/pgadmin-org/pgadmin4/assets/56965873/987251d5-db61-48ee-a4df-bc53a89edb7c)

- **If the System Stats extension does not exist, display the appropriate message**
    
    ![image](https://github.com/pgadmin-org/pgadmin4/assets/56965873/be706bae-75f1-4178-9beb-20811238d147)

- **System statistics features covered** 

  All the features are split into 4 different tabs with the following grouping:
	1. **Summary**
		  - OS information </br> 
			Tabular representation of the following OS properties.
		    > - Name
			> - Version
			> - Host name
			> - Domain name
			> - Architecture
			> - OS up since seconds
		  - Handle & process count graph </br> 
		     Streaming line chart to show changes in handle and process count over time.
		     > - Handle count: Number of object handles that are currently open in the operating system. 
		     > - Process count: Number of processes that are currently running on the operating system.
		  - CPU Information </br> 
		      Tabular representation of the following CPU properties.
		      > - Vendor
		      > - Description
		      > - Model name
		      > - No of cores
		      > - Architecture
		      > - Clock speed Hz
		      > - L1 dcache size
		      > - L1 icache size
		      > - L2 cache size
		      > - L3 cache size
   2. **CPU**
         - CPU Usage Information </br> 
	         Streaming line chart to show changes in CPU usage over time. Values are a percentage of time spent by CPUs for all operations.
	         Following modes are covered:
	         > - User mode normal
	         > - User mode niced
	         > - Kernal mode
	         > - Idle mode
         - Load Avg Information </br> 
	         Streaming line chart to show changes in the average load of the system over 1, 5, 10 and 15 minute intervals over time. 
         - Process CPU usage </br> 
	         Tabular representation of the CPU usage per process with the options to filter and sort.
	         > - Process ID
	         > - Process name
	         > - CPU usage value
    3. **Memory**
         - Memory Information </br> 
	         Streaming line chart to show changes in the memory usage. </br> 
	         Both Main and Swap memory are covered with following categories.
	         > - Total memory available
	         > - Used memory
	         > - Free memory
         - Process memory usage </br> 
	         Tabular representation of the memory usage per process with the options to filter and sort.
	          > - Process ID
	         > - Process name
	         > - Memory usage in bytes
	         > - Total memory used in bytes
    4. **Storage**
         - Disk Information </br> 
	         Tabular representation of the following properties for all the drive partitions.
	         > - File system
	         > - File system type
	         > - Mount point
	         > - Drive letter
	         > - Total space
	         > - Used space
	         > - Free space
	         > - Total inodes
	         > - Used inodes
	         > - Free inodes
	         
	         Graphical representation of Total space using a Pie chart and Stacked Bar chart for Used and Available space
         - I/O Analysis Information </br> 
	         Streaming line chart to show changes in the total number of operations, bytes transferred, and time spent in milliseconds for reading and writing over time for each disk. 

- **Provided option to configure the refresh rates for the API calls** `(File > Preferences > Dashboards > Refresh rates)`

- **SQL queries**

  All the required queries are present in the `pgadmin4\web\pgadmin\dashboard\templates\dashboard\sql\default\system_statistics.sql`

- **Backend support**

  All the backend related code to handle API requests and process the queries is present in the `pgadmin4\web\pgadmin\dashboard\__init__.py`

## Demo
A small demonstration of the work can be found [here](https://drive.google.com/file/d/1gfTYx4u-G21hWCeh70UfkpcVNROHyT17/view?usp=sharing).
