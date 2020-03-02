# Quick runner/wrapper for scholars-discovery project

Instructions:

## Import git repository into the base folder of this repository

1) git clone https://github.com/vivo-community/scholars-discovery.git 

NOTE: everything assumes the scholars-discovery code is checked out into the 
`scholars-discovery` directory. It is in the `.gitignore` file.

This will run off the `scholars-discovery` master branch.  Right now that might
not have a grapqhl endpoint - for the time being it may be best to switch to
the `vstf-staging` branch (e.g. `git checkout ...`).  The idea for this
project is you should be able to use it to run *any* branch in development mode.

##  copy TDB data into data-imported directory

Put TDB data in the data-imported/ directory.  Then `cd ..` (back up to scholars-discovery-setup directory)

2) `cd ..`
3) `docker-compose up`

### or (if already imported data into TDB)

3) `docker-compose up`

This is making us of a file `application-dev.yml` to configure `scholars-discovery`
for local development mode.  It also defaults to importing data into the index 
at startup.

However, you can change `middleware.index.onStartup=false` **after the first start up** 
has finished and imported into the index succesfully - so that it doesn't rebuild the
index every time.

```yaml
# file: application-dev.yml

...
middleware:
  index:
    onStartup: true # change to false after first run
...

```

## Sample Query

Once you have running, can go this http://localhost:9000/gui and run GraphQL queries.
Here is a sample one:

```graphql
query {
  personsFacetedSearch(
    facets: [{field: "keywords"}],
    filters: [{field: "preferredTitle", value:"Software Developer"}]
    paging: { pageSize:100, pageNumber: 0},
    query: "*",
  ) {
    content {
      id
      name
      keywords
      preferredTitle
      positions {
        organizations {
          label
        }
      }
    }
    page {
      totalPages
      number
      size
      totalElements
    }
    facets {
      field
      entries {
        content { 
          value
          count 
        }
      }
    }
  }
}
```

