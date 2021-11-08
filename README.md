
## OLCF Software Services Development Team - Coding Exercise 01

### Overview

The simplified exercise below is representative of the kind applications we build within the OLCF Software Services Development Team. When you are finished with the coding exercise, please send your submission directly to us via email as a `.tar.gz` or `.zip` file. For programmers experienced in web application design, the exercise should take approximately (2) hours to complete. For programmers with no recent experience with web application design, the exercise could take approximately (10) hours, if you account for time spent "getting up to speed".

### Deliverable

Using a Ruby or Python based web application framework (e.g. Rails, Sinatra, Django, or Flask), write a small application that provides a single RESTful API endpoint which returns a filtered set of "batch job records" from the provided dataset. We have no preference between Ruby and Python. For the sake of this exercise, think of a "batch job record" as a concise summary of some work done on a supercomputer.

Your deliverable, at minimum:

- Should provide an informative `README.md` (i.e., run/installation instructions, what could be improved, etc.)
- Should ETL the provided CSV into a database of your choosing
- Should gracefully handle omissions within the example dataset
- Should provide a functional API endpoint at `/batch_jobs`
- Your API endpoint should respond with a [JSON-API](http://jsonapi.org/format/#fetching-resources) document (string)
- Your API endpoint should correctly filter the dataset based upon any combination of filter keywords (see "Filter Keywords" section below)
- Should provide basic test(s) as you see fit

Please avoid identifying yourslf by name within the source code itself (delete that `.git` directory!) Limited use of open-source third-party gems/packages beyond your chosen framework is fine, as long as your `README` describes the build/installation process thoroughly. We will build and run your source code exactly according to your instructions.

### Specifications

Below are detailed specifications to which your deliverable should adhere.

#### Example Data

The example batch job record dataset is contained in this repo in the `example_batch_records.csv` file.

Each batch job record occupies one line of the file, and each line contains up to (3) comma-delimited attributes for the record, in this order:

  * `batch_number` (*integer*) - A number assigned to the batch job by the batch scheduler
  * `submitted_at` (*ISO 8601 datetime*) - The time the batch job was submitted
  * `nodes_used` (*integer*) - The number of nodes the batch job used

The data are well-formatted, but some attribute values could be missing entirely.

#### API Endpoint:

`GET http://localhost/batch_jobs`

#### API Endpoint Filtering

`GET http://localhost/batch_jobs?filter[submitted_after]='2018-02-28T15:00:00+00:00'&filter[submitted_before]='2018-03-01T15:00:00+00:00'&filter[min_nodes]=2&filter[max_nodes]=20`

#### API Response

The expected response is a [JSON-API](http://jsonapi.org/format/#fetching-resources) document (string) representing an array of objects, one object per record. For example, parsing the API response string should yield something like:

```
{
  "links": {
    "self": "http://localhost/batch_jobs?filter[max_nodes]=20"
  },
  "data": [
    { "type": "batch_jobs",
      "id": "1",
      "attributes": {
        "batch_number": 945,
        "submitted_at": "2018-03-30T15:00:00+00:00",
        "nodes_used": 8
      }
    }, {
      "type": "batch_jobs",
      "id": "2",
      "attributes": {
        "batch_number": 946,
        "submitted_at": "2018-03-30T15:07:01+00:00",
        "nodes_used": 20
      }
    }, ...
  ]
}
```

All filter parameters are optional. All filters should be inclusive (e.g. `filter[min_nodes]=2&filter[max_nodes]=4` should return records that used 2, 3, or 4 nodes).

#### Filter Keywords

  * `[submitted_after]=` (*ISO 8601 datetime*) - return only records submitted on or after the given date
  * `[submitted_before]=` (*ISO 8601 datetime*) - return only records submitted on or before the given date
  * `[min_nodes]=` (*integer*) - return only records that used at least the given number of nodes
  * `[max_nodes]=` (*integer*) - return only records that used at most the given number of nodes





