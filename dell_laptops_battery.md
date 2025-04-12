To pronlongue the life of your battery, it's good practice to keep the charge between 50% and 80%. If you have a DELL laptop, the [Dell Command Configure Application](https://www.dell.com/support/home/en-uk/drivers/DriversDetails?driverId=V01T5&lwp=rt) it's very usefull for this. 

If your DELL laptop stays plugged in for long periods, run this to help prolong the battery's life:

    sudo /opt/dell/dcc/cctk --PrimaryBattChargeCfg=Custom:50-80

When you need the battery fully charged because you're going to use the laptop on battery only, run this with enough time to charge before unplugging:

    sudo /opt/dell/dcc/cctk --PrimaryBattChargeCfg=PrimAcUse

