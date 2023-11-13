# Flask-tutorials

Q1 Send a custom HTTP code back with a tuple.
You will reuse the server.py file you worked on in the last part. 
Create a new method named no_content with the @app.route decorator and URL of /no_content. 
The method does not have any arguments. Return a tuple with the JSON message No content found.
Test:  curl -X GET -i -w '\n' localhost:5000/no_content

Q2 Send custom HTTP code back with the make_response() method.
Create a second method named index_explicit with the @app.route decorator and a URL of /exp. 
The method does not have any arguments.
Use the make_response() method to create a new response.
Set the status to 200.
Test: curl -X GET -i -w '\n' localhost:5000/exp




Q3 Create a method called name_search with the @app.route decorator. 
This method should be called when a client requests for the /name_search URL.
The method will not accept any parameter, however, will look for the argument q in the incoming request URL.
This argument holds the first_name the client is looking for. The method returns:

Person information with a status of HTTP 400 if the first_name is found in the data
Message of Invalid input parameter with a status of HTTP 422 if the argument q is missing from the request
Message of Person not found with a status code of HTTP 404 if the person is not found in the data
Test: curl -X GET -i -w '\n' "localhost:5000/name_search?q=Abdel"

Next, test that the method returns HTTP 422 if the argument q is missing:
Test: curl -X GET -i -w '\n' "localhost:5000/name_search"

Finally, let's test the case where the first_name is not present in our list of people:
Test: curl -X GET -i -w '\n' "localhost:5000/name_search?q=qwerty"


You are asked to implement both of these endpoints in this exercise.
You will also implement a count method that returns the total number of persons in the data list.
This will help confirm that the two methods GET and DELETE work, as required.




Q4 Create /count endpoint.

Add the @app.get() decorator for the /count URL.
Define the count function that simply returns the number of items in the data list.
Test: curl -X GET -i -w '\n' "localhost:5000/count"




Implement the GET endpoint to ask for a person by id.

Q5 Create a new endpoint for http://localhost/person/unique_identifier.
The method should be named find_by_uuid. 
It should take an argument of type UUID and return the person JSON if found.
If the person is not found, the method should return a 404 with a message of person not found.
Finally, the client (curl) should only be able to call this method by passing a valid UUID type id.
Test: curl -X GET -i localhost:5000/person/66c09925-589a-43b6-9a5d-d1601cf53287

If you pass in an invalid UUID, the server should return a 404 message.: Test: curl -X GET -i localhost:5000/person/not-a-valid-uuid
Finally, pass in a valid UUID that does not exist in the data list. 
The method should return a 404 with a message of person not found.:
Test: curl -X GET -i localhost:5000/person/11111111-589a-43b6-9a5d-d1601cf51111





Q6 Implement the DELETE endpoint to delete a person resource.
Create a new endpoint for DELETE http://localhost/person/unique_identifier.
The method should be named delete_by_uuid. 
It should take in an argument of type UUID and delete the person from the data list with that id. 
If the person is not found, the method should return a 404 with a message of person not found. 
Finally, the client (curl) should call this method by passing a valid UUID type id.
Test: curl -X DELETE -i localhost:5000/person/66c09925-589a-43b6-9a5d-d1601cf53287

You can now use the count endpoint you added earlier to test if the number of persons has reduced by one.:
Test: curl -X GET -i localhost:5000/count

If you pass an invalid UUID, the server should return a 404 message.
Test: curl -X DELETE -i localhost:5000/person/not-a-valid-uuid

Finally, pass in a valid UUID that does not exist in the data list. 
The method should return a 404 with a message of person not found. 
Test:curl -X DELETE -i localhost:5000/person/11111111-589a-43b6-9a5d-d1601cf51111





Q7 Create a method called add_by_uuid with the @app.route decorator.
This method should be called when a client requests with the POST method for the /person URL.
The method will not accept any parameter but will grab the person details from the JSON body of the POST request.
The method returns:
person id if the person was successfully added to data; HTTP 200 code
message of Invalid input parameter with a status of HTTP 422 if the json body is missing

You can test the endpoint with the following CURL command. 
Ensure that the server is running in the terminal as in the previous steps.
Test:
curl -X POST -i -w '\n' \
  --url http://localhost:5000/person \
  --header 'Content-Type: application/json' \
  --data '{
        "id": "4e1e61b4-8a27-11ed-a1eb-0242ac120002",
        "first_name": "John",
        "last_name": "Horne",
        "graduation_year": 2001,
        "address": "1 hill drive",
        "city": "Atlanta",
        "zip": "30339",
        "country": "United States",
        "avatar": "http://dummyimage.com/139x100.png/cc0000/ffffff"
}'

You can also test the case where you send an empty JSON to the enpoint by using the following command:
Test: 
curl -X POST -i -w '\n' \
  --url http://localhost:5000/person \
  --header 'Content-Type: application/json' \
  --data '{}'


In this final part of the lab, you will add application level global handlers to handle errors like 404 (not found) and 500 (internal server error). 
Recall from the video that Flask makes it easy to handle these global error handlers using the errorhandler() decorator.

If you make an invalid request to the server now, Flask will return an HTML page with the 404 error. 
Something like this:

Command:curl -X POST -i -w '\n' http://localhost:5000/notvalid

Create a method called api_not_found with the @app.errorhandler decorator.
This method will return a message of API not found with an HTTP status code of 404 whenever the client requests a URL that does not lead to any endpoints defined by the server.
You can test the endpoint with the following CURL command. 
Ensure that the server is running in the terminal, as in the previous steps.
Test: curl -X POST -i -w '\n' http://localhost:5000/notvalid
  


