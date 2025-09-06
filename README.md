# faceclip-pgvector

Face recognition pipeline using **Python**, **OpenCV**, and **PostgreSQL with pgvector** for efficient similarity search.  

Faces are represented as **feature vectors** using ORB descriptors that capture unique facial keypoints and patterns.

---

## Example

Original Image | Detected Faces
:-------------:|:-------------------------:
<img src="sample.jpeg" width="300"> | <p align="center"><img src="stored-faces/0.jpg" width="100"><img src="stored-faces/1.jpg" width="100"><img src="stored-faces/2.jpg" width="100"><img src="stored-faces/3.jpg" width="100"><img src="stored-faces/4.jpg" width="100"></p>

---

## How It Works
1. **Face Detection** – Extract faces using OpenCV Haar Cascade  
2. **Feature Extraction** – Convert faces into ORB feature vectors  
3. **Database Storage** – Store vectors in PostgreSQL with pgvector  
4. **Similarity Search** – Find closest matches using vector similarity  

<p align="center"><img src="embeddings.jpg" width="500"></p>

---

## Features
- ✅ **Offline** - no external dependencies
- ✅ **Lightweight** - no large model downloads  
- ✅ **Fast** - quick ORB feature computation
- ✅ **Scalable** - efficient similarity search with pgvector

---

## Installation
```bash
git clone https://github.com/Longman-max/faceclip-pgvector.git
cd faceclip-pgvector
pip install -r requirements.txt
```

## Usage

**1. Detect faces:**
```python
import cv2
haar_cascade = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")
faces = haar_cascade.detectMultiScale(img, scaleFactor=1.05, minNeighbors=5)
```

**2. Extract and store features:**
```python
orb = cv2.ORB_create(nfeatures=100)
keypoints, descriptors = orb.detectAndCompute(img, None)
# Store in PostgreSQL with pgvector
```

**3. Find similar faces:**
```python
cur.execute("SELECT * FROM pictures ORDER BY embedding <-> %s LIMIT 1;", (query_vector,))
```

---

## Database Setup
```sql
CREATE EXTENSION vector;
CREATE TABLE pictures (filename TEXT, embedding vector(512));
```