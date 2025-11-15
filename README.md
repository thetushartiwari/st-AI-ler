# 👗 Stailer - AI-Powered Fashion Recommendation System

An intelligent e-commerce platform that learns user preferences and provides personalized clothing recommendations using machine learning algorithms.

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- pip (Python package manager)

### Installation & Setup

1. **Clone/Download the project**
```bash
cd stailer_ecom
```

2. **Create virtual environment**
```bash
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac
```

3. **Install dependencies**
```bash
pip install django pandas scikit-learn numpy pillow
```

4. **Run migrations**
```bash
python manage.py migrate
```

5. **Load sample data**
```bash
python manage.py shell
>>> from products.load_data import run
>>> run()
>>> exit()
```

6. **Start the server**
```bash
python manage.py runserver
```

7. **Access the application**
Open your browser and go to: `http://127.0.0.1:8000/`

## 🎯 Features

- **Smart Recommendations**: AI-powered suggestions based on user preferences
- **Color-Aware Matching**: Understands color similarities and differences
- **Gender Filtering**: Filter products by Men's, Women's, or All categories
- **Interactive UI**: Like/Dislike buttons with real-time recommendations
- **User Preference Learning**: System learns from user interactions

## 🏗️ Project Structure

```
stailer_ecom/
├── products/                 # Main Django app
│   ├── models.py            # Database models
│   ├── views.py             # View functions
│   ├── urls.py              # URL routing
│   ├── recommendation.py    # ML recommendation engine
│   ├── load_data.py         # Data loading script
│   └── templates/           # HTML templates
├── stailer/                 # Django project settings
│   ├── settings.py          # Project configuration
│   └── urls.py              # Main URL configuration
└── ecommerce_products.csv   # Sample product data
```

## 🧠 How It Works

1. **User Interaction**: User likes/dislikes products
2. **Preference Storage**: System saves preferences in database
3. **ML Analysis**: Algorithm analyzes product features (color, category, description)
4. **Recommendation Generation**: 
   - **Like**: Shows similar products (same colors/categories)
   - **Dislike**: Shows different products (different colors/categories)
5. **Continuous Learning**: System improves with more user interactions

---

# 📚 Interview Preparation Guide

## 🎯 Core Concepts & Technologies

### 1. **Django Framework**
**What it is**: High-level Python web framework following MVC pattern
**Key Concepts**:
- **Models**: Database abstraction layer (Product, UserPreference)
- **Views**: Business logic handling (product_list, toggle_like)
- **Templates**: HTML rendering with Django template language
- **URL Routing**: URL patterns mapping to views
- **ORM**: Object-Relational Mapping for database operations

**Interview Questions**:
- "Explain Django's MVT architecture"
- "How do you handle user authentication in Django?"
- "What's the difference between Django's ORM and raw SQL?"

### 2. **Machine Learning & Recommendation Systems**

#### **Content-Based Filtering**
**Concept**: Recommends items similar to what user liked before
**Implementation**: 
- TF-IDF vectorization of product descriptions
- Cosine similarity calculation
- Color extraction and matching

**Key Algorithm**:
```python
# TF-IDF Vectorization
tfidf = TfidfVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(product_descriptions)

# Cosine Similarity
similarity = cosine_similarity(tfidf_matrix, tfidf_matrix)
```

#### **Collaborative Filtering Elements**
**Concept**: Uses user preferences to find similar users/items
**Implementation**: UserPreference model tracks likes/dislikes

### 3. **Natural Language Processing (NLP)**
**Color Extraction**: Regex-based color detection from product names
**Text Processing**: TF-IDF for feature extraction from descriptions

### 4. **Database Design**
**Models**:
- **Product**: Stores clothing items with metadata
- **UserPreference**: Tracks user likes/dislikes
- **Relationships**: Foreign key relationships between models

### 5. **Frontend Technologies**
**AJAX**: Asynchronous requests for like/dislike actions
**JavaScript**: Dynamic content updates without page refresh
**CSS Grid**: Modern responsive layout

## 🔧 Technical Implementation Details

### **Recommendation Algorithm Flow**:

1. **Data Preprocessing**:
   ```python
   # Extract features from products
   df['text'] = df['name'] + ' ' + df['category'] + ' ' + df['description']
   df['colors'] = df['text'].apply(extract_colors)
   ```

2. **Similarity Calculation**:
   ```python
   # TF-IDF + Cosine Similarity
   tfidf_matrix = tfidf.fit_transform(df['text'])
   similarity = cosine_similarity(tfidf_matrix, tfidf_matrix)
   ```

3. **Color-Based Scoring**:
   ```python
   # Boost similar colors for likes
   color_sim = get_color_similarity(target_colors, other_colors)
   if color_sim > 0:
       score += 0.3 * color_sim
   ```

4. **User Preference Integration**:
   ```python
   # Personalize based on user history
   user_prefs = UserPreference.objects.filter(user=user)
   # Boost scores for similar items to liked ones
   # Reduce scores for similar items to disliked ones
   ```

### **API Endpoints**:
- `GET /` - Product listing with filters
- `GET /product/<id>/` - Product detail page
- `POST /toggle-like/<id>/` - Like/dislike action
- `POST /get-recommendations/<id>/` - Get recommendations

## 🎤 Common Interview Questions & Answers

### **Q: How does your recommendation system work?**
**A**: "I implemented a hybrid recommendation system combining content-based filtering with user preference learning. The system uses TF-IDF vectorization to analyze product descriptions and cosine similarity to find similar items. For likes, it boosts products with similar colors and categories. For dislikes, it avoids similar colors and categories, showing more diverse options."

### **Q: How do you handle the cold start problem?**
**A**: "For new users without preferences, the system falls back to content-based recommendations using product similarity. As users interact more, their preferences are stored and used to personalize recommendations."

### **Q: What's the time complexity of your recommendation algorithm?**
**A**: "The TF-IDF vectorization is O(n*m) where n is products and m is vocabulary size. Cosine similarity calculation is O(n²). For real-time recommendations, I could optimize by pre-computing similarity matrices and using caching."

### **Q: How would you scale this system?**
**A**: "I'd implement several optimizations:
1. **Caching**: Redis for similarity matrices and user preferences
2. **Database**: Move to PostgreSQL with proper indexing
3. **Microservices**: Separate recommendation service
4. **ML Pipeline**: Batch processing for similarity calculations
5. **CDN**: For product images and static content"

### **Q: How do you measure recommendation quality?**
**A**: "I'd implement several metrics:
1. **Click-through Rate**: How often users click recommendations
2. **Conversion Rate**: How often recommendations lead to purchases
3. **Diversity**: Ensure recommendations aren't too similar
4. **Coverage**: How many products get recommended
5. **A/B Testing**: Compare different algorithms"

## 🚀 Advanced Concepts to Discuss

### **1. Machine Learning Pipeline**
- **Feature Engineering**: Color extraction, text preprocessing
- **Model Selection**: Why TF-IDF + Cosine Similarity
- **Evaluation Metrics**: Precision, Recall, F1-Score
- **Hyperparameter Tuning**: Similarity thresholds, boost factors

### **2. System Architecture**
- **MVC Pattern**: Django's Model-View-Template
- **RESTful APIs**: JSON responses for AJAX calls
- **Database Optimization**: Indexing, query optimization
- **Caching Strategy**: Redis, Memcached

### **3. Performance Optimization**
- **Lazy Loading**: Load recommendations on demand
- **Pagination**: Handle large product catalogs
- **Image Optimization**: Compress product images
- **Database Queries**: Use select_related, prefetch_related

### **4. Security Considerations**
- **CSRF Protection**: Django's built-in CSRF tokens
- **Input Validation**: Sanitize user inputs
- **SQL Injection**: Django ORM prevents this
- **XSS Protection**: Template auto-escaping

## 🎯 Key Takeaways for Interview

1. **Problem-Solving**: Show how you identified and solved the recommendation challenge
2. **Technical Depth**: Understand ML algorithms, Django architecture, database design
3. **Scalability**: Think about how to handle millions of users and products
4. **User Experience**: Consider how recommendations improve shopping experience
5. **Data-Driven**: Explain how you'd measure and improve the system

## 🔗 Additional Learning Resources

- **Django Documentation**: https://docs.djangoproject.com/
- **Scikit-learn**: https://scikit-learn.org/
- **Recommendation Systems**: "Programming Collective Intelligence" by Toby Segaran
- **Machine Learning**: "Hands-On Machine Learning" by Aurélien Géron

---

**Built with ❤️ using Django, Python, and Machine Learning**
