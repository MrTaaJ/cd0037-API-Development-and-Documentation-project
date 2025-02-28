# API Development and Documentation Final Project

## Trivia App

Udacity is invested in creating bonding experiences for its employees and students. A bunch of team members got the idea to hold trivia on a regular basis and created a webpage to manage the trivia app and play the game, but their API experience is limited and still needs to be built out.

That's where you come in! Help them finish the trivia app so they can start holding trivia and seeing who's the most knowledgeable of the bunch. The application must:

1. Display questions - both all questions and by category. Questions should show the question, category and difficulty rating by default and can show/hide the answer.
2. Delete questions.
3. Add questions and require that they include question and answer text.
4. Search for questions based on a text query string.
5. Play the quiz game, randomizing either all questions or within a specific category.

Completing this trivia app will give you the ability to structure plan, implement, and test an API - skills essential for enabling your future applications to communicate with others.

## Starting and Submitting the Project

[Fork](https://help.github.com/en/articles/fork-a-repo) the project repository and [clone](https://help.github.com/en/articles/cloning-a-repository) your forked repository to your machine. Work on the project locally and make sure to push all your changes to the remote repository before submitting the link to your repository in the Classroom.

## About the Stack

We started the full stack application for you. It is designed with some key functional areas:

### Backend

The [backend](./backend/README.md) directory contains a partially completed Flask and SQLAlchemy server. You will work primarily in `__init__.py` to define your endpoints and can reference models.py for DB and SQLAlchemy setup. These are the files you'd want to edit in the backend:

1. `backend/flaskr/__init__.py`
2. `backend/test_flaskr.py`

> View the [Backend README](./backend/README.md) for more details.

### Frontend

The [frontend](./frontend/README.md) directory contains a complete React frontend to consume the data from the Flask server. If you have prior experience building a frontend application, you should feel free to edit the endpoints as you see fit for the backend you design. If you do not have prior experience building a frontend application, you should read through the frontend code before starting and make notes regarding:

1. What are the end points and HTTP methods the frontend is expecting to consume?
2. How are the requests from the frontend formatted? Are they expecting certain parameters or payloads?

Pay special attention to what data the frontend is expecting from each API response to help guide how you format your API. The places where you may change the frontend behavior, and where you should be looking for the above information, are marked with `TODO`. These are the files you'd want to edit in the frontend:

1. `frontend/src/components/QuestionView.js`
2. `frontend/src/components/FormView.js`
3. `frontend/src/components/QuizView.js`

By making notes ahead of time, you will practice the core skill of being able to read and understand code and will have a simple plan to follow to build out the endpoints of your backend API.

> View the [Frontend README](./frontend/README.md) for more details.

## API Reference

### Getting Started
- Base URL: At present this app can only be run locally and is not hosted as a base URL. The backend app is hosted at the default, `http://127.0.0.1:5000/`, which is set as a proxy in the frontend configuration. 
- Authentication: This version of the application does not require authentication or API keys. 

### Error Handling
Errors are returned as JSON objects in the following format:
```
{
    "success": False, 
    "error": 404,
    "message": "resource not found"
}
```
The API will return three error types when requests fail:
- 400: bad request
- 404: resource not found
- 422: unprocessable 

### Endpoints 

#### GET /categories
- General:
    - Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
    - Request Arguments: None
    - Returns: An object with a single key, categories, that contains an object of id: category_string, key: value pairs, and the success value.
- Sample: `curl http://127.0.0.1:5000/categories`

``` {
    'success': true
    'categories': {
        "1": "Science",
        "2": "Art",
        "3": "Geography",
        "4": "History",
        "5": "Entertainment",
        "6": "Sports"
    }
        
}
```

#### GET /questions?page=${integer}
- General:
    - Fetches a paginated set of questions, a total number of questions, and all categories.
    - Request Arguments: page - integer
    - Returns: An object with 10 paginated questions, total questions, object including all categories, and the success value.
- Sample: `curl http://127.0.0.1:5000/questions` or `curl http://127.0.0.1:5000/questions?page=2`
```
{
    'success': true
    'questions': [
        {
            'id': 1,
            'question': 'This is a question',
            'answer': 'This is an answer',
            'difficulty': 5,
            'category': 2
        },
        {
            'id': 2,
            'question': 'This is another question',
            'answer': 'This is another answer',
            'difficulty': 3,
            'category': 6
        },
    ],
    'total_questions': 100,
    'categories': { 
        '1' : "Science",
        '2' : "Art",
        '3' : "Geography",
        '4' : "History",
        '5' : "Entertainment",
        '6' : "Sports"
    }
}
```

### DELETE '/questions/${id}'
- General:
    - Deletes a specified question using the id of the question
    - Request Arguments: id - integer
    - Returns: An object with 10 paginated questions, total questions, object including all categories, deleted question's id, and the success value.
- Sample: `curl -X DELETE http://127.0.0.1:5000/questions/2`
```
{
    'success': true
    'deleted': 2,
    'questions': [
        {
            'id': 1,
            'question': 'This is a question',
            'answer': 'This is an answer',
            'difficulty': 5,
            'category': 2
        },
    ],
    'total_questions': 100,
    'categories': { 
        '1' : "Science",
        '2' : "Art",
        '3' : "Geography",
        '4' : "History",
        '5' : "Entertainment",
        '6' : "Sports"
    }
}
```

### GET '/categories/${id}/questions'
- General:
    - Fetches questions for a category specified by id request argument
    - Request Arguments: id - integer
    - Returns: An object with questions for the specified category, total questions, current category string, and the success value.
- Sample: `curl http://127.0.0.1:5000/categories/4/questions`
```
{
    'success': true
    'questions': [
        {
            'id': 1,
            'question': 'This is a question',
            'answer': 'This is an answer',
            'difficulty': 5,
            'category': 4
        },
        {
            'id': 2,
            'question': 'This is another question',
            'answer': 'This is another answer',
            'difficulty': 3,
            'category': 4
        },
    ],
    'total_questions': 100,
    'current_categories': 'History'
}

```

### POST '/questions'
- General:
    - Sends a post request in order to add a new question
    - Request Body:
        ```
        {
            'question':  'A new Question?',
            'answer':  'New Answer',
            'difficulty': 4,
            'category': 1,
        }
        ```
    - Returns: An object with 10 paginated questions, total questions, object including all categories, created question's id, and the success value.
- Sample: ` curl -X POST http://127.0.0.1:5000/questions -H "Content-Type: application/json" -d "{\"question\":\"A new Question?\", \"answer\":\"New Answer\", \"difficulty\":\"4\", \"category\":\"1\"}" `
```
{
    'success': true
    'created': 3,
    'questions': [
        {
            'id': 1,
            'question': 'This is a question',
            'answer': 'This is an answer',
            'difficulty': 5,
            'category': 2
        },
        {
            'id': 2,
            'question': 'This is another question',
            'answer': 'This is another answer',
            'difficulty': 3,
            'category': 6
        },
    ],
    'total_questions': 100,
    'categories': { 
        '1' : "Science",
        '2' : "Art",
        '3' : "Geography",
        '4' : "History",
        '5' : "Entertainment",
        '6' : "Sports"
    }
}
```

### POST '/questions'
- General:
    - Sends a post request in order to search for a specific question by search term
    - Request Body:
        ```
        {
            'searchTerm': 'You search for this'
        }
        ```
    - Returns: An array of questions, a number of totalQuestions that met the search term, an object including all categories, and the success value.
- Sample: ` curl -X POST http://127.0.0.1:5000/questions -H "Content-Type: application/json" -d "{\"searTerm\":\"You search for this\"}" `
```
{
    'success': true
    'questions': [
        {
            'id': 1,
            'question': 'This is a question',
            'answer': 'This is an answer',
            'difficulty': 5,
            'category': 2
        },
        {
            'id': 2,
            'question': 'This is another question',
            'answer': 'This is another answer',
            'difficulty': 3,
            'category': 6
        },
    ],
    'total_questions': 100,
    'categories': { 
        '1' : "Science",
        '2' : "Art",
        '3' : "Geography",
        '4' : "History",
        '5' : "Entertainment",
        '6' : "Sports"
    }
}
```

### POST '/quizzes'
- General:
    - Sends a post request in order to search for a specific question by search term
    - Request Body:
        ```
        {
            'previous_questions': [1, 4, 20, 15]
            'quiz_category': 'current category'
        }
        ```
    - Returns: A single randomly selected new question object and the success value.
- Sample: ` curl -X POST http://127.0.0.1:5000/quizzes -H "Content-Type: application/json" -d "{\"previous_questions\":\"[1, 4, 20, 15]\", \"quiz_category\":\"3\"}" `

```
{
    'success': true
    'questions': [
        {
            'id': 1,
            'question': 'This is a question',
            'answer': 'This is an answer',
            'difficulty': 5,
            'category': 3
        }
    ]
}
```