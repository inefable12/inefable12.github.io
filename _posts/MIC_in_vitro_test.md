---
layout: post
author: Jesus Alvarado
title: Gráfica MIC con Matplotlib
---

![800x600](https://www.delftstack.com/img/Matplotlib/Matplotlib%20plot%20two%20histograms%20at%20the%20same%20time%20without%20overlapping%20bars.png?ezimgfmt=rs:640x480/rscb5/ng:webp/ngcb5)

## Qué es un MIC?

Es la concentración inhibitoria necesaria para inhibir el crecimiento de la mitad de la población bacteriana. 
Hace poco recibimos unos resultados del MIC de 2 compuestos en 2 cepas diferentes de Mycobacterium tuberculosis: la pirazinamida y nuestro compuesto que podemos llamar L1 (o ligando número 1).
Una manera bastante práctica de interpretar estos resultados es utilizando un diagrama de barras, entonces aquí veremos cómo generar una gráfica de barras para este caso:

> Datos: 


> Una gráfica de barras conteniendo el docking score de estos complejos en una enzima, en su forma nativa y 5 mutantes.

## 1. Reporte de complejos   

Cuando hablamos de complejos o compuestos de coordinación en solución, acuosa por ejemplo, nos enfrentamos a un sistema dinámico que puede intercambiar algún o algunos ligandos con su entorno, con moléculas de agua. Esta especiación se va a encontrar intrinsecamente asociada con factores cinéticos y termodinámicos. En este trabajo diseñamos 22 estructuras, donde algunos complejos podían contener 1 o más intercambios de ligando con agua, mantener ligandos sulfatos o pirazinamida, además de modificar su carga total. 

> El objetivo es resumir el contenido de ligandos en el complejo

`Paso 1` Importar librerías

Numpy y Pandas serán abordados en un siguiente apartado. En este caso nos enfocaremos unicamente en matplotlib:

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

> Según David Zarruk, profesor del curso de analítica predictiva y modelos de regresión en Python, una librería de programación es un conjunto de funciones que alguien escribió en alguna parte del mundo y ha disponibilizado para que cualquiera pueda utilizarlo de forma gratuita. 

Una librería en Python es un conjunto de implementos funcionales que te ayudarán a codificar todo este lenguaje de programación para crear una interfaz independiente. Las librerías de Python son amplias y cuentan con gran cantidad de producciones en contenidos. Constan de diversos módulos que permiten el acceso de funcionalidades específicas del sistema como entrada y salida de archivos, soluciones estandarizadas a problemas de programación, etc. Además, dependiendo del sistema operativo que tengas, puedes conseguir diferentes funciones de cada una de las librerías de Python. Por ejemplo, para el sistema Windows se incluye la biblioteca estándar completa junto con componentes adicionales. Un plus para las librerías de Python es que cuentan con una colección de componentes como, por ejemplo, programas individuales, módulos, paquetes, frameworks, aplicaciones y más funciones que puedes encontrar en Python Package Index.

`Paso 2` Cargar data

En este caso emplearemos un archivo csv que se encuentra en nuestro drive. Abrimos un nuevo cuaderno de código (notebook), disponible en https://colab.research.google.com/, lo conectamos a drive y nos dirigimos a la carpeta que contiene el csv:

```
%cd /content/drive/MyDrive/0_3_python/graficas_jm
!ls
```
- Output:

```
/content/drive/MyDrive/0_3_python/graficas_jm
Libro1.csv
```
`Paso 3` Invocamos pandas para visualizar la data

Para que la primera columna sea tomada como índice se considera index_col 

```
j1 = pd.read_csv('Libro1.csv',index_col='Molecule')
j1.head(3)
```
- Output

Molecule | Total_chrg | PZA_lig	| PZA_lig_at |	Water_lig |	Sulfate
--- | --- | --- | --- | --- | ---
PZA | 0 | 1 |	0 |	0 |	0
FePZA_1 |	2 |	2 |	4 |	2 |	0
FePZA_1i	| 2	| 2	| 4	| 2	| 0

`Paso 4` Identificamos las columnas
```
j1.columns
```
- Output

```
Index(['Total_charge', 'PZA_ligand', 'PZA_ligant_atoms', 'Water_ligand',
       'Sulfate'],
      dtype='object')
```

`Paso 5` Creación de DataFrame

Para que el contenido de enteros sea tratado como tal, requerimos crear un DataFrame, que estamos guardando en la variable j2 (y nuevamente visualizando su encabezado)

```
j2 = pd.DataFrame(j1, columns = ['Total_charge',
                                 'PZA_ligand','PZA_ligant_atoms',
                                 'Water_ligand', 'Sulfate'],dtype='object')
j2.head(3)
```
```
type(j2['Total_charge'][0])
```
- Output

```
int
```

`Paso 6` Creación de index

Podemos asignar un índice en particular, en este caso tomando la salida de j2.columns o j1.columns asignamos la lista de elementos en el índice a la variable index_1:

```
index_1 = ['PZA','FePZA_1','FePZA_1i','FePZA','FePZAi','FePZA_1SO4','FePZA_1SO4i','FePZA_2SO4','FePZA_2SO4i','FePZA_2SO4-pza','FePZA_2SO4-pzai','FePZA_2SO4-pza2','ZnPZA_1','ZnPZA_1i','ZnPZA','ZnPZAi','ZnPZA_1SO4','ZnPZA_1SO4i','ZnPZA_2SO4','ZnPZA_2SO4i','ZnPZA_2SO4-pza','ZnPZA_2SO4-pzai','ZnPZA_2SO4-pza2']
index_1
```
- Output

```
['PZA',
 'FePZA_1',
 'FePZA_1i',
 'FePZA',
 'FePZAi',
 'FePZA_1SO4',
 'FePZA_1SO4i',
 'FePZA_2SO4',
 'FePZA_2SO4i',
 'FePZA_2SO4-pza',
 'FePZA_2SO4-pzai',
 'FePZA_2SO4-pza2',
 'ZnPZA_1',
 'ZnPZA_1i',
 'ZnPZA',
 'ZnPZAi',
 'ZnPZA_1SO4',
 'ZnPZA_1SO4i',
 'ZnPZA_2SO4',
 'ZnPZA_2SO4i',
 'ZnPZA_2SO4-pza',
 'ZnPZA_2SO4-pzai',
 'ZnPZA_2SO4-pza2']
```

`Paso 7` Gráfica

Cada una de las columnas van asociadas a una variable, estas luego son indexadas a un nuevo DataFrame df1, indicándose además el índice.

> nombre_dataframe.plot.bar es el comando para generar la gráfica de barras

Los rótulos, por ejemplo del eje X, pueden tener un ángulo de rotación, esto se indica con rot=90, en nuestro caso vertical.
"Figure" y figsize nos permiten modificar la proporción entre horizontal y vertical.

```
#Columnas
Total_charge = list(j2['Total_charge'])
PZA_ligand = list(j2['PZA_ligand'])
PZA_ligant_atoms = list(j2['PZA_ligant_atoms'])
Water_ligand  = list(j2['Water_ligand'])
Sulfate = list(j2['Sulfate'])
#Rótulos
df1 = pd.DataFrame({'Total_charge':Total_charge,
                    'PZA_ligand':PZA_ligand,
                    'PZA_ligant_atoms':PZA_ligant_atoms,
                    'Water_ligand':Water_ligand,
                    'Sulfate':Sulfate
                    }, 
                   index=index_1)
ax = df1.plot.bar(rot=90)
plt.figure(figsize=(40,5))
plt.show()
```

- Output

![download](https://user-images.githubusercontent.com/25043666/147608282-5ea0ad6d-a0d1-4390-8ab1-6bbaf68a31a3.png)

`Paso 8` Guardar la imagen vectorizada

```
fig = ax.get_figure()
fig.savefig('jes.svg',dpi=600)
```

> La imagen generada se puede guardar en diversos formatos, sin embargo, disponer de la imagen vectorizada es un lujo. Programas como Inkscape (libre) o CorelDraw ($) nos permiten personalizar aún más la gráfica, mejorar la resolución, entre otras cosas.


## 2. Docking score   

`Paso 1` Idem a la parte 1

Teniendo en cuenta que requerimos disponer de las librerías, pasamos a importar la data usando pandas.

```
#docking score
ds = pd.read_csv('Libro2.csv',index_col='Molecule')
ds.head(23)
```

`Paso 2` Exploración de datos & DataFrame

Comandos como "ds.info()" y "ds.columns" nos permitirán verificar la data importada.

```
ds1 = pd.DataFrame(ds, columns = ['3PL1-WT', 'Mut-ASN49', 
                                  'Mut-ARG51', 'Mut-ARG57', 
                                  'Mut-TYR71', 'Mut-ALA139'],
                   dtype='object')
ds1.head(3)
```

- Output

Molecule | 3PL1-WT | Mut-ASN49 | Mut-ARG51 | Mut-ARG57 | Mut-TYR71 | Mut-ALA139
--- | --- | --- | --- | --- | --- | ---
PZA | -5.723 | -6.475 | -6.434 | -5.905 | -6.244 | -6.432
FePZA_1 | -8.588 | -9.175 | -9.698 | -6.428 | -8.126 | -9.444


`Paso 3` Gráfica

```
#Columnas
WT = list(ds1['3PL1-WT'])
Mut_ASN49 = list(ds1['Mut-ASN49'])
Mut_ARG51 = list(ds1['Mut-ARG51'])
Mut_TYR71  = list(ds1['Mut-TYR71'])
Mut_ALA139 = list(ds1['Mut-ALA139'])
#Rótulos
ds2 = pd.DataFrame({'WT':WT,
                    'Mut_ASN49':Mut_ASN49,
                    'Mut_ARG51':Mut_ARG51,
                    'Mut_TYR71':Mut_TYR71,
                    'Mut_ALA139':Mut_ALA139
                    }, 
                   index=index_ds1)
ax = ds2.plot(kind='bar', rot=90, width=0.6)#, linewidth=2.0)
plt.xlabel('pH 6.0', fontsize=12, fontweight='bold')
plt.ylabel('Docking score (kcal/mol)', fontsize=12, fontweight='bold')
#plt.figure(figsize=(5,3))#,dpi=None)
plt.legend(bbox_to_anchor=(0.98, 0.40), loc=1, borderaxespad=0)
plt.show()
```
- Output