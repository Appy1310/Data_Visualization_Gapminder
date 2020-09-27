
##  Animated scatterplot demonstrating the impact of lower fertility on improved life expectancy




```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import imageio

sns.set()
```


```python
# Importing Data
fert = pd.read_csv('gapminder_total_fertility.csv', index_col=0)
life = pd.read_excel('gapminder_lifeexpectancy.xlsx', index_col=0)
population = pd.read_excel('gapminder_population.xlsx', index_col=0)
continents = pd.read_csv('continents.csv', sep=';')
continents = pd.read_csv('continents.csv', sep=';')



```


```python
fert.columns = fert.columns.astype(int)
fert.columns
```


```python
fert.index.name = 'country'
fert = fert.reset_index()
fert = fert.melt(id_vars='country', var_name='year', value_name='fertility_rate')
fert
```


```python
life.index.name = 'country'
life = life.reset_index()
life = life.melt(id_vars='country', var_name='year', value_name='life_expectancy')
life
```


```python
population.index.name = 'country'
population = population.reset_index()
population = population.melt(id_vars='country', var_name='year', value_name='population')
population
```


```python
df = fert.merge(population)
df = life.merge(df)
df = continents.merge(df)
df
```


```python
df_subset = df.loc[df['country'].isin(df['country'].unique())]
df_subset
```


```python
# Sample plot for a single year 

sns.set_style("white")

sns.scatterplot(x='fertility_rate', y='life_expectancy', s=df_subset['population']/100000, hue = 'continent',
            data=df_subset[ df_subset.year == 1960], alpha=0.6)
plt.legend(loc='center right', bbox_to_anchor=(1.5, .5),borderaxespad=0, ncol=1)
#plt.legend(loc='center left', bbox_to_anchor=(1.5, 0.5), ncol=1)
#plt.figure(figsize=(300,100))
sns.set(rc={'figure.figsize':(10,6)})

plt.tight_layout()
plt.show()

```


```python
# saving scatterplots for year 1960 to 2015

years_to_plot = list(range(1960,2015))
for year in years_to_plot:
    
        # Save scatterplots throughout 1960 to 2015 
        df = df_subset[ df_subset.year == year]
        #df = df['year']     tmp = data[ data.year == y ]
        sns.set_style("white")

        sns.scatterplot(x='fertility_rate', y='life_expectancy', s=df['population']/100000, hue = 'continent',
            data=df, alpha=0.6)

        plt.title(f'Year: {year}')
        plt.axis((0,10,0,100))
        plt.legend(loc='center right', bbox_to_anchor=(1.5, .5),borderaxespad=0)
        plt.tight_layout()
        sns.set(rc={'figure.figsize':(10,6)})



        plt.savefig(f'snsplot_images_{year}.png')
        
        plt.figure()

        
images = []

#creating an animated GIF using saved pictures
for year in years_to_plot:
        # Create a gif from the images using the imageio library 
        filename = f'snsplot_images_{year}.png'
        image = imageio.imread(filename)
        images.append(image)
        
        
        
imageio.mimsave('output_sns.gif', images, duration = 0.3)
#imageio.mimsave('output_sns.gif', images, duration = 0.06)

```
