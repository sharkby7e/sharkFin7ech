# sharkFin7ech

## Description

This is a server-side application that allows the user to store important information for their online store. They can list products with prices and descriptions, and then categorize and tag the products.
This ensures that all stock remains categorized and users can search products by their associated tags. The user can also add product, categories and tags, as well as update and delete any of those three.

## Table of contents

- [Key Functions](#key-functions)
- [Installation](#installation)
- [Usage](#usage)
- [License](#license)
- [Contact/Questions](#questions)
- [Summary](#summary-and-learning-points)

## Video Preview of Application

This application is not deployed, please click the link below to view a video of the application functioning.

Alternatively, you can [install](#installation) this application on your own machine and try it yourself.

[Click to view the application video demo](https://drive.google.com/file/d/1gpkEIPPF95u-nLRZIYmM7tLB7mnH69w4/view)

## Technologies Employed

| Techlogy | Implementation/Use   |
| -------- | -------------------- |
| Node.js  | JavaScript runtime   |
| NPM      | Manage node packages |
| sequlize | node package for ORM |
| mySQL    | database             |
| Insomnia | testing API routes   |
| Router   | modular routing      |

## Key Functionality

### router.delete

This is an example of one of the delete routes that I used in this application. It uses the query params
in order to find which entry in the table to delete. It returns an error if the one that the user specified
doesn't exist within the database. I used an async await in order to make sure that the query of the database
finished before the work was done

```javascript
router.delete("/:id", async (req, res) => {
  try {
    const categoryData = await Category.destroy({
      where: {
        id: req.params.id,
      },
    });
    if (!categoryData) {
      res.status(404).json({ message: "No category with given ID found!" });
      return;
    }
    res.status(200).json(categoryData);
  } catch (err) {
    res.status(500).json(err);
  }
});
```

### belongsToMany

This is what developed the relationship of many to many between products and tags. Tags had many products
and Products had many tags, and so we had to make a third "joining" relationship to associate the two.

```javascript
Product.belongsToMany(Tag, {
  through: {
    model: ProductTag,
    unique: false,
  },
  as: "product_tags",
});

Tag.belongsToMany(Product, {
  through: {
    model: ProductTag,
    unique: false,
  },
  as: "tag_products",
});
```

## Installation

To install this application, clone the repository

```
git@github.com:sharkby7e/sharkFin7ech.git
```

navigate into the directory, and then run the following command

```
npm i
```

## Usage

To use the program, first we will need to create the database to hold information. In mySQL shell input:

```
source db/schema.sql
```

Then, in order to access the database, create a .env file in the cloned home directory

```shell
touch .env
```

Within the .env file, you will have to input the following information

```
DB_USER = 'root'
DB_PW = <YOUR PASSWORD>
DB_NAME = 'ecommerce_db'
```

Save the file, and then we will need to seed the database.

```shell
node seeds/index.js
```

To start the server run:

```
npm start
```

The server is now running, and you can make requests to the API routes.

## License

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

## Questions?

Please contact me at:

sgquin@gmail.com

Alternatively, visit my github:

https://www.github.com/sharkby7e

## Summary and Learning Points

This project was an introduction to Object Relational Mapping(ORM), which allowed me to have model and store and manipulate data in myriad ways.
Once the models for our data were created, I was able to ensure that all data that I added was related to each other in the correct ways, and
was able to be retrieved efficiently. This project also showcases my ability to build functioning API's that can take requests, query a database,
and serve information back to the user.
This project showed the benefits of using sequlize to query a database. Even though their built in methods can be picky, I much prefer using sequlize
to writing raw SQL.
