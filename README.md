# Analyzing Yelp Data For The Top 10 US States With The Most Restaurants

## About This Project
In this notebook, I performed exploratory data analysis on the Kaggle Yelp dataset. I focused on using text and visualizations to identify answers to questions centered around the metrics provided in the dataset. 

## Data Source
The data set used for this notebook was the [Kaggle Yelp Dataset](https://www.kaggle.com/datasets/yelp-dataset/yelp-dataset). The dataset is a subset of Yelp's businesses, reiews, and user data. This notebook utilized a subset of the business and review data centered around restaurants in specific states.

## Python Libraries Used
- pandas
- matplotlib
- seaborn
- folium
- word cloud

## Data Science Topic(s) Applied
- Exploratory Data Analysis
- Data Cleaning/Filtering
- Data Visualization

## Code Snippets From Various Visualizations

```python
### Visualizing the Restaurant Count For The Top 10 States - Categorical Single Barplot
plot_restaurant_count = sns.catplot(
    data = top_ten_states,
    x = "state",
    y = "count",
    kind = "bar",
    height = 12, 
    aspect = .85,
    edgecolor = "0"
)
plot_restaurant_count.set(
    title = "Top 10 States With The Most Restaurants",
    xlabel = "State",
    ylabel = "Count"
)
plt.show()
``` 
![single_barplot](/images/single_barplot.png)

```python
### Visualizing Restaurant Review Counts for the Top 10 States - Box and Whisker Plot
plot_resturant_review_count_distribution = sns.boxplot(
    data = subset_restaurants_top_ten,
    x = "review_count",
    y = "state",
    order = ["AZ", "FL", "ID", "IN", "LA", "MO", "NJ", "NV", "PA", "TN"],
    hue = "state",
    hue_order = ["AZ", "FL", "ID", "IN", "LA", "MO", "NJ", "NV", "PA", "TN"],
    dodge = False,
    orient = "h",
    flierprops = {"marker": "x"}
)
plot_resturant_review_count_distribution.set(
    title = "Review Count Distribution for the Top 10 States",
    xlabel = "Review Count",
    ylabel = "State"
)
plt.show()
```
![box_and_whisker_plot](/images/box_and_whisker_plot.png)

```python
### Visualizing the Top 3 Restaurants in Arizona - Folium Map
## Defining Figure Size
figure_arizona = folium.Figure(width = 750,height = 750)

## Generating the Map
map_arizona_top_three = folium.Map(
    location = [subset_restaurants_az_top_three["latitude"].mean(), subset_restaurants_az_top_three["longitude"].mean()],
    zoom_start = 10,
    min_zoom = 10, 
    tiles = "cartodbpositron"
    ).add_to(figure_arizona)

## Adding Markers For Restaurants
# Restaurant 1
folium.Marker(
    location = [subset_restaurants_az_top_three.iloc[0, 6], subset_restaurants_az_top_three.iloc[0, 7]],
    popup = folium.Popup(
        f"Name: {subset_restaurants_az_top_three.iloc[0, 1]}" 
        + "<br>" + 
        f"Address: {subset_restaurants_az_top_three.iloc[0, 2]}" 
        + "<br>" + 
        f"City: {subset_restaurants_az_top_three.iloc[0, 3]}" 
        + "<br>" + 
        f"State: {subset_restaurants_az_top_three.iloc[0, 4]}" 
        + "<br>" + 
        f"Stars: {subset_restaurants_az_top_three.iloc[0, 8]}" 
        + "<br>" + 
        f"Review Count: {subset_restaurants_az_top_three.iloc[0, 9]}"
        + "<br>" +
        f"Categories: {subset_restaurants_az_top_three.iloc[0, 12]}",
        min_width = 200, 
        max_width = 200
    ),
    parse_html = True,
    icon = folium.Icon(color = "black", prefix = "fa", icon = "utensils"),
).add_to(map_arizona_top_three)

# Restaurant 2
folium.Marker(
    location = [subset_restaurants_az_top_three.iloc[1, 6], subset_restaurants_az_top_three.iloc[1, 7]],
    popup = folium.Popup(
        f"Name: {subset_restaurants_az_top_three.iloc[1, 1]}" 
        + "<br>" + 
        f"Address: {subset_restaurants_az_top_three.iloc[1, 2]}" 
        + "<br>" + 
        f"City: {subset_restaurants_az_top_three.iloc[1, 3]}" 
        + "<br>" + 
        f"State: {subset_restaurants_az_top_three.iloc[1, 4]}" 
        + "<br>" + 
        f"Stars: {subset_restaurants_az_top_three.iloc[1, 8]}" 
        + "<br>" + 
        f"Review Count: {subset_restaurants_az_top_three.iloc[1, 9]}"
        + "<br>" +
        f"Categories: {subset_restaurants_az_top_three.iloc[1, 12]}", 
        min_width = 200, 
        max_width = 200
    ),
    parse_html = True,
    icon = folium.Icon(color = "black", prefix = "fa", icon = "utensils"),
).add_to(map_arizona_top_three)

# Restaurant 3
folium.Marker(
    location = [subset_restaurants_az_top_three.iloc[2, 6], subset_restaurants_az_top_three.iloc[2, 7]],
    popup = folium.Popup(
        f"Name: {subset_restaurants_az_top_three.iloc[2, 1]}" 
        + "<br>" + 
        f"Address: {subset_restaurants_az_top_three.iloc[2, 2]}" 
        + "<br>" + 
        f"City: {subset_restaurants_az_top_three.iloc[2, 3]}" 
        + "<br>" + 
        f"State: {subset_restaurants_az_top_three.iloc[2, 4]}" 
        + "<br>" + 
        f"Stars: {subset_restaurants_az_top_three.iloc[2, 8]}" 
        + "<br>" + 
        f"Review Count: {subset_restaurants_az_top_three.iloc[2, 9]}"
        + "<br>" +
        f"Categories: {subset_restaurants_az_top_three.iloc[2, 12]}",
        min_width = 200, 
        max_width = 200
    ),
    parse_html = True,
    icon = folium.Icon(color = "black", prefix = "fa", icon = "utensils"),
).add_to(map_arizona_top_three)

## Outputting the Map
map_arizona_top_three
```
![folium_map](/images/folium_map.gif)

```python
### Visualizing Review Text For Top Three Arizona Restaurants - Word Cloud
review_text_az = " ".join(review for review in subset_review_az_top["text"])
review_text_az_word_cloud = WordCloud(stopwords = stopwords, background_color = "white",  width = 1920, height = 1080, random_state = 1).generate(review_text_az)
plt.imshow(review_text_az_word_cloud, interpolation = "bilinear")
plt.title("Customer Reviews for Arizona's Top Three Restaurants", fontsize = 18, color = "black")
plt.axis("off")
plt.show()
```
![word_cloud](/images/word_cloud.png)

```python
### Review Sentiments for Arizona's Top Three Restaurants - Triple Barplot
plot_subset_review_az_top = subset_review_az_top.groupby(subset_review_az_top.business_id)[["useful", "funny", "cool"]].mean().plot(kind = "bar")
plt.title("Usefulness, Funniness, and Cooliness of Reviews for the Top 3 Restaurants in Arizona", fontsize = 18)
plt.xticks([0, 1, 2], ["Barista Del Barrio", "Tumerico", "Barrio Bread"], rotation = 360)
plot_subset_review_az_top.set_xlabel("Business Name")
plt.show()
```
![triple_barplot](/images/triple_barplot.png)
