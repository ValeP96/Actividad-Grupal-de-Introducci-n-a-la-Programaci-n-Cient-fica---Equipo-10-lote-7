# Expresión génica en pacientes con cáncer

### Introducción

Se analiza a continuación un dataset que contiene información de la expresión de 46 genes en 65 pacientes, cada uno con distintos tipos de tratamiento y características tumorales. La expresión génica puede proporcionar información clave sobre cómo responden diferentes pacientes a tratamientos específicos, especialmente en el contexto del cáncer. Este informe analiza la expresión de genes claves en pacientes bajo dos tratamientos (A y B), con el objetivo de identificar patrones de expresión asociados a cada tratamiento.

En primer lugar, importamos el dataset bajo el nombre df:

```{r}
df <- read_csv("Dataset expresión genes.csv")
```
Deben instalarse y cargarse los paquetes apropiados. En este caso utilizamos tidyverse para asistir en el análisis de los datos, así como ggplot2 y pheatmap para realizar los gráficos pertinentes. También se utiliza patchwork para unir los gráficos individuales de un mismo análisis.

```{r}
library(tidyverse)
library(ggplot2)
library(pheatmap)
library(patchwork)
```

### Parámetros bioquímicos

En los histogramas de *Parámetros bioquímicos* se representa la concentración de diferentes biomoléculas en pacientes con cáncer. Dependiendo de la biomolécula analizada, se pueden observar distintas distribuciones de datos.

```{r histogramas bioquimica}
# Creación de los histogramas de cada molécula bioquímica a analizar
glucosa <- ggplot(df,aes(x=glucosa)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Glucosa",x="Nivel de glucosa (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "red")

leucocitos <- ggplot(df,aes(x=leucocitos)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Leucocitos",x="Nivel de leucocitos (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "blue")

linfocitos <- ggplot(df,aes(x=linfocitos)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Linfocitos",x="Nivel de linfocitos (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "green")

neutrofilos <- ggplot(df,aes(x=neutrofilos)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Neutrófilos",x="Nivel de neutrófilos (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "yellow")

chol <- ggplot(df,aes(x=chol)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Chol",x="Nivel de chol (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "red")

hdl <- ggplot(df,aes(x=hdl)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="HDL",x="Nivel de HDL (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "blue")


hierro <- ggplot(df,aes(x=hierro)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Hierro",x="Nivel de hierro (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "green")

igA <- ggplot(df,aes(x=igA)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="IgA",x="Nivel de igA (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "yellow")

igE <- ggplot(df,aes(x=igE)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="IgE",x="Nivel de igE (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "red")

igG <- ggplot(df,aes(x=igG)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="IgG",x="Nivel de igG (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "blue")

igN <- ggplot(df,aes(x=igN)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="IgN",x="Nivel de igN (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "green")

ldl <- ggplot(df,aes(x=ldl)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="LDL",x="Nivel de LDL (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "yellow")

pcr <- ggplot(df,aes(x=pcr)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="PCR",x="Nivel de PCR (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "red")

transferrina <- ggplot(df,aes(x=transferrina)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Transferrina",x="Nivel de transferrina (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "blue")

trigliceridos <- ggplot(df,aes(x=trigliceridos)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="Triglicéridos",x="Nivel de triglicéridos (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "green")

cpk <- ggplot(df,aes(x=cpk)) + 
  geom_histogram(aes(y=..density..),fill =c("powderblue"), color="black", bins = 30) +
  labs(title="CPK",x="Nivel de CPK (md/dL)", y="Frecuencia") +
  theme_minimal() +
  theme(plot.title= element_text(hjust=0.5, face="bold")) +
  geom_density(linewidth=0.6, color = "yellow")

# Unión de todos los gráficos individuales
freq_bioquim <- (glucosa + leucocitos + linfocitos + neutrofilos + chol + hdl + hierro + 
                   igA + igE + igG + igN + ldl + pcr + transferrina + trigliceridos + cpk) + 
  plot_layout(ncol=4) +
  plot_annotation(title="Concentración de moléculas bioquímicas en los pacientes", 
                  theme=theme(plot.title=element_text(hjust=0.5, size = 20)))
freq_bioquim
```

### Distribución de la expresión de genes comparados por tipo de tratamiento

### Distribución de la expresión de genes comparados por tipo de tumor

Realizamos gráficos de caja para poder observar cómo varía la expresión de los genes estudiados en función del tipo de tumor:
* CCR o cáncer colorectal
* CM o cáncer de mama
* CP o cáncer de pulmón

```{r boxplot tumor}

```

Para la mayoría de los genes, se observa una mayor expresión génica en pacientes con cáncer de pulmón, seguida por aquellos con cáncer de mama y por último con tumor colorrectal.

### Distribución de la expresión de genes sgún la edad

### Mapeo del perfil de expresión génica

