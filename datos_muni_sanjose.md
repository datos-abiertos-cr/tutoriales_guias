Datos disponibles
-----------------

Gracias a la iniciativa de [Gobierno
Abierto](http://www.gobiernoabierto.go.cr/) de Costa Rica tenemos la
posibilidad de obtener datos de diferentes entes a través de la conexión
con API's.

Esto nos da la posibilidad de no pasar por engorrosos procesos de
limpiar datos a partir de formatos casi imposibles como pdf's, .csv
extravagantes u otros formatos difíciles de trabajar.

En este caso vamos a utilizar el lenguaje de programación R y el paquete
[junr](https://cran.r-project.org/web/packages/junr/index.html) de
[Frans van Dunné](https://github.com/FvD) para conectarnos al API de la
municipalidad de San José y revisar qué datos se encuentran disponibles.

### Obtener el API key de la municipalidad de San José

En la siguiente [dirección](http://datosabiertos.msj.go.cr/developers/)
podrá encontrar el API key necesario para hacer el llamado.

Vamos a crear como objetos nuestras credenciales para hacer la conexión
al API de la municipalidad de San José. El url que aparece acá es el que
se debe de usar para conectarnos al sistema de la muni de San José. Si
están siguiendo ete ejemplo pueden escribir el mismo que aparece acá.

    url_base <- "http://api.datosabiertos.msj.go.cr/api/v2/datastreams/"
    api_key <- SU_API_KEY

### Obtener el indice de las tablas

Ya que tenemos las credenciales listas (api key & url), vamos a revisar
el índice de las tablas de datos que la municipalidad ha liberado:

    # Traemos el indice  de los datos que existen
    indice <- get_index(url_base, api_key = api_key)

    # Revisamos estructura del objeto 
    glimpse(indice)

    ## Observations: 257
    ## Variables: 18
    ## $ status          <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ description     <chr> "Valor absoluto y Tasa por 100.000 2006 - 2014",…
    ## $ parameters      <list> [[], [], [], [], [], [], [], [], [], [], [], []…
    ## $ tags            <list> [<"san jose", "municipalidad", "muertes violent…
    ## $ timestamp       <dbl> 1.547586e+12, 1.539808e+12, 1.516222e+12, 1.5162…
    ## $ created_at      <int> 1547586293, 1539744226, 1516221668, 1516220839, …
    ## $ title           <chr> "Cantidad y Tasa de muertes violentas", "Densida…
    ## $ modified_at     <int> 1547586492, 1539808089, 1516221733, 1516221641, …
    ## $ category_id     <chr> "83642", "83639", "83481", "83481", "83482", "83…
    ## $ methods         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ sources         <list> [<>, <"Municipalidad de San José", "Instituto N…
    ## $ total_revisions <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ frequency       <chr> "", "ondemand", "", "", "", "ondemand", "ondeman…
    ## $ link            <chr> "http://datosabiertos.msj.go.cr/dataviews/250481…
    ## $ user            <chr> "sanjose", "sanjose", "sanjose", "sanjose", "Msj…
    ## $ status_str      <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    ## $ guid            <chr> "CANTI-Y-TASA-DE-92062", "DENSI-DE-POBLA-POR-DIS…
    ## $ category_name   <chr> "Características Sociales", "Demografía", "Educa…

Cada uno de los elementos que allí aparecen son distintas tablas que
pueden tener diferentes dimensiones (filas y columnas). Es como una
descripción de lo que trata cada tabla. Si queremos traer a nuestro
entorno los datos de una tabla en específico tenemos que hacerlo con su
**GUID**.

De toda la tabla que llamamos en el paso anterior, vamos a hacer una
selección de dos columnas: título y el GUID

    guid <- indice %>% 
      select(title, guid)

    glimpse(guid)

    ## Observations: 257
    ## Variables: 2
    ## $ title <chr> "Cantidad y Tasa de muertes violentas", "Densidad de Pobla…
    ## $ guid  <chr> "CANTI-Y-TASA-DE-92062", "DENSI-DE-POBLA-POR-DISTR", "DIST…

### Obtener las dimensiones de las tablas

Ahora bien, ya sabemos el título y el identificador de la tabla, pero
tenemos que tener cuidado. Son muchas tablas y algunas pueden estar
vacías, ser datos resumidos o con muy pocos datos que no nos servirían
de nada.

Por suerte el paquete **junr** tiene una función que nos muestra la
dimensión de las tablas, lo cual nos da mayor información sobre las
características que tiene.

Hay que aclarar que la municipalidad de San José tiene muchas tablas y
si usamos la función para obtener las dimensiones se puede demorar sus
minutos.

    # Por la cantidad de tablas que hay, esta funcion se demora mucho
    dimensiones <- get_dimensions(base_url = url_base, api_key = api_key)

    # Mostrar primeras seis observaciones
    head(dimensiones)

    ##                          GUID NROW NCOL DIM
    ## 2       CANTI-Y-TASA-DE-92062    6   10  60
    ## 21   DENSI-DE-POBLA-POR-DISTR   15   12 180
    ## 3  DISTR-RELAT-DE-ESTUD-MATRI   12   10 120
    ## 4  DISTR-DE-ESTUD-MATRI-77173   12   10 120
    ## 5        POA-PRESU-ORDIN-2018  119    3 357
    ## 6     INVER-EN-COLON-EN-43060    7    6  42

#### Cuadro con características de las tablas de la muni de SJ

Ya que tenemos las dimensiones de las tablas, podemos acomodar la
información que hemos extraido hasta el momento de tal manera que
tengamos un cuadro final con la información importante resumida: el
título de la tabla, el GUID y las dimensiones.

    # Unir el titulo a dimensiones
    dimensiones <- left_join(dimensiones, guid, by = c("GUID" = "guid"))

    # Ordenar tablas con mayores dimensiones
    dim_ordenado <- dimensiones %>%
        arrange(desc(DIM))

    # Revisar primeras entradas
    head(dim_ordenado)

    ##                         GUID NROW NCOL DIM
    ## 1          ASIST-EDUCA-SUPER  108    9 972
    ## 2                DEFUN-28990   64   13 832
    ## 3                DEFUN-43464   64   13 832
    ## 4                NACIM-43692   64   13 832
    ## 5 DISTR-DE-POBLA-OCUPA-56023   52   11 572
    ## 6 DISTR-DE-POBLA-OCUPA-36314   52   11 572
    ##                                                                title
    ## 1                                      Asistencia Educación Superior
    ## 2                                                        Defunciones
    ## 3                                                        Defunciones
    ## 4                                                        Nacimientos
    ## 5 Distribución de población ocupada por Grupo Ocupacional (Relativo)
    ## 6            Distribución de población ocupada por Grupo Ocupacional

### Llamar datos de una tabla específica:

Ya con esto podemos hacer una mejor selección de las tablas que nos
serían útiles. Cuando ya tengamos identificada una tabla tenemos que
anotar su GUID que utilizaremos en la función:

    # Anotar en un objeto el GUID de la tabla que nos interesa
    guid_tabla <- "ASIST-EDUCA-SUPER"

    # Llamar la tabla. Usamos las mismas credenciales que habíamos usado
    asist_educacion <- get_data(base_url = url_base, api_key = SU_API_KEY, 
                                guid = guid_tabla)

¡Listo! Ya tenemos nuestra tabla de interés con los datos. Vamos a
revisarla

    head(asist_educacion)

    ##   Distrito   Sexo Tenencia de título Parauniversitaria Universitaria Total
    ## 2 San José Hombre                 Sí              1951         24058 26009
    ## 3 San José Hombre                 No               653          3498  4151
    ## 4 San José Hombre              Total              2604         27556 30160
    ## 5 San José  Mujer                 Sí              2599         26085 28684
    ## 6 San José  Mujer                 No               924          4092  5016
    ## 7 San José  Mujer              Total              3523         30177 33700
    ##   Población de 17 años y más
    ## 2                           
    ## 3                           
    ## 4                     100238
    ## 5                           
    ## 6                           
    ## 7                     116461
    ##   Porcentaje de población con educación superior
    ## 2                                               
    ## 3                                               
    ## 4                                           30.1
    ## 5                                               
    ## 6                                               
    ## 7                                           28.9
    ##   Porcentaje de población con educación superior y título
    ## 2                                                        
    ## 3                                                        
    ## 4                                                    86.2
    ## 5                                                        
    ## 6                                                        
    ## 7                                                    85.1

    glimpse(asist_educacion)

    ## Observations: 108
    ## Variables: 9
    ## $ Distrito                                                  <fct> San Jo…
    ## $ Sexo                                                      <fct> Hombre…
    ## $ `Tenencia de título`                                      <fct> Sí, No…
    ## $ Parauniversitaria                                         <fct> 1951, …
    ## $ Universitaria                                             <fct> 24058,…
    ## $ Total                                                     <fct> 26009,…
    ## $ `Población de 17 años y más`                              <fct> , , 10…
    ## $ `Porcentaje de población con educación superior`          <fct> , , 30…
    ## $ `Porcentaje de población con educación superior y título` <fct> , , 86…

Referencias
-----------

-   [vignette del paquete
    junr](https://cran.r-project.org/web/packages/junr/vignettes/acceder-junar-api.html)
-   [Página desarrolladores Municipalidad de San
    José](http://datosabiertos.msj.go.cr/developers/)
-   [Gobierno abierto Costa Rica](http://www.gobiernoabierto.go.cr/)
