# Dev Tools Console:
          GET /_cluster/health
          
          GET /_cat/indices?v
          
          PUT /pages
          
          GET /_cat/shards?v
          
          GET /pages
          
          GET /_cat/nodes?v
          
          DELETE /pages

# create indic including no. of shards and replicas

          PUT /products
          {
            "settings": {
              "number_of_shards": 2,
              "number_of_replicas": 2
            }
          }

# insert new doc in existing indic

          POST /products/_doc
          {
            "name": "poco f1",
            "price": "22k",
            "in_stock": 100
          }

# Create new doc mentioning doc-id

          PUT /products/_doc/100
          {
            "name": "poco M3",
            "price": "12k",
            "in_stock": 1000
          }

# GET via doc-id

          GET /products/_doc/100

# All doc in indic

          GET /products/_search

# update doc fileds value

          POST /products/_update/100
          {
            "doc": {
              "price": "10.5k",
              "in_stock": 99
            }
          }


# add fields

          POST /products/_update/100
          {
            "doc": {
              "tags": ["electronics"]
            }
          }

# Example-1 : Modify source based on script, "ctx" is a variable that refers to as context, then we set to "_source" then "in_stock" and decrement the value by 1

          POST /products/_update/100
          {
            "script": {
              "source": "ctx._source.in_stock--"
            }
          }


## Example-2 :

          POST /products/_update/100
          {
            "script": {
              "source": "ctx._source.in_stock = 10"
            }
          }

## Example-3 : Modify source based on script, "ctx" is a variable that refers to context, then we set to "_source" then "in_stock" and subtract the in_stock by "quantity" defiened in "param" and set the value as the new "in_stock"

          POST /products/_update/100
          {
            "script": {
              "source": "ctx._source.in_stock -= params.quantity",
              "params": {
                "quantity": 4
              }
            }
          }

          POST /products/_update/100
          {
            "script": {
              "source": "ctx._source.in_stock -= params.quantity",
              "params": {
                "quantity": 1
              }
            }
          }


# GET via doc-id

          GET /products/_doc/100

# Using condition in "script"

          POST /products/_update/100
          {
            "script": {
              "source": """
                if (ctx._source.in_stock == 0) {
                  ctx.op = 'noop';
                }
                
              ctx._source.in_stock --;
              
              """
            }
          }


#  UPSERT
##  - If the doc already exixts it does as instructed in "script"
##  - If the doc doesn't exist, it will create new as mentioned in "upsert" 
  

          POST /products/_update/101
          {
            "script": {
              "source": "ctx._source.in_stock++"
            },
            "upsert": {
              "name": "NOTHING CMF PHONE 1",
              "price": "16k",
              "in_stock": 5
            }
          }

          GET /products/_doc/100

# Replace Documents

        PUT /products/_doc/100
        {
          "name": "TOSTER",
          "price": "2k",
          "in_stock": 50
        }
# DELETE a document

          DELETE /products/_doc/101
          
          GET /products/_doc/101
