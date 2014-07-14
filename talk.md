# Data usability

## Introduction

Good morning. Thank you all for being here, and thank you to Max, Rufus, and all
the other organisers for putting CSVconf together.

Today I'm going to be talking to you about shipping containers and data
packages. And I think I typoed my subtitle, because that's meant to read "An
experiment in how far you can stretch an analogy before the audience start
throwing tomatoes."

Let's get started.

[SLIDE]

Once upon a time, global trade looked like this. Bags and sacks and bushels and
barrels, all carried from ship to shore and back by dock workers.

[SLIDE]

These dock workers were known variously as stevedores or longshoremen. Their jobs
were dangerous, and required them to have unusual strength and stamina. Loading
and unloading ships was a slow process.

[SLIDE]

Today, it looks more like this. Starting in the mid-50s, standardised shipping
containers, known today as ISO containers, made it possible to move goods
several orders of magnitude faster, and several orders of magnitude more safely,
than the longshoremen did.

[SLIDE]

Goods that can't be shipped in containers or in bulk like oil or grain are known
as "break-bulk cargo". Shipping break-bulk cargo is much slower, and vastly more
expensive than shipping containerised cargo.

## Data

[SLIDE]

We are currently in about 1956 or 1957 in the world of data. The first few
container ships might have sailed, but we are still firmly in the era of
"break-bulk" data. Nearly every dataset released by governments, companies, and
civil society organisations alike is released in its own special format, or its
own special variant of a standard format. Special formats preclude the use of
automated tools, and require that humans are involved in writing data cleansing
tools, custom loaders, and so on, before the data can be analysed.

Even among smart, technically aware publishers--the kinds of people who know
what "CSV" stands for and know what "ragged rows" are--the formats in which data
are published still look a bit like bags and cartons and crates.

So why is this a problem? Well, back in the physical world of "break-bulk"
cargo, a large fraction of the shipping costs are associated with the points
where the cargo is handled -- getting the sacks and crates and barrels on and
off the boat. For a 10,000 mile journey, up to 50% of the costs may be
associated with the two 10 mile trips through origin and destination ports.

In the world of data, a large fraction of our time is spent getting data into a
format where we can use automated (or semi-automated) analysis tools on it. Data
retrieval and cleaning are the most boring parts of the job, and they often take
almost as long as the real analysis.

And, just as in the era of longshoremen, handling the data usually requires
people with special training.

Let's look at the impact that containerisation had on global shipping, in a
format that everyone here will understand.

[SLIDE]

- immediately
- one of the very first container ship sailings, made by the SS Ideal X in 1956,
  had a loading cost of 15.8 cents per ton versus $5.86 per ton for a medium
  sized break-bulk ship [fn1]
- factor of 40 productivity gain

- 1959 vs 1976
- Average time in port: 3 weeks -> 18 hours [fn2]
- Labour productivity: 0.627 tons per man hour -> 4234 tons per man hour [fn2]
- factor of 7000 productivity gain


A three order-of-magnitude productivity gain is a big deal.

[SLIDE]

So, I think packaging formats are like pensions and government websites: boring
but really important.

## What made containers a success?

We can ask why the shipping container was such an enormous success, and for my
part, I think it comes down to three important factors:

[SLIDE]

1. Simple
2. Opaque
3. Moderately diverse

First, Containers are simple. They are steel boxes with standardised dimensions
and holes of a specified size at the corners. That's it. Any engineering firm in
the world can make an ISO-compatible container.

[BUILD]

Containers are opaque. Most of the tools that handle them are entirely ignorant
of what they contain. Manufacturers of container cranes do not need to know
anything about cars, bananas, or laptop computers.

[BUILD]

And last, not all containers are identical. While the important features of the
containers remain the same, the ISO standards recognise that a single container
will not suit all circumstances. Externally, containers come in several
different sizes. Internally, there are a number of sub-standards for shipping
different kinds of goods: liquids, dry goods, Euro-pallets, and so on.

So, if we want to make a successful data package format, starting with these
properties as design goals probably isn't a bad idea.

[SLIDE]

## Introducing the Data Package

History lesson over for a moment, I'm going to tell you about a container for
data that aims to do for data processing what the ISO container did for marine
shipping. It's called the data package.

[SLIDE]

This is a minimal data package:

    datapackage.json
    {
      "name": "gold-prices"
    }

It's a file, called "datapackage.json". The contents of the file are in a format
called JSON, which I imagine you all know. The JSON objet in the file has only
one required field: "name".

It's functionally useless, sure.

[SLIDE]

But so is an empty shipping container. It's just a (standardised) bucket that we
can put stuff in. Even giving it a standard name and format buys us something,
just like the standard dimensions of the ISO container.

[SLIDE]

Here's a more likely data package:

    datapackage.json
    {
      "name": "gold-prices",
      "title": "Gold Prices (Monthly in USD)",
      "license": "odc-pddl",
      "sources": [{
        "name": "Bundesbank statistics",
        "web": "http://www.bundesbank.de/Navigation/[...]"
      }],
      "resources": [{
        "path": "data.csv",
        "format": "csv",
        "schema": {
          "fields": [
            {"type": "date", "name": "date"},
            {"type": "number", "name": "price"}
          ]
        }
      }],
      "version": "1.0.42"
    }

We can see here that this data package has, not just a name, but a title, a
version, and refers to some data: the "resources" field contains here reference
to a file, "data.csv".

[SLIDE]

Simplifying slightly, we can see the following fields in the data package.

    datapackage.json
    {
      "name": "gold-prices",                      # Identifier ([a-z0-9._-]+)
      "title": "Gold Prices (Monthly in USD)",    # One-sentence description
      "license": "odc-pddl",                      # Open License identifier
      "sources": [...],                           # Array of Source objects (defined)
      "resources": [...],                         # Array of Resource objects (partially defined)
      "version": "1.0.42"                         # Semver-compatible version
    }

[BUILD]

... and alongside this JSON file, we have, in this case, a single CSV file
containing our data.

[SLIDE]

In particular, what I've shown you here is an example of a Tabular Data Package.
A Tabular Data Package is a Data Package with specific properties. In that way,
it's a bit like...

[SLIDE]

...one of these bad boys. Who can tell me what this is?

You really need to brush up on your container-spotting skills.

This is the Rolls Royce of shipping containers. A 45 foot high-cube palletwide.
It's a shipping container that's sized to accept two regular pallets side by
side. It's a standard container, but with some additional constraints.

[SLIDE]

And Tabular Data Packages are the Rolls Royce of the data package world. Tabular
Data Packages must have at least one data file (or "resource"), which must be in
CSV format. The data package must contain a "schema" field on the resource,
describing the content of the CSV.

The schema field is further specified in a companion specification to the Data
Package, known as JSON Table Schema.

And that's it. There are more pieces to the Data Package family, and I'll tell
you where to find out about them in a moment.

## Data Package tools

But what's the point? Well, just as with containerised shipping, the point is to
make it possible to build standardised handling equipment that makes dealing
with data easier. Some great people have already started building:

[SLIDE]

- Low-level handling tools (cranes)
  + Data Package Manager [https://github.com/okfn/dpm]
  + Python library [https://github.com/tryggvib/datapackage]
  + R library [https://github.com/QBRC/RODProt]
  + Ruby library [https://github.com/theodi/datapackage.rb]

- High-level authoring and management tools
  + Data Packager [http://datapackager.okfn.org]
  + CSVLint.io [http://csvlint.io/]
  + Git Data Package viewer [http://git-viewer.labs.theodi.org/]
  + Datacentral, the "poor man's CKAN" [https://github.com/centraldedados/datacentral]

Thank you to all those of you who've already picked up the data package standard
and run with it.

## Wrapup

Ok, so by now I'm sure I've convinced you all that data packages are the bees
knees, and you're all wondering what you need to do? Well, the first thing to
say is that all the standards I've described are community authored, and are
available at

[SLIDE]

   dataprotocols.org

Please go read the standards, edit the standards, improve the standards.

[SLIDE]

You can also find dozens of example datasets published as data packages at:

    https://github.com/datasets/

Already, there is data on:

    country-codes: ISO-3166 country codes, ITU dialing codes, ISO 4217 currency
                   codes
    geo-boundaries-us-110m: First-order administrative boundaries for the US in
                            .shp, .geojson, and .topojson formats
    browser-stats: Browser usage statistics
    gold-prices: Monthly gold prices since 1950
    gdp: Country, Regional, and World GDP since the 1960s

[SLIDE]

And, most importantly of all: PUT IT IN A BOX. Release your own data as data
packages for others to use.

Standardised packaging formats have the ability to drastically simplify the
process of discovering, retrieving, and managing data. So please help us leave
the era of "break-bulk data".

I will leave you with just one last thought: not all shipping is containerised,
and there are situations in which data packages will not be not the right answer
to your problem.

[SLIDE]

    not-for-everyone.png

Thank you for listening.

[SLIDE]
