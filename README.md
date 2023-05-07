# Sonification Workshop

This is a Sonification Workshop created to explore data with Timescaledb and
Sonic PI.

This workshop covers the basics of Timescaledb and Sonic PI and mixing both.


## Requirements for the training

- PostgreSQL
- TimescaleDB extension

Or just sign up for [Timescale Cloud](https://cloud.timescale.com).

- Sonic PI
- Ruby version 3.2.1
- Headsets are highly recommended


## Installation

Clone this repository:

```bash
git clone https://github.com/jonatas/sonification-workshop
cd sonification-workshop
```

## Preview usage

I run almost all my presentation on VIM using [presenting.vim](https://github.com/sotte/presenting.vim)
but I also build a special markdown preview to allow me to run the examples and plot the data from query results
to make it clear in some cases.

I build [md-show](https://github.com/jonatas/md-show) to help me with my presentations.
It works as a command-line tool to serve the markdown content as an HTML content
and I added some extra features to focus in presentation mode, allowing me to
use it during my talks.

You can install and run it if you have ruby available in your computer.

```bash
gem install md-show
md-show making-noise-workshop.md $PG_URI
```

Where `PG_URI` is a postgresql URI connection. Example: `postgres://jonatasdp@localhost:5432/openweather`

The HTML presentation will be served on http://localhost:4567.
You can navigate through the presentation using the left and right arrow keys, or by clicking the "Previous" and "Next" buttons.

### Features

The HTML presentation includes the following features:

- Syntax highlighting for SQL code snippets using Prism.js
- Click in the SQL code to run the snippet and display the results.
- Automatic slide deck creation based on Markdown headers (H1 and H2)
- Simple plotting of data as a scatter plot when the result contains columns named "x" and "y"

# Sessions

* [JOnTheBeach 2023](https://jonthebeach.com/workshops/Introductory-Workshop-for-Sonification-Process.-Creating-noise-with-Time-Series-data).
