# chatGPT4o rewrite of tutorial code (tutorial: https://www.natasshaselvaraj.com/interactive-dashboard-python/)
# chatGPT provide the following notes with the code, related to python version changes:
#    The deprecated imports dash_core_components and dash_html_components are replaced with from dash import dcc, html.
#    The choropleth map is updated to use current properties and formats.
#    The bar and pie charts are updated similarly to maintain consistency and readability.

#in addition to the tutorial instructions for installing python libraries as prerequisities, this also required
#this library installation:  pip install dash-boostrap-components

import dash
from dash import dcc, html
import dash_bootstrap_components as dbc
from dash.dependencies import Input, Output, State
import plotly.graph_objs as go
import pandas as pd

external_stylesheets = [
    'https://codepen.io/chriddyp/pen/bWLwgP.css',
    dbc.themes.BOOTSTRAP,
    'style.css'
]

app = dash.Dash(__name__, external_stylesheets=external_stylesheets)
server = app.server

df = pd.read_csv('restaurants_zomato.csv', encoding="ISO-8859-1")

navbar = dbc.Nav()

# Choropleth map
col_label = "country_code"
col_values = "count"
v = df[col_label].value_counts()
new = pd.DataFrame({
    col_label: v.index,
    col_values: v.values
})
map_fig = dcc.Graph(
    id='map',
    figure={
        'data': [{
            'locations': new['country_code'],
            'z': new['count'],
            'colorscale': 'Earth',
            'reversescale': True,
            'hoverinfo': 'location+z',
            'type': 'choropleth'
        }],
        'layout': {
            'title': {
                'text': 'Restaurant Frequency by Location',
                'font': {
                    'size': 20,
                    'color': 'white'
                }
            },
            "paper_bgcolor": "#111111",
            "plot_bgcolor": "#111111",
            "height": 800,
            "geo": {
                'bgcolor': 'rgba(0,0,0,0)'
            }
        }
    }
)

# Bar chart - Top Restaurants in India
df2 = df.groupby(by='Restaurant Name')['Votes'].mean().reset_index().sort_values('Votes', ascending=False)
df3 = df2.head(10)
bar1 = dcc.Graph(
    id='bar1',
    figure={
        'data': [go.Bar(
            x=df3['Restaurant Name'],
            y=df3['Votes']
        )],
        'layout': {
            'title': {
                'text': 'Top Restaurants in India',
                'font': {'size': 20, 'color': 'white'}
            },
            "paper_bgcolor": "#111111",
            "plot_bgcolor": "#111111",
            'height': 600,
            "line": {"color": "white", "width": 4, "dash": "dash"},
            'xaxis': {
                'tickfont': {'color': 'white'},
                'showgrid': False,
                'title': '',
                'color': 'white'
            },
            'yaxis': {
                'tickfont': {'color': 'white'},
                'showgrid': False,
                'title': 'Number of Votes',
                'color': 'white'
            }
        }
    }
)

# Pie chart - Rating Distribution
col_label = "Rating text"
col_values = "Count"
v = df[col_label].value_counts()
new2 = pd.DataFrame({
    col_label: v.index,
    col_values: v.values
})
pie3 = dcc.Graph(
    id="pie3",
    figure={
        "data": [{
            "labels": new2['Rating text'],
            "values": new2['Count'],
            "hoverinfo": "label+percent",
            "hole": 0.7,
            "type": "pie",
            'marker': {'colors': ['#0052cc', '#3385ff', '#99c2ff']},
            "showlegend": True
        }],
        "layout": {
            "title": {
                "text": "Rating Distribution",
                "font": {'size': 20, 'color': 'white'}
            },
            "paper_bgcolor": "#111111",
            "showlegend": True,
            'height': 600,
            'marker': {'colors': ['#0052cc', '#3385ff', '#99c2ff']},
            "annotations": [{
                "font": {"size": 20},
                "showarrow": False,
                "text": "",
                "x": 0.2,
                "y": 0.2
            }],
            "legend": {
                "fontColor": "white",
                "tickfont": {'color': 'white'}
            },
            "legenditem": {
                "textfont": {'color': 'white'}
            }
        }
    }
)

graphRow1 = dbc.Row([dbc.Col(map_fig, md=12)])
graphRow2 = dbc.Row([dbc.Col(bar1, md=6), dbc.Col(pie3, md=6)])

app.layout = html.Div([navbar, html.Br(), graphRow1, html.Br(), graphRow2], style={'backgroundColor': 'black'})

if __name__ == '__main__':
    app.run_server(debug=True, port=8056)
