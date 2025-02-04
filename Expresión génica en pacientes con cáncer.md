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
library(gtsummary)
library(stats)
library(nortest)
library(car)
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
Generamos diagramas de caja para comprar cómo varía  la exrpresion de los genes estudiados en funcion al tipo de tratamiento:
* Tratamiento A
* Tratamiento B
```{r boxplot tratamiento}

##Análisis de Normalidad

#Filtrar por tipo de tratamiento
data_treatment <- df %>% group_by(trat)

#Realizar Test de Shapiro-Wilk a cada gen
normality_tests <- df %>% 
  pivot_longer(cols = starts_with("AQ_"), names_to = "Gen", values_to = "Value") %>% 
  group_by(Gen, trat) %>% 
  summarise(p_value = shapiro.test(Value)$p.value, .groups = "drop") %>% 
  pivot_wider(names_from = trat, values_from = p_value, names_prefix = "Tratamiento_")
normality_tests

##Calcular estadísticas descriptivas para distribuciones no-normales
# Mediana e IQR debido a la no-normalidad de los datos.
descriptive_stats <- df %>% 
  pivot_longer(cols = starts_with("AQ_"), names_to = "Gen", values_to = "Value") %>% 
  group_by(Gen, trat) %>% 
  summarise(
    Median = median(Value, na.rm = TRUE),
    IQR = IQR(Value, na.rm = TRUE),
    .groups = "drop"
  ) %>% 
  mutate(Median_IQR = paste0(Median, " (", IQR, ")")) %>% 
  select(Gen, trat, Median_IQR) %>% 
  pivot_wider(names_from = trat, values_from = Median_IQR) %>% 
  rename(`Tratamiento A` = tratA, `Tratamiento B` = tratB)
colnames(descriptive_stats) <- c("Gen", "Tratamiento A\nP50 (RIQ)", "Tratamiento B\nP50 (RIQ)")
descriptive_stats


##Aplicación de test estadísticos no paramétricos
# Prueba de Wilcoxon para la compraracion de dos grupos independientes.
gen_pvalues <- df %>% 
    pivot_longer(cols = starts_with("AQ_"), names_to = "Gen", values_to = "Value") %>% 
    group_by(Gen) %>% 
    summarise(
      p_value = tryCatch({
        wilcox.test(Value ~ trat, exact = FALSE)$p.value
      }, error = function(e) NA),
      .groups = "drop"
    )
print(gen_pvalues)


##Generación de tabla resumen que permita una interpretacion mas clara de los resultados
# Unión de estadísticas descriptivas y p-valores
tabla_resumen <- descriptive_stats %>% 
  left_join(gen_pvalues, by = "Gen") %>% 
  rename(`P-Value` = p_value)
tabla_resumen

#Formación de gráficos gen versus los dos tipos de tratamientos
Plot1 <-ggplot(df, aes(fill = trat, y = AQ_ALOX5)) + 
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.2) + 
  labs(title = "AQ_ALOX5", x = "Tratamiento", y = "Expressión") + 
  scale_fill_manual(values = c("lightpink", "lightblue")) + 
  theme_minimal() + 
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot2 <- ggplot(df, aes(fill = trat, y = AQ_CD274)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_CD274", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot3 <- ggplot(df, aes(fill = trat, y = AQ_CHKA)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_CHKA", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8),legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot4 <- ggplot(df, aes(fill = trat, y =AQ_CSF2)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_CSF2", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8),legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot5 <- ggplot(df, aes(fill = trat, y = AQ_FOXO3)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_FOX3", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot6 <- ggplot(df, aes(fill = trat, y = AQ_IL6)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_IL6", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot7 <- ggplot(df, aes(fill = trat, y = AQ_LDHA)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_LDHA", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot8 <- ggplot(df, aes(fill = trat, y = AQ_LIF)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_LIF", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot9 <- ggplot(df, aes(fill = trat, y = AQ_MAPK1)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_MAPK1", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot10 <- ggplot(df, aes(fill = trat, y = AQ_NOS2)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_NOS2", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot11 <- ggplot(df, aes(fill = trat, y = AQ_IFNG)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_IFNG", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot12 <- ggplot(df, aes(fill = trat, y = AQ_PDCD1)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_PDCD1", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot13 <- ggplot(df, aes(fill = trat, y =  AQ_PPARG)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_PPARG", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot14 <- ggplot(df, aes(fill = trat, y =  AQ_TGFB1)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_TFGB1", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

Plot15 <- ggplot(df, aes(fill = trat, y =   AQ_TNF)) +
  geom_boxplot(outlier.color = "black", outlier.size = 2, width = 0.7) +
  labs(title = "AQ_TNF", x = "Tratamiento", y = "Expression") +
  scale_fill_manual(values = c("lightpink", "lightblue")) +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5, face = "bold", size=7), axis.title.x = element_text(size = 8), axis.title.y = element_text(size = 8), legend.text = element_text(size = 6),legend.title = element_text(size = 7))

# Unión de todos los plots en una sola gráfica 
library("patchwork")
combined_plot <- Plot1 + Plot2 + Plot3 + Plot4 + Plot5 + Plot6 + Plot7 + Plot8 + Plot9 + Plot10 + Plot11 + Plot12 + Plot13 + Plot14 + Plot15 + plot_layout(ncol = 3)
print(combined_plot)
```
No se observan grandes diferencias en la expresión de los genes de interés según el tratamiento aplicado. Sin embargo, se aprecian ligeras diferencias en la expresión de los genes LIF, PD-L1, MAPK1, FOXO3, TGFB1 y CHKA, siendo mayor en los pacientes sometidos al tratamiento A.

### Distribución de la expresión de genes comparados por tipo de tumor

En primer lugar creamos un vector que contiene todos los genes a estudiar. Luego realizamos gráficos de caja para poder observar cómo varía la expresión de los genes estudiados en función del tipo de tumor:
* CCR o cáncer colorectal
* CM o cáncer de mama
* CP o cáncer de pulmón

```{r boxplot tumor}
for (gen in genes) {
  boxplot <- df %>%
    ggplot(aes(x = tumor, y = .data[[gen]], fill = tumor)) +
    geom_boxplot() +
    scale_fill_manual(values = c("CM" = "lightcoral", "CCR" = "lightblue", "CP" = "lightgreen")) +
    labs(
      title = paste("Distribucion de la expresión de", gen, "por tipo de tumor"),
      x = "Tipo de Tumor",
      y = "Nivel de Expresión"
    ) +
    theme_classic() +
    theme(legend.position = "none")
  print(boxplot)
}
```

Para la mayoría de los genes, se observa una mayor expresión génica en pacientes con cáncer de pulmón, seguida por aquellos con cáncer de mama y por último con tumor colorrectal.

### Distribución de la expresión de genes según la edad
En función de la edad, tendremos 2 categorías según la mediana de los datos (categoría 1: edad <percentil 50 de edad; categoría 2: edad >=percentil 50 de edad). 
Tenemos una variable categórica que es edad y una numérica que es genes, como n es mayor que 30, pasamos a mirar la homogeneidad de varianzas.
```{r}
# Calcular el percentil 50 (mediana) de la variable 'edad'
percentil_50 <- median(df$edad, na.rm = TRUE)

# Crear una nueva variable categórica basada en el percentil 50
categoria_edad <- ifelse(df$edad < percentil_50, "Categoría 1: edad < percentil_50", "Categoría 2: edad <= percentil_50")

# Filtrar columnas que comienzan con "AQ_" para los genes 
genes <- df %>% select(starts_with("AQ_"))

# Realizar el test de Levene
# Para asegurar de que categoria_edad es un factor utilizamos as.factor

categoria_edad <- as.factor(categoria_edad)

# Aplicar el test de Levene a cada columna en genes
resultados_levene <- genes %>%
  summarise(across(
    everything(),
    ~ leveneTest(.x, categoria_edad)$`Pr(>F)`[1],
    .names = "levene_{.col}"
  ))

print(resultados_levene)

# Filtrar valores p menores a 0.05 en resultados_levene
valores_significativos <- resultados_levene %>%
  select(where(is.numeric)) %>% # Seleccionar solo columnas numéricas
  summarise(across(everything(), ~ any(.x < 0.05), .names = "es_significativo_{.col}"))

# Mostrar las columnas con valores significativos
print(valores_significativos)

# Hemos encontrado que el gen AQ_IL6 tiene un valor de 0'012, no cumple con la homogeneidad de varianzas. 
# Aplicaremos el test de Welch al gen AQ_IL6, y el t-student a los genes con homogeneidad.

# Aplicamos test estadisticos
gen_welch <- c("AQ_IL6")
genes_t.test <- genes %>% select(-AQ_IL6)
genes_t.test <- colnames(genes_t.test)

df_tabla <- cbind(genes, categoria_edad)
df_tabla <- na.omit(df_tabla)
any(is.na(df_tabla))

tabla3 <- df_tabla %>%
  tbl_summary(
    by = categoria_edad,
    statistic = all_continuous() ~ "{median} ({p25} - {p75})",
    digits = all_continuous() ~ function(x) format(x, digits = 2, scientific = TRUE)
  ) %>%
  add_p(
    test = list(
      all_of(as.character(genes_t.test)) ~ "t.test", 
      all_of(as.character(gen_welch)) ~ function(x) {
        # Aplicar t-test con var.equal = FALSE para Welch
        test_result <- t.test(gen_welch ~ df_tabla$categoria_edad, var.equal = FALSE)
        return(test_result$p.value)
      }
    ),
    pvalue_fun = ~ ifelse(.x < 0.05, paste0(style_pvalue(.x, digits = 3), "*"), style_pvalue(.x, digits = 3))
  )
tabla3

# Al no aparecer el valor de p-value del test de Welch en la tabla3, lo calcularemos a parte:
test_resultwelch <- t.test(df$AQ_IL6 ~ df_tabla$categoria_edad, var.equal = FALSE)
print(test_resultwelch)
```

En ninguna de las dos categorías, se rechaza la hipótesis nula al tener valores de p-value superiores a 0'05. Por lo tanto, las medias de los dos grupos son iguales.

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
