# Sonification Intro

**Creating noise with Time Series data**

### Sonic PI & TimescaleDB

##### Jônatas Davi Paganini - **jonatas@timescale.com**

# **Jônatas Davi Paganini**

**Developer Advocate @ Timescale**

* Ruby since 2007.
* Postgresql since 2004.

```ruby
{ twitter: '@jonatasdp', github: '@jonatas' }
```

# Agenda

```ruby
plan = {
  "Why? My story with Sonification": 10.minutes,
  "The TimescaleDB & The Open Weather dataset": 20.minutes,
  "Introduction to Sonic PI": 1.hour,
  "Piscale - My exploratory project": 1.hour,
  "Exploration": 0.5.hour
}
```


# What is Sonification?

- Sonification: Turning data into sound
- Allows perception of data through hearing

# Why Sonification?

- Alternate way to understand data
- Enhances perception of data patterns
- Useful where visual representation is not practical

# Examples of Sonification

- Geiger counters: Sound indicates radiation levels
- Heart rate monitors: Sound indicates heart rate
- Sonic Pi: Music synthesizer used for sonifying data

```ruby
# Sonic Pi sonification example
data = [60, 62, 64, 65, 67, 69, 71, 72]
data.each do |d|
  play d
  sleep 0.5
end
```

# My past sonification ideas:

* https://github.com/jonatas/dj-trader - Listen to market data
* https://github.com/jonatas/mandala - Listen to your painted mandala

Now I work at Timescaledb, so why not make noise from any data?

* https://github.com/jonatas/piscale - Combine Sonic PI with Timescaledb to make noise from any database query

# What inspires me

* Car engines are noisy.
* You know the "normal" noise
* You can detect something when it breaks (Flat tires, mal-functioning)
* Why not make our or company KPIs or convert our Dashboards to sound system?
* Blind people can use it!

# Challenges in Sonification

- Making sound meaningful
- Avoiding sensory overload

# The Future of Sonification

- Potential applications in data analysis, accessibility, education, and arts
- Active area of research and developmen

# Data: The New Gold

- Valuable and versatile
- Drives innovation and decision-making

# Recyclable and Repurposable

- Multiple uses from a single dataset
- Combining data sources for new insights

# Constantly Evolving

- Growing and changing over time
- Opportunities for continuous enrichment

# A Lasting Legacy

- Enduring relevance
- Foundation for future discoveries

# Why music?

> Music gives a soul to the universe,
> wings to the mind,
> flight to the imagination,
> and life to everything." - Plato

# Making Data Accessible

- Bridging the gap for visually impaired individuals
- Enabling new ways to explore and understand data

# Enhancing Data Comprehension

- Conveying information through auditory channels
- Leveraging the power of sound to reveal patterns and trends

# Encouraging Inclusivity

- Expanding data literacy across diverse audiences
- Promoting universal design principles in data presentation

# Stimulating Creativity

- Inspiring innovative approaches to data analysis
- Fostering interdisciplinary collaborations between data and music

# Structure - Workshop overview

Instructor and participant introductions.

# Raise your hand 🙋

* Are you familiar with time series data?
* Are you familiar with music design?
* Do you know Ruby?
* Have you tried Sonic PI?

# Micro crash course about Ruby

* Primitive types (String, Symbol, Numbers, Iterators, Array, Sets, Maps)
* Loops
* Blocks
* Methods

# Interface

Interact with open weather dataset via psql:

```bash
psql open-weather
```

> you can use your favorite tool if you want ;)

Use `createdb open-weather` in case you don't have it yet.

# Extension

If you don't have timescaledb installed on your database, enable the extension:

```sql
CREATE EXTENSION timescaledb;
```

You can skip this step if you're using Timescale Cloud or Timescale docker
images.

# Download

              █████████████████████████████████████
              █████████████████████████████████████
              ████ ▄▄▄▄▄ █▀█▀▄█▀▄▀ █ ▄▄█ ▄▄▄▄▄ ████
              ████ █   █ █▀▄▀▀ ▀ ▄▄▀█ ▄█ █   █ ████
              ████ █▄▄▄█ █▀▄▀▄▀▀█▀▀██▄ █ █▄▄▄█ ████
              ████▄▄▄▄▄▄▄█▄▀ ▀▄█ ▀▄█ ▀ █▄▄▄▄▄▄▄████
              ████▄ ▄▄ ▀▄  ▄█ ▄ ▄▄▀▄▀ ▀▄▀ ▀▄█▄▀████
              ████   ▄▄▄▄▄ █▄█▀█▀ ▀█▄▀████▄▀█▀█████
              ████▄▀▄▀▄▄▄▄▀▄▄ ▄▄▄▄ ▄█ ▀▀▀▀▀▄▄█▀████
              ████▄▄▀  ▄▄ ▄▀▄█▀█  ▄▄▄▀  ▀▀ ▄▄▀█████
              ████  ▄▄█ ▄▀▄█ ▀▄ ▄▄▀   █ ▀ ▀▄ █▀████
              ████ ██▀▄▀▄ ██▄▀ ▄▄ ▀▄██ ▀ ▄▄█▄▀█████
              ████▄█▄██▄▄▄▀  ▀█▄▄▄▀▄█▄ ▄▄▄ ▀   ████
              ████ ▄▄▄▄▄ █▄█ █▄▄▀  ▄▄  █▄█ ▄▄▀█████
              ████ █   █ █ ▄█▀█▄▄██▄█▀ ▄▄▄▄▀ █▀████
              ████ █▄▄▄█ █ █████  ▀█▄▄  ▄ ▄ ▄ █████
              ████▄▄▄▄▄▄▄█▄██▄▄▄███▄█▄██▄▄▄█▄██████
              █████████████████████████████████████
              █████████████████████████████████████

https://github.com/jonatas/sql-data-science-training

# Setup schema

Use the `schema.sql` file  for the next few steps if you want. Our steps will
be:

1. Create table
2. Create indices
3. Transform the table into hypertable

# Table

```sql
CREATE TABLE public.weather_metrics (
    "time" timestamp without time zone NOT NULL,
    timezone_shift integer,
    city_name text,
    temp_c double precision,
    humidity_percent double precision,
    wind_speed_ms double precision,
    -- rain, snow and more ...
);
```

# Hypertable

```sql
SELECT create_hypertable('weather_metrics', 'time',
  chunk_time_interval => INTERVAL '1 month');
```

# Import

We'll use the CSVs in the `data` folder from

**https://github.com/jonatas/sql-data-science-training**.

```sql
\i schema.sql
\COPY weather_metrics FROM './data/nairobi.csv' DELIMITER ',' CSV HEADER;
\COPY weather_metrics FROM './data/new_york.csv' DELIMITER ',' CSV HEADER;
\COPY weather_metrics FROM './data/toronto.csv' DELIMITER ',' CSV HEADER;
\COPY weather_metrics FROM './data/stockholm.csv' DELIMITER ',' CSV HEADER;
\COPY weather_metrics FROM './data/princeton.csv' DELIMITER ',' CSV HEADER;
\COPY weather_metrics FROM './data/vienna.csv' DELIMITER ',' CSV HEADER;
```
> copy commands should be executed line by line

# When?

When you need to...

* Ingest intensive amount of events - AKA Time Series data
* Improve query performance
* Improve ingestion rates
* Continuous aggregate your data for dashboards and reports
* You need to move old data to cheaper storages but still keep it accessible.

# Why?

* To optimize time-series/data insensive applications
* To save infrastructure resources 🤑
      
# How?

* Using the chunks to abstract table partitions.
  - Faster queries - more paralelization on the chunks
* Using compression to save storage.
  - 96% of storage savings after adopt the compression feature
  - More data with the same machine configuration
* Hooking query plans to parallelize and manage smaller amounts of data.
  - Less CPU cycles - optimize CPU resources

# Psql

Let's dive into the hypertable definition:

```sql
\d+ weather_metrics
```

# Partition

Hypertable will partition date by time interval.

Let's run EXPLAIN ANALYZE in the previous query:

```sql
EXPLAIN ANALYZE
SELECT avg(temp_c)
FROM weather_metrics
WHERE city_name = 'New York'
AND time BETWEEN '2022-01-01' AND '2022-01-31';
```

# Introduction to Sonic Pi

> Experience the sound of code.

[sonic-pi.net](https://sonic-pi.net)

# Play a Note

Play a note using the `play` function. By default, it uses a beep synth.

```ruby
play :C4
sleep 1
```

# Different Synthesizers

Choose different synths using the `use_synth` function.

```ruby
use_synth :piano
play :C4
sleep 1

use_synth :fm
play :C4
sleep 1
```

# Loops

Create loops using the `loop` function.

```ruby
loop do
  play :C4
  sleep 0.5
  play :E4
  sleep 0.5
end
```

# Threads

Run multiple loops simultaneously using `in_thread`.

```ruby
in_thread do
  loop do
    play :C4
    sleep 1
  end
end

in_thread do
  loop do
    play :E4
    sleep 1
  end
end
```


# Piscale

https://github.com/jonatas/piscale

project and its implementation
  - Connecting to a database
  - Creating and using hypertables
  - Querying Time Series data


# Introduction to Sonic PI (45 minutes)

- What is Sonic PI?
- Setting up Sonic PI

# Basic Sonic PI concepts

# Synths and samples

Sonic Pi provides a variety of synthesizers (synths)
and samples to create different sounds. Use `use_synth` to select a synth and `play` to play notes. Use `sample` to play samples.

```ruby
use_synth :piano
play :C4
sleep 1

sample :loop_amen
sleep 2
```

# Timing and synchronization

In Sonic Pi, timing is controlled using sleep and sync.
Sleep adds a delay before the next command,
while sync waits for a cue before continuing.

```ruby
in_thread do
  loop do
    play :C4
    sleep 1
    play :E4
    sleep 1
  end
end

in_thread do
  loop do
    sample :bd_haus
    sleep 0.5
  end
end
```

# Effects and transformations

Sonic Pi offers a range of effects and transformations.
Use with_fx to apply an effect to the code inside the block.

```ruby
with_fx :reverb, room: 0.8 do
  use_synth :piano
  play_pattern_timed [:C4, :D4, :E4], [0.5, 0.5, 0.5]
end
```

> room will make the reverb expand or contract

# Example 1: Simple melody

A simple melody using the piano synth and sleep for timing.

```ruby
use_synth :piano
play_pattern_timed [:C4, :D4, :E4, :F4, :G4, :A4, :B4, :C5], [0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5]
```

# Twinkle Twinkle little star

```ruby
use_synth :piano
use_bpm 110

play_pattern_timed [:C4, :C4, :G4, :G4, :A4, :A4, :G4], [1, 1, 1, 1, 1, 1, 2]
play_pattern_timed [:F4, :F4, :E4, :E4, :D4, :D4, :C4], [1, 1, 1, 1, 1, 1, 2]
play_pattern_timed [:G4, :G4, :F4, :F4, :E4, :E4, :D4], [1, 1, 1, 1, 1, 1, 2]
play_pattern_timed [:G4, :G4, :F4, :F4, :E4, :E4, :D4], [1, 1, 1, 1, 1, 1, 2]
play_pattern_timed [:C4, :C4, :G4, :G4, :A4, :A4, :G4], [1, 1, 1, 1, 1, 1, 2]
play_pattern_timed [:F4, :F4, :E4, :E4, :D4, :D4, :C4], [1, 1, 1, 1, 1, 1, 2]
```

# Combining TimescaleDB and Sonic PI

1. Intro to [Piscale](https://github.com/jonatas/piscale)
2. Setup Sonic PI to accept connections
3. Send commands from client to Sonic PI server
4. Generating sound from Time Series data
  - lttb_demo: Playing temperatures from New York
  - candlestick_demo: Dark ambience based on candlestick data
  - candle_beat: Creating beats with yearly candlestick data
  - candle_chord: Playing chords based on monthly candlestick data

> Be creative and develop your own ideas

# Piscale

A wrapper to send commands to Sonic Pi

```ruby
class Piscale
  # Demo methods goes here...
  def run msg
    puts msg
    server.run msg
  end

  def server
    @server ||= SonicPi.new 4560
  end
end
```

# Sonic Pi setup

Enable the `Cues` window and watch the logs.

Adapt the **port** as necessary after the first run.

```ruby
live_loop :tsdb do
  msg = sync("/osc:127.0.0.1:58113/run-code").last
  eval(msg)
end
```

Also in the "Audio" menu

* Enable external synths
* Enable Audio inputs

# Setup Piscale locally

```bash
git clone https://github.com/jonatas/piscale
cd piscale
rbenv local 3.2.1
bundle install
```

Create a `.envrc` file with your Postgresql URI configuration:

```bash
export PG_URI=postgres://REPLACE-WITH-YOUR-USER@localhost:5432/REPLACE-DB-NAME
```

Then allow it:

```bash
direnv allow .envrc
```

# Test a simple command

Use `bin/console` to load the environment:

```bash
bin/console
```

Now, send a Sonic PI instruction using "run":

```ruby
run "play :c4"
```

# Timescaledb gem

Now, we're going to also use Timescaledb gem.

https://github.com/jonatas/timescaledb

* Ruby wrapper for the timescaledb extension
* Implements several toolkit functions
* Allow us to build queries directly from Ruby

# WeatherMetric model

Latest record:

```ruby
WeatherMetric.order(time: :desc).first
```

Getting all distinct city names:

```ruby
WeatherMetric.select("distinct city_name")
```

# Counting NY records:

```ruby
WeatherMetric.where(city_name: "New York").count
```

# Min/Max data time range

```ruby
WeatherMetric
  .select("min(time), max(time)")
  .find_by(city_name: "New York")
  .attributes
```

# Data-driven

* Let's dive into some effects
* Reverb

```ruby
use_synth :piano
play 64
```

# NY is getting warmer?

```ruby
WeatherMetric
  .select("time_bucket('1 year', time), avg(temp_c) as temperature")
  .where(city_name: "New York")
  .where("EXTRACT(MONTH FROM time) IN (6, 7, 8)")
  .where("EXTRACT(HOUR FROM (time + timezone_shift::text::interval)) BETWEEN 12 AND 16")
  .group(1)
  .each do |row|
    run "#{row.time_bucket.year};
    play 44 + #{row.temperature}"
    sleep 0.3
  end
```

# Play with temperature

```ruby
play_temperature city_name: "Vienna", base: 60, temperature: "min"
play_temperature city_name: "Nairobi", base: 44, temperature: "max"
```

# apply reverb

```ruby
use_synth :piano
with_fx :reverb, room: 3 do
  play 64
end
```

# Reverb with humidity

Adjusting reverb based on humidity and wind speed

```ruby
reverb_with_humidity(city_name: "New York", bpm: :humidity)
reverb_with_humidity(city_name: "New York", bpm: :wind)
```

# Candle chord

Play chords based on Open / High / Low / Close values from candlestick.

```ruby
def candle_chord
  candles = ny.candlestick(timeframe: '1 month', volume: "1")
  previous = candles.first
  candles.each do |candle|
    o,h,l,c = candle["open"], candle.high, candle.low, candle.close
    base = :C4
    chords = [o,h,l,c].map {|temp| "#{base.inspect} + #{temp}" }
    run "play_chord [#{chords.join(', ')}]"
    sleep 0.7

    previous = candle
  end
end
```

# Candlestick beat

```ruby
def candle_beat
  candles = ny.candlestick(timeframe: '1y', volume: "1")
  previous = candles.first
  candles.each do |candle|
    beat_time = (candle.high_time - candle.open_time) / 1.year
      puts candle.high_time
      o,h,l,c = candle["open"], candle.high, candle.low, candle.close
      [o,h,l,c].each do |amp|
      run "sample :ambi_drone, beat_stretch: #{beat_time}, amp: #{amp}"
      sleep beat_time * 0.95
    end
    previous = candle
  end
end
```

# That's all folks!

# Try it yourself

* Create noise from your own ideas
* Share your noise with others


# Dream about it

![Moog Synth](https://upload.wikimedia.org/wikipedia/commons/e/e6/Moog_Modular_55_img2.jpg)

By .The original uploader was Kimi95 at Italian Wikipedia. - http://www.vintagesynth.com/moog/m55_5whole.jpg e, CC BY 3.0, https://commons.wikimedia.org/w/index.php?curid=7745469

# Imagine a world with real synthesizer hardware integrated with data analysis.

# Enrich the lives of blind individuals by enabling them to experience data through sound.

# Democratize data analysis

Make it accessible to people with visual impairments.

# Extra Resources


        █████████████████████████████████████
        █████████████████████████████████████
        ████ ▄▄▄▄▄ █▀▄█▀ ▄▄ ▀▀▄▄██ ▄▄▄▄▄ ████
        ████ █   █ █▄   ▄█ ▀▀▄▀▄▄█ █   █ ████
        ████ █▄▄▄█ █ ▀█▀██▄  █▀▀██ █▄▄▄█ ████
        ████▄▄▄▄▄▄▄█ ▀▄█ █ ▀ ▀▄▀ █▄▄▄▄▄▄▄████
        ████▄ █ ▀▀▄ ▀█ ▀▄▄▄█ ▀█▄▀█ ▄▄▀▄▄▀████
        ████  ▀▄█▀▄▄▀▄▄█▄▀▀█▄█ ▄ ▄██ ▄  █████
        █████▀▀▀ █▄▄▀█▀█▀ ▄█▄ ▀   ▄██▄█▄▄████
        ████  █▄ ▀▄ ▀  ▄▀▀ ▀█▀ █ ▀ ▄▄ ▄ ▄████
        ██████ ▄█▄▄▀█ ▄  ▀█▄▀ ▄█ ▀▀ █▀█▄▀████
        ████▄█▀▀  ▄ ▀ ▀▀▄▄▀█▄ ▀█▀ █  ██  ████
        ████▄█▄▄▄▄▄▄  ▄█ ▄▄█ ▀▀  ▄▄▄ █ ▀▀████
        ████ ▄▄▄▄▄ █▄▀█▀█▀█▀▄▄ █ █▄█ ▀▀ █████
        ████ █   █ █▀  █▀▄▀▄▄▄█▀ ▄   ▀▀█▄████
        ████ █▄▄▄█ █▀▀▀▄█ ██ ▀▀  ▀██ ▄▄▀▄████
        ████▄▄▄▄▄▄▄█▄▄▄▄▄██▄█▄██▄▄▄▄▄▄█▄█████
        █████████████████████████████████████
        █████████████████████████████████████

- https://github.com/jonatas/timescaledb
- https://github.com/jonatas/piscale
- https://github.com/jonatas/sonification-workshop

Join the Timescale Community: https://timescale.com/community

# Thanks

- [@jonatasdp](https://twitter.com/jonatasdp) on {Twitter,Instagram,Linkedin}
- Github: [@jonatas](https://github.com/jonatas)


#### Jônatas Davi Paganini
