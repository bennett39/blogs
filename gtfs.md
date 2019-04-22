# Parsing GTFS format transit data in real time with Python

This week I started playing around with real-time transit data from New
York City's Metropolitan Transit Authority. It's really cool, and quite
easy to get your hands on this data. However, getting started with the
GTFS data format can be challenging at first.

This is the quick setup guide for Python that I wish I had earlier this
week.

## Get yourself an API key

Before we start, you'll need to decide what GTFS transit data you want
to access.

I live in New York, so I'm working with [NYC's real-time data
feeds](https://datamine.mta.info/). Most major cities offer their data
and the GTFS format has become a standard.

Check out this awesome list of [public
APIs](https://github.com/toddmotto/public-apis#transportation) for ideas
of transportation data (or any other type of data) you can use for your
projects!

## Install Google's GTFS Python library

Google has already created a library of GTFS bindings for Python. This
took me a little while to find and start using, so I'll help you out
with some setup instructions.

To install:

```bash
pip install --upgrade gtfs-realtime-bindings
```

Now try:

```bash
python -m google.transit.gtfs_realtime_pb2
```

If you don't see an error, you've got it installed correctly!

## Install requests if you haven't already

Most Python developers will already have the requests library installed.
It allows you to make HTTP requests from within a Python program.

To install:

```bash
pip install --upgrade requests
```

To check your installation, start a Python shell:

```bash
python
```

Import requests and check your version:

```python
>>> import requests
>>> requests.__version__
'2.21.0'
```

## Building a barebones data feed

Now the fun part. Let's get some data!

Essentially, we need to do three things to start working with real-time
transit data:

1. Initialize the FeedMessage parser from Google
2. Get the response from the API
3. Pass the response to the parser

Then we can start working with or storing the data however we want.

### 1. Initialize an instance of FeedMessage

Google defines a FeedMessage class in its library. We'll add data to
this class later, but right now we just need to initialize it.

```python
feed = gtfs_realtime_pb2.FeedMessage()
```

### 2. Get the response from the API

Let's go get the data from the API now that we'll pass to the FeedParser
in step 3.

```python
response = requests.get(<URL OF YOUR GTFS SOURCE>, allow_redirects=True)
```

This is the most basic use of the requests library. We allow redirects
so the API can route our request.

### 3. Pass the response to the parser

The FeedMessage class has a ParseFromString() method to read in the
data.

```python
feed.ParseFromString(response.content)
```

The parsed data is now available in the entity attribute:

```python
>>> len(feed.entity)
373
>>> feed.entity[0]
id: "000001"
trip_update {
  trip {
    trip_id: "057150_1..N03R"
    start_date: "20190422"
    route_id: "1"
  }
  stop_time_update {
    arrival {
      time: 1555943380
    }
    stop_id: "101N"
  }
}
```

## Use trip_update

Not every entity in the feed will have a real-time update of transit
status. Destinations and departure locations might also be included.

If you want to just focus on data that is updating a currently ongoing
revenue trip, then filter for trip_update using the FeedMessage's
HasField method:

```python
for entity in feed.entity:
    if entity.HasField('trip_update'):
        # Do something
```

For example, in my data today, only 218 of my 373 entities were actually
trip updates:

```python
>>> len(feed.entity)
373
>>> sum([1 for ent in feed.entity if ent.HasField('trip_update')])
218
>>> sum([1 for ent in feed.entity if not ent.HasField('trip_update')])
155
```

## Put it all together

Want to see a barebones application to pull GTFS data in action?

[Clone my repo](https://github.com/bennett39/mta-real-time-data.git) and
follow the instructions in the README!



