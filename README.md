# Trend-analysis-for-MEDEVAC-data
# Trend-analysis-for-MEDEVAC-data
# Load necessary libraries1
library(ggplot2)       # For creating plots
library(gganimate)     # For animation
library(readxl)        # For reading Excel files
library(gifski)        # For rendering GIFs
library(transformr)    # For smooth transitions

# Load the dataset from the specified Excel file
trend_analysis <- read_excel("E:/Work in Libya ,Nepal, Dang/AMET/Analysis/Trend analysis/trend_analysis.xlsx")
# Check column names to ensure they match expectations
print(names(trend_analysis))

# Ensure 'Year' is numeric and properly formatted
trend_analysis$Year <- as.numeric(gsub("[^0-9]", "", trend_analysis$Year))
trend_analysis$Year <- round(trend_analysis$Year)

# Convert 'Aircraft' to a factor for proper categorical coloring
trend_analysis$Aircraft <- as.factor(trend_analysis$Aircraft)

# Create the animated plot
p <- ggplot(trend_analysis, aes(x = Year, y = medevacNo, color = Aircraft, group = Aircraft)) +
  geom_line(linewidth = 1.2) +    # Draw lines connecting the data points
  geom_point(size = 2) +          # Add points at each data value
  labs(
    title = "Yearly Trends of MEDEVAC in Nepali Army ",  # Dynamic title updates per frame
    x = "Year",
    y = "Number of MEDEVAC"
  ) +
  scale_x_continuous(
    breaks = seq(floor(min(trend_analysis$Year, na.rm = TRUE)), 
                 ceiling(max(trend_analysis$Year, na.rm = TRUE)), 
                 by = 1)
  ) +
  theme_minimal() +  # Clean visual theme
  
  # gganimate: Animate over the 'Year' variable
  transition_reveal(Year) +   # Shows the trend over time gradually
  ease_aes('linear')          # Smooth transition effect

# Save the animation as a GIF file
animate(p, duration = 10, fps = 10, width = 800, height = 600, renderer = gifski_renderer("medevac_trends.gif"))

# Show the animation in RStudio
p
