# Project: Final Database Exam

#Duties and responsibilities
CRUD OPearions: Abrar
TESTING : Faiyaz
Git and Setup:Naveed

# Project: Final Database Exam

## Duties and Responsibilities
- *CRUD Operations:* Abrar
- *Testing:* Faiyaz
- *Git and Setup:* Naveed

## SQL Table Definitions

```sql
-- Create table for authors
CREATE TABLE authors (
    authorid SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

-- Create table for genres
CREATE TABLE genres (
    genre_id SERIAL PRIMARY KEY,
    name VARCHAR(50)
);

-- Create table for books
CREATE TABLE books (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(200),
    genre_id INT,
    author_id INT,
    publisher_id INT,
    published_date DATE,
    rating DECIMAL(3, 2),
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id),
    FOREIGN KEY (author_id) REFERENCES authors(authorid)
);

-- Create table for publishers
CREATE TABLE publishers (
    publisher_id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

-- Create table for customers
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    join_date DATE,
    total_spent DECIMAL(10, 2)
);

-- Create table for reviews
CREATE TABLE reviews (
    review_id SERIAL PRIMARY KEY,
    book_id INT,
    customer_id INT,
    review_date DATE,
    rating DECIMAL(3, 2),
    comment TEXT,
    FOREIGN KEY (book_id) REFERENCES books(book_id),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- Create table for sales
CREATE TABLE sales (
    sale_id SERIAL PRIMARY KEY,
    book_id INT,
    customer_id INT,
    sale_date DATE,
    amount DECIMAL(10, 2),
    FOREIGN KEY (book_id) REFERENCES books(book_id),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

#CRUD OPERATION
import { Entity, Column, PrimaryColumn } from "typeorm";

@Entity()
export class Customer {
  @PrimaryColumn()
  CustomerID: number;

  @Column({
    length: 20,
  })
  Name: string;

  @Column({
    length: 20,
    unique: true,
  })
  Email: string;

  @Column("date")
  RegistrationDate: string;

  @Column("decimal", { precision: 10, scale: 2, default: 0 })
  TotalSpentLastYear: number;
}

import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class Customer {
  @PrimaryGeneratedColumn()
  CustomerID: number;

  @Column({
    length: 20,
  })
  Name: string;

  @Column({
    length: 20,
    unique: true,
  })
  Email: string;

  @Column("date")
  RegistrationDate: string;

  @Column("decimal", { precision: 10, scale: 2, default: 0 })
  TotalSpentLastYear: number;
}


import { Entity, Column, PrimaryGeneratedColumn, ManyToOne } from "typeorm";
import { Customer } from "./Customer"; // Assume you have a Customer entity

@Entity()
export class Order {
  @PrimaryGeneratedColumn()
  OrderID: number;

  @ManyToOne(() => Customer)
  CustomerID: Customer;

  @Column("datetime")
  OrderDate: Date;

  @Column("decimal", { precision: 10, scale: 2 })
  TotalAmount: number;
}

import { Entity, Column, PrimaryGeneratedColumn, ManyToOne } from "typeorm";
import { Customer } from "./Customer"; // Ensure you have this entity defined

@Entity()
export class Order {
  @PrimaryGeneratedColumn()
  OrderID: number;

  @ManyToOne(() => Customer)
  CustomerID: Customer;

  @Column("datetime")
  OrderDate: Date;

  @Column("decimal", { precision: 10, scale: 2 })
  TotalAmount: number;
}

import { Entity, Column, PrimaryGeneratedColumn, OneToMany } from "typeorm";
import { Book } from "./Book";

@Entity()
export class Publisher {
  @PrimaryGeneratedColumn()
  PublisherID: number;

  @Column({
    length: 100,
  })
  Name: string;

  @OneToMany(() => Book, (book) => book.Publisher)
  books: Book[];
}

import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class Author {
  @PrimaryGeneratedColumn()
  authorid: number;

  @Column({ length: 100 })
  name: string;
}


import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class Publisher {
  @PrimaryGeneratedColumn()
  PublisherID: number;

  @Column({
    length: 20,
  })
  Name: string;

  @Column({
    length: 20,
    nullable: true,
  })
  ContactInfo: string;
}


import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";

@Entity()
export class Genre {
  @PrimaryGeneratedColumn()
  genre_id: number;

  @Column({ length: 50 })
  name: string;
}


### For TESTING Setting Up Testing Environment
npm install jest ts-jest @types/jest typeorm sqlite3 --save-dev

jest Setup
import { createConnection, getConnection } from "typeorm";

beforeAll(async () => {
  await createConnection({
    type: "sqlite",
    database: ":memory:",
    dropSchema: true,
    entities: ["src/entity/**/*.ts"],
    synchronize: true,
    logging: false,
  });
});

afterAll(async () => {
  const conn = getConnection();
  await conn.close();
});

##author.genre.test.ts
import { getRepository } from "typeorm";
import { Author } from "./Author";
import { Genre } from "./Genre";

describe("Author and Genre Entities", () => {
  it("should create and save an author", async () => {
    const authorRepo = getRepository(Author);

    const author = new Author();
    author.name = "J.K. Rowling";

    const savedAuthor = await authorRepo.save(author);

    expect(savedAuthor.authorid).toBeDefined();
    expect(savedAuthor.name).toBe("J.K. Rowling");

    const foundAuthor = await authorRepo.findOne(savedAuthor.authorid);
    expect(foundAuthor).toEqual(savedAuthor);
  });

  it("should create and save a genre", async () => {
    const genreRepo = getRepository(Genre);

    const genre = new Genre();
    genre.name = "Fantasy";

    const savedGenre = await genreRepo.save(genre);

    expect(savedGenre.genre_id).toBeDefined();
    expect(savedGenre.name).toBe("Fantasy");

    const foundGenre = await genreRepo.findOne(savedGenre.genre_id);
    expect(foundGenre).toEqual(savedGenre);
  });
});


### Requirements

You will need NPM and Node installed on your local machine. It is highly
recommended that you use a environment manager. The environment manager will
prevent pollution of your local system.


##### Linux/macOs

I highly recommend [NVM](https://github.com/nvm-sh/nvm).
Read about the details on the NVM project page.

###### Windows

Setting up development for Windows is a little bit more complicated. There are
three (3) pieces of technology you will most likely need:

1. terminal
2. SSH
3. git

### Starting Development

Validate that you have Node and NPM:

```bash
node -v
```

```bash
npm -v
```

If you have them installed, you will be given the version number.

Install the required dependencies:

```bash
npm install
```

Start the development environment:

```bash
npm start
```

### Tests

Run tests with:

```bash
npm run test
```

### Starting Development

Validate that you have Node and NPM:

```bash
node -v
```

```bash
npm -v
```

### Using docker compose

These commands are to be ran in the docker compose directory.

#### Build the Image

```bash
docker-compose build
```

#### Run the image

```bash
docker-compose up -d
```

#### Build and Run the image

```bash
docker-compose up -d --build
```
import { getRepository } from "typeorm";
import { Author } from "./Author";
import { Genre } from "./Genre";
import { Publisher } from "./Publisher";
import { Book } from "./Book";
import { Customer } from "./Customer";
import { Review } from "./Review";
import { Sale } from "./Sale";

describe("Bookstore Database Entities", () => {
  it("should create and retrieve an author", async () => {
    const authorRepo = getRepository(Author);
    const author = new Author();
    author.name = "George Orwell";

    const savedAuthor = await authorRepo.save(author);
    expect(savedAuthor.authorid).toBeDefined();
    expect(savedAuthor.name).toBe("George Orwell");

    const foundAuthor = await authorRepo.findOne(savedAuthor.authorid);
    expect(foundAuthor).toEqual(savedAuthor);
  });

  it("should create and retrieve a genre", async () => {
    const genreRepo = getRepository(Genre);
    const genre = new Genre();
    genre.name = "Dystopian";

    const savedGenre = await genreRepo.save(genre);
    expect(savedGenre.genre_id).toBeDefined();
    expect(savedGenre.name).toBe("Dystopian");

    const foundGenre = await genreRepo.findOne(savedGenre.genre_id);
    expect(foundGenre).toEqual(savedGenre);
  });

  it("should create and retrieve a publisher", async () => {
    const publisherRepo = getRepository(Publisher);
    const publisher = new Publisher();
    publisher.name = "Penguin Books";

    const savedPublisher = await publisherRepo.save(publisher);
    expect(savedPublisher.publisher_id).toBeDefined();
    expect(savedPublisher.name).toBe("Penguin Books");

    const foundPublisher = await publisherRepo.findOne(savedPublisher.publisher_id);
    expect(foundPublisher).toEqual(savedPublisher);
  });

  it("should create and retrieve a book", async () => {
    const authorRepo = getRepository(Author);
    const genreRepo = getRepository(Genre);
    const publisherRepo = getRepository(Publisher);
    const bookRepo = getRepository(Book);

    const author = await authorRepo.save({ name: "George Orwell" });
    const genre = await genreRepo.save({ name: "Dystopian" });
    const publisher = await publisherRepo.save({ name: "Penguin Books" });

    const book = new Book();
    book.title = "1984";
    book.author = author;
    book.genre = genre;
    book.publisher = publisher;
    book.published_date = new Date("1949-06-08");
    book.rating = 4.8;

    const savedBook = await bookRepo.save(book);
    expect(savedBook.book_id).toBeDefined();
    expect(savedBook.title).toBe("1984");

    const foundBook = await bookRepo.findOne(savedBook.book_id, { relations: ["author", "genre", "publisher"] });
    expect(foundBook).toEqual(savedBook);
  });

  it("should create and retrieve a customer", async () => {
    const customerRepo = getRepository(Customer);
    const customer = new Customer();
    customer.name = "John Doe";
    customer.email = "johndoe@example.com";
    customer.join_date = new Date("2024-01-01");
    customer.total_spent = 100.0;

    const savedCustomer = await customerRepo.save(customer);
    expect(savedCustomer.customer_id).toBeDefined();
    expect(savedCustomer.name).toBe("John Doe");

    const foundCustomer = await customerRepo.findOne(savedCustomer.customer_id);
    expect(foundCustomer).toEqual(savedCustomer);
  });

  it("should create and retrieve a review", async () => {
    const bookRepo = getRepository(Book);
    const customerRepo = getRepository(Customer);
    const reviewRepo = getRepository(Review);

    const book = await bookRepo.save({ title: "1984", author: null, genre: null, publisher: null });
    const customer = await customerRepo.save({ name: "John Doe", email: "johndoe@example.com", join_date: new Date("2024-01-01"), total_spent: 100.0 });

    const review = new Review();
    review.book = book;
    review.customer = customer;
    review.review_date = new Date("2024-01-02");
    review.rating = 4.5;
    review.comment = "A must-read classic!";

    const savedReview = await reviewRepo.save(review);
    expect(savedReview.review_id).toBeDefined();
    expect(savedReview.comment).toBe("A must-read classic!");

    const foundReview = await reviewRepo.findOne(savedReview.review_id, { relations: ["book", "customer"] });
    expect(foundReview).toEqual(savedReview);
  });

  it("should create and retrieve a sale", async () => {
    const bookRepo = getRepository(Book);
    const customerRepo = getRepository(Customer);
    const saleRepo = getRepository(Sale);

    const book = await bookRepo.save({ title: "1984", author: null, genre: null, publisher: null });
    const customer = await customerRepo.save({ name: "John Doe", email: "johndoe@example.com", join_date: new Date("2024-01-01"), total_spent: 100.0 });

    const sale = new Sale();
    sale.book = book;
    sale.customer = customer;
    sale.sale_date = new Date("2024-01-03");
    sale
