# Content-Based Book Recommendation System

![License](https://img.shields.io/badge/license-MIT-green)
![Python Version](https://img.shields.io/badge/python-3.9+-blue)
![Scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)

A simple yet effective content-based book recommendation system that suggests books similar to a user's choice based on their metadata. This project utilizes TF-IDF and Cosine Similarity to measure the likeness between books.

---

## 📋 Table of Contents
- [About The Project](#about-the-project)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Built With](#built-with)
- [License](#license)
- [Contact](#contact)

---

## 📝 About The Project

Recommendation systems are a cornerstone of modern content platforms. This project demonstrates the **content-based filtering** approach, where recommendations are generated based on the attributes of the items themselves. Unlike collaborative filtering, this method doesn't require user rating data, making it effective even for new items or users.

The system takes a book title as input and returns a list of the top 10 most similar books from the dataset.

### Project Goals
- To build a functional content-based recommendation system for books.
- To apply Natural Language Processing (NLP) techniques like TF-IDF for feature extraction from text data.
- To use Cosine Similarity to quantify the similarity between different books.

---

## 🔧 How It Works

The recommendation logic follows these steps:
1.  **Data Loading & Cleaning**: The Goodreads book dataset is loaded and essential columns (`title`, `authors`, `publisher`, etc.) are selected.
2.  **Feature Engineering**: A combined feature string is created for each book from its metadata (authors, publisher, and title).
3.  **Text Vectorization**: The **TF-IDF (Term Frequency-Inverse Document Frequency) Vectorizer** is used to convert the combined text features into a matrix of numerical vectors. This process gives higher weight to words that are significant to a specific book's description but not common across all books.
4.  **Similarity Calculation**: **Cosine Similarity** is calculated between the TF-IDF vectors of all books. This metric measures the cosine of the angle between two vectors, providing a similarity score from 0 (not similar) to 1 (identical).
5.  **Recommendation Generation**: When a user inputs a book title, the system finds its corresponding vector and retrieves the top 10 books with the highest cosine similarity scores.

---

## 📂 Project Structure
A clean and organized project structure:
```
scikit-learn-book-recommendation/
├── data/
│   └── books.csv
├── notebooks/
│   └── book_recommendation_system.ipynb
├── .gitignore
├── book_recommendation_system.py
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

To get this project up and running on your local machine, follow these steps.

### Prerequisites
- Python 3.8 or higher

### Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/irgidev/scikit-learn-book-recommendation.git](https://github.com/irgidev/scikit-learn-book-recommendation.git)
    cd scikit-learn-book-recommendation
    ```
2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # For Windows: venv\Scripts\activate
    ```
3.  **Install the required dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

---

## 💻 Usage

You can interact with the project in two ways:

1.  **Jupyter Notebook (for exploration):**
    Open and run the cells in `notebooks/book_recommendation_system.ipynb` to see the step-by-step process of data cleaning, model building, and recommendation.

2.  **Python Script (for direct use):**
    Run the script from your terminal. It will prompt you to enter a book title from the dataset to get recommendations.
    ```bash
    python book_recommendation_system.py
    ```
    Example interaction:
    ```
    Enter a book title from the dataset:
    The Hobbit
    
    Top 10 book recommendations for "The Hobbit":
    1. The Lord of the Rings (The Lord of the Rings, #1-3)
    2. The Silmarillion
    3. The Two Towers (The Lord of the Rings, #2)
    4. The Return of the King (The Lord of the Rings, #3)
    5. J.R.R. Tolkien 4-Book Boxed Set: The Hobbit and The Lord of the Rings
    ... and so on
    ```

---

## 🛠️ Built With
- **[Scikit-learn](https://scikit-learn.org/)**: For TF-IDF Vectorizer and Cosine Similarity.
- **[Pandas](https://pandas.pydata.org/)**: For data manipulation and analysis.
- **[NumPy](https://numpy.org/)**: For numerical operations.

---

## 📬 Contact
Irgi Setiawan - www.linkedin.com/in/irgi-setiawan-85135130a - irgisetiawan3008@gmail.com
