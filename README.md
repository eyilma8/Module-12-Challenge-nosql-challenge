# Module-12-Challenge-nosql-challenge
No SQL challenge


# Display the number of documents in the customers collection
customers.count_documents({})

# Create a query that finds the customers who have a Toyota
query = {'car_make': "Toyota"}

# Print the number of results
print("Number of documents in result:", customers.count_documents(query))

# Print just the first result from the query
print("First result:")
results = mechanics.find(query)
pprint(results[0])

# Change the data type from String to Double for wages.hourly_rate
mechanics.update_many({}, 
                      [ 
                        {'$set':{ "wages.hourly_rate" : {'$toDouble': "$wages.hourly_rate"}}} 
                      ]
                    )
# Select only the mechanic_name and wages.hourly_rate fields from the mechanics collection
query = {}
fields = {'mechanic_name': 1, 'wages.hourly_rate': 1}

# Capture the results to a variable
results = mechanics.find(query, fields)

# Pretty print the results
for result in results:
    pprint(result)

# Select every field from the mechanics collection except the car_specialties field
query = {}
fields = {'car_specialties': 0}

# Capture the results to a variable
results = mechanics.find(query, fields)

# Pretty print the first two results
for i in range(2):
    pprint(results[i])


ohio_daily_records.update_many({}, [ {'$set': { "CO.PERCENT_COMPLETE" : {'$toDouble': "$CO.PERCENT_COMPLETE"},
                                                "CO.DAILY_AQI_VALUE" : {'$toInt': "$CO.DAILY_AQI_VALUE"}
                                              } 
                                     } ]
                              )

# Find how many documents have a height greater than or equal to 40cm
height_gte_40_documents = artifacts.count_documents({'measurements.elementMeasurements.Height': {'$gte': 40}})
height_gte_40_documents

# Write an aggregation query that counts the number of documents, grouped by "country"
query = [{'$group': {'_id': "$country", 'count': { '$sum': 1 }}}]


# Print the first 10 results
pprint(results[0:10])


# Convert mongo result to Pandas DataFrame
result_df = pd.DataFrame(results)

print("Rows in DataFrame: ", len(result_df))
result_df.head(10)



# have a classification where "Wood" is the value.
match_query = {'$match': {'classification': {'$regex': "Wood"}}}

# Write an aggregation query that counts the number of documents and finds the maximum height,
# grouped by "classification"
group_query = {'$group': {'_id': "$classification", 
                          'count': { '$sum': 1 },
                          'max_height': { '$max': '$measurements.elementMeasurements.Height' }}}

# Create a dictionary that will allow the pipeline to sort by count in descending order, 
# then sort by classification in alphabetical order
sort_values = {'$sort': { 'count': -1, '_id': 1 }}

# Put the pipeline together
pipeline = [match_query, group_query, sort_values]




























