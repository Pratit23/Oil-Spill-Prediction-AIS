# Oil Spill Mitigation with AIS Data
| A MODEL FOR MITIGATING OCEANIC OIL SPILLS USING AIS DATA FROM SHIPS

---

The data used within this research originates from two different sources. Ship movement data is broadcasted by marine vessels worldwide via the Automatic Identification System (AIS) and made available by AIS service providers on the Internet. Weather data required for the model is made available by the Environmental Research Division’s Data Access Program (ERDDAP). Both datasets are then combined and used to create the prediction model.

The AIS data was collected from MarineCadastre which provides open source AIS data of each day. MarineCadastre is a government website owned by Bureau of Ocean Energy Management (BOEM) and the National Oceanic and Atmospheric Administration (NOAA).

As our data sources for AIS data do not provide any marine weather information, but wind information only, it is required to get marine weather information.

The marine weather information is provided by ERDDAP which allows downloading marine weather information based on time, latitude, longitude and selected variables as gridded data, which means that it contains the selected variables for a chosen area in a 0.5 degr

The weather information is based on the third-generation wind-wave model WAVEWATCH III developed by the Marine Modeling and Analysis Branch (MMAB) of the Environmental Modeling Center (EMC) of the National Centers for Environmental Protection (NCEP).ee grid.


### Proposed System

---

- Prediction model uses AIS data to predict the vessel speed.
- The predicted vessel speed is compared with live vessel speed.
- If the speed is over a specific difference threshold then the ship is flagged and further checking can be done by the authorities.
- Further steps will be checking for SAR images and check for Oil Spills.


### Steps
---

1. Scanning live ship AIS Data
    - Ships / Vessels transmit live AIS data to the nearest ports at a particular frequency.
2. Predicting speed with the live AIS Data
    - With the AIS data recieved, we can process it through our model and get a predicted speed.
3. Comparing live and predicted speed
    - The live speed and the predicted speed is compared to check if it matches the criteria.
4. Further actions of flagged ships
    - If the difference between the predicted and live falls below a specific difference threshold then it is flagged


## Implementation

---

- **Data Acquisition**
    - For our purpose, we needed 2 major types of data:
        - `AIS Data`
            
            ![image](https://user-images.githubusercontent.com/43881544/206900093-bfe89523-e437-404f-a589-d79e9844ee98.png)
            
            - This AIS Data is from MarineCadastre
            - Owned by BOEM & NOAA
            - Does not provide weather information
        - `Weather Data`
            
            ![image](https://user-images.githubusercontent.com/43881544/206900107-a31a73c4-0cbf-4689-9ad9-ceb2b15a9f84.png)
            
            - Provided by ERDDAP
            - Time Series Data with Lat and Lng information with 0.5-degree grid
            - Based on wind-wave model WAVEWATCH III
            - Developed by MMAB
- **Data Preprocessing**
    
    Data preparation is necessary to join ship and weather data. The latitude and longitude of the ships and the weather data is indicated in degrees, but the weather data is provided in a 0.5 degree grid. In order to being able to match both datasets, the ship positions must be adjusted and rounded to next integer or half of an integer.
    
    Furthermore, the longitude of the weather data is not indicated in the range ± 180, but 0 to 359 degrees. Hence, the longitude of the ship position must be added to 180. The adjusted position data is then used to join both datasets.
    
    1. Treating Null Values
        - Since we are dealing with huge chunks of time-series based data, we truncated the ones that didn't match our criteria useful for stitching dataset
    2. Working with coordinates
        - The latitude and longitude of the ships and the weather data is indicated in degrees, but the weather data is provided in a 0.5 degree grid.
    3. Stitching Dataset
        - Combining the Vessel Data and the weather data based on Time and Location attributes
- **Correlation Analysis & Feature Selection**
    
    
    - Each weather feature and AIS data feature correlation with SOG is plotted. Owned by BOEM & NOAA
    - The features with the higher correlation values are chosen.
        
        <img src="https://user-images.githubusercontent.com/43881544/206900124-92f8387e-9757-4064-8500-ec09764f4fa9.png" height="300px" width="300px" />
        
        Correlation Analysis via a Heat Map
        
    <img src="https://user-images.githubusercontent.com/43881544/206900157-fdc7bb67-9bac-4a48-8dec-1f85da5a5116.png" width="300px" />
    
    Correlation Value sorted in descending manner
    
- **Model Comparison**
    
    ![image](https://user-images.githubusercontent.com/43881544/206900178-a3d12ab9-5f0b-4dba-b3b2-d08ac49141da.png)
    
    - Here the accuracy difference between MLR and Polynomial Regression is shown. As the data is mostly non-linear Polynomial has a marginal difference.
- **Problems Faced**
    - `Data Acquisition`
        - AIS Data from MarineCadastre
        - Owned by BOEM & NOAA
        - Does not provide weather information
        - Short time frame for data collection, less number of useful data for analysis
        - Marine Traffic at Port / Harbour
    - `Stitching Data`
        - Matching the common ground between Vessel and Weather Data

### Conclusion

---

After gathering our data from various sources useful for regression. It turns out that the Weather Data has stronger influence on the output for the regression. The findings of this work along with other factors could be applied in a prediction model for estimating delays etc. This can be done by comparing the predicted and the actual real live data and if the difference if above certain threshold, then we can flag that ship. For any flagged ship we can use it's SAR Images and apply an Image Classification Model (using Segmentation etc) to understand if there actually is an Oil Spill.

### References

---

1. Factors Affecting Ocean-Going Cargo Ship Speed and Arrival Time; Erwin Filtz
2. Oil Spill Detection and Characterization from Satellite Image Using Artificial Neural Network Algorithm
3. Modeling Historical AIS Data For Vessel Path
4. Prediction: A Comprehensive Treatment


