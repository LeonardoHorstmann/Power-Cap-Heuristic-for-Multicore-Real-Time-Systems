Altered Files:
	- app/test_pedf.cc: 
		- contains test application code

	- app/test_pedf_traits.h: 
		- test configuration code

	- include/architecture/ia32/cpu.h:
		- new functions to read MSRs and apply clock modulation;
			- pp1_energy_status()
			- pp0_energy_status() 
			- pkg_energy_status()
			- dram_energy_status()
			- rapl_power_unit()
			- rapl_energy_unit()
			check 14.9.3 “Package RAPL Domain” of Intel SDM for the above functions understanding
			- temperature()
			- clock (Reg64 clock)

	- include/architecture/ia32/monitoring_capture.h:
		- code that defines monitoring system

	- src/architecture/ia32/monitoring_capture_send.cc:
		- monitoring system printing methods

	- include/pmu.h:
		- configured sandy_bridge's PMU events on the enum Event
		- added variables to make it easier events set up

	- include/scheduler.h:
		- configurations of monitoring schedulers (schedulers that activates monitoring condition):
			- M<scheduler_name> (MCEDF, MPEDF, MGEDF)
			- included init function for PMU configuration and boolean variable to enabling captures

	- include/thread.h:
		- variables included to monitoring system:
			- volatile int _missed_deadlines;
    		- int _times_p_count;
    		- static Monitoring_Capture* _thread_monitor;
    		- static volatile bool _end_capture;
    		- static unsigned int _global_deadline_misses;
    		- static unsigned int _prev_global_deadline_misses;

    - include/periodic_thread.h:
		- calculates _times_p_count
		- moment captures inside of wait_next method

	- include/system/types.h:
		- added created schedulers
    
    - src/component/thread.cc:
    	- moment captures inside of dispatch method
    	- system exit code on idle changed to print captured data at the end of execution 

    - src/component/thread_init.cc:
    	- allocates memory and initializes monitoring system

    - src/component/semaphore.cc:
    	- register global_deadline_misses

    - src/architecture/ia32/pmu.cc:
    	- set up of events numbering
    		- CPU::Reg32 Intel_Sandy_Bridge_PMU::_events[PMU_Common::EVENTS]
