
##Heroes of Pimoli Data Analysis

####-There are a lot of repeat purchasers. Look for trends on why they buy what they buy. What are the trends of these items in gameplay?
####-Ages 20-24 are spending the most bread. Market to them and look at what they tend to purchase. Look for ways to grow this bracket.
####-High-priced, rare items are worth while. The revenue collected from their sales is Lit.


```python
#dependencies
import numpy as np
import pandas as pd
```


```python
#file path
file = "Resources/purchase_data.json"
```


```python
#make a data frame with json data
df = pd.read_json(file)
df.head()
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
#count total users
users = df["SN"].nunique()
#rename SN to total users
total_users = pd.DataFrame({"Total Users": [users]}, columns=["Total Users"])
total_users
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
      <th>Total Users</th>
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
#count UNIQUE items
unique_items = df["Item ID"].nunique()
#count total sales
total_sales = df["Price"].count()
#sum total revenue
total_revenue = df["Price"].sum()
#get average item price
avg_price = round(df["Price"].mean(), 2)
#summary analysis
purchase_analysis = pd.DataFrame({"Unique Items": [unique_items],
                                  "Total Sales": [total_sales],
                                  "Total Revenue": [total_revenue],
                                  "Average Purchase Price": [avg_price]},
                                  columns= ["Unique Items", "Total Sales",
                                  "Total Revenue", "Average Purchase Price"])
#add $ signs
purchase_analysis.style.format({"Total Revenue": "${:.2f}", "Average Purchase Price": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_c4aad1ae_74bb_11e8_b6c0_5a0024898601" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Unique Items</th> 
        <th class="col_heading level0 col1" >Total Sales</th> 
        <th class="col_heading level0 col2" >Total Revenue</th> 
        <th class="col_heading level0 col3" >Average Purchase Price</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c4aad1ae_74bb_11e8_b6c0_5a0024898601level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_c4aad1ae_74bb_11e8_b6c0_5a0024898601row0_col0" class="data row0 col0" >183</td> 
        <td id="T_c4aad1ae_74bb_11e8_b6c0_5a0024898601row0_col1" class="data row0 col1" >780</td> 
        <td id="T_c4aad1ae_74bb_11e8_b6c0_5a0024898601row0_col2" class="data row0 col2" >$2286.33</td> 
        <td id="T_c4aad1ae_74bb_11e8_b6c0_5a0024898601row0_col3" class="data row0 col3" >$2.93</td> 
    </tr></tbody> 
</table> 




```python
#count males
male_users = df[df["Gender"] == "Male"]["SN"].nunique()
#count females
female_users = df[df["Gender"] == "Female"]["SN"].nunique()
#count enbies
other_users = users - male_users - female_users
#%male
percent_male = ((male_users/users)*100)
#%female
percent_female = ((female_users/users)*100)
#%enby
percent_other = ((other_users/users)*100)
#summary analysis
gender_analysis = pd.DataFrame({"Gender": ["Male", "Female", "Other / Non-Disclosed"],
                                "Player %": [percent_male, percent_female, percent_other],
                                "Totals": [male_users, female_users, other_users]},
                                columns= ["Gender", "Player %", "Totals"])
#make gender the first column
gender_analysis = gender_analysis.set_index("Gender")
#sdd percentage symbol
gender_analysis.style.format({"Player %": "{:.2f}%"}) 
```




<style  type="text/css" >
</style>  
<table id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Player %</th> 
        <th class="col_heading level0 col1" >Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601row0_col0" class="data row0 col0" >81.15%</td> 
        <td id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601row0_col1" class="data row0 col1" >465</td> 
    </tr>    <tr> 
        <th id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601row1_col0" class="data row1 col0" >17.45%</td> 
        <td id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601row1_col1" class="data row1 col1" >100</td> 
    </tr>    <tr> 
        <th id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601row2_col0" class="data row2 col0" >1.40%</td> 
        <td id="T_c4b8e2c6_74bb_11e8_92a2_5a0024898601row2_col1" class="data row2 col1" >8</td> 
    </tr></tbody> 
</table> 




```python
#count sales to males
male_sales = df[df["Gender"] == "Male"]["Price"].count()
#count sales to females
female_sales = df[df["Gender"] == "Female"]["Price"].count()
#count sales to enbies
other_sales = total_sales - female_sales - male_sales
#average sale price for males
male_sales_avg = df[df["Gender"] == "Male"]['Price'].mean()
#average sale price for females
female_sales_avg = df[df["Gender"] == "Female"]['Price'].mean()
#average sale price for others
other_sales_avg = df[df["Gender"] == "Other / Non-Disclosed"]['Price'].mean()
#sum total male revenue
male_rev_total = df[df["Gender"] == "Male"]['Price'].sum()
#sum total female revenue
female_rev_total = df[df["Gender"] == "Female"]['Price'].sum()
#sum total enbie revenue
other_rev_total = df[df["Gender"] == "Other / Non-Disclosed"]['Price'].sum()
# normalized totals...whatever that means...GOOGLE!
#male normz
male_normz = male_rev_total/male_users
#female normz
female_normz = female_rev_total/female_users
#other normz
other_normz = other_rev_total/other_users

#make le dataframe for summary analysis
gender_sales = pd.DataFrame({"Gender": ["Male", "Female", "Other / Non-Disclosed"], 
                             "Sale Count": [male_sales, female_sales, other_sales],
                             "Average Sale Price": [male_sales_avg, female_sales_avg, other_sales_avg], 
                             "Total Revenue Values": [male_rev_total, female_rev_total, other_rev_total],
                             "Normalized Totals": [male_normz, female_normz, other_normz]},
                             columns =["Gender", "Sale Count", "Average Sale Price",
                             "Total Revenue Values", "Normalized Totals"])
#set gender in index column
gender_sales = gender_sales.set_index("Gender")
#format data for appropriate symbols
gender_sales.style.format({"Average Sale Price": "${:.2f}", "Total Revenue Values": "${:.2f}", "Normalized Totals": "${:.2f}" })
```




<style  type="text/css" >
</style>  
<table id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Sale Count</th> 
        <th class="col_heading level0 col1" >Average Sale Price</th> 
        <th class="col_heading level0 col2" >Total Revenue Values</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row0_col0" class="data row0 col0" >633</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row0_col1" class="data row0 col1" >$2.95</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row0_col2" class="data row0 col2" >$1867.68</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row0_col3" class="data row0 col3" >$4.02</td> 
    </tr>    <tr> 
        <th id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row1_col0" class="data row1 col0" >136</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row1_col1" class="data row1 col1" >$2.82</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row1_col2" class="data row1 col2" >$382.91</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row1_col3" class="data row1 col3" >$3.83</td> 
    </tr>    <tr> 
        <th id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row2_col0" class="data row2 col0" >11</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row2_col1" class="data row2 col1" >$3.25</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row2_col2" class="data row2 col2" >$35.74</td> 
        <td id="T_c4d162ec_74bb_11e8_8ba5_5a0024898601row2_col3" class="data row2 col3" >$4.47</td> 
    </tr></tbody> 
</table> 




```python
#make player binzzzz..ugh this is such a long assignment *cries*
tenyears = df[df["Age"] <10]
loteens = df[(df["Age"] >=10) & (df["Age"] <=14)]
hiteens = df[(df["Age"] >=15) & (df["Age"] <=19)]
lotwent = df[(df["Age"] >=20) & (df["Age"] <=24)]
hitwent = df[(df["Age"] >=25) & (df["Age"] <=29)]
lothirt = df[(df["Age"] >=30) & (df["Age"] <=34)]
hithirt = df[(df["Age"] >=35) & (df["Age"] <=39)]
loforty = df[(df["Age"] >=40) & (df["Age"] <=44)]
hiforty = df[(df["Age"] >=45) & (df["Age"] <=49)]

#summary analysis
age_sales_analysis = pd.DataFrame({"Age": ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40-44", "45-49"],
                                   "Sales Count": [tenyears["Price"].count(), loteens["Price"].count(), hiteens["Price"].count(), lotwent["Price"].count(), hitwent["Price"].count(), lothirt["Price"].count(), hithirt["Price"].count(), loforty["Price"].count(), hiforty["Price"].count()],
                                   "Average Sale Price": [tenyears["Price"].mean(), loteens["Price"].mean(), hiteens["Price"].mean(), lotwent["Price"].mean(), hitwent["Price"].mean(), lothirt["Price"].mean(), hithirt["Price"].mean(), loforty["Price"].mean(), hiforty["Price"].mean()], 
                                   "Total Revenue Values": [tenyears["Price"].sum(), loteens["Price"].sum(), hiteens["Price"].sum(), lotwent["Price"].sum(), hitwent["Price"].sum(), lothirt["Price"].sum(), hithirt["Price"].sum(), loforty["Price"].sum(), hiforty["Price"].sum()],
                                   "Normalized Totals": [tenyears["Price"].sum()/tenyears['SN'].nunique(), loteens["Price"].sum()/loteens['SN'].nunique(), hiteens["Price"].sum()/hiteens['SN'].nunique(), 
                                    lotwent["Price"].sum()/lotwent['SN'].nunique(), hitwent["Price"].sum()/hitwent['SN'].nunique(), 
                                    lothirt["Price"].sum()/lothirt['SN'].nunique(), hithirt["Price"].sum()/hithirt['SN'].nunique(), 
                                    loforty["Price"].sum()/loforty['SN'].nunique(), hiforty["Price"].sum()/hiforty['SN'].nunique()]}, 
                                    columns = ["Age", "Sales Count", "Average Sale Price", "Total Revenue Values", "Normalized Totals"])

#set age to first column
age_sales_analysis= age_sales_analysis.set_index("Age")
#config the symbols...again...this is tedious yo.
age_sales_analysis.style.format({"Average Sale Price": "${:.2f}", "Total Revenue Values": "${:.2f}", "Normalized Totals": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Sales Count</th> 
        <th class="col_heading level0 col1" >Average Sale Price</th> 
        <th class="col_heading level0 col2" >Total Revenue Values</th> 
        <th class="col_heading level0 col3" >Normalized Totals</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row0" class="row_heading level0 row0" ><10</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row0_col0" class="data row0 col0" >28</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row0_col1" class="data row0 col1" >$2.98</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row0_col2" class="data row0 col2" >$83.46</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row0_col3" class="data row0 col3" >$4.39</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row1_col0" class="data row1 col0" >35</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row1_col1" class="data row1 col1" >$2.77</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row1_col2" class="data row1 col2" >$96.95</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row1_col3" class="data row1 col3" >$4.22</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row2_col0" class="data row2 col0" >133</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row2_col1" class="data row2 col1" >$2.91</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row2_col2" class="data row2 col2" >$386.42</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row2_col3" class="data row2 col3" >$3.86</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row3_col0" class="data row3 col0" >336</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row3_col1" class="data row3 col1" >$2.91</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row3_col2" class="data row3 col2" >$978.77</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row3_col3" class="data row3 col3" >$3.78</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row4_col0" class="data row4 col0" >125</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row4_col1" class="data row4 col1" >$2.96</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row4_col2" class="data row4 col2" >$370.33</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row4_col3" class="data row4 col3" >$4.26</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row5_col0" class="data row5 col0" >64</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row5_col1" class="data row5 col1" >$3.08</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row5_col2" class="data row5 col2" >$197.25</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row5_col3" class="data row5 col3" >$4.20</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row6_col0" class="data row6 col0" >42</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row6_col1" class="data row6 col1" >$2.84</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row6_col2" class="data row6 col2" >$119.40</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row6_col3" class="data row6 col3" >$4.42</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row7" class="row_heading level0 row7" >40-44</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row7_col0" class="data row7 col0" >16</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row7_col1" class="data row7 col1" >$3.19</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row7_col2" class="data row7 col2" >$51.03</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row7_col3" class="data row7 col3" >$5.10</td> 
    </tr>    <tr> 
        <th id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601level0_row8" class="row_heading level0 row8" >45-49</th> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row8_col0" class="data row8 col0" >1</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row8_col1" class="data row8 col1" >$2.72</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row8_col2" class="data row8 col2" >$2.72</td> 
        <td id="T_c4e9d3e8_74bb_11e8_bc09_5a0024898601row8_col3" class="data row8 col3" >$2.72</td> 
    </tr></tbody> 
</table> 




```python
#grab series of highest total sales revenue for all users
sn_total_purchase = df.groupby('SN')['Price'].sum().to_frame()
#grab series of highest total sales count for all user
sn_purchase_count = df.groupby('SN')['Price'].count().to_frame()
#grab series of highest total avg purchase price for all user
sn_purchase_avg = df.groupby('SN')['Price'].mean().to_frame()

#change names and generate column for total purchase value
sn_total_purchase.columns=["Total Purchase Value"]
join_one = sn_total_purchase.join(sn_purchase_count, how="left")
join_one.columns=["Total Purchase Value", "Purchase Count"]

#merge all value columns
join_two = join_one.join(sn_purchase_avg, how="inner")
join_two.columns=["Total Purchase Value", "Purchase Count", "Average Purchase Price"]

#generate top 5 spenders and format data...
top_spenders_df = join_two[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
top_spenders_final = top_spenders_df.sort_values('Total Purchase Value', ascending=False).head()
top_spenders_final.style.format({"Average Purchase Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_c4fd5802_74bb_11e8_bade_5a0024898601" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >SN</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c4fd5802_74bb_11e8_bade_5a0024898601level0_row0" class="row_heading level0 row0" >Undirrala66</th> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row0_col0" class="data row0 col0" >5</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row0_col1" class="data row0 col1" >$3.41</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row0_col2" class="data row0 col2" >$17.06</td> 
    </tr>    <tr> 
        <th id="T_c4fd5802_74bb_11e8_bade_5a0024898601level0_row1" class="row_heading level0 row1" >Saedue76</th> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row1_col0" class="data row1 col0" >4</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row1_col1" class="data row1 col1" >$3.39</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row1_col2" class="data row1 col2" >$13.56</td> 
    </tr>    <tr> 
        <th id="T_c4fd5802_74bb_11e8_bade_5a0024898601level0_row2" class="row_heading level0 row2" >Mindimnya67</th> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row2_col0" class="data row2 col0" >4</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row2_col1" class="data row2 col1" >$3.18</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row2_col2" class="data row2 col2" >$12.74</td> 
    </tr>    <tr> 
        <th id="T_c4fd5802_74bb_11e8_bade_5a0024898601level0_row3" class="row_heading level0 row3" >Haellysu29</th> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row3_col0" class="data row3 col0" >3</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row3_col1" class="data row3 col1" >$4.24</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row3_col2" class="data row3 col2" >$12.73</td> 
    </tr>    <tr> 
        <th id="T_c4fd5802_74bb_11e8_bade_5a0024898601level0_row4" class="row_heading level0 row4" >Eoda93</th> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row4_col0" class="data row4 col0" >3</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row4_col1" class="data row4 col1" >$3.86</td> 
        <td id="T_c4fd5802_74bb_11e8_bade_5a0024898601row4_col2" class="data row4 col2" >$11.58</td> 
    </tr></tbody> 
</table> 




```python
#merge dataframes to find purchase count, total purchase value for items
#reset indices to dataframes can be merged on specific elements
premergeone = df.groupby("Item Name").sum().reset_index()
premergetwo = df.groupby("Item ID").sum().reset_index()
premergethree = df.groupby("Item Name").count().reset_index()

#merge dataframes
mergeone = pd.merge(premergeone, premergetwo, on="Price")
mergetwo = pd.merge(premergethree, mergeone, on="Item Name")

#begin creating final dataframe by manipulating data
mergetwo["Gender"] = (mergetwo["Price_y"]/mergetwo["Item ID"]).round(2)

mergetwo_renamed = mergetwo.rename(columns={"Age": "Purchase Count", "Gender": "Item Price", "Item ID": "null", "Price_y": "Total Purchase Value", "Item ID_y": "Item ID"})

#grab columns we are looking for
clean_df = mergetwo_renamed[["Item ID", "Item Name", "Purchase Count", "Item Price", "Total Purchase Value"]]

prefinal_df = clean_df.set_index(['Item Name', 'Item ID'])
popular_items_final = prefinal_df.sort_values('Purchase Count', ascending=False).head(6)
popular_items_final.style.format({"Item Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_c52556c2_74bb_11e8_9806_5a0024898601" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item Name</th> 
        <th class="index_name level1" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level0_row0" class="row_heading level0 row0" >Arcane Gem</th> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level1_row0" class="row_heading level1 row0" >84</th> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row0_col0" class="data row0 col0" >11</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row0_col1" class="data row0 col1" >$2.23</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row0_col2" class="data row0 col2" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level0_row1" class="row_heading level0 row1" >Betrayal, Whisper of Grieving Widows</th> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level1_row1" class="row_heading level1 row1" >39</th> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row1_col0" class="data row1 col0" >11</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row1_col1" class="data row1 col1" >$2.35</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row1_col2" class="data row1 col2" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level0_row2" class="row_heading level0 row2" >Trickster</th> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level1_row2" class="row_heading level1 row2" >31</th> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row2_col0" class="data row2 col0" >9</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row2_col1" class="data row2 col1" >$2.07</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row2_col2" class="data row2 col2" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level0_row3" class="row_heading level0 row3" >Woeful Adamantite Claymore</th> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level1_row3" class="row_heading level1 row3" >175</th> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row3_col0" class="data row3 col0" >9</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row3_col1" class="data row3 col1" >$1.24</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row3_col2" class="data row3 col2" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level0_row4" class="row_heading level0 row4" >Serenity</th> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level1_row4" class="row_heading level1 row4" >13</th> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row4_col0" class="data row4 col0" >9</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row4_col1" class="data row4 col1" >$1.49</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row4_col2" class="data row4 col2" >$13.41</td> 
    </tr>    <tr> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level0_row5" class="row_heading level0 row5" >Retribution Axe</th> 
        <th id="T_c52556c2_74bb_11e8_9806_5a0024898601level1_row5" class="row_heading level1 row5" >34</th> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row5_col0" class="data row5 col0" >9</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row5_col1" class="data row5 col1" >$4.14</td> 
        <td id="T_c52556c2_74bb_11e8_9806_5a0024898601row5_col2" class="data row5 col2" >$37.26</td> 
    </tr></tbody> 
</table> 




```python
#use prefinal dataframe from prior to step to find most profitable items
profit_items_final = prefinal_df.sort_values('Total Purchase Value', ascending=False).head()
#final format. hopefully we lean a faster wqay to do this...
profit_items_final.style.format({"Item Price": "${:.2f}", "Total Purchase Value": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_c5272418_74bb_11e8_80b4_5a0024898601" > 
<thead>    <tr> 
        <th class="blank" ></th> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Item Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item Name</th> 
        <th class="index_name level1" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level0_row0" class="row_heading level0 row0" >Retribution Axe</th> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level1_row0" class="row_heading level1 row0" >34</th> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row0_col0" class="data row0 col0" >9</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row0_col1" class="data row0 col1" >$4.14</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row0_col2" class="data row0 col2" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level0_row1" class="row_heading level0 row1" >Spectral Diamond Doomblade</th> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level1_row1" class="row_heading level1 row1" >115</th> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row1_col0" class="data row1 col0" >7</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row1_col1" class="data row1 col1" >$4.25</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row1_col2" class="data row1 col2" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level0_row2" class="row_heading level0 row2" >Orenmir</th> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level1_row2" class="row_heading level1 row2" >32</th> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row2_col0" class="data row2 col0" >6</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row2_col1" class="data row2 col1" >$4.95</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row2_col2" class="data row2 col2" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level0_row3" class="row_heading level0 row3" >Singed Scalpel</th> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level1_row3" class="row_heading level1 row3" >103</th> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row3_col0" class="data row3 col0" >6</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row3_col1" class="data row3 col1" >$4.87</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row3_col2" class="data row3 col2" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level0_row4" class="row_heading level0 row4" >Splitter, Foe Of Subtlety</th> 
        <th id="T_c5272418_74bb_11e8_80b4_5a0024898601level1_row4" class="row_heading level1 row4" >107</th> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row4_col0" class="data row4 col0" >8</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row4_col1" class="data row4 col1" >$3.61</td> 
        <td id="T_c5272418_74bb_11e8_80b4_5a0024898601row4_col2" class="data row4 col2" >$28.88</td> 
    </tr></tbody> 
</table> 


