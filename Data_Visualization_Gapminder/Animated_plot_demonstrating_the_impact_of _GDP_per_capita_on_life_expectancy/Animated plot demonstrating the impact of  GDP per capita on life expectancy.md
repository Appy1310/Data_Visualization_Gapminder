
## Animated plot demonstrating the impact of  GDP per capita on life expectancy


```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import imageio

sns.set()
```


```python
# Get the data (csv file is hosted on the web)
url = 'https://python-graph-gallery.com/wp-content/uploads/gapminderData.csv'
gapminder = pd.read_csv(url)
gapminder.head(5)
gapminder.loc[gapminder['year']==1962]

```


```python
gapminder['year'].unique()
years_to_plot = list(gapminder['year'].unique())
years_to_plot

```


```python
# Sample plot for a single year 

df = gapminder[ gapminder.year == 1977]

sns.scatterplot(x='lifeExp', y='gdpPercap', data=df , s=df['pop']/100000, hue = 'continent')
plt.show()
```


```python
# saving scatterplots for different years

years_to_plot = list(gapminder['year'].unique())
for year in years_to_plot:
    
        # Save scatterplots throughout the years 
        df = gapminder[ gapminder.year == year]
        # Setting up a white background
        sns.set_style("white")

        sns.scatterplot(x='lifeExp', y='gdpPercap', data=df , s=df['pop']/100000, hue = 'continent')

       # plt.title(f'Year: {year}')
       # plt.axis((0,10,0,100))
        plt.legend(loc='center right', bbox_to_anchor=(1.3, .5),borderaxespad=0)
        plt.tight_layout()
        sns.set(rc={'figure.figsize':(10,6)})
        #plt.show()
        plt.tight_layout()
        plt.savefig(f'snsplot_images_{year}.png')
        
        plt.figure()

        
images = []

#creating an animated GIF using saved pictures
for year in years_to_plot:
        # Create a gif from the images using the imageio library 
        filename = f'snsplot_images_{year}.png'
        image = imageio.imread(filename)
        images.append(image)
        
        
        
imageio.mimsave('output_sns_2.gif', images, duration = 0.50)
#imageio.mimsave('output_sns.gif', images, duration = 0.06)

```
