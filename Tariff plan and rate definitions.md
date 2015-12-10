

# Chargeback. Defining rates with variable measures
*_This document is a modification of the official documentation of CloudForms, public in https://access.redhat.com/documentation/en-US/Red_Hat_CloudForms/3.2/html/User_Guide/chap-Chargeback_and_Timelines.html_
***
4.1. CHARGEBACK
* 4.1.1. Chargeback Rates
* 4.1.2. Creating Chargeback Rates
* 4.1.3. Assigning Chargeback Rates
* 4.1.4. Creating a Chargeback Report

##4.1. CHARGEBACK

The chargeback feature allows you to calculate monetary virtual machine charges based on owner or company tag. To use this feature you must be collecting capacity and utilization data. For information on server control settings and capacity & utilization collection settings, see [CloudForms Management Engine Settings and Operations Guide](https://access.redhat.com/documentation/en-US/Red_Hat_CloudForms/3.2/html/Settings_and_Operations_Guide/index.html)

### 4.1.1. Chargeback Rates
ManageIQ Management Engine provides a default set of rates for calculating chargeback costs, but you can create your own set of computing and storage costs by navigating to Cloud Intelligence → Chargeback and clicking the Rates accordion.

Chargeback costs are computed using a set formula based on hourly cost per unit and hourly usage. For example, calculating the Memory Used Cost in dollars ($) for a day can be expressed in the following ways:

* Memory allocation per hour (in MB) * __Hourly Allocation cost per UNIT__ * Number of Memory Allocation metrics available for the day
* Sum of Memory allocation for the day (in MB) * __Hourly Allocation cost per UNIT__
* Sum of Memory allocation for the day (in MB) * __Daily Allocation cost per UNIT__ / 24

In this case the __Hourly Allocation cost per UNIT__ and the __Daily Allocation cost per UNIT__ are cost define in the rate. Thus the admin can define a cost in differents values of __UNIT__ like (B, KB, MB or GB). The system translates this units to the defaults measure (MB in this case) 

With this new functionality is possible to define (new, edit and copy) a rate selecting the measurement units by metrics:
![](http://res.cloudinary.com/ddtbdqfq7/image/upload/v1445604777/Captura_de_pantalla_de_2015-10-19_10-33-34_ug5qek.png)

### Fixed and variable rates
Each metric has a fixed rate and a variable rate. The variable rate is the one that varies with the measure given by the application. The variable rate will be multiplied by the measured value of the metric. On the contrary the fixed rate will remain the same and will be applied always even when the value is 0.

For example: we have that 9.29 GB of memory is used in a day with the variable rate set at 0,0098 dollar ($) per megabyte per day and the fixed rate set at 5,00 dollar ($) per day, the Memory Used Cost would be $39,06.
* 9.29 GB = 9514.08 MB
* 9514.08 MB *  (0,0098 per MB per day) + 5,00 (fixed amount) = 937,379
* 937,379 / 24 = 39,06 Memory Used Cost

If the used memory is 0, the Memory Used Cost would be $0,21:

* 0 MB * (0,0098 per MB per day) + 5,00 = 5,00
* 5,00 / 24 = 0,21 Memory Used Cost

### Chargeback tiers

The rate of each metric is defned through tiers which are intervals with given fixed and variable rates. By default there is a unique tier which cover all values from -Infinity to +Infinity. An example of a rate defined by tears would be: 


        Allocated CPU:
        
            -∞-0 CPUs: 0 + 0$/month/cpu
            
            1-4  CPUs: 5 + 1.2$/month/cpu

            5-8  CPUs: 5 + 1$/month/cpu

            9-+∞  vCPU: 5+0.8$/month/cpu

##Example 4.1. Memory Used Cost

In a scenario where 9.29 GB of memory is used in a day with the chargeback rate set at 0,0098 dollar ($) per megabyte per day, the Memory Used Cost would be $38,84.
* 9.29 GB = 9514.08 MB
* 9514.08 MB *  (0,0098 per MB per day) = 932,379
* 932,379 / 24 = 38,84 Memory Used Cost

In the same scenario where 9.29 GB of memory is used in a day but with the chargeback rate set at ten dollar ($10) per gigabyte per day, the Memory Used Cost would be $38,84.

* 9.29 GB = 9514.08 MB
* 9514.08 MB * (10 per GB per day)
* 9514.08 MB * (10/1024 per MB per day) = 932,379
* 932,379 / 24 = 38,84 Memory Used Cost

In this case is easer for a user to define a rate by GB. ManageIQ Management Engine is responsible for performing the corresponding numerics transformations.

## 4.1.2. Creating Chargeback Rates

ManageIQ Management Engine allows you to create your own set of computing and storage costs.

__Procedure 4.1. To Create Chargeback Rates__

1. Navigate to Cloud Intelligence → Chargeback.
1. Click the Rates accordion and select either Compute or Storage.
1. 
   * Use Compute to set chargeback rates for CPU, disk I/O, memory, network I/O, and fixed items.
   * Use Storage to set chargeback rates for fixed and storage items.
1. Click   (Configuration),   (Add a new Chargeback Rate) to create a new chargeback rate.
1. Type in a Description for the chargeback rate.
1. For each item that you want to set, create the tiers as you want and type a fixed and a variable rate for each of them. Then select a time option and the __unit option__ in the event that the item supporting unit selection.
1. Click Add.

## 4.1.3. Assigning Chargeback Rates

ManageIQ Management Engine allows you to assign chargeback rates by choosing from Compute and Storage.

__Procedure 4.2. To Assign Chargeback Rates__

1. Navigate to Cloud Intelligence → Chargeback.
1. Click the Assignments accordion, and click either Compute or Storage.
   * Use Compute to assign a compute chargeback rate. You can assign chargeback rates to The Enterprise, Selected Clusters, Selected Infrastructure Providers, or Tagged VMs and Instances.
   * Use Storage to assign a storage chargeback rate. You can assign chargeback rates to The Enterprise, Selected Datastores, or Tagged Datastores
1. From the Basic Info area, use the Assign To list to select a type of assignee to assign the rate set to. The options displayed vary based on the type you selected.
1. For each item to set, select the chargeback rate to use.
1. Click Save.

__Result:__
The rate is assigned. The next time you generate a chargeback report, these values will be used.

__NOTE__
Note that when viewing chargeback, there is a rate for a virtual machine for the number of the CPUs. The chargeback for this parameter is calculated based on when the virtual machine is running. If the virtual machine is not running, then it is not charged for CPU allocation. 

##[4.1.4. Creating a Chargeback Report](https://access.redhat.com/documentation/en-US/Red_Hat_CloudForms/3.2/html/User_Guide/chap-Chargeback_and_Timelines.html#To_create_a_chargeback_report)

A report with a rate __$0.02 daily per MHz__ on the entity Used CPU the appliance generate the following report. In this report the cost of this resource was __$0.81__.
![](http://res.cloudinary.com/ddtbdqfq7/image/upload/v1445612004/Captura_de_pantalla_de_2015-10-16_14-38-31_ppp36s.png)

If the rate it is changed  to __$0.02 daily per KHz__ the appliance generate the following report. In this report the cost of this resource was $829.07, where __$0.81 * 1024 = $829.44__

![](http://res.cloudinary.com/ddtbdqfq7/image/upload/v1445612004/Captura_de_pantalla_de_2015-10-16_14-46-50_w1lgou.png)
