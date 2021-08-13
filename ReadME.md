# Sample Swagger API with C# using MongoDB and Docker

## The second largest heading
###### Tutorial was implemented at https://dev.to/etnicholson/developing-a-crudapi-with-asp-net-core-mongodb-docker-swagger-cf4
## Steps to follow to create an application

1. Create empty web api
2. Install 2 packages: MongoDB.Driver and Swashbuckle.AspNetCore

3. Create Models folder and data model as class (in this example Supplement.cs)
        - database connection string needs to be set in appsettings.json

         Connection string for api to accessing from inside docker network 
                '''
                    "ConnectionStrings": {
                        "SupplementDB": "mongodb://mongo:27017"
                      },
                '''
         Connection string for api to accessing from outside docker network
                '''
                    "ConnectionStrings": {
                            "SupplementDB": "mongodb://localhost:27017"
                       },
                '''

4. Create Services folder and create class containing database Tasks (in this example SupplementService.cs)
        - service uses mongodb and the connection string will be used here (as IConfiguration config parameter)
               '''

                private readonly IMongoCollection<Supplement> _supplement;

                public SupplementService(IConfiguration config)
                {
                    // Connects to MongoDB.
                    var client = new MongoClient(config.GetConnectionString("SupplementDB"));
                    // Gets the supplementDB.
                    var database = client.GetDatabase("SupplementDB");
                    //Fetches the supplement collection.
                    _supplement = database.GetCollection<Supplement>("Supplements");
                }

               '''
            
        - created service needs to be configured at Startup.cs in ConfigureServices method AddScope and AddSwaggerGen needs to be added.

                '''
                    services.AddScoped<SupplementService>();


                    services.AddSwaggerGen(c =>
                    {
                        c.SwaggerDoc("v1", new OpenApiInfo
                        {
                            Title = "Supplement API",
                            Version = "v1",
                            Description = "Supplement API tutorial using MongoDB",
                        });
                    });
                '''


5. Create Controllers folder and create class containing ActionResults that will be used by swagger. (in this example SupplementController.cs)

        - controller uses the service created.

        '''
                private readonly SupplementService _supplementService;

                public SupplementController(SupplementService supplementService)
                {
                    _supplementService = supplementService;
                }
        '''


        - created controllers needs to be added at Startup.cs in ConfigureServices

        '''
         services.AddControllers();
        '''




6. Go to http://localhost:5002/swagger/index.html to access swaggerpage