# Face-Login-System
To create a face login system using Python, PyAutoGUI, and facial recognition, we can break the process into a few steps. We will use the OpenCV library for facial recognition, and PyAutoGUI will help us automate actions such as logging in or handling the interface.

Here’s how you can achieve a basic face recognition-based login system using Python:
Steps:

    Install necessary libraries:
        Install opencv-python for image processing and facial recognition.
        Install pyautogui for automating the graphical interface actions.
        Install face_recognition for easier face recognition functionality.

    You can install these using pip:

    pip install opencv-python pyautogui face_recognition

    Create the face recognition model: We will use OpenCV and face_recognition to handle face detection and recognition.

    Create a login system: After recognizing the face, the system will automate the login process using PyAutoGUI.

Full Code Example:

import cv2
import face_recognition
import pyautogui
import numpy as np
import time

# Step 1: Load known face encodings (for demo purposes, using a pre-recorded face)
def load_known_faces():
    # Load a sample image of the user
    user_image = face_recognition.load_image_file("user_image.jpg")  # Change to the image file path
    user_encoding = face_recognition.face_encodings(user_image)[0]
    return [user_encoding]  # Returning the list of known encodings

# Step 2: Initialize webcam and face recognition model
def initialize_face_recognition():
    # Start the webcam
    video_capture = cv2.VideoCapture(0)
    known_face_encodings = load_known_faces()

    return video_capture, known_face_encodings

# Step 3: Capture frames and perform face recognition
def recognize_face(video_capture, known_face_encodings):
    while True:
        ret, frame = video_capture.read()  # Read a frame from the webcam
        if not ret:
            break
        
        # Convert the image to RGB (OpenCV uses BGR by default)
        rgb_frame = frame[:, :, ::-1]
        
        # Find faces and their encodings in the frame
        face_locations = face_recognition.face_locations(rgb_frame)
        face_encodings = face_recognition.face_encodings(rgb_frame, face_locations)
        
        for (top, right, bottom, left), face_encoding in zip(face_locations, face_encodings):
            matches = face_recognition.compare_faces(known_face_encodings, face_encoding)

            if True in matches:
                # If the face matches, proceed to login
                print("Face recognized! Proceeding with login.")
                perform_login()
                break
        # Display the image for debugging purposes
        cv2.imshow("Face Login System", frame)
        
        # Exit on pressing 'q'
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

# Step 4: Perform automated login using PyAutoGUI
def perform_login():
    # Simulating a keyboard input using PyAutoGUI (e.g., logging into the system)
    pyautogui.write('password')  # Use actual password if needed
    pyautogui.press('enter')  # Press 'enter' to login

    # Additional automation can be done here for the login process
    print("Logged in successfully.")

# Step 5: Main function to handle the logic
def main():
    # Initialize the face recognition and video capture
    video_capture, known_face_encodings = initialize_face_recognition()

    try:
        # Start face recognition
        recognize_face(video_capture, known_face_encodings)
    finally:
        # Release the video capture and close the window
        video_capture.release()
        cv2.destroyAllWindows()

if __name__ == "__main__":
    main()

Explanation of Code:

    Loading Known Faces: The load_known_faces function loads an image of the user (user_image.jpg), processes it, and extracts the face encoding. This encoding is then used for recognition.

    Face Recognition: The recognize_face function continuously captures frames from the webcam. For each frame, it detects faces, encodes them, and compares them to known faces. If a match is found, it proceeds to login.

    Login Automation: Once the face is recognized, the perform_login function is triggered, which simulates typing a password and hitting the "Enter" key using pyautogui.

    Handling Webcam Input: OpenCV’s VideoCapture object is used to access the webcam and process the frames. The face_recognition library is used to detect faces and their encodings.

Running the Code:

    Make sure you have a sample image (user_image.jpg) for face recognition.
    Run the script.
    The program will open the webcam feed and check for faces.
    Once the correct face is recognized, it will simulate the login process using PyAutoGUI.

Things to consider:

    Security: This implementation is very basic. In a real-world application, additional security features (e.g., multi-factor authentication) would be necessary.
    Performance: Depending on the system and hardware, real-time face recognition may cause delays. Make sure to optimize performance if needed.

This should help you get started with creating a face login system using Python, OpenCV, face_recognition, and PyAutoGUI.
