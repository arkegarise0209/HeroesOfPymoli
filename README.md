
# Heroes Of Pymoli Analysis

-  Although the total number of players is 573, the total number of purchases is 780. This shows that many players are repeat customers.
-  Of those 573 players, 81.15% of them are Male users. This shows that Heroes of Pymoli predominantly caters to the interests of Males.
-  Also, 45.20% of the game's users fall between the ages of 20-24. A good target market would probably be Males between the ages of 20-24.
-  Although Retribution Axe	was the most expensive item at $4.14, it still was one of the top 5 most popular items purchased. This shows that an item that enhances a player's experience will be purchased regardless of it's price comparison to other items.


```python
#Dependencies
import pandas as pd
```


```python
#File path
file = "Resources/purchase_data.json"
#File read
purchase_data= pd.read_json(file)
#View file data
purchase_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>



### Player Count


```python
#Calculate amount of players
player_count = len(purchase_data["SN"].unique())
#Create DataFrame 
player_count_df = pd.DataFrame([{"Total Players": player_count}])
#Reset index to "Total Players" instead of numbers OPTIONAL
player_count_df.set_index("Total Players", inplace=True)
player_count_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
    </tr>
    <tr>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>573</th>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Total


```python
#Calculate number of unique items
items = len(purchase_data["Item ID"].unique())
#Calculate total number of purchases
total_purchases = purchase_data["Price"].count()
#Calculate total revenue
total_revenue = purchase_data["Price"].sum()
#Calculate average purchase price per item
average_item_price = total_revenue/total_purchases

#Create Purchase Analysis DataFrame
purchase_analysis_df = pd.DataFrame({"Number of Unique Items": [items],
                                    "Average Price": [average_item_price],
                                    "Number of Purchases": [total_purchases],
                                    "Total Revenue": [total_revenue]}, columns=["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"])

#Format Purchase Analysis DataFrame
purchase_analysis_df.style.format({"Average Price": "${:.2f}","Total Revenue": "${:,.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_410ee528_273f_11e8_938b_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of Unique Items</th> 
        <th class="col_heading level0 col1" >Average Price</th> 
        <th class="col_heading level0 col2" >Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_410ee528_273f_11e8_938b_80ce62114cc3level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_410ee528_273f_11e8_938b_80ce62114cc3row0_col0" class="data row0 col0" >183</td> 
        <td id="T_410ee528_273f_11e8_938b_80ce62114cc3row0_col1" class="data row0 col1" >$2.93</td> 
        <td id="T_410ee528_273f_11e8_938b_80ce62114cc3row0_col2" class="data row0 col2" >780</td> 
        <td id="T_410ee528_273f_11e8_938b_80ce62114cc3row0_col3" class="data row0 col3" >$2,286.33</td> 
    </tr></tbody> 
</table> 



### Gender Demographics


```python
#Clean player count
no_dubs = purchase_data.drop_duplicates(["SN"], keep="last") 
#Calculate number of players for each possible gender value
gender_counts= no_dubs["Gender"].value_counts().reset_index()
#Calculate percent of players from each gender value
gender_counts["Percentage of Players"] = gender_counts["Gender"]/player_count * 100
#Rename columns
gender_counts.rename(columns={"index":"","Gender":"Total Count"}, inplace=True)
#Set index as gender
gender_counts.set_index([""], inplace=True)
#Format Gender Demographics DataFrame
gender_counts.style.format({"Percentage of Players":"{:.2f}%"})
```




<style  type="text/css" >
</style>  
<table id="T_8d14266c_273e_11e8_9a17_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Count</th> 
        <th class="col_heading level0 col1" >Percentage of Players</th> 
    </tr>    <tr> 
        <th class="index_name level0" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8d14266c_273e_11e8_9a17_80ce62114cc3level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_8d14266c_273e_11e8_9a17_80ce62114cc3row0_col0" class="data row0 col0" >465</td> 
        <td id="T_8d14266c_273e_11e8_9a17_80ce62114cc3row0_col1" class="data row0 col1" >81.15%</td> 
    </tr>    <tr> 
        <th id="T_8d14266c_273e_11e8_9a17_80ce62114cc3level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_8d14266c_273e_11e8_9a17_80ce62114cc3row1_col0" class="data row1 col0" >100</td> 
        <td id="T_8d14266c_273e_11e8_9a17_80ce62114cc3row1_col1" class="data row1 col1" >17.45%</td> 
    </tr>    <tr> 
        <th id="T_8d14266c_273e_11e8_9a17_80ce62114cc3level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_8d14266c_273e_11e8_9a17_80ce62114cc3row2_col0" class="data row2 col0" >8</td> 
        <td id="T_8d14266c_273e_11e8_9a17_80ce62114cc3row2_col1" class="data row2 col1" >1.40%</td> 
    </tr></tbody> 
</table> 



### Purchasing Analysis (Gender)


```python
#Create DataFrame that counts number of purchases per gender
gender_purchase_count_df = pd.DataFrame(purchase_data.groupby("Gender")["Gender"].count())
#Create DataFrame that calculates "Total Purchase Value" per gender
gender_total_purchase_values_df = pd.DataFrame(purchase_data.groupby("Gender")["Price"].sum())
#Merge above DataFrames 
gender_purchase_analysis_df = pd.merge(gender_purchase_count_df,gender_total_purchase_values_df,left_index=True,right_index=True)
#Rename columns
gender_purchase_analysis_df.rename(columns={"Gender":"Purchase Count","Price":"Total Purchase Value"}, inplace=True)
#Calculate "Average Purchase Price"
gender_purchase_analysis_df["Average Purchase Price"] = gender_purchase_analysis_df["Total Purchase Value"]/gender_purchase_analysis_df["Purchase Count"]
#Merge above DataFrame with Gender Demographics DataFrame 
gender_purchase_analysis_df = gender_purchase_analysis_df.merge(gender_counts, left_index=True,right_index=True)
#Calculate "Normalized Totals"
gender_purchase_analysis_df["Normalized Totals"] = gender_purchase_analysis_df["Total Purchase Value"]/gender_purchase_analysis_df["Total Count"]
#Delete unwanted columns
del gender_purchase_analysis_df["Total Count"]
del gender_purchase_analysis_df["Percentage of Players"]
#Format Purchasing Analysis DataFrame (Gender) 
gender_purchase_analysis_df.style.format({"Total Purchase Value":"${:,.2f}","Average Purchase Price":"${:.2f}","Normalized Totals":"${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_8d2e4924_273e_11e8_a239_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Total Purchase Value</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8d2e4924_273e_11e8_a239_80ce62114cc3level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row0_col0" class="data row0 col0" >136</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row0_col1" class="data row0 col1" >$382.91</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row0_col2" class="data row0 col2" >$2.82</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row0_col3" class="data row0 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_8d2e4924_273e_11e8_a239_80ce62114cc3level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row1_col0" class="data row1 col0" >633</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row1_col1" class="data row1 col1" >$1,867.68</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row1_col2" class="data row1 col2" >$2.95</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row1_col3" class="data row1 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_8d2e4924_273e_11e8_a239_80ce62114cc3level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row2_col0" class="data row2 col0" >11</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row2_col1" class="data row2 col1" >$35.74</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row2_col2" class="data row2 col2" >$3.25</td> 
        <td id="T_8d2e4924_273e_11e8_a239_80ce62114cc3row2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 



### Age Demographics


```python
#Create a column "age_range" based on conditional of age range
purchase_data.loc[(purchase_data["Age"] < 10), "age_range"] = "<10"
purchase_data.loc[(purchase_data["Age"] >= 10) & (purchase_data["Age"] <= 14), "age_range"] = "10 - 14"
purchase_data.loc[(purchase_data["Age"] >= 15) & (purchase_data["Age"] <= 19), "age_range"] = "15 - 19"
purchase_data.loc[(purchase_data["Age"] >= 20) & (purchase_data["Age"] <= 24), "age_range"] = "20 - 24"
purchase_data.loc[(purchase_data["Age"] >= 25) & (purchase_data["Age"] <= 29), "age_range"] = "25 - 29"
purchase_data.loc[(purchase_data["Age"] >= 30) & (purchase_data["Age"] <= 34), "age_range"] = "30 - 34"
purchase_data.loc[(purchase_data["Age"] >= 35) & (purchase_data["Age"] <= 39), "age_range"] = "35 - 39"
purchase_data.loc[(purchase_data["Age"] >= 40), "age_range"] = "40+"

#Clean player count
no_dubs_age = purchase_data.drop_duplicates(["SN"], keep="last") 
#Calculate number of players for each possible age_range
age_counts= no_dubs_age["age_range"].value_counts()
#Reset index
age_counts=age_counts.reset_index()
#Sort index
age_counts= age_counts.sort_values("index")
#Calculate percent of players from each age_range
age_counts["Percentage of Players"] = age_counts["age_range"]/player_count * 100
#Rename Columns
age_counts.rename(columns={"index":" ","age_range":"Total Count"}, inplace=True)
#Set index
age_counts=age_counts.set_index(" ")
#Format Age Demographics DataFrame
age_counts.style.format({"Percentage of Players":"{:.2f}%"})
```




<style  type="text/css" >
</style>  
<table id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Count</th> 
        <th class="col_heading level0 col1" >Percentage of Players</th> 
    </tr>    <tr> 
        <th class="index_name level0" > </th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row0" class="row_heading level0 row0" >10 - 14</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row0_col0" class="data row0 col0" >23</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row0_col1" class="data row0 col1" >4.01%</td> 
    </tr>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row1" class="row_heading level0 row1" >15 - 19</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row1_col0" class="data row1 col0" >100</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row1_col1" class="data row1 col1" >17.45%</td> 
    </tr>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row2" class="row_heading level0 row2" >20 - 24</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row2_col0" class="data row2 col0" >259</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row2_col1" class="data row2 col1" >45.20%</td> 
    </tr>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row3" class="row_heading level0 row3" >25 - 29</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row3_col0" class="data row3 col0" >87</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row3_col1" class="data row3 col1" >15.18%</td> 
    </tr>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row4" class="row_heading level0 row4" >30 - 34</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row4_col0" class="data row4 col0" >47</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row4_col1" class="data row4 col1" >8.20%</td> 
    </tr>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row5" class="row_heading level0 row5" >35 - 39</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row5_col0" class="data row5 col0" >27</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row5_col1" class="data row5 col1" >4.71%</td> 
    </tr>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row6" class="row_heading level0 row6" >40+</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row6_col0" class="data row6 col0" >11</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row6_col1" class="data row6 col1" >1.92%</td> 
    </tr>    <tr> 
        <th id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3level0_row7" class="row_heading level0 row7" ><10</th> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row7_col0" class="data row7 col0" >19</td> 
        <td id="T_8d4088e8_273e_11e8_89c5_80ce62114cc3row7_col1" class="data row7 col1" >3.32%</td> 
    </tr></tbody> 
</table> 



### Purchasing Analysis (Age)


```python
#Create a column "age_range" based on conditional of age range
purchase_data.loc[(purchase_data["Age"] < 10), "age_range"] = "<10"
purchase_data.loc[(purchase_data["Age"] >= 10) & (purchase_data["Age"] <= 14), "age_range"] = "10 - 14"
purchase_data.loc[(purchase_data["Age"] >= 15) & (purchase_data["Age"] <= 19), "age_range"] = "15 - 19"
purchase_data.loc[(purchase_data["Age"] >= 20) & (purchase_data["Age"] <= 24), "age_range"] = "20 - 24"
purchase_data.loc[(purchase_data["Age"] >= 25) & (purchase_data["Age"] <= 29), "age_range"] = "25 - 29"
purchase_data.loc[(purchase_data["Age"] >= 30) & (purchase_data["Age"] <= 34), "age_range"] = "30 - 34"
purchase_data.loc[(purchase_data["Age"] >= 35) & (purchase_data["Age"] <= 39), "age_range"] = "35 - 39"
purchase_data.loc[(purchase_data["Age"] >= 40), "age_range"] = "40+" 


#Calculate "Purchase Count"
purch_count_age_df = pd.DataFrame(purchase_data.groupby("age_range")["SN"].count())
#Calculate "Average Purchase Price"
average_purch_price_age_df = pd.DataFrame(purchase_data.groupby("age_range")["Price"].mean()) 
#Merge above DataFrames
age_demographics_df = pd.merge(purch_count_age_df, average_purch_price_age_df, left_index=True, right_index=True)
#Rename columns
age_demographics_df.rename(columns={"SN":"Purchase Count","Price":"Average Purchase Price"}, inplace=True)
#Calculate "Total Purchase Value"
age_demographics_df["Total Purchase Value"] = age_demographics_df["Average Purchase Price"] * age_demographics_df["Purchase Count"]
#Merge current DataFrame with "Age Demographics DataFrame"
age_demographics_df = pd.merge(age_demographics_df, age_counts, left_index=True, right_index=True)
#Calculate "Normalized Totals"
age_demographics_df["Normalized Totals"] = age_demographics_df["Total Purchase Value"]/age_demographics_df["Total Count"]
#Delete unwanted columns
del age_demographics_df["Total Count"]
del age_demographics_df["Percentage of Players"]
#Format Purchasing Analysis DataFrame (Age)
age_demographics_df.style.format({"Total Purchase Value":"${:,.2f}","Average Purchase Price":"${:.2f}","Normalized Totals":"${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_8d523e26_273e_11e8_b541_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >age_range</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row0" class="row_heading level0 row0" >10 - 14</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row0_col0" class="data row0 col0" >35</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row0_col1" class="data row0 col1" >$2.77</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row0_col2" class="data row0 col2" >$96.95</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row0_col3" class="data row0 col3" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row1" class="row_heading level0 row1" >15 - 19</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row1_col0" class="data row1 col0" >133</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row1_col1" class="data row1 col1" >$2.91</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row1_col2" class="data row1 col2" >$386.42</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row1_col3" class="data row1 col3" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row2" class="row_heading level0 row2" >20 - 24</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row2_col0" class="data row2 col0" >336</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row2_col2" class="data row2 col2" >$978.77</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row2_col3" class="data row2 col3" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row3" class="row_heading level0 row3" >25 - 29</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row3_col0" class="data row3 col0" >125</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row3_col1" class="data row3 col1" >$2.96</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row3_col2" class="data row3 col2" >$370.33</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row3_col3" class="data row3 col3" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row4" class="row_heading level0 row4" >30 - 34</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row4_col0" class="data row4 col0" >64</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row4_col1" class="data row4 col1" >$3.08</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row4_col2" class="data row4 col2" >$197.25</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row4_col3" class="data row4 col3" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row5" class="row_heading level0 row5" >35 - 39</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row5_col0" class="data row5 col0" >42</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row5_col1" class="data row5 col1" >$2.84</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row5_col2" class="data row5 col2" >$119.40</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row5_col3" class="data row5 col3" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row6" class="row_heading level0 row6" >40+</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row6_col0" class="data row6 col0" >17</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row6_col1" class="data row6 col1" >$3.16</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row6_col2" class="data row6 col2" >$53.75</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row6_col3" class="data row6 col3" >$4.89</td> 
    </tr>    <tr> 
        <th id="T_8d523e26_273e_11e8_b541_80ce62114cc3level0_row7" class="row_heading level0 row7" ><10</th> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row7_col0" class="data row7 col0" >28</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row7_col1" class="data row7 col1" >$2.98</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row7_col2" class="data row7 col2" >$83.46</td> 
        <td id="T_8d523e26_273e_11e8_b541_80ce62114cc3row7_col3" class="data row7 col3" >$4.39</td> 
    </tr></tbody> 
</table> 



### Top Spenders


```python
#Group by screen name calculate "Purchase Count" per player
purch_count_top5_df = pd.DataFrame(purchase_data.groupby("SN")["Price"].count())
#Group by screen name calculate "Total Purchase Value" per player
purch_total_top5_df = pd.DataFrame(purchase_data.groupby("SN")["Price"].sum())
#Merge above DataFrames
top_spenders_df = pd.merge(purch_count_top5_df, purch_total_top5_df, left_index=True, right_index=True)
#Group by screen name, calculate "Average Purchase Price" per player, add to current DataFrame
top_spenders_df["Average Purchase Price"] = purchase_data.groupby("SN")["Price"].mean()
#Rename columns
top_spenders_df.rename(columns={"Price_x":"Purchase Count","Price_y":"Total Purchase Value"}, inplace=True)
#Sort "Top Sellers" DataFrame by Total Purchase Value (descending) & display top 5 spenders
top_spenders_df = top_spenders_df.sort_values(by=["Total Purchase Value"], ascending=False).head()
#Format Top Spenders DataFrame
top_spenders_df.style.format({"Total Purchase Value":"${:,.2f}","Average Purchase Price":"${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Total Purchase Value</th> 
        <th class="col_heading level0 col2" >Average Purchase Price</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3level0_row0" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row0_col0" class="data row0 col0" >5</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row0_col1" class="data row0 col1" >$17.06</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row0_col2" class="data row0 col2" >$3.41</td> 
    </tr>    <tr> 
        <th id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3level0_row1" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row1_col0" class="data row1 col0" >4</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row1_col1" class="data row1 col1" >$13.56</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row1_col2" class="data row1 col2" >$3.39</td> 
    </tr>    <tr> 
        <th id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3level0_row2" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row2_col0" class="data row2 col0" >4</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row2_col1" class="data row2 col1" >$12.74</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row2_col2" class="data row2 col2" >$3.18</td> 
    </tr>    <tr> 
        <th id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3level0_row3" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row3_col0" class="data row3 col0" >3</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row3_col1" class="data row3 col1" >$12.73</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row3_col2" class="data row3 col2" >$4.24</td> 
    </tr>    <tr> 
        <th id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3level0_row4" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row4_col0" class="data row4 col0" >3</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row4_col1" class="data row4 col1" >$11.58</td> 
        <td id="T_8d5ee048_273e_11e8_bf22_80ce62114cc3row4_col2" class="data row4 col2" >$3.86</td> 
    </tr></tbody> 
</table> 



### Most Popular Items


```python
#Group by Item ID & calculate "Purchase Count" per player
top5_items_df = pd.DataFrame(purchase_data.groupby("Item ID")["Item ID"].count())
#Sort Item ID by "Purchase Count"
top5_items_df.sort_values(by=["Item ID"], ascending=False, inplace=True)
#Keep first 6 rows since there is a tie for 5th
top5_items_df = top5_items_df.iloc[0:6][:]
#Group by Item ID & calculate "Total Purchase Value" per player
purch_total_top5_items_df = pd.DataFrame(purchase_data.groupby("Item ID")["Price"].sum())
#Merge above DataFrames
most_popular_items_df = pd.merge(top5_items_df,purch_total_top5_items_df, left_index=True, right_index=True)
#Clean Item ID values
no_dubs_item = purchase_data.drop_duplicates(["Item ID"], keep="last")
#Merge clean Item ID with current DataFrame
most_popular_items_df = pd.merge(most_popular_items_df, no_dubs_item, left_index=True, right_on=["Item ID"])
#Keep wanted columns
most_popular_items_df = most_popular_items_df[["Item ID","Item Name","Item ID_x","Price_y","Price_x"]]
#Set Item ID & Item Name as indices
most_popular_items_df.set_index(["Item ID", "Item Name"], inplace=True)
#Rename columns
most_popular_items_df.rename(columns={"Item ID_x":"Purchase Count","Price_y":"Item Price","Price_x":"Total Purchase Value"}, inplace=True)
#Format Most Popular Items DataFrame
most_popular_items_df.style.format({"Total Purchase Value":"${:,.2f}","Item Price":"${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level0_row0" class="row_heading level0 row0" >39</th> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level1_row0" class="row_heading level1 row0" >Betrayal, Whisper of Grieving Widows</th> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row0_col0" class="data row0 col0" >11</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row0_col1" class="data row0 col1" >$2.35</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row0_col2" class="data row0 col2" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level0_row1" class="row_heading level0 row1" >84</th> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level1_row1" class="row_heading level1 row1" >Arcane Gem</th> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row1_col0" class="data row1 col0" >11</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row1_col1" class="data row1 col1" >$2.23</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row1_col2" class="data row1 col2" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level0_row2" class="row_heading level0 row2" >31</th> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level1_row2" class="row_heading level1 row2" >Trickster</th> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row2_col0" class="data row2 col0" >9</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row2_col1" class="data row2 col1" >$2.07</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row2_col2" class="data row2 col2" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level0_row3" class="row_heading level0 row3" >175</th> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level1_row3" class="row_heading level1 row3" >Woeful Adamantite Claymore</th> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row3_col0" class="data row3 col0" >9</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row3_col1" class="data row3 col1" >$1.24</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row3_col2" class="data row3 col2" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level0_row4" class="row_heading level0 row4" >13</th> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level1_row4" class="row_heading level1 row4" >Serenity</th> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row4_col0" class="data row4 col0" >9</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row4_col1" class="data row4 col1" >$1.49</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row4_col2" class="data row4 col2" >$13.41</td> 
    </tr>    <tr> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level0_row5" class="row_heading level0 row5" >34</th> 
        <th id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3level1_row5" class="row_heading level1 row5" >Retribution Axe</th> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row5_col0" class="data row5 col0" >9</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row5_col1" class="data row5 col1" >$4.14</td> 
        <td id="T_8d7b6dc2_273e_11e8_8d20_80ce62114cc3row5_col2" class="data row5 col2" >$37.26</td> 
    </tr></tbody> 
</table> 



### Most Profitable Items


```python
#Group by Item ID & calculate "Purchase Count" per player
top5_items_df = pd.DataFrame(purchase_data.groupby("Item ID")["Item ID"].count())
#Group by Item ID & calculate "Total Purchase Value" per player
purch_total_top5_items_df = pd.DataFrame(purchase_data.groupby("Item ID")["Price"].sum())
#Merge above DataFrames
most_popular_items_df = pd.merge(top5_items_df,purch_total_top5_items_df, left_index=True, right_index=True)
#Clean Item ID values
no_dubs_item = purchase_data.drop_duplicates(["Item ID"], keep="last")
#Merge clean Item ID with current DataFrame
most_popular_items_df = pd.merge(most_popular_items_df, no_dubs_item, left_index=True, right_on=["Item ID"])
#Keep wanted columns
most_popular_items_df = most_popular_items_df[["Item ID","Item Name","Item ID_x","Price_y","Price_x"]]
#Set Item ID & Item Name as indices
most_popular_items_df.set_index(["Item ID", "Item Name"], inplace=True)
#Rename columns
most_popular_items_df.rename(columns={"Item ID_x":"Purchase Count","Price_y":"Item Price","Price_x":"Total Purchase Value"}, inplace=True)
#Sort by Total Purchase Value for most profitable (descending) & display top 5 most profitable items 
most_popular_items_df = most_popular_items_df.sort_values(by=["Total Purchase Value"], ascending=False).head(5)
#Format Most Popular Items DataFrame
most_popular_items_df.style.format({"Total Purchase Value":"${:,.2f}","Item Price":"${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_8d88748c_273e_11e8_a314_80ce62114cc3" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="index_name level1" >Item Name</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level0_row0" class="row_heading level0 row0" >34</th> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level1_row0" class="row_heading level1 row0" >Retribution Axe</th> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row0_col0" class="data row0 col0" >9</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row0_col1" class="data row0 col1" >$4.14</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row0_col2" class="data row0 col2" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level0_row1" class="row_heading level0 row1" >115</th> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level1_row1" class="row_heading level1 row1" >Spectral Diamond Doomblade</th> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row1_col0" class="data row1 col0" >7</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row1_col1" class="data row1 col1" >$4.25</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row1_col2" class="data row1 col2" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level0_row2" class="row_heading level0 row2" >32</th> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level1_row2" class="row_heading level1 row2" >Orenmir</th> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row2_col0" class="data row2 col0" >6</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row2_col1" class="data row2 col1" >$4.95</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row2_col2" class="data row2 col2" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level0_row3" class="row_heading level0 row3" >103</th> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level1_row3" class="row_heading level1 row3" >Singed Scalpel</th> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row3_col0" class="data row3 col0" >6</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row3_col1" class="data row3 col1" >$4.87</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row3_col2" class="data row3 col2" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level0_row4" class="row_heading level0 row4" >107</th> 
        <th id="T_8d88748c_273e_11e8_a314_80ce62114cc3level1_row4" class="row_heading level1 row4" >Splitter, Foe Of Subtlety</th> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row4_col0" class="data row4 col0" >8</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row4_col1" class="data row4 col1" >$3.61</td> 
        <td id="T_8d88748c_273e_11e8_a314_80ce62114cc3row4_col2" class="data row4 col2" >$28.88</td> 
    </tr></tbody> 
</table> 


