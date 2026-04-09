Documentación del desarrollo de gráficos e indicadores en Power BI



1\. Selección de Top 5 empresas por década

&#x20;  Para evitar saturar la visualización y destacar la reestructuración del sector, se limitó el análisis a las 5 organizaciones con mayor cantidad de misiones por década.



Primero, se creó una medida para contabilizar las misiones:

Misiones = COUNTROWS('TD\_MISIONES')



Luego, se definió una medida de ranking para ordenar las empresas según la cantidad de misiones dentro del contexto del visual:



Rank Top 5 =

IF(

ISINSCOPE('TD\_MISIONES'\[Company]),

RANKX(

ALLSELECTED('TD\_MISIONES'\[Company]),

\[Misiones],

,

DESC

)

)



Finalmente, se aplicó un filtro en el visual para mostrar únicamente aquellas compañias con ranking menor o igual a 5.



2\. Transformación y tipado de datos

&#x20;  El dataset original presentaba inconsistencias en los tipos de datos, por lo que se realizó una normalización en Power Query. Se definieron los tipos adecuados para cada columna, lo que permitió asegurar la correcta interpretación y análisis de la información:



= Table.TransformColumnTypes(

\#"Encabezados promovidos",

{

{"Company", type text},

{"Location", type text},

{"Date", type date},

{"Time", type time},

{"Rocket", type text},

{"Mission", type text},

{"RocketStatus", type text},

{"Price", Currency.Type},

{"MissionStatus", type text}

}

)



3\. Creación de variables temporales

&#x20;  A partir de la columna Date se generaron nuevas variables para facilitar el análisis temporal.



Primero, se extrajo el año:

Year = YEAR(\[Date])



Luego, se creó la columna Decade para agrupar los datos por década:



= Table.AddColumn(

\#"Columnas con nombre cambiado",

"Decade",

each Number.IntegerDivide(\[Year], 10) \* 10

)



Estas transformaciones permiten analizar tendencias de largo plazo y simplificar la interpretación de los datos.



