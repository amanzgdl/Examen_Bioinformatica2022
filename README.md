# Examen_Bioinformatica2022

## 1. Comandos de Linux. Tengo un fichero llamado genes.txt con 3 columnas: Nombre_de_gen, cromosoma y posición. Separados por tabulados. ¿Qué comandos utilizarías para saber?:

### ¿Cuántas palabras hay en el fichero?
#### cat genes.txt | wc 
### ¿Cómo cambiarías todos los tabulados por guiones?
#### cat genes.txt | sed 's///-/g' > genes.txt
### ¿Cómo cambiarías solo la primera aparición?
#### cat genes.txt | sed 's///-/g > genes.txt
### ¿Cómo buscarías todos los genes excepto el gen llamado DSTYK?
#### cat genes.txt | grep -v DSTYK

### Ahora vamos a responder a una serie de preguntas utilizando el dataset "weather" de nycflights13: 
```{r}
weather<- nycflights13::weather
library(tidyverse)
view(weather)
?weather
```

## 2. Preguntas básicas:

### ¿Cuántas columnas y cuántos registros tiene este dataset?
```{r}
ncol(weather) #15 columnas
nrow(weather) # 26115 registros
```
###  ¿Cuántos “origin” diferentes existen y para cada uno de ellos cuántos registros hay?
```{r}
weather %>% 
  group_by(origin) %>% 
  summarise(
    n_row = n()
  )

```

### En LGA, ¿cuáles fueron la mediana del wind_speed y media de pressure?
```{r}
weather %>% 
  filter(origin == "LGA") %>% 
  group_by(origin) %>% 
  summarise(
    mediana_ws  = median(wind_speed, na.rm = TRUE),
    media_pressure = mean(pressure, na.rm = TRUE)
  )

```

### Después de eliminar los NA de la columna wind_gust, calcula para cada mes la media de wind_speed y wind_gust, y el número de casos. 

```{r}
no_NA_weather <- 
weather %>% 
filter(!is.na(wind_gust))

weather %>%
  filter(!is.na(wind_gust)) %>% 
  group_by(month) %>% 
  summarise(
    media_ws = mean(wind_speed),
    media_wg = mean(wind_gust),
    n_casos = n()
  )
  
```

## Plot


### Crea el plot anterior
```{r}

weather_EWR <-
  weather %>% 
  filter(origin == "EWR") %>% 
  select(origin, month, temp, humid)

weather_JFK <-
  weather %>% 
  filter(origin == "JFK") %>% 
  select(origin, month, temp, humid)

weather_LGA <-
  weather %>% 
  filter(origin == "LGA") %>% 
  select(origin, month, temp, humid)

par(mfrow = c(1,3))
boxplot(weather_EWR$temp ~ weather_EWR$month, main = "EWR", xlab = "Months", ylab = "ºC", col = "red")
boxplot(weather_JFK$temp ~ weather_JFK$month, main = "JKF", xlab = "Months", ylab = "ºC", col = "green")
boxplot(weather_EWR$temp ~ weather_EWR$month, main = "LGA", xlab = "Months", ylab = "ºC", col = "blue")
```

### Crea una funcion que plotee el plot anterior
```{r}
#plot_meteo <- function(dat = weather, meteo = "temp", titulo = "Punto de rocío", unidades = "º F")
#{
# dat <- data.frame(dat)
 #dat %>% 
#   group_by(origin)
 #  par(mfrow = c(1,3)) 
  # my_plot <- boxplot(meteo ~ month, main = titulo, ylab = unidades ) 
#   { summarise(
#    r = mean(meteo)
#   )
# print(mean(meteo))
# return(r)
 #  }
#} 



#plot_meteo(meteo = "humid", titulo = "Humedad", unidades = "Relative humidity")

```

## 4. El día de tu cumpleaños:
### a. Qué correlación tuvieron la temperatura y humedad en cada uno de los origin? Plotealo mediante puntos con ggplot.
```{r}
cor.test(weather_EWR$temp, weather_EWR$humid)
cor.test(weather_JFK$temp, weather_JFK$humid)
cor.test(weather_LGA$temp, weather_LGA$humid)
#Dado que los p-values son menores de 0 podemos decir que hay una correlacion entre ambos, como es positivo podemos decir que al incrementar uno incrementa el otro.

ggplot(weather) +
  geom_point(aes(humid, temp, col = origin))
```

### b. Si comparas la temperatura en los origins JFK y LGA ¿son estadísticamentediferentes? ¿Qué p-valor consigues? Plotea los boxplots. 

```{r}
t.test(weather_JFK$temp, weather_LGA$temp)
#Parecen ser estadisitcamente diferentes ya que nos da un p-value de 0.001131 (menor de 0.05).
boxplot(weather$temp ~ weather$origin, xlab = "Origin", ylab = "ºC")
```

## 5. Observa la siguiente imagen:
### ¿Cuál es el punto con FC (Fold change) más grande en valores absolutos?

#### Mirandolo en términos absolutos sería el punto morado más cerca del - 10 logFC y del 5 en -log10(pvalue). (Si nos fijamos en los que están señalados con nombre sería el Csn1s2b)

### ¿Qué gen sobreexpresado es el más significativo?

#### Sería el gen Rbp1, ya que está sobreexpresado (FC positivo) y tiene el menor p-value (más significativo)

## 6. Sube el examen a github y escribe a continuación tu url aquí

### https://github.com/amanzgdl/Examen_Bioinformatica2022.git

## Acaba el documento con el comando sessionInfo()
```{r}
sessionInfo()
```
