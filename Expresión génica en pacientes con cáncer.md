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

```

### Distribución de la expresión de genes comparados por tipo de tratamiento

### Distribución de la expresión de genes comparados por tipo de tumor

Realizamos gráficos de caja para poder observar cómo varía la expresión de los genes estudiados en función del tipo de tumor:
* CCR o cáncer colorectal
* CM o cáncer de mama
* CP o cáncer de pulmón
Para ver cómo varia la expresion de todos los genes de mi base de datos, primero voy a crear un vector llamado "genes" dónde voy a guardar los niveles de expresión de cada uno de ellos, para después poder hacer el grafico de cajas completo. 
```{r boxplot tumor}
#crear el vector con los genes
genes <- df %>%
  select(starts_with("AQ_")
#boxplot utilizando un condicionante
for (gen in genes) { 
  boxplot_tipotumor <- ggplot(data = df, aes(x = tumor, y = .data[[gen]], fill = tumor)) +
    geom_boxplot(color = "black") +
    labs(
      title = paste("Distribucion por tipo de tumor"),
      x = "Tipo de Tumor",
      y = "Nivel de Expresión"
    ) +
    theme_classic() +
    theme(
      plot.title = element_text(hjust = 0.5, face = "bold"),
      axis.title = element_text(face = "bold")
    ) +
    scale_fill_manual(values = c("CCR" = "yelllow", "CM" = "blue", "CP" = "green"))
```

Para la mayoría de los genes, se observa una mayor expresión génica en pacientes con cáncer de pulmón, seguida por aquellos con cáncer de mama y por último con tumor colorrectal.

### Distribución de la expresión de genes sgún la edad

### Mapeo del perfil de expresión génica
Mapeamos los valores de expresión de genes para poder visualizar posibles patrones entre los datos de los pacientes realizando un Heatmap. El script que utilizamos es el siguiente:

```{r}
datos2 <- df %>%
  select(starts_with("AQ_") -AQ_ADIPOQ, -AQ_NOX5)
  as.matrix(df)
```
```{r}
rownames(df) <- datos2$id 
scale(df)
set.seed(1995)
map <- pheatmap(datos2, 
                main = "Perfil de Expresión Génica",
                scale = "row", 
                cluster_cols = TRUE,
                cluster_rows = TRUE, legend_breaks = c(-3, 3, 4.9),
                legend_labels = c("Baja", "Alta", "Expresión\n"),
                legend = TRUE,
                color = colorRampPalette(c("blue", "ivory", "red"))(10000))
```
Obervando el gráfico, sacamos las siguientes conclusiones: 
- El gen CCL5, que codifica para una quimiocina implicada en la respuesta inflamatoria, está ampliamente expresado en la mayoría de los pacientes, siendo el que tiene un mayor nivel de expresión generalmente, excepto en 3 pacientes.
- Otro gen que se expresa altamente en la mayoría de los pacientes es el TGB1, lo cual es lógico ya que codifica para (TGF-β1), una citocina multifuncional con un papel central en la regulación de una amplia variedad de procesos biológicos.
- El gen GPX1 está altamente expresado en dos pacientes y en un menor nivel en 6 pacientes. Este gen codifica para una enzima que protege del daño oxidativo. 
- El gen FOX03 regula la resistencia al estrés y está expresado en alto nivel más o menos en la mitad de los pacientes y en la otra en un nivel menor.
- Los genes ADIPOQ y NOX5 solamente están expresados altamente en un paciente, por eso se eliminaron previamente, para no interferir en los resultados. 
