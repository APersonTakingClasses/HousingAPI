-- Overview --
For this assignment, you will be updating your "CSV Parser" assignment to be a little bit more powerful. Rather than just calculating a few hardcoded statistics and printing them out, you will be creating a server that lets the user specify what they want to calculate and then calculating that data on the fly.

-- Data and Libraries --
The data source for this assignment will be the same California housing data CSV file that you used in assignment 2. Like in assignment 2, I would recommend using the Apache Commons CSV parsing library to read out data from this file. For this assignment though, you will also need to use the Spring Framework and the Spring Web module like we've been doing in class. It would totally be possible to update your CSV parsing code to use Spring but it might be a bit simpler to start a new Spring project (using the Spring Initializr site or the IntelliJ "new project") helper and then porting your old code over to the new project. Spring is a bit more annoying to add to an existing project than the libraries we've worked with so far.

-- Part 1: The API -- 
Rather than just calculating overall statistics from the CSV file and printing them out, your API will let the user request more fine-grained statistics and will calculate the result on the fly for them. Theoretically, this API could be the "back end" for a user-friendly housing data website (but you won't have to build that part).

Your API should have just one endpoint. It should handle GET requests for "/housing-data". All input parameters will be passed as query string parameters. These parameters will specify what statistics you should calculate. The user should be able to specify the following parameters:

  field: This parameter tells the API which field to calculate stats for. It is required and should be either "price" or "squareFeet"
  statistic: This parameter tells the API which statistic to calculate. It is also required and should be one of "min", "max", "average", "sum", "range". Range is calculated as max-min.
  zipCode: This optional parameter tells the API to filter housing data entries by zip code before calculating statistics. If not provided, calculate the statistic on all data.
  startDate: This optional parameter tells the API to filter housing data by date. If provided, filter out housing data from before the start date before calculating your statistic.
  endDate: This optional parameter tells the API to filter housing data by date. If provided, filter out housing data from after the end date before calculating your statistic.
An example of a request to your API would be a GET requests with a URL like "http://localhost:8080/housing-data?field=price&statistic=average&zipCode=95838&startDate=05-21-2008". This request would mean that you want the server to calculate the average price of houses in the 95838 zip code after May 21st 2008 (no explicit end date). You should return a value in the format:

{
  result: 123.45,
  numHouses: 123
}

-- Part 2: Unit Testing --
This project is more complex than the previous CSV parsing assignment. In that assignment, it might have been possible to write most of your code in a single file and maybe even a single method. For this assignment, that would probably be a bit less feasible. Like the Tic Tac Toe assignment, it probably makes sense to break this assignment into multiple files according to different components of the assignment. I recommend having at least one class/file to manage each of the following aspects of the assignment:
Loading data from the CSV file. It might make sense to convert the CSVRecords into something easier to work with
Filtering the records based on the query string parameters provided
Calculating the expected statistics for the expected field given the filtered records
If you want to break things down even further, go for it!

Once you have your data broken into at least these units, please write at least 2 automated JUnit tests for each of the three different features above (loading, filtering, calculating). Your data loading test can be more of an integration test if you want (it can load a real file) but the other 2 should be true unit tests. In other words, your automated test for filtering data should not involve any file reading or statistic calculating. Your tests for the statistic calculation should not involve any file reading or filtering code.

-- Submissions --
As with other assignments, please submit a link to a private GitHub repository. Please remember to add me as a collaborator! I'd recommend using a new repo rather than re-using the repo from assignment 2

-- Tips / Hints --
Don't try to do everything in one file/method. Your code will be messy
Don't try to do everything at once. Can you replicate assignment 2 but have it calculate stats when called from an HTTP API and have it return the stats in an HTTP response?
It might be a good idea to convert the CsvRecords that are returned by the Commons CSV library into something a bit nicer. Maybe you should make a new class that represents a row in the CSV file and has methods to access the fields you're interested in.
It is possible to load the CSV file once when the server loads rather than on every individual request. This will make the server faster and might help keep your code cleaner. The @PostConstruct annotation in Spring might be a good place to do your file reading. You can maybe store the data from the file into a List or other object you control and then not need to load it again.
