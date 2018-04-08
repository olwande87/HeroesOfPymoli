

```python
import pandas as pd
import numpy as np
```


```python
# The path to file
file = "purchase_data.json"
file_df = pd.read_json(file, orient = "records")
file_df.head()

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




```python
#Total number of players
player_demographics = file_df.loc[:, ["Gender", "SN", "Age"]]
player_demographics.head()
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
      <th>Gender</th>
      <th>SN</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>Aelalis34</td>
      <td>38</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>Eolo46</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>Assastnya25</td>
      <td>34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>Pheusrical25</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Male</td>
      <td>Aela59</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total number of players
player_demographics = player_demographics.drop_duplicates()
num_players = player_demographics.count()[0]
num_players
```




    573




```python

pd.DataFrame({"Total Players": [num_players]})
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total number of unique items
average_item_price = file_df["Price"].mean()
total_purchase_value = file_df["Price"].sum()
purchase_count = file_df["Price"].count()
item_count = len(file_df["Item ID"].unique())

# Create DataFrame to hold results
summary_table = pd.DataFrame({"Number of Unique Items": item_count,
                               "Total Revenue": [total_purchase_value],
                               "Number of Purchases": [purchase_count],
                               "Average Purchase Price": [average_item_price]})

# Formating
summary_table = summary_table.round(2)
summary_table ["Average Price"] = summary_table["Average Purchase Price"].map("${:,.2f}".format)
summary_table ["Total Revenue"] = summary_table["Total Revenue"].map("${:,.2f}".format)
summary_table = summary_table.loc[:, ["Number of Unique Items", "Average Purchase Price", "Number of Purchases", "Total Revenue"]]

summary_table
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
      <th>Number of Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchase count
gender_demographics_totals = player_demographics["Gender"].value_counts()
gender_demographics_percents = gender_demographics_totals / num_players * 100

# Rename columns
gender_demographics = pd.DataFrame({"Total Count": gender_demographics_totals,
                                    "Percentage of Players": gender_demographics_percents})

gender_demographics = gender_demographics.round(2)


gender_demographics
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing analysis
gender_purchase_total = file_df.groupby(["Gender"]).sum()["Price"].rename("Total Purchase Value")
gender_average = file_df.groupby(["Gender"]).mean()["Price"].rename("Average Purchase Value")
gender_counts = file_df.groupby(["Gender"]).count()["Price"].rename("Purchase Count")


# normalized Purchasing
normalized_total = gender_purchase_total / gender_demographics["Total Count"]

# Rename columns
gender_data = pd.DataFrame({"Normalized Total": normalized_total, 
                            "Purchase Count": gender_counts, 
                            "Total Purchase Value": gender_purchase_total, 
                            "Average Purchase Value": gender_average})
gender_data
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
      <th>Average Purchase Value</th>
      <th>Normalized Total</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>2.815515</td>
      <td>3.829100</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>2.950521</td>
      <td>4.016516</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>3.249091</td>
      <td>4.467500</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_bins = [0, 9.90, 14.90, 19.90, 24.9, 29.9, 34.90, 39.90, 9999999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", ">40"]

#Age Demographics
player_demographics["Age Ranges"] = pd.cut(player_demographics["Age"], age_bins, labels=group_names)
player_demographics.head()
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
      <th>Gender</th>
      <th>SN</th>
      <th>Age</th>
      <th>Age Ranges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>Aelalis34</td>
      <td>38</td>
      <td>35-39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>Eolo46</td>
      <td>21</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>Assastnya25</td>
      <td>34</td>
      <td>30-34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>Pheusrical25</td>
      <td>21</td>
      <td>20-24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Male</td>
      <td>Aela59</td>
      <td>23</td>
      <td>20-24</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_demographics_totals = player_demographics["Age Ranges"].value_counts()
age_demographics_percents = age_demographics_totals / num_players * 100

age_demographics = pd.DataFrame({"Total Count": age_demographics_totals, "Percent of Players": age_demographics_percents})
age_demographics = age_demographics.sort_index()
age_demographics
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
      <th>Percent of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.315881</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.013962</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.452007</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.200698</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.183246</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.202443</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.712042</td>
      <td>27</td>
    </tr>
    <tr>
      <th>&gt;40</th>
      <td>1.919721</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top spenders
user_total = file_df.groupby(["SN"]).sum()["Price"].rename("Total Purchase Amount")
user_average = file_df.groupby(["SN"]).mean()["Price"].rename("Average Purchase Price")
user_count = file_df.groupby(["SN"]).count()["Price"].rename("Purchase Count")

user_data = pd.DataFrame({"Total Purchase Amount": user_total,
                          "Average Purchase Price": user_average,
                          "Purchase Count": user_count})

# Data Frame
user_data.sort_values("Total Purchase Amount", ascending=False).head(5)
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
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Amount</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>3.412000</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3.390000</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>3.185000</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>4.243333</td>
      <td>3</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3.860000</td>
      <td>3</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most popular items
user_total = file_df.groupby(["Item ID", "Item Name"]).sum()["Price"]
user_total.head()
```




    Item ID  Item Name         
    0        Splinter              1.82
    1        Crucifer              9.12
    2        Verdict               3.40
    3        Phantomlight          1.79
    4        Bloodlord's Fetish    2.28
    Name: Price, dtype: float64


