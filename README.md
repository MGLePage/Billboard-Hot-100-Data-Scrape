# Billboard-Hot-100-Data-Scrape
Billboard Hot 100 Data Scrape To CSV File

#Import libraries
from bs4 import BeautifulSoup
import requests
import csv

#Open file to store song title and artist in CSV file
file = open('billboard_hot_list.csv', 'w')

#Create a variable for writing to CSV file
writer = csv.writer(file)

#Create the header row for CSV file
writer.writerow(['-Title-', '-Artist-'])

#Request Billboard Hot 100 Webpage & store it as variable
page_to_scrape = requests.get('https://www.billboard.com/charts/hot-100/')

#Using beautiful soup to parse text in web page and store it as a variable
soup = BeautifulSoup(page_to_scrape.text, 'html.parser')

#Finding the top song and assigning variable
title_top = soup.findAll('h3', attrs={'class':'c-title a-no-trucate a-font-primary-bold-s u-letter-spacing-0021 u-font-size-23@tablet lrv-u-font-size-16 u-line-height-125 u-line-height-normal@mobile-max a-truncate-ellipsis u-max-width-245 u-max-width-230@tablet-only u-letter-spacing-0028@tablet'})

#Finding the top artist and assigning variable
artist_top = soup.findAll('span', attrs={'c-label a-no-trucate a-font-primary-s lrv-u-font-size-14@mobile-max u-line-height-normal@mobile-max u-letter-spacing-0021 lrv-u-display-block a-truncate-ellipsis-2line u-max-width-330 u-max-width-230@tablet-only u-font-size-20@tablet'})

#For Loop to obtain the top song and artist and inputting them in the CSV file
for title_top, artist_top in zip(title_top, artist_top):
    print(title_top.text.strip() + artist_top.text.strip())
    #writing the top song and artist to the first row in the CSV file
    writer.writerow([title_top.text.strip(), artist_top.text.strip()])

#Finding the other 99 artists
title = soup.findAll('h3', attrs={'class':'c-title a-no-trucate a-font-primary-bold-s u-letter-spacing-0021 lrv-u-font-size-18@tablet lrv-u-font-size-16 u-line-height-125 u-line-height-normal@mobile-max a-truncate-ellipsis u-max-width-330 u-max-width-230@tablet-only'})

#Finding the other 99  songs
artist = soup.findAll('span', attrs={'class':'c-label a-no-trucate a-font-primary-s lrv-u-font-size-14@mobile-max u-line-height-normal@mobile-max u-letter-spacing-0021 lrv-u-display-block a-truncate-ellipsis-2line u-max-width-330 u-max-width-230@tablet-only'})

#For loop to obtain the the other 99 artists and songs and inputting them into the CSV file
for title, artist in zip(title, artist):
    print(title.text.strip() + artist.text.strip())
    #write each song an title to a row in the CSV file
    writer.writerow([title.text.strip(), artist.text.strip()])
file.close()
