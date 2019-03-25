我记得前几天张大妈在帖子里面贴了些狗东的优惠券码。  
这些优惠券码是以图像的形式出现的，因此需要开两个浏览器，一个看图，另外一个输入。  

当然也可以用tesseract 来识别这些图，而经过我的测试，使用没有经过Train的tesseract 来做这件事，成功率并不是很高。  

原始图这里我不贴了  

我直接使用命令来识别这些图:  

    
    
        tesseract 1.jpg 1.txt -l eng
        tesseract 2.jpg 2.txt -l eng
        tesseract 3.jpg 3.txt -l eng
    


然后比较tesseract的到结果和肉眼识别的结果:   


    
        wdiff -n y1.txt 1.txt |colordiff
        [-fb59367a4cbbbd66-]
        [-5f345927023fbd73-]
         
         
         
         
         
         
         
         
    
        {+fb59367adcbbbd66+}
        {+5345927023fbd73+}
        e06ad21966736612
        [-f1b83f66faeaa80e-]
        {+flb83f66faeaas0e+}
        1c705f43b3b7b5a2
        [-002f03f2513ca0d9-]
        9212cfd758db3539
        5a839b0b74388241
        [-85bb5b2930b53bcd-]
        {+85bb5b2930b53bed+}
        9e56795e85ded228
    





        wdiff -n y2.txt 2.txt.txt |colordiff
        [-79f9bda47a9602d6-]{+7979bda47a9602d6+}
        2705b5c65abb0abe
        eb498440d9d502c4
        2db38851d7192c88
        [-a7cf9b7f7cced1ff-]
        {+ff+}
        dd9967f0b3975aee
        [-e4ae153c12b9e94d-]
        [-ed7e2aa417e3c440-]
        {+edae153c12b9e94d+}
        {+ed7e2aad17e3c440+}
        291732b612ca071e
        [-4514a9c63370986c-]
        [-c401c0e06a6292c4-]
        [-d9fa11fcfa5249d6-]
        [-1266b9f1af4fb44f-]
        [-d2412e36da0a174a-]
        {+4514a9c63370986C+}
        {+cA01c0e06a6292c4+}
        {+d9fal1fcfa5249d6+}
        {+d2412e36da0al74a+}
        00f45c48eb782a2c
        [-672842cf976f2223-]
        [-f58bb161657c0e2f-]
        [-02f3a7db44ed0e97-]
        [-585278bd17f2d035-]
        [-8269c2f0a4ac6b86-]
        {+f8bb161657c0e2F+}
        {+02f8a7db44ed0e97+}
        {+585278bd1712d035+}
        {+8269c2f0adacéb86+}
    





        wdiff -n y3.txt 3.txt.txt |colordiff
        [-9a72090ca1658f59-]{+9a72090ca1658159+}
        79e44903aa7e2ee7
        [-a6183fe1c7b18653-]
        {+a6183felc7b18653+}
        e9c53c416f791b85
        ced79be7906d2ee8
        [-6565e4cf15aac08b-]
        [-a2122f06a7e07430-]
        [-2a69f0243141f1d6-]
        [-7273f58fd77da446-]
        {+6565e4cfl5aac08b+}
        {+a2122{06a7e07430+}
        {+2a6910243141f1d6+}
        {+7273f68fd77da446+}
        440281af84f3ba22
        577ee43275b0ac2a
        6508c09968c2f0db
        83b1dd1a9162a3b3
        c42b235341d88bd3
        23e5befd7de7821d
        [-9a2d6a92c2c8a57e-]
        {+Sa2d6a92c2c8a57e+}
        742f848626d5bd96
        009339bb0b5d2d78
        dacbf47093be9ef8
        [-fe10ca6d2ab4e0a8-]
        {+fel0ca6d2ab4e0a8+}
    


没有经过train的tesseract 在识别 这些来自张大妈的image的时候表现的并不是足够的好。  
很多次出现 1 和l 无法识别的问题。  
然而用眼来识别，我几乎感觉眼都快看瞎了。   
