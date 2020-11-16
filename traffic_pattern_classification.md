# Pattern Classification Model of Traffic Data including Missing Values
<button onclick="location.href='https://github.com/leee5495/DWLab_2020'" type="button">&#128193; GitHub Repository</button>
<button onclick="location.href='https://leee5495.github.io/pdf/KDBC_lej.pdf'" type="button">&#128196; Paper - Presented at KDBC 2019</button>
<br><br>


### Research Motivation
---
Problems with Time Series Sensor Data
1. **Missing values** due to troubles in transmission and sensor defects<br>
   → Needs proper processing before analysis
   
2. **Requires time** until meaningful amount of data is collected<br>
   → Needs to accurately analyze small amount of data
   
3. **Physical restrictions** that put locational constraint of the sensors<br>
   → Needs to analyze data that is affected by other variables

<br><br>


### Suggested Model
---
Pattern Classification Model based on Attention Mechanism and CNN for Analysis of Traffic Data including Missing Values
<br>

<img src="images/traffic_pattern.png?raw=true"/>
<br>

1. **Attention Mechanism**<br>
   Added an attention mechanism to focus on important entries in data<br>
   Produces weights for each entries in data with an additional meta data input of whether each entry is missing
   
2. **Deep model structure of CNN and Feed Forward layers**<br>
   Used deep model structure of CNN and Feed Forward layers to accurately classify minor pattern differences

<br><br>

### Experimentation Data
---
- Used traffic data measured every 5 minutes on 101 North Freway, which is close to Dodger Stadium (consisting of 288 elements per day)
- Designed a model that classifies whether the stadium held a game that day from the day's traffic data

![image](https://user-images.githubusercontent.com/39192405/93020209-c8fda180-f616-11ea-9221-4b1e169d5da5.png)

<br><br>

### Classification Result of Traffic Loop Time Series Data
---
![traffic_classification_res1](https://user-images.githubusercontent.com/39192405/93121397-bd39da00-f6ff-11ea-9e7f-7a0e1278ce1a.png)

- The result shows that the **proposed attention mechanism improves the classification accuracy**

<br><br>

### Classification Result on Additional Traffic Data
---
![traffic_classification_res2](https://user-images.githubusercontent.com/39192405/93121396-bc08ad00-f6ff-11ea-9295-6ff1ea005008.png)

- The result shows that the attention mechanism **improves classification accuracy of data with missing values**
- The result also shows that the **attention mechanism can improve classification accuacy of data without missing values**

<br><br>

### Conclusion
---

&nbsp;`Conclustion`
- **Proposed an attention mechanism** that can improve classification accuracy of time series sensor data with missing values without any complicated preprocessing procedure
- Through various experiments proved that the **attention mechanism works to improve classification accurary with time series data with missing values**

&nbsp;`Future Work`
- **Experiment with other types of time series data** to see whether the mechanism can be applied to different sensor data
- Experiement with multiple layers of Feed Forward layer for attention mechanism
  
<br><br>
