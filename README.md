# Joker vs. Roman Reigns Image Classifier

This project is a simple Convolutional Neural Network (CNN) built using Keras and TensorFlow. It's trained to classify images into two categories: **Joker** or **Roman Reigns**.

The repository includes scripts to:
* Automatically download training images from Google.
* Build, train, and save the classification model.
* Load the trained model to make predictions on new images.

## 📂 Files

* **`Train.py`**: This script builds the CNN model architecture, loads the training and validation data, trains the model, and saves the trained model architecture as `model.json` and its weights as `model.h5`.
* **`test.py`**: This script loads the saved `model.json` and `model.h5` files, then iterates through a test directory, classifying each image as either 'Joker' or 'RomanReigns'.
* **`Data_Collection.py`** (Optional): A script using `pygoogle_image` to download the images needed for training. (Based on the code snippet provided).

## 🚀 How to Use

Follow these steps to get the project up and running.

### 1. Prerequisites

You need to have Python and several libraries installed. You can install them using pip:

```bash
pip install tensorflow keras numpy pillow pygoogle-image
```

### 2. Step 1: Collect Data

Run the data collection script (or create one) to download your images. You will need to run it for each class.

```python
# Data_Collection.py
from pygoogle_image import image as pi

# Download images for the first class
pi.download("the joker movie", limit=50)

# Download images for the second class
pi.download("roman reigns wwe", limit=50)
```

### 3. Step 2: Organize Your Dataset

The `Train.py` script expects a specific folder structure. After downloading your images, you must manually create this structure and split your images:

```
Dataset/
├── train/
│   ├── joker/
│   │   ├── joker_img1.jpg
│   │   ├── joker_img2.jpg
│   │   └── ...
│   └── romanreigns/
│       ├── roman_img1.jpg
│       ├── roman_img2.jpg
│       └── ...
└── val/
    ├── joker/
    │   ├── joker_val1.jpg
    │   └── ...
    └── romanreigns/
        ├── roman_val1.jpg
        └── ...
```
* **`Dataset/train`**: Contains the majority of your images for training.
* **`Dataset/val`**: Contains a smaller set of images for validation.
* The script will automatically label images based on the folder name (e.g., all images in `train/joker` are labeled 'joker').

### 4. Step 3: Train the Model

Before running `Train.py`, make sure you've **fixed the bug** in the original file.

**Fix in `Train.py`:**
Change this line:
```python
model.save_weights("model.weights.h5")
```
To this (to match `test.py`):
```python
model.save_weights("model.h5")
```

Now, run the training script:
```bash
python Train.py
```
This will train the model for 50 epochs and create two files: `model.json` and `model.h5`.

### 5. Step 4: Test the Model

Before running `test.py`, make sure you've **fixed the bug** in the original file.

**Fix in `test.py`:**
Change this logic:
```python
if result[0][0] == 1:
    prediction = 'RomanReigns'
else:
    prediction = 'Joker'
```
To this (because the model outputs a probability, not an exact 1 or 0):
```python
if result[0][0] > 0.5:
    prediction = 'RomanReigns'
else:
    prediction = 'Joker'
```

Make sure to also update the `path` variable in `test.py` to point to your *own* test image folder.

```python
# Update this path in test.py to your test images
path = 'C:/Users/your_user/path/to/Dataset/test'
```

After making the fixes, run the test script:
```bash
python test.py
```
It will print the prediction for each `.jpeg` image found in your test path.

## 🧠 Model Architecture

This project uses a simple Convolutional Neural Network (CNN). The architecture is:

1.  **Conv2D** (32 filters, 3x3 kernel, ReLU activation)
2.  **MaxPooling2D** (2x2 pool size)
3.  **Flatten**
4.  **Dense** (128 units, ReLU activation)
5.  **Dense** (1 unit, Sigmoid activation) - *Output Layer*

The model is compiled with the `adam` optimizer and `binary_crossentropy` loss, which is standard for two-class classification.

Train  Images:
<img width="1345" height="743" alt="Screenshot 2025-10-25 083429" src="https://github.com/user-attachments/assets/a29862b6-8380-4a7b-a8c6-ef85766ab719" />
<img width="1377" height="697" alt="Screenshot 2025-10-25 083446" src="https://github.com/user-attachments/assets/76f9047e-bd2b-40f6-873c-21f13aa1d5d3" />
<img width="1536" height="1004" alt="Screenshot 2025-10-25 083634" src="https://github.com/user-attachments/assets/0629e550-56ff-4015-85ad-75b241fac0e2" />


Test Images:
<img width="1315" height="724" alt="Screenshot 2025-10-25 083410" src="https://github.com/user-attachments/assets/7e6d00dc-d092-4a54-9365-5c81b427fd5d" />
<img width="1644" height="1041" alt="Screenshot 2025-10-25 083528" src="https://github.com/user-attachments/assets/c60a9ee9-90c4-4075-8f9c-6d99fa79ee19" />
