library(qcc)
library(plotly)
library(htmlwidgets)
data_filtered <- X028...028[X028...028$Machine == 1 & X028...028$Temperature == 303 & X028...028$Pressure == 100, ]
xbar_one_chart_obj <- qcc(data_filtered$PartLength, type = "xbar.one", plot = FALSE)
center_line <- xbar_one_chart_obj$center
ucl_line <- xbar_one_chart_obj$limits[1,2]
lcl_line <- xbar_one_chart_obj$limits[1,1]
measurements <- xbar_one_chart_obj$data
plot_data <- data.frame(Index = 1:length(measurements), Measurement = measurements)
fig <- plot_ly(plot_data, x = ~Index, y = ~Measurement, type = 'scatter', mode = 'lines+markers', name = 'Part Length') %>% 
  add_trace(y = ~center_line, mode = 'lines', name = 'Center Line', line = list(color = '#D55E00', dash = 'dash')) %>% 
  add_trace(y = ~ucl_line, mode = 'lines', name = 'UCL', line = list(color = '#0072B2', dash = 'dash')) %>% 
  add_trace(y = ~lcl_line, mode = 'lines', name = 'LCL', line = list(color = '#0072B2', dash = 'dash')) %>% 
  layout(
    title = list(text = 'Xbar.one Chart: Part Length (Machine 1, Temp 303, Pres 100)', font = list(size = 20)),
    xaxis = list(title = list(text = 'Observation Index', font = list(size = 18)), tickfont = list(size = 14)),
    yaxis = list(title = list(text = 'Part Length', font = list(size = 18)), tickfont = list(size = 14)),
    plot_bgcolor = 'white'
  )
saveWidget(fig, file = "media/plots/xbar_one_mach1_temp303_pres100.html", selfcontained = TRUE)
