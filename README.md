## Smart Denomination Recognition System for Visually Impaired Individuals
This project focuses on developing a reliable denomination recognition system that empowers visually impaired individuals. Using image processing and machine learning techniques, this project aims to detect currency denominations accurately under various conditions to promote independence and financial security.

## About
The Denomination Recognition system assists visually impaired users by identifying different banknote denominations in real-time. The project tackles challenges such as lighting variations, currency design differences, and image noise. The system leverages image processing techniques to detect unique currency features and convert visual information into auditory alerts, enhancing accessibility.

## Features
Image Acquisition: Captures images of currency through a camera interface.
Edge Detection: Utilizes Canny edge detection to enhance currency feature boundaries.
Pattern Recognition: Employs machine learning to identify unique patterns of each denomination.
Real-World Adaptability: Handles different environmental conditions, including lighting variations and varying currency designs.
Region of Interest (ROI) Masking: Reduces processing time by focusing on the relevant area of the captured image.
Voice Alert: Provides audio feedback for each detected denomination.

## Requirements
HARDWARE REQUIREMENTS 

• Processor : Intel core processor 2.6.0 GHZ • RAM : 4 GB 

• Hard disk : 160 GB 

• Keyboard : Standard keyboard 

• Monitor : 15-inch colour monitor 

SOFTWARE REQUIREMENTS 
• Server Side : Python 3.7.4(64-bit) or (32-bit)  

• IDE : Pycharm 

• Libraries : OpenCV, Tensorflow, KERAS

• OS : Windows 10 64 –bit 

## System Architecture
Preprocessing: Converts images to grayscale and applies Gaussian blur to reduce noise.
Edge Detection: Highlights currency edges using the Canny algorithm.
ROI Selection: Focuses on the central region of the image where the currency is expected to be located.
Pattern Recognition: Identifies denomination patterns through image classification algorithms.
Voice Feedback: Announces the recognized denomination to the user.

![system architecture](https://github.com/user-attachments/assets/c0e31a90-6878-4bfc-b4fb-a6643e20c2b8)

## Program
```
from tkinter import *
import os
from tkinter import filedialog
import cv2
from tkinter import messagebox
import win32com.client as wincl

speak = wincl.Dispatch("SAPI.SpVoice")


def file_success():
    global file_success_screen
    file_success_screen = Toplevel(training_screen)
    file_success_screen.title("File Upload Success")
    file_success_screen.geometry("150x100")

    Label(file_success_screen, text="File Upload Success").pack()
    Button(file_success_screen, text="OK", font=('Palatino Linotype', 15),
           height="2", width="30").pack()


def training():
    global training_screen
    global clicked

    training_screen = Toplevel(main_screen)
    training_screen.title("Training")
    training_screen.geometry("600x450+650+150")
    training_screen.minsize(120, 1)
    training_screen.maxsize(1604, 881)
    training_screen.resizable(1, 1)

    Label(training_screen, text="Upload Image",
          foreground="#000000", width="300", height="2",
          font=("Palatino Linotype", 16)).pack()
    Label(training_screen, text="").pack()

    options = ["10", "20", "50", "100", "200", "500", "2000", "Fake"]
    clicked = StringVar()
    clicked.set("Normal")

    drop = OptionMenu(training_screen, clicked, *options)
    drop.config(width="30")
    drop.pack()

    Button(training_screen, text="Upload Image", font=('Palatino Linotype', 15),
           height="2", width="30", command=img_training).pack()


def img_training():
    name1 = clicked.get()
    if name1 == "Normal":
        messagebox.showerror("Error", "Please select a valid denomination.")
        return

    import_file_path = filedialog.askopenfilename()
    if not import_file_path:
        return

    splname = os.path.split(import_file_path)[1]

    image = cv2.imread(import_file_path)
    filename = f'Data/Train/{name1}/{splname}'

    os.makedirs(os.path.dirname(filename), exist_ok=True)
    cv2.imwrite(filename, image)

    image_resized = cv2.resize(image, (780, 540))
    gray = cv2.cvtColor(image_resized, cv2.COLOR_BGR2GRAY)
    cv2.imshow('Original image', image_resized)
    cv2.imshow('Gray image', gray)
    cv2.waitKey(0)
    cv2.destroyAllWindows()


def full_training():
    import Model as mm


def testing1():
    import cv2 as cv
    import easyocr

    reader = easyocr.Reader(['en'])
    cap = cv.VideoCapture(0)
    frame_count = 0
    total = 0

    while cap.isOpened():
        hasFrame, frame = cap.read()
        if not hasFrame:
            break

        frame_count += 1
        if frame_count % 5 == 0:
            result = reader.readtext(frame)
            for detection in result:
                text = detection[1]
                if text.isdigit():
                    total += int(text)
                    speak.Speak(f"{text} rupee, Total {total}")

        if cv.waitKey(1) & 0xFF == ord('q'):
            break
        cv.imshow('frame', frame)

    cap.release()
    cv.destroyAllWindows()


def testing():
    global testing_screen
    testing_screen = Toplevel(main_screen)
    testing_screen.title("Testing")
    testing_screen.geometry("600x450+650+150")
    testing_screen.minsize(120, 1)
    testing_screen.maxsize(1604, 881)
    testing_screen.resizable(1, 1)

    Label(testing_screen, text="Upload Image", width="300", height="2",
          font=("Palatino Linotype", 16)).pack()
    Label(testing_screen, text="").pack()

    Button(testing_screen, text="Upload Image", font=('Palatino Linotype', 15),
           height="2", width="30", command=img_test).pack()


def img_test():
    import_file_path = filedialog.askopenfilename()
    if not import_file_path:
        return

    image = cv2.imread(import_file_path)
    filename = 'Output/Out/Test.jpg'
    os.makedirs(os.path.dirname(filename), exist_ok=True)
    cv2.imwrite(filename, image)

    img = cv2.imread(import_file_path)
    img = cv2.resize(img, ((int)(img.shape[1] / 5), (int)(img.shape[0] / 5)))

    original = img.copy()
    img_resized = cv2.resize(original, (960, 540))
    cv2.imshow('Original Image', img_resized)

    gray = cv2.cvtColor(img_resized, cv2.COLOR_BGR2GRAY)
    dst = cv2.fastNlMeansDenoisingColored(img_resized, None, 10, 10, 7, 21)
    cv2.imshow("Noise Removal", dst)
    cv2.waitKey(0)
    cv2.destroyAllWindows()
    result()


def result():
    import tensorflow as tf
    from tensorflow.keras.preprocessing import image
    import numpy as np

    classifierLoad = tf.keras.models.load_model('currency.h5')
    test_image = image.load_img('./Output/Out/Test.jpg', target_size=(500, 400))
    test_image = np.expand_dims(test_image, axis=0)
    result = classifierLoad.predict(test_image)

    denominations = ["10", "20", "50", "100", "200", "500", "2000", "Fake"]
    out = denominations[np.argmax(result)]
    speak.Speak(f"Classification Result: {out}")
    messagebox.showinfo("Result", f"Classification Result: {out}")


def main_account_screen():
    global main_screen
    main_screen = Tk()
    width = 600
    height = 600
    screen_width = main_screen.winfo_screenwidth()
    screen_height = main_screen.winfo_screenheight()
    x = (screen_width / 2) - (width / 2)
    y = (screen_height / 2) - (height / 2)
    main_screen.geometry(f"{width}x{height}+{int(x)}+{int(y)}")
    main_screen.resizable(0, 0)
    main_screen.title("Currency Prediction")

    Label(text="Currency Prediction", width="300", height="5",
          font=("Palatino Linotype", 16)).pack()

    Button(text="Upload Image", font=('Palatino Linotype', 15),
           height="2", width="20", command=training).pack()
    Label(text="").pack()

    Button(text="Training", font=('Palatino Linotype', 15),
           height="2", width="20", command=full_training).pack()
    Label(text="").pack()

    Button(text="Image Testing", font=('Palatino Linotype', 15),
           height="2", width="20", command=testing).pack()
    Label(text="").pack()

    Button(text="Video Testing", font=('Palatino Linotype', 15),
           height="2", width="20", command=testing1).pack()

    main_screen.mainloop()


if _name_ == "_main_":
    main_account_screen()
```

## Output

![WhatsApp Image 2024-12-21 at 08 34 12_7380acaf](https://github.com/user-attachments/assets/09ac4966-7751-47b2-bd2c-d91e24ef39c6)


## Results and Impact
Output Type: Voice alert and visual display of recognized denomination.

Performance: Works effectively under typical indoor lighting conditions, with limitations in very low light.

Strengths: Real-time detection, high accuracy, and improved financial independence.

Limitations: Reduced accuracy in dim lighting and extremely complex currency designs.

This project serves as a foundation for future developments in assistive technologies and contributes to creating a more inclusive and accessible digital environment.

## Articles published / References
1.Wei Sun, Xiaorui Zhang, and Xiaozheng He, "Lightweight Image Classifier Using Dilated and Depthwise Separable Convolutions," Journal of Cloud Computing, 2020.

2.Rushikesh Jadhav et al., "Currency Recognition using Machine Learning," IRJET, 2022.

3.Park et al., "Deep Feature-Based Three-Stage Detection of Banknotes and Coins for Assisting Visually Impaired People," IEEE Access, 2020.




