# BizCardX_-Extracting-Business-Card-Data-with-OCR
BizCardX_ Extracting Business Card Data with OCR

This project is designed in PyCharm Community edition

Process involved in the project,
1. Extract business card from user
2. Convert the text image in to text data
3. Analyse and organize data in single format
4. Store the data in the database
5. If required user can update the data
6. User able to delete the data as well

Packages used in the project are,
Numpy, Pandas, Pickle, streamlit, streamlit_option_menu, streamlit_authenticator, easyocr, psycopg2, re, io, PIL

UI of the project is designed by "Streamlit", "streamlit_option_menu", and "streamlit_authenticator" used for user Login protection of the application and password management.

"Easyocr" packaged used to convert the image file in to respective language text, organize the text by using "re" package.

"io" and "PIL" packages used to prepare and process the image to store.

Both text and image stored in to postgreSQL database with the help of "psycopg2" package.

The text data can be modified and stored in the database and delete option also provided to manage data.

This project is open for may industry applications and very big scope for improvising to particular application.

I have designed the project in the view of sales industry company employee storing their customer business cards in the very simple company portal
