# CineMatch---Movie-Recommendation-System

## Overview
CineMatch is an AI-powered movie recommendation system that suggests movies similar to the ones users select. It utilizes machine learning techniques to find similarities between movies and provides recommendations along with movie posters.

![Project Banner](Project%20Banner.png)

## Features
- **Movie Recommendations**: Provides five movie suggestions based on user selection.
- **Poster Fetching**: Displays posters for the recommended movies.
- **User-friendly Interface**: Built with Streamlit for an intuitive experience.

## Dataset
The system uses a dataset containing movie titles, unique movie IDs, and similarity scores generated from movie features. The dataset is preprocessed and stored in pickle files (`movie_dict.pkl` and `similarity.pkl`).

## How It Works
### 1. Data Preparation
- A dataset containing movie metadata is used.
- The movie similarity matrix is computed using machine learning techniques.
- The processed data is saved using `pickle` for fast retrieval.

```python
movies_dict = pickle.load(open("movie_dict.pkl", "rb"))
movies = pd.DataFrame(movies_dict)
similarity = pickle.load(open("similarity.pkl", "rb"))
```

### 2. Recommendation Algorithm
- When a user selects a movie, the system finds similar movies using a precomputed similarity matrix.
- The most relevant movies are returned along with their posters.

```python
def recommend(movie):
    movie_index = movies[movies["title"] == movie].index[0]
    distances = similarity[movie_index]
    movies_list = sorted(list(enumerate(distances)), reverse=True, key=lambda x: x[1])[1:6]
    
    recommended_movies = []
    recommended_movies_posters = []
    for item in movies_list:
        movie_id = movies.iloc[item[0]]["id"]
        recommended_movies.append(movies.iloc[item[0]]["title"])
        recommended_movies_posters.append(fetch_poster(movie_id))
    
    return recommended_movies, recommended_movies_posters
```

### 3. Fetching Movie Posters
- The system fetches posters from **The Movie Database (TMDb)** API.

```python
def fetch_poster(id):
    response = requests.get(
        f'https://api.themoviedb.org/3/movie/{id}?api_key=YOUR_API_KEY&language=en-US')
    data = response.json()
    return "https://image.tmdb.org/t/p/w500/" + data['poster_path']
```

### 4. User Interface
- The interface is built using **Streamlit**, allowing users to select a movie and get recommendations.
- The results are displayed in a structured layout with posters.

```python
st.title("CineMatch - AI Movie Recommendation System")
selected_movie_name = st.selectbox("Select a movie:", movies["title"].values)

if st.button("Recommend"):
    names, posters = recommend(selected_movie_name)
    
    col1, col2, col3, col4, col5 = st.columns(5)
    
    with col1:
        st.text(names[0])
        st.image(posters[0])
    with col2:
        st.text(names[1])
        st.image(posters[1])
    with col3:
        st.text(names[2])
        st.image(posters[2])
    with col4:
        st.text(names[3])
        st.image(posters[3])
    with col5:
        st.text(names[4])
        st.image(posters[4])
```

## Installation & Usage
### Prerequisites
- Python 3.x
- Streamlit
- Pandas
- Requests

### Setup Instructions
1. Clone this repository:
   ```bash
   git clone https://github.com/your-repo/cinematch.git
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the application:
   ```bash
   streamlit run app.py
   ```


![Project Screenshot](Project%20Screenshot.png)


## Future Improvements
- **Enhanced Recommendation Algorithm**: Use deep learning models for better predictions.
- **User Login System**: Allow users to save their preferences.
- **More Filters**: Include genre-based and popularity-based filtering.

## License
This project is licensed under the MIT License.

---
**Author**: Yashvardhan Singh

**GitHub**: [YashVdSingh07]([https://github.com/your-profile](https://github.com/YashVdSingh07))

