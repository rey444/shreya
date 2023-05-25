---
toc: true
layout: post
description: Code for my final project.
categories: [t3]
title: Reading Log Code 
---

# Backend

from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()


class Reader(db.Model):
    __tablename__ = 'readers'

    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(255), unique=True, nullable=False)
    name = db.Column(db.String(255), unique=False, nullable=False)
    book = db.Column(db.String(255), unique=False, nullable=False)
    finished_date = db.Column(db.String(255), unique=False, nullable=False)
    rating = db.Column(db.Integer, unique=False, nullable=False)

    def __init__(self, username, name, book, finished_date, rating):
        self.username = username
        self.name = name
        self.book = book
        self.finished_date = finished_date
        self.rating = rating

    def as_dict(self):
        return {
            'id': self.id,
            'username': self.username,
            'name': self.name,
            'book': self.book,
            'finished_date': self.finished_date,
            'rating': self.rating
        }

# Backend Continued

from flask import Blueprint, request, jsonify
from flask_restful import Api, Resource
from sqlalchemy.exc import IntegrityError
from .reader_model import db, Reader

reader_api = Blueprint('reader_api', __name__, url_prefix='/api/readers')
api = Api(reader_api)


class ReaderAPI(Resource):
    def post(self):
        data = request.get_json()
        username = data.get('username')
        name = data.get('name')
        book = data.get('book')
        finished_date = data.get('finished_date')
        rating = data.get('rating')

        if not all([username, name, book, finished_date, rating]):
            return {'message': 'Missing required fields'}, 400

        try:
            reader = Reader(username=username, name=name, book=book, finished_date=finished_date, rating=rating)
            db.session.add(reader)
            db.session.commit()
            return reader.as_dict()
        except IntegrityError:
            db.session.rollback()
            return {'message': 'Username already exists'}, 400

    def get(self):
        readers = Reader.query.all()
        return jsonify([reader.as_dict() for reader in readers])

    def put(self):
        data = request.get_json()
        reader_id = data.get('id')
        reader = Reader.query.get(reader_id)

        if not reader:
            return {'message': 'Reader not found'}, 404

        reader.username = data.get('username', reader.username)
        reader.name = data.get('name', reader.name)
        reader.book = data.get('book', reader.book)
        reader.finished_date = data.get('finished_date', reader.finished_date)
        reader.rating = data.get('rating', reader.rating)

        db.session.commit()
        return reader.as_dict()

    def delete(self):
        data = request.get_json()
        reader_id = data.get('id')
        reader = Reader.query.get(reader_id)

        if not reader:
            return {'message': 'Reader not found'}, 404

        db.session.delete(reader)
        db.session.commit()
        return reader.as_dict()


api.add_resource(ReaderAPI, '/')

# Frontend

<!DOCTYPE html>
<html>
<head>
    <title>Book Log</title>
    <script src="main.js"></script>
</head>
<body>
    <h1>Book Log</h1>
    <form id="bookForm">
        <label for="username">Username:</label>
        <input type="text" id="username" required><br>
        <label for="name">Name:</label>
        <input type="text" id="name" required><br>
        <label for="book">Book:</label>
        <input type="text" id="book" required><br>
        <label for="finished_date">Finished Date:</label>
        <input type="text" id="finished_date" required><br>
        <label for="rating">Rating:</label>
        <input type="number" id="rating" min="1" max="5" required><br>
        <button type="submit">Submit</button>
    </form>
    <div id="bookList"></div>
</body>
</html>

# Frontend Connection to Backend

document.addEventListener("DOMContentLoaded", function() {
    fetchBooks();
    document.getElementById("bookForm").addEventListener("submit", addBook);
});

function fetchBooks() {
    fetch("/api/readers")
        .then(response => response.json())
        .then(data => displayBooks(data))
        .catch(error => console.log(error));
}

function displayBooks(books) {
    const bookList = document.getElementById("bookList");
    bookList.innerHTML = "";

    books.forEach(book => {
        const bookItem = document.createElement("div");
        bookItem.classList.add("book-item");

        const bookInfo = document.createElement("p");
        bookInfo.innerHTML = `<strong>${book.username}</strong> read <em>${book.book}</em> and gave it a rating of ${book.rating}.`;

        bookItem.appendChild(bookInfo);
        bookList.appendChild(bookItem);
    });
}

function addBook(event) {
    event.preventDefault();

    const username = document.getElementById("username").value;
    const name = document.getElementById("name").value;
    const book = document.getElementById("book").value;
    const finished_date = document.getElementById("finished_date").value;
    const rating = document.getElementById("rating").value;

    const bookData = {
        username: username,
        name: name,
        book: book,
        finished_date: finished_date,
        rating: rating
    };

    fetch("/api/readers", {
        method: "POST",
        headers: {
            "Content-Type": "application/json"
        },
        body: JSON.stringify(bookData)
    })
    .then(response => response.json())
    .then(data => {
        console.log(data);
        fetchBooks();
        document.getElementById("bookForm").reset();
    })
    .catch(error => console.log(error));
}
