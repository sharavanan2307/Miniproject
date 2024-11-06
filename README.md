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




