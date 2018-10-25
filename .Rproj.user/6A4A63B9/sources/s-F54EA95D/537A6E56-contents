
library(ggplot2)
library(leaflet)
library(readr)
library(eurostat)
library(RColorBrewer)
library(dplyr)
library(tidyr)

carceri <- read_csv2("data/Foglio1.csv")

popup_sov <- paste0(
  "Struttura: ", carceri$Struttura,
  "<br />Capienza: ", carceri$Capienza,
  ifelse(carceri$sovraffollamento < 0,
         paste0("<br />Sovraffollamento: ", carceri$sovraffollamento*-1),""))

# Tentativo fallito
pal_sov <- case_when(
  carceri$sovraffollamento > 0 ~ colorNumeric("Greens", domain = carceri$sovraffollamento),
  TRUE ~ colorNumeric("OrRd", domain = carceri$sovraffollamento))

pal_sov_pos <- colorNumeric("Greens", domain = carceri$sovraffollamento)
pal_sov_neg <- colorNumeric("OrRd", domain = carceri$sovraffollamento)

leaflet(carceri) %>% 
  addProviderTiles("Stamen.Toner",
                   options = providerTileOptions(opacity = .7)) %>% 
  setView(12.646361, 42.504154, zoom = 6) %>% 
  addCircles(data=carceri, ~Long, ~Lat, popup=popup_sov, weight = 5, radius = ~Presenti*10,
             color = ~pal_sov(carceri$sovraffollamento), stroke = T, fillOpacity = 0.8) 

#Elimino NA

carceri_sub <- subset(carceri, !is.na(carceri$sovraffollamento))

# Metodo da Stackoverflow per creare una palette personale

## Make vector of colors for values smaller than 0 (20 colors)
rc1 <- colorRampPalette(colors = c("dark red", "white"), space = "Lab")(468)

## Make vector of colors for values larger than 0 (180 colors)
rc2 <- colorRampPalette(colors = c("white", "dark green"), space = "Lab")(218)

## Combine the two color palettes
rampcols <- c(rc1, rc2)

mypal <- colorNumeric(palette = rampcols, domain = carceri$sovraffollamento)

## If you want to preview the color range, run the following code
#previewColors(colorNumeric(palette = rampcols, domain = NULL), values = -468:218)

leaflet(carceri_sub) %>% 
  addProviderTiles("CartoDB.DarkMatter",
                   options = providerTileOptions(opacity = .8)) %>% 
  setView(12.646361, 42.504154, zoom = 6) %>% 
  addCircles(data=carceri, ~Long, ~Lat, popup=popup_sov, weight = 5, radius = ~Presenti*10,
             color = ~mypal(carceri$sovraffollamento),
             stroke = T, fillOpacity = .9)
