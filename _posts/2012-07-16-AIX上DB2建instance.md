# AIX上经常使用的DB2建instance的语句 

    

    #useradd -g db2iadm1 -m db2inst6
    #useradd -g db2fadm1 -m db2fenc6
    #db2icrt -u db2fenc6 -p 60034 db2inst6
    #chuser fsize=-1 fsize_hard=-1 data=-1 data_hard=-1 stack=-1 stack_hard=-1 rss=-1 rss_hard=-1 nofiles=-1 nofiles_hard=-1 db2inst6
    #db2 get dbm cfg|grep -i svcename
    #db2 update dbm cfg using SVCENAME DB2_db2inst6
    #db2set -lr |grep -i db2comm=tcpip
    #db2iauto -on db2inst6


1 .打开self_tuning memory.

    

    db2 update db cfg for sample using SELF_TUNING_MEM ON immediate
    db2 get db cfg for sample |grep -i database_memory
    db2 get db cfg for sample |grep -i sortheap
    db2 get db cfg for sample |grep -i sheapthres_shr
    db2 get db cfg for sample |grep -i locklist
    db2 get db cfg for sample |grep -i self_tuning
    db2 "update database configuration for sample using SELF_TUNING_MEM ON"
    db2 "update database configuration for sample using PCKCACHESZ AUTOMATIC"
    db2 "update database configuration for sample using LOCKLIST AUTOMATIC"
    db2 "update database configuration for sample using MAXLOCKS automatic"
    db2 "update database configuration for sample using SORTHEAP AUTOMATIC"
    db2 "update database configuration for sample using SHEAPTHRES_SHR AUTOMATIC"
    db2 "update database configuration for sample using DATABASE_MEMORY AUTOMATIC"
