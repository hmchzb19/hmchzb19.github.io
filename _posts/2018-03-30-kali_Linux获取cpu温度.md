笔记本先后经历了两次返场维修，第一次，因为主板自动断电，第二次则是因为SSD硬盘插上去认不到。 
过了20天左右的时间，笔记本才能正常使用。但是开机感觉风扇的转速一会飞快，一会儿又没有声音，怀疑CPU的风扇有问题。 

所以研究了下如何检查CPU的温度： 

### 两种方案：
####ACPI 

`acpi_available`
显示没有输出，我只能man一下看看。 


        
    
        NAME
               acpi_available - test whether ACPI subsystem is available
    
        SYNOPSIS
               acpi_available
    
        DESCRIPTION
               acpi_available checks whether the ACPI ("Advanced Configuration and Power Interface") subsystem is available, as evidenced by the presence of the /proc/acpi directory.
    
        OPTIONS
               None.
    
        EXIT STATUS
               0 (true) ACPI subsystem is available
               1 (false) ACPI subsystem is not available
    


    看来我需要检查acpi_available的返回值。 

        
        root@kali:~# acpi_available
        root@kali:~# echo $?
        0
        apt-get install acpi
        root@kali:~# acpi -t
        Thermal 0: active, 90.0 degrees C
        Thermal 1: ok, 0.0 degrees C 



    看到这个温度，我有点懵，这个温度有点过高了。 

#### 使用lm-sensors来确定下：

        apt-get install lm-sensors
        sensors-detect
        一路yes下来，最后看到这个
        Now follows a summary of the probes I have just done.
        Just press ENTER to continue:
        
        Driver `coretemp':
          * Chip `Intel digital thermal sensor' (confidence: 9)
        
        To load everything that is needed, add this to /etc/modules:
        #----cut here----
        # Chip drivers
        coretemp
        #----cut here----
        If you have some drivers built into your kernel, the list above will
        contain too many modules. Skip the appropriate ones!
        
        Do you want to add these lines automatically to /etc/modules? (yes/NO)yes
        Successful!
        
        Monitoring programs won't work until the needed modules are
        loaded. You may want to run '/etc/init.d/kmod start'
        to load them.
        
        Unloading i2c-dev... OK
        Unloading cpuid... OK                                   


    到这里配置完成，使用命令来查看下：

        
        root@kali:~# sensors
        acpitz-virtual-0
        Adapter: Virtual device
        temp1: +49.0°C (crit = +105.0°C)
        temp2: +0.0°C (crit = +105.0°C)
    
        coretemp-isa-0000
        Adapter: ISA adapter
        Package id 0: +47.0°C (high = +105.0°C, crit = +105.0°C)
        Core 0: +45.0°C (high = +105.0°C, crit = +105.0°C)
        Core 1: +46.0°C (high = +105.0°C, crit = +105.0°C)


    过了段时间，大概1小时后，温度降到了这样。

#### 使用logger 脚本记录10分钟温度的变化。

        exec 1> >(logger -s -t $(basename $0)) 2>&1
    
        i=0
        while [[ $i -le 20 ]];do
            sleep 30
            sensors
            let i=$i+1
        done

