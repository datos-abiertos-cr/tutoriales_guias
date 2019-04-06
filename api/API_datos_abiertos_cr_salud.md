---
title: "API datos abiertos cr salud"
output: 
  html_document:
    code_folding: show
    fig_height: 30
    fig_width: 30
    keep_md: yes
    toc: yes
    toc_depth: 2
toc_float: yes
---

#Objetivo
Desarrollar el tutorial de [Frans Van Dunné](https://cran.r-project.org/web/packages/junr/vignettes/acceder-junar-api.html) sobre APIs


```r
library(junr)
url_base <- "http://micit.cloudapi.junar.com/api/v2/datastreams/"
api_key <- "f6d3102afb404c87f21edbe71a2987330e084975"
```


```r
get_index(url_base, api_key)
```

```r
list_guid(url_base, api_key)
```

```
##  [1] "INVES-POR-AREA-CIENT-Y"        "INVES-SEGUN-SECTO-DE-EJECU"   
##  [3] "PERSO-EN-INVES-Y-DESAR"        "TOTAL-DE-DIPLO-OTORG-2006"    
##  [5] "TOTAL-DE-DIPLO-OTORG-POR"      "INVES-POR-AREA-CIENT-14530"   
##  [7] "NUMER-DE-PERSO-EN-INVES"       "EMPRE-INNOV-POR-TIPO-DE"      
##  [9] "EMPRE-INNOV-RESPE-AL-TOTAL"    "PORCE-DE-EMPRE-QUE-85973"     
## [11] "FACTO-QUE-HAN-OBSTA-LA"        "FACTO-QUE-HAN-OBSTA-67988"    
## [13] "PORCE-DE-EMPRE-QUE-HAN"        "IMPAC-DE-LAS-INNOV-EN"        
## [15] "USO-DEL-INTER-POR-PARTE"       "EMPRE-QUE-UTILI-MECAN-DE"     
## [17] "NUMER-DE-COMPU-PROME-POR"      "TICS"                         
## [19] "USO-DE-LAS-COMPU-POR"          "USO-DE-CONEX-DE-RED"          
## [21] "INVER-EN-ID-SEGUN-SECTO"       "RELAC-DE-LAS-EMPRE-CON"       
## [23] "INVER-PROME-EN-ID-37718"       "DE-INVER-EN-ID-33111"         
## [25] "REGUL-Y-ORGAN-DE-LAS"          "INVER-EN-ID-RESPE-27207"      
## [27] "INVER-EN-ACT-RESPE-81080"      "INVER-EN-ACT-POR-TIPO"        
## [29] "INVER-EN-ACT-POR-86755"        "INVER-EN-ACT-RESPE-11094"     
## [31] "INVER-EN-ACT-RESPE-AL"         "INVER-EN-ID-RESPE-AL"         
## [33] "INVER-EN-IDPIB-VARIO-PAISE"    "DIPLO-OTORG-POR-AREA-CIENT"   
## [35] "COMPO-DEL-PRODU-INTER-BRUTO"   "INVER-EN-ID-RESPE-22546"      
## [37] "INVER-EN-ID-SEGUN-AREA"        "RED-DE-INDIC-DE-CIENC"        
## [39] "INVER-EN-ACT-SEGUN-48789"      "INVER-EN-ACT-E-ID"            
## [41] "INVER-EN-INVES-Y-DESAR"        "INVER-EN-ACTIV-CIENT-Y"       
## [43] "INVER-EN-ACT-SEGUN-SECTO"      "PROYE-DE-ID-POR-TIPO"         
## [45] "PROYE-EN-INVES-Y-DESAR"        "DE-INVER-EN-ID-DE"            
## [47] "INVER-ID-VENTA-DE-LAS"         "COMPU-PROME-POR-TAMAN-DE"     
## [49] "CONEX-DE-RED-UTILI-POR"        "INVES-CON-DOCTO-SZONA-GEO"    
## [51] "INVER-ESTIM-EN-ID-SECTO"       "INVER-PROME-EN-ID-POR"        
## [53] "INVES-SEGUN-GRADO-ACADE-38395" "INVES-SEGUN-GRADO-ACADE-Y"    
## [55] "MECAN-DE-SEGUR-INFOR-EN"       "RAZON-QUE-DIFIC-INVER-EN"     
## [57] "RAZON-QUE-DIFIC-INVER-56203"   "REGUL-DE-ACTIV-ID-EN"         
## [59] "RELAC-DE-EMPRE-CON-AGENT"      "USO-COMPU-EN-LA-NUBE"         
## [61] "USO-DE-INTER-EN-85495"         "ACTIV-DIRIG-A-INNOV-SEGUN"    
## [63] "DESTI-DE-INNOV-EMPRE-SEGUN"    "DIPLO-OTORG-SEGUN-AREA-CIENT" 
## [65] "EMPRE-INNOV"                   "EMPRE-QUE-NO-ACCED-A"         
## [67] "FACTO-QUE-OBSTA-INNOV-66943"   "FACTO-QUE-OBSTA-INNOV-EMPRE"  
## [69] "IMPAC-DE-LAS-INNOV-EMPRE"      "INVES-CON-DOCTO-SEGUN-ZONA"   
## [71] "PERSO-DEDIC-A-ID-POR"          "INVES-SEGUN-GRADO-ACADE-14316"
## [73] "PERSO-DEDIC-A-ID"              "PERSO-DEDIC-A-ID-SEGUN"       
## [75] "PRUEB-82934"                   "USO-DE-LAS-COMPU-EN"
```

