### Suggested Repo Name
**`VolvoServiceCSIAnalysis`**  
- **Why**: Highlights the dual focus on service complaints (models, issues) and CSI trends, while keeping it concise and descriptive.

---

### README.md

```markdown
# Volvo Service & CSI Analysis (2021–2025)

This repository explores Volvo’s service complaints and Customer Satisfaction Index (CSI) trends across U.S. regions from 2021 to 2025, focusing on how model-specific issues (e.g., XC60 electrical problems, VNL Trucks’ recalls) impact satisfaction and Volvo’s corrective actions. Built in RStudio, it features a suite of advanced visualizations—from bar plots to 3D interactive networks—revealing the interplay between complaints, models, and CSI recovery.

## Project Structure

- **Data Files**:
  - `volvo_service_complaints_2021_2025.csv`: Service complaints by year and type.
  - `volvo_model_complaints_2021_2025.csv`: Model-specific complaint counts.
  - `volvo_csi_dips_2021_2025.csv`: CSI scores with dips by region.
  - `volvo_csi_post_fix_2025.csv`: Pre- and Post-Fix CSI scores for 2025.

- **R Scripts**:
  - `volvo_complaints_bar.R`: Bar plot of complaints by year/type.
  - `volvo_model_complaints_stack.R`: Stacked bar plot of model complaints.
  - `volvo_csi_dips_line.R`: Line plot of CSI dips.
  - `volvo_csi_post_fix_bar.R`: Pre/Post CSI bar plot.
  - `volvo_csi_terrain_map.R`: Terrain map of Post-Fix CSI.
  - `volvo_csi_3d_surface.R`: 3D surface + bubble plot.
  - `volvo_csi_parallel_anim.R`: Animated parallel coordinates plot.
  - `volvo_csi_sankey_3d.R`: 3D Sankey + force-directed network.

- **Outputs**:
  - PNGs: `volvo_complaints_bar.png`, `volvo_model_complaints_stack.png`, etc.
  - GIF: `volvo_csi_parallel_anim_models_2025.gif`.
  - HTML: `volvo_csi_terrain_map_2025.html`, `volvo_csi_3d_surface_2025.html`, etc.

## Setup

1. **Prerequisites**:
   - R and RStudio installed.
   - Required packages:
     ```R
     install.packages(c("ggplot2", "gganimate", "plotly", "leaflet", "dplyr", "tidyr", "networkD3", "htmlwidgets"))
     ```

2. **Clone the Repo**:
   ```bash
   git clone https://github.com/[YourUsername]/VolvoServiceCSIAnalysis.git
   cd VolvoServiceCSIAnalysis
   ```

3. **Run Scripts**:
   - Place `.csv` files in `C:/Users/Veteran` (or adjust `setwd()`).
   - Source each `.R` script in RStudio (`Ctrl+Shift+S`) to generate outputs.

## Data Files

### `volvo_service_complaints_2021_2025.csv`
```csv
Year,Complaint_Type,Frequency,Corrective_Action,Action_Details
2021,Electrical,150,Software Update,Update BECM software for XC40 BEV
2021,Airbags,200,Recall,Replace driver’s airbag in S60/XC60
2022,Brakes,80,Part Replacement,Replace sticky rear brake pads
2024,Electrical,300,Recall,Fix Bendix EC-80 unit in VNL trucks
```

### `volvo_model_complaints_2021_2025.csv`
```csv
Model,Complaint_Type,Complaint_Count,Top_Year
XC60,Electrical,250,2022
XC40 BEV,Electrical,180,2021
S60,Airbags,200,2021
VNL Trucks,Electrical,350,2024
XC90,Brakes,120,2023
```

### `volvo_csi_dips_2021_2025.csv`
```csv
Region,Year,CSI_Score,Change_from_Prev
Northeast,2021,75,NA
Northeast,2022,70,-5
Midwest,2021,72,NA
Midwest,2024,65,-7
South,2021,80,NA
South,2022,74,-6
```

### `volvo_csi_post_fix_2025.csv`
```csv
Region,Pre_Fix_CSI,Post_Fix_CSI,Change
Northeast,70,73,3
Midwest,65,70,5
South,74,78,4
Southeast,82,85,3
West,68,71,3
```

## Visualizations

### 1. Complaints by Year/Type
**Script**: `volvo_complaints_bar.R`
```R
library(ggplot2)
setwd("C:/Users/Veteran")
data <- read.csv("volvo_service_complaints_2021_2025.csv")
p <- ggplot(data, aes(x = Year, y = Frequency, fill = Complaint_Type)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Volvo Service Complaints (2021-2025)", x = "Year", y = "Number of Complaints") +
  scale_fill_brewer(palette = "Set2", name = "Complaint Type") +
  theme_minimal() + theme(axis.text.x = element_text(angle = 45, hjust = 1))
print(p)
ggsave("volvo_complaints_bar.png", width = 10, height = 6, dpi = 300)
```

### 2. Model Complaints
**Script**: `volvo_model_complaints_stack.R`
```R
library(ggplot2)
setwd("C:/Users/Veteran")
data <- read.csv("volvo_model_complaints_2021_2025.csv")
p <- ggplot(data, aes(x = Model, y = Complaint_Count, fill = Complaint_Type)) +
  geom_bar(stat = "identity") +
  labs(title = "Volvo Models with Most Complaints (2021-2025)", x = "Model", y = "Complaint Count") +
  scale_fill_brewer(palette = "Set3", name = "Complaint Type") +
  theme_minimal() + theme(axis.text.x = element_text(angle = 45, hjust = 1))
print(p)
ggsave("volvo_model_complaints_stack.png", width = 10, height = 6, dpi = 300)
```

### 3. CSI Dips by Region
**Script**: `volvo_csi_dips_line.R`
```R
library(ggplot2)
setwd("C:/Users/Veteran")
data <- read.csv("volvo_csi_dips_2021_2025.csv")
p <- ggplot(data, aes(x = Year, y = CSI_Score, color = Region)) +
  geom_line(size = 1) + geom_point(size = 2) +
  geom_text(aes(label = ifelse(Change_from_Prev < -5, Change_from_Prev, "")), vjust = -1, size = 3.5) +
  labs(title = "Volvo CSI Dips by U.S. Region (2021-2025)", x = "Year", y = "CSI Score") +
  scale_x_continuous(breaks = unique(data$Year)) + scale_y_continuous(limits = c(60, 85)) +
  theme_minimal() + theme(legend.position = "bottom")
plot.new()
print(p)
ggsave("volvo_csi_dips_line.png", width = 10, height = 6, dpi = 300)
```

### 4. Pre/Post CSI Comparison
**Script**: `volvo_csi_post_fix_bar.R`
```R
library(ggplot2)
library(tidyr)
setwd("C:/Users/Veteran")
data <- read.csv("volvo_csi_post_fix_2025.csv") %>%
  pivot_longer(cols = c("Pre_Fix_CSI", "Post_Fix_CSI"), names_to = "Period", values_to = "CSI")
p <- ggplot(data, aes(x = Region, y = CSI, fill = Period)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Volvo CSI Pre- and Post-Fix (2025)", x = "Region", y = "CSI Score") +
  scale_fill_manual(values = c("Pre_Fix_CSI" = "grey50", "Post_Fix_CSI" = "skyblue"), name = "Period", labels = c("Pre-Fix", "Post-Fix")) +
  theme_minimal() + theme(axis.text.x = element_text(angle = 45, hjust = 1))
print(p)
ggsave("volvo_csi_post_fix_bar.png", width = 10, height = 6, dpi = 300)
```

### 5. Terrain Map of Post-Fix CSI
**Script**: `volvo_csi_terrain_map.R`
```R
library(leaflet)
library(dplyr)
setwd("C:/Users/Veteran")
data <- read.csv("volvo_csi_post_fix_2025.csv")
city_data <- data.frame(City = c("New York", "Chicago", "Miami", "Atlanta", "Los Angeles"), Region = c("Northeast", "Midwest", "South", "Southeast", "West"), Population = c(8.1, 2.6, 0.4, 0.5, 3.8), Lat = c(40.7128, 41.8781, 25.7617, 33.7490, 34.0522), Lon = c(-74.0060, -87.6298, -80.1918, -84.3880, -118.2437)) %>%
  left_join(data, by = "Region") %>%
  mutate(popup_text = paste("<b>", City, "</b><br>", "Region:", Region, "<br>", "Post-Fix CSI:", Post_Fix_CSI, "<br>", "Change from Pre-Fix:", Change, "<br>", "Population (M):", Population))
map <- leaflet(data = city_data) %>%
  addProviderTiles(providers$Esri.WorldTopoMap, options = providerTileOptions(noWrap = TRUE)) %>%
  setView(lng = -98.5795, lat = 39.8283, zoom = 4) %>%
  addCircles(lng = ~Lon, lat = ~Lat, radius = ~Population * 50000, color = ~case_when(Post_Fix_CSI >= 80 ~ "green", Post_Fix_CSI >= 70 ~ "yellow", TRUE ~ "red"), fillOpacity = 0.7, popup = ~popup_text, label = ~City) %>%
  addLegend(position = "bottomright", colors = c("green", "yellow", "red"), labels = c("High (80+)", "Medium (70-79)", "Low (<70)"), title = "Post-Fix CSI Score")
print(map)
htmlwidgets::saveWidget(map, "volvo_csi_terrain_map_2025.html", selfcontained = TRUE)
```

### 6. 3D Surface + Bubble Plot
**Script**: `volvo_csi_3d_surface.R`
```R
library(plotly)
library(dplyr)
setwd("C:/Users/Veteran")
data <- read.csv("volvo_csi_post_fix_2025.csv")
city_data <- data.frame(City = c("New York", "Chicago", "Miami", "Atlanta", "Los Angeles"), Region = c("Northeast", "Midwest", "South", "Southeast", "West"), Population = c(8.1, 2.6, 0.4, 0.5, 3.8), X = c(1, 2, 3, 4, 5), Y = rep(1, 5)) %>%
  left_join(data, by = "Region") %>%
  mutate(hover_text = paste("City:", City, "<br>", "Region:", Region, "<br>", "Post-Fix CSI:", Post_Fix_CSI, "<br>", "Change:", Change, "<br>", "Population (M):", Population))
x_grid <- seq(1, 5, length.out = 20)
y_grid <- seq(0, 2, length.out = 20)
z_grid <- matrix(NA, nrow = length(x_grid), ncol = length(y_grid))
for (i in 1:length(x_grid)) for (j in 1:length(y_grid)) z_grid[i, j] <- approx(city_data$X, city_data$Post_Fix_CSI, xout = x_grid[i])$y
fig <- plot_ly() %>%
  add_surface(x = x_grid, y = y_grid, z = z_grid, colorscale = "Viridis", name = "CSI Surface", showscale = FALSE) %>%
  add_trace(data = city_data, x = ~X, y = ~Y, z = ~Post_Fix_CSI, type = "scatter3d", mode = "markers", marker = list(size = ~Population * 5, color = ~Change, colorscale = "RdBu", colorbar = list(title = "CSI Change"), line = list(width = 1, color = "black")), text = ~hover_text, hoverinfo = "text", name = "Cities") %>%
  layout(title = "Volvo Post-Fix CSI 2025: 3D Surface & City Impact", scene = list(xaxis = list(title = "Region (1=Northeast, 5=West)", tickvals = 1:5, ticktext = c("NE", "MW", "S", "SE", "W")), yaxis = list(title = "Time Since Fix (Synthetic)"), zaxis = list(title = "Post-Fix CSI", range = c(60, 90))))
print(fig)
htmlwidgets::saveWidget(fig, "volvo_csi_3d_surface_2025.html", selfcontained = TRUE)
```

### 7. Animated Parallel Coordinates
**Script**: `volvo_csi_parallel_anim.R`
```R
library(ggplot2)
library(gganimate)
library(plotly)
library(dplyr)
library(tidyr)
setwd("C:/Users/Veteran")
csi_data <- read.csv("volvo_csi_post_fix_2025.csv")
model_data <- read.csv("volvo_model_complaints_2021_2025.csv")
region_model_map <- data.frame(Region = c("Northeast", "Midwest", "South", "Southeast", "West"), Model = c("XC60", "VNL Trucks", "S60", "XC40 BEV", "XC90")) %>%
  left_join(model_data, by = "Model")
data <- csi_data %>% left_join(region_model_map, by = "Region")
data_long <- data %>% pivot_longer(cols = c("Pre_Fix_CSI", "Post_Fix_CSI"), names_to = "State", values_to = "CSI") %>%
  mutate(State = factor(State, levels = c("Pre_Fix_CSI", "Post_Fix_CSI"), labels = c("Pre-Fix", "Post-Fix")), Frame = as.numeric(State), hover_text = paste("Region:", Region, "<br>", "Model:", Model, "<br>", "Complaint:", Complaint_Type, "<br>", "Complaint Count:", Complaint_Count, "<br>", "CSI:", CSI, "<br>", "Change:", Change))
p <- ggplot(data_long, aes(x = State, y = CSI, group = Region, color = Complaint_Type)) +
  geom_line(size = 1.2, alpha = 0.8) + geom_point(size = 3) +
  labs(title = "Volvo CSI Transition: Pre- to Post-Fix (2025) by Model Impact", x = "State", y = "CSI Score", color = "Top Complaint Type") +
  scale_color_brewer(palette = "Set1") +
  theme_minimal() + theme(panel.grid.major.x = element_blank(), panel.grid.minor = element_blank(), legend.position = "bottom", plot.title = element_text(hjust = 0.5, size = 14, face = "bold")) +
  transition_states(Frame, transition_length = 2, state_length = 1) + enter_fade() + exit_fade()
anim <- animate(p, nframes = 50, fps = 10, width = 800, height = 600)
anim_save("volvo_csi_parallel_anim_models_2025.gif", animation = anim, path = "C:/Users/Veteran")
p_static <- ggplot(data_long, aes(x = State, y = CSI, group = Region, color = Complaint_Type, text = hover_text)) +
  geom_line(size = 1.2, alpha = 0.8) + geom_point(size = 3) +
  labs(title = "Volvo CSI Transition: Pre- to Post-Fix (2025) by Model Impact", x = "State", y = "CSI Score", color = "Top Complaint Type") +
  scale_color_brewer(palette = "Set1") +
  theme_minimal() + theme(panel.grid.major.x = element_blank(), panel.grid.minor = element_blank(), legend.position = "bottom", plot.title = element_text(hjust = 0.5, size = 14, face = "bold"))
interactive_plot <- ggplotly(p_static, tooltip = "text")
print(interactive_plot)
htmlwidgets::saveWidget(interactive_plot, "volvo_csi_parallel_static_models_2025.html", selfcontained = TRUE)
```

### 8. 3D Sankey + Force-Directed Network
**Script**: `volvo_csi_sankey_3d.R`
```R
library(networkD3)
library(dplyr)
library(plotly)
setwd("C:/Users/Veteran")
csi_data <- read.csv("volvo_csi_post_fix_2025.csv")
model_data <- read.csv("volvo_model_complaints_2021_2025.csv")
region_model_map <- data.frame(Region = c("Northeast", "Midwest", "South", "Southeast", "West"), Model = c("XC60", "VNL Trucks", "S60", "XC40 BEV", "XC90")) %>%
  left_join(model_data, by = "Model")
data <- csi_data %>% left_join(region_model_map, by = "Region")
nodes <- data.frame(name = c(paste(data$Region, "Pre-Fix", sep = "_"), paste(data$Region, "Post-Fix", sep = "_"), data$Model, data$Complaint_Type), stringsAsFactors = FALSE) %>% distinct()
nodes$node_id <- 0:(nrow(nodes) - 1)
links <- rbind(
  data.frame(source = match(paste(data$Region, "Pre-Fix", sep = "_"), nodes$name) - 1, target = match(data$Model, nodes$name) - 1, value = data$Pre_Fix_CSI / 10),
  data.frame(source = match(data$Model, nodes$name) - 1, target = match(data$Complaint_Type, nodes$name) - 1, value = data$Complaint_Count / 50),
  data.frame(source = match(data$Model, nodes$name) - 1, target = match(paste(data$Region, "Post-Fix", sep = "_"), nodes$name) - 1, value = data$Post_Fix_CSI / 10)
)
sankey <- sankeyNetwork(Links = links, Nodes = nodes, Source = "source", Target = "target", Value = "value", NodeID = "name", fontSize = 12, nodeWidth = 30, iterations = 100)
htmlwidgets::saveWidget(sankey, "volvo_csi_sankey_2025.html", selfcontained = TRUE)
nodes_3d <- nodes %>% mutate(group = case_when(grepl("Pre-Fix", name) ~ "Pre-Fix", grepl("Post-Fix", name) ~ "Post-Fix", name %in% data$Model ~ "Model", TRUE ~ "Complaint"))
fig <- plot_ly() %>%
  add_trace(data = links, x = ~nodes$name[source + 1], y = ~nodes$name[target + 1], z = ~value, type = "scatter3d", mode = "lines", line = list(width = 2, color = "#888"), opacity = 0.5, name = "Flows") %>%
  add_trace(data = nodes_3d, x = ~node_id, y = ~rep(0, nrow(nodes_3d)), z = ~rep(0, nrow(nodes_3d)), type = "scatter3d", mode = "markers+text", marker = list(size = 10, color = ~group, colorscale = "Viridis"), text = ~name, hoverinfo = "text", name = "Nodes") %>%
  layout(title = "3D Force-Directed Network: Volvo CSI Flow (2025)", scene = list(xaxis = list(title = "Node Index"), yaxis = list(title = "Y (Arbitrary)"), zaxis = list(title = "Value (Scaled)")))
print(fig)
htmlwidgets::saveWidget(fig, "volvo_csi_3d_force_2025.html", selfcontained = TRUE)
```

## Why It Matters
Volvo’s brand is synonymous with safety and reliability, but these visualizations reveal cracks in that armor—electrical failures in VNL Trucks, airbag recalls in S60s, brake issues in XC90s—that dent customer trust. Linking these complaints to CSI dips and subsequent recoveries (e.g., Midwest’s +5 after fixing VNL Trucks) shows how Volvo’s responses (recalls, software updates) directly influence satisfaction. This matters because it’s not just about fixing cars—it’s about preserving Volvo’s reputation in a competitive market where trust drives loyalty and sales. For owners, it’s insight into their vehicle’s weak spots; for Volvo, it’s a roadmap to prioritize fixes and regain ground.

## Conclusion
This project blends data science and visualization artistry to unpack Volvo’s service challenges and CSI trends. From simple bar plots to a 3D Sankey network, it offers a multi-angle view of how model-specific issues shape customer experiences. Fork it, tweak it, or dive deeper with your own Volvo data—there’s plenty to explore!
```

---

### Notes
- **All Code Included**: Every script is fully listed with `print()` commands for RStudio display, matching your final versions.
- **Data Files**: Exact `.csv` contents from our work—ready to copy-paste into files.
- **Why It Matters**: Ties the tech (complaints, CSI) to real-world stakes (trust, sales), making it compelling.
- **Repo Name**: `VolvoServiceCSIAnalysis` fits the scope and sounds sharp.

Upload this to GitHub with your `.csv` files, scripts, and outputs, and you’ve got a killer repo! Let me know if you want to polish it further—maybe a badge or screenshot? 🚗✨


---

### Notes
- **AI & RStudio Focus**: Emphasizes Grok’s role and RStudio’s execution—your LinkedIn network will see the tech stack shine.
- **.csv Files**: Included verbatim—shows the raw data fueling the project.
- **Viz Explanations**: Each plot’s *what* and *why* are clear, distinguishing bar plots from 3D networks.
- **GitHub Placeholder**: Add your repo URL (e.g., `https://github.com/[YourUsername]/VolvoServiceCSIAnalysis`).
- **Emojis**: Sprinkled for energy—keeps it fun yet pro.
- **Call to Action**: Invites engagement—perfect for LinkedIn buzz.

