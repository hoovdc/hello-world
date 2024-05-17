#based on this tutorial: https://www.natasshaselvaraj.com/interactive-dashboard-python/
#tutorial was written 2021, so I'm just going to have chatGPT rewrite all the code for current version of Python, 
#and then I'll review/study/run that.
#see PythonDashV2

import dash
from dash import dcc
from dash import html
# import dash_bootstrap_components as dbc
from dash.dependencies import Input, Output, State
import plotly.graph_objs as go
import plotly.express as px
import numpy as np
import pandas as pd

external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']

app = dash.Dash (__name__,external_stylesheets=external_stylesheets)
server = app.server

df = pd.read_csv('restaurants_zomato.csv', encoding="ISO-8859-1")

#country iso with counts
col_label = "country_code"
col_values = "count"

v = df[col_label].value_counts()
new = pd.DataFrame({
    col_label: v.index,
    col_values: v.values
})

hexcode = 0

#the tutorial has a different version of this code in the step-by-step section versus 
#the final copy of the code at the bottom of the tutorial.  Using final copy from bottom here and other places

borders = [hexcode for x in range(len(new))],
map = dcc.Graph(
        id='8',
        figure = {
        'data': [{
        'locations':new['country_code'],
        'z':new['count'],
        'colorscale': 'Earth',
        'reversescale':True,
        'hover-name':new['country_code'],
        'type': 'choropleth'
    
    }],

    'layout':{'title':dict(
            
        text = 'Restaurant Frequency by Location',
        font = dict(size=20,
        color = 'white')),
        "paper_bgcolor":"#111111",
        "plot_bgcolor":"#111111",
        "height": 800,
        "geo":dict(bgcolor= 'rgba(0,0,0,0)') } 
        
        }
)

df2 = pd.DataFrame(df.groupby(by="Restaurant Name")['Votes'].mean())
df2 = df2.reset_index()
df2 = df2.sort_values(['Votes'], ascending=False)
df3 = df2.head(10)

bar1 = dcc.Graph(id='bar1',
                figure={
        'data': [go.Bar(x=df3['Restaurant Name'],
                                y=df3['Votes'])],
                'layout': {'title':dict(
                    text = 'Top Restaurants in India',
                    font = dict(size=20,
                    color = 'white')),
                "paper_bgcolor":"#111111",
                "plot_bgcolor":"#111111",
                'height':600,
                "line":dict(
                        color="white",
                        width=4,
                        dash="dash",
                    ),
                'xaxis' : dict(tickfont=dict(
                    color='white'),showgrid=False,title='',color='white'),
                'yaxis' : dict(tickfont=dict(
                    color='white'),showgrid=False,title='Number of Votes',color='white')
            }})

col_label = "Rating text"
col_values = "Count"

v = df[col_label].value_counts()
new2 = pd.DataFrame({
    col_label: v.index,
    col_values: v.values
})
          
pie3 = dcc.Graph(
        id = "pie3",
        figure = {
          "data": [
            {
            "labels":new2['Rating text'],
            "values":new2['Count'],
              "hoverinfo":"label+percent",
              "hole": .7,
              "type": "pie",
                 'marker': {'colors': [
                                                   '#0052cc',  
                                                   '#3385ff',
                                                   '#99c2ff'
                                                  ]
                                       },
             "showlegend": True
}],
          "layout": {
                "title" : dict(text ="Rating Distribution",
                               font =dict(
                               size=20,
                               color = 'white')),
                "paper_bgcolor":"#111111",
                "showlegend":True,
                'height':600,
                'marker': {'colors': [
                                                 '#0052cc',  
                                                 '#3385ff',
                                                 '#99c2ff'
                                                ]
                                     },
                "annotations": [
                    {
                        "font": {
                            "size": 20
                        },
                        "showarrow": False,
                        "text": "",
                        "x": 0.2,
                        "y": 0.2
                    }
                ],
                "showlegend": True,
                "legend":dict(fontColor="white",tickfont={'color':'white' }),
                "legenditem": {
    "textfont": {
       'color':'white'
     }
              }
        } }
)

#deviating from the tutorial, with ChatGPT's help, because libraries used in tuturial have changed since tutorial was written

# Redefine the choropleth map using current libraries 
map_fig = go.Figure(data=go.Choropleth(
    locations=new['country_code'],
    z=new['count'],
    colorscale='Earth',
    reversescale=True,
    locationmode='ISO-3',  # Assuming ISO-3 country codes are used
   hovertext=new['country_code'],
    type='choropleth'
))

#rewriting the tutorial code on bar chart for current python language and libraries

# Define the layout of the app
app.layout = html.Div([
    # First row with the choropleth map
    html.Div([
        dcc.Graph(figure=map_fig)
    ], style={'width': '100%', 'padding': '20px', 'boxSizing': 'border-box'}),

    # Second row with bar1 and pie3 side-by-side
    html.Div([
        html.Div([
            dcc.Graph(figure=bar1)
        ], style={'width': '50%', 'display': 'inline-block', 'padding': '20px', 'boxSizing': 'border-box'}),
       html.Div([
            dcc.Graph(figure=pie3)
        ], style={'width': '50%', 'display': 'inline-block', 'padding': '20px', 'boxSizing': 'border-box'})
    ], style={'width': '100%', 'display': 'flex', 'justifyContent': 'space-between'})

])

# Run the app
if __name__ == '__main__':
    app.run_server(debug=True)

