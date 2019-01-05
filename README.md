Create your project folder

cd into your project folder
In your terminal, run 'npm init' or 'npm init -y'
Make sure that your entry point is server.js. If you run 'npm init -y', you will have to change the entry point in your package.json

Run 'npm i express mongoose hbs method-override' in your project folder

This will install your dependencies for the MEHN stack application.

Create a server.js file either using the GUI in VS code or by running 'touch server.js' in your project folder.

In your server file, require express, invoke an instance of express. Set your port to listen at localhost 3000 and tell your server to listen at the Port. An example of this might look something like this.



`const express = require('express')
const app = express()

const PORT = process.env.PORT || 3000

app.listen(PORT, () => {
    console.log(`Server is listening on PORT: ${PORT}`)
})`

Run 'node server.js' to test that this is working.

Create a folder for your controllers, routes, models, db, and views. 

Now, let's set up our database and send it some dummy data. In your db folder, create a connection.js file. This is where we want to connect to our database using mongoose. Here we want to require mongoose and connect to our database. We use the mongoose.connect method to connect to our database. It might look something like this: 

`mongoose.connect("mongodb://localhost/your-data-base-name").then(() => {
    console.log("MONGODB is now connected")
})`

Don't forget to export mongoose here.

Now we need to create a schema and model for our application. If we were to set up a model for cars, our schema might look something like this: 

`const Car = new Schema({
    make: String,
    model: String,
    year: Number,
    color: String,
    insured: Boolean
})`

Refer back to previous projects for a full example of a model and the syntax. We will need to import mongoose and mongoose.Schema as well as export our new model.

Now that we have connected to our database and created a blue print for our cars, we need to send some seed data to test that it is working. In your db directory, create a seeds.js file. In our new seeds file, we need to require our cars model. Once we do that, we need to use mongoose methods to create and send that data to our database. It might looks something like this:

`const Car = require('../models/Car')

Car.deleteMany({})
    .then(() => {
        const civic = Car.create({
            make: 'Honda',
            model: 'Civic',
            year: 2015,
            color: 'Red',
            insured: true
        })
            .then((car) => {
                Promise.all([civic]).then(() => {
                    car.save()
                    console.log('successfully seeded')
                })
            })
    })`

First, we delete all of our data in the database, then we create a new car called civic. We use Car.create({}) to create our new car object, then we save that car in our data base. Let's run 'node db/seeds'. If all is well, you should now have some data seeded. To test this, either check in MongoDB Compass or check in your command line by running :

'mongo'
'show dbs'
'use *db name*'
'show collections'
'db.collectionName.find().pretty()'

Your data should now be displayed.

Let's now try to display the data in our browser. We need to set up a route and a controller action now. In your routes folder, create an index.js file. 

In this file, we need to require express and the invoke express.Router(). We also need to import our carController (which we haven't made just yet). Let's write an index route for our cars. It may look something like 'router.get('/cars', carController.index).
Now we need to export our router.

In our server.js file we need to import and use our new router. 

We will require our router from './routes/index' and we will use it by writing 'app.use('/', router)'.

The last step is to create our carController. In your controllers folder, create a file called carController.js. 

Here we import our Car model and define a variable called carController which will be an object in which we will write our controller actions. Let's get started with our index action.

`const Car = require('../models/Car')

const carController = {
    index: (req, res) => {
        Car.find({}).then((car) => {
            res.send(car)
        })
    }
}

module.exports = carController`

If all goes well, when we go to localhost:3000/cars, we should see our seeded car data displayed in the browser. 

Now I'll leave it to you all to connect the views via handlebars but if you check in the calendar, you should be able to find the handlebars lesson to reference. As always, we are available for questions. Hopefully this will be of some use to you all as you go into project 2. Good luck!